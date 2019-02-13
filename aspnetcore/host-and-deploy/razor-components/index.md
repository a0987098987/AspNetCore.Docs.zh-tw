---
title: 裝載和部署 Razor 元件
author: guardrex
description: 了解如何使用 ASP.NET Core、內容傳遞網路 (CDN)、檔案伺服器和 GitHub Pages 裝載和部署 Razor 元件和 Blazor 應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: host-and-deploy/razor-components/index
ms.openlocfilehash: 9debd75128ceecb805fc673a8182a785fc9f7942
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667927"
---
# <a name="host-and-deploy-razor-components"></a>裝載和部署 Razor 元件

作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

## <a name="publish-the-app"></a>發行應用程式

應用程式在發行組態中使用 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令發佈以供部署。 IDE 可能會自動使用內建的發佈功能處理執行 `dotnet publish` 命令，因此可能不需要以手動方式從命令提示字元執行命令，視使用中的開發工具而定。

```console
dotnet publish -c Release
```

`dotnet publish` 會觸發專案相依性的[還原](/dotnet/core/tools/dotnet-restore)，並[建置](/dotnet/core/tools/dotnet-build)專案，然後才建立資產以供部署。 在建置程序中，會移除未使用的方法和組件，減少應用程式的下載大小和載入時間。 部署會建立在 */bin/Release/&lt;target-framework&gt;/publish* 資料夾中。

*publish* 資料夾中的資產會部署至網頁伺服器。 部署可能是手動或自動的程序，視使用中的開發工具而定。

## <a name="host-configuration-values"></a>主機組態值

使用[伺服器端裝載模型](xref:razor-components/hosting-models#server-side-hosting-model)的 Razor 元件應用程式，可以接受 [Web 主機組態值](xref:fundamentals/host/web-host#host-configuration-values)。

使用[用戶端裝載模型](xref:razor-components/hosting-models#client-side-hosting-model)的 Blazor 應用程式，可以在開發環境中，在執行時接受下列主機組態值作為命令列引數。

### <a name="content-root"></a>內容根目錄

`--contentroot` 引數設定包含應用程式內容檔案的目錄絕對路徑。

* 在命令提示字元，於本機執行應用程式時傳遞引數。 從應用程式目錄，執行：

  ```console
  dotnet run --contentroot=/<content-root>
  ```

* 新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。 在使用 Visual Studio 偵錯工具執行應用程式時，和從命令提示字元以 `dotnet run` 執行應用程式時，會取得這項設定。

  ```json
  "commandLineArgs": "--contentroot=/<content-root>"
  ```

* 在 Visual Studio 中，在 [屬性] > [偵錯] > [應用程式引數] 中指定引數。 在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。

  ```console
  --contentroot=/<content-root>
  ```

### <a name="path-base"></a>路徑基底

`--pathbase` 引數會以非根虛擬路徑，為本機執行的應用程式設定應用程式基底路徑 (`<base>` 標籤 `href` 會設為非 `/` 的路徑以供預備和生產之用)。 如需詳細資訊，請參閱[應用程式基底路徑](#app-base-path)一節。

> [!IMPORTANT]
> 不同於提供給 `<base>` 標籤 `href` 的路徑，在傳遞 `--pathbase` 引數值時請勿包含尾端的斜線 (`/`)。 如果應用程式基底路徑提供於 `<base>` 標籤作為 `<base href="/CoolApp/" />` (包括尾端的斜線)，請將命令列引數值傳遞為 `--pathbase=/CoolApp` (不含尾端的斜線)。

* 在命令提示字元，於本機執行應用程式時傳遞引數。 從應用程式目錄，執行：

  ```console
  dotnet run --pathbase=/<virtual-path>
  ```

* 新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。 在使用 Visual Studio 偵錯工具執行應用程式時，和從命令提示字元以 `dotnet run` 執行應用程式時，會取得這項設定。

  ```json
  "commandLineArgs": "--pathbase=/<virtual-path>"
  ```

* 在 Visual Studio 中，在 [屬性] > [偵錯] > [應用程式引數] 中指定引數。 在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。

  ```console
  --pathbase=/<virtual-path>
  ```

### <a name="urls"></a>URL

`--urls` 引數表示接聽要求的 IP 位址或主機位址，以及連接埠和通訊協定。

* 在命令提示字元，於本機執行應用程式時傳遞引數。 從應用程式目錄，執行：

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* 新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。 在使用 Visual Studio 偵錯工具執行應用程式時，和從命令提示字元以 `dotnet run` 執行應用程式時，會取得這項設定。

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* 在 Visual Studio 中，在 [屬性] > [偵錯] > [應用程式引數] 中指定引數。 在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deploy-a-client-side-blazor-app"></a>部署用戶端 Blazor 應用程式

使用[用戶端裝載模型](xref:razor-components/hosting-models#client-side-hosting-model)：

* Blazor 應用程式、其相依性，和 .NET 執行階段會下載至瀏覽器中。
* 應用程式會直接在瀏覽器 UI 執行緒上執行。 支援下列策略之一：
  * Blazor 應用程式由 ASP.NET Core 應用程式提供服務。 介紹於[使用 ASP.NET Core 的用戶端 Blazor 裝載部署](#client-side-blazor-hosted-deployment-with-aspnet-core)一節。
  * Blazor 應用程式放在靜態裝載的網頁伺服器或服務上，其中 .NET 不會用來提供服務給 Blazor 應用程式。 其於[用戶端 Blazor 獨立部署](#client-side-blazor-standalone-deployment)一節中介紹。
  
### <a name="configure-the-linker"></a>設定連結器

Blazor 在每個組建上執行中繼語言 (IL) 連結，以從輸出組件移除不必要的 IL。 您可以控制組建上的組件連結。 如需詳細資訊，請參閱<xref:host-and-deploy/razor-components/configure-linker>。

### <a name="rewrite-urls-for-correct-routing"></a>重寫 URL 以便正確地路由

用戶端應用程式中的頁面元件傳送要求，並不只是將要求傳送到伺服器端的裝載應用程式那麼簡單。 請考慮具有兩個頁面的用戶端應用程式：

* **_Main.cshtml_** &ndash; 在應用程式根目錄載入，並包含 [關於] 頁面的連結 (`href="About"`)。
* **_About.cshtml_** &ndash; [關於] 頁面。

使用瀏覽器的網址列要求應用程式預設文件時 (例如 `https://www.contoso.com/`)：

1. 瀏覽器提出要求。
1. 傳回預設頁面，這通常是 *index.html*。
1. *index.html* 啟動載入應用程式。
1. 會載入 Blazor 的路由器 ，且 Razor 主頁面 (*Main.cshtml*) 隨即出現。

在主頁面上，選取 [關於] 頁面的連結會載入 [關於] 頁面。 選取您用戶端上適用的 [關於] 頁面連結，因為 Blazor 路由器會停止瀏覽器，使其不從網際網路對 `www.contoso.com` 提出針對 `About` 的要求，並自行提供 [關於] 頁面。 對於「用戶端應用程式內」內部頁面的所有要求運作方式相同：要求不會對於網際網路上伺服器裝載的資源，觸發以瀏覽器為基礎的要求。 路由器會在內部處理要求。

如果使用瀏覽器之網址列提出對 `www.contoso.com/About` 的要求，則要求會失敗。 在應用程式的網際網路主機上沒有這類資源存在，因此會傳回「404 找不到」的回應。

因為瀏覽器會提出要求給以網際網路為基礎的主機以取得用戶端頁面，所以網頁伺服器和裝載服務必須針對實際上不在伺服器上資源，將所有要求重新撰寫至 *index.html* 頁面。 傳回 *index.html* 時，應用程式的用戶端路由器會接管，並以正確的資源回應。

### <a name="app-base-path"></a>應用程式基底路徑

應用程式基底路徑是伺服器上的虛擬應用程式根路徑。 例如，對於位於 Contoso 伺服器虛擬資料夾 `/CoolApp/` 的應用程式，能使用 `https://www.contoso.com/CoolApp` 到達它，且具有 `/CoolApp/` 虛擬基底路徑。 藉由將應用程式基底路徑設為 `CoolApp/`，應用程式會察覺它虛擬地位於伺服器上何處。 應用程式可以使用應用程式基底路徑，從不在根目錄中之元件建構相對於應用程式根目錄的 URL。 這可讓存在於不同目錄結構層級的元件，建置連結以連至應用程式內所有位置的其他資源。 應用程式基底路徑也會用來攔截超連結點擊，其中連結的 `href` 目標是在應用程式基底路徑 URI 空間內&mdash;Blazor 路由器會處理內部瀏覽。

在許多裝載案例中，應用程式之伺服器虛擬路徑是應用程式的根目錄。 在這些情況中，應用程式基底路徑是正斜線 (`<base href="/" />`)，這是應用程式的預設組態。 在其他裝載案例中，例如 GitHub Pages 和 IIS 虛擬目錄或子應用程式，應用程式基底路徑必須設定為應用程式的伺服器虛擬路徑。 若要設定應用程式的基底路徑，請在 *index.html* 中新增或更新 `<base>` 標籤，這可在 `<head>` 標籤項目內找到。 將 `href` 屬性值設為 `<virtual-path>/` (尾端的斜線為必要)，其中 `<virtual-path>/` 是應用程式伺服器上的完整虛擬應用程式根路徑。 在上述範例中，虛擬路徑設為 `CoolApp/`: `<base href="CoolApp/" />`。

對於已設定非根目錄虛擬路徑的應用程式 (例如 `<base href="CoolApp/" />`)，應用程式「在本機執行時」無法找到它的資源。 若要在本機開發和測試期間解決這個問題，您可以提供「基底路徑」引數，讓它在執行時符合 `<base>` 標籤的 `href` 值。

若要在本機執行應用程式時傳遞路徑基底引數與根路徑 (`/`)，請從應用程式的目錄執行下列命令：

```console
dotnet run --pathbase=/CoolApp
```

應用程式在 `http://localhost:port/CoolApp` 本機回應。

如需詳細資訊，請參閱[路徑基底主機組態值](#path-base)一節。

> [!IMPORTANT]
> 如果應用程式使用[用戶端裝載模型](xref:razor-components/hosting-models#client-side-hosting-model) (根據 **Blazor** 專案範本)，且裝載為 ASP.NET Core 應用程式中的 IIS 子應用程式，請務必停用繼承的 ASP.NET Core 模組處理常式，或是確定 *web.config* 檔案中，根 (父系) 應用程式的 `<handlers>` 區段不由子應用程式繼承。
>
> 移除應用程式已發佈 *web.config* 檔案中的處理常式，方法是新增 `<handlers>` 區段到檔案：
>
> ```xml
> <handlers>
>   <remove name="aspNetCore" />
> </handlers>
> ```
>
> 或者，使用 `<location>` 項目，並將 `inheritInChildApplications` 設為 `false`，以停用根 (父系) 應用程式的 `<system.webServer>` 區段繼承：
>
> ```xml
> <?xml version="1.0" encoding="utf-8"?>
> <configuration>
>  <location path="." inheritInChildApplications="false">
>    <system.webServer>
>      <handlers>
>        <add name="aspNetCore" ... />
>      </handlers>
>      <aspNetCore ... />
>    </system.webServer>
>  </location>
> </configuration>
> ```
>
> 除了如本節中所述設定應用程式的基底路徑，也會執行移除處理常式或停用繼承。 在應用程式的 *index.html* 檔案，將應用程式基底路徑設為在 IIS 中設定子應用程式時使用的 IIS 別名。

### <a name="client-side-blazor-hosted-deployment-with-aspnet-core"></a>使用 ASP.NET Core 的用戶端 Blazor 裝載部署

「裝載部署」會將用戶端 Blazor 應用程式，從伺服器上執行的 [ASP.NET Core 應用程式](xref:index)，提供給瀏覽器。

Blazor 應用程式隨附於發佈輸出中的 ASP.NET Core 應用程式，以便兩個應用程式會一起部署。 需要有能夠裝載 ASP.NET Core 應用程式的網頁伺服器。 對於裝載部署，Visual Studio 包含 **Blazor (已裝載 ASP.NET Core)** 專案範本 (使用 [dotnet new](/dotnet/core/tools/dotnet-new) 命令時的 `blazorhosted` 範本)。

如需 ASP.NET Core 應用程式裝載和部署的詳細資訊，請參閱 <xref:host-and-deploy/index>。

如需部署至 Azure App Service 的資訊，請參閱下列主題：

<xref:tutorials/publish-to-azure-webapp-using-vs>  
了解如何使用 Visual Studio 將 ASP.NET Core 應用程式發行到 Azure App Service。

### <a name="client-side-blazor-standalone-deployment"></a>用戶端 Blazor 獨立部署

「獨立部署」會以一組直接由用戶端要求的靜態檔案來提供服務給用戶端 Blazor 應用程式。 網頁伺服器不用來提供服務給 Blazor 應用程式。

#### <a name="client-side-blazor-standalone-hosting-with-iis"></a>使用 IIS 的用戶端 Blazor 獨立裝載

IIS 是足以支援 Blazor 應用程式的靜態檔案伺服器。 若要設定 IIS 來裝載 Blazor，請參閱[在 IIS 上建置靜態網站](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis)。

已發佈的資產建立於 \\bin\\Release\\&lt;目標 Framework&gt;\\publish 資料夾中。 在網頁伺服器或裝載服務上，裝載 *publish* 資料夾的內容。

**web.config**

發佈 Blazor 專案時，會建立 *web.config* 檔案，並使用下列 IIS 組態：

* 針對下列副檔名設定 MIME 類型：
  * *\*.dll*：`application/octet-stream`
  * *\*.json*：`application/json`
  * *\*.wasm*：`application/wasm`
  * *\*.woff*：`application/font-woff`
  * *\*.woff2*：`application/font-woff`
* 針對下列 MIME 類型會啟用 HTTP 壓縮：
  * `application/octet-stream`
  * `application/wasm`
* 建立 URL Rewrite Module 規則：
  * 提供應用程式靜態資產所在的子目錄 (&lt;組件名稱&gt;\\dist\\&lt;要求的路徑&gt;)。
  * 建立 SPA 後援路由，讓非檔案資產要求重新導向至其靜態資產資料夾中的應用程式預設文件 (&lt;組件名稱&gt;\\dist\\index.html)。

**安裝 URL Rewrite Module**

需要 [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite)，才可重寫 URL。 預設不會安裝模組，且無法用來安裝作為網頁伺服器 (IIS) 角色服務功能。 必須從 IIS 網站下載模組。 請使用 Web Platform Installer 安裝模組：

1. 在本機上，巡覽至 [URL Rewrite Module 下載頁面](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads)。 如需英文版，請選取 [WebPI] 下載 WebPI 安裝程式。 如需其他語言，請選取適當的伺服器架構 (x86 x64) 來下載安裝程式。
1. 將安裝程式複製到伺服器。 執行安裝程式。 選取 [安裝] 按鈕，並接受授權條款。 安裝完成之後，不需要重新啟動伺服器。

**設定網站**

將網站的 [實體路徑] 設為應用程式的資料夾。 此資料夾包含：

* IIS 用來設定網站的 *web.config* 檔，包括必要的重新導向規則和檔案內容類型。
* 應用程式的靜態資產資料夾。

**疑難排解**

如果收到「500 內部伺服器錯誤」，且 IIS 管理員在嘗試存取網站的組態時擲回錯誤，請確認是否已安裝 URL Rewrite Module。 未安裝此模組時，IIS 無法剖析 *web.config* 檔案。 這導致 IIS 管理員無法載入網站的組態，且網站無法提供 Blazor 的靜態檔案。

如需針對部署至 IIS 進行疑難排解的詳細資訊，請參閱 <xref:host-and-deploy/iis/troubleshoot>。

#### <a name="client-side-blazor-standalone-hosting-with-nginx"></a>使用 Nginx 的用戶端 Blazor 獨立裝載

下列 *nginx.conf* 檔案已簡化，示範如何將 Nginx 設定為每當找不到磁碟上的對應檔案時便傳送 *Index.html* 檔案。

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /Index.html =404;
        }
    }
}
```

如需生產環境 Nginx 網頁伺服器組態的詳細資訊，請參閱[建立 NGINX Plus 和 NGINX 組態檔](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/)。

#### <a name="client-side-blazor-standalone-hosting-with-nginx-in-docker"></a>在 Docker 中使用 Nginx 的用戶端 Blazor 獨立裝載

若要在 Docker 中使用 Nginx 裝載 Blazor，請設定 Dockerfile 以使用以 Alpine 為基礎的 Nginx 映像。 更新 Dockerfile，將 *nginx.config* 檔案複製到容器內。

如下列範例所示，新增一行至 Dockerfile：

```
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

#### <a name="client-side-blazor-standalone-hosting-with-github-pages"></a>使用 GitHub Pages 的用戶端 Blazor 獨立裝載

若要處理 URL 重寫，請新增 *404.html* 檔，以及處理將要求重新導向至 *index.html* 頁面的指令碼。 如需社群所提供的範例實作，請參閱 [GitHub Pages 的單一頁面應用程式](http://spa-github-pages.rafrex.com/) (GitHub 上的 [rafrex/spa-github-pages](https://github.com/rafrex/spa-github-pages#readme))。 使用社群方法的範例可在 [GitHub 上的 blazor-demo/blazor-demo.github.io](https://github.com/blazor-demo/blazor-demo.github.io) ([即時網站](https://blazor-demo.github.io/)) 看到。

使用專案網站，而非組織網站時，請在 *index.html* 中新增或更新 `<base>` 標籤。 將 `href` 屬性值設為 `<repository-name>/`，其中 `<repository-name>/` 是 GitHub 存放庫名稱。

## <a name="deploy-a-server-side-razor-components-app"></a>部署伺服器端 Razor 元件應用程式

使用[伺服器端裝載模型](xref:razor-components/hosting-models#server-side-hosting-model)，Razor 元件會從 ASP.NET Core 應用程式內在伺服器上執行。 UI 更新、事件處理及 JavaScript 呼叫會透過 SignalR 連線處理。

應用程式隨附於發佈輸出中的 ASP.NET Core 應用程式，以便兩個應用程式會一起部署。 需要有能夠裝載 ASP.NET Core 應用程式的網頁伺服器。 對於伺服器端部署，Visual Studio 包含 **Blazor (ASP.NET Core 中的伺服器端)** 專案範本 (使用 [dotnet new](/dotnet/core/tools/dotnet-new) 命令時的 `blazorserver` 範本)。

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

發佈 ASP.NET Core 應用程式時，Razor 元件應用程式包含在發佈輸出中，以便 ASP.NET Core 應用程式和 Razor 元件應用程式可以一起部署。 如需 ASP.NET Core 應用程式裝載和部署的詳細資訊，請參閱 <xref:host-and-deploy/index>。

如需部署至 Azure App Service 的資訊，請參閱下列主題：

<xref:tutorials/publish-to-azure-webapp-using-vs>  
了解如何使用 Visual Studio 將 ASP.NET Core 應用程式發行到 Azure App Service。
