---
title: 裝載及部署 ASP.NET Core Blazor 用戶端
author: guardrex
description: 了解如何使用 ASP.NET Core、內容傳遞網路 (CDN)、檔案伺服器和 GitHub Pages 來裝載和部署 Blazor 應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: host-and-deploy/blazor/client-side
ms.openlocfilehash: c9822205d38f765cf80748bc2b379c11ec7c1c57
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773590"
---
# <a name="host-and-deploy-aspnet-core-blazor-client-side"></a>裝載及部署 ASP.NET Core Blazor 用戶端

作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)

使用[用戶端裝載模型](xref:blazor/hosting-models#client-side)：

* Blazor 應用程式、其相依性，和 .NET 執行階段會下載至瀏覽器中。
* 應用程式會直接在瀏覽器 UI 執行緒上執行。

支援下列部署策略：

* Blazor 應用程式由 ASP.NET Core 應用程式提供服務。 此策略已於[搭配 ASP.NET Core 的已裝載部署](#hosted-deployment-with-aspnet-core)一節中涵蓋。
* Blazor 應用程式放在靜態裝載的網頁伺服器或服務上，其中 .NET 不會用來提供服務給 Blazor 應用程式。 此策略涵蓋于[獨立部署](#standalone-deployment)一節，其中包含將 Blazor 用戶端應用程式裝載為 IIS 子應用程式的相關資訊。

## <a name="rewrite-urls-for-correct-routing"></a>重寫 URL 以便正確地路由

用戶端應用程式中頁面元件的路由要求，與伺服器端託管應用程式中的路由要求並不簡單。 請考慮具有兩個元件的用戶端應用程式：

* *Main.razor* &ndash; 載入應用程式根目錄，同時包含 `About` 元件 (`href="About"`) 的連結。
* *About.razor* &ndash; `About` 元件。

使用瀏覽器的網址列要求應用程式預設文件時 (例如 `https://www.contoso.com/`)：

1. 瀏覽器提出要求。
1. 傳回預設頁面，這通常是 *index.html*。
1. *index.html* 啟動載入應用程式。
1. Blazor 的路由器會載入，且會轉譯 Razor `Main` 元件。

在主頁面中，選取用戶端 `About` 元件的連結有用，因為 Blazor 路由器會停止瀏覽器，使其不再從網際網路對 `www.contoso.com` 提出針對 `About` 的要求，並自行提供轉譯的 `About` 元件。 對於「用戶端應用程式內」內部端點的所有要求運作方式相同：要求不會對於網際網路上伺服器裝載的資源，觸發以瀏覽器為基礎的要求。 路由器會在內部處理要求。

如果使用瀏覽器之網址列提出對 `www.contoso.com/About` 的要求，則要求會失敗。 在應用程式的網際網路主機上沒有這類資源存在，因此會傳回「404 - 找不到」的回應。

因為瀏覽器會提出要求給以網際網路為基礎的主機以取得用戶端頁面，所以網頁伺服器和裝載服務必須針對實際上不在伺服器上資源，將所有要求重新撰寫至 *index.html* 頁面。 傳回 *index.html* 時，應用程式的用戶端路由器會接管，並以正確的資源回應。

部署到 IIS 伺服器時，您可以使用 URL 重寫模組搭配應用*程式已發佈的 web.config 檔案*。 如需詳細資訊，請參閱[IIS](#iis)一節。

## <a name="hosted-deployment-with-aspnet-core"></a>搭配 ASP.NET Core 的已裝載部署

「裝載部署」會將 Blazor 用戶端應用程式從 Web 伺服器上執行的 [ASP.NET Core 應用程式](xref:index)提供給瀏覽器。

Blazor 應用程式隨附於發佈輸出中的 ASP.NET Core 應用程式，以便兩個應用程式會一起部署。 需要有能夠裝載 ASP.NET Core 應用程式的網頁伺服器。 針對裝載部署，Visual Studio 包含 **Blazor WebAssembly 應用程式**專案範本 (使用 [dotnet new](/dotnet/core/tools/dotnet-new) 命令時的 `blazorwasm` 範本)，並已選取 [已裝載] 選項。

如需 ASP.NET Core 應用程式裝載和部署的詳細資訊，請參閱 <xref:host-and-deploy/index>。

如需部署至 Azure App Service 的相關資訊，請參閱 <xref:tutorials/publish-to-azure-webapp-using-vs>。

## <a name="standalone-deployment"></a>獨立部署

「獨立部署」會以一組直接由用戶端要求的靜態檔案來提供服務給 Blazor 用戶端應用程式。 所有靜態檔案伺服器都能夠支援 Blazor 應用程式。

獨立式部署資產會發佈至 [bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist] 資料夾。

### <a name="iis"></a>IIS

IIS 是足以支援 Blazor 應用程式的靜態檔案伺服器。 若要設定 IIS 來裝載 Blazor，請參閱[在 IIS 上建置靜態網站](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis)。

已發行的資產會建立在 */bin/Release/{TARGET FRAMEWORK}/publish* 資料夾中。 在網頁伺服器或裝載服務上，裝載 *publish* 資料夾的內容。

#### <a name="webconfig"></a>web.config

發佈 Blazor 專案時，會建立 *web.config* 檔案，並使用下列 IIS 組態：

* 針對下列副檔名設定 MIME 類型：
  * *.dll* &ndash; `application/octet-stream`
  * *.json* &ndash; `application/json`
  * *.wasm* &ndash; `application/wasm`
  * *.woff* &ndash; `application/font-woff`
  * *.woff2* &ndash; `application/font-woff`
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

  將`<handlers>`區段新增至檔案，以移除 Blazor 應用*程式已發佈的 web.config 檔案*中的處理常式：

  ```xml
  <handlers>
    <remove name="aspNetCore" />
  </handlers>
  ```

* `<system.webServer>` `inheritInChildApplications`使用設定為的`false`元素，停用根（父系）應用程式區段`<location>`的繼承：

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

如果收到「500 - 內部伺服器錯誤」，且 IIS 管理員在嘗試存取網站設定時擲回錯誤，請確認是否已安裝 URL Rewrite 模組。 未安裝此模組時，IIS 無法剖析 *web.config* 檔案。 這導致 IIS 管理員無法載入網站的組態，且網站無法提供 Blazor 的靜態檔案。

如需針對部署至 IIS 進行疑難排解的詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis>。

### <a name="azure-storage"></a>Azure 儲存體

[Azure 儲存體](/azure/storage/)靜態檔案裝載允許裝載無伺服器 Blazor 應用程式。 支援自訂網域名稱、Azure 內容傳遞網路 (CDN) 及 HTTPS。

當 Blob 服務針對儲存體帳戶上的靜態網站裝載啟用時：

* 將 [索引文件名稱] 設定為 `index.html`。
* 將 [錯誤文件路徑] 設定為 `index.html`。 Razor 元件和其他非檔案端點不會位於由 Blob 服務所存放之靜態內容中的實體路徑上。 當系統接收到針對這些資源之一，且應由 Blazor 路由器處理的要求時，由 Blob 服務所產生的「404 - 找不到」錯誤會將要求路由至**錯誤文件路徑**。 系統會傳回 *index.html* Blob，且 Blazor 路由器會載入並處理該路徑。

如需詳細資訊，請參閱[Azure 儲存體中的靜態網站代管](/azure/storage/blobs/storage-blob-static-website)。

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

若要在 Docker 中使用 Nginx 裝載 Blazor，請設定 Dockerfile 以使用以 Alpine 為基礎的 Nginx 映像。 更新 Dockerfile，將 *nginx.config* 檔案複製到容器內。

如下列範例所示，新增一行至 Dockerfile：

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="github-pages"></a>GitHub Pages

若要處理 URL 重寫，請新增 *404.html* 檔，以及處理將要求重新導向至 *index.html* 頁面的指令碼。 如需社群所提供的範例實作，請參閱 [GitHub Pages 的單一頁面應用程式](https://spa-github-pages.rafrex.com/) (GitHub 上的 [rafrex/spa-github-pages](https://github.com/rafrex/spa-github-pages#readme))。 使用社群方法的範例可在 [GitHub 上的 blazor-demo/blazor-demo.github.io](https://github.com/blazor-demo/blazor-demo.github.io) ([即時網站](https://blazor-demo.github.io/)) 看到。

使用專案網站，而非組織網站時，請在 *index.html* 中新增或更新 `<base>` 標籤。 將 `href` 屬性值設定為 GitHub 存放庫名稱，並在尾端加上斜線 (例如 `my-repository/`)。

## <a name="host-configuration-values"></a>主機組態值

使用[用戶端裝載模型](xref:blazor/hosting-models#client-side)的 Blazor 應用程式，可以在開發環境中，在執行時接受下列主機組態值作為命令列引數。

### <a name="content-root"></a>內容根目錄

`--contentroot` 引數設定包含應用程式內容檔案的目錄絕對路徑。 在下列範例中，`/content-root-path` 是應用程式的內容根路徑。

* 在命令提示字元，於本機執行應用程式時傳遞引數。 從應用程式目錄，執行：

  ```console
  dotnet run --contentroot=/content-root-path
  ```

* 新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。 此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* 在 Visual Studio 中，在 [屬性] > [偵錯] > [應用程式引數] 中指定引數。 在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a>路徑基底

引數`href`會設定應用程式以非根相對 URL 路徑在本機執行的應用程式基底路徑（ `<base>`此標籤會設定為預備和`/`生產以外的路徑）。 `--pathbase` 在下列範例中，`/relative-URL-path` 是應用程式的路徑基底。 如需詳細資訊，請參閱[應用程式基底路徑](xref:host-and-deploy/blazor/index#app-base-path)。

> [!IMPORTANT]
> 不同於提供給 `<base>` 標籤 `href` 的路徑，在傳遞 `--pathbase` 引數值時請勿包含尾端的斜線 (`/`)。 如果應用程式基底路徑提供於 `<base>` 標籤作為 `<base href="/CoolApp/">` (包括尾端的斜線)，請將命令列引數值傳遞為 `--pathbase=/CoolApp` (不含尾端的斜線)。

* 在命令提示字元，於本機執行應用程式時傳遞引數。 從應用程式目錄，執行：

  ```console
  dotnet run --pathbase=/relative-URL-path
  ```

* 新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。 此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。

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

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* 新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。 此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* 在 Visual Studio 中，在 [屬性] > [偵錯] > [應用程式引數] 中指定引數。 在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="configure-the-linker"></a>設定連結器

Blazor 在每個組建上執行中繼語言 (IL) 連結，以從輸出組件移除不必要的 IL。 組件連結可在組建上控制。 如需詳細資訊，請參閱 <xref:host-and-deploy/blazor/configure-linker>。
