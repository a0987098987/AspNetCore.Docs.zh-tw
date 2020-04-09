---
title: 託管與部署ASP.NET核心BlazorWeb 組裝
author: guardrex
description: 瞭解如何使用ASP.NET核心、內容傳遞Blazor網路 (CDN)、檔案伺服器和 GitHub 頁面託管和部署應用。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/webassembly
ms.openlocfilehash: f364d94085d175fde5596c222ef21852c0106ec1
ms.sourcegitcommit: 72792e349458190b4158fcbacb87caf3fc605268
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80751127"
---
# <a name="host-and-deploy-aspnet-core-opno-locblazor-webassembly"></a>託管與部署ASP.NET核心BlazorWeb 組裝

作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

與[BlazorWeb 群組載託管模型](xref:blazor/hosting-models#blazor-webassembly):

* 應用Blazor、其依賴項和 .NET 運行時將下載到瀏覽器。
* 應用程式會直接在瀏覽器 UI 執行緒上執行。

支援以下部署政策:

* 該應用程式Blazor由ASP.NET核心應用提供。 此策略已於[搭配 ASP.NET Core 的已裝載部署](#hosted-deployment-with-aspnet-core)一節中涵蓋。
* 該應用程式Blazor放置在靜態託管 Web 伺服器或服務上,其中 .NETBlazor不用於為 應用提供服務。 此策略在[「獨立部署](#standalone-deployment)」部分中介紹,其中包括Blazor有關將 WebAssembly 應用託管為 IIS 子應用的資訊。

## <a name="rewrite-urls-for-correct-routing"></a>重寫 URL 以便正確地路由

Blazor WebAssembly 應用中的頁面元件的路由請求不像伺服器託管應用中的Blazor路由請求那樣簡單。 考慮具有Blazor兩個元件的 WebAssembly 應用:

* *Main.razor* &ndash; 載入應用程式根目錄，同時包含 `About` 元件 (`href="About"`) 的連結。
* *About.razor* &ndash; `About` 元件。

使用瀏覽器的網址列要求應用程式預設文件時 (例如 `https://www.contoso.com/`)：

1. 瀏覽器提出要求。
1. 傳回預設頁面，這通常是 *index.html*。
1. *index.html* 啟動載入應用程式。
1. Blazor路由器負載,並呈現 Razor`Main`元件。

在「主頁」 中`About`, 選擇指向元件的連結在用戶端上有效Blazor, 因為路由器阻止瀏覽器在 Internet`About`上向`About``www.contoso.com`for 和提供呈現元件本身的請求。 WebAssembly 應用中的所有內部終結點請求的工作方式相同:請求不會觸發對 Internet 上伺服器託管資源的基於流覽*Blazor* 器的請求。 路由器會在內部處理要求。

如果使用瀏覽器之網址列提出對 `www.contoso.com/About` 的要求，則要求會失敗。 在應用程式的網際網路主機上沒有這類資源存在，因此會傳回「404 - 找不到」** 的回應。

因為瀏覽器會提出要求給以網際網路為基礎的主機以取得用戶端頁面，所以網頁伺服器和裝載服務必須針對實際上不在伺服器上資源，將所有要求重新撰寫至 *index.html* 頁面。 返回*索引.html*時,Blazor應用的 路由器接管並回應正確的資源。

部署到IIS伺服器時,可以將URL重寫模組與應用發佈的*Web.config檔*一起使用。 有關詳細資訊,請參閱[IIS](#iis)部分。

## <a name="hosted-deployment-with-aspnet-core"></a>搭配 ASP.NET Core 的已裝載部署

*託管部署*將BlazorWebAssembly 應用從在 Web 伺服器上運行[ASP.NET 核心應用](xref:index)的瀏覽器提供服務。

用戶端BlazorWebAssembly 應用將發佈到伺服器應用的 */bin/發佈/[目標框架]/發佈/wwwroot*資料夾中,以及伺服器應用的任何其他靜態 Web 資源。 兩個應用程式一起部署。 需要有能夠裝載 ASP.NET Core 應用程式的網頁伺服器。 對於託管部署,Visual Studio 包括`-ho|--hosted``dotnet new`**BlazorWebAssembly 應用**專案`blazorwasm`樣本(使用[dotnet 新](/dotnet/core/tools/dotnet-new)指令時的範本),並選擇了 **『託管**』選項( 使用該 命令時)。

如需 ASP.NET Core 應用程式裝載和部署的詳細資訊，請參閱 <xref:host-and-deploy/index>。

如需部署至 Azure App Service 的相關資訊，請參閱 <xref:tutorials/publish-to-azure-webapp-using-vs>。

## <a name="standalone-deployment"></a>獨立部署

*獨立部署*Blazor將 WebAssembly 應用作為用戶端直接請求的一組靜態檔提供服務。 任何靜態文件伺服器都能夠為Blazor應用提供服務。

獨立部署資產發佈到 */bin/發佈/[目標框架]/發佈/wwwroot*檔夾中。

### <a name="iis"></a>IIS

IIS 是可用於應用Blazor的可運行靜態文件伺服器。 要將 IISBlazor設定為主機,請參閱 在[IIS 上建構靜態網站](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis)。

已發行的資產會建立在 */bin/Release/{TARGET FRAMEWORK}/publish* 資料夾中。 在網頁伺服器或裝載服務上，裝載 *publish* 資料夾的內容。

#### <a name="webconfig"></a>web.config

在Blazor清單中使用以下 IIS 設定建立*Web.config*檔:

* 針對下列副檔名設定 MIME 類型：
  * *.dll* &ndash;`application/octet-stream`
  * *.json* &ndash;`application/json`
  * *.wasm* &ndash; `application/wasm`
  * *.woff* &ndash; `application/font-woff`
  * *.woff2* &ndash; `application/font-woff`
* 針對下列 MIME 類型會啟用 HTTP 壓縮：
  * `application/octet-stream`
  * `application/wasm`
* 建立 URL Rewrite Module 規則：
  * 提供應用靜態資產所在的子目錄 *(wwwroot/[PATH 請求])。*
  * 創建 SPA 回退路由,以便將非文件資產的請求重定向到其靜態資產資料夾 *(wwwroot/index.html*) 中的應用默認文檔。
  
#### <a name="use-a-custom-webconfig"></a>使用自訂 Web.config

要使用自訂*Web.config*檔,

1. 將自訂*Web.config*檔案放在專案資料夾的根目錄。
1. 將以下目標新增到項目檔 *(.csproj*):

   ```xml
   <Target Name="CopyWebConfigOnPublish" AfterTargets="Publish">
     <Copy SourceFiles="web.config" DestinationFolder="$(PublishDir)" />
   </Target>
   ```
   
> [!NOTE]
> WebAssembly 應用中不支援`<IsWebConfigTransformDisabled>`使用 MSBuild`true`屬性,[因為它適用於部署到 IIS 的 ASP.NET 核心應用。](xref:host-and-deploy/iis/index#webconfig-file) Blazor 有關詳細資訊,請參閱[提供自Blazor訂 WASM Web.config(dotnet/aspnetcore #20569)所需的複製目標](https://github.com/dotnet/aspnetcore/issues/20569)。

#### <a name="install-the-url-rewrite-module"></a>安裝 URL Rewrite 模組

需要 [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite)，才可重寫 URL。 預設不會安裝此模組，且其無法用來安裝為網頁伺服器 (IIS) 角色服務功能。 必須從 IIS 網站下載模組。 請使用 Web Platform Installer 安裝模組：

1. 在本機上，巡覽至 [URL Rewrite Module 下載頁面](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads)。 如需英文版，請選取 [WebPI]**** 下載 WebPI 安裝程式。 如需其他語言，請選取適當的伺服器架構 (x86 x64) 來下載安裝程式。
1. 將安裝程式複製到伺服器。 執行安裝程式。 選取 [安裝]**** 按鈕，並接受授權條款。 安裝完成之後，不需要重新啟動伺服器。

#### <a name="configure-the-website"></a>設定網站

將網站的 [實體路徑]**** 設為應用程式的資料夾。 此資料夾包含：

* IIS 用來設定網站的 *web.config* 檔，包括必要的重新導向規則和檔案內容類型。
* 應用程式的靜態資產資料夾。

#### <a name="host-as-an-iis-sub-app"></a>主機作為 IIS 子應用

如果獨立應用作為 IIS 子應用託管,請執行以下任一操作:

* 禁用繼承的ASP.NET核心模組處理程式。

  以新增檔案新增部份Blazor`<handlers>`來移除應用程式已發布*的 Web.config*檔案中的處理程式:

  ```xml
  <handlers>
    <remove name="aspNetCore" />
  </handlers>
  ```

* 使用`<system.webServer>``<location>`設定為的元素關閉根(父)應用部份的繼承`false` `inheritInChildApplications`

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

除了[配置應用的基本路徑](xref:host-and-deploy/blazor/index#app-base-path)外,還執行刪除處理程式或禁用繼承。 在應用程式的 *index.html* 檔案，將應用程式基底路徑設為在 IIS 中設定子應用程式時使用的 IIS 別名。

#### <a name="troubleshooting"></a>疑難排解

如果收到「500 - 內部伺服器錯誤」**，且 IIS 管理員在嘗試存取網站設定時擲回錯誤，請確認是否已安裝 URL Rewrite 模組。 未安裝此模組時，IIS 無法剖析 *web.config* 檔案。 這可以防止 IIS 管理器載入網站的配置和Blazor網站提供 靜態檔。

如需針對部署至 IIS 進行疑難排解的詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis>。

### <a name="azure-storage"></a>Azure 儲存體

[Azure 儲存](/azure/storage/)靜態檔案託管允許Blazor無 伺服器應用託管。 支援自訂網域名稱、Azure 內容傳遞網路 (CDN) 及 HTTPS。

當 Blob 服務針對儲存體帳戶上的靜態網站裝載啟用時：

* 將 [索引文件名稱]**** 設定為 `index.html`。
* 將 [錯誤文件路徑]**** 設定為 `index.html`。 Razor 元件和其他非檔案端點不會位於由 Blob 服務所存放之靜態內容中的實體路徑上。 當收到Blazor路由器應處理的這些資源之一的請求時,blob 服務產生的*404 - 未找到*錯誤將請求路由到**錯誤文件路徑**。 傳*回 index.html* Blazorblob, 路由器載入和處理路徑。

如需詳細資訊，請參閱 [Azure 儲存體中的靜態網站裝載](/azure/storage/blobs/storage-blob-static-website)。

### <a name="nginx"></a>Nginx

以下*nginx.conf*檔被簡化,以展示如何設定 Nginx 以在磁碟上找不到相應的檔時發送*index.html*檔。

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

要使用BlazorNginx 在 Docker 中託管,請使用 Docker 檔來使用基於阿爾卑斯的 Nginx 映射。 更新 Dockerfile，將 *nginx.config* 檔案複製到容器內。

如下列範例所示，新增一行至 Dockerfile：

```dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="apache"></a>Apache

要將BlazorWeb 組裝應用部署到 CentOS 7 或更高版本:

1. 建立 Apache 設定檔。 下面的範例是一個簡化的設定檔 (*blazorapp.config*):

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

1. 將 Apache 設定`/etc/httpd/conf.d/`檔放入 目錄中,該目錄是 CentOS 7 中的預設 Apache 設定目錄。

1. 將應用的檔案放入`/var/www/blazorapp`目錄中(在配置`DocumentRoot`檔中指定的位置)。

1. 重新啟動 Apache 服務。

有關詳細資訊,請參閱[mod_mime](https://httpd.apache.org/docs/2.4/mod/mod_mime.html)和[mod_deflate](https://httpd.apache.org/docs/current/mod/mod_deflate.html)。

### <a name="github-pages"></a>GitHub Pages

若要處理 URL 重寫，請新增 *404.html* 檔，以及處理將要求重新導向至 *index.html* 頁面的指令碼。 如需社群所提供的範例實作，請參閱 [GitHub Pages 的單一頁面應用程式](https://spa-github-pages.rafrex.com/) (GitHub 上的 [rafrex/spa-github-pages](https://github.com/rafrex/spa-github-pages#readme))。 使用社群方法的範例可在 [GitHub 上的 blazor-demo/blazor-demo.github.io](https://github.com/blazor-demo/blazor-demo.github.io) ([即時網站](https://blazor-demo.github.io/)) 看到。

使用專案網站，而非組織網站時，請在 *index.html* 中新增或更新 `<base>` 標籤。 將 `href` 屬性值設定為 GitHub 存放庫名稱，並在尾端加上斜線 (例如 `my-repository/`)。

## <a name="host-configuration-values"></a>主機組態值

WebAssembly 應用可以接受以下主機配置值作為開發環境中運行時的命令列參數[Blazor](xref:blazor/hosting-models#blazor-webassembly)。

### <a name="content-root"></a>內容根目錄

參數`--contentroot`設定包含應用內容檔([內容根](xref:fundamentals/index#content-root))的目錄的絕對路徑。 在下列範例中，`/content-root-path` 是應用程式的內容根路徑。

* 在命令提示字元，於本機執行應用程式時傳遞引數。 從應用程式目錄，執行：

  ```dotnetcli
  dotnet run --contentroot=/content-root-path
  ```

* 新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。 此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* 在視覺化工作室中,在**屬性** > **除錯** > **應用程式參數中指定參數**。 在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a>路徑基底

參數`--pathbase`設置使用非根相對 URL 路徑在本地運行的應用的應用`<base>`基本路徑`href`( 標記 設置為`/`過渡和生產以外的 路徑)。 在下列範例中，`/relative-URL-path` 是應用程式的路徑基底。 有關詳細資訊,請參閱[應用基本路徑](xref:host-and-deploy/blazor/index#app-base-path)。

> [!IMPORTANT]
> 不同於提供給 `<base>` 標籤 `href` 的路徑，在傳遞 `--pathbase` 引數值時請勿包含尾端的斜線 (`/`)。 如果應用程式基底路徑提供於 `<base>` 標籤作為 `<base href="/CoolApp/">` (包括尾端的斜線)，請將命令列引數值傳遞為 `--pathbase=/CoolApp` (不含尾端的斜線)。

* 在命令提示字元，於本機執行應用程式時傳遞引數。 從應用程式目錄，執行：

  ```dotnetcli
  dotnet run --pathbase=/relative-URL-path
  ```

* 新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。 此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。

  ```json
  "commandLineArgs": "--pathbase=/relative-URL-path"
  ```

* 在視覺化工作室中,在**屬性** > **除錯** > **應用程式參數中指定參數**。 在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。

  ```console
  --pathbase=/relative-URL-path
  ```

### <a name="urls"></a>URL

`--urls` 引數會搭配要用來接聽要求的連接埠和通訊協定，設定 IP 位址或主機位址。

* 在命令提示字元，於本機執行應用程式時傳遞引數。 從應用程式目錄，執行：

  ```dotnetcli
  dotnet run --urls=http://127.0.0.1:0
  ```

* 新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。 此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* 在視覺化工作室中,在**屬性** > **除錯** > **應用程式參數中指定參數**。 在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="configure-the-linker"></a>設定連結器

Blazor在每個版本生成上執行中間語言 (IL) 連結,從輸出程式集中刪除不必要的 IL。 如需詳細資訊，請參閱 <xref:host-and-deploy/blazor/configure-linker>。
