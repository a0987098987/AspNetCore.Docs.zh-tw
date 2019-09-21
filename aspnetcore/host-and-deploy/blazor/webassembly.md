---
title: 裝載和部署 ASP.NET Core Blazor WebAssembly
author: guardrex
description: 了解如何使用 ASP.NET Core、內容傳遞網路 (CDN)、檔案伺服器和 GitHub Pages 來裝載和部署 Blazor 應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: host-and-deploy/blazor/webassembly
ms.openlocfilehash: b7de145ae9924dd405e67f60fc93d81549834164
ms.sourcegitcommit: e5a74f882c14eaa0e5639ff082355e130559ba83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/20/2019
ms.locfileid: "71168226"
---
# <a name="host-and-deploy-aspnet-core-blazor-webassembly"></a><span data-ttu-id="6119f-103">裝載和部署 ASP.NET Core Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="6119f-103">Host and deploy ASP.NET Core Blazor WebAssembly</span></span>

<span data-ttu-id="6119f-104">作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="6119f-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="6119f-105">使用[Blazor WebAssembly 裝載模型](xref:blazor/hosting-models#blazor-webassembly)：</span><span class="sxs-lookup"><span data-stu-id="6119f-105">With the [Blazor WebAssembly hosting model](xref:blazor/hosting-models#blazor-webassembly):</span></span>

* <span data-ttu-id="6119f-106">Blazor 應用程式、其相依性，和 .NET 執行階段會下載至瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="6119f-106">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="6119f-107">應用程式會直接在瀏覽器 UI 執行緒上執行。</span><span class="sxs-lookup"><span data-stu-id="6119f-107">The app is executed directly on the browser UI thread.</span></span>

<span data-ttu-id="6119f-108">支援下列部署策略：</span><span class="sxs-lookup"><span data-stu-id="6119f-108">The following deployment strategies are supported:</span></span>

* <span data-ttu-id="6119f-109">Blazor 應用程式由 ASP.NET Core 應用程式提供服務。</span><span class="sxs-lookup"><span data-stu-id="6119f-109">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="6119f-110">此策略已於[搭配 ASP.NET Core 的已裝載部署](#hosted-deployment-with-aspnet-core)一節中涵蓋。</span><span class="sxs-lookup"><span data-stu-id="6119f-110">This strategy is covered in the [Hosted deployment with ASP.NET Core](#hosted-deployment-with-aspnet-core) section.</span></span>
* <span data-ttu-id="6119f-111">Blazor 應用程式放在靜態裝載的網頁伺服器或服務上，其中 .NET 不會用來提供服務給 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="6119f-111">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="6119f-112">此策略涵蓋于[獨立部署](#standalone-deployment)一節，其中包含將 Blazor WebAssembly 應用程式裝載為 IIS 子應用程式的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="6119f-112">This strategy is covered in the [Standalone deployment](#standalone-deployment) section, which includes information on hosting a Blazor WebAssembly app as an IIS sub-app.</span></span>

## <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="6119f-113">重寫 URL 以便正確地路由</span><span class="sxs-lookup"><span data-stu-id="6119f-113">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="6119f-114">Blazor WebAssembly 應用程式中頁面元件的路由要求，並不像是 Blazor 伺服器（裝載的應用程式）中的路由要求一樣簡單。</span><span class="sxs-lookup"><span data-stu-id="6119f-114">Routing requests for page components in a Blazor WebAssembly app isn't as straightforward as routing requests in a Blazor Server, hosted app.</span></span> <span data-ttu-id="6119f-115">假設有兩個元件的 Blazor WebAssembly 應用程式：</span><span class="sxs-lookup"><span data-stu-id="6119f-115">Consider a Blazor WebAssembly app with two components:</span></span>

* <span data-ttu-id="6119f-116">*Main.razor* &ndash; 載入應用程式根目錄，同時包含 `About` 元件 (`href="About"`) 的連結。</span><span class="sxs-lookup"><span data-stu-id="6119f-116">*Main.razor* &ndash; Loads at the root of the app and contains a link to the `About` component (`href="About"`).</span></span>
* <span data-ttu-id="6119f-117">*About.razor* &ndash; `About` 元件。</span><span class="sxs-lookup"><span data-stu-id="6119f-117">*About.razor* &ndash; `About` component.</span></span>

<span data-ttu-id="6119f-118">使用瀏覽器的網址列要求應用程式預設文件時 (例如 `https://www.contoso.com/`)：</span><span class="sxs-lookup"><span data-stu-id="6119f-118">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="6119f-119">瀏覽器提出要求。</span><span class="sxs-lookup"><span data-stu-id="6119f-119">The browser makes a request.</span></span>
1. <span data-ttu-id="6119f-120">傳回預設頁面，這通常是 *index.html*。</span><span class="sxs-lookup"><span data-stu-id="6119f-120">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="6119f-121">*index.html* 啟動載入應用程式。</span><span class="sxs-lookup"><span data-stu-id="6119f-121">*index.html* bootstraps the app.</span></span>
1. <span data-ttu-id="6119f-122">Blazor 的路由器會載入，且會轉譯 Razor `Main` 元件。</span><span class="sxs-lookup"><span data-stu-id="6119f-122">Blazor's router loads, and the Razor `Main` component is rendered.</span></span>

<span data-ttu-id="6119f-123">在主頁面中，選取用戶端 `About` 元件的連結有用，因為 Blazor 路由器會停止瀏覽器，使其不再從網際網路對 `www.contoso.com` 提出針對 `About` 的要求，並自行提供轉譯的 `About` 元件。</span><span class="sxs-lookup"><span data-stu-id="6119f-123">In the Main page, selecting the link to the `About` component works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the rendered `About` component itself.</span></span> <span data-ttu-id="6119f-124">*Blazor WebAssembly 應用程式內*內部端點的所有要求都以相同的方式工作：要求不會對於網際網路上伺服器裝載的資源，觸發以瀏覽器為基礎的要求。</span><span class="sxs-lookup"><span data-stu-id="6119f-124">All of the requests for internal endpoints *within the Blazor WebAssembly app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="6119f-125">路由器會在內部處理要求。</span><span class="sxs-lookup"><span data-stu-id="6119f-125">The router handles the requests internally.</span></span>

<span data-ttu-id="6119f-126">如果使用瀏覽器之網址列提出對 `www.contoso.com/About` 的要求，則要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="6119f-126">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="6119f-127">在應用程式的網際網路主機上沒有這類資源存在，因此會傳回「404 - 找不到」的回應。</span><span class="sxs-lookup"><span data-stu-id="6119f-127">No such resource exists on the app's Internet host, so a *404 - Not Found* response is returned.</span></span>

<span data-ttu-id="6119f-128">因為瀏覽器會提出要求給以網際網路為基礎的主機以取得用戶端頁面，所以網頁伺服器和裝載服務必須針對實際上不在伺服器上資源，將所有要求重新撰寫至 *index.html* 頁面。</span><span class="sxs-lookup"><span data-stu-id="6119f-128">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="6119f-129">當傳回*index. html*時，應用程式的 Blazor 路由器會接管並回應正確的資源。</span><span class="sxs-lookup"><span data-stu-id="6119f-129">When *index.html* is returned, the app's Blazor router takes over and responds with the correct resource.</span></span>

<span data-ttu-id="6119f-130">部署到 IIS 伺服器時，您可以使用 URL 重寫模組搭配應用*程式已發佈的 web.config 檔案*。</span><span class="sxs-lookup"><span data-stu-id="6119f-130">When deploying to an IIS server, you can use the URL Rewrite Module with the app's published *web.config* file.</span></span> <span data-ttu-id="6119f-131">如需詳細資訊，請參閱[IIS](#iis)一節。</span><span class="sxs-lookup"><span data-stu-id="6119f-131">For more information, see the [IIS](#iis) section.</span></span>

## <a name="hosted-deployment-with-aspnet-core"></a><span data-ttu-id="6119f-132">搭配 ASP.NET Core 的已裝載部署</span><span class="sxs-lookup"><span data-stu-id="6119f-132">Hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="6119f-133">*託管部署*可從 web 伺服器上執行的[ASP.NET Core 應用程式](xref:index)，將 Blazor WebAssembly 應用程式提供給瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="6119f-133">A *hosted deployment* serves the Blazor WebAssembly app to browsers from an [ASP.NET Core app](xref:index) that runs on a web server.</span></span>

<span data-ttu-id="6119f-134">Blazor 應用程式隨附於發佈輸出中的 ASP.NET Core 應用程式，以便兩個應用程式會一起部署。</span><span class="sxs-lookup"><span data-stu-id="6119f-134">The Blazor app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="6119f-135">需要有能夠裝載 ASP.NET Core 應用程式的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="6119f-135">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="6119f-136">針對裝載部署，Visual Studio 包含 **Blazor WebAssembly 應用程式**專案範本 (使用 [dotnet new](/dotnet/core/tools/dotnet-new) 命令時的 `blazorwasm` 範本)，並已選取 [已裝載] 選項。</span><span class="sxs-lookup"><span data-stu-id="6119f-136">For a hosted deployment, Visual Studio includes the **Blazor WebAssembly App** project template (`blazorwasm` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command) with the **Hosted** option selected.</span></span>

<span data-ttu-id="6119f-137">如需 ASP.NET Core 應用程式裝載和部署的詳細資訊，請參閱 <xref:host-and-deploy/index>。</span><span class="sxs-lookup"><span data-stu-id="6119f-137">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="6119f-138">如需部署至 Azure App Service 的相關資訊，請參閱 <xref:tutorials/publish-to-azure-webapp-using-vs>。</span><span class="sxs-lookup"><span data-stu-id="6119f-138">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>

## <a name="standalone-deployment"></a><span data-ttu-id="6119f-139">獨立部署</span><span class="sxs-lookup"><span data-stu-id="6119f-139">Standalone deployment</span></span>

<span data-ttu-id="6119f-140">*獨立部署*可將 Blazor WebAssembly 應用程式當做一組直接由用戶端要求的靜態檔案來提供。</span><span class="sxs-lookup"><span data-stu-id="6119f-140">A *standalone deployment* serves the Blazor WebAssembly app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="6119f-141">所有靜態檔案伺服器都能夠支援 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="6119f-141">Any static file server is able to serve the Blazor app.</span></span>

<span data-ttu-id="6119f-142">獨立式部署資產會發佈至 [bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="6119f-142">Standalone deployment assets are published to the *bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist* folder.</span></span>

### <a name="iis"></a><span data-ttu-id="6119f-143">IIS</span><span class="sxs-lookup"><span data-stu-id="6119f-143">IIS</span></span>

<span data-ttu-id="6119f-144">IIS 是足以支援 Blazor 應用程式的靜態檔案伺服器。</span><span class="sxs-lookup"><span data-stu-id="6119f-144">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="6119f-145">若要設定 IIS 來裝載 Blazor，請參閱[在 IIS 上建置靜態網站](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="6119f-145">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="6119f-146">已發行的資產會建立在 */bin/Release/{TARGET FRAMEWORK}/publish* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="6119f-146">Published assets are created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="6119f-147">在網頁伺服器或裝載服務上，裝載 *publish* 資料夾的內容。</span><span class="sxs-lookup"><span data-stu-id="6119f-147">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

#### <a name="webconfig"></a><span data-ttu-id="6119f-148">web.config</span><span class="sxs-lookup"><span data-stu-id="6119f-148">web.config</span></span>

<span data-ttu-id="6119f-149">發佈 Blazor 專案時，會建立 *web.config* 檔案，並使用下列 IIS 組態：</span><span class="sxs-lookup"><span data-stu-id="6119f-149">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="6119f-150">針對下列副檔名設定 MIME 類型：</span><span class="sxs-lookup"><span data-stu-id="6119f-150">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="6119f-151">*.dll* &ndash; `application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="6119f-151">*.dll* &ndash; `application/octet-stream`</span></span>
  * <span data-ttu-id="6119f-152">*.json* &ndash; `application/json`</span><span class="sxs-lookup"><span data-stu-id="6119f-152">*.json* &ndash; `application/json`</span></span>
  * <span data-ttu-id="6119f-153">*.wasm* &ndash; `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="6119f-153">*.wasm* &ndash; `application/wasm`</span></span>
  * <span data-ttu-id="6119f-154">*.woff* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="6119f-154">*.woff* &ndash; `application/font-woff`</span></span>
  * <span data-ttu-id="6119f-155">*.woff2* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="6119f-155">*.woff2* &ndash; `application/font-woff`</span></span>
* <span data-ttu-id="6119f-156">針對下列 MIME 類型會啟用 HTTP 壓縮：</span><span class="sxs-lookup"><span data-stu-id="6119f-156">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="6119f-157">建立 URL Rewrite Module 規則：</span><span class="sxs-lookup"><span data-stu-id="6119f-157">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="6119f-158">提供應用程式靜態資產所在的子目錄 ( *{組件名稱}/dist/{要求的路徑}* )。</span><span class="sxs-lookup"><span data-stu-id="6119f-158">Serve the sub-directory where the app's static assets reside (*{ASSEMBLY NAME}/dist/{PATH REQUESTED}*).</span></span>
  * <span data-ttu-id="6119f-159">建立 SPA 後援路由，讓非檔案資產要求重新導向至其靜態資產資料夾中的應用程式預設文件 ( *{組件名稱}/dist/index.html*)。</span><span class="sxs-lookup"><span data-stu-id="6119f-159">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*{ASSEMBLY NAME}/dist/index.html*).</span></span>

#### <a name="install-the-url-rewrite-module"></a><span data-ttu-id="6119f-160">安裝 URL Rewrite 模組</span><span class="sxs-lookup"><span data-stu-id="6119f-160">Install the URL Rewrite Module</span></span>

<span data-ttu-id="6119f-161">需要 [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite)，才可重寫 URL。</span><span class="sxs-lookup"><span data-stu-id="6119f-161">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="6119f-162">預設不會安裝此模組，且其無法用來安裝為網頁伺服器 (IIS) 角色服務功能。</span><span class="sxs-lookup"><span data-stu-id="6119f-162">The module isn't installed by default, and it isn't available for install as a Web Server (IIS) role service feature.</span></span> <span data-ttu-id="6119f-163">必須從 IIS 網站下載模組。</span><span class="sxs-lookup"><span data-stu-id="6119f-163">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="6119f-164">請使用 Web Platform Installer 安裝模組：</span><span class="sxs-lookup"><span data-stu-id="6119f-164">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="6119f-165">在本機上，巡覽至 [URL Rewrite Module 下載頁面](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads)。</span><span class="sxs-lookup"><span data-stu-id="6119f-165">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="6119f-166">如需英文版，請選取 [WebPI] 下載 WebPI 安裝程式。</span><span class="sxs-lookup"><span data-stu-id="6119f-166">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="6119f-167">如需其他語言，請選取適當的伺服器架構 (x86 x64) 來下載安裝程式。</span><span class="sxs-lookup"><span data-stu-id="6119f-167">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="6119f-168">將安裝程式複製到伺服器。</span><span class="sxs-lookup"><span data-stu-id="6119f-168">Copy the installer to the server.</span></span> <span data-ttu-id="6119f-169">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="6119f-169">Run the installer.</span></span> <span data-ttu-id="6119f-170">選取 [安裝] 按鈕，並接受授權條款。</span><span class="sxs-lookup"><span data-stu-id="6119f-170">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="6119f-171">安裝完成之後，不需要重新啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="6119f-171">A server restart isn't required after the install completes.</span></span>

#### <a name="configure-the-website"></a><span data-ttu-id="6119f-172">設定網站</span><span class="sxs-lookup"><span data-stu-id="6119f-172">Configure the website</span></span>

<span data-ttu-id="6119f-173">將網站的 [實體路徑] 設為應用程式的資料夾。</span><span class="sxs-lookup"><span data-stu-id="6119f-173">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="6119f-174">此資料夾包含：</span><span class="sxs-lookup"><span data-stu-id="6119f-174">The folder contains:</span></span>

* <span data-ttu-id="6119f-175">IIS 用來設定網站的 *web.config* 檔，包括必要的重新導向規則和檔案內容類型。</span><span class="sxs-lookup"><span data-stu-id="6119f-175">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="6119f-176">應用程式的靜態資產資料夾。</span><span class="sxs-lookup"><span data-stu-id="6119f-176">The app's static asset folder.</span></span>

#### <a name="host-as-an-iis-sub-app"></a><span data-ttu-id="6119f-177">以 IIS 子應用程式的形式裝載</span><span class="sxs-lookup"><span data-stu-id="6119f-177">Host as an IIS sub-app</span></span>

<span data-ttu-id="6119f-178">如果獨立應用程式裝載為 IIS 子應用程式，請執行下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="6119f-178">If a standalone app is hosted as an IIS sub-app, perform either of the following:</span></span>

* <span data-ttu-id="6119f-179">停用繼承的 ASP.NET Core 模組處理常式。</span><span class="sxs-lookup"><span data-stu-id="6119f-179">Disable the inherited ASP.NET Core Module handler.</span></span>

  <span data-ttu-id="6119f-180">將`<handlers>`區段新增至檔案，以移除 Blazor 應用*程式已發佈的 web.config 檔案*中的處理常式：</span><span class="sxs-lookup"><span data-stu-id="6119f-180">Remove the handler in the Blazor app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>

  ```xml
  <handlers>
    <remove name="aspNetCore" />
  </handlers>
  ```

* <span data-ttu-id="6119f-181">`<system.webServer>` `inheritInChildApplications`使用設定為的`false`元素，停用根（父系）應用程式區段`<location>`的繼承：</span><span class="sxs-lookup"><span data-stu-id="6119f-181">Disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>

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

<span data-ttu-id="6119f-182">除了設定[應用程式的基底路徑](xref:host-and-deploy/blazor/index#app-base-path)之外，也會執行移除處理常式或停用繼承。</span><span class="sxs-lookup"><span data-stu-id="6119f-182">Removing the handler or disabling inheritance is performed in addition to [configuring the app's base path](xref:host-and-deploy/blazor/index#app-base-path).</span></span> <span data-ttu-id="6119f-183">在應用程式的 *index.html* 檔案，將應用程式基底路徑設為在 IIS 中設定子應用程式時使用的 IIS 別名。</span><span class="sxs-lookup"><span data-stu-id="6119f-183">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

#### <a name="troubleshooting"></a><span data-ttu-id="6119f-184">疑難排解</span><span class="sxs-lookup"><span data-stu-id="6119f-184">Troubleshooting</span></span>

<span data-ttu-id="6119f-185">如果收到「500 - 內部伺服器錯誤」，且 IIS 管理員在嘗試存取網站設定時擲回錯誤，請確認是否已安裝 URL Rewrite 模組。</span><span class="sxs-lookup"><span data-stu-id="6119f-185">If a *500 - Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="6119f-186">未安裝此模組時，IIS 無法剖析 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="6119f-186">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="6119f-187">這導致 IIS 管理員無法載入網站的組態，且網站無法提供 Blazor 的靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="6119f-187">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="6119f-188">如需針對部署至 IIS 進行疑難排解的詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="6119f-188">For more information on troubleshooting deployments to IIS, see <xref:test/troubleshoot-azure-iis>.</span></span>

### <a name="azure-storage"></a><span data-ttu-id="6119f-189">Azure 儲存體</span><span class="sxs-lookup"><span data-stu-id="6119f-189">Azure Storage</span></span>

<span data-ttu-id="6119f-190">[Azure 儲存體](/azure/storage/)靜態檔案裝載允許裝載無伺服器 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="6119f-190">[Azure Storage](/azure/storage/) static file hosting allows serverless Blazor app hosting.</span></span> <span data-ttu-id="6119f-191">支援自訂網域名稱、Azure 內容傳遞網路 (CDN) 及 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="6119f-191">Custom domain names, the Azure Content Delivery Network (CDN), and HTTPS are supported.</span></span>

<span data-ttu-id="6119f-192">當 Blob 服務針對儲存體帳戶上的靜態網站裝載啟用時：</span><span class="sxs-lookup"><span data-stu-id="6119f-192">When the blob service is enabled for static website hosting on a storage account:</span></span>

* <span data-ttu-id="6119f-193">將 [索引文件名稱] 設定為 `index.html`。</span><span class="sxs-lookup"><span data-stu-id="6119f-193">Set the **Index document name** to `index.html`.</span></span>
* <span data-ttu-id="6119f-194">將 [錯誤文件路徑] 設定為 `index.html`。</span><span class="sxs-lookup"><span data-stu-id="6119f-194">Set the **Error document path** to `index.html`.</span></span> <span data-ttu-id="6119f-195">Razor 元件和其他非檔案端點不會位於由 Blob 服務所存放之靜態內容中的實體路徑上。</span><span class="sxs-lookup"><span data-stu-id="6119f-195">Razor components and other non-file endpoints don't reside at physical paths in the static content stored by the blob service.</span></span> <span data-ttu-id="6119f-196">當系統接收到針對這些資源之一，且應由 Blazor 路由器處理的要求時，由 Blob 服務所產生的「404 - 找不到」錯誤會將要求路由至**錯誤文件路徑**。</span><span class="sxs-lookup"><span data-stu-id="6119f-196">When a request for one of these resources is received that the Blazor router should handle, the *404 - Not Found* error generated by the blob service routes the request to the **Error document path**.</span></span> <span data-ttu-id="6119f-197">系統會傳回 *index.html* Blob，且 Blazor 路由器會載入並處理該路徑。</span><span class="sxs-lookup"><span data-stu-id="6119f-197">The *index.html* blob is returned, and the Blazor router loads and processes the path.</span></span>

<span data-ttu-id="6119f-198">如需詳細資訊，請參閱[Azure 儲存體中的靜態網站代管](/azure/storage/blobs/storage-blob-static-website)。</span><span class="sxs-lookup"><span data-stu-id="6119f-198">For more information, see [Static website hosting in Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span></span>

### <a name="nginx"></a><span data-ttu-id="6119f-199">Nginx</span><span class="sxs-lookup"><span data-stu-id="6119f-199">Nginx</span></span>

<span data-ttu-id="6119f-200">下列 *nginx.conf* 檔案已簡化，示範如何將 Nginx 設定為每當找不到磁碟上的對應檔案時，便傳送 *Index.html* 檔案。</span><span class="sxs-lookup"><span data-stu-id="6119f-200">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *index.html* file whenever it can't find a corresponding file on disk.</span></span>

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

<span data-ttu-id="6119f-201">如需生產環境 Nginx 網頁伺服器組態的詳細資訊，請參閱[建立 NGINX Plus 和 NGINX 組態檔](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/)。</span><span class="sxs-lookup"><span data-stu-id="6119f-201">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

### <a name="nginx-in-docker"></a><span data-ttu-id="6119f-202">Docker 中的 Nginx</span><span class="sxs-lookup"><span data-stu-id="6119f-202">Nginx in Docker</span></span>

<span data-ttu-id="6119f-203">若要在 Docker 中使用 Nginx 裝載 Blazor，請設定 Dockerfile 以使用以 Alpine 為基礎的 Nginx 映像。</span><span class="sxs-lookup"><span data-stu-id="6119f-203">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="6119f-204">更新 Dockerfile，將 *nginx.config* 檔案複製到容器內。</span><span class="sxs-lookup"><span data-stu-id="6119f-204">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="6119f-205">如下列範例所示，新增一行至 Dockerfile：</span><span class="sxs-lookup"><span data-stu-id="6119f-205">Add one line to the Dockerfile, as shown in the following example:</span></span>

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="github-pages"></a><span data-ttu-id="6119f-206">GitHub Pages</span><span class="sxs-lookup"><span data-stu-id="6119f-206">GitHub Pages</span></span>

<span data-ttu-id="6119f-207">若要處理 URL 重寫，請新增 *404.html* 檔，以及處理將要求重新導向至 *index.html* 頁面的指令碼。</span><span class="sxs-lookup"><span data-stu-id="6119f-207">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="6119f-208">如需社群所提供的範例實作，請參閱 [GitHub Pages 的單一頁面應用程式](https://spa-github-pages.rafrex.com/) (GitHub 上的 [rafrex/spa-github-pages](https://github.com/rafrex/spa-github-pages#readme))。</span><span class="sxs-lookup"><span data-stu-id="6119f-208">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](https://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="6119f-209">使用社群方法的範例可在 [GitHub 上的 blazor-demo/blazor-demo.github.io](https://github.com/blazor-demo/blazor-demo.github.io) ([即時網站](https://blazor-demo.github.io/)) 看到。</span><span class="sxs-lookup"><span data-stu-id="6119f-209">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="6119f-210">使用專案網站，而非組織網站時，請在 *index.html* 中新增或更新 `<base>` 標籤。</span><span class="sxs-lookup"><span data-stu-id="6119f-210">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="6119f-211">將 `href` 屬性值設定為 GitHub 存放庫名稱，並在尾端加上斜線 (例如 `my-repository/`)。</span><span class="sxs-lookup"><span data-stu-id="6119f-211">Set the `href` attribute value to the GitHub repository name with a trailing slash (for example, `my-repository/`.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="6119f-212">主機組態值</span><span class="sxs-lookup"><span data-stu-id="6119f-212">Host configuration values</span></span>

<span data-ttu-id="6119f-213">[Blazor WebAssembly apps](xref:blazor/hosting-models#blazor-webassembly)可以在開發環境中的執行時間，接受下列主機設定值做為命令列引數。</span><span class="sxs-lookup"><span data-stu-id="6119f-213">[Blazor WebAssembly apps](xref:blazor/hosting-models#blazor-webassembly) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="6119f-214">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="6119f-214">Content Root</span></span>

<span data-ttu-id="6119f-215">`--contentroot` 引數設定包含應用程式內容檔案的目錄絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="6119f-215">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files.</span></span> <span data-ttu-id="6119f-216">在下列範例中，`/content-root-path` 是應用程式的內容根路徑。</span><span class="sxs-lookup"><span data-stu-id="6119f-216">In the following examples, `/content-root-path` is the app's content root path.</span></span>

* <span data-ttu-id="6119f-217">在命令提示字元，於本機執行應用程式時傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="6119f-217">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="6119f-218">從應用程式目錄，執行：</span><span class="sxs-lookup"><span data-stu-id="6119f-218">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --contentroot=/content-root-path
  ```

* <span data-ttu-id="6119f-219">新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="6119f-219">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="6119f-220">此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。</span><span class="sxs-lookup"><span data-stu-id="6119f-220">This setting is used when the app is run with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* <span data-ttu-id="6119f-221">在 Visual Studio 中，在 [屬性] > [偵錯] > [應用程式引數] 中指定引數。</span><span class="sxs-lookup"><span data-stu-id="6119f-221">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="6119f-222">在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="6119f-222">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a><span data-ttu-id="6119f-223">路徑基底</span><span class="sxs-lookup"><span data-stu-id="6119f-223">Path base</span></span>

<span data-ttu-id="6119f-224">引數`href`會設定應用程式以非根相對 URL 路徑在本機執行的應用程式基底路徑（ `<base>`此標籤會設定為預備和`/`生產以外的路徑）。 `--pathbase`</span><span class="sxs-lookup"><span data-stu-id="6119f-224">The `--pathbase` argument sets the app base path for an app run locally with a non-root relative URL path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="6119f-225">在下列範例中，`/relative-URL-path` 是應用程式的路徑基底。</span><span class="sxs-lookup"><span data-stu-id="6119f-225">In the following examples, `/relative-URL-path` is the app's path base.</span></span> <span data-ttu-id="6119f-226">如需詳細資訊，請參閱[應用程式基底路徑](xref:host-and-deploy/blazor/index#app-base-path)。</span><span class="sxs-lookup"><span data-stu-id="6119f-226">For more information, see [App base path](xref:host-and-deploy/blazor/index#app-base-path).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6119f-227">不同於提供給 `<base>` 標籤 `href` 的路徑，在傳遞 `--pathbase` 引數值時請勿包含尾端的斜線 (`/`)。</span><span class="sxs-lookup"><span data-stu-id="6119f-227">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="6119f-228">如果應用程式基底路徑提供於 `<base>` 標籤作為 `<base href="/CoolApp/">` (包括尾端的斜線)，請將命令列引數值傳遞為 `--pathbase=/CoolApp` (不含尾端的斜線)。</span><span class="sxs-lookup"><span data-stu-id="6119f-228">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/">` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="6119f-229">在命令提示字元，於本機執行應用程式時傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="6119f-229">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="6119f-230">從應用程式目錄，執行：</span><span class="sxs-lookup"><span data-stu-id="6119f-230">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --pathbase=/relative-URL-path
  ```

* <span data-ttu-id="6119f-231">新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="6119f-231">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="6119f-232">此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。</span><span class="sxs-lookup"><span data-stu-id="6119f-232">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/relative-URL-path"
  ```

* <span data-ttu-id="6119f-233">在 Visual Studio 中，在 [屬性] > [偵錯] > [應用程式引數] 中指定引數。</span><span class="sxs-lookup"><span data-stu-id="6119f-233">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="6119f-234">在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="6119f-234">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/relative-URL-path
  ```

### <a name="urls"></a><span data-ttu-id="6119f-235">URL</span><span class="sxs-lookup"><span data-stu-id="6119f-235">URLs</span></span>

<span data-ttu-id="6119f-236">`--urls` 引數會搭配要用來接聽要求的連接埠和通訊協定，設定 IP 位址或主機位址。</span><span class="sxs-lookup"><span data-stu-id="6119f-236">The `--urls` argument sets the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="6119f-237">在命令提示字元，於本機執行應用程式時傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="6119f-237">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="6119f-238">從應用程式目錄，執行：</span><span class="sxs-lookup"><span data-stu-id="6119f-238">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="6119f-239">新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="6119f-239">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="6119f-240">此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。</span><span class="sxs-lookup"><span data-stu-id="6119f-240">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="6119f-241">在 Visual Studio 中，在 [屬性] > [偵錯] > [應用程式引數] 中指定引數。</span><span class="sxs-lookup"><span data-stu-id="6119f-241">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="6119f-242">在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="6119f-242">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="configure-the-linker"></a><span data-ttu-id="6119f-243">設定連結器</span><span class="sxs-lookup"><span data-stu-id="6119f-243">Configure the Linker</span></span>

<span data-ttu-id="6119f-244">Blazor 在每個組建上執行中繼語言 (IL) 連結，以從輸出組件移除不必要的 IL。</span><span class="sxs-lookup"><span data-stu-id="6119f-244">Blazor performs Intermediate Language (IL) linking on each build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="6119f-245">組件連結可在組建上控制。</span><span class="sxs-lookup"><span data-stu-id="6119f-245">Assembly linking can be controlled on build.</span></span> <span data-ttu-id="6119f-246">如需詳細資訊，請參閱<xref:host-and-deploy/blazor/configure-linker>。</span><span class="sxs-lookup"><span data-stu-id="6119f-246">For more information, see <xref:host-and-deploy/blazor/configure-linker>.</span></span>
