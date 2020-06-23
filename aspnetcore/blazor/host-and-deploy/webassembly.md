---
title: 裝載和部署 ASP.NET Core Blazor WebAssembly
author: guardrex
description: 瞭解如何 Blazor 使用 ASP.NET Core、內容傳遞網路（CDN）、檔案伺服器和 GitHub 頁面來裝載和部署應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/07/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/host-and-deploy/webassembly
ms.openlocfilehash: 7e0263200ebb9ce60f7234af3cbb18c5aeaa3e09
ms.sourcegitcommit: 066d66ea150f8aab63f9e0e0668b06c9426296fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/23/2020
ms.locfileid: "85243521"
---
# <a name="host-and-deploy-aspnet-core-blazor-webassembly"></a>裝載和部署 ASP.NET Core Blazor WebAssembly

By [Luke Latham](https://github.com/guardrex)、 [Rainer Stropek](https://www.timecockpit.com)、 [Daniel Roth](https://github.com/danroth27)、 [Ben Adams](https://twitter.com/ben_a_adams)和[Safia Abdalla](https://safia.rocks)

使用[ Blazor WebAssembly 裝載模型](xref:blazor/hosting-models#blazor-webassembly)：

* Blazor應用程式、其相依性和 .net 執行時間會以平行方式下載至瀏覽器。
* 應用程式會直接在瀏覽器 UI 執行緒上執行。

支援下列部署策略：

* Blazor應用程式是由 ASP.NET Core 應用程式提供。 此策略已於[搭配 ASP.NET Core 的已裝載部署](#hosted-deployment-with-aspnet-core)一節中涵蓋。
* Blazor應用程式會放在靜態裝載的 web 伺服器或服務上，其中 .net 不會用來服務 Blazor 應用程式。 此策略在[獨立部署](#standalone-deployment)一節中涵蓋，其中包括裝載 Blazor WebAssembly 應用程式作為 IIS 子應用程式的相關資訊。

## <a name="compression"></a>壓縮

Blazor發佈 WebAssembly 應用程式時，會在發佈期間以靜態方式壓縮輸出，以減少應用程式的大小，並移除執行時間壓縮的額外負荷。 使用下列壓縮演算法：

* [Brotli](https://tools.ietf.org/html/rfc7932) （最高層級）
* [Gzip](https://tools.ietf.org/html/rfc1952)

Blazor依賴主機來提供適當的壓縮檔案。 當使用 ASP.NET Core 裝載的專案時，主專案可以執行內容協調並提供靜態壓縮檔案。 裝載 Blazor WebAssembly 獨立應用程式時，可能需要額外的工作，以確保提供靜態壓縮檔案：

* 如需 IIS `web.config` 壓縮設定，請參閱[Iis： Brotli 和 Gzip 壓縮](#brotli-and-gzip-compression)一節。 
* 在不支援靜態壓縮檔案內容協商（例如 GitHub 頁面）的靜態裝載解決方案上裝載時，請考慮將應用程式設定為提取和解碼 Brotli 壓縮檔案：

  * 從應用程式中的[google/Brotli GitHub 存放庫](https://github.com/google/brotli/)參考 Brotli 解碼器。
  * 更新應用程式以使用此解碼器。 將中結束記號內的標記變更 `<body>` `wwwroot/index.html` 為下列內容：
  
    ```html
    <script src="brotli.decode.min.js"></script>
    <script src="_framework/blazor.webassembly.js" autostart="false"></script>
    <script>
    Blazor.start({
      loadBootResource: function (type, name, defaultUri, integrity) {
        if (type !== 'dotnetjs' && location.hostname !== 'localhost') {
          return (async function () {
            const response = await fetch(defaultUri + '.br', { cache: 'no-cache' });
            if (!response.ok) {
              throw new Error(response.statusText);
            }
            const originalResponseBuffer = await response.arrayBuffer();
            const originalResponseArray = new Int8Array(originalResponseBuffer);
            const decompressedResponseArray = BrotliDecode(originalResponseArray);
            const contentType = type === 
          'dotnetwasm' ? 'application/wasm' : 'application/octet-stream';
            return new Response(decompressedResponseArray, 
          { headers: { 'content-type': contentType } });
          })();
        }
      }
    });
  </script>
  ```
   
若要停用壓縮，請將 `BlazorEnableCompression` MSBuild 屬性新增至應用程式的專案檔，並將值設定為 `false` ：

```xml
<PropertyGroup>
  <BlazorEnableCompression>false</BlazorEnableCompression>
</PropertyGroup>
```

## <a name="rewrite-urls-for-correct-routing"></a>重寫 URL 以便正確地路由

WebAssembly 應用程式中頁面元件的路由要求 Blazor ，並不像伺服器上裝載的 Blazor 應用程式中的路由要求一樣簡單。 假設有 Blazor 兩個元件的 WebAssembly 應用程式：

* `Main.razor`：在應用程式的根目錄載入，並包含 `About` 元件（）的連結 `href="About"` 。
* `About.razor`： `About` 元件。

使用瀏覽器的網址列要求應用程式預設文件時 (例如 `https://www.contoso.com/`)：

1. 瀏覽器提出要求。
1. 預設頁面會傳回，這通常是 `index.html` 。
1. `index.html`啟動應用程式。
1. Blazor的路由器會載入，並 Razor `Main` 呈現元件。

在主頁面中，選取元件的連結可 `About` 在用戶端上運作，因為 Blazor 路由器會阻止瀏覽器針對在網際網路上提出要求， `www.contoso.com` `About` 並提供所呈現 `About` 元件本身。 * Blazor WebAssembly 應用程式內*內部端點的所有要求都以相同的方式工作：要求不會對網際網路上伺服器裝載的資源觸發瀏覽器型要求。 路由器會在內部處理要求。

如果使用瀏覽器之網址列提出對 `www.contoso.com/About` 的要求，則要求會失敗。 在應用程式的網際網路主機上沒有這類資源存在，因此會傳回「404 - 找不到」** 的回應。

由於瀏覽器會對以網際網路為基礎的主機要求用戶端頁面，因此 web 伺服器和主機服務必須針對不在伺服器上的資源，將所有要求重寫至 `index.html` 頁面。 `index.html`傳回時，應用程式的 Blazor 路由器會接管並以正確的資源回應。

部署到 IIS 伺服器時，您可以使用 URL 重寫模組搭配應用程式的已發佈檔案 `web.config` 。 如需詳細資訊，請參閱[IIS](#iis)一節。

## <a name="hosted-deployment-with-aspnet-core"></a>搭配 ASP.NET Core 的已裝載部署

*託管部署* Blazor 可從 web 伺服器上執行的[ASP.NET Core 應用程式](xref:index)，將 WebAssembly 應用程式提供給瀏覽器。

用戶端 Blazor WebAssembly 應用程式會發佈到 `/bin/Release/{TARGET FRAMEWORK}/publish/wwwroot` 伺服器應用程式的資料夾，以及伺服器應用程式的任何其他靜態 web 資產。 這兩個應用程式會一起部署。 需要有能夠裝載 ASP.NET Core 應用程式的網頁伺服器。 針對裝載的部署，Visual Studio 包括** Blazor WebAssembly 應用程式**專案範本（ `blazorwasm` 使用命令時的範本 [`dotnet new`](/dotnet/core/tools/dotnet-new) ）以及 **`Hosted`** 選取的選項（ `-ho|--hosted` 使用命令時 `dotnet new` ）。

如需 ASP.NET Core 應用程式裝載和部署的詳細資訊，請參閱 <xref:host-and-deploy/index>。

如需部署至 Azure App Service 的相關資訊，請參閱 <xref:tutorials/publish-to-azure-webapp-using-vs>。

## <a name="standalone-deployment"></a>獨立部署

*獨立部署* Blazor 會將 WebAssembly 應用程式當做一組直接由用戶端要求的靜態檔案來提供。 任何靜態檔案伺服器都可以服務 Blazor 應用程式。

獨立部署資產會發佈到 `/bin/Release/{TARGET FRAMEWORK}/publish/wwwroot` 資料夾中。

### <a name="azure-app-service"></a>Azure App Service

BlazorWebAssembly apps 可以部署到 Windows 上的 Azure App 服務，在[IIS](#iis)上裝載應用程式。

Blazor目前不支援將獨立 WebAssembly 應用程式部署至適用于 Linux 的 Azure App Service。 目前無法使用可裝載應用程式的 Linux 伺服器映射。 正在進行工作，以啟用此案例。

### <a name="iis"></a>IIS

IIS 是適用于應用程式的靜態檔案伺服器 Blazor 。 若要設定 IIS 來裝載 Blazor ，請參閱[在 Iis 上建立靜態網站](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis)。

已發行的資產會建立在 `/bin/Release/{TARGET FRAMEWORK}/publish` 資料夾中。 裝載 `publish` 網頁伺服器或主控服務上的資料夾內容。

#### <a name="webconfig"></a>web.config

Blazor發行專案時， `web.config` 會使用下列 IIS 設定來建立檔案：

* 針對下列副檔名設定 MIME 類型：
  * `.dll`: `application/octet-stream`
  * `.json`: `application/json`
  * `.wasm`: `application/wasm`
  * `.woff`: `application/font-woff`
  * `.woff2`: `application/font-woff`
* 針對下列 MIME 類型會啟用 HTTP 壓縮：
  * `application/octet-stream`
  * `application/wasm`
* 建立 URL Rewrite Module 規則：
  * 為應用程式的靜態資產所在的子目錄提供服務（ `wwwroot/{PATH REQUESTED}` ）。
  * 建立 SPA fallback 路由，讓非檔案資產的要求會重新導向至其靜態資產資料夾中的應用程式預設檔（ `wwwroot/index.html` ）。
  
#### <a name="use-a-custom-webconfig"></a>使用自訂 web.config

若要使用自訂檔案 `web.config` ，請將自訂檔案放 `web.config` 在專案資料夾的根目錄，併發行專案。

#### <a name="install-the-url-rewrite-module"></a>安裝 URL Rewrite 模組

需要 [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite)，才可重寫 URL。 預設不會安裝此模組，且其無法用來安裝為網頁伺服器 (IIS) 角色服務功能。 必須從 IIS 網站下載模組。 請使用 Web Platform Installer 安裝模組：

1. 在本機上，巡覽至 [URL Rewrite Module 下載頁面](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads)。 如需英文版，請選取 [WebPI]**** 下載 WebPI 安裝程式。 如需其他語言，請選取適當的伺服器架構 (x86 x64) 來下載安裝程式。
1. 將安裝程式複製到伺服器。 執行安裝程式。 選取 [安裝]**** 按鈕，並接受授權條款。 安裝完成之後，不需要重新啟動伺服器。

#### <a name="configure-the-website"></a>設定網站

將網站的 [實體路徑]**** 設為應用程式的資料夾。 此資料夾包含：

* `web.config`IIS 用來設定網站的檔案，包括必要的重新導向規則和檔案內容類型。
* 應用程式的靜態資產資料夾。

#### <a name="host-as-an-iis-sub-app"></a>以 IIS 子應用程式的形式裝載

如果獨立應用程式裝載為 IIS 子應用程式，請執行下列其中一項：

* 停用繼承的 ASP.NET Core 模組處理常式。

  Blazor `web.config` 將區段新增至檔案，以移除應用程式已發佈檔案中的處理常式 `<handlers>` ：

  ```xml
  <handlers>
    <remove name="aspNetCore" />
  </handlers>
  ```

* `<system.webServer>`使用設定為的元素，停用根（父系）應用程式區段的繼承 `<location>` `inheritInChildApplications` `false` ：

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <configuration>
    <location path="." inheritInChildApplications="false">
      <system.webServer>
        <handlers>
          <add name="aspNetCore" ... />
        </handlers>
        <aspNetCore ... />
      </system.webServer>
    </location>
  </configuration>
  ```

除了設定[應用程式的基底路徑](xref:blazor/host-and-deploy/index#app-base-path)之外，也會執行移除處理常式或停用繼承。 將應用程式檔案中的應用程式基底路徑，設定 `index.html` 為在 iis 中設定子應用程式時所使用的 iis 別名。

#### <a name="brotli-and-gzip-compression"></a>Brotli 和 Gzip 壓縮

您可以透過設定 IIS `web.config` 來提供 Brotli 或 Gzip 壓縮 Blazor 的資產。 如需範例設定，請參閱 [`web.config`](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/blazor/host-and-deploy/webassembly/_samples/web.config?raw=true) 。

#### <a name="troubleshooting"></a>疑難排解

如果收到「500 - 內部伺服器錯誤」**，且 IIS 管理員在嘗試存取網站設定時擲回錯誤，請確認是否已安裝 URL Rewrite 模組。 未安裝模組時，IIS 無法剖析該檔案 `web.config` 。 這可防止 IIS 管理員從服務的靜態檔案載入網站的設定和網站 Blazor 。

如需針對部署至 IIS 進行疑難排解的詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis>。

### <a name="azure-storage"></a>Azure 儲存體

[Azure 儲存體](/azure/storage/)靜態檔案裝載允許無伺服器 Blazor 應用程式裝載。 支援自訂網域名稱、Azure 內容傳遞網路 (CDN) 及 HTTPS。

當 Blob 服務針對儲存體帳戶上的靜態網站裝載啟用時：

* 將 [索引文件名稱]**** 設定為 `index.html`。
* 將 [錯誤文件路徑]**** 設定為 `index.html`。 Razor元件和其他非檔案端點不會位於 blob 服務所儲存之靜態內容中的實體路徑。 收到路由器應處理的其中一個資源的要求時 Blazor ，由 blob 服務產生的*404-找不*到的錯誤會將要求路由傳送至**錯誤檔路徑**。 `index.html`會傳回 blob，且路由器會 Blazor 載入並處理路徑。

如果在執行時間因為檔案標頭中有不適當的 MIME 類型而未載入檔案 `Content-Type` ，請採取下列其中一個動作：

* 設定您的工具，以在部署檔案時設定正確的 MIME 類型（ `Content-Type` 標頭）。
* 在部署應用程式之後，變更檔案的 MIME 類型（ `Content-Type` 標頭）。

  在每個檔案的儲存體總管（Azure 入口網站）中：
  
  1. 以滑鼠右鍵按一下檔案，然後選取 [**屬性**]。
  1. 設定**ContentType** ，然後選取 [**儲存**] 按鈕。

如需詳細資訊，請參閱 [Azure 儲存體中的靜態網站裝載](/azure/storage/blobs/storage-blob-static-website)。

### <a name="nginx"></a>Nginx

下列檔案 `nginx.conf` 已簡化，示範如何設定 Nginx，以便在 `index.html` 每次找不到磁片上的對應檔案時傳送檔案。

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /index.html =404;
        }
    }
}
```

如需生產環境 Nginx 網頁伺服器組態的詳細資訊，請參閱[建立 NGINX Plus 和 NGINX 組態檔](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/)。

### <a name="nginx-in-docker"></a>Docker 中的 Nginx

若要 Blazor 使用 Nginx 在 Docker 中裝載，請將 Dockerfile 設定為使用以 Alpine 為基礎的 Nginx 映射。 更新 Dockerfile 以將檔案複製 `nginx.config` 到容器中。

如下列範例所示，新增一行至 Dockerfile：

```dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="apache"></a>Apache

若要將 Blazor WebAssembly 應用程式部署至 CentOS 7 或更新版本：

1. 建立 Apache 設定檔。 下列範例是簡化的設定檔案（ `blazorapp.config` ）：

   ```
   <VirtualHost *:80>
       ServerName www.example.com
       ServerAlias *.example.com

       DocumentRoot "/var/www/blazorapp"
       ErrorDocument 404 /index.html

       AddType application/wasm .wasm
       AddType application/octet-stream .dll
   
       <Directory "/var/www/blazorapp">
           Options -Indexes
           AllowOverride None
       </Directory>

       <IfModule mod_deflate.c>
           AddOutputFilterByType DEFLATE text/css
           AddOutputFilterByType DEFLATE application/javascript
           AddOutputFilterByType DEFLATE text/html
           AddOutputFilterByType DEFLATE application/octet-stream
           AddOutputFilterByType DEFLATE application/wasm
           <IfModule mod_setenvif.c>
           BrowserMatch ^Mozilla/4 gzip-only-text/html
           BrowserMatch ^Mozilla/4.0[678] no-gzip
           BrowserMatch bMSIE !no-gzip !gzip-only-text/html
       </IfModule>
       </IfModule>

       ErrorLog /var/log/httpd/blazorapp-error.log
       CustomLog /var/log/httpd/blazorapp-access.log common
   </VirtualHost>
   ```

1. 將 Apache 設定檔案放在 `/etc/httpd/conf.d/` 目錄中，這是 CentOS 7 中的預設 Apache 設定目錄。

1. 將應用程式的檔案放入 `/var/www/blazorapp` 目錄中（在 `DocumentRoot` 設定檔中指定的位置）。

1. 重新開機 Apache 服務。

如需詳細資訊，請參閱 [`mod_mime`](https://httpd.apache.org/docs/2.4/mod/mod_mime.html) 和 [`mod_deflate`](https://httpd.apache.org/docs/current/mod/mod_deflate.html) 。

### <a name="github-pages"></a>GitHub Pages

若要處理 URL 重寫，請新增具有腳本的檔案，以處理將要求重新導向 `404.html` 至 `index.html` 頁面。 如需社群所提供的範例實作，請參閱 [GitHub Pages 的單一頁面應用程式](https://spa-github-pages.rafrex.com/) (GitHub 上的 [rafrex/spa-github-pages](https://github.com/rafrex/spa-github-pages#readme))。 使用社群方法的範例可在 [GitHub 上的 blazor-demo/blazor-demo.github.io](https://github.com/blazor-demo/blazor-demo.github.io) ([即時網站](https://blazor-demo.github.io/)) 看到。

當您使用專案網站而非組織網站時，請在中新增或更新 `<base>` 標記 `index.html` 。 將 `href` 屬性值設定為 GitHub 存放庫名稱，並在尾端加上斜線 (例如 `my-repository/`)。

## <a name="host-configuration-values"></a>主機組態值

[ Blazor WebAssembly apps](xref:blazor/hosting-models#blazor-webassembly)可以在開發環境中的執行時間，接受下列主機設定值做為命令列引數。

### <a name="content-root"></a>內容根目錄

`--contentroot`引數會設定包含應用程式內容檔案（[內容根](xref:fundamentals/index#content-root)）之目錄的絕對路徑。 在下列範例中，`/content-root-path` 是應用程式的內容根路徑。

* 在命令提示字元，於本機執行應用程式時傳遞引數。 從應用程式目錄，執行：

  ```dotnetcli
  dotnet run --contentroot=/content-root-path
  ```

* 在 IIS Express 設定檔中，將專案新增至應用程式的 `launchSettings.json` 檔案。 **IIS Express** 此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* 在 Visual Studio 中，于 [**屬性**] [  >  **調試**  >  **程式引數**] 中指定引數。 在 [Visual Studio] 屬性頁中設定引數，會將引數新增至檔案 `launchSettings.json` 。

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a>路徑基底

`--pathbase`引數會設定應用程式以非根相對 URL 路徑在本機執行的應用程式基底路徑（此標籤 `<base>` `href` 會設定為 `/` 預備和生產以外的路徑）。 在下列範例中，`/relative-URL-path` 是應用程式的路徑基底。 如需詳細資訊，請參閱[應用程式基底路徑](xref:blazor/host-and-deploy/index#app-base-path)。

> [!IMPORTANT]
> 不同於提供給 `<base>` 標籤 `href` 的路徑，在傳遞 `--pathbase` 引數值時請勿包含尾端的斜線 (`/`)。 如果應用程式基底路徑提供於 `<base>` 標籤作為 `<base href="/CoolApp/">` (包括尾端的斜線)，請將命令列引數值傳遞為 `--pathbase=/CoolApp` (不含尾端的斜線)。

* 在命令提示字元，於本機執行應用程式時傳遞引數。 從應用程式目錄，執行：

  ```dotnetcli
  dotnet run --pathbase=/relative-URL-path
  ```

* 在 IIS Express 設定檔中，將專案新增至應用程式的 `launchSettings.json` 檔案。 **IIS Express** 此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。

  ```json
  "commandLineArgs": "--pathbase=/relative-URL-path"
  ```

* 在 Visual Studio 中，于 [**屬性**] [  >  **調試**  >  **程式引數**] 中指定引數。 在 [Visual Studio] 屬性頁中設定引數，會將引數新增至檔案 `launchSettings.json` 。

  ```console
  --pathbase=/relative-URL-path
  ```

### <a name="urls"></a>URL

`--urls` 引數會搭配要用來接聽要求的連接埠和通訊協定，設定 IP 位址或主機位址。

* 在命令提示字元，於本機執行應用程式時傳遞引數。 從應用程式目錄，執行：

  ```dotnetcli
  dotnet run --urls=http://127.0.0.1:0
  ```

* 在 IIS Express 設定檔中，將專案新增至應用程式的 `launchSettings.json` 檔案。 **IIS Express** 此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* 在 Visual Studio 中，于 [**屬性**] [  >  **調試**  >  **程式引數**] 中指定引數。 在 [Visual Studio] 屬性頁中設定引數，會將引數新增至檔案 `launchSettings.json` 。

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="configure-the-linker"></a>設定連結器

Blazor在每個發行組建上執行中繼語言（IL）連結，以從輸出元件移除不必要的 IL。 如需詳細資訊，請參閱 <xref:blazor/host-and-deploy/configure-linker>。

## <a name="custom-boot-resource-loading"></a>自訂開機資源載入

BlazorWebAssembly 應用程式可以使用函式進行初始化， `loadBootResource` 以覆寫內建的開機資源載入機制。 `loadBootResource`在下列情況下使用：

* 允許使用者載入靜態資源，例如時區資料或 `dotnet.wasm` CDN。
* 使用 HTTP 要求載入壓縮的元件，並將它們解壓縮到不支援從伺服器提取壓縮內容的主機用戶端上。
* 將每個要求重新導向至新名稱，以將資源別名設為不同名稱 `fetch` 。

`loadBootResource`參數會出現在下表中。

| 參數    | 描述 |
| ------------ | ----------- |
| `type`       | 資源類型。 運算子類型： `assembly` 、 `pdb` 、 `dotnetjs` 、 `dotnetwasm` 、`timezonedata` |
| `name`       | 資源名稱。 |
| `defaultUri` | 資源的相對或絕對 URI。 |
| `integrity`  | 代表回應中預期內容的完整性字串。 |

`loadBootResource`傳回下列任何一項，以覆寫載入進程：

* URI 字串。 在下列範例（ `wwwroot/index.html` ）中，會從 CDN 提供下列檔案 `https://my-awesome-cdn.com/` ：

  * `dotnet.*.js`
  * `dotnet.wasm`
  * 時區資料

  ```html
  ...

  <script src="_framework/blazor.webassembly.js" autostart="false"></script>
  <script>
    Blazor.start({
      loadBootResource: function (type, name, defaultUri, integrity) {
        console.log(`Loading: '${type}', '${name}', '${defaultUri}', '${integrity}'`);
        switch (type) {
          case 'dotnetjs':
          case 'dotnetwasm':
          case 'timezonedata':
            return `https://my-awesome-cdn.com/blazorwebassembly/3.2.0/${name}`;
        }
      }
    });
  </script>
  ```

* `Promise<Response>`. `integrity`在標頭中傳遞參數，以保留預設的完整性檢查行為。

  下列範例（ `wwwroot/index.html` ）會將自訂 HTTP 標頭新增至輸出要求，並將參數傳遞至 `integrity` `fetch` 呼叫：
  
  ```html
  <script src="_framework/blazor.webassembly.js" autostart="false"></script>
  <script>
    Blazor.start({
      loadBootResource: function (type, name, defaultUri, integrity) {
        return fetch(defaultUri, { 
          cache: 'no-cache',
          integrity: integrity,
          headers: { 'MyCustomHeader': 'My custom value' }
        });
      }
    });
  </script>
  ```

* `null`/`undefined`，這會產生預設的載入行為。

外部來源必須傳回瀏覽器所需的 CORS 標頭，以允許跨原始來源資源載入。 根據預設，Cdn 通常會提供必要的標頭。

您只需要指定自訂行為的類型。 根據 `loadBootResource` 預設載入行為，架構會載入未指定的類型。

## <a name="change-the-filename-extension-of-dll-files"></a>變更 DLL 檔案的副檔名

如果您需要變更應用程式已發佈檔案的副檔名 `.dll` ，請遵循本節中的指導方針。

發行應用程式之後，請使用 shell 腳本或 DevOps 組建管線，將檔案重新命名 `.dll` 為使用不同的副檔名。 `.dll`以 `wwwroot` 應用程式已發佈輸出目錄中的檔案為目標（例如， `{CONTENT ROOT}/bin/Release/netstandard2.1/publish/wwwroot` ）。

在下列範例中，檔案 `.dll` 會重新命名為使用 `.bin` 副檔名。

在 Windows 上：

```powershell
dir .\_framework\_bin | rename-item -NewName { $_.name -replace ".dll\b",".bin" }
((Get-Content .\_framework\blazor.boot.json -Raw) -replace '.dll"','.bin"') | Set-Content .\_framework\blazor.boot.json
```

如果服務工作者資產也在使用中，請新增下列命令：

```powershell
((Get-Content .\service-worker-assets.js -Raw) -replace '.dll"','.bin"') | Set-Content .\service-worker-assets.js
```

在 Linux 或 macOS 上：

```console
for f in _framework/_bin/*; do mv "$f" "`echo $f | sed -e 's/\.dll\b/.bin/g'`"; done
sed -i 's/\.dll"/.bin"/g' _framework/blazor.boot.json
```

如果服務工作者資產也在使用中，請新增下列命令：

```console
sed -i 's/\.dll"/.bin"/g' service-worker-assets.js
```
   
若要使用不同的副檔名 `.bin` ，請 `.bin` 在上述命令中取代。

若要解決壓縮的 `blazor.boot.json.gz` 和檔案 `blazor.boot.json.br` ，請採用下列其中一種方法：

* 移除壓縮的 `blazor.boot.json.gz` 和檔案 `blazor.boot.json.br` 。 此方法會停用壓縮。
* 重新壓縮更新的檔案 `blazor.boot.json` 。

先前的指導方針也適用于服務工作者資產正在使用時。 移除或重新壓縮 `wwwroot/service-worker-assets.js.br` 和 `wwwroot/service-worker-assets.js.gz` 。 否則，瀏覽器中的檔案完整性檢查會失敗。

下列 Windows 範例會使用放在專案根目錄中的 PowerShell 腳本。

`ChangeDLLExtensions.ps1:`:

```powershell
param([string]$filepath,[string]$tfm)
dir $filepath\bin\Release\$tfm\wwwroot\_framework\_bin | rename-item -NewName { $_.name -replace ".dll\b",".bin" }
((Get-Content $filepath\bin\Release\$tfm\wwwroot\_framework\blazor.boot.json -Raw) -replace '.dll"','.bin"') | Set-Content $filepath\bin\Release\$tfm\wwwroot\_framework\blazor.boot.json
Remove-Item $filepath\bin\Release\$tfm\wwwroot\_framework\blazor.boot.json.gz
```

如果服務工作者資產也在使用中，請新增下列命令：

```powershell
((Get-Content $filepath\bin\Release\$tfm\wwwroot\service-worker-assets.js -Raw) -replace '.dll"','.bin"') | Set-Content $filepath\bin\Release\$tfm\wwwroot\service-worker-assets.js
```

在專案檔中，腳本會在發行應用程式之後執行：

```xml
<Target Name="ChangeDLLFileExtensions" AfterTargets="Publish" Condition="'$(Configuration)'=='Release'">
  <Exec Command="powershell.exe -command &quot;&amp; { .\ChangeDLLExtensions.ps1 '$(SolutionDir)' '$(TargetFramework)'}&quot;" />
</Target>
```

若要提供意見反應，請造訪[aspnetcore/問題 #5477](https://github.com/dotnet/aspnetcore/issues/5477)。
 
