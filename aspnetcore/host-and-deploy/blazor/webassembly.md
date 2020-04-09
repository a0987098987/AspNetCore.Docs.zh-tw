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
# <a name="host-and-deploy-aspnet-core-opno-locblazor-webassembly"></a><span data-ttu-id="127b9-103">託管與部署ASP.NET核心BlazorWeb 組裝</span><span class="sxs-lookup"><span data-stu-id="127b9-103">Host and deploy ASP.NET Core Blazor WebAssembly</span></span>

<span data-ttu-id="127b9-104">作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="127b9-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="127b9-105">與[BlazorWeb 群組載託管模型](xref:blazor/hosting-models#blazor-webassembly):</span><span class="sxs-lookup"><span data-stu-id="127b9-105">With the [Blazor WebAssembly hosting model](xref:blazor/hosting-models#blazor-webassembly):</span></span>

* <span data-ttu-id="127b9-106">應用Blazor、其依賴項和 .NET 運行時將下載到瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="127b9-106">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="127b9-107">應用程式會直接在瀏覽器 UI 執行緒上執行。</span><span class="sxs-lookup"><span data-stu-id="127b9-107">The app is executed directly on the browser UI thread.</span></span>

<span data-ttu-id="127b9-108">支援以下部署政策:</span><span class="sxs-lookup"><span data-stu-id="127b9-108">The following deployment strategies are supported:</span></span>

* <span data-ttu-id="127b9-109">該應用程式Blazor由ASP.NET核心應用提供。</span><span class="sxs-lookup"><span data-stu-id="127b9-109">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="127b9-110">此策略已於[搭配 ASP.NET Core 的已裝載部署](#hosted-deployment-with-aspnet-core)一節中涵蓋。</span><span class="sxs-lookup"><span data-stu-id="127b9-110">This strategy is covered in the [Hosted deployment with ASP.NET Core](#hosted-deployment-with-aspnet-core) section.</span></span>
* <span data-ttu-id="127b9-111">該應用程式Blazor放置在靜態託管 Web 伺服器或服務上,其中 .NETBlazor不用於為 應用提供服務。</span><span class="sxs-lookup"><span data-stu-id="127b9-111">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="127b9-112">此策略在[「獨立部署](#standalone-deployment)」部分中介紹,其中包括Blazor有關將 WebAssembly 應用託管為 IIS 子應用的資訊。</span><span class="sxs-lookup"><span data-stu-id="127b9-112">This strategy is covered in the [Standalone deployment](#standalone-deployment) section, which includes information on hosting a Blazor WebAssembly app as an IIS sub-app.</span></span>

## <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="127b9-113">重寫 URL 以便正確地路由</span><span class="sxs-lookup"><span data-stu-id="127b9-113">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="127b9-114">Blazor WebAssembly 應用中的頁面元件的路由請求不像伺服器託管應用中的Blazor路由請求那樣簡單。</span><span class="sxs-lookup"><span data-stu-id="127b9-114">Routing requests for page components in a Blazor WebAssembly app isn't as straightforward as routing requests in a Blazor Server, hosted app.</span></span> <span data-ttu-id="127b9-115">考慮具有Blazor兩個元件的 WebAssembly 應用:</span><span class="sxs-lookup"><span data-stu-id="127b9-115">Consider a Blazor WebAssembly app with two components:</span></span>

* <span data-ttu-id="127b9-116">*Main.razor* &ndash; 載入應用程式根目錄，同時包含 `About` 元件 (`href="About"`) 的連結。</span><span class="sxs-lookup"><span data-stu-id="127b9-116">*Main.razor* &ndash; Loads at the root of the app and contains a link to the `About` component (`href="About"`).</span></span>
* <span data-ttu-id="127b9-117">*About.razor* &ndash; `About` 元件。</span><span class="sxs-lookup"><span data-stu-id="127b9-117">*About.razor* &ndash; `About` component.</span></span>

<span data-ttu-id="127b9-118">使用瀏覽器的網址列要求應用程式預設文件時 (例如 `https://www.contoso.com/`)：</span><span class="sxs-lookup"><span data-stu-id="127b9-118">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="127b9-119">瀏覽器提出要求。</span><span class="sxs-lookup"><span data-stu-id="127b9-119">The browser makes a request.</span></span>
1. <span data-ttu-id="127b9-120">傳回預設頁面，這通常是 *index.html*。</span><span class="sxs-lookup"><span data-stu-id="127b9-120">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="127b9-121">*index.html* 啟動載入應用程式。</span><span class="sxs-lookup"><span data-stu-id="127b9-121">*index.html* bootstraps the app.</span></span>
1. Blazor<span data-ttu-id="127b9-122">路由器負載,並呈現 Razor`Main`元件。</span><span class="sxs-lookup"><span data-stu-id="127b9-122">'s router loads, and the Razor `Main` component is rendered.</span></span>

<span data-ttu-id="127b9-123">在「主頁」 中`About`, 選擇指向元件的連結在用戶端上有效Blazor, 因為路由器阻止瀏覽器在 Internet`About`上向`About``www.contoso.com`for 和提供呈現元件本身的請求。</span><span class="sxs-lookup"><span data-stu-id="127b9-123">In the Main page, selecting the link to the `About` component works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the rendered `About` component itself.</span></span> <span data-ttu-id="127b9-124">WebAssembly 應用中的所有內部終結點請求的工作方式相同:請求不會觸發對 Internet 上伺服器託管資源的基於流覽*Blazor* 器的請求。</span><span class="sxs-lookup"><span data-stu-id="127b9-124">All of the requests for internal endpoints *within the Blazor WebAssembly app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="127b9-125">路由器會在內部處理要求。</span><span class="sxs-lookup"><span data-stu-id="127b9-125">The router handles the requests internally.</span></span>

<span data-ttu-id="127b9-126">如果使用瀏覽器之網址列提出對 `www.contoso.com/About` 的要求，則要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="127b9-126">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="127b9-127">在應用程式的網際網路主機上沒有這類資源存在，因此會傳回「404 - 找不到」\*\* 的回應。</span><span class="sxs-lookup"><span data-stu-id="127b9-127">No such resource exists on the app's Internet host, so a *404 - Not Found* response is returned.</span></span>

<span data-ttu-id="127b9-128">因為瀏覽器會提出要求給以網際網路為基礎的主機以取得用戶端頁面，所以網頁伺服器和裝載服務必須針對實際上不在伺服器上資源，將所有要求重新撰寫至 *index.html* 頁面。</span><span class="sxs-lookup"><span data-stu-id="127b9-128">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="127b9-129">返回*索引.html*時,Blazor應用的 路由器接管並回應正確的資源。</span><span class="sxs-lookup"><span data-stu-id="127b9-129">When *index.html* is returned, the app's Blazor router takes over and responds with the correct resource.</span></span>

<span data-ttu-id="127b9-130">部署到IIS伺服器時,可以將URL重寫模組與應用發佈的*Web.config檔*一起使用。</span><span class="sxs-lookup"><span data-stu-id="127b9-130">When deploying to an IIS server, you can use the URL Rewrite Module with the app's published *web.config* file.</span></span> <span data-ttu-id="127b9-131">有關詳細資訊,請參閱[IIS](#iis)部分。</span><span class="sxs-lookup"><span data-stu-id="127b9-131">For more information, see the [IIS](#iis) section.</span></span>

## <a name="hosted-deployment-with-aspnet-core"></a><span data-ttu-id="127b9-132">搭配 ASP.NET Core 的已裝載部署</span><span class="sxs-lookup"><span data-stu-id="127b9-132">Hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="127b9-133">*託管部署*將BlazorWebAssembly 應用從在 Web 伺服器上運行[ASP.NET 核心應用](xref:index)的瀏覽器提供服務。</span><span class="sxs-lookup"><span data-stu-id="127b9-133">A *hosted deployment* serves the Blazor WebAssembly app to browsers from an [ASP.NET Core app](xref:index) that runs on a web server.</span></span>

<span data-ttu-id="127b9-134">用戶端BlazorWebAssembly 應用將發佈到伺服器應用的 */bin/發佈/[目標框架]/發佈/wwwroot*資料夾中,以及伺服器應用的任何其他靜態 Web 資源。</span><span class="sxs-lookup"><span data-stu-id="127b9-134">The client Blazor WebAssembly app is published into the */bin/Release/{TARGET FRAMEWORK}/publish/wwwroot* folder of the server app, along with any other static web assets of the server app.</span></span> <span data-ttu-id="127b9-135">兩個應用程式一起部署。</span><span class="sxs-lookup"><span data-stu-id="127b9-135">The two apps are deployed together.</span></span> <span data-ttu-id="127b9-136">需要有能夠裝載 ASP.NET Core 應用程式的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="127b9-136">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="127b9-137">對於託管部署,Visual Studio 包括`-ho|--hosted``dotnet new`**BlazorWebAssembly 應用**專案`blazorwasm`樣本(使用[dotnet 新](/dotnet/core/tools/dotnet-new)指令時的範本),並選擇了 **『託管**』選項( 使用該 命令時)。</span><span class="sxs-lookup"><span data-stu-id="127b9-137">For a hosted deployment, Visual Studio includes the **Blazor WebAssembly App** project template (`blazorwasm` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command) with the **Hosted** option selected (`-ho|--hosted` when using the `dotnet new` command).</span></span>

<span data-ttu-id="127b9-138">如需 ASP.NET Core 應用程式裝載和部署的詳細資訊，請參閱 <xref:host-and-deploy/index>。</span><span class="sxs-lookup"><span data-stu-id="127b9-138">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="127b9-139">如需部署至 Azure App Service 的相關資訊，請參閱 <xref:tutorials/publish-to-azure-webapp-using-vs>。</span><span class="sxs-lookup"><span data-stu-id="127b9-139">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>

## <a name="standalone-deployment"></a><span data-ttu-id="127b9-140">獨立部署</span><span class="sxs-lookup"><span data-stu-id="127b9-140">Standalone deployment</span></span>

<span data-ttu-id="127b9-141">*獨立部署*Blazor將 WebAssembly 應用作為用戶端直接請求的一組靜態檔提供服務。</span><span class="sxs-lookup"><span data-stu-id="127b9-141">A *standalone deployment* serves the Blazor WebAssembly app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="127b9-142">任何靜態文件伺服器都能夠為Blazor應用提供服務。</span><span class="sxs-lookup"><span data-stu-id="127b9-142">Any static file server is able to serve the Blazor app.</span></span>

<span data-ttu-id="127b9-143">獨立部署資產發佈到 */bin/發佈/[目標框架]/發佈/wwwroot*檔夾中。</span><span class="sxs-lookup"><span data-stu-id="127b9-143">Standalone deployment assets are published into the */bin/Release/{TARGET FRAMEWORK}/publish/wwwroot* folder.</span></span>

### <a name="iis"></a><span data-ttu-id="127b9-144">IIS</span><span class="sxs-lookup"><span data-stu-id="127b9-144">IIS</span></span>

<span data-ttu-id="127b9-145">IIS 是可用於應用Blazor的可運行靜態文件伺服器。</span><span class="sxs-lookup"><span data-stu-id="127b9-145">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="127b9-146">要將 IISBlazor設定為主機,請參閱 在[IIS 上建構靜態網站](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="127b9-146">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="127b9-147">已發行的資產會建立在 */bin/Release/{TARGET FRAMEWORK}/publish* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="127b9-147">Published assets are created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="127b9-148">在網頁伺服器或裝載服務上，裝載 *publish* 資料夾的內容。</span><span class="sxs-lookup"><span data-stu-id="127b9-148">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

#### <a name="webconfig"></a><span data-ttu-id="127b9-149">web.config</span><span class="sxs-lookup"><span data-stu-id="127b9-149">web.config</span></span>

<span data-ttu-id="127b9-150">在Blazor清單中使用以下 IIS 設定建立*Web.config*檔:</span><span class="sxs-lookup"><span data-stu-id="127b9-150">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="127b9-151">針對下列副檔名設定 MIME 類型：</span><span class="sxs-lookup"><span data-stu-id="127b9-151">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="127b9-152">*.dll* &ndash;`application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="127b9-152">*.dll* &ndash; `application/octet-stream`</span></span>
  * <span data-ttu-id="127b9-153">*.json* &ndash;`application/json`</span><span class="sxs-lookup"><span data-stu-id="127b9-153">*.json* &ndash; `application/json`</span></span>
  * <span data-ttu-id="127b9-154">*.wasm* &ndash; `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="127b9-154">*.wasm* &ndash; `application/wasm`</span></span>
  * <span data-ttu-id="127b9-155">*.woff* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="127b9-155">*.woff* &ndash; `application/font-woff`</span></span>
  * <span data-ttu-id="127b9-156">*.woff2* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="127b9-156">*.woff2* &ndash; `application/font-woff`</span></span>
* <span data-ttu-id="127b9-157">針對下列 MIME 類型會啟用 HTTP 壓縮：</span><span class="sxs-lookup"><span data-stu-id="127b9-157">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="127b9-158">建立 URL Rewrite Module 規則：</span><span class="sxs-lookup"><span data-stu-id="127b9-158">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="127b9-159">提供應用靜態資產所在的子目錄 *(wwwroot/[PATH 請求])。*</span><span class="sxs-lookup"><span data-stu-id="127b9-159">Serve the sub-directory where the app's static assets reside (*wwwroot/{PATH REQUESTED}*).</span></span>
  * <span data-ttu-id="127b9-160">創建 SPA 回退路由,以便將非文件資產的請求重定向到其靜態資產資料夾 *(wwwroot/index.html*) 中的應用默認文檔。</span><span class="sxs-lookup"><span data-stu-id="127b9-160">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*wwwroot/index.html*).</span></span>
  
#### <a name="use-a-custom-webconfig"></a><span data-ttu-id="127b9-161">使用自訂 Web.config</span><span class="sxs-lookup"><span data-stu-id="127b9-161">Use a custom web.config</span></span>

<span data-ttu-id="127b9-162">要使用自訂*Web.config*檔,</span><span class="sxs-lookup"><span data-stu-id="127b9-162">To use a custom *web.config* file:</span></span>

1. <span data-ttu-id="127b9-163">將自訂*Web.config*檔案放在專案資料夾的根目錄。</span><span class="sxs-lookup"><span data-stu-id="127b9-163">Place the custom *web.config* file at the root of the project folder.</span></span>
1. <span data-ttu-id="127b9-164">將以下目標新增到項目檔 *(.csproj*):</span><span class="sxs-lookup"><span data-stu-id="127b9-164">Add the following target to the project file (*.csproj*):</span></span>

   ```xml
   <Target Name="CopyWebConfigOnPublish" AfterTargets="Publish">
     <Copy SourceFiles="web.config" DestinationFolder="$(PublishDir)" />
   </Target>
   ```
   
> [!NOTE]
> <span data-ttu-id="127b9-165">WebAssembly 應用中不支援`<IsWebConfigTransformDisabled>`使用 MSBuild`true`屬性,[因為它適用於部署到 IIS 的 ASP.NET 核心應用。](xref:host-and-deploy/iis/index#webconfig-file) Blazor</span><span class="sxs-lookup"><span data-stu-id="127b9-165">Use of the MSBuild property `<IsWebConfigTransformDisabled>` set to `true` isn't supported in Blazor WebAssembly apps [as it is for ASP.NET Core apps deployed to IIS](xref:host-and-deploy/iis/index#webconfig-file).</span></span> <span data-ttu-id="127b9-166">有關詳細資訊,請參閱[提供自Blazor訂 WASM Web.config(dotnet/aspnetcore #20569)所需的複製目標](https://github.com/dotnet/aspnetcore/issues/20569)。</span><span class="sxs-lookup"><span data-stu-id="127b9-166">For more information, see [Copy target required to provide custom Blazor WASM web.config (dotnet/aspnetcore #20569)](https://github.com/dotnet/aspnetcore/issues/20569).</span></span>

#### <a name="install-the-url-rewrite-module"></a><span data-ttu-id="127b9-167">安裝 URL Rewrite 模組</span><span class="sxs-lookup"><span data-stu-id="127b9-167">Install the URL Rewrite Module</span></span>

<span data-ttu-id="127b9-168">需要 [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite)，才可重寫 URL。</span><span class="sxs-lookup"><span data-stu-id="127b9-168">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="127b9-169">預設不會安裝此模組，且其無法用來安裝為網頁伺服器 (IIS) 角色服務功能。</span><span class="sxs-lookup"><span data-stu-id="127b9-169">The module isn't installed by default, and it isn't available for install as a Web Server (IIS) role service feature.</span></span> <span data-ttu-id="127b9-170">必須從 IIS 網站下載模組。</span><span class="sxs-lookup"><span data-stu-id="127b9-170">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="127b9-171">請使用 Web Platform Installer 安裝模組：</span><span class="sxs-lookup"><span data-stu-id="127b9-171">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="127b9-172">在本機上，巡覽至 [URL Rewrite Module 下載頁面](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads)。</span><span class="sxs-lookup"><span data-stu-id="127b9-172">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="127b9-173">如需英文版，請選取 [WebPI]\*\*\*\* 下載 WebPI 安裝程式。</span><span class="sxs-lookup"><span data-stu-id="127b9-173">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="127b9-174">如需其他語言，請選取適當的伺服器架構 (x86 x64) 來下載安裝程式。</span><span class="sxs-lookup"><span data-stu-id="127b9-174">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="127b9-175">將安裝程式複製到伺服器。</span><span class="sxs-lookup"><span data-stu-id="127b9-175">Copy the installer to the server.</span></span> <span data-ttu-id="127b9-176">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="127b9-176">Run the installer.</span></span> <span data-ttu-id="127b9-177">選取 [安裝]\*\*\*\* 按鈕，並接受授權條款。</span><span class="sxs-lookup"><span data-stu-id="127b9-177">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="127b9-178">安裝完成之後，不需要重新啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="127b9-178">A server restart isn't required after the install completes.</span></span>

#### <a name="configure-the-website"></a><span data-ttu-id="127b9-179">設定網站</span><span class="sxs-lookup"><span data-stu-id="127b9-179">Configure the website</span></span>

<span data-ttu-id="127b9-180">將網站的 [實體路徑]\*\*\*\* 設為應用程式的資料夾。</span><span class="sxs-lookup"><span data-stu-id="127b9-180">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="127b9-181">此資料夾包含：</span><span class="sxs-lookup"><span data-stu-id="127b9-181">The folder contains:</span></span>

* <span data-ttu-id="127b9-182">IIS 用來設定網站的 *web.config* 檔，包括必要的重新導向規則和檔案內容類型。</span><span class="sxs-lookup"><span data-stu-id="127b9-182">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="127b9-183">應用程式的靜態資產資料夾。</span><span class="sxs-lookup"><span data-stu-id="127b9-183">The app's static asset folder.</span></span>

#### <a name="host-as-an-iis-sub-app"></a><span data-ttu-id="127b9-184">主機作為 IIS 子應用</span><span class="sxs-lookup"><span data-stu-id="127b9-184">Host as an IIS sub-app</span></span>

<span data-ttu-id="127b9-185">如果獨立應用作為 IIS 子應用託管,請執行以下任一操作:</span><span class="sxs-lookup"><span data-stu-id="127b9-185">If a standalone app is hosted as an IIS sub-app, perform either of the following:</span></span>

* <span data-ttu-id="127b9-186">禁用繼承的ASP.NET核心模組處理程式。</span><span class="sxs-lookup"><span data-stu-id="127b9-186">Disable the inherited ASP.NET Core Module handler.</span></span>

  <span data-ttu-id="127b9-187">以新增檔案新增部份Blazor`<handlers>`來移除應用程式已發布*的 Web.config*檔案中的處理程式:</span><span class="sxs-lookup"><span data-stu-id="127b9-187">Remove the handler in the Blazor app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>

  ```xml
  <handlers>
    <remove name="aspNetCore" />
  </handlers>
  ```

* <span data-ttu-id="127b9-188">使用`<system.webServer>``<location>`設定為的元素關閉根(父)應用部份的繼承`false` `inheritInChildApplications`</span><span class="sxs-lookup"><span data-stu-id="127b9-188">Disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>

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

<span data-ttu-id="127b9-189">除了[配置應用的基本路徑](xref:host-and-deploy/blazor/index#app-base-path)外,還執行刪除處理程式或禁用繼承。</span><span class="sxs-lookup"><span data-stu-id="127b9-189">Removing the handler or disabling inheritance is performed in addition to [configuring the app's base path](xref:host-and-deploy/blazor/index#app-base-path).</span></span> <span data-ttu-id="127b9-190">在應用程式的 *index.html* 檔案，將應用程式基底路徑設為在 IIS 中設定子應用程式時使用的 IIS 別名。</span><span class="sxs-lookup"><span data-stu-id="127b9-190">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

#### <a name="troubleshooting"></a><span data-ttu-id="127b9-191">疑難排解</span><span class="sxs-lookup"><span data-stu-id="127b9-191">Troubleshooting</span></span>

<span data-ttu-id="127b9-192">如果收到「500 - 內部伺服器錯誤」\*\*，且 IIS 管理員在嘗試存取網站設定時擲回錯誤，請確認是否已安裝 URL Rewrite 模組。</span><span class="sxs-lookup"><span data-stu-id="127b9-192">If a *500 - Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="127b9-193">未安裝此模組時，IIS 無法剖析 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="127b9-193">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="127b9-194">這可以防止 IIS 管理器載入網站的配置和Blazor網站提供 靜態檔。</span><span class="sxs-lookup"><span data-stu-id="127b9-194">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="127b9-195">如需針對部署至 IIS 進行疑難排解的詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="127b9-195">For more information on troubleshooting deployments to IIS, see <xref:test/troubleshoot-azure-iis>.</span></span>

### <a name="azure-storage"></a><span data-ttu-id="127b9-196">Azure 儲存體</span><span class="sxs-lookup"><span data-stu-id="127b9-196">Azure Storage</span></span>

<span data-ttu-id="127b9-197">[Azure 儲存](/azure/storage/)靜態檔案託管允許Blazor無 伺服器應用託管。</span><span class="sxs-lookup"><span data-stu-id="127b9-197">[Azure Storage](/azure/storage/) static file hosting allows serverless Blazor app hosting.</span></span> <span data-ttu-id="127b9-198">支援自訂網域名稱、Azure 內容傳遞網路 (CDN) 及 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="127b9-198">Custom domain names, the Azure Content Delivery Network (CDN), and HTTPS are supported.</span></span>

<span data-ttu-id="127b9-199">當 Blob 服務針對儲存體帳戶上的靜態網站裝載啟用時：</span><span class="sxs-lookup"><span data-stu-id="127b9-199">When the blob service is enabled for static website hosting on a storage account:</span></span>

* <span data-ttu-id="127b9-200">將 [索引文件名稱]\*\*\*\* 設定為 `index.html`。</span><span class="sxs-lookup"><span data-stu-id="127b9-200">Set the **Index document name** to `index.html`.</span></span>
* <span data-ttu-id="127b9-201">將 [錯誤文件路徑]\*\*\*\* 設定為 `index.html`。</span><span class="sxs-lookup"><span data-stu-id="127b9-201">Set the **Error document path** to `index.html`.</span></span> <span data-ttu-id="127b9-202">Razor 元件和其他非檔案端點不會位於由 Blob 服務所存放之靜態內容中的實體路徑上。</span><span class="sxs-lookup"><span data-stu-id="127b9-202">Razor components and other non-file endpoints don't reside at physical paths in the static content stored by the blob service.</span></span> <span data-ttu-id="127b9-203">當收到Blazor路由器應處理的這些資源之一的請求時,blob 服務產生的*404 - 未找到*錯誤將請求路由到**錯誤文件路徑**。</span><span class="sxs-lookup"><span data-stu-id="127b9-203">When a request for one of these resources is received that the Blazor router should handle, the *404 - Not Found* error generated by the blob service routes the request to the **Error document path**.</span></span> <span data-ttu-id="127b9-204">傳*回 index.html* Blazorblob, 路由器載入和處理路徑。</span><span class="sxs-lookup"><span data-stu-id="127b9-204">The *index.html* blob is returned, and the Blazor router loads and processes the path.</span></span>

<span data-ttu-id="127b9-205">如需詳細資訊，請參閱 [Azure 儲存體中的靜態網站裝載](/azure/storage/blobs/storage-blob-static-website)。</span><span class="sxs-lookup"><span data-stu-id="127b9-205">For more information, see [Static website hosting in Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span></span>

### <a name="nginx"></a><span data-ttu-id="127b9-206">Nginx</span><span class="sxs-lookup"><span data-stu-id="127b9-206">Nginx</span></span>

<span data-ttu-id="127b9-207">以下*nginx.conf*檔被簡化,以展示如何設定 Nginx 以在磁碟上找不到相應的檔時發送*index.html*檔。</span><span class="sxs-lookup"><span data-stu-id="127b9-207">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *index.html* file whenever it can't find a corresponding file on disk.</span></span>

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

<span data-ttu-id="127b9-208">如需生產環境 Nginx 網頁伺服器組態的詳細資訊，請參閱[建立 NGINX Plus 和 NGINX 組態檔](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/)。</span><span class="sxs-lookup"><span data-stu-id="127b9-208">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

### <a name="nginx-in-docker"></a><span data-ttu-id="127b9-209">Docker 中的 Nginx</span><span class="sxs-lookup"><span data-stu-id="127b9-209">Nginx in Docker</span></span>

<span data-ttu-id="127b9-210">要使用BlazorNginx 在 Docker 中託管,請使用 Docker 檔來使用基於阿爾卑斯的 Nginx 映射。</span><span class="sxs-lookup"><span data-stu-id="127b9-210">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="127b9-211">更新 Dockerfile，將 *nginx.config* 檔案複製到容器內。</span><span class="sxs-lookup"><span data-stu-id="127b9-211">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="127b9-212">如下列範例所示，新增一行至 Dockerfile：</span><span class="sxs-lookup"><span data-stu-id="127b9-212">Add one line to the Dockerfile, as shown in the following example:</span></span>

```dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="apache"></a><span data-ttu-id="127b9-213">Apache</span><span class="sxs-lookup"><span data-stu-id="127b9-213">Apache</span></span>

<span data-ttu-id="127b9-214">要將BlazorWeb 組裝應用部署到 CentOS 7 或更高版本:</span><span class="sxs-lookup"><span data-stu-id="127b9-214">To deploy a Blazor WebAssembly app to CentOS 7 or later:</span></span>

1. <span data-ttu-id="127b9-215">建立 Apache 設定檔。</span><span class="sxs-lookup"><span data-stu-id="127b9-215">Create the Apache configuration file.</span></span> <span data-ttu-id="127b9-216">下面的範例是一個簡化的設定檔 (*blazorapp.config*):</span><span class="sxs-lookup"><span data-stu-id="127b9-216">The following example is a simplified configuration file (*blazorapp.config*):</span></span>

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

1. <span data-ttu-id="127b9-217">將 Apache 設定`/etc/httpd/conf.d/`檔放入 目錄中,該目錄是 CentOS 7 中的預設 Apache 設定目錄。</span><span class="sxs-lookup"><span data-stu-id="127b9-217">Place the Apache configuration file into the `/etc/httpd/conf.d/` directory, which is the default Apache configuration directory in CentOS 7.</span></span>

1. <span data-ttu-id="127b9-218">將應用的檔案放入`/var/www/blazorapp`目錄中(在配置`DocumentRoot`檔中指定的位置)。</span><span class="sxs-lookup"><span data-stu-id="127b9-218">Place the app's files into the `/var/www/blazorapp` directory (the location specified to `DocumentRoot` in the configuration file).</span></span>

1. <span data-ttu-id="127b9-219">重新啟動 Apache 服務。</span><span class="sxs-lookup"><span data-stu-id="127b9-219">Restart the Apache service.</span></span>

<span data-ttu-id="127b9-220">有關詳細資訊,請參閱[mod_mime](https://httpd.apache.org/docs/2.4/mod/mod_mime.html)和[mod_deflate](https://httpd.apache.org/docs/current/mod/mod_deflate.html)。</span><span class="sxs-lookup"><span data-stu-id="127b9-220">For more information, see [mod_mime](https://httpd.apache.org/docs/2.4/mod/mod_mime.html) and [mod_deflate](https://httpd.apache.org/docs/current/mod/mod_deflate.html).</span></span>

### <a name="github-pages"></a><span data-ttu-id="127b9-221">GitHub Pages</span><span class="sxs-lookup"><span data-stu-id="127b9-221">GitHub Pages</span></span>

<span data-ttu-id="127b9-222">若要處理 URL 重寫，請新增 *404.html* 檔，以及處理將要求重新導向至 *index.html* 頁面的指令碼。</span><span class="sxs-lookup"><span data-stu-id="127b9-222">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="127b9-223">如需社群所提供的範例實作，請參閱 [GitHub Pages 的單一頁面應用程式](https://spa-github-pages.rafrex.com/) (GitHub 上的 [rafrex/spa-github-pages](https://github.com/rafrex/spa-github-pages#readme))。</span><span class="sxs-lookup"><span data-stu-id="127b9-223">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](https://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="127b9-224">使用社群方法的範例可在 [GitHub 上的 blazor-demo/blazor-demo.github.io](https://github.com/blazor-demo/blazor-demo.github.io) ([即時網站](https://blazor-demo.github.io/)) 看到。</span><span class="sxs-lookup"><span data-stu-id="127b9-224">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="127b9-225">使用專案網站，而非組織網站時，請在 *index.html* 中新增或更新 `<base>` 標籤。</span><span class="sxs-lookup"><span data-stu-id="127b9-225">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="127b9-226">將 `href` 屬性值設定為 GitHub 存放庫名稱，並在尾端加上斜線 (例如 `my-repository/`)。</span><span class="sxs-lookup"><span data-stu-id="127b9-226">Set the `href` attribute value to the GitHub repository name with a trailing slash (for example, `my-repository/`.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="127b9-227">主機組態值</span><span class="sxs-lookup"><span data-stu-id="127b9-227">Host configuration values</span></span>

<span data-ttu-id="127b9-228">WebAssembly 應用可以接受以下主機配置值作為開發環境中運行時的命令列參數[Blazor](xref:blazor/hosting-models#blazor-webassembly)。</span><span class="sxs-lookup"><span data-stu-id="127b9-228">[Blazor WebAssembly apps](xref:blazor/hosting-models#blazor-webassembly) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="127b9-229">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="127b9-229">Content root</span></span>

<span data-ttu-id="127b9-230">參數`--contentroot`設定包含應用內容檔([內容根](xref:fundamentals/index#content-root))的目錄的絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="127b9-230">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files ([content root](xref:fundamentals/index#content-root)).</span></span> <span data-ttu-id="127b9-231">在下列範例中，`/content-root-path` 是應用程式的內容根路徑。</span><span class="sxs-lookup"><span data-stu-id="127b9-231">In the following examples, `/content-root-path` is the app's content root path.</span></span>

* <span data-ttu-id="127b9-232">在命令提示字元，於本機執行應用程式時傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="127b9-232">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="127b9-233">從應用程式目錄，執行：</span><span class="sxs-lookup"><span data-stu-id="127b9-233">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --contentroot=/content-root-path
  ```

* <span data-ttu-id="127b9-234">新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="127b9-234">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="127b9-235">此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。</span><span class="sxs-lookup"><span data-stu-id="127b9-235">This setting is used when the app is run with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* <span data-ttu-id="127b9-236">在視覺化工作室中,在**屬性** > **除錯** > **應用程式參數中指定參數**。</span><span class="sxs-lookup"><span data-stu-id="127b9-236">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="127b9-237">在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="127b9-237">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a><span data-ttu-id="127b9-238">路徑基底</span><span class="sxs-lookup"><span data-stu-id="127b9-238">Path base</span></span>

<span data-ttu-id="127b9-239">參數`--pathbase`設置使用非根相對 URL 路徑在本地運行的應用的應用`<base>`基本路徑`href`( 標記 設置為`/`過渡和生產以外的 路徑)。</span><span class="sxs-lookup"><span data-stu-id="127b9-239">The `--pathbase` argument sets the app base path for an app run locally with a non-root relative URL path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="127b9-240">在下列範例中，`/relative-URL-path` 是應用程式的路徑基底。</span><span class="sxs-lookup"><span data-stu-id="127b9-240">In the following examples, `/relative-URL-path` is the app's path base.</span></span> <span data-ttu-id="127b9-241">有關詳細資訊,請參閱[應用基本路徑](xref:host-and-deploy/blazor/index#app-base-path)。</span><span class="sxs-lookup"><span data-stu-id="127b9-241">For more information, see [App base path](xref:host-and-deploy/blazor/index#app-base-path).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="127b9-242">不同於提供給 `<base>` 標籤 `href` 的路徑，在傳遞 `--pathbase` 引數值時請勿包含尾端的斜線 (`/`)。</span><span class="sxs-lookup"><span data-stu-id="127b9-242">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="127b9-243">如果應用程式基底路徑提供於 `<base>` 標籤作為 `<base href="/CoolApp/">` (包括尾端的斜線)，請將命令列引數值傳遞為 `--pathbase=/CoolApp` (不含尾端的斜線)。</span><span class="sxs-lookup"><span data-stu-id="127b9-243">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/">` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="127b9-244">在命令提示字元，於本機執行應用程式時傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="127b9-244">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="127b9-245">從應用程式目錄，執行：</span><span class="sxs-lookup"><span data-stu-id="127b9-245">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --pathbase=/relative-URL-path
  ```

* <span data-ttu-id="127b9-246">新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="127b9-246">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="127b9-247">此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。</span><span class="sxs-lookup"><span data-stu-id="127b9-247">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/relative-URL-path"
  ```

* <span data-ttu-id="127b9-248">在視覺化工作室中,在**屬性** > **除錯** > **應用程式參數中指定參數**。</span><span class="sxs-lookup"><span data-stu-id="127b9-248">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="127b9-249">在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="127b9-249">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/relative-URL-path
  ```

### <a name="urls"></a><span data-ttu-id="127b9-250">URL</span><span class="sxs-lookup"><span data-stu-id="127b9-250">URLs</span></span>

<span data-ttu-id="127b9-251">`--urls` 引數會搭配要用來接聽要求的連接埠和通訊協定，設定 IP 位址或主機位址。</span><span class="sxs-lookup"><span data-stu-id="127b9-251">The `--urls` argument sets the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="127b9-252">在命令提示字元，於本機執行應用程式時傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="127b9-252">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="127b9-253">從應用程式目錄，執行：</span><span class="sxs-lookup"><span data-stu-id="127b9-253">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="127b9-254">新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="127b9-254">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="127b9-255">此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。</span><span class="sxs-lookup"><span data-stu-id="127b9-255">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="127b9-256">在視覺化工作室中,在**屬性** > **除錯** > **應用程式參數中指定參數**。</span><span class="sxs-lookup"><span data-stu-id="127b9-256">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="127b9-257">在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="127b9-257">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="configure-the-linker"></a><span data-ttu-id="127b9-258">設定連結器</span><span class="sxs-lookup"><span data-stu-id="127b9-258">Configure the Linker</span></span>

Blazor<span data-ttu-id="127b9-259">在每個版本生成上執行中間語言 (IL) 連結,從輸出程式集中刪除不必要的 IL。</span><span class="sxs-lookup"><span data-stu-id="127b9-259"> performs Intermediate Language (IL) linking on each Release build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="127b9-260">如需詳細資訊，請參閱 <xref:host-and-deploy/blazor/configure-linker>。</span><span class="sxs-lookup"><span data-stu-id="127b9-260">For more information, see <xref:host-and-deploy/blazor/configure-linker>.</span></span>
