---
title: 裝載和部署 ASP.NET Core Blazor WebAssembly
author: guardrex
description: 瞭解如何使用 ASP.NET Core、內容傳遞Blazor網路（CDN）、檔案伺服器和 GitHub 頁面來裝載和部署應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/07/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: host-and-deploy/blazor/webassembly
ms.openlocfilehash: e136a401beffe9cc7e29906b3631ab3f068b30fd
ms.sourcegitcommit: 84b46594f57608f6ac4f0570172c7051df507520
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2020
ms.locfileid: "82967593"
---
# <a name="host-and-deploy-aspnet-core-blazor-webassembly"></a><span data-ttu-id="03eaf-103">裝載和部署 ASP.NET Core Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="03eaf-103">Host and deploy ASP.NET Core Blazor WebAssembly</span></span>

<span data-ttu-id="03eaf-104">By [Luke Latham](https://github.com/guardrex)、 [Rainer Stropek](https://www.timecockpit.com)、 [Daniel Roth](https://github.com/danroth27)、 [Ben Adams](https://twitter.com/ben_a_adams)和[Safia Abdalla](https://safia.rocks)</span><span class="sxs-lookup"><span data-stu-id="03eaf-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), [Daniel Roth](https://github.com/danroth27), [Ben Adams](https://twitter.com/ben_a_adams), and [Safia Abdalla](https://safia.rocks)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="03eaf-105">使用[Blazor WebAssembly 裝載模型](xref:blazor/hosting-models#blazor-webassembly)：</span><span class="sxs-lookup"><span data-stu-id="03eaf-105">With the [Blazor WebAssembly hosting model](xref:blazor/hosting-models#blazor-webassembly):</span></span>

* <span data-ttu-id="03eaf-106">Blazor 應用程式、其相依性和 .NET 執行時間會以平行方式下載至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="03eaf-106">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser in parallel.</span></span>
* <span data-ttu-id="03eaf-107">應用程式會直接在瀏覽器 UI 執行緒上執行。</span><span class="sxs-lookup"><span data-stu-id="03eaf-107">The app is executed directly on the browser UI thread.</span></span>

<span data-ttu-id="03eaf-108">支援下列部署策略：</span><span class="sxs-lookup"><span data-stu-id="03eaf-108">The following deployment strategies are supported:</span></span>

* <span data-ttu-id="03eaf-109">Blazor 應用程式由 ASP.NET Core 應用程式提供服務。</span><span class="sxs-lookup"><span data-stu-id="03eaf-109">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="03eaf-110">此策略已於[搭配 ASP.NET Core 的已裝載部署](#hosted-deployment-with-aspnet-core)一節中涵蓋。</span><span class="sxs-lookup"><span data-stu-id="03eaf-110">This strategy is covered in the [Hosted deployment with ASP.NET Core](#hosted-deployment-with-aspnet-core) section.</span></span>
* <span data-ttu-id="03eaf-111">Blazor 應用程式放在靜態裝載的網頁伺服器或服務上，其中 .NET 不會用來提供服務給 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="03eaf-111">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="03eaf-112">此策略涵蓋于[獨立部署](#standalone-deployment)一節，其中包含將 Blazor WebAssembly 應用程式裝載為 IIS 子應用程式的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="03eaf-112">This strategy is covered in the [Standalone deployment](#standalone-deployment) section, which includes information on hosting a Blazor WebAssembly app as an IIS sub-app.</span></span>

## <a name="brotli-precompression"></a><span data-ttu-id="03eaf-113">Brotli precompression</span><span class="sxs-lookup"><span data-stu-id="03eaf-113">Brotli precompression</span></span>

<span data-ttu-id="03eaf-114">發行 Blazor WebAssembly 應用程式時，會使用最高層級的[Brotli 壓縮演算法](https://tools.ietf.org/html/rfc7932)來 precompressed 輸出，以減少應用程式大小，並移除執行時間壓縮的需求。</span><span class="sxs-lookup"><span data-stu-id="03eaf-114">When a Blazor WebAssembly app is published, the output is precompressed using the [Brotli compression algorithm](https://tools.ietf.org/html/rfc7932) at the highest level to reduce the app size and remove the need for runtime compression.</span></span>

<span data-ttu-id="03eaf-115">如需 IIS *web.config*壓縮設定，請參閱[iis： Brotli 和 Gzip 壓縮](#brotli-and-gzip-compression)一節。</span><span class="sxs-lookup"><span data-stu-id="03eaf-115">For IIS *web.config* compression configuration, see the [IIS: Brotli and Gzip compression](#brotli-and-gzip-compression) section.</span></span>

## <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="03eaf-116">重寫 URL 以便正確地路由</span><span class="sxs-lookup"><span data-stu-id="03eaf-116">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="03eaf-117">Blazor WebAssembly 應用程式中頁面元件的路由要求，並不像是 Blazor 伺服器（裝載的應用程式）中的路由要求一樣簡單。</span><span class="sxs-lookup"><span data-stu-id="03eaf-117">Routing requests for page components in a Blazor WebAssembly app isn't as straightforward as routing requests in a Blazor Server, hosted app.</span></span> <span data-ttu-id="03eaf-118">假設有兩個元件的 Blazor WebAssembly 應用程式：</span><span class="sxs-lookup"><span data-stu-id="03eaf-118">Consider a Blazor WebAssembly app with two components:</span></span>

* <span data-ttu-id="03eaf-119">*Main.razor* &ndash; 載入應用程式根目錄，同時包含 `About` 元件 (`href="About"`) 的連結。</span><span class="sxs-lookup"><span data-stu-id="03eaf-119">*Main.razor* &ndash; Loads at the root of the app and contains a link to the `About` component (`href="About"`).</span></span>
* <span data-ttu-id="03eaf-120">*About.razor* &ndash; `About` 元件。</span><span class="sxs-lookup"><span data-stu-id="03eaf-120">*About.razor* &ndash; `About` component.</span></span>

<span data-ttu-id="03eaf-121">使用瀏覽器的網址列要求應用程式預設文件時 (例如 `https://www.contoso.com/`)：</span><span class="sxs-lookup"><span data-stu-id="03eaf-121">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="03eaf-122">瀏覽器提出要求。</span><span class="sxs-lookup"><span data-stu-id="03eaf-122">The browser makes a request.</span></span>
1. <span data-ttu-id="03eaf-123">傳回預設頁面，這通常是 *index.html*。</span><span class="sxs-lookup"><span data-stu-id="03eaf-123">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="03eaf-124">*index.html* 啟動載入應用程式。</span><span class="sxs-lookup"><span data-stu-id="03eaf-124">*index.html* bootstraps the app.</span></span>
1. <span data-ttu-id="03eaf-125">Blazor 的路由器會載入，且會轉譯 Razor `Main` 元件。</span><span class="sxs-lookup"><span data-stu-id="03eaf-125">Blazor's router loads, and the Razor `Main` component is rendered.</span></span>

<span data-ttu-id="03eaf-126">在主頁面中，選取用戶端 `About` 元件的連結有用，因為 Blazor 路由器會停止瀏覽器，使其不再從網際網路對 `www.contoso.com` 提出針對 `About` 的要求，並自行提供轉譯的 `About` 元件。</span><span class="sxs-lookup"><span data-stu-id="03eaf-126">In the Main page, selecting the link to the `About` component works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the rendered `About` component itself.</span></span> <span data-ttu-id="03eaf-127">*Blazor WebAssembly 應用程式內*內部端點的所有要求都以相同的方式工作：要求不會對網際網路上伺服器裝載的資源觸發瀏覽器型要求。</span><span class="sxs-lookup"><span data-stu-id="03eaf-127">All of the requests for internal endpoints *within the Blazor WebAssembly app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="03eaf-128">路由器會在內部處理要求。</span><span class="sxs-lookup"><span data-stu-id="03eaf-128">The router handles the requests internally.</span></span>

<span data-ttu-id="03eaf-129">如果使用瀏覽器之網址列提出對 `www.contoso.com/About` 的要求，則要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="03eaf-129">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="03eaf-130">在應用程式的網際網路主機上沒有這類資源存在，因此會傳回「404 - 找不到」\*\* 的回應。</span><span class="sxs-lookup"><span data-stu-id="03eaf-130">No such resource exists on the app's Internet host, so a *404 - Not Found* response is returned.</span></span>

<span data-ttu-id="03eaf-131">因為瀏覽器會提出要求給以網際網路為基礎的主機以取得用戶端頁面，所以網頁伺服器和裝載服務必須針對實際上不在伺服器上資源，將所有要求重新撰寫至 *index.html* 頁面。</span><span class="sxs-lookup"><span data-stu-id="03eaf-131">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="03eaf-132">當傳回*index. html*時，應用程式的 Blazor 路由器會接管並回應正確的資源。</span><span class="sxs-lookup"><span data-stu-id="03eaf-132">When *index.html* is returned, the app's Blazor router takes over and responds with the correct resource.</span></span>

<span data-ttu-id="03eaf-133">部署到 IIS 伺服器時，您可以使用 URL 重寫模組搭配應用*程式已發佈的 web.config 檔案*。</span><span class="sxs-lookup"><span data-stu-id="03eaf-133">When deploying to an IIS server, you can use the URL Rewrite Module with the app's published *web.config* file.</span></span> <span data-ttu-id="03eaf-134">如需詳細資訊，請參閱[IIS](#iis)一節。</span><span class="sxs-lookup"><span data-stu-id="03eaf-134">For more information, see the [IIS](#iis) section.</span></span>

## <a name="hosted-deployment-with-aspnet-core"></a><span data-ttu-id="03eaf-135">搭配 ASP.NET Core 的已裝載部署</span><span class="sxs-lookup"><span data-stu-id="03eaf-135">Hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="03eaf-136">*託管部署*可從 web 伺服器上執行的[ASP.NET Core 應用程式](xref:index)，將 Blazor WebAssembly 應用程式提供給瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="03eaf-136">A *hosted deployment* serves the Blazor WebAssembly app to browsers from an [ASP.NET Core app](xref:index) that runs on a web server.</span></span>

<span data-ttu-id="03eaf-137">用戶端 Blazor WebAssembly 應用程式會發佈到伺服器應用程式的 */BIN/RELEASE/{TARGET FRAMEWORK}/publish/wwwroot*資料夾中，以及伺服器應用程式的任何其他靜態 web 資產。</span><span class="sxs-lookup"><span data-stu-id="03eaf-137">The client Blazor WebAssembly app is published into the */bin/Release/{TARGET FRAMEWORK}/publish/wwwroot* folder of the server app, along with any other static web assets of the server app.</span></span> <span data-ttu-id="03eaf-138">這兩個應用程式會一起部署。</span><span class="sxs-lookup"><span data-stu-id="03eaf-138">The two apps are deployed together.</span></span> <span data-ttu-id="03eaf-139">需要有能夠裝載 ASP.NET Core 應用程式的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="03eaf-139">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="03eaf-140">針對**裝載的部署**，Visual Studio 包括**Blazor WebAssembly 應用程式**專案範本（`blazorwasm`使用[dotnet new](/dotnet/core/tools/dotnet-new)命令時的範本）和選取的裝載選項（`-ho|--hosted`使用`dotnet new`命令時）。</span><span class="sxs-lookup"><span data-stu-id="03eaf-140">For a hosted deployment, Visual Studio includes the **Blazor WebAssembly App** project template (`blazorwasm` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command) with the **Hosted** option selected (`-ho|--hosted` when using the `dotnet new` command).</span></span>

<span data-ttu-id="03eaf-141">如需 ASP.NET Core 應用程式裝載和部署的詳細資訊，請參閱 <xref:host-and-deploy/index>。</span><span class="sxs-lookup"><span data-stu-id="03eaf-141">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="03eaf-142">如需部署至 Azure App Service 的相關資訊，請參閱 <xref:tutorials/publish-to-azure-webapp-using-vs>。</span><span class="sxs-lookup"><span data-stu-id="03eaf-142">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>

## <a name="standalone-deployment"></a><span data-ttu-id="03eaf-143">獨立部署</span><span class="sxs-lookup"><span data-stu-id="03eaf-143">Standalone deployment</span></span>

<span data-ttu-id="03eaf-144">*獨立部署*可將 Blazor WebAssembly 應用程式當做一組直接由用戶端要求的靜態檔案來提供。</span><span class="sxs-lookup"><span data-stu-id="03eaf-144">A *standalone deployment* serves the Blazor WebAssembly app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="03eaf-145">所有靜態檔案伺服器都能夠支援 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="03eaf-145">Any static file server is able to serve the Blazor app.</span></span>

<span data-ttu-id="03eaf-146">獨立部署資產會發佈到 */BIN/RELEASE/{TARGET FRAMEWORK}/publish/wwwroot*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="03eaf-146">Standalone deployment assets are published into the */bin/Release/{TARGET FRAMEWORK}/publish/wwwroot* folder.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="03eaf-147">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="03eaf-147">Azure App Service</span></span>

<span data-ttu-id="03eaf-148">Blazor WebAssembly 應用程式可以部署到 Windows 上的 Azure App 服務，在[IIS](#iis)上裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="03eaf-148">Blazor WebAssembly apps can be deployed to Azure App Services on Windows, which hosts the app on [IIS](#iis).</span></span>

<span data-ttu-id="03eaf-149">目前不支援將獨立 Blazor WebAssembly 應用程式部署至適用于 Linux 的 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="03eaf-149">Deploying a standalone Blazor WebAssembly app to Azure App Service for Linux isn't currently supported.</span></span> <span data-ttu-id="03eaf-150">目前無法使用可裝載應用程式的 Linux 伺服器映射。</span><span class="sxs-lookup"><span data-stu-id="03eaf-150">A Linux server image to host the app isn't available at this time.</span></span> <span data-ttu-id="03eaf-151">正在進行工作，以啟用此案例。</span><span class="sxs-lookup"><span data-stu-id="03eaf-151">Work is in progress to enable this scenario.</span></span>

### <a name="iis"></a><span data-ttu-id="03eaf-152">IIS</span><span class="sxs-lookup"><span data-stu-id="03eaf-152">IIS</span></span>

<span data-ttu-id="03eaf-153">IIS 是足以支援 Blazor 應用程式的靜態檔案伺服器。</span><span class="sxs-lookup"><span data-stu-id="03eaf-153">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="03eaf-154">若要設定 IIS 來裝載 Blazor，請參閱[在 IIS 上建置靜態網站](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="03eaf-154">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="03eaf-155">已發行的資產會建立在 */bin/Release/{TARGET FRAMEWORK}/publish* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="03eaf-155">Published assets are created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="03eaf-156">在網頁伺服器或裝載服務上，裝載 *publish* 資料夾的內容。</span><span class="sxs-lookup"><span data-stu-id="03eaf-156">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

#### <a name="webconfig"></a><span data-ttu-id="03eaf-157">web.config</span><span class="sxs-lookup"><span data-stu-id="03eaf-157">web.config</span></span>

<span data-ttu-id="03eaf-158">發佈 Blazor 專案時，會建立 *web.config* 檔案，並使用下列 IIS 組態：</span><span class="sxs-lookup"><span data-stu-id="03eaf-158">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="03eaf-159">針對下列副檔名設定 MIME 類型：</span><span class="sxs-lookup"><span data-stu-id="03eaf-159">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="03eaf-160">*.dll* &ndash;`application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="03eaf-160">*.dll* &ndash; `application/octet-stream`</span></span>
  * <span data-ttu-id="03eaf-161">*. json* &ndash;`application/json`</span><span class="sxs-lookup"><span data-stu-id="03eaf-161">*.json* &ndash; `application/json`</span></span>
  * <span data-ttu-id="03eaf-162">*.wasm* &ndash; `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="03eaf-162">*.wasm* &ndash; `application/wasm`</span></span>
  * <span data-ttu-id="03eaf-163">*.woff* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="03eaf-163">*.woff* &ndash; `application/font-woff`</span></span>
  * <span data-ttu-id="03eaf-164">*.woff2* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="03eaf-164">*.woff2* &ndash; `application/font-woff`</span></span>
* <span data-ttu-id="03eaf-165">針對下列 MIME 類型會啟用 HTTP 壓縮：</span><span class="sxs-lookup"><span data-stu-id="03eaf-165">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="03eaf-166">建立 URL Rewrite Module 規則：</span><span class="sxs-lookup"><span data-stu-id="03eaf-166">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="03eaf-167">提供應用程式靜態資產所在的子目錄（已*要求 wwwroot/{PATH}*）。</span><span class="sxs-lookup"><span data-stu-id="03eaf-167">Serve the sub-directory where the app's static assets reside (*wwwroot/{PATH REQUESTED}*).</span></span>
  * <span data-ttu-id="03eaf-168">建立 SPA fallback 路由，讓非檔案資產的要求會重新導向至其靜態資產資料夾（*wwwroot/index.html*）中的應用程式預設檔。</span><span class="sxs-lookup"><span data-stu-id="03eaf-168">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*wwwroot/index.html*).</span></span>
  
#### <a name="use-a-custom-webconfig"></a><span data-ttu-id="03eaf-169">使用自訂的 web.config</span><span class="sxs-lookup"><span data-stu-id="03eaf-169">Use a custom web.config</span></span>

<span data-ttu-id="03eaf-170">若要使用自訂的*web.config*檔案，請*將自訂的 web.config 檔案*放在專案資料夾的根目錄，併發行專案。</span><span class="sxs-lookup"><span data-stu-id="03eaf-170">To use a custom *web.config* file, place the custom *web.config* file at the root of the project folder and publish the project.</span></span>

#### <a name="install-the-url-rewrite-module"></a><span data-ttu-id="03eaf-171">安裝 URL Rewrite 模組</span><span class="sxs-lookup"><span data-stu-id="03eaf-171">Install the URL Rewrite Module</span></span>

<span data-ttu-id="03eaf-172">需要 [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite)，才可重寫 URL。</span><span class="sxs-lookup"><span data-stu-id="03eaf-172">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="03eaf-173">預設不會安裝此模組，且其無法用來安裝為網頁伺服器 (IIS) 角色服務功能。</span><span class="sxs-lookup"><span data-stu-id="03eaf-173">The module isn't installed by default, and it isn't available for install as a Web Server (IIS) role service feature.</span></span> <span data-ttu-id="03eaf-174">必須從 IIS 網站下載模組。</span><span class="sxs-lookup"><span data-stu-id="03eaf-174">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="03eaf-175">請使用 Web Platform Installer 安裝模組：</span><span class="sxs-lookup"><span data-stu-id="03eaf-175">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="03eaf-176">在本機上，巡覽至 [URL Rewrite Module 下載頁面](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads)。</span><span class="sxs-lookup"><span data-stu-id="03eaf-176">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="03eaf-177">如需英文版，請選取 [WebPI]\*\*\*\* 下載 WebPI 安裝程式。</span><span class="sxs-lookup"><span data-stu-id="03eaf-177">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="03eaf-178">如需其他語言，請選取適當的伺服器架構 (x86 x64) 來下載安裝程式。</span><span class="sxs-lookup"><span data-stu-id="03eaf-178">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="03eaf-179">將安裝程式複製到伺服器。</span><span class="sxs-lookup"><span data-stu-id="03eaf-179">Copy the installer to the server.</span></span> <span data-ttu-id="03eaf-180">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="03eaf-180">Run the installer.</span></span> <span data-ttu-id="03eaf-181">選取 [安裝]\*\*\*\* 按鈕，並接受授權條款。</span><span class="sxs-lookup"><span data-stu-id="03eaf-181">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="03eaf-182">安裝完成之後，不需要重新啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="03eaf-182">A server restart isn't required after the install completes.</span></span>

#### <a name="configure-the-website"></a><span data-ttu-id="03eaf-183">設定網站</span><span class="sxs-lookup"><span data-stu-id="03eaf-183">Configure the website</span></span>

<span data-ttu-id="03eaf-184">將網站的 [實體路徑]\*\*\*\* 設為應用程式的資料夾。</span><span class="sxs-lookup"><span data-stu-id="03eaf-184">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="03eaf-185">此資料夾包含：</span><span class="sxs-lookup"><span data-stu-id="03eaf-185">The folder contains:</span></span>

* <span data-ttu-id="03eaf-186">IIS 用來設定網站的 *web.config* 檔，包括必要的重新導向規則和檔案內容類型。</span><span class="sxs-lookup"><span data-stu-id="03eaf-186">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="03eaf-187">應用程式的靜態資產資料夾。</span><span class="sxs-lookup"><span data-stu-id="03eaf-187">The app's static asset folder.</span></span>

#### <a name="host-as-an-iis-sub-app"></a><span data-ttu-id="03eaf-188">以 IIS 子應用程式的形式裝載</span><span class="sxs-lookup"><span data-stu-id="03eaf-188">Host as an IIS sub-app</span></span>

<span data-ttu-id="03eaf-189">如果獨立應用程式裝載為 IIS 子應用程式，請執行下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="03eaf-189">If a standalone app is hosted as an IIS sub-app, perform either of the following:</span></span>

* <span data-ttu-id="03eaf-190">停用繼承的 ASP.NET Core 模組處理常式。</span><span class="sxs-lookup"><span data-stu-id="03eaf-190">Disable the inherited ASP.NET Core Module handler.</span></span>

  <span data-ttu-id="03eaf-191">將`<handlers>`區段新增至檔案，以移除 Blazor 應用*程式已發佈的 web.config 檔案*中的處理常式：</span><span class="sxs-lookup"><span data-stu-id="03eaf-191">Remove the handler in the Blazor app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>

  ```xml
  <handlers>
    <remove name="aspNetCore" />
  </handlers>
  ```

* <span data-ttu-id="03eaf-192">使用`inheritInChildApplications`設定為`false`的`<location>`元素，停用根（ `<system.webServer>`父系）應用程式區段的繼承：</span><span class="sxs-lookup"><span data-stu-id="03eaf-192">Disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>

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

<span data-ttu-id="03eaf-193">除了設定[應用程式的基底路徑](xref:host-and-deploy/blazor/index#app-base-path)之外，也會執行移除處理常式或停用繼承。</span><span class="sxs-lookup"><span data-stu-id="03eaf-193">Removing the handler or disabling inheritance is performed in addition to [configuring the app's base path](xref:host-and-deploy/blazor/index#app-base-path).</span></span> <span data-ttu-id="03eaf-194">在應用程式的 *index.html* 檔案，將應用程式基底路徑設為在 IIS 中設定子應用程式時使用的 IIS 別名。</span><span class="sxs-lookup"><span data-stu-id="03eaf-194">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

#### <a name="brotli-and-gzip-compression"></a><span data-ttu-id="03eaf-195">Brotli 和 Gzip 壓縮</span><span class="sxs-lookup"><span data-stu-id="03eaf-195">Brotli and Gzip compression</span></span>

<span data-ttu-id="03eaf-196">您可以*透過 web.config 來設定 IIS，以*提供 Brotli 或 Gzip 壓縮 Blazor 資產。</span><span class="sxs-lookup"><span data-stu-id="03eaf-196">IIS can be configured via *web.config* to serve Brotli or Gzip compressed Blazor assets.</span></span> <span data-ttu-id="03eaf-197">如需設定範例，請參閱[web.config](webassembly/_samples/web.config?raw=true)。</span><span class="sxs-lookup"><span data-stu-id="03eaf-197">For an example configuration, see [web.config](webassembly/_samples/web.config?raw=true).</span></span>

#### <a name="troubleshooting"></a><span data-ttu-id="03eaf-198">疑難排解</span><span class="sxs-lookup"><span data-stu-id="03eaf-198">Troubleshooting</span></span>

<span data-ttu-id="03eaf-199">如果收到「500 - 內部伺服器錯誤」\*\*，且 IIS 管理員在嘗試存取網站設定時擲回錯誤，請確認是否已安裝 URL Rewrite 模組。</span><span class="sxs-lookup"><span data-stu-id="03eaf-199">If a *500 - Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="03eaf-200">未安裝此模組時，IIS 無法剖析 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="03eaf-200">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="03eaf-201">這導致 IIS 管理員無法載入網站的組態，且網站無法提供 Blazor 的靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="03eaf-201">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="03eaf-202">如需針對部署至 IIS 進行疑難排解的詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="03eaf-202">For more information on troubleshooting deployments to IIS, see <xref:test/troubleshoot-azure-iis>.</span></span>

### <a name="azure-storage"></a><span data-ttu-id="03eaf-203">Azure 儲存體</span><span class="sxs-lookup"><span data-stu-id="03eaf-203">Azure Storage</span></span>

<span data-ttu-id="03eaf-204">[Azure 儲存體](/azure/storage/)靜態檔案裝載允許裝載無伺服器 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="03eaf-204">[Azure Storage](/azure/storage/) static file hosting allows serverless Blazor app hosting.</span></span> <span data-ttu-id="03eaf-205">支援自訂網域名稱、Azure 內容傳遞網路 (CDN) 及 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="03eaf-205">Custom domain names, the Azure Content Delivery Network (CDN), and HTTPS are supported.</span></span>

<span data-ttu-id="03eaf-206">當 Blob 服務針對儲存體帳戶上的靜態網站裝載啟用時：</span><span class="sxs-lookup"><span data-stu-id="03eaf-206">When the blob service is enabled for static website hosting on a storage account:</span></span>

* <span data-ttu-id="03eaf-207">將 [索引文件名稱]\*\*\*\* 設定為 `index.html`。</span><span class="sxs-lookup"><span data-stu-id="03eaf-207">Set the **Index document name** to `index.html`.</span></span>
* <span data-ttu-id="03eaf-208">將 [錯誤文件路徑]\*\*\*\* 設定為 `index.html`。</span><span class="sxs-lookup"><span data-stu-id="03eaf-208">Set the **Error document path** to `index.html`.</span></span> <span data-ttu-id="03eaf-209">Razor 元件和其他非檔案端點不會位於由 Blob 服務所存放之靜態內容中的實體路徑上。</span><span class="sxs-lookup"><span data-stu-id="03eaf-209">Razor components and other non-file endpoints don't reside at physical paths in the static content stored by the blob service.</span></span> <span data-ttu-id="03eaf-210">當系統接收到針對這些資源之一，且應由 Blazor 路由器處理的要求時，由 Blob 服務所產生的「404 - 找不到」\*\* 錯誤會將要求路由至**錯誤文件路徑**。</span><span class="sxs-lookup"><span data-stu-id="03eaf-210">When a request for one of these resources is received that the Blazor router should handle, the *404 - Not Found* error generated by the blob service routes the request to the **Error document path**.</span></span> <span data-ttu-id="03eaf-211">系統會傳回 *index.html* Blob，且 Blazor 路由器會載入並處理該路徑。</span><span class="sxs-lookup"><span data-stu-id="03eaf-211">The *index.html* blob is returned, and the Blazor router loads and processes the path.</span></span>

<span data-ttu-id="03eaf-212">如需詳細資訊，請參閱 [Azure 儲存體中的靜態網站裝載](/azure/storage/blobs/storage-blob-static-website)。</span><span class="sxs-lookup"><span data-stu-id="03eaf-212">For more information, see [Static website hosting in Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span></span>

### <a name="nginx"></a><span data-ttu-id="03eaf-213">Nginx</span><span class="sxs-lookup"><span data-stu-id="03eaf-213">Nginx</span></span>

<span data-ttu-id="03eaf-214">下列*nginx*檔案已簡化，示範如何將 nginx 設定為每當找不到磁片上的對應檔案時，就傳送 *.html 檔案。*</span><span class="sxs-lookup"><span data-stu-id="03eaf-214">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *index.html* file whenever it can't find a corresponding file on disk.</span></span>

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

<span data-ttu-id="03eaf-215">如需生產環境 Nginx 網頁伺服器組態的詳細資訊，請參閱[建立 NGINX Plus 和 NGINX 組態檔](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/)。</span><span class="sxs-lookup"><span data-stu-id="03eaf-215">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

### <a name="nginx-in-docker"></a><span data-ttu-id="03eaf-216">Docker 中的 Nginx</span><span class="sxs-lookup"><span data-stu-id="03eaf-216">Nginx in Docker</span></span>

<span data-ttu-id="03eaf-217">若要在 Docker 中使用 Nginx 裝載 Blazor，請設定 Dockerfile 以使用以 Alpine 為基礎的 Nginx 映像。</span><span class="sxs-lookup"><span data-stu-id="03eaf-217">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="03eaf-218">更新 Dockerfile，將 *nginx.config* 檔案複製到容器內。</span><span class="sxs-lookup"><span data-stu-id="03eaf-218">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="03eaf-219">如下列範例所示，新增一行至 Dockerfile：</span><span class="sxs-lookup"><span data-stu-id="03eaf-219">Add one line to the Dockerfile, as shown in the following example:</span></span>

```dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="apache"></a><span data-ttu-id="03eaf-220">Apache</span><span class="sxs-lookup"><span data-stu-id="03eaf-220">Apache</span></span>

<span data-ttu-id="03eaf-221">若要將 Blazor WebAssembly 應用程式部署到 CentOS 7 或更新版本：</span><span class="sxs-lookup"><span data-stu-id="03eaf-221">To deploy a Blazor WebAssembly app to CentOS 7 or later:</span></span>

1. <span data-ttu-id="03eaf-222">建立 Apache 設定檔。</span><span class="sxs-lookup"><span data-stu-id="03eaf-222">Create the Apache configuration file.</span></span> <span data-ttu-id="03eaf-223">下列範例是一個簡化的設定檔案（*blazorapp*）：</span><span class="sxs-lookup"><span data-stu-id="03eaf-223">The following example is a simplified configuration file (*blazorapp.config*):</span></span>

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

1. <span data-ttu-id="03eaf-224">將 Apache 設定檔案放在`/etc/httpd/conf.d/`目錄中，這是 CentOS 7 中的預設 Apache 設定目錄。</span><span class="sxs-lookup"><span data-stu-id="03eaf-224">Place the Apache configuration file into the `/etc/httpd/conf.d/` directory, which is the default Apache configuration directory in CentOS 7.</span></span>

1. <span data-ttu-id="03eaf-225">將應用程式的檔案放入`/var/www/blazorapp`目錄`DocumentRoot`中（在設定檔中指定的位置）。</span><span class="sxs-lookup"><span data-stu-id="03eaf-225">Place the app's files into the `/var/www/blazorapp` directory (the location specified to `DocumentRoot` in the configuration file).</span></span>

1. <span data-ttu-id="03eaf-226">重新開機 Apache 服務。</span><span class="sxs-lookup"><span data-stu-id="03eaf-226">Restart the Apache service.</span></span>

<span data-ttu-id="03eaf-227">如需詳細資訊，請參閱[mod_mime](https://httpd.apache.org/docs/2.4/mod/mod_mime.html)和[mod_deflate](https://httpd.apache.org/docs/current/mod/mod_deflate.html)。</span><span class="sxs-lookup"><span data-stu-id="03eaf-227">For more information, see [mod_mime](https://httpd.apache.org/docs/2.4/mod/mod_mime.html) and [mod_deflate](https://httpd.apache.org/docs/current/mod/mod_deflate.html).</span></span>

### <a name="github-pages"></a><span data-ttu-id="03eaf-228">GitHub Pages</span><span class="sxs-lookup"><span data-stu-id="03eaf-228">GitHub Pages</span></span>

<span data-ttu-id="03eaf-229">若要處理 URL 重寫，請新增 *404.html* 檔，以及處理將要求重新導向至 *index.html* 頁面的指令碼。</span><span class="sxs-lookup"><span data-stu-id="03eaf-229">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="03eaf-230">如需社群所提供的範例實作，請參閱 [GitHub Pages 的單一頁面應用程式](https://spa-github-pages.rafrex.com/) (GitHub 上的 [rafrex/spa-github-pages](https://github.com/rafrex/spa-github-pages#readme))。</span><span class="sxs-lookup"><span data-stu-id="03eaf-230">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](https://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="03eaf-231">使用社群方法的範例可在 [GitHub 上的 blazor-demo/blazor-demo.github.io](https://github.com/blazor-demo/blazor-demo.github.io) ([即時網站](https://blazor-demo.github.io/)) 看到。</span><span class="sxs-lookup"><span data-stu-id="03eaf-231">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="03eaf-232">使用專案網站，而非組織網站時，請在 *index.html* 中新增或更新 `<base>` 標籤。</span><span class="sxs-lookup"><span data-stu-id="03eaf-232">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="03eaf-233">將 `href` 屬性值設定為 GitHub 存放庫名稱，並在尾端加上斜線 (例如 `my-repository/`)。</span><span class="sxs-lookup"><span data-stu-id="03eaf-233">Set the `href` attribute value to the GitHub repository name with a trailing slash (for example, `my-repository/`.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="03eaf-234">主機組態值</span><span class="sxs-lookup"><span data-stu-id="03eaf-234">Host configuration values</span></span>

<span data-ttu-id="03eaf-235">[Blazor WebAssembly apps](xref:blazor/hosting-models#blazor-webassembly)可以在開發環境中的執行時間，接受下列主機設定值做為命令列引數。</span><span class="sxs-lookup"><span data-stu-id="03eaf-235">[Blazor WebAssembly apps](xref:blazor/hosting-models#blazor-webassembly) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="03eaf-236">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="03eaf-236">Content root</span></span>

<span data-ttu-id="03eaf-237">`--contentroot`引數會設定包含應用程式內容檔案（[內容根](xref:fundamentals/index#content-root)）之目錄的絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="03eaf-237">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files ([content root](xref:fundamentals/index#content-root)).</span></span> <span data-ttu-id="03eaf-238">在下列範例中，`/content-root-path` 是應用程式的內容根路徑。</span><span class="sxs-lookup"><span data-stu-id="03eaf-238">In the following examples, `/content-root-path` is the app's content root path.</span></span>

* <span data-ttu-id="03eaf-239">在命令提示字元，於本機執行應用程式時傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="03eaf-239">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="03eaf-240">從應用程式目錄，執行：</span><span class="sxs-lookup"><span data-stu-id="03eaf-240">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --contentroot=/content-root-path
  ```

* <span data-ttu-id="03eaf-241">新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="03eaf-241">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="03eaf-242">此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。</span><span class="sxs-lookup"><span data-stu-id="03eaf-242">This setting is used when the app is run with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* <span data-ttu-id="03eaf-243">在 Visual Studio 中，于 [**屬性** > ] [**調試** > **程式引數**] 中指定引數。</span><span class="sxs-lookup"><span data-stu-id="03eaf-243">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="03eaf-244">在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="03eaf-244">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a><span data-ttu-id="03eaf-245">路徑基底</span><span class="sxs-lookup"><span data-stu-id="03eaf-245">Path base</span></span>

<span data-ttu-id="03eaf-246">引數`href`會設定應用程式以非根相對 URL 路徑在本機執行的應用程式基底路徑（ `<base>`此標籤會設定為預備和`/`生產以外的路徑）。 `--pathbase`</span><span class="sxs-lookup"><span data-stu-id="03eaf-246">The `--pathbase` argument sets the app base path for an app run locally with a non-root relative URL path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="03eaf-247">在下列範例中，`/relative-URL-path` 是應用程式的路徑基底。</span><span class="sxs-lookup"><span data-stu-id="03eaf-247">In the following examples, `/relative-URL-path` is the app's path base.</span></span> <span data-ttu-id="03eaf-248">如需詳細資訊，請參閱[應用程式基底路徑](xref:host-and-deploy/blazor/index#app-base-path)。</span><span class="sxs-lookup"><span data-stu-id="03eaf-248">For more information, see [App base path](xref:host-and-deploy/blazor/index#app-base-path).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="03eaf-249">不同於提供給 `<base>` 標籤 `href` 的路徑，在傳遞 `--pathbase` 引數值時請勿包含尾端的斜線 (`/`)。</span><span class="sxs-lookup"><span data-stu-id="03eaf-249">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="03eaf-250">如果應用程式基底路徑提供於 `<base>` 標籤作為 `<base href="/CoolApp/">` (包括尾端的斜線)，請將命令列引數值傳遞為 `--pathbase=/CoolApp` (不含尾端的斜線)。</span><span class="sxs-lookup"><span data-stu-id="03eaf-250">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/">` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="03eaf-251">在命令提示字元，於本機執行應用程式時傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="03eaf-251">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="03eaf-252">從應用程式目錄，執行：</span><span class="sxs-lookup"><span data-stu-id="03eaf-252">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --pathbase=/relative-URL-path
  ```

* <span data-ttu-id="03eaf-253">新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="03eaf-253">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="03eaf-254">此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。</span><span class="sxs-lookup"><span data-stu-id="03eaf-254">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/relative-URL-path"
  ```

* <span data-ttu-id="03eaf-255">在 Visual Studio 中，于 [**屬性** > ] [**調試** > **程式引數**] 中指定引數。</span><span class="sxs-lookup"><span data-stu-id="03eaf-255">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="03eaf-256">在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="03eaf-256">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/relative-URL-path
  ```

### <a name="urls"></a><span data-ttu-id="03eaf-257">URL</span><span class="sxs-lookup"><span data-stu-id="03eaf-257">URLs</span></span>

<span data-ttu-id="03eaf-258">`--urls` 引數會搭配要用來接聽要求的連接埠和通訊協定，設定 IP 位址或主機位址。</span><span class="sxs-lookup"><span data-stu-id="03eaf-258">The `--urls` argument sets the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="03eaf-259">在命令提示字元，於本機執行應用程式時傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="03eaf-259">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="03eaf-260">從應用程式目錄，執行：</span><span class="sxs-lookup"><span data-stu-id="03eaf-260">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="03eaf-261">新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="03eaf-261">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="03eaf-262">此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。</span><span class="sxs-lookup"><span data-stu-id="03eaf-262">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="03eaf-263">在 Visual Studio 中，于 [**屬性** > ] [**調試** > **程式引數**] 中指定引數。</span><span class="sxs-lookup"><span data-stu-id="03eaf-263">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="03eaf-264">在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="03eaf-264">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="configure-the-linker"></a><span data-ttu-id="03eaf-265">設定連結器</span><span class="sxs-lookup"><span data-stu-id="03eaf-265">Configure the Linker</span></span>

<span data-ttu-id="03eaf-266">Blazor 會在每個發行組建上執行中繼語言（IL）連結，以從輸出元件中移除不必要的 IL。</span><span class="sxs-lookup"><span data-stu-id="03eaf-266">Blazor performs Intermediate Language (IL) linking on each Release build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="03eaf-267">如需詳細資訊，請參閱<xref:host-and-deploy/blazor/configure-linker>。</span><span class="sxs-lookup"><span data-stu-id="03eaf-267">For more information, see <xref:host-and-deploy/blazor/configure-linker>.</span></span>

## <a name="custom-boot-resource-loading"></a><span data-ttu-id="03eaf-268">自訂開機資源載入</span><span class="sxs-lookup"><span data-stu-id="03eaf-268">Custom boot resource loading</span></span>

<span data-ttu-id="03eaf-269">您可以使用`loadBootResource`函式來初始化 Blazor WebAssembly 應用程式，以覆寫內建的開機資源載入機制。</span><span class="sxs-lookup"><span data-stu-id="03eaf-269">A Blazor WebAssembly app can be initialized with the `loadBootResource` function to override the built-in boot resource loading mechanism.</span></span> <span data-ttu-id="03eaf-270">在`loadBootResource`下列情況下使用：</span><span class="sxs-lookup"><span data-stu-id="03eaf-270">Use `loadBootResource` for the following scenarios:</span></span>

* <span data-ttu-id="03eaf-271">允許使用者從 CDN 載入靜態資源，例如時區資料或*dotnet wasm。*</span><span class="sxs-lookup"><span data-stu-id="03eaf-271">Allow users to load static resources, such as timezone data or *dotnet.wasm* from a CDN.</span></span>
* <span data-ttu-id="03eaf-272">使用 HTTP 要求載入壓縮的元件，並將它們解壓縮到不支援從伺服器提取壓縮內容的主機用戶端上。</span><span class="sxs-lookup"><span data-stu-id="03eaf-272">Load compressed assemblies using an HTTP request and decompress them on the client for hosts that don't support fetching compressed contents from the server.</span></span>
* <span data-ttu-id="03eaf-273">將每個`fetch`要求重新導向至新名稱，以將資源別名設為不同名稱。</span><span class="sxs-lookup"><span data-stu-id="03eaf-273">Alias resources to a different name by redirecting each `fetch` request to a new name.</span></span>

<span data-ttu-id="03eaf-274">`loadBootResource`參數會出現在下表中。</span><span class="sxs-lookup"><span data-stu-id="03eaf-274">`loadBootResource` parameters appear in the following table.</span></span>

| <span data-ttu-id="03eaf-275">參數</span><span class="sxs-lookup"><span data-stu-id="03eaf-275">Parameter</span></span>    | <span data-ttu-id="03eaf-276">描述</span><span class="sxs-lookup"><span data-stu-id="03eaf-276">Description</span></span> |
| ------------ | ----------- |
| `type`       | <span data-ttu-id="03eaf-277">資源類型。</span><span class="sxs-lookup"><span data-stu-id="03eaf-277">The type of the resource.</span></span> <span data-ttu-id="03eaf-278">運算子類型： `assembly`、 `pdb`、 `dotnetjs`、 `dotnetwasm`、`timezonedata`</span><span class="sxs-lookup"><span data-stu-id="03eaf-278">Permissable types: `assembly`, `pdb`, `dotnetjs`, `dotnetwasm`, `timezonedata`</span></span> |
| `name`       | <span data-ttu-id="03eaf-279">資源名稱。</span><span class="sxs-lookup"><span data-stu-id="03eaf-279">The name of the resource.</span></span> |
| `defaultUri` | <span data-ttu-id="03eaf-280">資源的相對或絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="03eaf-280">The relative or absolute URI of the resource.</span></span> |
| `integrity`  | <span data-ttu-id="03eaf-281">代表回應中預期內容的完整性字串。</span><span class="sxs-lookup"><span data-stu-id="03eaf-281">The integrity string representing the expected content in the response.</span></span> |

<span data-ttu-id="03eaf-282">`loadBootResource`傳回下列任何一項，以覆寫載入進程：</span><span class="sxs-lookup"><span data-stu-id="03eaf-282">`loadBootResource` returns any of the following to override the loading process:</span></span>

* <span data-ttu-id="03eaf-283">URI 字串。</span><span class="sxs-lookup"><span data-stu-id="03eaf-283">URI string.</span></span> <span data-ttu-id="03eaf-284">在下列範例（*wwwroot/index.html*）中，從 CDN 提供下列檔案`https://my-awesome-cdn.com/`：</span><span class="sxs-lookup"><span data-stu-id="03eaf-284">In the following example (*wwwroot/index.html*), the following files are served from a CDN at `https://my-awesome-cdn.com/`:</span></span>

  * <span data-ttu-id="03eaf-285">*dotnet.\*.js*</span><span class="sxs-lookup"><span data-stu-id="03eaf-285">*dotnet.\*.js*</span></span>
  * <span data-ttu-id="03eaf-286">*dotnet. wasm*</span><span class="sxs-lookup"><span data-stu-id="03eaf-286">*dotnet.wasm*</span></span>
  * <span data-ttu-id="03eaf-287">時區資料</span><span class="sxs-lookup"><span data-stu-id="03eaf-287">Timezone data</span></span>

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

* <span data-ttu-id="03eaf-288">`Promise<Response>`.</span><span class="sxs-lookup"><span data-stu-id="03eaf-288">`Promise<Response>`.</span></span> <span data-ttu-id="03eaf-289">在標`integrity`頭中傳遞參數，以保留預設的完整性檢查行為。</span><span class="sxs-lookup"><span data-stu-id="03eaf-289">Pass the `integrity` parameter in a header to retain the default integrity-checking behavior.</span></span>

  <span data-ttu-id="03eaf-290">下列範例（*wwwroot/index.html*）會將自訂 HTTP 標頭新增至輸出要求，並`integrity`將參數傳遞至`fetch`呼叫：</span><span class="sxs-lookup"><span data-stu-id="03eaf-290">The following example (*wwwroot/index.html*) adds a custom HTTP header to the outbound requests and passes the `integrity` parameter through to the `fetch` call:</span></span>
  
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

* <span data-ttu-id="03eaf-291">`null`/`undefined`，這會產生預設的載入行為。</span><span class="sxs-lookup"><span data-stu-id="03eaf-291">`null`/`undefined`, which results in the default loading behavior.</span></span>

<span data-ttu-id="03eaf-292">外部來源必須傳回瀏覽器所需的 CORS 標頭，以允許跨原始來源資源載入。</span><span class="sxs-lookup"><span data-stu-id="03eaf-292">External sources must return the required CORS headers for browsers to allow the cross-origin resource loading.</span></span> <span data-ttu-id="03eaf-293">根據預設，Cdn 通常會提供必要的標頭。</span><span class="sxs-lookup"><span data-stu-id="03eaf-293">CDNs usually provide the required headers by default.</span></span>

<span data-ttu-id="03eaf-294">您只需要指定自訂行為的類型。</span><span class="sxs-lookup"><span data-stu-id="03eaf-294">You only need to specify types for custom behaviors.</span></span> <span data-ttu-id="03eaf-295">根據預設載入行為`loadBootResource` ，架構會載入未指定的類型。</span><span class="sxs-lookup"><span data-stu-id="03eaf-295">Types not specified to `loadBootResource` are loaded by the framework per their default loading behaviors.</span></span>

## <a name="change-the-filename-extension-of-dll-files"></a><span data-ttu-id="03eaf-296">變更 DLL 檔案的副檔名</span><span class="sxs-lookup"><span data-stu-id="03eaf-296">Change the filename extension of DLL files</span></span>

<span data-ttu-id="03eaf-297">如果您需要變更應用程式已發佈 *.dll*檔案的副檔名，請遵循本節中的指導方針。</span><span class="sxs-lookup"><span data-stu-id="03eaf-297">In case you have a need to change the filename extensions of the app's published *.dll* files, follow the guidance in this section.</span></span>

<span data-ttu-id="03eaf-298">發行應用程式之後，請使用 shell 腳本或 DevOps 組建管線，將 *.dll*檔案重新命名為使用不同的副檔名。</span><span class="sxs-lookup"><span data-stu-id="03eaf-298">After publishing the app, use a shell script or DevOps build pipeline to rename *.dll* files to use a different file extension.</span></span> <span data-ttu-id="03eaf-299">以應用程式已發佈輸出的*wwwroot*目錄中的 *.dll*檔案為目標（例如 *{CONTENT ROOT}/bin/Release/netstandard2.1/publish/wwwroot*）。</span><span class="sxs-lookup"><span data-stu-id="03eaf-299">Target the *.dll* files in the *wwwroot* directory of the app's published output (for example, *{CONTENT ROOT}/bin/Release/netstandard2.1/publish/wwwroot*).</span></span>

<span data-ttu-id="03eaf-300">在下列範例中， *.dll*檔案已重新命名為使用*bin*副檔名。</span><span class="sxs-lookup"><span data-stu-id="03eaf-300">In the following examples, *.dll* files are renamed to use the *.bin* file extension.</span></span>

<span data-ttu-id="03eaf-301">在 Windows 上：</span><span class="sxs-lookup"><span data-stu-id="03eaf-301">On Windows:</span></span>

```powershell
dir .\_framework\_bin | rename-item -NewName { $_.name -replace ".dll\b",".bin" }
((Get-Content .\_framework\blazor.boot.json -Raw) -replace '.dll"','.bin"') | Set-Content .\_framework\blazor.boot.json
```

<span data-ttu-id="03eaf-302">在 Linux 或 macOS 上：</span><span class="sxs-lookup"><span data-stu-id="03eaf-302">On Linux or macOS:</span></span>

```console
for f in _framework/_bin/*; do mv "$f" "`echo $f | sed -e 's/\.dll\b/.bin/g'`"; done
sed -i 's/\.dll"/.bin"/g' _framework/blazor.boot.json
```
   
<span data-ttu-id="03eaf-303">若要使用與*bin*不同的副檔名，請在上述命令中取代 *. bin* 。</span><span class="sxs-lookup"><span data-stu-id="03eaf-303">To use a different file extension than *.bin*, replace *.bin* in the preceding commands.</span></span>

<span data-ttu-id="03eaf-304">若要解決壓縮的*blazor gz*和*blazor.boot.json.br*檔案，請採用下列其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="03eaf-304">To address the compressed *blazor.boot.json.gz* and *blazor.boot.json.br* files, adopt either of the following approaches:</span></span>

* <span data-ttu-id="03eaf-305">移除壓縮的*blazor gz*和*blazor.boot.json.br*檔案。</span><span class="sxs-lookup"><span data-stu-id="03eaf-305">Remove the compressed *blazor.boot.json.gz* and *blazor.boot.json.br* files.</span></span> <span data-ttu-id="03eaf-306">此方法會停用壓縮。</span><span class="sxs-lookup"><span data-stu-id="03eaf-306">Compression is disabled with this approach.</span></span>
* <span data-ttu-id="03eaf-307">重新壓縮已更新的*blazor*檔案。</span><span class="sxs-lookup"><span data-stu-id="03eaf-307">Recompress the updated *blazor.boot.json* file.</span></span>

<span data-ttu-id="03eaf-308">下列 Windows 範例會使用放在專案根目錄中的 PowerShell 腳本。</span><span class="sxs-lookup"><span data-stu-id="03eaf-308">The following Windows example uses a PowerShell script placed at the root of the project.</span></span>

<span data-ttu-id="03eaf-309">*ChangeDLLExtensions. ps1：*：</span><span class="sxs-lookup"><span data-stu-id="03eaf-309">*ChangeDLLExtensions.ps1:*:</span></span>

```powershell
param([string]$filepath,[string]$tfm)
dir $filepath\bin\Release\$tfm\wwwroot\_framework\_bin | rename-item -NewName { $_.name -replace ".dll\b",".bin" }
((Get-Content $filepath\bin\Release\$tfm\wwwroot\_framework\blazor.boot.json -Raw) -replace '.dll"','.bin"') | Set-Content $filepath\bin\Release\$tfm\wwwroot\_framework\blazor.boot.json
Remove-Item $filepath\bin\Release\$tfm\wwwroot\_framework\blazor.boot.json.gz
```

<span data-ttu-id="03eaf-310">在專案檔中，腳本會在發行應用程式之後執行：</span><span class="sxs-lookup"><span data-stu-id="03eaf-310">In the project file, the script is run after publishing the app:</span></span>

```xml
<Target Name="ChangeDLLFileExtensions" AfterTargets="Publish" Condition="'$(Configuration)'=='Release'">
  <Exec Command="powershell.exe -command &quot;&amp; { .\ChangeDLLExtensions.ps1 '$(SolutionDir)' '$(TargetFramework)'}&quot;" />
</Target>
```

<span data-ttu-id="03eaf-311">若要提供意見反應，請造訪[aspnetcore/問題 #5477](https://github.com/dotnet/aspnetcore/issues/5477)。</span><span class="sxs-lookup"><span data-stu-id="03eaf-311">To provide feedback, visit [aspnetcore/issues #5477](https://github.com/dotnet/aspnetcore/issues/5477).</span></span>
