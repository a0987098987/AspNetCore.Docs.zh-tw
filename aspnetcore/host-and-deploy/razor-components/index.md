---
title: 裝載和部署 Razor 元件
author: guardrex
description: 了解如何使用 ASP.NET Core、內容傳遞網路 (CDN)、檔案伺服器和 GitHub Pages 裝載和部署 Razor 元件和 Blazor 應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/22/2019
uid: host-and-deploy/razor-components/index
---
# <a name="host-and-deploy-razor-components"></a><span data-ttu-id="cc5ed-103">裝載和部署 Razor 元件</span><span class="sxs-lookup"><span data-stu-id="cc5ed-103">Host and deploy Razor Components</span></span>

<span data-ttu-id="cc5ed-104">作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="cc5ed-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

## <a name="publish-the-app"></a><span data-ttu-id="cc5ed-105">發行應用程式</span><span class="sxs-lookup"><span data-stu-id="cc5ed-105">Publish the app</span></span>

<span data-ttu-id="cc5ed-106">應用程式在發行組態中使用 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令發佈以供部署。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-106">Apps are published for deployment in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="cc5ed-107">IDE 可能會自動使用內建的發佈功能處理執行 `dotnet publish` 命令，因此可能不需要以手動方式從命令提示字元執行命令，視使用中的開發工具而定。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-107">An IDE may handle executing the `dotnet publish` command automatically using its built-in publishing features, so it might not be necessary to manually execute the command from a command prompt depending on the development tools in use.</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="cc5ed-108">`dotnet publish` 會觸發專案相依性的[還原](/dotnet/core/tools/dotnet-restore)，並[建置](/dotnet/core/tools/dotnet-build)專案，然後才建立資產以供部署。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-108">`dotnet publish` triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="cc5ed-109">在建置程序中，會移除未使用的方法和組件，減少應用程式的下載大小和載入時間。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-109">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span> <span data-ttu-id="cc5ed-110">部署會建立在 */bin/Release/&lt;target-framework&gt;/publish* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-110">The deployment is created in the */bin/Release/&lt;target-framework&gt;/publish* folder.</span></span>

<span data-ttu-id="cc5ed-111">*publish* 資料夾中的資產會部署至網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-111">The assets in the *publish* folder are deployed to the web server.</span></span> <span data-ttu-id="cc5ed-112">部署可能是手動或自動的程序，視使用中的開發工具而定。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-112">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="cc5ed-113">主機組態值</span><span class="sxs-lookup"><span data-stu-id="cc5ed-113">Host configuration values</span></span>

<span data-ttu-id="cc5ed-114">使用[伺服器端裝載模型](xref:razor-components/hosting-models#server-side-hosting-model)的 Razor 元件應用程式，可以接受 [Web 主機組態值](xref:fundamentals/host/web-host#host-configuration-values)。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-114">Razor Components apps that use the [server-side hosting model](xref:razor-components/hosting-models#server-side-hosting-model) can accept [Web Host configuration values](xref:fundamentals/host/web-host#host-configuration-values).</span></span>

<span data-ttu-id="cc5ed-115">使用[用戶端裝載模型](xref:razor-components/hosting-models#client-side-hosting-model)的 Blazor 應用程式，可以在開發環境中，在執行時接受下列主機組態值作為命令列引數。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-115">Blazor apps that use the [client-side hosting model](xref:razor-components/hosting-models#client-side-hosting-model) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="cc5ed-116">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="cc5ed-116">Content Root</span></span>

<span data-ttu-id="cc5ed-117">`--contentroot` 引數設定包含應用程式內容檔案的目錄絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-117">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files.</span></span>

* <span data-ttu-id="cc5ed-118">在命令提示字元，於本機執行應用程式時傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-118">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="cc5ed-119">從應用程式目錄，執行：</span><span class="sxs-lookup"><span data-stu-id="cc5ed-119">From the app's directory, execute:</span></span>

  ```console
  dotnet run --contentroot=/<content-root>
  ```

* <span data-ttu-id="cc5ed-120">新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-120">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="cc5ed-121">在使用 Visual Studio 偵錯工具執行應用程式時，和從命令提示字元以 `dotnet run` 執行應用程式時，會取得這項設定。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-121">This setting is picked up when running the app with the Visual Studio Debugger and when running the app from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/<content-root>"
  ```

* <span data-ttu-id="cc5ed-122">在 Visual Studio 中，在 [屬性] > [偵錯] > [應用程式引數] 中指定引數。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-122">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="cc5ed-123">在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-123">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/<content-root>
  ```

### <a name="path-base"></a><span data-ttu-id="cc5ed-124">路徑基底</span><span class="sxs-lookup"><span data-stu-id="cc5ed-124">Path base</span></span>

<span data-ttu-id="cc5ed-125">`--pathbase` 引數會以非根虛擬路徑，為本機執行的應用程式設定應用程式基底路徑 (`<base>` 標籤 `href` 會設為非 `/` 的路徑以供預備和生產之用)。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-125">The `--pathbase` argument sets the app base path for an app run locally with a non-root virtual path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="cc5ed-126">如需詳細資訊，請參閱[應用程式基底路徑](#app-base-path)一節。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-126">For more information, see the [App base path](#app-base-path) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cc5ed-127">不同於提供給 `<base>` 標籤 `href` 的路徑，在傳遞 `--pathbase` 引數值時請勿包含尾端的斜線 (`/`)。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-127">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="cc5ed-128">如果應用程式基底路徑提供於 `<base>` 標籤作為 `<base href="/CoolApp/" />` (包括尾端的斜線)，請將命令列引數值傳遞為 `--pathbase=/CoolApp` (不含尾端的斜線)。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-128">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/" />` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="cc5ed-129">在命令提示字元，於本機執行應用程式時傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-129">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="cc5ed-130">從應用程式目錄，執行：</span><span class="sxs-lookup"><span data-stu-id="cc5ed-130">From the app's directory, execute:</span></span>

  ```console
  dotnet run --pathbase=/<virtual-path>
  ```

* <span data-ttu-id="cc5ed-131">新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-131">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="cc5ed-132">在使用 Visual Studio 偵錯工具執行應用程式時，和從命令提示字元以 `dotnet run` 執行應用程式時，會取得這項設定。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-132">This setting is picked up when running the app with the Visual Studio Debugger and when running the app from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/<virtual-path>"
  ```

* <span data-ttu-id="cc5ed-133">在 Visual Studio 中，在 [屬性] > [偵錯] > [應用程式引數] 中指定引數。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-133">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="cc5ed-134">在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-134">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/<virtual-path>
  ```

### <a name="urls"></a><span data-ttu-id="cc5ed-135">URL</span><span class="sxs-lookup"><span data-stu-id="cc5ed-135">URLs</span></span>

<span data-ttu-id="cc5ed-136">`--urls` 引數表示接聽要求的 IP 位址或主機位址，以及連接埠和通訊協定。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-136">The `--urls` argument indicates the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="cc5ed-137">在命令提示字元，於本機執行應用程式時傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-137">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="cc5ed-138">從應用程式目錄，執行：</span><span class="sxs-lookup"><span data-stu-id="cc5ed-138">From the app's directory, execute:</span></span>

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="cc5ed-139">新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-139">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="cc5ed-140">在使用 Visual Studio 偵錯工具執行應用程式時，和從命令提示字元以 `dotnet run` 執行應用程式時，會取得這項設定。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-140">This setting is picked up when running the app with the Visual Studio Debugger and when running the app from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="cc5ed-141">在 Visual Studio 中，在 [屬性] > [偵錯] > [應用程式引數] 中指定引數。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-141">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="cc5ed-142">在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-142">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deploy-a-client-side-blazor-app"></a><span data-ttu-id="cc5ed-143">部署用戶端 Blazor 應用程式</span><span class="sxs-lookup"><span data-stu-id="cc5ed-143">Deploy a client-side Blazor app</span></span>

<span data-ttu-id="cc5ed-144">使用[用戶端裝載模型](xref:razor-components/hosting-models#client-side-hosting-model)：</span><span class="sxs-lookup"><span data-stu-id="cc5ed-144">With the [client-side hosting model](xref:razor-components/hosting-models#client-side-hosting-model):</span></span>

* <span data-ttu-id="cc5ed-145">Blazor 應用程式、其相依性，和 .NET 執行階段會下載至瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-145">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="cc5ed-146">應用程式會直接在瀏覽器 UI 執行緒上執行。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-146">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="cc5ed-147">支援下列策略之一：</span><span class="sxs-lookup"><span data-stu-id="cc5ed-147">Either of the following strategies is supported:</span></span>
  * <span data-ttu-id="cc5ed-148">Blazor 應用程式由 ASP.NET Core 應用程式提供服務。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-148">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="cc5ed-149">介紹於[使用 ASP.NET Core 的用戶端 Blazor 裝載部署](#client-side-blazor-hosted-deployment-with-aspnet-core)一節。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-149">Covered in the [Client-side Blazor hosted deployment with ASP.NET Core](#client-side-blazor-hosted-deployment-with-aspnet-core) section.</span></span>
  * <span data-ttu-id="cc5ed-150">Blazor 應用程式放在靜態裝載的網頁伺服器或服務上，其中 .NET 不會用來提供服務給 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-150">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="cc5ed-151">其於[用戶端 Blazor 獨立部署](#client-side-blazor-standalone-deployment)一節中介紹。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-151">Covered in the [Client-side Blazor standalone deployment](#client-side-blazor-standalone-deployment) section.</span></span>

### <a name="configure-the-linker"></a><span data-ttu-id="cc5ed-152">設定連結器</span><span class="sxs-lookup"><span data-stu-id="cc5ed-152">Configure the Linker</span></span>

<span data-ttu-id="cc5ed-153">Blazor 在每個組建上執行中繼語言 (IL) 連結，以從輸出組件移除不必要的 IL。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-153">Blazor performs Intermediate Language (IL) linking on each build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="cc5ed-154">您可以控制組建上的組件連結。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-154">You can control assembly linking on build.</span></span> <span data-ttu-id="cc5ed-155">如需詳細資訊，請參閱<xref:host-and-deploy/razor-components/configure-linker>。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-155">For more information, see <xref:host-and-deploy/razor-components/configure-linker>.</span></span>

### <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="cc5ed-156">重寫 URL 以便正確地路由</span><span class="sxs-lookup"><span data-stu-id="cc5ed-156">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="cc5ed-157">用戶端應用程式中的頁面元件傳送要求，並不只是將要求傳送到伺服器端的裝載應用程式那麼簡單。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-157">Routing requests for page components in a client-side app isn't as simple as routing requests to a server-side, hosted app.</span></span> <span data-ttu-id="cc5ed-158">請考慮具有兩個頁面的用戶端應用程式：</span><span class="sxs-lookup"><span data-stu-id="cc5ed-158">Consider a client-side app with two pages:</span></span>

* <span data-ttu-id="cc5ed-159">**_Main.cshtml_** &ndash; 在應用程式根目錄載入，並包含 [關於] 頁面的連結 (`href="About"`)。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-159">**_Main.cshtml_** &ndash; Loads at the root of the app and contains a link to the About page (`href="About"`).</span></span>
* <span data-ttu-id="cc5ed-160">**_About.cshtml_** &ndash; [關於] 頁面。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-160">**_About.cshtml_** &ndash; About page.</span></span>

<span data-ttu-id="cc5ed-161">使用瀏覽器的網址列要求應用程式預設文件時 (例如 `https://www.contoso.com/`)：</span><span class="sxs-lookup"><span data-stu-id="cc5ed-161">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="cc5ed-162">瀏覽器提出要求。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-162">The browser makes a request.</span></span>
1. <span data-ttu-id="cc5ed-163">傳回預設頁面，這通常是 *index.html*。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-163">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="cc5ed-164">*index.html* 啟動載入應用程式。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-164">*index.html* bootstraps the app.</span></span>
1. <span data-ttu-id="cc5ed-165">會載入 Blazor 的路由器 ，且 Razor 主頁面 (*Main.cshtml*) 隨即出現。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-165">Blazor's router loads and the Razor Main page (*Main.cshtml*) is displayed.</span></span>

<span data-ttu-id="cc5ed-166">在主頁面上，選取 [關於] 頁面的連結會載入 [關於] 頁面。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-166">On the Main page, selecting the link to the About page loads the About page.</span></span> <span data-ttu-id="cc5ed-167">選取您用戶端上適用的 [關於] 頁面連結，因為 Blazor 路由器會停止瀏覽器，使其不從網際網路對 `www.contoso.com` 提出針對 `About` 的要求，並自行提供 [關於] 頁面。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-167">Selecting the link to the About page works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the About page itself.</span></span> <span data-ttu-id="cc5ed-168">對於「用戶端應用程式內」內部頁面的所有要求運作方式相同：要求不會對於網際網路上伺服器裝載的資源，觸發以瀏覽器為基礎的要求。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-168">All of the requests for internal pages *within the client-side app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="cc5ed-169">路由器會在內部處理要求。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-169">The router handles the requests internally.</span></span>

<span data-ttu-id="cc5ed-170">如果使用瀏覽器之網址列提出對 `www.contoso.com/About` 的要求，則要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-170">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="cc5ed-171">在應用程式的網際網路主機上沒有這類資源存在，因此會傳回「404 找不到」的回應。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-171">No such resource exists on the app's Internet host, so a *404 Not found* response is returned.</span></span>

<span data-ttu-id="cc5ed-172">因為瀏覽器會提出要求給以網際網路為基礎的主機以取得用戶端頁面，所以網頁伺服器和裝載服務必須針對實際上不在伺服器上資源，將所有要求重新撰寫至 *index.html* 頁面。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-172">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="cc5ed-173">傳回 *index.html* 時，應用程式的用戶端路由器會接管，並以正確的資源回應。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-173">When *index.html* is returned, the app's client-side router takes over and responds with the correct resource.</span></span>

### <a name="app-base-path"></a><span data-ttu-id="cc5ed-174">應用程式基底路徑</span><span class="sxs-lookup"><span data-stu-id="cc5ed-174">App base path</span></span>

<span data-ttu-id="cc5ed-175">應用程式基底路徑是伺服器上的虛擬應用程式根路徑。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-175">The app base path is the virtual app root path on the server.</span></span> <span data-ttu-id="cc5ed-176">例如，對於位於 Contoso 伺服器虛擬資料夾 `/CoolApp/` 的應用程式，能使用 `https://www.contoso.com/CoolApp` 到達它，且具有 `/CoolApp/` 虛擬基底路徑。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-176">For example, an app that resides on the Contoso server in a virtual folder at `/CoolApp/` is reached at `https://www.contoso.com/CoolApp` and has a virtual base path of `/CoolApp/`.</span></span> <span data-ttu-id="cc5ed-177">藉由將應用程式基底路徑設為 `CoolApp/`，應用程式會察覺它虛擬地位於伺服器上何處。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-177">By setting the app base path to `CoolApp/`, the app is made aware of where it virtually resides on the server.</span></span> <span data-ttu-id="cc5ed-178">應用程式可以使用應用程式基底路徑，從不在根目錄中之元件建構相對於應用程式根目錄的 URL。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-178">The app can use the app base path to construct URLs relative to the app root from a component that isn't in the root directory.</span></span> <span data-ttu-id="cc5ed-179">這可讓存在於不同目錄結構層級的元件，建置連結以連至應用程式內所有位置的其他資源。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-179">This allows components that exist at different levels of the directory structure to build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="cc5ed-180">應用程式基底路徑也會用來攔截超連結點擊，其中連結的 `href` 目標是在應用程式基底路徑 URI 空間內&mdash;Blazor 路由器會處理內部瀏覽。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-180">The app base path is also used to intercept hyperlink clicks where the `href` target of the link is within the app base path URI space&mdash;the Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="cc5ed-181">在許多裝載案例中，應用程式之伺服器虛擬路徑是應用程式的根目錄。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-181">In many hosting scenarios, the server's virtual path to the app is the root of the app.</span></span> <span data-ttu-id="cc5ed-182">在這些情況中，應用程式基底路徑是正斜線 (`<base href="/" />`)，這是應用程式的預設組態。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-182">In these cases, the app base path is a forward slash (`<base href="/" />`), which is the default configuration for an app.</span></span> <span data-ttu-id="cc5ed-183">在其他裝載案例中，例如 GitHub Pages 和 IIS 虛擬目錄或子應用程式，應用程式基底路徑必須設定為應用程式的伺服器虛擬路徑。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-183">In other hosting scenarios, such as GitHub Pages and IIS virtual directories or sub-applications, the app base path must be set to the server's virtual path to the app.</span></span> <span data-ttu-id="cc5ed-184">若要設定應用程式的基底路徑，請在 *index.html* 中新增或更新 `<base>` 標籤，這可在 `<head>` 標籤項目內找到。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-184">To set the app's base path, add or update the `<base>` tag in *index.html* found within the `<head>` tag elements.</span></span> <span data-ttu-id="cc5ed-185">將 `href` 屬性值設為 `<virtual-path>/` (尾端的斜線為必要)，其中 `<virtual-path>/` 是應用程式伺服器上的完整虛擬應用程式根路徑。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-185">Set the `href` attribute value to `<virtual-path>/` (the trailing slash is required), where `<virtual-path>/` is the full virtual app root path on the server for the app.</span></span> <span data-ttu-id="cc5ed-186">在上述範例中，虛擬路徑設為 `CoolApp/`: `<base href="CoolApp/" />`。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-186">In the preceding example, the virtual path is set to `CoolApp/`: `<base href="CoolApp/" />`.</span></span>

<span data-ttu-id="cc5ed-187">對於已設定非根目錄虛擬路徑的應用程式 (例如 `<base href="CoolApp/" />`)，應用程式「在本機執行時」無法找到它的資源。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-187">For an app with a non-root virtual path configured (for example, `<base href="CoolApp/" />`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="cc5ed-188">若要在本機開發和測試期間解決這個問題，您可以提供「基底路徑」引數，讓它在執行時符合 `<base>` 標籤的 `href` 值。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-188">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span>

<span data-ttu-id="cc5ed-189">若要在本機執行應用程式時傳遞路徑基底引數與根路徑 (`/`)，請從應用程式的目錄執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="cc5ed-189">To pass the path base argument with the root path (`/`) when running the app locally, execute the following command from the app's directory:</span></span>

```console
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="cc5ed-190">應用程式在 `http://localhost:port/CoolApp` 本機回應。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-190">The app responds locally at `http://localhost:port/CoolApp`.</span></span>

<span data-ttu-id="cc5ed-191">如需詳細資訊，請參閱[路徑基底主機組態值](#path-base)一節。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-191">For more information, see the [path base host configuration value](#path-base) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cc5ed-192">如果應用程式使用[用戶端裝載模型](xref:razor-components/hosting-models#client-side-hosting-model) (根據 **Blazor** 專案範本)，且裝載為 ASP.NET Core 應用程式中的 IIS 子應用程式，請務必停用繼承的 ASP.NET Core 模組處理常式，或是確定 *web.config* 檔案中，根 (父系) 應用程式的 `<handlers>` 區段不由子應用程式繼承。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-192">If an app uses the [client-side hosting model](xref:razor-components/hosting-models#client-side-hosting-model) (based on the **Blazor** project template) and is hosted as an IIS sub-application in an ASP.NET Core app, it's important to disable the inherited ASP.NET Core Module handler or make sure the root (parent) app's `<handlers>` section in the *web.config* file isn't inherited by the sub-app.</span></span>
>
> <span data-ttu-id="cc5ed-193">移除應用程式已發佈 *web.config* 檔案中的處理常式，方法是新增 `<handlers>` 區段到檔案：</span><span class="sxs-lookup"><span data-stu-id="cc5ed-193">Remove the handler in the app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>
>
> ```xml
> <handlers>
>   <remove name="aspNetCore" />
> </handlers>
> ```
>
> <span data-ttu-id="cc5ed-194">或者，使用 `<location>` 項目，並將 `inheritInChildApplications` 設為 `false`，以停用根 (父系) 應用程式的 `<system.webServer>` 區段繼承：</span><span class="sxs-lookup"><span data-stu-id="cc5ed-194">Alternatively, disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>
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
> <span data-ttu-id="cc5ed-195">除了如本節中所述設定應用程式的基底路徑，也會執行移除處理常式或停用繼承。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-195">Removing the handler or disabling inheritance is performed in addition to configuring the app's base path as described in this section.</span></span> <span data-ttu-id="cc5ed-196">在應用程式的 *index.html* 檔案，將應用程式基底路徑設為在 IIS 中設定子應用程式時使用的 IIS 別名。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-196">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

### <a name="client-side-blazor-hosted-deployment-with-aspnet-core"></a><span data-ttu-id="cc5ed-197">使用 ASP.NET Core 的用戶端 Blazor 裝載部署</span><span class="sxs-lookup"><span data-stu-id="cc5ed-197">Client-side Blazor hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="cc5ed-198">「裝載部署」會將用戶端 Blazor 應用程式，從伺服器上執行的 [ASP.NET Core 應用程式](xref:index)，提供給瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-198">A *hosted deployment* serves the client-side Blazor app to browsers from an [ASP.NET Core app](xref:index) that runs on a server.</span></span>

<span data-ttu-id="cc5ed-199">Blazor 應用程式隨附於發佈輸出中的 ASP.NET Core 應用程式，以便兩個應用程式會一起部署。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-199">The Blazor app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="cc5ed-200">需要有能夠裝載 ASP.NET Core 應用程式的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-200">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="cc5ed-201">對於裝載部署，Visual Studio 包含 **Blazor (已裝載 ASP.NET Core)** 專案範本 (使用 [dotnet new](/dotnet/core/tools/dotnet-new) 命令時的 `blazorhosted` 範本)。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-201">For a hosted deployment, Visual Studio includes the **Blazor (ASP.NET Core hosted)** project template (`blazorhosted` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<span data-ttu-id="cc5ed-202">如需 ASP.NET Core 應用程式裝載和部署的詳細資訊，請參閱 <xref:host-and-deploy/index>。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-202">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="cc5ed-203">如需部署至 Azure App Service 的資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="cc5ed-203">For information on deploying to Azure App Service, see the following topics:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>

<span data-ttu-id="cc5ed-204">了解如何使用 Visual Studio 將 ASP.NET Core 應用程式發行到 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-204">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

### <a name="client-side-blazor-standalone-deployment"></a><span data-ttu-id="cc5ed-205">用戶端 Blazor 獨立部署</span><span class="sxs-lookup"><span data-stu-id="cc5ed-205">Client-side Blazor standalone deployment</span></span>

<span data-ttu-id="cc5ed-206">「獨立部署」會以一組直接由用戶端要求的靜態檔案來提供服務給用戶端 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-206">A *standalone deployment* serves the client-side Blazor app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="cc5ed-207">網頁伺服器不用來提供服務給 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-207">A web server isn't used to serve the Blazor app.</span></span>

#### <a name="client-side-blazor-standalone-hosting-with-iis"></a><span data-ttu-id="cc5ed-208">使用 IIS 的用戶端 Blazor 獨立裝載</span><span class="sxs-lookup"><span data-stu-id="cc5ed-208">Client-side Blazor standalone hosting with IIS</span></span>

<span data-ttu-id="cc5ed-209">IIS 是足以支援 Blazor 應用程式的靜態檔案伺服器。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-209">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="cc5ed-210">若要設定 IIS 來裝載 Blazor，請參閱[在 IIS 上建置靜態網站](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-210">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="cc5ed-211">已發佈的資產建立於 \\bin\\Release\\&lt;目標 Framework&gt;\\publish 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-211">Published assets are created in the *\\bin\\Release\\&lt;target-framework&gt;\\publish* folder.</span></span> <span data-ttu-id="cc5ed-212">在網頁伺服器或裝載服務上，裝載 *publish* 資料夾的內容。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-212">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

<span data-ttu-id="cc5ed-213">**web.config**</span><span class="sxs-lookup"><span data-stu-id="cc5ed-213">**web.config**</span></span>

<span data-ttu-id="cc5ed-214">發佈 Blazor 專案時，會建立 *web.config* 檔案，並使用下列 IIS 組態：</span><span class="sxs-lookup"><span data-stu-id="cc5ed-214">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="cc5ed-215">針對下列副檔名設定 MIME 類型：</span><span class="sxs-lookup"><span data-stu-id="cc5ed-215">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="cc5ed-216">*\*.dll*：`application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="cc5ed-216">*\*.dll*: `application/octet-stream`</span></span>
  * <span data-ttu-id="cc5ed-217">*\*.json*：`application/json`</span><span class="sxs-lookup"><span data-stu-id="cc5ed-217">*\*.json*: `application/json`</span></span>
  * <span data-ttu-id="cc5ed-218">*\*.wasm*：`application/wasm`</span><span class="sxs-lookup"><span data-stu-id="cc5ed-218">*\*.wasm*: `application/wasm`</span></span>
  * <span data-ttu-id="cc5ed-219">*\*.woff*：`application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="cc5ed-219">*\*.woff*: `application/font-woff`</span></span>
  * <span data-ttu-id="cc5ed-220">*\*.woff2*：`application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="cc5ed-220">*\*.woff2*: `application/font-woff`</span></span>
* <span data-ttu-id="cc5ed-221">針對下列 MIME 類型會啟用 HTTP 壓縮：</span><span class="sxs-lookup"><span data-stu-id="cc5ed-221">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="cc5ed-222">建立 URL Rewrite Module 規則：</span><span class="sxs-lookup"><span data-stu-id="cc5ed-222">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="cc5ed-223">提供應用程式靜態資產所在的子目錄 (&lt;組件名稱&gt;\\dist\\&lt;要求的路徑&gt;)。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-223">Serve the sub-directory where the app's static assets reside (*&lt;assembly_name&gt;\\dist\\&lt;path_requested&gt;*).</span></span>
  * <span data-ttu-id="cc5ed-224">建立 SPA 後援路由，讓非檔案資產要求重新導向至其靜態資產資料夾中的應用程式預設文件 (&lt;組件名稱&gt;\\dist\\index.html)。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-224">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*&lt;assembly_name&gt;\\dist\\index.html*).</span></span>

<span data-ttu-id="cc5ed-225">**安裝 URL Rewrite Module**</span><span class="sxs-lookup"><span data-stu-id="cc5ed-225">**Install the URL Rewrite Module**</span></span>

<span data-ttu-id="cc5ed-226">需要 [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite)，才可重寫 URL。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-226">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="cc5ed-227">預設不會安裝模組，且無法用來安裝作為網頁伺服器 (IIS) 角色服務功能。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-227">The module isn't installed by default, and it isn't available for install as an Web Server (IIS) role service feature.</span></span> <span data-ttu-id="cc5ed-228">必須從 IIS 網站下載模組。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-228">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="cc5ed-229">請使用 Web Platform Installer 安裝模組：</span><span class="sxs-lookup"><span data-stu-id="cc5ed-229">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="cc5ed-230">在本機上，巡覽至 [URL Rewrite Module 下載頁面](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads)。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-230">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="cc5ed-231">如需英文版，請選取 [WebPI] 下載 WebPI 安裝程式。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-231">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="cc5ed-232">如需其他語言，請選取適當的伺服器架構 (x86 x64) 來下載安裝程式。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-232">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="cc5ed-233">將安裝程式複製到伺服器。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-233">Copy the installer to the server.</span></span> <span data-ttu-id="cc5ed-234">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-234">Run the installer.</span></span> <span data-ttu-id="cc5ed-235">選取 [安裝] 按鈕，並接受授權條款。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-235">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="cc5ed-236">安裝完成之後，不需要重新啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-236">A server restart isn't required after the install completes.</span></span>

<span data-ttu-id="cc5ed-237">**設定網站**</span><span class="sxs-lookup"><span data-stu-id="cc5ed-237">**Configure the website**</span></span>

<span data-ttu-id="cc5ed-238">將網站的 [實體路徑] 設為應用程式的資料夾。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-238">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="cc5ed-239">此資料夾包含：</span><span class="sxs-lookup"><span data-stu-id="cc5ed-239">The folder contains:</span></span>

* <span data-ttu-id="cc5ed-240">IIS 用來設定網站的 *web.config* 檔，包括必要的重新導向規則和檔案內容類型。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-240">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="cc5ed-241">應用程式的靜態資產資料夾。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-241">The app's static asset folder.</span></span>

<span data-ttu-id="cc5ed-242">**疑難排解**</span><span class="sxs-lookup"><span data-stu-id="cc5ed-242">**Troubleshooting**</span></span>

<span data-ttu-id="cc5ed-243">如果收到「500 內部伺服器錯誤」，且 IIS 管理員在嘗試存取網站的組態時擲回錯誤，請確認是否已安裝 URL Rewrite Module。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-243">If a *500 Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="cc5ed-244">未安裝此模組時，IIS 無法剖析 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-244">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="cc5ed-245">這導致 IIS 管理員無法載入網站的組態，且網站無法提供 Blazor 的靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-245">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="cc5ed-246">如需針對部署至 IIS 進行疑難排解的詳細資訊，請參閱 <xref:host-and-deploy/iis/troubleshoot>。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-246">For more information on troubleshooting deployments to IIS, see <xref:host-and-deploy/iis/troubleshoot>.</span></span>

#### <a name="client-side-blazor-standalone-hosting-with-nginx"></a><span data-ttu-id="cc5ed-247">使用 Nginx 的用戶端 Blazor 獨立裝載</span><span class="sxs-lookup"><span data-stu-id="cc5ed-247">Client-side Blazor standalone hosting with Nginx</span></span>

<span data-ttu-id="cc5ed-248">下列 *nginx.conf* 檔案已簡化，示範如何將 Nginx 設定為每當找不到磁碟上的對應檔案時便傳送 *Index.html* 檔案。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-248">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *Index.html* file whenever it can't find a corresponding file on disk.</span></span>

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

<span data-ttu-id="cc5ed-249">如需生產環境 Nginx 網頁伺服器組態的詳細資訊，請參閱[建立 NGINX Plus 和 NGINX 組態檔](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/)。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-249">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

#### <a name="client-side-blazor-standalone-hosting-with-nginx-in-docker"></a><span data-ttu-id="cc5ed-250">在 Docker 中使用 Nginx 的用戶端 Blazor 獨立裝載</span><span class="sxs-lookup"><span data-stu-id="cc5ed-250">Client-side Blazor standalone hosting with Nginx in Docker</span></span>

<span data-ttu-id="cc5ed-251">若要在 Docker 中使用 Nginx 裝載 Blazor，請設定 Dockerfile 以使用以 Alpine 為基礎的 Nginx 映像。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-251">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="cc5ed-252">更新 Dockerfile，將 *nginx.config* 檔案複製到容器內。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-252">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="cc5ed-253">如下列範例所示，新增一行至 Dockerfile：</span><span class="sxs-lookup"><span data-stu-id="cc5ed-253">Add one line to the Dockerfile, as shown in the following example:</span></span>

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

#### <a name="client-side-blazor-standalone-hosting-with-github-pages"></a><span data-ttu-id="cc5ed-254">使用 GitHub Pages 的用戶端 Blazor 獨立裝載</span><span class="sxs-lookup"><span data-stu-id="cc5ed-254">Client-side Blazor standalone hosting with GitHub Pages</span></span>

<span data-ttu-id="cc5ed-255">若要處理 URL 重寫，請新增 *404.html* 檔，以及處理將要求重新導向至 *index.html* 頁面的指令碼。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-255">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="cc5ed-256">如需社群所提供的範例實作，請參閱 [GitHub Pages 的單一頁面應用程式](http://spa-github-pages.rafrex.com/) (GitHub 上的 [rafrex/spa-github-pages](https://github.com/rafrex/spa-github-pages#readme))。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-256">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](http://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="cc5ed-257">使用社群方法的範例可在 [GitHub 上的 blazor-demo/blazor-demo.github.io](https://github.com/blazor-demo/blazor-demo.github.io) ([即時網站](https://blazor-demo.github.io/)) 看到。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-257">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="cc5ed-258">使用專案網站，而非組織網站時，請在 *index.html* 中新增或更新 `<base>` 標籤。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-258">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="cc5ed-259">將 `href` 屬性值設為 `<repository-name>/`，其中 `<repository-name>/` 是 GitHub 存放庫名稱。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-259">Set the `href` attribute value to `<repository-name>/`, where `<repository-name>/` is the GitHub repository name.</span></span>

## <a name="deploy-a-server-side-razor-components-app"></a><span data-ttu-id="cc5ed-260">部署伺服器端 Razor 元件應用程式</span><span class="sxs-lookup"><span data-stu-id="cc5ed-260">Deploy a server-side Razor Components app</span></span>

<span data-ttu-id="cc5ed-261">使用[伺服器端裝載模型](xref:razor-components/hosting-models#server-side-hosting-model)，Razor 元件會從 ASP.NET Core 應用程式內在伺服器上執行。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-261">With the [server-side hosting model](xref:razor-components/hosting-models#server-side-hosting-model), Razor Components is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="cc5ed-262">UI 更新、事件處理及 JavaScript 呼叫會透過 SignalR 連線處理。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-262">UI updates, event handling, and JavaScript calls are handled over a SignalR connection.</span></span>

<span data-ttu-id="cc5ed-263">應用程式隨附於發佈輸出中的 ASP.NET Core 應用程式，以便兩個應用程式會一起部署。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-263">The app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="cc5ed-264">需要有能夠裝載 ASP.NET Core 應用程式的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-264">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="cc5ed-265">對於伺服器端部署，Visual Studio 會包含 **Razor Components**  專案範本 (在使用 [dotnet new](/dotnet/core/tools/dotnet-new) 命令時為 `razorcomponents` 範本)。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-265">For a server-side deployment, Visual Studio includes the **Razor Components** project template (`razorcomponents` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

<span data-ttu-id="cc5ed-266">發佈 ASP.NET Core 應用程式時，Razor 元件應用程式包含在發佈輸出中，以便 ASP.NET Core 應用程式和 Razor 元件應用程式可以一起部署。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-266">When the ASP.NET Core app is published, the Razor Components app is included in the published output so that the ASP.NET Core app and the Razor Components app can be deployed together.</span></span> <span data-ttu-id="cc5ed-267">如需 ASP.NET Core 應用程式裝載和部署的詳細資訊，請參閱 <xref:host-and-deploy/index>。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-267">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="cc5ed-268">如需部署至 Azure App Service 的資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="cc5ed-268">For information on deploying to Azure App Service, see the following topics:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>

<span data-ttu-id="cc5ed-269">了解如何使用 Visual Studio 將 ASP.NET Core 應用程式發行到 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="cc5ed-269">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>
