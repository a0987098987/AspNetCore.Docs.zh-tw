---
title: 裝載和部署 ASP.NET CoreBlazor WebAssembly
author: guardrex
description: 瞭解如何 Blazor 使用 ASP.NET Core、內容傳遞網路（CDN）、檔案伺服器和 GitHub 頁面來裝載和部署應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/07/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/host-and-deploy/webassembly
ms.openlocfilehash: 47ba6f54c68158b3f6dcbbdda06ec8747cf88241
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85402542"
---
# <a name="host-and-deploy-aspnet-core-blazor-webassembly"></a><span data-ttu-id="c855b-103">裝載和部署 ASP.NET CoreBlazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="c855b-103">Host and deploy ASP.NET Core Blazor WebAssembly</span></span>

<span data-ttu-id="c855b-104">By [Luke Latham](https://github.com/guardrex)、 [Rainer Stropek](https://www.timecockpit.com)、 [Daniel Roth](https://github.com/danroth27)、 [Ben Adams](https://twitter.com/ben_a_adams)和[Safia Abdalla](https://safia.rocks)</span><span class="sxs-lookup"><span data-stu-id="c855b-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), [Daniel Roth](https://github.com/danroth27), [Ben Adams](https://twitter.com/ben_a_adams), and [Safia Abdalla](https://safia.rocks)</span></span>

<span data-ttu-id="c855b-105">使用[ Blazor WebAssembly 裝載模型](xref:blazor/hosting-models#blazor-webassembly)：</span><span class="sxs-lookup"><span data-stu-id="c855b-105">With the [Blazor WebAssembly hosting model](xref:blazor/hosting-models#blazor-webassembly):</span></span>

* <span data-ttu-id="c855b-106">Blazor應用程式、其相依性和 .net 執行時間會以平行方式下載至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="c855b-106">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser in parallel.</span></span>
* <span data-ttu-id="c855b-107">應用程式會直接在瀏覽器 UI 執行緒上執行。</span><span class="sxs-lookup"><span data-stu-id="c855b-107">The app is executed directly on the browser UI thread.</span></span>

<span data-ttu-id="c855b-108">支援下列部署策略：</span><span class="sxs-lookup"><span data-stu-id="c855b-108">The following deployment strategies are supported:</span></span>

* <span data-ttu-id="c855b-109">Blazor應用程式是由 ASP.NET Core 應用程式提供。</span><span class="sxs-lookup"><span data-stu-id="c855b-109">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="c855b-110">此策略已於[搭配 ASP.NET Core 的已裝載部署](#hosted-deployment-with-aspnet-core)一節中涵蓋。</span><span class="sxs-lookup"><span data-stu-id="c855b-110">This strategy is covered in the [Hosted deployment with ASP.NET Core](#hosted-deployment-with-aspnet-core) section.</span></span>
* <span data-ttu-id="c855b-111">Blazor應用程式會放在靜態裝載的 web 伺服器或服務上，其中 .net 不會用來服務 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c855b-111">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="c855b-112">此策略涵蓋于[獨立部署](#standalone-deployment)一節，其中包含將 Blazor WebAssembly 應用程式裝載為 IIS 子應用程式的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="c855b-112">This strategy is covered in the [Standalone deployment](#standalone-deployment) section, which includes information on hosting a Blazor WebAssembly app as an IIS sub-app.</span></span>

## <a name="compression"></a><span data-ttu-id="c855b-113">壓縮</span><span class="sxs-lookup"><span data-stu-id="c855b-113">Compression</span></span>

<span data-ttu-id="c855b-114">當 Blazor WebAssembly 應用程式發佈時，會在發佈期間以靜態方式壓縮輸出，以減少應用程式的大小，並移除執行時間壓縮的額外負荷。</span><span class="sxs-lookup"><span data-stu-id="c855b-114">When a Blazor WebAssembly app is published, the output is statically compressed during publish to reduce the app's size and remove the overhead for runtime compression.</span></span> <span data-ttu-id="c855b-115">使用下列壓縮演算法：</span><span class="sxs-lookup"><span data-stu-id="c855b-115">The following compression algorithms are used:</span></span>

* <span data-ttu-id="c855b-116">[Brotli](https://tools.ietf.org/html/rfc7932) （最高層級）</span><span class="sxs-lookup"><span data-stu-id="c855b-116">[Brotli](https://tools.ietf.org/html/rfc7932) (highest level)</span></span>
* [<span data-ttu-id="c855b-117">Gzip</span><span class="sxs-lookup"><span data-stu-id="c855b-117">Gzip</span></span>](https://tools.ietf.org/html/rfc1952)

Blazor<span data-ttu-id="c855b-118">依賴主機來提供適當的壓縮檔案。</span><span class="sxs-lookup"><span data-stu-id="c855b-118"> relies on the host to the serve the appropriate compressed files.</span></span> <span data-ttu-id="c855b-119">當使用 ASP.NET Core 裝載的專案時，主專案可以執行內容協調並提供靜態壓縮檔案。</span><span class="sxs-lookup"><span data-stu-id="c855b-119">When using an ASP.NET Core hosted project, the host project is capable of performing content negotiation and serving the statically-compressed files.</span></span> <span data-ttu-id="c855b-120">裝載 Blazor WebAssembly 獨立應用程式時，可能需要額外的工作，以確保提供靜態壓縮檔案：</span><span class="sxs-lookup"><span data-stu-id="c855b-120">When hosting a Blazor WebAssembly standalone app, additional work might be required to ensure that statically-compressed files are served:</span></span>

* <span data-ttu-id="c855b-121">如需 IIS `web.config` 壓縮設定，請參閱[Iis： Brotli 和 Gzip 壓縮](#brotli-and-gzip-compression)一節。</span><span class="sxs-lookup"><span data-stu-id="c855b-121">For IIS `web.config` compression configuration, see the [IIS: Brotli and Gzip compression](#brotli-and-gzip-compression) section.</span></span> 
* <span data-ttu-id="c855b-122">在不支援靜態壓縮檔案內容協商（例如 GitHub 頁面）的靜態裝載解決方案上裝載時，請考慮將應用程式設定為提取和解碼 Brotli 壓縮檔案：</span><span class="sxs-lookup"><span data-stu-id="c855b-122">When hosting on static hosting solutions that don't support statically-compressed file content negotiation, such as GitHub Pages, consider configuring the app to fetch and decode Brotli compressed files:</span></span>

  * <span data-ttu-id="c855b-123">從應用程式中的[google/Brotli GitHub 存放庫](https://github.com/google/brotli/)參考 Brotli 解碼器。</span><span class="sxs-lookup"><span data-stu-id="c855b-123">Reference the Brotli decoder from the [google/brotli GitHub repository](https://github.com/google/brotli/) in the app.</span></span>
  * <span data-ttu-id="c855b-124">更新應用程式以使用此解碼器。</span><span class="sxs-lookup"><span data-stu-id="c855b-124">Update the app to use the decoder.</span></span> <span data-ttu-id="c855b-125">將中結束記號內的標記變更 `<body>` `wwwroot/index.html` 為下列內容：</span><span class="sxs-lookup"><span data-stu-id="c855b-125">Change the markup inside the the closing `<body>` tag in `wwwroot/index.html` to the following:</span></span>
  
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
   
<span data-ttu-id="c855b-126">若要停用壓縮，請將 `BlazorEnableCompression` MSBuild 屬性新增至應用程式的專案檔，並將值設定為 `false` ：</span><span class="sxs-lookup"><span data-stu-id="c855b-126">To disable compression, add the `BlazorEnableCompression` MSBuild property to the app's project file and set the value to `false`:</span></span>

```xml
<PropertyGroup>
  <BlazorEnableCompression>false</BlazorEnableCompression>
</PropertyGroup>
```

## <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="c855b-127">重寫 URL 以便正確地路由</span><span class="sxs-lookup"><span data-stu-id="c855b-127">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="c855b-128">應用程式中頁面元件的路由要求 Blazor WebAssembly Blazor Server ，與託管應用程式中的路由要求並不簡單。</span><span class="sxs-lookup"><span data-stu-id="c855b-128">Routing requests for page components in a Blazor WebAssembly app isn't as straightforward as routing requests in a Blazor Server, hosted app.</span></span> <span data-ttu-id="c855b-129">假設有 Blazor WebAssembly 兩個元件的應用程式：</span><span class="sxs-lookup"><span data-stu-id="c855b-129">Consider a Blazor WebAssembly app with two components:</span></span>

* <span data-ttu-id="c855b-130">`Main.razor`：在應用程式的根目錄載入，並包含 `About` 元件（）的連結 `href="About"` 。</span><span class="sxs-lookup"><span data-stu-id="c855b-130">`Main.razor`: Loads at the root of the app and contains a link to the `About` component (`href="About"`).</span></span>
* <span data-ttu-id="c855b-131">`About.razor`： `About` 元件。</span><span class="sxs-lookup"><span data-stu-id="c855b-131">`About.razor`: `About` component.</span></span>

<span data-ttu-id="c855b-132">使用瀏覽器的網址列要求應用程式預設文件時 (例如 `https://www.contoso.com/`)：</span><span class="sxs-lookup"><span data-stu-id="c855b-132">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="c855b-133">瀏覽器提出要求。</span><span class="sxs-lookup"><span data-stu-id="c855b-133">The browser makes a request.</span></span>
1. <span data-ttu-id="c855b-134">預設頁面會傳回，這通常是 `index.html` 。</span><span class="sxs-lookup"><span data-stu-id="c855b-134">The default page is returned, which is usually `index.html`.</span></span>
1. <span data-ttu-id="c855b-135">`index.html`啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="c855b-135">`index.html` bootstraps the app.</span></span>
1. Blazor<span data-ttu-id="c855b-136">的路由器會載入，並 Razor `Main` 呈現元件。</span><span class="sxs-lookup"><span data-stu-id="c855b-136">'s router loads, and the Razor `Main` component is rendered.</span></span>

<span data-ttu-id="c855b-137">在主頁面中，選取元件的連結可 `About` 在用戶端上運作，因為 Blazor 路由器會阻止瀏覽器針對在網際網路上提出要求， `www.contoso.com` `About` 並提供所呈現 `About` 元件本身。</span><span class="sxs-lookup"><span data-stu-id="c855b-137">In the Main page, selecting the link to the `About` component works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the rendered `About` component itself.</span></span> <span data-ttu-id="c855b-138">\* Blazor WebAssembly 應用程式內\*內部端點的所有要求都是以相同方式處理：要求不會對網際網路上伺服器裝載的資源觸發瀏覽器型要求。</span><span class="sxs-lookup"><span data-stu-id="c855b-138">All of the requests for internal endpoints *within the Blazor WebAssembly app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="c855b-139">路由器會在內部處理要求。</span><span class="sxs-lookup"><span data-stu-id="c855b-139">The router handles the requests internally.</span></span>

<span data-ttu-id="c855b-140">如果使用瀏覽器之網址列提出對 `www.contoso.com/About` 的要求，則要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="c855b-140">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="c855b-141">在應用程式的網際網路主機上沒有這類資源存在，因此會傳回「404 - 找不到」\*\* 的回應。</span><span class="sxs-lookup"><span data-stu-id="c855b-141">No such resource exists on the app's Internet host, so a *404 - Not Found* response is returned.</span></span>

<span data-ttu-id="c855b-142">由於瀏覽器會對以網際網路為基礎的主機要求用戶端頁面，因此 web 伺服器和主機服務必須針對不在伺服器上的資源，將所有要求重寫至 `index.html` 頁面。</span><span class="sxs-lookup"><span data-stu-id="c855b-142">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the `index.html` page.</span></span> <span data-ttu-id="c855b-143">`index.html`傳回時，應用程式的 Blazor 路由器會接管並以正確的資源回應。</span><span class="sxs-lookup"><span data-stu-id="c855b-143">When `index.html` is returned, the app's Blazor router takes over and responds with the correct resource.</span></span>

<span data-ttu-id="c855b-144">部署到 IIS 伺服器時，您可以使用 URL 重寫模組搭配應用程式的已發佈檔案 `web.config` 。</span><span class="sxs-lookup"><span data-stu-id="c855b-144">When deploying to an IIS server, you can use the URL Rewrite Module with the app's published `web.config` file.</span></span> <span data-ttu-id="c855b-145">如需詳細資訊，請參閱[IIS](#iis)一節。</span><span class="sxs-lookup"><span data-stu-id="c855b-145">For more information, see the [IIS](#iis) section.</span></span>

## <a name="hosted-deployment-with-aspnet-core"></a><span data-ttu-id="c855b-146">搭配 ASP.NET Core 的已裝載部署</span><span class="sxs-lookup"><span data-stu-id="c855b-146">Hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="c855b-147">*託管部署* Blazor WebAssembly 可從 web 伺服器上執行的[ASP.NET Core 應用程式](xref:index)，將應用程式提供給瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="c855b-147">A *hosted deployment* serves the Blazor WebAssembly app to browsers from an [ASP.NET Core app](xref:index) that runs on a web server.</span></span>

<span data-ttu-id="c855b-148">用戶端 Blazor WebAssembly 應用程式會發佈到 `/bin/Release/{TARGET FRAMEWORK}/publish/wwwroot` 伺服器應用程式的資料夾，以及伺服器應用程式的任何其他靜態 web 資產。</span><span class="sxs-lookup"><span data-stu-id="c855b-148">The client Blazor WebAssembly app is published into the `/bin/Release/{TARGET FRAMEWORK}/publish/wwwroot` folder of the server app, along with any other static web assets of the server app.</span></span> <span data-ttu-id="c855b-149">這兩個應用程式會一起部署。</span><span class="sxs-lookup"><span data-stu-id="c855b-149">The two apps are deployed together.</span></span> <span data-ttu-id="c855b-150">需要有能夠裝載 ASP.NET Core 應用程式的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="c855b-150">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="c855b-151">針對裝載的部署，Visual Studio 包含\*\* Blazor WebAssembly 應用程式\*\*專案範本（ `blazorwasm` 使用命令時的範本 [`dotnet new`](/dotnet/core/tools/dotnet-new) ）並 **`Hosted`** 選取選項（ `-ho|--hosted` 使用命令時 `dotnet new` ）。</span><span class="sxs-lookup"><span data-stu-id="c855b-151">For a hosted deployment, Visual Studio includes the **Blazor WebAssembly App** project template (`blazorwasm` template when using the [`dotnet new`](/dotnet/core/tools/dotnet-new) command) with the **`Hosted`** option selected (`-ho|--hosted` when using the `dotnet new` command).</span></span>

<span data-ttu-id="c855b-152">如需 ASP.NET Core 應用程式裝載和部署的詳細資訊，請參閱 <xref:host-and-deploy/index>。</span><span class="sxs-lookup"><span data-stu-id="c855b-152">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="c855b-153">如需部署至 Azure App Service 的相關資訊，請參閱 <xref:tutorials/publish-to-azure-webapp-using-vs>。</span><span class="sxs-lookup"><span data-stu-id="c855b-153">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>

## <a name="standalone-deployment"></a><span data-ttu-id="c855b-154">獨立部署</span><span class="sxs-lookup"><span data-stu-id="c855b-154">Standalone deployment</span></span>

<span data-ttu-id="c855b-155">*獨立部署* Blazor WebAssembly 會將應用程式當做一組直接由用戶端要求的靜態檔案來提供。</span><span class="sxs-lookup"><span data-stu-id="c855b-155">A *standalone deployment* serves the Blazor WebAssembly app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="c855b-156">任何靜態檔案伺服器都可以服務 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c855b-156">Any static file server is able to serve the Blazor app.</span></span>

<span data-ttu-id="c855b-157">獨立部署資產會發佈到 `/bin/Release/{TARGET FRAMEWORK}/publish/wwwroot` 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="c855b-157">Standalone deployment assets are published into the `/bin/Release/{TARGET FRAMEWORK}/publish/wwwroot` folder.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="c855b-158">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c855b-158">Azure App Service</span></span>

Blazor WebAssembly<span data-ttu-id="c855b-159">應用程式可以部署到 Windows 上的 Azure App 服務，在[IIS](#iis)上裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="c855b-159"> apps can be deployed to Azure App Services on Windows, which hosts the app on [IIS](#iis).</span></span>

<span data-ttu-id="c855b-160">Blazor WebAssembly目前不支援將獨立應用程式部署至適用于 Linux 的 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="c855b-160">Deploying a standalone Blazor WebAssembly app to Azure App Service for Linux isn't currently supported.</span></span> <span data-ttu-id="c855b-161">目前無法使用可裝載應用程式的 Linux 伺服器映射。</span><span class="sxs-lookup"><span data-stu-id="c855b-161">A Linux server image to host the app isn't available at this time.</span></span> <span data-ttu-id="c855b-162">正在進行工作，以啟用此案例。</span><span class="sxs-lookup"><span data-stu-id="c855b-162">Work is in progress to enable this scenario.</span></span>

### <a name="iis"></a><span data-ttu-id="c855b-163">IIS</span><span class="sxs-lookup"><span data-stu-id="c855b-163">IIS</span></span>

<span data-ttu-id="c855b-164">IIS 是適用于應用程式的靜態檔案伺服器 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="c855b-164">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="c855b-165">若要設定 IIS 來裝載 Blazor ，請參閱[在 Iis 上建立靜態網站](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="c855b-165">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="c855b-166">已發行的資產會建立在 `/bin/Release/{TARGET FRAMEWORK}/publish` 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="c855b-166">Published assets are created in the `/bin/Release/{TARGET FRAMEWORK}/publish` folder.</span></span> <span data-ttu-id="c855b-167">裝載 `publish` 網頁伺服器或主控服務上的資料夾內容。</span><span class="sxs-lookup"><span data-stu-id="c855b-167">Host the contents of the `publish` folder on the web server or hosting service.</span></span>

#### <a name="webconfig"></a><span data-ttu-id="c855b-168">web.config</span><span class="sxs-lookup"><span data-stu-id="c855b-168">web.config</span></span>

<span data-ttu-id="c855b-169">Blazor發行專案時， `web.config` 會使用下列 IIS 設定來建立檔案：</span><span class="sxs-lookup"><span data-stu-id="c855b-169">When a Blazor project is published, a `web.config` file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="c855b-170">針對下列副檔名設定 MIME 類型：</span><span class="sxs-lookup"><span data-stu-id="c855b-170">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="c855b-171">`.dll`: `application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="c855b-171">`.dll`: `application/octet-stream`</span></span>
  * <span data-ttu-id="c855b-172">`.json`: `application/json`</span><span class="sxs-lookup"><span data-stu-id="c855b-172">`.json`: `application/json`</span></span>
  * <span data-ttu-id="c855b-173">`.wasm`: `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="c855b-173">`.wasm`: `application/wasm`</span></span>
  * <span data-ttu-id="c855b-174">`.woff`: `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="c855b-174">`.woff`: `application/font-woff`</span></span>
  * <span data-ttu-id="c855b-175">`.woff2`: `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="c855b-175">`.woff2`: `application/font-woff`</span></span>
* <span data-ttu-id="c855b-176">針對下列 MIME 類型會啟用 HTTP 壓縮：</span><span class="sxs-lookup"><span data-stu-id="c855b-176">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="c855b-177">建立 URL Rewrite Module 規則：</span><span class="sxs-lookup"><span data-stu-id="c855b-177">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="c855b-178">為應用程式的靜態資產所在的子目錄提供服務（ `wwwroot/{PATH REQUESTED}` ）。</span><span class="sxs-lookup"><span data-stu-id="c855b-178">Serve the sub-directory where the app's static assets reside (`wwwroot/{PATH REQUESTED}`).</span></span>
  * <span data-ttu-id="c855b-179">建立 SPA fallback 路由，讓非檔案資產的要求會重新導向至其靜態資產資料夾中的應用程式預設檔（ `wwwroot/index.html` ）。</span><span class="sxs-lookup"><span data-stu-id="c855b-179">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (`wwwroot/index.html`).</span></span>
  
#### <a name="use-a-custom-webconfig"></a><span data-ttu-id="c855b-180">使用自訂 web.config</span><span class="sxs-lookup"><span data-stu-id="c855b-180">Use a custom web.config</span></span>

<span data-ttu-id="c855b-181">若要使用自訂檔案 `web.config` ，請將自訂檔案放 `web.config` 在專案資料夾的根目錄，併發行專案。</span><span class="sxs-lookup"><span data-stu-id="c855b-181">To use a custom `web.config` file, place the custom `web.config` file at the root of the project folder and publish the project.</span></span>

#### <a name="install-the-url-rewrite-module"></a><span data-ttu-id="c855b-182">安裝 URL Rewrite 模組</span><span class="sxs-lookup"><span data-stu-id="c855b-182">Install the URL Rewrite Module</span></span>

<span data-ttu-id="c855b-183">需要 [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite)，才可重寫 URL。</span><span class="sxs-lookup"><span data-stu-id="c855b-183">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="c855b-184">預設不會安裝此模組，且其無法用來安裝為網頁伺服器 (IIS) 角色服務功能。</span><span class="sxs-lookup"><span data-stu-id="c855b-184">The module isn't installed by default, and it isn't available for install as a Web Server (IIS) role service feature.</span></span> <span data-ttu-id="c855b-185">必須從 IIS 網站下載模組。</span><span class="sxs-lookup"><span data-stu-id="c855b-185">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="c855b-186">請使用 Web Platform Installer 安裝模組：</span><span class="sxs-lookup"><span data-stu-id="c855b-186">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="c855b-187">在本機上，巡覽至 [URL Rewrite Module 下載頁面](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads)。</span><span class="sxs-lookup"><span data-stu-id="c855b-187">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="c855b-188">如需英文版，請選取 [WebPI]\*\*\*\* 下載 WebPI 安裝程式。</span><span class="sxs-lookup"><span data-stu-id="c855b-188">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="c855b-189">如需其他語言，請選取適當的伺服器架構 (x86 x64) 來下載安裝程式。</span><span class="sxs-lookup"><span data-stu-id="c855b-189">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="c855b-190">將安裝程式複製到伺服器。</span><span class="sxs-lookup"><span data-stu-id="c855b-190">Copy the installer to the server.</span></span> <span data-ttu-id="c855b-191">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="c855b-191">Run the installer.</span></span> <span data-ttu-id="c855b-192">選取 [安裝]\*\*\*\* 按鈕，並接受授權條款。</span><span class="sxs-lookup"><span data-stu-id="c855b-192">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="c855b-193">安裝完成之後，不需要重新啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="c855b-193">A server restart isn't required after the install completes.</span></span>

#### <a name="configure-the-website"></a><span data-ttu-id="c855b-194">設定網站</span><span class="sxs-lookup"><span data-stu-id="c855b-194">Configure the website</span></span>

<span data-ttu-id="c855b-195">將網站的 [實體路徑]\*\*\*\* 設為應用程式的資料夾。</span><span class="sxs-lookup"><span data-stu-id="c855b-195">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="c855b-196">此資料夾包含：</span><span class="sxs-lookup"><span data-stu-id="c855b-196">The folder contains:</span></span>

* <span data-ttu-id="c855b-197">`web.config`IIS 用來設定網站的檔案，包括必要的重新導向規則和檔案內容類型。</span><span class="sxs-lookup"><span data-stu-id="c855b-197">The `web.config` file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="c855b-198">應用程式的靜態資產資料夾。</span><span class="sxs-lookup"><span data-stu-id="c855b-198">The app's static asset folder.</span></span>

#### <a name="host-as-an-iis-sub-app"></a><span data-ttu-id="c855b-199">以 IIS 子應用程式的形式裝載</span><span class="sxs-lookup"><span data-stu-id="c855b-199">Host as an IIS sub-app</span></span>

<span data-ttu-id="c855b-200">如果獨立應用程式裝載為 IIS 子應用程式，請執行下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="c855b-200">If a standalone app is hosted as an IIS sub-app, perform either of the following:</span></span>

* <span data-ttu-id="c855b-201">停用繼承的 ASP.NET Core 模組處理常式。</span><span class="sxs-lookup"><span data-stu-id="c855b-201">Disable the inherited ASP.NET Core Module handler.</span></span>

  <span data-ttu-id="c855b-202">Blazor `web.config` 將區段新增至檔案，以移除應用程式已發佈檔案中的處理常式 `<handlers>` ：</span><span class="sxs-lookup"><span data-stu-id="c855b-202">Remove the handler in the Blazor app's published `web.config` file by adding a `<handlers>` section to the file:</span></span>

  ```xml
  <handlers>
    <remove name="aspNetCore" />
  </handlers>
  ```

* <span data-ttu-id="c855b-203">`<system.webServer>`使用設定為的元素，停用根（父系）應用程式區段的繼承 `<location>` `inheritInChildApplications` `false` ：</span><span class="sxs-lookup"><span data-stu-id="c855b-203">Disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>

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

<span data-ttu-id="c855b-204">除了設定[應用程式的基底路徑](xref:blazor/host-and-deploy/index#app-base-path)之外，也會執行移除處理常式或停用繼承。</span><span class="sxs-lookup"><span data-stu-id="c855b-204">Removing the handler or disabling inheritance is performed in addition to [configuring the app's base path](xref:blazor/host-and-deploy/index#app-base-path).</span></span> <span data-ttu-id="c855b-205">將應用程式檔案中的應用程式基底路徑，設定 `index.html` 為在 iis 中設定子應用程式時所使用的 iis 別名。</span><span class="sxs-lookup"><span data-stu-id="c855b-205">Set the app base path in the app's `index.html` file to the IIS alias used when configuring the sub-app in IIS.</span></span>

#### <a name="brotli-and-gzip-compression"></a><span data-ttu-id="c855b-206">Brotli 和 Gzip 壓縮</span><span class="sxs-lookup"><span data-stu-id="c855b-206">Brotli and Gzip compression</span></span>

<span data-ttu-id="c855b-207">您可以透過設定 IIS `web.config` 來提供 Brotli 或 Gzip 壓縮 Blazor 的資產。</span><span class="sxs-lookup"><span data-stu-id="c855b-207">IIS can be configured via `web.config` to serve Brotli or Gzip compressed Blazor assets.</span></span> <span data-ttu-id="c855b-208">如需範例設定，請參閱 [`web.config`](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/blazor/host-and-deploy/webassembly/_samples/web.config?raw=true) 。</span><span class="sxs-lookup"><span data-stu-id="c855b-208">For an example configuration, see [`web.config`](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/blazor/host-and-deploy/webassembly/_samples/web.config?raw=true).</span></span>

#### <a name="troubleshooting"></a><span data-ttu-id="c855b-209">疑難排解</span><span class="sxs-lookup"><span data-stu-id="c855b-209">Troubleshooting</span></span>

<span data-ttu-id="c855b-210">如果收到「500 - 內部伺服器錯誤」\*\*，且 IIS 管理員在嘗試存取網站設定時擲回錯誤，請確認是否已安裝 URL Rewrite 模組。</span><span class="sxs-lookup"><span data-stu-id="c855b-210">If a *500 - Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="c855b-211">未安裝模組時，IIS 無法剖析該檔案 `web.config` 。</span><span class="sxs-lookup"><span data-stu-id="c855b-211">When the module isn't installed, the `web.config` file can't be parsed by IIS.</span></span> <span data-ttu-id="c855b-212">這可防止 IIS 管理員從服務的靜態檔案載入網站的設定和網站 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="c855b-212">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="c855b-213">如需針對部署至 IIS 進行疑難排解的詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="c855b-213">For more information on troubleshooting deployments to IIS, see <xref:test/troubleshoot-azure-iis>.</span></span>

### <a name="azure-storage"></a><span data-ttu-id="c855b-214">Azure 儲存體</span><span class="sxs-lookup"><span data-stu-id="c855b-214">Azure Storage</span></span>

<span data-ttu-id="c855b-215">[Azure 儲存體](/azure/storage/)靜態檔案裝載允許無伺服器 Blazor 應用程式裝載。</span><span class="sxs-lookup"><span data-stu-id="c855b-215">[Azure Storage](/azure/storage/) static file hosting allows serverless Blazor app hosting.</span></span> <span data-ttu-id="c855b-216">支援自訂網域名稱、Azure 內容傳遞網路 (CDN) 及 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="c855b-216">Custom domain names, the Azure Content Delivery Network (CDN), and HTTPS are supported.</span></span>

<span data-ttu-id="c855b-217">當 Blob 服務針對儲存體帳戶上的靜態網站裝載啟用時：</span><span class="sxs-lookup"><span data-stu-id="c855b-217">When the blob service is enabled for static website hosting on a storage account:</span></span>

* <span data-ttu-id="c855b-218">將 [索引文件名稱]\*\*\*\* 設定為 `index.html`。</span><span class="sxs-lookup"><span data-stu-id="c855b-218">Set the **Index document name** to `index.html`.</span></span>
* <span data-ttu-id="c855b-219">將 [錯誤文件路徑]\*\*\*\* 設定為 `index.html`。</span><span class="sxs-lookup"><span data-stu-id="c855b-219">Set the **Error document path** to `index.html`.</span></span> Razor<span data-ttu-id="c855b-220">元件和其他非檔案端點不會位於 blob 服務所儲存之靜態內容中的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="c855b-220"> components and other non-file endpoints don't reside at physical paths in the static content stored by the blob service.</span></span> <span data-ttu-id="c855b-221">收到路由器應處理的其中一個資源的要求時 Blazor ，由 blob 服務產生的*404-找不*到的錯誤會將要求路由傳送至**錯誤檔路徑**。</span><span class="sxs-lookup"><span data-stu-id="c855b-221">When a request for one of these resources is received that the Blazor router should handle, the *404 - Not Found* error generated by the blob service routes the request to the **Error document path**.</span></span> <span data-ttu-id="c855b-222">`index.html`會傳回 blob，且路由器會 Blazor 載入並處理路徑。</span><span class="sxs-lookup"><span data-stu-id="c855b-222">The `index.html` blob is returned, and the Blazor router loads and processes the path.</span></span>

<span data-ttu-id="c855b-223">如果在執行時間因為檔案標頭中有不適當的 MIME 類型而未載入檔案 `Content-Type` ，請採取下列其中一個動作：</span><span class="sxs-lookup"><span data-stu-id="c855b-223">If files aren't loaded at runtime due to inappropriate MIME types in the files' `Content-Type` headers, take either of the following actions:</span></span>

* <span data-ttu-id="c855b-224">設定您的工具，以在部署檔案時設定正確的 MIME 類型（ `Content-Type` 標頭）。</span><span class="sxs-lookup"><span data-stu-id="c855b-224">Configure your tooling to set the correct MIME types (`Content-Type` headers) when the files are deployed.</span></span>
* <span data-ttu-id="c855b-225">在部署應用程式之後，變更檔案的 MIME 類型（ `Content-Type` 標頭）。</span><span class="sxs-lookup"><span data-stu-id="c855b-225">Change the MIME types (`Content-Type` headers) for the files after the app is deployed.</span></span>

  <span data-ttu-id="c855b-226">在每個檔案的儲存體總管（Azure 入口網站）中：</span><span class="sxs-lookup"><span data-stu-id="c855b-226">In Storage Explorer (Azure portal) for each file:</span></span>
  
  1. <span data-ttu-id="c855b-227">以滑鼠右鍵按一下檔案，然後選取 [**屬性**]。</span><span class="sxs-lookup"><span data-stu-id="c855b-227">Right-click the file and select **Properties**.</span></span>
  1. <span data-ttu-id="c855b-228">設定**ContentType** ，然後選取 [**儲存**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="c855b-228">Set the **ContentType** and select the **Save** button.</span></span>

<span data-ttu-id="c855b-229">如需詳細資訊，請參閱 [Azure 儲存體中的靜態網站裝載](/azure/storage/blobs/storage-blob-static-website)。</span><span class="sxs-lookup"><span data-stu-id="c855b-229">For more information, see [Static website hosting in Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span></span>

### <a name="nginx"></a><span data-ttu-id="c855b-230">Nginx</span><span class="sxs-lookup"><span data-stu-id="c855b-230">Nginx</span></span>

<span data-ttu-id="c855b-231">下列檔案 `nginx.conf` 已簡化，示範如何設定 Nginx，以便在 `index.html` 每次找不到磁片上的對應檔案時傳送檔案。</span><span class="sxs-lookup"><span data-stu-id="c855b-231">The following `nginx.conf` file is simplified to show how to configure Nginx to send the `index.html` file whenever it can't find a corresponding file on disk.</span></span>

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

<span data-ttu-id="c855b-232">如需生產環境 Nginx 網頁伺服器組態的詳細資訊，請參閱[建立 NGINX Plus 和 NGINX 組態檔](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/)。</span><span class="sxs-lookup"><span data-stu-id="c855b-232">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

### <a name="nginx-in-docker"></a><span data-ttu-id="c855b-233">Docker 中的 Nginx</span><span class="sxs-lookup"><span data-stu-id="c855b-233">Nginx in Docker</span></span>

<span data-ttu-id="c855b-234">若要 Blazor 使用 Nginx 在 Docker 中裝載，請將 Dockerfile 設定為使用以 Alpine 為基礎的 Nginx 映射。</span><span class="sxs-lookup"><span data-stu-id="c855b-234">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="c855b-235">更新 Dockerfile 以將檔案複製 `nginx.config` 到容器中。</span><span class="sxs-lookup"><span data-stu-id="c855b-235">Update the Dockerfile to copy the `nginx.config` file into the container.</span></span>

<span data-ttu-id="c855b-236">如下列範例所示，新增一行至 Dockerfile：</span><span class="sxs-lookup"><span data-stu-id="c855b-236">Add one line to the Dockerfile, as shown in the following example:</span></span>

```dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="apache"></a><span data-ttu-id="c855b-237">Apache</span><span class="sxs-lookup"><span data-stu-id="c855b-237">Apache</span></span>

<span data-ttu-id="c855b-238">若要將 Blazor WebAssembly 應用程式部署至 CentOS 7 或更新版本：</span><span class="sxs-lookup"><span data-stu-id="c855b-238">To deploy a Blazor WebAssembly app to CentOS 7 or later:</span></span>

1. <span data-ttu-id="c855b-239">建立 Apache 設定檔。</span><span class="sxs-lookup"><span data-stu-id="c855b-239">Create the Apache configuration file.</span></span> <span data-ttu-id="c855b-240">下列範例是簡化的設定檔案（ `blazorapp.config` ）：</span><span class="sxs-lookup"><span data-stu-id="c855b-240">The following example is a simplified configuration file (`blazorapp.config`):</span></span>

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

1. <span data-ttu-id="c855b-241">將 Apache 設定檔案放在 `/etc/httpd/conf.d/` 目錄中，這是 CentOS 7 中的預設 Apache 設定目錄。</span><span class="sxs-lookup"><span data-stu-id="c855b-241">Place the Apache configuration file into the `/etc/httpd/conf.d/` directory, which is the default Apache configuration directory in CentOS 7.</span></span>

1. <span data-ttu-id="c855b-242">將應用程式的檔案放入 `/var/www/blazorapp` 目錄中（在 `DocumentRoot` 設定檔中指定的位置）。</span><span class="sxs-lookup"><span data-stu-id="c855b-242">Place the app's files into the `/var/www/blazorapp` directory (the location specified to `DocumentRoot` in the configuration file).</span></span>

1. <span data-ttu-id="c855b-243">重新開機 Apache 服務。</span><span class="sxs-lookup"><span data-stu-id="c855b-243">Restart the Apache service.</span></span>

<span data-ttu-id="c855b-244">如需詳細資訊，請參閱 [`mod_mime`](https://httpd.apache.org/docs/2.4/mod/mod_mime.html) 和 [`mod_deflate`](https://httpd.apache.org/docs/current/mod/mod_deflate.html) 。</span><span class="sxs-lookup"><span data-stu-id="c855b-244">For more information, see [`mod_mime`](https://httpd.apache.org/docs/2.4/mod/mod_mime.html) and [`mod_deflate`](https://httpd.apache.org/docs/current/mod/mod_deflate.html).</span></span>

### <a name="github-pages"></a><span data-ttu-id="c855b-245">GitHub Pages</span><span class="sxs-lookup"><span data-stu-id="c855b-245">GitHub Pages</span></span>

<span data-ttu-id="c855b-246">若要處理 URL 重寫，請新增具有腳本的檔案，以處理將要求重新導向 `404.html` 至 `index.html` 頁面。</span><span class="sxs-lookup"><span data-stu-id="c855b-246">To handle URL rewrites, add a `404.html` file with a script that handles redirecting the request to the `index.html` page.</span></span> <span data-ttu-id="c855b-247">如需社群所提供的範例實作，請參閱 [GitHub Pages 的單一頁面應用程式](https://spa-github-pages.rafrex.com/) (GitHub 上的 [rafrex/spa-github-pages](https://github.com/rafrex/spa-github-pages#readme))。</span><span class="sxs-lookup"><span data-stu-id="c855b-247">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](https://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="c855b-248">使用社群方法的範例可在 [GitHub 上的 blazor-demo/blazor-demo.github.io](https://github.com/blazor-demo/blazor-demo.github.io) ([即時網站](https://blazor-demo.github.io/)) 看到。</span><span class="sxs-lookup"><span data-stu-id="c855b-248">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="c855b-249">當您使用專案網站而非組織網站時，請在中新增或更新 `<base>` 標記 `index.html` 。</span><span class="sxs-lookup"><span data-stu-id="c855b-249">When using a project site instead of an organization site, add or update the `<base>` tag in `index.html`.</span></span> <span data-ttu-id="c855b-250">將 `href` 屬性值設定為 GitHub 存放庫名稱，並在尾端加上斜線 (例如 `my-repository/`)。</span><span class="sxs-lookup"><span data-stu-id="c855b-250">Set the `href` attribute value to the GitHub repository name with a trailing slash (for example, `my-repository/`.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="c855b-251">主機組態值</span><span class="sxs-lookup"><span data-stu-id="c855b-251">Host configuration values</span></span>

<span data-ttu-id="c855b-252">[ Blazor WebAssembly 應用程式](xref:blazor/hosting-models#blazor-webassembly)可以在開發環境中的執行時間，接受下列主機設定值做為命令列引數。</span><span class="sxs-lookup"><span data-stu-id="c855b-252">[Blazor WebAssembly apps](xref:blazor/hosting-models#blazor-webassembly) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="c855b-253">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="c855b-253">Content root</span></span>

<span data-ttu-id="c855b-254">`--contentroot`引數會設定包含應用程式內容檔案（[內容根](xref:fundamentals/index#content-root)）之目錄的絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="c855b-254">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files ([content root](xref:fundamentals/index#content-root)).</span></span> <span data-ttu-id="c855b-255">在下列範例中，`/content-root-path` 是應用程式的內容根路徑。</span><span class="sxs-lookup"><span data-stu-id="c855b-255">In the following examples, `/content-root-path` is the app's content root path.</span></span>

* <span data-ttu-id="c855b-256">在命令提示字元，於本機執行應用程式時傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="c855b-256">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="c855b-257">從應用程式目錄，執行：</span><span class="sxs-lookup"><span data-stu-id="c855b-257">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --contentroot=/content-root-path
  ```

* <span data-ttu-id="c855b-258">在 IIS Express 設定檔中，將專案新增至應用程式的 `launchSettings.json` 檔案。 **IIS Express**</span><span class="sxs-lookup"><span data-stu-id="c855b-258">Add an entry to the app's `launchSettings.json` file in the **IIS Express** profile.</span></span> <span data-ttu-id="c855b-259">此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。</span><span class="sxs-lookup"><span data-stu-id="c855b-259">This setting is used when the app is run with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* <span data-ttu-id="c855b-260">在 Visual Studio 中，于 [**屬性**] [  >  **調試**  >  **程式引數**] 中指定引數。</span><span class="sxs-lookup"><span data-stu-id="c855b-260">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="c855b-261">在 [Visual Studio] 屬性頁中設定引數，會將引數新增至檔案 `launchSettings.json` 。</span><span class="sxs-lookup"><span data-stu-id="c855b-261">Setting the argument in the Visual Studio property page adds the argument to the `launchSettings.json` file.</span></span>

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a><span data-ttu-id="c855b-262">路徑基底</span><span class="sxs-lookup"><span data-stu-id="c855b-262">Path base</span></span>

<span data-ttu-id="c855b-263">`--pathbase`引數會設定應用程式以非根相對 URL 路徑在本機執行的應用程式基底路徑（此標籤 `<base>` `href` 會設定為 `/` 預備和生產以外的路徑）。</span><span class="sxs-lookup"><span data-stu-id="c855b-263">The `--pathbase` argument sets the app base path for an app run locally with a non-root relative URL path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="c855b-264">在下列範例中，`/relative-URL-path` 是應用程式的路徑基底。</span><span class="sxs-lookup"><span data-stu-id="c855b-264">In the following examples, `/relative-URL-path` is the app's path base.</span></span> <span data-ttu-id="c855b-265">如需詳細資訊，請參閱[應用程式基底路徑](xref:blazor/host-and-deploy/index#app-base-path)。</span><span class="sxs-lookup"><span data-stu-id="c855b-265">For more information, see [App base path](xref:blazor/host-and-deploy/index#app-base-path).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c855b-266">不同於提供給 `<base>` 標籤 `href` 的路徑，在傳遞 `--pathbase` 引數值時請勿包含尾端的斜線 (`/`)。</span><span class="sxs-lookup"><span data-stu-id="c855b-266">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="c855b-267">如果應用程式基底路徑提供於 `<base>` 標籤作為 `<base href="/CoolApp/">` (包括尾端的斜線)，請將命令列引數值傳遞為 `--pathbase=/CoolApp` (不含尾端的斜線)。</span><span class="sxs-lookup"><span data-stu-id="c855b-267">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/">` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="c855b-268">在命令提示字元，於本機執行應用程式時傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="c855b-268">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="c855b-269">從應用程式目錄，執行：</span><span class="sxs-lookup"><span data-stu-id="c855b-269">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --pathbase=/relative-URL-path
  ```

* <span data-ttu-id="c855b-270">在 IIS Express 設定檔中，將專案新增至應用程式的 `launchSettings.json` 檔案。 **IIS Express**</span><span class="sxs-lookup"><span data-stu-id="c855b-270">Add an entry to the app's `launchSettings.json` file in the **IIS Express** profile.</span></span> <span data-ttu-id="c855b-271">此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。</span><span class="sxs-lookup"><span data-stu-id="c855b-271">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/relative-URL-path"
  ```

* <span data-ttu-id="c855b-272">在 Visual Studio 中，于 [**屬性**] [  >  **調試**  >  **程式引數**] 中指定引數。</span><span class="sxs-lookup"><span data-stu-id="c855b-272">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="c855b-273">在 [Visual Studio] 屬性頁中設定引數，會將引數新增至檔案 `launchSettings.json` 。</span><span class="sxs-lookup"><span data-stu-id="c855b-273">Setting the argument in the Visual Studio property page adds the argument to the `launchSettings.json` file.</span></span>

  ```console
  --pathbase=/relative-URL-path
  ```

### <a name="urls"></a><span data-ttu-id="c855b-274">URL</span><span class="sxs-lookup"><span data-stu-id="c855b-274">URLs</span></span>

<span data-ttu-id="c855b-275">`--urls` 引數會搭配要用來接聽要求的連接埠和通訊協定，設定 IP 位址或主機位址。</span><span class="sxs-lookup"><span data-stu-id="c855b-275">The `--urls` argument sets the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="c855b-276">在命令提示字元，於本機執行應用程式時傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="c855b-276">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="c855b-277">從應用程式目錄，執行：</span><span class="sxs-lookup"><span data-stu-id="c855b-277">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="c855b-278">在 IIS Express 設定檔中，將專案新增至應用程式的 `launchSettings.json` 檔案。 **IIS Express**</span><span class="sxs-lookup"><span data-stu-id="c855b-278">Add an entry to the app's `launchSettings.json` file in the **IIS Express** profile.</span></span> <span data-ttu-id="c855b-279">此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。</span><span class="sxs-lookup"><span data-stu-id="c855b-279">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="c855b-280">在 Visual Studio 中，于 [**屬性**] [  >  **調試**  >  **程式引數**] 中指定引數。</span><span class="sxs-lookup"><span data-stu-id="c855b-280">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="c855b-281">在 [Visual Studio] 屬性頁中設定引數，會將引數新增至檔案 `launchSettings.json` 。</span><span class="sxs-lookup"><span data-stu-id="c855b-281">Setting the argument in the Visual Studio property page adds the argument to the `launchSettings.json` file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="configure-the-linker"></a><span data-ttu-id="c855b-282">設定連結器</span><span class="sxs-lookup"><span data-stu-id="c855b-282">Configure the Linker</span></span>

Blazor<span data-ttu-id="c855b-283">在每個發行組建上執行中繼語言（IL）連結，以從輸出元件移除不必要的 IL。</span><span class="sxs-lookup"><span data-stu-id="c855b-283"> performs Intermediate Language (IL) linking on each Release build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="c855b-284">如需詳細資訊，請參閱 <xref:blazor/host-and-deploy/configure-linker> 。</span><span class="sxs-lookup"><span data-stu-id="c855b-284">For more information, see <xref:blazor/host-and-deploy/configure-linker>.</span></span>

## <a name="custom-boot-resource-loading"></a><span data-ttu-id="c855b-285">自訂開機資源載入</span><span class="sxs-lookup"><span data-stu-id="c855b-285">Custom boot resource loading</span></span>

<span data-ttu-id="c855b-286">Blazor WebAssembly應用程式可以使用函式進行初始化 `loadBootResource` ，以覆寫內建的開機資源載入機制。</span><span class="sxs-lookup"><span data-stu-id="c855b-286">A Blazor WebAssembly app can be initialized with the `loadBootResource` function to override the built-in boot resource loading mechanism.</span></span> <span data-ttu-id="c855b-287">`loadBootResource`在下列情況下使用：</span><span class="sxs-lookup"><span data-stu-id="c855b-287">Use `loadBootResource` for the following scenarios:</span></span>

* <span data-ttu-id="c855b-288">允許使用者載入靜態資源，例如時區資料或 `dotnet.wasm` CDN。</span><span class="sxs-lookup"><span data-stu-id="c855b-288">Allow users to load static resources, such as timezone data or `dotnet.wasm` from a CDN.</span></span>
* <span data-ttu-id="c855b-289">使用 HTTP 要求載入壓縮的元件，並將它們解壓縮到不支援從伺服器提取壓縮內容的主機用戶端上。</span><span class="sxs-lookup"><span data-stu-id="c855b-289">Load compressed assemblies using an HTTP request and decompress them on the client for hosts that don't support fetching compressed contents from the server.</span></span>
* <span data-ttu-id="c855b-290">將每個要求重新導向至新名稱，以將資源別名設為不同名稱 `fetch` 。</span><span class="sxs-lookup"><span data-stu-id="c855b-290">Alias resources to a different name by redirecting each `fetch` request to a new name.</span></span>

<span data-ttu-id="c855b-291">`loadBootResource`參數會出現在下表中。</span><span class="sxs-lookup"><span data-stu-id="c855b-291">`loadBootResource` parameters appear in the following table.</span></span>

| <span data-ttu-id="c855b-292">參數</span><span class="sxs-lookup"><span data-stu-id="c855b-292">Parameter</span></span>    | <span data-ttu-id="c855b-293">說明</span><span class="sxs-lookup"><span data-stu-id="c855b-293">Description</span></span> |
| ------------ | ----------- |
| `type`       | <span data-ttu-id="c855b-294">資源類型。</span><span class="sxs-lookup"><span data-stu-id="c855b-294">The type of the resource.</span></span> <span data-ttu-id="c855b-295">運算子類型： `assembly` 、 `pdb` 、 `dotnetjs` 、 `dotnetwasm` 、`timezonedata`</span><span class="sxs-lookup"><span data-stu-id="c855b-295">Permissable types: `assembly`, `pdb`, `dotnetjs`, `dotnetwasm`, `timezonedata`</span></span> |
| `name`       | <span data-ttu-id="c855b-296">資源名稱。</span><span class="sxs-lookup"><span data-stu-id="c855b-296">The name of the resource.</span></span> |
| `defaultUri` | <span data-ttu-id="c855b-297">資源的相對或絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="c855b-297">The relative or absolute URI of the resource.</span></span> |
| `integrity`  | <span data-ttu-id="c855b-298">代表回應中預期內容的完整性字串。</span><span class="sxs-lookup"><span data-stu-id="c855b-298">The integrity string representing the expected content in the response.</span></span> |

<span data-ttu-id="c855b-299">`loadBootResource`傳回下列任何一項，以覆寫載入進程：</span><span class="sxs-lookup"><span data-stu-id="c855b-299">`loadBootResource` returns any of the following to override the loading process:</span></span>

* <span data-ttu-id="c855b-300">URI 字串。</span><span class="sxs-lookup"><span data-stu-id="c855b-300">URI string.</span></span> <span data-ttu-id="c855b-301">在下列範例（ `wwwroot/index.html` ）中，會從 CDN 提供下列檔案 `https://my-awesome-cdn.com/` ：</span><span class="sxs-lookup"><span data-stu-id="c855b-301">In the following example (`wwwroot/index.html`), the following files are served from a CDN at `https://my-awesome-cdn.com/`:</span></span>

  * `dotnet.*.js`
  * `dotnet.wasm`
  * <span data-ttu-id="c855b-302">時區資料</span><span class="sxs-lookup"><span data-stu-id="c855b-302">Timezone data</span></span>

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

* <span data-ttu-id="c855b-303">`Promise<Response>`.</span><span class="sxs-lookup"><span data-stu-id="c855b-303">`Promise<Response>`.</span></span> <span data-ttu-id="c855b-304">`integrity`在標頭中傳遞參數，以保留預設的完整性檢查行為。</span><span class="sxs-lookup"><span data-stu-id="c855b-304">Pass the `integrity` parameter in a header to retain the default integrity-checking behavior.</span></span>

  <span data-ttu-id="c855b-305">下列範例（ `wwwroot/index.html` ）會將自訂 HTTP 標頭新增至輸出要求，並將參數傳遞至 `integrity` `fetch` 呼叫：</span><span class="sxs-lookup"><span data-stu-id="c855b-305">The following example (`wwwroot/index.html`) adds a custom HTTP header to the outbound requests and passes the `integrity` parameter through to the `fetch` call:</span></span>
  
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

* <span data-ttu-id="c855b-306">`null`/`undefined`，這會產生預設的載入行為。</span><span class="sxs-lookup"><span data-stu-id="c855b-306">`null`/`undefined`, which results in the default loading behavior.</span></span>

<span data-ttu-id="c855b-307">外部來源必須傳回瀏覽器所需的 CORS 標頭，以允許跨原始來源資源載入。</span><span class="sxs-lookup"><span data-stu-id="c855b-307">External sources must return the required CORS headers for browsers to allow the cross-origin resource loading.</span></span> <span data-ttu-id="c855b-308">根據預設，Cdn 通常會提供必要的標頭。</span><span class="sxs-lookup"><span data-stu-id="c855b-308">CDNs usually provide the required headers by default.</span></span>

<span data-ttu-id="c855b-309">您只需要指定自訂行為的類型。</span><span class="sxs-lookup"><span data-stu-id="c855b-309">You only need to specify types for custom behaviors.</span></span> <span data-ttu-id="c855b-310">根據 `loadBootResource` 預設載入行為，架構會載入未指定的類型。</span><span class="sxs-lookup"><span data-stu-id="c855b-310">Types not specified to `loadBootResource` are loaded by the framework per their default loading behaviors.</span></span>

## <a name="change-the-filename-extension-of-dll-files"></a><span data-ttu-id="c855b-311">變更 DLL 檔案的副檔名</span><span class="sxs-lookup"><span data-stu-id="c855b-311">Change the filename extension of DLL files</span></span>

<span data-ttu-id="c855b-312">如果您需要變更應用程式已發佈檔案的副檔名 `.dll` ，請遵循本節中的指導方針。</span><span class="sxs-lookup"><span data-stu-id="c855b-312">In case you have a need to change the filename extensions of the app's published `.dll` files, follow the guidance in this section.</span></span>

<span data-ttu-id="c855b-313">發行應用程式之後，請使用 shell 腳本或 DevOps 組建管線，將檔案重新命名 `.dll` 為使用不同的副檔名。</span><span class="sxs-lookup"><span data-stu-id="c855b-313">After publishing the app, use a shell script or DevOps build pipeline to rename `.dll` files to use a different file extension.</span></span> <span data-ttu-id="c855b-314">`.dll`以 `wwwroot` 應用程式已發佈輸出目錄中的檔案為目標（例如， `{CONTENT ROOT}/bin/Release/netstandard2.1/publish/wwwroot` ）。</span><span class="sxs-lookup"><span data-stu-id="c855b-314">Target the `.dll` files in the `wwwroot` directory of the app's published output (for example, `{CONTENT ROOT}/bin/Release/netstandard2.1/publish/wwwroot`).</span></span>

<span data-ttu-id="c855b-315">在下列範例中，檔案 `.dll` 會重新命名為使用 `.bin` 副檔名。</span><span class="sxs-lookup"><span data-stu-id="c855b-315">In the following examples, `.dll` files are renamed to use the `.bin` file extension.</span></span>

<span data-ttu-id="c855b-316">在 Windows 上：</span><span class="sxs-lookup"><span data-stu-id="c855b-316">On Windows:</span></span>

```powershell
dir .\_framework\_bin | rename-item -NewName { $_.name -replace ".dll\b",".bin" }
((Get-Content .\_framework\blazor.boot.json -Raw) -replace '.dll"','.bin"') | Set-Content .\_framework\blazor.boot.json
```

<span data-ttu-id="c855b-317">如果服務工作者資產也在使用中，請新增下列命令：</span><span class="sxs-lookup"><span data-stu-id="c855b-317">If service worker assets are also in use, add the following command:</span></span>

```powershell
((Get-Content .\service-worker-assets.js -Raw) -replace '.dll"','.bin"') | Set-Content .\service-worker-assets.js
```

<span data-ttu-id="c855b-318">在 Linux 或 macOS 上：</span><span class="sxs-lookup"><span data-stu-id="c855b-318">On Linux or macOS:</span></span>

```console
for f in _framework/_bin/*; do mv "$f" "`echo $f | sed -e 's/\.dll\b/.bin/g'`"; done
sed -i 's/\.dll"/.bin"/g' _framework/blazor.boot.json
```

<span data-ttu-id="c855b-319">如果服務工作者資產也在使用中，請新增下列命令：</span><span class="sxs-lookup"><span data-stu-id="c855b-319">If service worker assets are also in use, add the following command:</span></span>

```console
sed -i 's/\.dll"/.bin"/g' service-worker-assets.js
```
   
<span data-ttu-id="c855b-320">若要使用不同的副檔名 `.bin` ，請 `.bin` 在上述命令中取代。</span><span class="sxs-lookup"><span data-stu-id="c855b-320">To use a different file extension than `.bin`, replace `.bin` in the preceding commands.</span></span>

<span data-ttu-id="c855b-321">若要解決壓縮的 `blazor.boot.json.gz` 和檔案 `blazor.boot.json.br` ，請採用下列其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="c855b-321">To address the compressed `blazor.boot.json.gz` and `blazor.boot.json.br` files, adopt either of the following approaches:</span></span>

* <span data-ttu-id="c855b-322">移除壓縮的 `blazor.boot.json.gz` 和檔案 `blazor.boot.json.br` 。</span><span class="sxs-lookup"><span data-stu-id="c855b-322">Remove the compressed `blazor.boot.json.gz` and `blazor.boot.json.br` files.</span></span> <span data-ttu-id="c855b-323">此方法會停用壓縮。</span><span class="sxs-lookup"><span data-stu-id="c855b-323">Compression is disabled with this approach.</span></span>
* <span data-ttu-id="c855b-324">重新壓縮更新的檔案 `blazor.boot.json` 。</span><span class="sxs-lookup"><span data-stu-id="c855b-324">Recompress the updated `blazor.boot.json` file.</span></span>

<span data-ttu-id="c855b-325">先前的指導方針也適用于服務工作者資產正在使用時。</span><span class="sxs-lookup"><span data-stu-id="c855b-325">The preceding guidance also applies when service worker assets are in use.</span></span> <span data-ttu-id="c855b-326">移除或重新壓縮 `wwwroot/service-worker-assets.js.br` 和 `wwwroot/service-worker-assets.js.gz` 。</span><span class="sxs-lookup"><span data-stu-id="c855b-326">Remove or recompress `wwwroot/service-worker-assets.js.br` and `wwwroot/service-worker-assets.js.gz`.</span></span> <span data-ttu-id="c855b-327">否則，瀏覽器中的檔案完整性檢查會失敗。</span><span class="sxs-lookup"><span data-stu-id="c855b-327">Otherwise, file integrity checks fail in the browser.</span></span>

<span data-ttu-id="c855b-328">下列 Windows 範例會使用放在專案根目錄中的 PowerShell 腳本。</span><span class="sxs-lookup"><span data-stu-id="c855b-328">The following Windows example uses a PowerShell script placed at the root of the project.</span></span>

<span data-ttu-id="c855b-329">`ChangeDLLExtensions.ps1:`:</span><span class="sxs-lookup"><span data-stu-id="c855b-329">`ChangeDLLExtensions.ps1:`:</span></span>

```powershell
param([string]$filepath,[string]$tfm)
dir $filepath\bin\Release\$tfm\wwwroot\_framework\_bin | rename-item -NewName { $_.name -replace ".dll\b",".bin" }
((Get-Content $filepath\bin\Release\$tfm\wwwroot\_framework\blazor.boot.json -Raw) -replace '.dll"','.bin"') | Set-Content $filepath\bin\Release\$tfm\wwwroot\_framework\blazor.boot.json
Remove-Item $filepath\bin\Release\$tfm\wwwroot\_framework\blazor.boot.json.gz
```

<span data-ttu-id="c855b-330">如果服務工作者資產也在使用中，請新增下列命令：</span><span class="sxs-lookup"><span data-stu-id="c855b-330">If service worker assets are also in use, add the following command:</span></span>

```powershell
((Get-Content $filepath\bin\Release\$tfm\wwwroot\service-worker-assets.js -Raw) -replace '.dll"','.bin"') | Set-Content $filepath\bin\Release\$tfm\wwwroot\service-worker-assets.js
```

<span data-ttu-id="c855b-331">在專案檔中，腳本會在發行應用程式之後執行：</span><span class="sxs-lookup"><span data-stu-id="c855b-331">In the project file, the script is run after publishing the app:</span></span>

```xml
<Target Name="ChangeDLLFileExtensions" AfterTargets="Publish" Condition="'$(Configuration)'=='Release'">
  <Exec Command="powershell.exe -command &quot;&amp; { .\ChangeDLLExtensions.ps1 '$(SolutionDir)' '$(TargetFramework)'}&quot;" />
</Target>
```

<span data-ttu-id="c855b-332">若要提供意見反應，請造訪[aspnetcore/問題 #5477](https://github.com/dotnet/aspnetcore/issues/5477)。</span><span class="sxs-lookup"><span data-stu-id="c855b-332">To provide feedback, visit [aspnetcore/issues #5477](https://github.com/dotnet/aspnetcore/issues/5477).</span></span>
 
