---
title: 裝載和部署 ASP.NET Core Blazor WebAssembly
author: guardrex
description: 瞭解如何使用 ASP.NET Core、內容傳遞網路（CDN）、檔案伺服器和 GitHub 頁面來裝載和部署 Blazor 應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/11/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/webassembly
ms.openlocfilehash: 748ac9969134f4c89cc8c1235958dcc7ac1d1080
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434274"
---
# <a name="host-and-deploy-aspnet-core-opno-locblazor-webassembly"></a>裝載和部署 ASP.NET Core Blazor WebAssembly

作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

使用[Blazor WebAssembly 裝載模型](xref:blazor/hosting-models#blazor-webassembly)：

* Blazor 應用程式、其相依性和 .NET 執行時間會下載至瀏覽器。
* 應用程式會直接在瀏覽器 UI 執行緒上執行。

支援下列部署策略：

* Blazor 應用程式是由 ASP.NET Core 應用程式提供。 此策略已於[搭配 ASP.NET Core 的已裝載部署](#hosted-deployment-with-aspnet-core)一節中涵蓋。
* Blazor 應用程式會放在靜態裝載的 web 伺服器或服務上，其中 .NET 不會用來服務 Blazor 應用程式。 此策略涵蓋于[獨立部署](#standalone-deployment)一節，其中包含將 Blazor WebAssembly 應用程式裝載為 IIS 子應用程式的相關資訊。

## <a name="rewrite-urls-for-correct-routing"></a>重寫 URL 以便正確地路由

Blazor WebAssembly 應用程式中頁面元件的路由要求，並不像 Blazor 伺服器（裝載的應用程式）中的路由要求一樣簡單。 請考慮具有兩個元件的 Blazor WebAssembly 應用程式：

* *主要的 razor* &ndash; 會在應用程式的根目錄載入，並包含 `About` 元件（`href="About"`）的連結。
* *關於 razor* &ndash; `About` 元件。

使用瀏覽器的網址列要求應用程式預設文件時 (例如 `https://www.contoso.com/`)：

1. 瀏覽器提出要求。
1. 傳回預設頁面，這通常是 *index.html*。
1. *index.html* 啟動載入應用程式。
1. Blazor的路由器載入，並轉譯 Razor `Main` 元件。

在主頁面中，選取 `About` 元件的連結可在用戶端上運作，因為 Blazor 路由器會停止瀏覽器在網際網路上提出要求，以 `www.contoso.com` `About`，並提供轉譯的 `About` 元件本身。 *Blazor WebAssembly 應用程式內*內部端點的所有要求都是以相同方式執行：要求不會對網際網路上伺服器裝載的資源觸發瀏覽器型要求。 路由器會在內部處理要求。

如果使用瀏覽器之網址列提出對 `www.contoso.com/About` 的要求，則要求會失敗。 在應用程式的網際網路主機上沒有這類資源存在，因此會傳回「404 - 找不到」的回應。

因為瀏覽器會提出要求給以網際網路為基礎的主機以取得用戶端頁面，所以網頁伺服器和裝載服務必須針對實際上不在伺服器上資源，將所有要求重新撰寫至 *index.html* 頁面。 當傳回*index. html*時，應用程式的 Blazor 路由器會接管並以正確的資源回應。

部署到 IIS 伺服器時，您可以使用 URL 重寫模組搭配應用*程式已發佈的 web.config 檔案*。 如需詳細資訊，請參閱[IIS](#iis)一節。

## <a name="hosted-deployment-with-aspnet-core"></a>搭配 ASP.NET Core 的已裝載部署

裝載的*部署*可從在 web 伺服器上執行的[ASP.NET Core 應用程式](xref:index)，將 Blazor WebAssembly 應用程式提供給瀏覽器。

用戶端 Blazor WebAssembly 應用程式會發佈到伺服器應用程式的 */BIN/RELEASE/{TARGET FRAMEWORK}/publish/wwwroot*資料夾，以及伺服器應用程式的任何其他靜態 web 資產。 這兩個應用程式會一起部署。 需要有能夠裝載 ASP.NET Core 應用程式的網頁伺服器。 針對裝載的部署，Visual Studio 包括 **Blazor WebAssembly 應用程式**專案範本（使用[dotnet new](/dotnet/core/tools/dotnet-new)命令時的`blazorwasm` 範本）和選取的**裝載選項（** 使用 `dotnet new` 命令時`-ho|--hosted`）。

如需 ASP.NET Core 應用程式裝載和部署的詳細資訊，請參閱 <xref:host-and-deploy/index>。

如需部署至 Azure App Service 的相關資訊，請參閱 <xref:tutorials/publish-to-azure-webapp-using-vs>。

## <a name="standalone-deployment"></a>獨立部署

*獨立部署*會將 Blazor WebAssembly 應用程式當做一組直接由用戶端要求的靜態檔案來提供。 任何靜態檔案伺服器都可以服務 Blazor 應用程式。

獨立部署資產會發佈到 */BIN/RELEASE/{TARGET FRAMEWORK}/publish/wwwroot*資料夾中。

### <a name="iis"></a>IIS

IIS 是適用于 Blazor 應用程式的靜態檔案伺服器。 若要設定 IIS 以裝載 Blazor，請參閱[在 iis 上建立靜態網站](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis)。

已發行的資產會建立在 */bin/Release/{TARGET FRAMEWORK}/publish* 資料夾中。 在網頁伺服器或裝載服務上，裝載 *publish* 資料夾的內容。

#### <a name="webconfig"></a>web.config

發行 Blazor 專案時，會使用下列 IIS 設定來建立*web.config*檔案：

* 針對下列副檔名設定 MIME 類型：
  * *.dll* &ndash; `application/octet-stream`
  * &ndash; `application/json`*的 json*
  * *. wasm* &ndash; `application/wasm`
  * *. woff* &ndash; `application/font-woff`
  * *. woff2* &ndash; `application/font-woff`
* 針對下列 MIME 類型會啟用 HTTP 壓縮：
  * `application/octet-stream`
  * `application/wasm`
* 建立 URL Rewrite Module 規則：
  * 提供應用程式靜態資產所在的子目錄 ( *{組件名稱}/dist/{要求的路徑}* )。
  * 建立 SPA 後援路由，讓非檔案資產要求重新導向至其靜態資產資料夾中的應用程式預設文件 ( *{組件名稱}/dist/index.html*)。

#### <a name="install-the-url-rewrite-module"></a>安裝 URL Rewrite 模組

需要 [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite)，才可重寫 URL。 預設不會安裝此模組，且其無法用來安裝為網頁伺服器 (IIS) 角色服務功能。 必須從 IIS 網站下載模組。 請使用 Web Platform Installer 安裝模組：

1. 在本機上，巡覽至 [URL Rewrite Module 下載頁面](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads)。 如需英文版，請選取 [WebPI] 下載 WebPI 安裝程式。 如需其他語言，請選取適當的伺服器架構 (x86 x64) 來下載安裝程式。
1. 將安裝程式複製到伺服器。 執行安裝程式。 選取 [安裝] 按鈕，並接受授權條款。 安裝完成之後，不需要重新啟動伺服器。

#### <a name="configure-the-website"></a>設定網站

將網站的 [實體路徑] 設為應用程式的資料夾。 此資料夾包含：

* IIS 用來設定網站的 *web.config* 檔，包括必要的重新導向規則和檔案內容類型。
* 應用程式的靜態資產資料夾。

#### <a name="host-as-an-iis-sub-app"></a>以 IIS 子應用程式的形式裝載

如果獨立應用程式裝載為 IIS 子應用程式，請執行下列其中一項：

* 停用繼承的 ASP.NET Core 模組處理常式。

  將 `<handlers>` 區段新增至檔案，以移除 Blazor 應用程式的已發行*web.config*檔案中的處理常式：

  ```xml
  <handlers>
    <remove name="aspNetCore" />
  </handlers>
  ```

* 使用 `<location>` 元素（`inheritInChildApplications` 設定為 `false`）停用根（父系）應用程式 `<system.webServer>` 區段的繼承：

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

除了設定[應用程式的基底路徑](xref:host-and-deploy/blazor/index#app-base-path)之外，也會執行移除處理常式或停用繼承。 在應用程式的 *index.html* 檔案，將應用程式基底路徑設為在 IIS 中設定子應用程式時使用的 IIS 別名。

#### <a name="troubleshooting"></a>疑難排解

如果收到「500 - 內部伺服器錯誤」，且 IIS 管理員在嘗試存取網站設定時擲回錯誤，請確認是否已安裝 URL Rewrite 模組。 未安裝此模組時，IIS 無法剖析 *web.config* 檔案。 這可防止 IIS 管理員載入網站的設定和網站，使其無法提供 Blazor的靜態檔案。

如需針對部署至 IIS 進行疑難排解的詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis>。

### <a name="azure-storage"></a>Azure 儲存體

[Azure 儲存體](/azure/storage/)靜態檔案裝載允許裝載無伺服器 Blazor 應用程式。 支援自訂網域名稱、Azure 內容傳遞網路 (CDN) 及 HTTPS。

當 Blob 服務針對儲存體帳戶上的靜態網站裝載啟用時：

* 將 [索引文件名稱] 設定為 `index.html`。
* 將 [錯誤文件路徑] 設定為 `index.html`。 Razor 元件和其他非檔案端點不會位於由 Blob 服務所存放之靜態內容中的實體路徑上。 收到 Blazor 路由器應處理的其中一個資源的要求時，由 blob 服務產生的*404-找不*到的錯誤會將要求路由傳送至**錯誤檔路徑**。 會傳回*索引 .html* blob，而 Blazor 路由器會載入並處理路徑。

如需詳細資訊，請參閱 [Azure 儲存體中的靜態網站裝載](/azure/storage/blobs/storage-blob-static-website)。

### <a name="nginx"></a>Nginx

下列 *nginx.conf* 檔案已簡化，示範如何將 Nginx 設定為每當找不到磁碟上的對應檔案時，便傳送 *Index.html* 檔案。

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

若要使用 Nginx 在 Docker 中裝載 Blazor，請將 Dockerfile 設定為使用以 Alpine 為基礎的 Nginx 映射。 更新 Dockerfile，將 *nginx.config* 檔案複製到容器內。

如下列範例所示，新增一行至 Dockerfile：

```dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="apache"></a>Apache

若要將 Blazor WebAssembly 應用程式部署至 CentOS 7 或更新版本：

1. 建立 Apache 設定檔。 下列範例是一個簡化的設定檔案（*blazorapp*）：

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

1. 將 Apache 設定檔放在 `/etc/httpd/conf.d/` 目錄中，這是 CentOS 7 中的預設 Apache 設定目錄。

1. 將應用程式的檔案放入 `/var/www/blazorapp` 目錄（指定要在設定檔中 `DocumentRoot` 的位置）。

1. 重新開機 Apache 服務。

如需詳細資訊，請參閱[mod_mime](https://httpd.apache.org/docs/2.4/mod/mod_mime.html)和[mod_deflate](https://httpd.apache.org/docs/current/mod/mod_deflate.html)。

### <a name="github-pages"></a>GitHub Pages

若要處理 URL 重寫，請新增 *404.html* 檔，以及處理將要求重新導向至 *index.html* 頁面的指令碼。 如需社群所提供的範例實作，請參閱 [GitHub Pages 的單一頁面應用程式](https://spa-github-pages.rafrex.com/) (GitHub 上的 [rafrex/spa-github-pages](https://github.com/rafrex/spa-github-pages#readme))。 使用社群方法的範例可在 [GitHub 上的 blazor-demo/blazor-demo.github.io](https://github.com/blazor-demo/blazor-demo.github.io) ([即時網站](https://blazor-demo.github.io/)) 看到。

使用專案網站，而非組織網站時，請在 `<base>`index.html*中新增或更新* 標籤。 將 `href` 屬性值設定為 GitHub 存放庫名稱，並在尾端加上斜線 (例如 `my-repository/`)。

## <a name="host-configuration-values"></a>主機組態值

[Blazor WebAssembly 應用程式](xref:blazor/hosting-models#blazor-webassembly)可以在開發環境中的執行時間，接受下列主機設定值做為命令列引數。

### <a name="content-root"></a>內容根目錄

`--contentroot` 引數會設定包含應用程式內容檔案（[內容根](xref:fundamentals/index#content-root)）之目錄的絕對路徑。 在下列範例中，`/content-root-path` 是應用程式的內容根路徑。

* 在命令提示字元，於本機執行應用程式時傳遞引數。 從應用程式目錄，執行：

  ```dotnetcli
  dotnet run --contentroot=/content-root-path
  ```

* 新增項目至應用程式 *IIS Express* 設定檔中的 **launchSettings.json** 檔案。 此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* 在 Visual Studio 中，在 [屬性] > [偵錯] > [應用程式引數] 中指定引數。 在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a>路徑基底

`--pathbase` 引數會設定應用程式以非根相對 URL 路徑在本機執行的應用程式基底路徑（`<base>` 標記 `href` 設定為預備和生產 `/` 以外的路徑）。 在下列範例中，`/relative-URL-path` 是應用程式的路徑基底。 如需詳細資訊，請參閱[應用程式基底路徑](xref:host-and-deploy/blazor/index#app-base-path)。

> [!IMPORTANT]
> 不同於提供給 `href` 標籤 `<base>` 的路徑，在傳遞 `/` 引數值時請勿包含尾端的斜線 (`--pathbase`)。 如果應用程式基底路徑提供於 `<base>` 標籤作為 `<base href="/CoolApp/">` (包括尾端的斜線)，請將命令列引數值傳遞為 `--pathbase=/CoolApp` (不含尾端的斜線)。

* 在命令提示字元，於本機執行應用程式時傳遞引數。 從應用程式目錄，執行：

  ```dotnetcli
  dotnet run --pathbase=/relative-URL-path
  ```

* 新增項目至應用程式 *IIS Express* 設定檔中的 **launchSettings.json** 檔案。 此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。

  ```json
  "commandLineArgs": "--pathbase=/relative-URL-path"
  ```

* 在 Visual Studio 中，在 [屬性] > [偵錯] > [應用程式引數] 中指定引數。 在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。

  ```console
  --pathbase=/relative-URL-path
  ```

### <a name="urls"></a>URL

`--urls` 引數會搭配要用來接聽要求的連接埠和通訊協定，設定 IP 位址或主機位址。

* 在命令提示字元，於本機執行應用程式時傳遞引數。 從應用程式目錄，執行：

  ```dotnetcli
  dotnet run --urls=http://127.0.0.1:0
  ```

* 新增項目至應用程式 *IIS Express* 設定檔中的 **launchSettings.json** 檔案。 此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* 在 Visual Studio 中，在 [屬性] > [偵錯] > [應用程式引數] 中指定引數。 在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="configure-the-linker"></a>設定連結器

Blazor 會在每個發行組建上執行中繼語言（IL）連結，以從輸出元件中移除不必要的 IL。 如需詳細資訊，請參閱 <xref:host-and-deploy/blazor/configure-linker>。
