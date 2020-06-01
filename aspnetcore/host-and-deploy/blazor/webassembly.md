---
<span data-ttu-id="5edc7-101">標題：「裝載和部署 ASP.NET Core Blazor WebAssembly ' 作者： guardrex 描述：」瞭解如何 Blazor 使用 ASP.NET Core、內容傳遞網路（CDN）、檔案伺服器和 GitHub 頁面來裝載和部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="5edc7-101">title: 'Host and deploy ASP.NET Core Blazor WebAssembly' author: guardrex description: 'Learn how to host and deploy a Blazor app using ASP.NET Core, Content Delivery Networks (CDN), file servers, and GitHub Pages.'</span></span>
<span data-ttu-id="5edc7-102">monikerRange： ' >= aspnetcore-3.1 ' ms-chap： riande ms. custom： mvc ms. date： 05/28/2020 no-loc：</span><span class="sxs-lookup"><span data-stu-id="5edc7-102">monikerRange: '>= aspnetcore-3.1' ms.author: riande ms.custom: mvc ms.date: 05/28/2020 no-loc:</span></span>
- <span data-ttu-id="5edc7-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="5edc7-103">'Blazor'</span></span>
- <span data-ttu-id="5edc7-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="5edc7-104">'Identity'</span></span>
- <span data-ttu-id="5edc7-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="5edc7-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="5edc7-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="5edc7-106">'Razor'</span></span>
- <span data-ttu-id="5edc7-107">' SignalR ' uid： host 和 deploy/blazor/webassembly</span><span class="sxs-lookup"><span data-stu-id="5edc7-107">'SignalR' uid: host-and-deploy/blazor/webassembly</span></span>

---
# <a name="host-and-deploy-aspnet-core-blazor-webassembly"></a><span data-ttu-id="5edc7-108">裝載和部署 ASP.NET Core Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="5edc7-108">Host and deploy ASP.NET Core Blazor WebAssembly</span></span>

<span data-ttu-id="5edc7-109">By [Luke Latham](https://github.com/guardrex)、 [Rainer Stropek](https://www.timecockpit.com)、 [Daniel Roth](https://github.com/danroth27)、 [Ben Adams](https://twitter.com/ben_a_adams)和[Safia Abdalla](https://safia.rocks)</span><span class="sxs-lookup"><span data-stu-id="5edc7-109">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), [Daniel Roth](https://github.com/danroth27), [Ben Adams](https://twitter.com/ben_a_adams), and [Safia Abdalla](https://safia.rocks)</span></span>

<span data-ttu-id="5edc7-110">使用[ Blazor WebAssembly 裝載模型](xref:blazor/hosting-models#blazor-webassembly)：</span><span class="sxs-lookup"><span data-stu-id="5edc7-110">With the [Blazor WebAssembly hosting model](xref:blazor/hosting-models#blazor-webassembly):</span></span>

* <span data-ttu-id="5edc7-111">Blazor應用程式、其相依性和 .net 執行時間會以平行方式下載至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="5edc7-111">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser in parallel.</span></span>
* <span data-ttu-id="5edc7-112">應用程式會直接在瀏覽器 UI 執行緒上執行。</span><span class="sxs-lookup"><span data-stu-id="5edc7-112">The app is executed directly on the browser UI thread.</span></span>

<span data-ttu-id="5edc7-113">支援下列部署策略：</span><span class="sxs-lookup"><span data-stu-id="5edc7-113">The following deployment strategies are supported:</span></span>

* <span data-ttu-id="5edc7-114">Blazor應用程式是由 ASP.NET Core 應用程式提供。</span><span class="sxs-lookup"><span data-stu-id="5edc7-114">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="5edc7-115">此策略已於[搭配 ASP.NET Core 的已裝載部署](#hosted-deployment-with-aspnet-core)一節中涵蓋。</span><span class="sxs-lookup"><span data-stu-id="5edc7-115">This strategy is covered in the [Hosted deployment with ASP.NET Core](#hosted-deployment-with-aspnet-core) section.</span></span>
* <span data-ttu-id="5edc7-116">Blazor應用程式會放在靜態裝載的 web 伺服器或服務上，其中 .net 不會用來服務 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5edc7-116">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="5edc7-117">此策略在[獨立部署](#standalone-deployment)一節中涵蓋，其中包括裝載 Blazor WebAssembly 應用程式作為 IIS 子應用程式的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="5edc7-117">This strategy is covered in the [Standalone deployment](#standalone-deployment) section, which includes information on hosting a Blazor WebAssembly app as an IIS sub-app.</span></span>

## <a name="precompression"></a><span data-ttu-id="5edc7-118">Precompression</span><span class="sxs-lookup"><span data-stu-id="5edc7-118">Precompression</span></span>

<span data-ttu-id="5edc7-119">Blazor發佈 WebAssembly 應用程式時，會 precompressed 輸出以減少應用程式的大小，並移除執行時間壓縮的需求。</span><span class="sxs-lookup"><span data-stu-id="5edc7-119">When a Blazor WebAssembly app is published, the output is precompressed to reduce the app's size and remove the need for runtime compression.</span></span> <span data-ttu-id="5edc7-120">使用下列壓縮演算法：</span><span class="sxs-lookup"><span data-stu-id="5edc7-120">The following compression algorithms are used:</span></span>

* <span data-ttu-id="5edc7-121">[Brotli](https://tools.ietf.org/html/rfc7932) （最高層級）</span><span class="sxs-lookup"><span data-stu-id="5edc7-121">[Brotli](https://tools.ietf.org/html/rfc7932) (highest level)</span></span>
* [<span data-ttu-id="5edc7-122">Gzip</span><span class="sxs-lookup"><span data-stu-id="5edc7-122">Gzip</span></span>](https://tools.ietf.org/html/rfc1952)

<span data-ttu-id="5edc7-123">若要停用壓縮，請將 `BlazorEnableCompression` MSBuild 屬性新增至應用程式的專案檔，並將值設定為 `false` ：</span><span class="sxs-lookup"><span data-stu-id="5edc7-123">To disable compression, add the `BlazorEnableCompression` MSBuild property to the app's project file and set the value to `false`:</span></span>

```xml
<PropertyGroup>
  <BlazorEnableCompression>false</BlazorEnableCompression>
</PropertyGroup>
```

<span data-ttu-id="5edc7-124">如需 IIS *web.config*壓縮設定，請參閱[iis： Brotli 和 Gzip 壓縮](#brotli-and-gzip-compression)一節。</span><span class="sxs-lookup"><span data-stu-id="5edc7-124">For IIS *web.config* compression configuration, see the [IIS: Brotli and Gzip compression](#brotli-and-gzip-compression) section.</span></span>

## <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="5edc7-125">重寫 URL 以便正確地路由</span><span class="sxs-lookup"><span data-stu-id="5edc7-125">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="5edc7-126">WebAssembly 應用程式中頁面元件的路由要求 Blazor ，並不像伺服器上裝載的 Blazor 應用程式中的路由要求一樣簡單。</span><span class="sxs-lookup"><span data-stu-id="5edc7-126">Routing requests for page components in a Blazor WebAssembly app isn't as straightforward as routing requests in a Blazor Server, hosted app.</span></span> <span data-ttu-id="5edc7-127">假設有 Blazor 兩個元件的 WebAssembly 應用程式：</span><span class="sxs-lookup"><span data-stu-id="5edc7-127">Consider a Blazor WebAssembly app with two components:</span></span>

* <span data-ttu-id="5edc7-128">*Main razor*：在應用程式的根目錄載入，並包含 `About` 元件（）的連結 `href="About"` 。</span><span class="sxs-lookup"><span data-stu-id="5edc7-128">*Main.razor*: Loads at the root of the app and contains a link to the `About` component (`href="About"`).</span></span>
* <span data-ttu-id="5edc7-129">*關於 razor*： `About` component。</span><span class="sxs-lookup"><span data-stu-id="5edc7-129">*About.razor*: `About` component.</span></span>

<span data-ttu-id="5edc7-130">使用瀏覽器的網址列要求應用程式預設文件時 (例如 `https://www.contoso.com/`)：</span><span class="sxs-lookup"><span data-stu-id="5edc7-130">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="5edc7-131">瀏覽器提出要求。</span><span class="sxs-lookup"><span data-stu-id="5edc7-131">The browser makes a request.</span></span>
1. <span data-ttu-id="5edc7-132">傳回預設頁面，這通常是 *index.html*。</span><span class="sxs-lookup"><span data-stu-id="5edc7-132">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="5edc7-133">*index.html* 啟動載入應用程式。</span><span class="sxs-lookup"><span data-stu-id="5edc7-133">*index.html* bootstraps the app.</span></span>
1. Blazor<span data-ttu-id="5edc7-134">的路由器會載入，並 Razor `Main` 呈現元件。</span><span class="sxs-lookup"><span data-stu-id="5edc7-134">'s router loads, and the Razor `Main` component is rendered.</span></span>

<span data-ttu-id="5edc7-135">在主頁面中，選取元件的連結可 `About` 在用戶端上運作，因為 Blazor 路由器會阻止瀏覽器針對在網際網路上提出要求， `www.contoso.com` `About` 並提供所呈現 `About` 元件本身。</span><span class="sxs-lookup"><span data-stu-id="5edc7-135">In the Main page, selecting the link to the `About` component works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the rendered `About` component itself.</span></span> <span data-ttu-id="5edc7-136">\* Blazor WebAssembly 應用程式內\*內部端點的所有要求都以相同的方式工作：要求不會對網際網路上伺服器裝載的資源觸發瀏覽器型要求。</span><span class="sxs-lookup"><span data-stu-id="5edc7-136">All of the requests for internal endpoints *within the Blazor WebAssembly app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="5edc7-137">路由器會在內部處理要求。</span><span class="sxs-lookup"><span data-stu-id="5edc7-137">The router handles the requests internally.</span></span>

<span data-ttu-id="5edc7-138">如果使用瀏覽器之網址列提出對 `www.contoso.com/About` 的要求，則要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="5edc7-138">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="5edc7-139">在應用程式的網際網路主機上沒有這類資源存在，因此會傳回「404 - 找不到」\*\* 的回應。</span><span class="sxs-lookup"><span data-stu-id="5edc7-139">No such resource exists on the app's Internet host, so a *404 - Not Found* response is returned.</span></span>

<span data-ttu-id="5edc7-140">因為瀏覽器會提出要求給以網際網路為基礎的主機以取得用戶端頁面，所以網頁伺服器和裝載服務必須針對實際上不在伺服器上資源，將所有要求重新撰寫至 *index.html* 頁面。</span><span class="sxs-lookup"><span data-stu-id="5edc7-140">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="5edc7-141">當傳回*index. html*時，應用程式的 Blazor 路由器會接管並以正確的資源回應。</span><span class="sxs-lookup"><span data-stu-id="5edc7-141">When *index.html* is returned, the app's Blazor router takes over and responds with the correct resource.</span></span>

<span data-ttu-id="5edc7-142">部署到 IIS 伺服器時，您可以使用 URL 重寫模組搭配應用*程式已發佈的 web.config 檔案*。</span><span class="sxs-lookup"><span data-stu-id="5edc7-142">When deploying to an IIS server, you can use the URL Rewrite Module with the app's published *web.config* file.</span></span> <span data-ttu-id="5edc7-143">如需詳細資訊，請參閱[IIS](#iis)一節。</span><span class="sxs-lookup"><span data-stu-id="5edc7-143">For more information, see the [IIS](#iis) section.</span></span>

## <a name="hosted-deployment-with-aspnet-core"></a><span data-ttu-id="5edc7-144">搭配 ASP.NET Core 的已裝載部署</span><span class="sxs-lookup"><span data-stu-id="5edc7-144">Hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="5edc7-145">*託管部署* Blazor 可從 web 伺服器上執行的[ASP.NET Core 應用程式](xref:index)，將 WebAssembly 應用程式提供給瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="5edc7-145">A *hosted deployment* serves the Blazor WebAssembly app to browsers from an [ASP.NET Core app](xref:index) that runs on a web server.</span></span>

<span data-ttu-id="5edc7-146">用戶端 Blazor WebAssembly 應用程式會發佈到伺服器應用程式的 */bin/Release/{TARGET FRAMEWORK}/publish/wwwroot*資料夾，以及伺服器應用程式的任何其他靜態 web 資產。</span><span class="sxs-lookup"><span data-stu-id="5edc7-146">The client Blazor WebAssembly app is published into the */bin/Release/{TARGET FRAMEWORK}/publish/wwwroot* folder of the server app, along with any other static web assets of the server app.</span></span> <span data-ttu-id="5edc7-147">這兩個應用程式會一起部署。</span><span class="sxs-lookup"><span data-stu-id="5edc7-147">The two apps are deployed together.</span></span> <span data-ttu-id="5edc7-148">需要有能夠裝載 ASP.NET Core 應用程式的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="5edc7-148">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="5edc7-149">針對裝載的部署，Visual Studio 包括\*\* Blazor WebAssembly 應用程式**專案範本（ `blazorwasm` 使用[dotnet new](/dotnet/core/tools/dotnet-new)命令時的範本）和選取**Hosted\*\*的裝載選項（ `-ho|--hosted` 使用命令時 `dotnet new` ）。</span><span class="sxs-lookup"><span data-stu-id="5edc7-149">For a hosted deployment, Visual Studio includes the **Blazor WebAssembly App** project template (`blazorwasm` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command) with the **Hosted** option selected (`-ho|--hosted` when using the `dotnet new` command).</span></span>

<span data-ttu-id="5edc7-150">如需 ASP.NET Core 應用程式裝載和部署的詳細資訊，請參閱 <xref:host-and-deploy/index>。</span><span class="sxs-lookup"><span data-stu-id="5edc7-150">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="5edc7-151">如需部署至 Azure App Service 的相關資訊，請參閱 <xref:tutorials/publish-to-azure-webapp-using-vs>。</span><span class="sxs-lookup"><span data-stu-id="5edc7-151">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>

## <a name="standalone-deployment"></a><span data-ttu-id="5edc7-152">獨立部署</span><span class="sxs-lookup"><span data-stu-id="5edc7-152">Standalone deployment</span></span>

<span data-ttu-id="5edc7-153">*獨立部署* Blazor 會將 WebAssembly 應用程式當做一組直接由用戶端要求的靜態檔案來提供。</span><span class="sxs-lookup"><span data-stu-id="5edc7-153">A *standalone deployment* serves the Blazor WebAssembly app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="5edc7-154">任何靜態檔案伺服器都可以服務 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5edc7-154">Any static file server is able to serve the Blazor app.</span></span>

<span data-ttu-id="5edc7-155">獨立部署資產會發佈到 */BIN/RELEASE/{TARGET FRAMEWORK}/publish/wwwroot*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="5edc7-155">Standalone deployment assets are published into the */bin/Release/{TARGET FRAMEWORK}/publish/wwwroot* folder.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="5edc7-156">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5edc7-156">Azure App Service</span></span>

Blazor<span data-ttu-id="5edc7-157">WebAssembly apps 可以部署到 Windows 上的 Azure App 服務，在[IIS](#iis)上裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="5edc7-157"> WebAssembly apps can be deployed to Azure App Services on Windows, which hosts the app on [IIS](#iis).</span></span>

<span data-ttu-id="5edc7-158">Blazor目前不支援將獨立 WebAssembly 應用程式部署至適用于 Linux 的 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="5edc7-158">Deploying a standalone Blazor WebAssembly app to Azure App Service for Linux isn't currently supported.</span></span> <span data-ttu-id="5edc7-159">目前無法使用可裝載應用程式的 Linux 伺服器映射。</span><span class="sxs-lookup"><span data-stu-id="5edc7-159">A Linux server image to host the app isn't available at this time.</span></span> <span data-ttu-id="5edc7-160">正在進行工作，以啟用此案例。</span><span class="sxs-lookup"><span data-stu-id="5edc7-160">Work is in progress to enable this scenario.</span></span>

### <a name="iis"></a><span data-ttu-id="5edc7-161">IIS</span><span class="sxs-lookup"><span data-stu-id="5edc7-161">IIS</span></span>

<span data-ttu-id="5edc7-162">IIS 是適用于應用程式的靜態檔案伺服器 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="5edc7-162">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="5edc7-163">若要設定 IIS 來裝載 Blazor ，請參閱[在 Iis 上建立靜態網站](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="5edc7-163">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="5edc7-164">已發行的資產會建立在 */bin/Release/{TARGET FRAMEWORK}/publish* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="5edc7-164">Published assets are created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="5edc7-165">在網頁伺服器或裝載服務上，裝載 *publish* 資料夾的內容。</span><span class="sxs-lookup"><span data-stu-id="5edc7-165">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

#### <a name="webconfig"></a><span data-ttu-id="5edc7-166">web.config</span><span class="sxs-lookup"><span data-stu-id="5edc7-166">web.config</span></span>

<span data-ttu-id="5edc7-167">Blazor發行專案時，會使用下列 IIS 設定來建立*web.config*檔案：</span><span class="sxs-lookup"><span data-stu-id="5edc7-167">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="5edc7-168">針對下列副檔名設定 MIME 類型：</span><span class="sxs-lookup"><span data-stu-id="5edc7-168">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="5edc7-169">*.dll*：`application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="5edc7-169">*.dll*: `application/octet-stream`</span></span>
  * <span data-ttu-id="5edc7-170">*. json*：`application/json`</span><span class="sxs-lookup"><span data-stu-id="5edc7-170">*.json*: `application/json`</span></span>
  * <span data-ttu-id="5edc7-171">*. wasm*：`application/wasm`</span><span class="sxs-lookup"><span data-stu-id="5edc7-171">*.wasm*: `application/wasm`</span></span>
  * <span data-ttu-id="5edc7-172">*. woff*：`application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="5edc7-172">*.woff*: `application/font-woff`</span></span>
  * <span data-ttu-id="5edc7-173">*. woff2*：`application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="5edc7-173">*.woff2*: `application/font-woff`</span></span>
* <span data-ttu-id="5edc7-174">針對下列 MIME 類型會啟用 HTTP 壓縮：</span><span class="sxs-lookup"><span data-stu-id="5edc7-174">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="5edc7-175">建立 URL Rewrite Module 規則：</span><span class="sxs-lookup"><span data-stu-id="5edc7-175">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="5edc7-176">提供應用程式靜態資產所在的子目錄（已*要求 wwwroot/{PATH}*）。</span><span class="sxs-lookup"><span data-stu-id="5edc7-176">Serve the sub-directory where the app's static assets reside (*wwwroot/{PATH REQUESTED}*).</span></span>
  * <span data-ttu-id="5edc7-177">建立 SPA fallback 路由，讓非檔案資產的要求會重新導向至其靜態資產資料夾（*wwwroot/index.html*）中的應用程式預設檔。</span><span class="sxs-lookup"><span data-stu-id="5edc7-177">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*wwwroot/index.html*).</span></span>
  
#### <a name="use-a-custom-webconfig"></a><span data-ttu-id="5edc7-178">使用自訂的 web.config</span><span class="sxs-lookup"><span data-stu-id="5edc7-178">Use a custom web.config</span></span>

<span data-ttu-id="5edc7-179">若要使用自訂的*web.config*檔案，請*將自訂的 web.config 檔案*放在專案資料夾的根目錄，併發行專案。</span><span class="sxs-lookup"><span data-stu-id="5edc7-179">To use a custom *web.config* file, place the custom *web.config* file at the root of the project folder and publish the project.</span></span>

#### <a name="install-the-url-rewrite-module"></a><span data-ttu-id="5edc7-180">安裝 URL Rewrite 模組</span><span class="sxs-lookup"><span data-stu-id="5edc7-180">Install the URL Rewrite Module</span></span>

<span data-ttu-id="5edc7-181">需要 [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite)，才可重寫 URL。</span><span class="sxs-lookup"><span data-stu-id="5edc7-181">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="5edc7-182">預設不會安裝此模組，且其無法用來安裝為網頁伺服器 (IIS) 角色服務功能。</span><span class="sxs-lookup"><span data-stu-id="5edc7-182">The module isn't installed by default, and it isn't available for install as a Web Server (IIS) role service feature.</span></span> <span data-ttu-id="5edc7-183">必須從 IIS 網站下載模組。</span><span class="sxs-lookup"><span data-stu-id="5edc7-183">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="5edc7-184">請使用 Web Platform Installer 安裝模組：</span><span class="sxs-lookup"><span data-stu-id="5edc7-184">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="5edc7-185">在本機上，巡覽至 [URL Rewrite Module 下載頁面](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads)。</span><span class="sxs-lookup"><span data-stu-id="5edc7-185">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="5edc7-186">如需英文版，請選取 [WebPI]\*\*\*\* 下載 WebPI 安裝程式。</span><span class="sxs-lookup"><span data-stu-id="5edc7-186">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="5edc7-187">如需其他語言，請選取適當的伺服器架構 (x86 x64) 來下載安裝程式。</span><span class="sxs-lookup"><span data-stu-id="5edc7-187">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="5edc7-188">將安裝程式複製到伺服器。</span><span class="sxs-lookup"><span data-stu-id="5edc7-188">Copy the installer to the server.</span></span> <span data-ttu-id="5edc7-189">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="5edc7-189">Run the installer.</span></span> <span data-ttu-id="5edc7-190">選取 [安裝]\*\*\*\* 按鈕，並接受授權條款。</span><span class="sxs-lookup"><span data-stu-id="5edc7-190">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="5edc7-191">安裝完成之後，不需要重新啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="5edc7-191">A server restart isn't required after the install completes.</span></span>

#### <a name="configure-the-website"></a><span data-ttu-id="5edc7-192">設定網站</span><span class="sxs-lookup"><span data-stu-id="5edc7-192">Configure the website</span></span>

<span data-ttu-id="5edc7-193">將網站的 [實體路徑]\*\*\*\* 設為應用程式的資料夾。</span><span class="sxs-lookup"><span data-stu-id="5edc7-193">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="5edc7-194">此資料夾包含：</span><span class="sxs-lookup"><span data-stu-id="5edc7-194">The folder contains:</span></span>

* <span data-ttu-id="5edc7-195">IIS 用來設定網站的 *web.config* 檔，包括必要的重新導向規則和檔案內容類型。</span><span class="sxs-lookup"><span data-stu-id="5edc7-195">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="5edc7-196">應用程式的靜態資產資料夾。</span><span class="sxs-lookup"><span data-stu-id="5edc7-196">The app's static asset folder.</span></span>

#### <a name="host-as-an-iis-sub-app"></a><span data-ttu-id="5edc7-197">以 IIS 子應用程式的形式裝載</span><span class="sxs-lookup"><span data-stu-id="5edc7-197">Host as an IIS sub-app</span></span>

<span data-ttu-id="5edc7-198">如果獨立應用程式裝載為 IIS 子應用程式，請執行下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="5edc7-198">If a standalone app is hosted as an IIS sub-app, perform either of the following:</span></span>

* <span data-ttu-id="5edc7-199">停用繼承的 ASP.NET Core 模組處理常式。</span><span class="sxs-lookup"><span data-stu-id="5edc7-199">Disable the inherited ASP.NET Core Module handler.</span></span>

  <span data-ttu-id="5edc7-200">Blazor將區段新增至檔案，以移除應用程式已發佈的*web.config*檔案中的處理常式 `<handlers>` ：</span><span class="sxs-lookup"><span data-stu-id="5edc7-200">Remove the handler in the Blazor app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>

  ```xml
  <handlers>
    <remove name="aspNetCore" />
  </handlers>
  ```

* <span data-ttu-id="5edc7-201">`<system.webServer>`使用設定為的元素，停用根（父系）應用程式區段的繼承 `<location>` `inheritInChildApplications` `false` ：</span><span class="sxs-lookup"><span data-stu-id="5edc7-201">Disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>

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

<span data-ttu-id="5edc7-202">除了設定[應用程式的基底路徑](xref:host-and-deploy/blazor/index#app-base-path)之外，也會執行移除處理常式或停用繼承。</span><span class="sxs-lookup"><span data-stu-id="5edc7-202">Removing the handler or disabling inheritance is performed in addition to [configuring the app's base path](xref:host-and-deploy/blazor/index#app-base-path).</span></span> <span data-ttu-id="5edc7-203">在應用程式的 *index.html* 檔案，將應用程式基底路徑設為在 IIS 中設定子應用程式時使用的 IIS 別名。</span><span class="sxs-lookup"><span data-stu-id="5edc7-203">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

#### <a name="brotli-and-gzip-compression"></a><span data-ttu-id="5edc7-204">Brotli 和 Gzip 壓縮</span><span class="sxs-lookup"><span data-stu-id="5edc7-204">Brotli and Gzip compression</span></span>

<span data-ttu-id="5edc7-205">您可以*透過 web.config 來設定 IIS，以*提供 Brotli 或 Gzip 壓縮的 Blazor 資產。</span><span class="sxs-lookup"><span data-stu-id="5edc7-205">IIS can be configured via *web.config* to serve Brotli or Gzip compressed Blazor assets.</span></span> <span data-ttu-id="5edc7-206">如需設定範例，請參閱[web.config](webassembly/_samples/web.config?raw=true)。</span><span class="sxs-lookup"><span data-stu-id="5edc7-206">For an example configuration, see [web.config](webassembly/_samples/web.config?raw=true).</span></span>

#### <a name="troubleshooting"></a><span data-ttu-id="5edc7-207">疑難排解</span><span class="sxs-lookup"><span data-stu-id="5edc7-207">Troubleshooting</span></span>

<span data-ttu-id="5edc7-208">如果收到「500 - 內部伺服器錯誤」\*\*，且 IIS 管理員在嘗試存取網站設定時擲回錯誤，請確認是否已安裝 URL Rewrite 模組。</span><span class="sxs-lookup"><span data-stu-id="5edc7-208">If a *500 - Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="5edc7-209">未安裝此模組時，IIS 無法剖析 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="5edc7-209">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="5edc7-210">這可防止 IIS 管理員從服務的靜態檔案載入網站的設定和網站 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="5edc7-210">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="5edc7-211">如需針對部署至 IIS 進行疑難排解的詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="5edc7-211">For more information on troubleshooting deployments to IIS, see <xref:test/troubleshoot-azure-iis>.</span></span>

### <a name="azure-storage"></a><span data-ttu-id="5edc7-212">Azure 儲存體</span><span class="sxs-lookup"><span data-stu-id="5edc7-212">Azure Storage</span></span>

<span data-ttu-id="5edc7-213">[Azure 儲存體](/azure/storage/)靜態檔案裝載允許無伺服器 Blazor 應用程式裝載。</span><span class="sxs-lookup"><span data-stu-id="5edc7-213">[Azure Storage](/azure/storage/) static file hosting allows serverless Blazor app hosting.</span></span> <span data-ttu-id="5edc7-214">支援自訂網域名稱、Azure 內容傳遞網路 (CDN) 及 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="5edc7-214">Custom domain names, the Azure Content Delivery Network (CDN), and HTTPS are supported.</span></span>

<span data-ttu-id="5edc7-215">當 Blob 服務針對儲存體帳戶上的靜態網站裝載啟用時：</span><span class="sxs-lookup"><span data-stu-id="5edc7-215">When the blob service is enabled for static website hosting on a storage account:</span></span>

* <span data-ttu-id="5edc7-216">將 [索引文件名稱]\*\*\*\* 設定為 `index.html`。</span><span class="sxs-lookup"><span data-stu-id="5edc7-216">Set the **Index document name** to `index.html`.</span></span>
* <span data-ttu-id="5edc7-217">將 [錯誤文件路徑]\*\*\*\* 設定為 `index.html`。</span><span class="sxs-lookup"><span data-stu-id="5edc7-217">Set the **Error document path** to `index.html`.</span></span> Razor<span data-ttu-id="5edc7-218">元件和其他非檔案端點不會位於 blob 服務所儲存之靜態內容中的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="5edc7-218"> components and other non-file endpoints don't reside at physical paths in the static content stored by the blob service.</span></span> <span data-ttu-id="5edc7-219">收到路由器應處理的其中一個資源的要求時 Blazor ，由 blob 服務產生的*404-找不*到的錯誤會將要求路由傳送至**錯誤檔路徑**。</span><span class="sxs-lookup"><span data-stu-id="5edc7-219">When a request for one of these resources is received that the Blazor router should handle, the *404 - Not Found* error generated by the blob service routes the request to the **Error document path**.</span></span> <span data-ttu-id="5edc7-220">會傳回*索引 .html* blob，且 Blazor 路由器會載入並處理路徑。</span><span class="sxs-lookup"><span data-stu-id="5edc7-220">The *index.html* blob is returned, and the Blazor router loads and processes the path.</span></span>

<span data-ttu-id="5edc7-221">如果在執行時間因為檔案標頭中有不適當的 MIME 類型而未載入檔案 `Content-Type` ，請採取下列其中一個動作：</span><span class="sxs-lookup"><span data-stu-id="5edc7-221">If files aren't loaded at runtime due to inappropriate MIME types in the files' `Content-Type` headers, take either of the following actions:</span></span>

* <span data-ttu-id="5edc7-222">設定您的工具，以在部署檔案時設定正確的 MIME 類型（ `Content-Type` 標頭）。</span><span class="sxs-lookup"><span data-stu-id="5edc7-222">Configure your tooling to set the correct MIME types (`Content-Type` headers) when the files are deployed.</span></span>
* <span data-ttu-id="5edc7-223">在部署應用程式之後，變更檔案的 MIME 類型（ `Content-Type` 標頭）。</span><span class="sxs-lookup"><span data-stu-id="5edc7-223">Change the MIME types (`Content-Type` headers) for the files after the app is deployed.</span></span>

  <span data-ttu-id="5edc7-224">在每個檔案的儲存體總管（Azure 入口網站）中：</span><span class="sxs-lookup"><span data-stu-id="5edc7-224">In Storage Explorer (Azure portal) for each file:</span></span>
  
  1. <span data-ttu-id="5edc7-225">以滑鼠右鍵按一下檔案，然後選取 [**屬性**]。</span><span class="sxs-lookup"><span data-stu-id="5edc7-225">Right-click the file and select **Properties**.</span></span>
  1. <span data-ttu-id="5edc7-226">設定**ContentType** ，然後選取 [**儲存**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="5edc7-226">Set the **ContentType** and select the **Save** button.</span></span>

<span data-ttu-id="5edc7-227">如需詳細資訊，請參閱 [Azure 儲存體中的靜態網站裝載](/azure/storage/blobs/storage-blob-static-website)。</span><span class="sxs-lookup"><span data-stu-id="5edc7-227">For more information, see [Static website hosting in Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span></span>

### <a name="nginx"></a><span data-ttu-id="5edc7-228">Nginx</span><span class="sxs-lookup"><span data-stu-id="5edc7-228">Nginx</span></span>

<span data-ttu-id="5edc7-229">下列*nginx*檔案已簡化，示範如何將 nginx 設定為每當找不到磁片上的對應檔案時，就傳送 *.html 檔案。*</span><span class="sxs-lookup"><span data-stu-id="5edc7-229">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *index.html* file whenever it can't find a corresponding file on disk.</span></span>

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

<span data-ttu-id="5edc7-230">如需生產環境 Nginx 網頁伺服器組態的詳細資訊，請參閱[建立 NGINX Plus 和 NGINX 組態檔](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/)。</span><span class="sxs-lookup"><span data-stu-id="5edc7-230">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

### <a name="nginx-in-docker"></a><span data-ttu-id="5edc7-231">Docker 中的 Nginx</span><span class="sxs-lookup"><span data-stu-id="5edc7-231">Nginx in Docker</span></span>

<span data-ttu-id="5edc7-232">若要 Blazor 使用 Nginx 在 Docker 中裝載，請將 Dockerfile 設定為使用以 Alpine 為基礎的 Nginx 映射。</span><span class="sxs-lookup"><span data-stu-id="5edc7-232">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="5edc7-233">更新 Dockerfile，將 *nginx.config* 檔案複製到容器內。</span><span class="sxs-lookup"><span data-stu-id="5edc7-233">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="5edc7-234">如下列範例所示，新增一行至 Dockerfile：</span><span class="sxs-lookup"><span data-stu-id="5edc7-234">Add one line to the Dockerfile, as shown in the following example:</span></span>

```dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="apache"></a><span data-ttu-id="5edc7-235">Apache</span><span class="sxs-lookup"><span data-stu-id="5edc7-235">Apache</span></span>

<span data-ttu-id="5edc7-236">若要將 Blazor WebAssembly 應用程式部署至 CentOS 7 或更新版本：</span><span class="sxs-lookup"><span data-stu-id="5edc7-236">To deploy a Blazor WebAssembly app to CentOS 7 or later:</span></span>

1. <span data-ttu-id="5edc7-237">建立 Apache 設定檔。</span><span class="sxs-lookup"><span data-stu-id="5edc7-237">Create the Apache configuration file.</span></span> <span data-ttu-id="5edc7-238">下列範例是一個簡化的設定檔案（*blazorapp*）：</span><span class="sxs-lookup"><span data-stu-id="5edc7-238">The following example is a simplified configuration file (*blazorapp.config*):</span></span>

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

1. <span data-ttu-id="5edc7-239">將 Apache 設定檔案放在 `/etc/httpd/conf.d/` 目錄中，這是 CentOS 7 中的預設 Apache 設定目錄。</span><span class="sxs-lookup"><span data-stu-id="5edc7-239">Place the Apache configuration file into the `/etc/httpd/conf.d/` directory, which is the default Apache configuration directory in CentOS 7.</span></span>

1. <span data-ttu-id="5edc7-240">將應用程式的檔案放入 `/var/www/blazorapp` 目錄中（在 `DocumentRoot` 設定檔中指定的位置）。</span><span class="sxs-lookup"><span data-stu-id="5edc7-240">Place the app's files into the `/var/www/blazorapp` directory (the location specified to `DocumentRoot` in the configuration file).</span></span>

1. <span data-ttu-id="5edc7-241">重新開機 Apache 服務。</span><span class="sxs-lookup"><span data-stu-id="5edc7-241">Restart the Apache service.</span></span>

<span data-ttu-id="5edc7-242">如需詳細資訊，請參閱[mod_mime](https://httpd.apache.org/docs/2.4/mod/mod_mime.html)和[mod_deflate](https://httpd.apache.org/docs/current/mod/mod_deflate.html)。</span><span class="sxs-lookup"><span data-stu-id="5edc7-242">For more information, see [mod_mime](https://httpd.apache.org/docs/2.4/mod/mod_mime.html) and [mod_deflate](https://httpd.apache.org/docs/current/mod/mod_deflate.html).</span></span>

### <a name="github-pages"></a><span data-ttu-id="5edc7-243">GitHub Pages</span><span class="sxs-lookup"><span data-stu-id="5edc7-243">GitHub Pages</span></span>

<span data-ttu-id="5edc7-244">若要處理 URL 重寫，請新增 *404.html* 檔，以及處理將要求重新導向至 *index.html* 頁面的指令碼。</span><span class="sxs-lookup"><span data-stu-id="5edc7-244">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="5edc7-245">如需社群所提供的範例實作，請參閱 [GitHub Pages 的單一頁面應用程式](https://spa-github-pages.rafrex.com/) (GitHub 上的 [rafrex/spa-github-pages](https://github.com/rafrex/spa-github-pages#readme))。</span><span class="sxs-lookup"><span data-stu-id="5edc7-245">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](https://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="5edc7-246">使用社群方法的範例可在 [GitHub 上的 blazor-demo/blazor-demo.github.io](https://github.com/blazor-demo/blazor-demo.github.io) ([即時網站](https://blazor-demo.github.io/)) 看到。</span><span class="sxs-lookup"><span data-stu-id="5edc7-246">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="5edc7-247">使用專案網站，而非組織網站時，請在 *index.html* 中新增或更新 `<base>` 標籤。</span><span class="sxs-lookup"><span data-stu-id="5edc7-247">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="5edc7-248">將 `href` 屬性值設定為 GitHub 存放庫名稱，並在尾端加上斜線 (例如 `my-repository/`)。</span><span class="sxs-lookup"><span data-stu-id="5edc7-248">Set the `href` attribute value to the GitHub repository name with a trailing slash (for example, `my-repository/`.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="5edc7-249">主機組態值</span><span class="sxs-lookup"><span data-stu-id="5edc7-249">Host configuration values</span></span>

<span data-ttu-id="5edc7-250">[ Blazor WebAssembly apps](xref:blazor/hosting-models#blazor-webassembly)可以在開發環境中的執行時間，接受下列主機設定值做為命令列引數。</span><span class="sxs-lookup"><span data-stu-id="5edc7-250">[Blazor WebAssembly apps](xref:blazor/hosting-models#blazor-webassembly) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="5edc7-251">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="5edc7-251">Content root</span></span>

<span data-ttu-id="5edc7-252">`--contentroot`引數會設定包含應用程式內容檔案（[內容根](xref:fundamentals/index#content-root)）之目錄的絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="5edc7-252">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files ([content root](xref:fundamentals/index#content-root)).</span></span> <span data-ttu-id="5edc7-253">在下列範例中，`/content-root-path` 是應用程式的內容根路徑。</span><span class="sxs-lookup"><span data-stu-id="5edc7-253">In the following examples, `/content-root-path` is the app's content root path.</span></span>

* <span data-ttu-id="5edc7-254">在命令提示字元，於本機執行應用程式時傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="5edc7-254">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="5edc7-255">從應用程式目錄，執行：</span><span class="sxs-lookup"><span data-stu-id="5edc7-255">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --contentroot=/content-root-path
  ```

* <span data-ttu-id="5edc7-256">新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="5edc7-256">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="5edc7-257">此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。</span><span class="sxs-lookup"><span data-stu-id="5edc7-257">This setting is used when the app is run with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* <span data-ttu-id="5edc7-258">在 Visual Studio 中，于 [**屬性**] [  >  **調試**  >  **程式引數**] 中指定引數。</span><span class="sxs-lookup"><span data-stu-id="5edc7-258">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="5edc7-259">在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="5edc7-259">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a><span data-ttu-id="5edc7-260">路徑基底</span><span class="sxs-lookup"><span data-stu-id="5edc7-260">Path base</span></span>

<span data-ttu-id="5edc7-261">`--pathbase`引數會設定應用程式以非根相對 URL 路徑在本機執行的應用程式基底路徑（此標籤 `<base>` `href` 會設定為 `/` 預備和生產以外的路徑）。</span><span class="sxs-lookup"><span data-stu-id="5edc7-261">The `--pathbase` argument sets the app base path for an app run locally with a non-root relative URL path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="5edc7-262">在下列範例中，`/relative-URL-path` 是應用程式的路徑基底。</span><span class="sxs-lookup"><span data-stu-id="5edc7-262">In the following examples, `/relative-URL-path` is the app's path base.</span></span> <span data-ttu-id="5edc7-263">如需詳細資訊，請參閱[應用程式基底路徑](xref:host-and-deploy/blazor/index#app-base-path)。</span><span class="sxs-lookup"><span data-stu-id="5edc7-263">For more information, see [App base path](xref:host-and-deploy/blazor/index#app-base-path).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5edc7-264">不同於提供給 `<base>` 標籤 `href` 的路徑，在傳遞 `--pathbase` 引數值時請勿包含尾端的斜線 (`/`)。</span><span class="sxs-lookup"><span data-stu-id="5edc7-264">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="5edc7-265">如果應用程式基底路徑提供於 `<base>` 標籤作為 `<base href="/CoolApp/">` (包括尾端的斜線)，請將命令列引數值傳遞為 `--pathbase=/CoolApp` (不含尾端的斜線)。</span><span class="sxs-lookup"><span data-stu-id="5edc7-265">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/">` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="5edc7-266">在命令提示字元，於本機執行應用程式時傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="5edc7-266">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="5edc7-267">從應用程式目錄，執行：</span><span class="sxs-lookup"><span data-stu-id="5edc7-267">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --pathbase=/relative-URL-path
  ```

* <span data-ttu-id="5edc7-268">新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="5edc7-268">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="5edc7-269">此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。</span><span class="sxs-lookup"><span data-stu-id="5edc7-269">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/relative-URL-path"
  ```

* <span data-ttu-id="5edc7-270">在 Visual Studio 中，于 [**屬性**] [  >  **調試**  >  **程式引數**] 中指定引數。</span><span class="sxs-lookup"><span data-stu-id="5edc7-270">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="5edc7-271">在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="5edc7-271">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/relative-URL-path
  ```

### <a name="urls"></a><span data-ttu-id="5edc7-272">URL</span><span class="sxs-lookup"><span data-stu-id="5edc7-272">URLs</span></span>

<span data-ttu-id="5edc7-273">`--urls` 引數會搭配要用來接聽要求的連接埠和通訊協定，設定 IP 位址或主機位址。</span><span class="sxs-lookup"><span data-stu-id="5edc7-273">The `--urls` argument sets the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="5edc7-274">在命令提示字元，於本機執行應用程式時傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="5edc7-274">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="5edc7-275">從應用程式目錄，執行：</span><span class="sxs-lookup"><span data-stu-id="5edc7-275">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="5edc7-276">新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="5edc7-276">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="5edc7-277">此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。</span><span class="sxs-lookup"><span data-stu-id="5edc7-277">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="5edc7-278">在 Visual Studio 中，于 [**屬性**] [  >  **調試**  >  **程式引數**] 中指定引數。</span><span class="sxs-lookup"><span data-stu-id="5edc7-278">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="5edc7-279">在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="5edc7-279">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="configure-the-linker"></a><span data-ttu-id="5edc7-280">設定連結器</span><span class="sxs-lookup"><span data-stu-id="5edc7-280">Configure the Linker</span></span>

Blazor<span data-ttu-id="5edc7-281">在每個發行組建上執行中繼語言（IL）連結，以從輸出元件移除不必要的 IL。</span><span class="sxs-lookup"><span data-stu-id="5edc7-281"> performs Intermediate Language (IL) linking on each Release build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="5edc7-282">如需詳細資訊，請參閱<xref:host-and-deploy/blazor/configure-linker>。</span><span class="sxs-lookup"><span data-stu-id="5edc7-282">For more information, see <xref:host-and-deploy/blazor/configure-linker>.</span></span>

## <a name="custom-boot-resource-loading"></a><span data-ttu-id="5edc7-283">自訂開機資源載入</span><span class="sxs-lookup"><span data-stu-id="5edc7-283">Custom boot resource loading</span></span>

<span data-ttu-id="5edc7-284">BlazorWebAssembly 應用程式可以使用函式進行初始化， `loadBootResource` 以覆寫內建的開機資源載入機制。</span><span class="sxs-lookup"><span data-stu-id="5edc7-284">A Blazor WebAssembly app can be initialized with the `loadBootResource` function to override the built-in boot resource loading mechanism.</span></span> <span data-ttu-id="5edc7-285">`loadBootResource`在下列情況下使用：</span><span class="sxs-lookup"><span data-stu-id="5edc7-285">Use `loadBootResource` for the following scenarios:</span></span>

* <span data-ttu-id="5edc7-286">允許使用者從 CDN 載入靜態資源，例如時區資料或*dotnet wasm。*</span><span class="sxs-lookup"><span data-stu-id="5edc7-286">Allow users to load static resources, such as timezone data or *dotnet.wasm* from a CDN.</span></span>
* <span data-ttu-id="5edc7-287">使用 HTTP 要求載入壓縮的元件，並將它們解壓縮到不支援從伺服器提取壓縮內容的主機用戶端上。</span><span class="sxs-lookup"><span data-stu-id="5edc7-287">Load compressed assemblies using an HTTP request and decompress them on the client for hosts that don't support fetching compressed contents from the server.</span></span>
* <span data-ttu-id="5edc7-288">將每個要求重新導向至新名稱，以將資源別名設為不同名稱 `fetch` 。</span><span class="sxs-lookup"><span data-stu-id="5edc7-288">Alias resources to a different name by redirecting each `fetch` request to a new name.</span></span>

<span data-ttu-id="5edc7-289">`loadBootResource`參數會出現在下表中。</span><span class="sxs-lookup"><span data-stu-id="5edc7-289">`loadBootResource` parameters appear in the following table.</span></span>

| <span data-ttu-id="5edc7-290">參數</span><span class="sxs-lookup"><span data-stu-id="5edc7-290">Parameter</span></span>    | <span data-ttu-id="5edc7-291">說明</span><span class="sxs-lookup"><span data-stu-id="5edc7-291">Description</span></span> |
| ------------ | ----------- |
| `type`       | <span data-ttu-id="5edc7-292">資源類型。</span><span class="sxs-lookup"><span data-stu-id="5edc7-292">The type of the resource.</span></span> <span data-ttu-id="5edc7-293">運算子類型： `assembly` 、 `pdb` 、 `dotnetjs` 、 `dotnetwasm` 、`timezonedata`</span><span class="sxs-lookup"><span data-stu-id="5edc7-293">Permissable types: `assembly`, `pdb`, `dotnetjs`, `dotnetwasm`, `timezonedata`</span></span> |
| `name`       | <span data-ttu-id="5edc7-294">資源名稱。</span><span class="sxs-lookup"><span data-stu-id="5edc7-294">The name of the resource.</span></span> |
| `defaultUri` | <span data-ttu-id="5edc7-295">資源的相對或絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="5edc7-295">The relative or absolute URI of the resource.</span></span> |
| `integrity`  | <span data-ttu-id="5edc7-296">代表回應中預期內容的完整性字串。</span><span class="sxs-lookup"><span data-stu-id="5edc7-296">The integrity string representing the expected content in the response.</span></span> |

<span data-ttu-id="5edc7-297">`loadBootResource`傳回下列任何一項，以覆寫載入進程：</span><span class="sxs-lookup"><span data-stu-id="5edc7-297">`loadBootResource` returns any of the following to override the loading process:</span></span>

* <span data-ttu-id="5edc7-298">URI 字串。</span><span class="sxs-lookup"><span data-stu-id="5edc7-298">URI string.</span></span> <span data-ttu-id="5edc7-299">在下列範例（*wwwroot/index.html*）中，從 CDN 提供下列檔案 `https://my-awesome-cdn.com/` ：</span><span class="sxs-lookup"><span data-stu-id="5edc7-299">In the following example (*wwwroot/index.html*), the following files are served from a CDN at `https://my-awesome-cdn.com/`:</span></span>

  * <span data-ttu-id="5edc7-300">*dotnet。 \*node.js*</span><span class="sxs-lookup"><span data-stu-id="5edc7-300">*dotnet.\*.js*</span></span>
  * <span data-ttu-id="5edc7-301">*dotnet. wasm*</span><span class="sxs-lookup"><span data-stu-id="5edc7-301">*dotnet.wasm*</span></span>
  * <span data-ttu-id="5edc7-302">時區資料</span><span class="sxs-lookup"><span data-stu-id="5edc7-302">Timezone data</span></span>

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

* <span data-ttu-id="5edc7-303">`Promise<Response>`.</span><span class="sxs-lookup"><span data-stu-id="5edc7-303">`Promise<Response>`.</span></span> <span data-ttu-id="5edc7-304">`integrity`在標頭中傳遞參數，以保留預設的完整性檢查行為。</span><span class="sxs-lookup"><span data-stu-id="5edc7-304">Pass the `integrity` parameter in a header to retain the default integrity-checking behavior.</span></span>

  <span data-ttu-id="5edc7-305">下列範例（*wwwroot/index.html*）會將自訂 HTTP 標頭新增至輸出要求，並將參數傳遞至 `integrity` `fetch` 呼叫：</span><span class="sxs-lookup"><span data-stu-id="5edc7-305">The following example (*wwwroot/index.html*) adds a custom HTTP header to the outbound requests and passes the `integrity` parameter through to the `fetch` call:</span></span>
  
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

* <span data-ttu-id="5edc7-306">`null`/`undefined`，這會產生預設的載入行為。</span><span class="sxs-lookup"><span data-stu-id="5edc7-306">`null`/`undefined`, which results in the default loading behavior.</span></span>

<span data-ttu-id="5edc7-307">外部來源必須傳回瀏覽器所需的 CORS 標頭，以允許跨原始來源資源載入。</span><span class="sxs-lookup"><span data-stu-id="5edc7-307">External sources must return the required CORS headers for browsers to allow the cross-origin resource loading.</span></span> <span data-ttu-id="5edc7-308">根據預設，Cdn 通常會提供必要的標頭。</span><span class="sxs-lookup"><span data-stu-id="5edc7-308">CDNs usually provide the required headers by default.</span></span>

<span data-ttu-id="5edc7-309">您只需要指定自訂行為的類型。</span><span class="sxs-lookup"><span data-stu-id="5edc7-309">You only need to specify types for custom behaviors.</span></span> <span data-ttu-id="5edc7-310">根據 `loadBootResource` 預設載入行為，架構會載入未指定的類型。</span><span class="sxs-lookup"><span data-stu-id="5edc7-310">Types not specified to `loadBootResource` are loaded by the framework per their default loading behaviors.</span></span>

## <a name="change-the-filename-extension-of-dll-files"></a><span data-ttu-id="5edc7-311">變更 DLL 檔案的副檔名</span><span class="sxs-lookup"><span data-stu-id="5edc7-311">Change the filename extension of DLL files</span></span>

<span data-ttu-id="5edc7-312">如果您需要變更應用程式已發佈 *.dll*檔案的副檔名，請遵循本節中的指導方針。</span><span class="sxs-lookup"><span data-stu-id="5edc7-312">In case you have a need to change the filename extensions of the app's published *.dll* files, follow the guidance in this section.</span></span>

<span data-ttu-id="5edc7-313">發行應用程式之後，請使用 shell 腳本或 DevOps 組建管線，將 *.dll*檔案重新命名為使用不同的副檔名。</span><span class="sxs-lookup"><span data-stu-id="5edc7-313">After publishing the app, use a shell script or DevOps build pipeline to rename *.dll* files to use a different file extension.</span></span> <span data-ttu-id="5edc7-314">以應用程式已發佈輸出的*wwwroot*目錄中的 *.dll*檔案為目標（例如 *{CONTENT ROOT}/bin/Release/netstandard2.1/publish/wwwroot*）。</span><span class="sxs-lookup"><span data-stu-id="5edc7-314">Target the *.dll* files in the *wwwroot* directory of the app's published output (for example, *{CONTENT ROOT}/bin/Release/netstandard2.1/publish/wwwroot*).</span></span>

<span data-ttu-id="5edc7-315">在下列範例中， *.dll*檔案已重新命名為使用*bin*副檔名。</span><span class="sxs-lookup"><span data-stu-id="5edc7-315">In the following examples, *.dll* files are renamed to use the *.bin* file extension.</span></span>

<span data-ttu-id="5edc7-316">在 Windows 上：</span><span class="sxs-lookup"><span data-stu-id="5edc7-316">On Windows:</span></span>

```powershell
dir .\_framework\_bin | rename-item -NewName { $_.name -replace ".dll\b",".bin" }
((Get-Content .\_framework\blazor.boot.json -Raw) -replace '.dll"','.bin"') | Set-Content .\_framework\blazor.boot.json
```

<span data-ttu-id="5edc7-317">如果服務工作者資產也在使用中，請新增下列命令：</span><span class="sxs-lookup"><span data-stu-id="5edc7-317">If service worker assets are also in use, add the following command:</span></span>

```powershell
((Get-Content .\service-worker-assets.js -Raw) -replace '.dll"','.bin"') | Set-Content .\service-worker-assets.js
```

<span data-ttu-id="5edc7-318">在 Linux 或 macOS 上：</span><span class="sxs-lookup"><span data-stu-id="5edc7-318">On Linux or macOS:</span></span>

```console
for f in _framework/_bin/*; do mv "$f" "`echo $f | sed -e 's/\.dll\b/.bin/g'`"; done
sed -i 's/\.dll"/.bin"/g' _framework/blazor.boot.json
```

<span data-ttu-id="5edc7-319">如果服務工作者資產也在使用中，請新增下列命令：</span><span class="sxs-lookup"><span data-stu-id="5edc7-319">If service worker assets are also in use, add the following command:</span></span>

```console
sed -i 's/\.dll"/.bin"/g' service-worker-assets.js
```
   
<span data-ttu-id="5edc7-320">若要使用與*bin*不同的副檔名，請在上述命令中取代 *. bin* 。</span><span class="sxs-lookup"><span data-stu-id="5edc7-320">To use a different file extension than *.bin*, replace *.bin* in the preceding commands.</span></span>

<span data-ttu-id="5edc7-321">若要解決壓縮的*blazor gz*和*blazor.boot.json.br*檔案，請採用下列其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="5edc7-321">To address the compressed *blazor.boot.json.gz* and *blazor.boot.json.br* files, adopt either of the following approaches:</span></span>

* <span data-ttu-id="5edc7-322">移除壓縮的*blazor gz*和*blazor.boot.json.br*檔案。</span><span class="sxs-lookup"><span data-stu-id="5edc7-322">Remove the compressed *blazor.boot.json.gz* and *blazor.boot.json.br* files.</span></span> <span data-ttu-id="5edc7-323">此方法會停用壓縮。</span><span class="sxs-lookup"><span data-stu-id="5edc7-323">Compression is disabled with this approach.</span></span>
* <span data-ttu-id="5edc7-324">重新壓縮已更新的*blazor*檔案。</span><span class="sxs-lookup"><span data-stu-id="5edc7-324">Recompress the updated *blazor.boot.json* file.</span></span>

<span data-ttu-id="5edc7-325">先前的指導方針也適用于服務工作者資產正在使用時。</span><span class="sxs-lookup"><span data-stu-id="5edc7-325">The preceding guidance also applies when service worker assets are in use.</span></span> <span data-ttu-id="5edc7-326">移除或重新壓縮*wwwroot/服務-worker-node.js*和*wwwroot/service-worker-assets。 gz*。</span><span class="sxs-lookup"><span data-stu-id="5edc7-326">Remove or recompress *wwwroot/service-worker-assets.js.br* and *wwwroot/service-worker-assets.js.gz*.</span></span> <span data-ttu-id="5edc7-327">否則，瀏覽器中的檔案完整性檢查會失敗。</span><span class="sxs-lookup"><span data-stu-id="5edc7-327">Otherwise, file integrity checks fail in the browser.</span></span>

<span data-ttu-id="5edc7-328">下列 Windows 範例會使用放在專案根目錄中的 PowerShell 腳本。</span><span class="sxs-lookup"><span data-stu-id="5edc7-328">The following Windows example uses a PowerShell script placed at the root of the project.</span></span>

<span data-ttu-id="5edc7-329">*ChangeDLLExtensions. ps1：*：</span><span class="sxs-lookup"><span data-stu-id="5edc7-329">*ChangeDLLExtensions.ps1:*:</span></span>

```powershell
param([string]$filepath,[string]$tfm)
dir $filepath\bin\Release\$tfm\wwwroot\_framework\_bin | rename-item -NewName { $_.name -replace ".dll\b",".bin" }
((Get-Content $filepath\bin\Release\$tfm\wwwroot\_framework\blazor.boot.json -Raw) -replace '.dll"','.bin"') | Set-Content $filepath\bin\Release\$tfm\wwwroot\_framework\blazor.boot.json
Remove-Item $filepath\bin\Release\$tfm\wwwroot\_framework\blazor.boot.json.gz
```

<span data-ttu-id="5edc7-330">如果服務工作者資產也在使用中，請新增下列命令：</span><span class="sxs-lookup"><span data-stu-id="5edc7-330">If service worker assets are also in use, add the following command:</span></span>

```powershell
((Get-Content $filepath\bin\Release\$tfm\wwwroot\service-worker-assets.js -Raw) -replace '.dll"','.bin"') | Set-Content $filepath\bin\Release\$tfm\wwwroot\service-worker-assets.js
```

<span data-ttu-id="5edc7-331">在專案檔中，腳本會在發行應用程式之後執行：</span><span class="sxs-lookup"><span data-stu-id="5edc7-331">In the project file, the script is run after publishing the app:</span></span>

```xml
<Target Name="ChangeDLLFileExtensions" AfterTargets="Publish" Condition="'$(Configuration)'=='Release'">
  <Exec Command="powershell.exe -command &quot;&amp; { .\ChangeDLLExtensions.ps1 '$(SolutionDir)' '$(TargetFramework)'}&quot;" />
</Target>
```

<span data-ttu-id="5edc7-332">若要提供意見反應，請造訪[aspnetcore/問題 #5477](https://github.com/dotnet/aspnetcore/issues/5477)。</span><span class="sxs-lookup"><span data-stu-id="5edc7-332">To provide feedback, visit [aspnetcore/issues #5477](https://github.com/dotnet/aspnetcore/issues/5477).</span></span>
 
