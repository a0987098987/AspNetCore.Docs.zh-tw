---
title: 裝載和部署 Blazor 用戶端
author: guardrex
description: 了解如何使用 ASP.NET Core、內容傳遞網路 (CDN)、檔案伺服器和 GitHub Pages 來裝載和部署 Blazor 應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/13/2019
uid: host-and-deploy/blazor/client-side
ms.openlocfilehash: ea8ece266809913e32ac212bc55cb3c2499c234f
ms.sourcegitcommit: ccbb84ae307a5bc527441d3d509c20b5c1edde05
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/19/2019
ms.locfileid: "65874979"
---
# <a name="host-and-deploy-blazor-client-side"></a><span data-ttu-id="d2a48-103">裝載和部署 Blazor 用戶端</span><span class="sxs-lookup"><span data-stu-id="d2a48-103">Host and deploy Blazor client-side</span></span>

<span data-ttu-id="d2a48-104">作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="d2a48-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="d2a48-105">主機組態值</span><span class="sxs-lookup"><span data-stu-id="d2a48-105">Host configuration values</span></span>

<span data-ttu-id="d2a48-106">使用[用戶端裝載模型](xref:blazor/hosting-models#client-side)的 Blazor 應用程式，可以在開發環境中，在執行時接受下列主機組態值作為命令列引數。</span><span class="sxs-lookup"><span data-stu-id="d2a48-106">Blazor apps that use the [client-side hosting model](xref:blazor/hosting-models#client-side) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="d2a48-107">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="d2a48-107">Content Root</span></span>

<span data-ttu-id="d2a48-108">`--contentroot` 引數設定包含應用程式內容檔案的目錄絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="d2a48-108">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files.</span></span> <span data-ttu-id="d2a48-109">在下列範例中，`/content-root-path` 是應用程式的內容根路徑。</span><span class="sxs-lookup"><span data-stu-id="d2a48-109">In the following examples, `/content-root-path` is the app's content root path.</span></span>

* <span data-ttu-id="d2a48-110">在命令提示字元，於本機執行應用程式時傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="d2a48-110">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="d2a48-111">從應用程式目錄，執行：</span><span class="sxs-lookup"><span data-stu-id="d2a48-111">From the app's directory, execute:</span></span>

  ```console
  dotnet run --contentroot=/content-root-path
  ```

* <span data-ttu-id="d2a48-112">新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="d2a48-112">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="d2a48-113">此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。</span><span class="sxs-lookup"><span data-stu-id="d2a48-113">This setting is used when the app is run with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* <span data-ttu-id="d2a48-114">在 Visual Studio 中，在 [屬性] > [偵錯] > [應用程式引數] 中指定引數。</span><span class="sxs-lookup"><span data-stu-id="d2a48-114">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="d2a48-115">在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="d2a48-115">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a><span data-ttu-id="d2a48-116">路徑基底</span><span class="sxs-lookup"><span data-stu-id="d2a48-116">Path base</span></span>

<span data-ttu-id="d2a48-117">`--pathbase` 引數會以非根虛擬路徑，為本機執行的應用程式設定應用程式基底路徑 (`<base>` 標籤 `href` 會設為非 `/` 的路徑以供預備和生產之用)。</span><span class="sxs-lookup"><span data-stu-id="d2a48-117">The `--pathbase` argument sets the app base path for an app run locally with a non-root virtual path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="d2a48-118">在下列範例中，`/virtual-path` 是應用程式的路徑基底。</span><span class="sxs-lookup"><span data-stu-id="d2a48-118">In the following examples, `/virtual-path` is the app's path base.</span></span> <span data-ttu-id="d2a48-119">如需詳細資訊，請參閱[應用程式基底路徑](#app-base-path)一節。</span><span class="sxs-lookup"><span data-stu-id="d2a48-119">For more information, see the [App base path](#app-base-path) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d2a48-120">不同於提供給 `<base>` 標籤 `href` 的路徑，在傳遞 `--pathbase` 引數值時請勿包含尾端的斜線 (`/`)。</span><span class="sxs-lookup"><span data-stu-id="d2a48-120">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="d2a48-121">如果應用程式基底路徑提供於 `<base>` 標籤作為 `<base href="/CoolApp/">` (包括尾端的斜線)，請將命令列引數值傳遞為 `--pathbase=/CoolApp` (不含尾端的斜線)。</span><span class="sxs-lookup"><span data-stu-id="d2a48-121">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/">` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="d2a48-122">在命令提示字元，於本機執行應用程式時傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="d2a48-122">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="d2a48-123">從應用程式目錄，執行：</span><span class="sxs-lookup"><span data-stu-id="d2a48-123">From the app's directory, execute:</span></span>

  ```console
  dotnet run --pathbase=/virtual-path
  ```

* <span data-ttu-id="d2a48-124">新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="d2a48-124">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="d2a48-125">此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。</span><span class="sxs-lookup"><span data-stu-id="d2a48-125">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/virtual-path"
  ```

* <span data-ttu-id="d2a48-126">在 Visual Studio 中，在 [屬性] > [偵錯] > [應用程式引數] 中指定引數。</span><span class="sxs-lookup"><span data-stu-id="d2a48-126">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="d2a48-127">在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="d2a48-127">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/virtual-path
  ```

### <a name="urls"></a><span data-ttu-id="d2a48-128">URL</span><span class="sxs-lookup"><span data-stu-id="d2a48-128">URLs</span></span>

<span data-ttu-id="d2a48-129">`--urls` 引數會搭配要用來接聽要求的連接埠和通訊協定，設定 IP 位址或主機位址。</span><span class="sxs-lookup"><span data-stu-id="d2a48-129">The `--urls` argument sets the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="d2a48-130">在命令提示字元，於本機執行應用程式時傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="d2a48-130">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="d2a48-131">從應用程式目錄，執行：</span><span class="sxs-lookup"><span data-stu-id="d2a48-131">From the app's directory, execute:</span></span>

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="d2a48-132">新增項目至應用程式 **IIS Express** 設定檔中的 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="d2a48-132">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="d2a48-133">此設定的使用時機為當應用程式是搭配 Visual Studio 偵錯工具並以 `dotnet run` 從命令提示字元執行的情況。</span><span class="sxs-lookup"><span data-stu-id="d2a48-133">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="d2a48-134">在 Visual Studio 中，在 [屬性] > [偵錯] > [應用程式引數] 中指定引數。</span><span class="sxs-lookup"><span data-stu-id="d2a48-134">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="d2a48-135">在 Visual Studio 屬性頁中設定引數，會將引數新增至 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="d2a48-135">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deployment"></a><span data-ttu-id="d2a48-136">部署</span><span class="sxs-lookup"><span data-stu-id="d2a48-136">Deployment</span></span>

<span data-ttu-id="d2a48-137">使用[用戶端裝載模型](xref:blazor/hosting-models#client-side)：</span><span class="sxs-lookup"><span data-stu-id="d2a48-137">With the [client-side hosting model](xref:blazor/hosting-models#client-side):</span></span>

* <span data-ttu-id="d2a48-138">Blazor 應用程式、其相依性，和 .NET 執行階段會下載至瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="d2a48-138">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="d2a48-139">應用程式會直接在瀏覽器 UI 執行緒上執行。</span><span class="sxs-lookup"><span data-stu-id="d2a48-139">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="d2a48-140">支援下列策略之一：</span><span class="sxs-lookup"><span data-stu-id="d2a48-140">Either of the following strategies is supported:</span></span>
  * <span data-ttu-id="d2a48-141">Blazor 應用程式由 ASP.NET Core 應用程式提供服務。</span><span class="sxs-lookup"><span data-stu-id="d2a48-141">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="d2a48-142">此策略已於[搭配 ASP.NET Core 的已裝載部署](#hosted-deployment-with-aspnet-core)一節中涵蓋。</span><span class="sxs-lookup"><span data-stu-id="d2a48-142">This strategy is covered in the [Hosted deployment with ASP.NET Core](#hosted-deployment-with-aspnet-core) section.</span></span>
  * <span data-ttu-id="d2a48-143">Blazor 應用程式放在靜態裝載的網頁伺服器或服務上，其中 .NET 不會用來提供服務給 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d2a48-143">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="d2a48-144">此策略已於[獨立部署](#standalone-deployment)一節中涵蓋。</span><span class="sxs-lookup"><span data-stu-id="d2a48-144">This strategy is covered in the [Standalone deployment](#standalone-deployment) section.</span></span>

## <a name="configure-the-linker"></a><span data-ttu-id="d2a48-145">設定連結器</span><span class="sxs-lookup"><span data-stu-id="d2a48-145">Configure the Linker</span></span>

<span data-ttu-id="d2a48-146">Blazor 在每個組建上執行中繼語言 (IL) 連結，以從輸出組件移除不必要的 IL。</span><span class="sxs-lookup"><span data-stu-id="d2a48-146">Blazor performs Intermediate Language (IL) linking on each build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="d2a48-147">組件連結可在組建上控制。</span><span class="sxs-lookup"><span data-stu-id="d2a48-147">Assembly linking can be controlled on build.</span></span> <span data-ttu-id="d2a48-148">如需詳細資訊，請參閱<xref:host-and-deploy/blazor/configure-linker>。</span><span class="sxs-lookup"><span data-stu-id="d2a48-148">For more information, see <xref:host-and-deploy/blazor/configure-linker>.</span></span>

## <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="d2a48-149">重寫 URL 以便正確地路由</span><span class="sxs-lookup"><span data-stu-id="d2a48-149">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="d2a48-150">用戶端應用程式中的頁面元件傳送要求，並不只是將要求傳送到伺服器端的裝載應用程式那麼簡單。</span><span class="sxs-lookup"><span data-stu-id="d2a48-150">Routing requests for page components in a client-side app isn't as simple as routing requests to a server-side, hosted app.</span></span> <span data-ttu-id="d2a48-151">請考慮具有兩個頁面的用戶端應用程式：</span><span class="sxs-lookup"><span data-stu-id="d2a48-151">Consider a client-side app with two pages:</span></span>

* <span data-ttu-id="d2a48-152">**_Main.razor** &ndash; 載入應用程式根目錄，同時包含 [關於] 頁面 (`href="About"`) 的連結。</span><span class="sxs-lookup"><span data-stu-id="d2a48-152">**_Main.razor** &ndash; Loads at the root of the app and contains a link to the About page (`href="About"`).</span></span>
* <span data-ttu-id="d2a48-153">**_About.razor** &ndash; [關於] 頁面。</span><span class="sxs-lookup"><span data-stu-id="d2a48-153">**_About.razor** &ndash; About page.</span></span>

<span data-ttu-id="d2a48-154">使用瀏覽器的網址列要求應用程式預設文件時 (例如 `https://www.contoso.com/`)：</span><span class="sxs-lookup"><span data-stu-id="d2a48-154">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="d2a48-155">瀏覽器提出要求。</span><span class="sxs-lookup"><span data-stu-id="d2a48-155">The browser makes a request.</span></span>
1. <span data-ttu-id="d2a48-156">傳回預設頁面，這通常是 *index.html*。</span><span class="sxs-lookup"><span data-stu-id="d2a48-156">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="d2a48-157">*index.html* 啟動載入應用程式。</span><span class="sxs-lookup"><span data-stu-id="d2a48-157">*index.html* bootstraps the app.</span></span>
1. <span data-ttu-id="d2a48-158">會載入 Blazor 的路由器，同時會顯示 Razor 主頁面 (*Main.razor*)。</span><span class="sxs-lookup"><span data-stu-id="d2a48-158">Blazor's router loads, and the Razor Main page (*Main.razor*) is displayed.</span></span>

<span data-ttu-id="d2a48-159">在主頁面上，選取 [關於] 頁面的連結會載入 [關於] 頁面。</span><span class="sxs-lookup"><span data-stu-id="d2a48-159">On the Main page, selecting the link to the About page loads the About page.</span></span> <span data-ttu-id="d2a48-160">選取您用戶端上適用的 [關於] 頁面連結，因為 Blazor 路由器會停止瀏覽器，使其不從網際網路對 `www.contoso.com` 提出針對 `About` 的要求，並自行提供 [關於] 頁面。</span><span class="sxs-lookup"><span data-stu-id="d2a48-160">Selecting the link to the About page works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the About page itself.</span></span> <span data-ttu-id="d2a48-161">對於「用戶端應用程式內」內部頁面的所有要求運作方式相同：要求不會對於網際網路上伺服器裝載的資源，觸發以瀏覽器為基礎的要求。</span><span class="sxs-lookup"><span data-stu-id="d2a48-161">All of the requests for internal pages *within the client-side app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="d2a48-162">路由器會在內部處理要求。</span><span class="sxs-lookup"><span data-stu-id="d2a48-162">The router handles the requests internally.</span></span>

<span data-ttu-id="d2a48-163">如果使用瀏覽器之網址列提出對 `www.contoso.com/About` 的要求，則要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="d2a48-163">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="d2a48-164">在應用程式的網際網路主機上沒有這類資源存在，因此會傳回「404 - 找不到」的回應。</span><span class="sxs-lookup"><span data-stu-id="d2a48-164">No such resource exists on the app's Internet host, so a *404 - Not Found* response is returned.</span></span>

<span data-ttu-id="d2a48-165">因為瀏覽器會提出要求給以網際網路為基礎的主機以取得用戶端頁面，所以網頁伺服器和裝載服務必須針對實際上不在伺服器上資源，將所有要求重新撰寫至 *index.html* 頁面。</span><span class="sxs-lookup"><span data-stu-id="d2a48-165">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="d2a48-166">傳回 *index.html* 時，應用程式的用戶端路由器會接管，並以正確的資源回應。</span><span class="sxs-lookup"><span data-stu-id="d2a48-166">When *index.html* is returned, the app's client-side router takes over and responds with the correct resource.</span></span>

## <a name="app-base-path"></a><span data-ttu-id="d2a48-167">應用程式基底路徑</span><span class="sxs-lookup"><span data-stu-id="d2a48-167">App base path</span></span>

<span data-ttu-id="d2a48-168">「應用程式基底路徑」是伺服器上的虛擬應用程式根路徑。</span><span class="sxs-lookup"><span data-stu-id="d2a48-168">The *app base path* is the virtual app root path on the server.</span></span> <span data-ttu-id="d2a48-169">例如，對於位於 Contoso 伺服器虛擬資料夾 `/CoolApp/` 的應用程式，能使用 `https://www.contoso.com/CoolApp` 到達它，且具有 `/CoolApp/` 虛擬基底路徑。</span><span class="sxs-lookup"><span data-stu-id="d2a48-169">For example, an app that resides on the Contoso server in a virtual folder at `/CoolApp/` is reached at `https://www.contoso.com/CoolApp` and has a virtual base path of `/CoolApp/`.</span></span> <span data-ttu-id="d2a48-170">對虛擬路徑 (`<base href="/CoolApp/">`) 設定應用程式的基底路徑，應用程式即可得知其虛擬位於伺服器的何處。</span><span class="sxs-lookup"><span data-stu-id="d2a48-170">By setting the app base path to the virtual path (`<base href="/CoolApp/">`), the app is made aware of where it virtually resides on the server.</span></span> <span data-ttu-id="d2a48-171">應用程式可以使用應用程式基底路徑，從不在根目錄中之元件建構相對於應用程式根目錄的 URL。</span><span class="sxs-lookup"><span data-stu-id="d2a48-171">The app can use the app base path to construct URLs relative to the app root from a component that isn't in the root directory.</span></span> <span data-ttu-id="d2a48-172">這可讓存在於不同目錄結構層級的元件，建置連結以連至應用程式內所有位置的其他資源。</span><span class="sxs-lookup"><span data-stu-id="d2a48-172">This allows components that exist at different levels of the directory structure to build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="d2a48-173">應用程式基底路徑也會用來攔截超連結點擊，其中連結的 `href` 目標是在應用程式基底路徑 URI 空間內&mdash;Blazor 路由器會處理內部瀏覽。</span><span class="sxs-lookup"><span data-stu-id="d2a48-173">The app base path is also used to intercept hyperlink clicks where the `href` target of the link is within the app base path URI space&mdash;the Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="d2a48-174">在許多裝載案例中，應用程式之伺服器虛擬路徑是應用程式的根目錄。</span><span class="sxs-lookup"><span data-stu-id="d2a48-174">In many hosting scenarios, the server's virtual path to the app is the root of the app.</span></span> <span data-ttu-id="d2a48-175">在這些情況中，應用程式基底路徑是正斜線 (`<base href="/" />`)，這是應用程式的預設組態。</span><span class="sxs-lookup"><span data-stu-id="d2a48-175">In these cases, the app base path is a forward slash (`<base href="/" />`), which is the default configuration for an app.</span></span> <span data-ttu-id="d2a48-176">在其他裝載案例中，例如 GitHub Pages 和 IIS 虛擬目錄或子應用程式，應用程式基底路徑必須設定為應用程式的伺服器虛擬路徑。</span><span class="sxs-lookup"><span data-stu-id="d2a48-176">In other hosting scenarios, such as GitHub Pages and IIS virtual directories or sub-applications, the app base path must be set to the server's virtual path to the app.</span></span> <span data-ttu-id="d2a48-177">若要設定應用程式的基底路徑，請更新 *wwwroot/index.html* 檔案 `<head>` 標籤項目內的 `<base>` 標籤。</span><span class="sxs-lookup"><span data-stu-id="d2a48-177">To set the app's base path, update the `<base>` tag within the `<head>` tag elements of the *wwwroot/index.html* file.</span></span> <span data-ttu-id="d2a48-178">將 `href` 屬性值設為 `/virtual-path/` (尾端的斜線為必要)，其中 `/virtual-path/` 是應用程式伺服器上的完整虛擬應用程式根路徑。</span><span class="sxs-lookup"><span data-stu-id="d2a48-178">Set the `href` attribute value to `/virtual-path/` (the trailing slash is required), where `/virtual-path/` is the full virtual app root path on the server for the app.</span></span> <span data-ttu-id="d2a48-179">在上述範例中，虛擬路徑設為 `/CoolApp/`: `<base href="/CoolApp/">`。</span><span class="sxs-lookup"><span data-stu-id="d2a48-179">In the preceding example, the virtual path is set to `/CoolApp/`: `<base href="/CoolApp/">`.</span></span>

<span data-ttu-id="d2a48-180">對於已設定非根目錄虛擬路徑的應用程式 (例如 `<base href="/CoolApp/">`)，應用程式「在本機執行時」無法找到它的資源。</span><span class="sxs-lookup"><span data-stu-id="d2a48-180">For an app with a non-root virtual path configured (for example, `<base href="/CoolApp/">`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="d2a48-181">若要在本機開發和測試期間解決這個問題，您可以提供「基底路徑」引數，讓它在執行時符合 `<base>` 標籤的 `href` 值。</span><span class="sxs-lookup"><span data-stu-id="d2a48-181">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span>

<span data-ttu-id="d2a48-182">若要在本機執行應用程式期間，傳遞路徑基底引數與根路徑 (`/`)，請從應用程式的目錄執行 `dotnet run` 命令，同時設定 `--pathbase` 選項：</span><span class="sxs-lookup"><span data-stu-id="d2a48-182">To pass the path base argument with the root path (`/`) when running the app locally, execute the `dotnet run` command from the app's directory with the `--pathbase` option:</span></span>

```console
dotnet run --pathbase=/{Virtual Path (no trailing slash)}
```

<span data-ttu-id="d2a48-183">若是虛擬基底路徑為 `/CoolApp/` (`<base href="/CoolApp/">`) 的應用程式，命令為：</span><span class="sxs-lookup"><span data-stu-id="d2a48-183">For an app with a virtual base path of `/CoolApp/` (`<base href="/CoolApp/">`), the command is:</span></span>

```console
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="d2a48-184">應用程式在 `http://localhost:port/CoolApp` 本機回應。</span><span class="sxs-lookup"><span data-stu-id="d2a48-184">The app responds locally at `http://localhost:port/CoolApp`.</span></span>

<span data-ttu-id="d2a48-185">如需詳細資訊，請參閱[路徑基底主機設定值](#path-base)一節。</span><span class="sxs-lookup"><span data-stu-id="d2a48-185">For more information, see the section on the [path base host configuration value](#path-base).</span></span>

<span data-ttu-id="d2a48-186">如果應用程式使用[用戶端裝載模型](xref:blazor/hosting-models#client-side) (根據 **Blazor** 專案範本、使用 [dotnet new](/dotnet/core/tools/dotnet-new) 命令時的 `blazor` 範本)，且裝載為 ASP.NET Core 應用程式中的 IIS 子應用程式，請務必停用繼承的 ASP.NET Core 模組處理常式，或是確定子應用程式並未繼承 *web.config* 檔案中根 (父系) 應用程式的 `<handlers>` 區段。</span><span class="sxs-lookup"><span data-stu-id="d2a48-186">If an app uses the [client-side hosting model](xref:blazor/hosting-models#client-side) (based on the **Blazor** project template; the `blazor` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command) and is hosted as an IIS sub-application in an ASP.NET Core app, it's important to disable the inherited ASP.NET Core Module handler or make sure the root (parent) app's `<handlers>` section in the *web.config* file isn't inherited by the sub-app.</span></span>

<span data-ttu-id="d2a48-187">移除應用程式已發佈 *web.config* 檔案中的處理常式，方法是新增 `<handlers>` 區段到檔案：</span><span class="sxs-lookup"><span data-stu-id="d2a48-187">Remove the handler in the app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>

```xml
<handlers>
  <remove name="aspNetCore" />
</handlers>
```

<span data-ttu-id="d2a48-188">或者，使用 `<location>` 項目，並將 `inheritInChildApplications` 設為 `false`，以停用根 (父系) 應用程式的 `<system.webServer>` 區段繼承：</span><span class="sxs-lookup"><span data-stu-id="d2a48-188">Alternatively, disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>

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

<span data-ttu-id="d2a48-189">除了如本節中所述設定應用程式的基底路徑，也會執行移除處理常式或停用繼承。</span><span class="sxs-lookup"><span data-stu-id="d2a48-189">Removing the handler or disabling inheritance is performed in addition to configuring the app's base path as described in this section.</span></span> <span data-ttu-id="d2a48-190">在應用程式的 *index.html* 檔案，將應用程式基底路徑設為在 IIS 中設定子應用程式時使用的 IIS 別名。</span><span class="sxs-lookup"><span data-stu-id="d2a48-190">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

## <a name="hosted-deployment-with-aspnet-core"></a><span data-ttu-id="d2a48-191">搭配 ASP.NET Core 的已裝載部署</span><span class="sxs-lookup"><span data-stu-id="d2a48-191">Hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="d2a48-192">「裝載部署」會將用戶端 Blazor 應用程式，從伺服器上執行的 [ASP.NET Core 應用程式](xref:index)，提供給瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="d2a48-192">A *hosted deployment* serves the client-side Blazor app to browsers from an [ASP.NET Core app](xref:index) that runs on a server.</span></span>

<span data-ttu-id="d2a48-193">Blazor 應用程式隨附於發佈輸出中的 ASP.NET Core 應用程式，以便兩個應用程式會一起部署。</span><span class="sxs-lookup"><span data-stu-id="d2a48-193">The Blazor app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="d2a48-194">需要有能夠裝載 ASP.NET Core 應用程式的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="d2a48-194">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="d2a48-195">對於裝載部署，Visual Studio 包含 **Blazor (已裝載 ASP.NET Core)** 專案範本 (使用 [dotnet new](/dotnet/core/tools/dotnet-new) 命令時的 `blazorhosted` 範本)。</span><span class="sxs-lookup"><span data-stu-id="d2a48-195">For a hosted deployment, Visual Studio includes the **Blazor (ASP.NET Core hosted)** project template (`blazorhosted` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<span data-ttu-id="d2a48-196">如需 ASP.NET Core 應用程式裝載和部署的詳細資訊，請參閱 <xref:host-and-deploy/index>。</span><span class="sxs-lookup"><span data-stu-id="d2a48-196">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="d2a48-197">如需部署至 Azure App Service 的相關資訊，請參閱 <xref:tutorials/publish-to-azure-webapp-using-vs>。</span><span class="sxs-lookup"><span data-stu-id="d2a48-197">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>

## <a name="standalone-deployment"></a><span data-ttu-id="d2a48-198">獨立部署</span><span class="sxs-lookup"><span data-stu-id="d2a48-198">Standalone deployment</span></span>

<span data-ttu-id="d2a48-199">「獨立部署」會以一組直接由用戶端要求的靜態檔案來提供服務給用戶端 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d2a48-199">A *standalone deployment* serves the client-side Blazor app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="d2a48-200">網頁伺服器不用來提供服務給 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d2a48-200">A web server isn't used to serve the Blazor app.</span></span>

### <a name="iis"></a><span data-ttu-id="d2a48-201">IIS</span><span class="sxs-lookup"><span data-stu-id="d2a48-201">IIS</span></span>

<span data-ttu-id="d2a48-202">IIS 是足以支援 Blazor 應用程式的靜態檔案伺服器。</span><span class="sxs-lookup"><span data-stu-id="d2a48-202">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="d2a48-203">若要設定 IIS 來裝載 Blazor，請參閱[在 IIS 上建置靜態網站](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="d2a48-203">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="d2a48-204">已發行的資產會建立在 */bin/Release/{TARGET FRAMEWORK}/publish* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="d2a48-204">Published assets are created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="d2a48-205">在網頁伺服器或裝載服務上，裝載 *publish* 資料夾的內容。</span><span class="sxs-lookup"><span data-stu-id="d2a48-205">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

#### <a name="webconfig"></a><span data-ttu-id="d2a48-206">web.config</span><span class="sxs-lookup"><span data-stu-id="d2a48-206">web.config</span></span>

<span data-ttu-id="d2a48-207">發佈 Blazor 專案時，會建立 *web.config* 檔案，並使用下列 IIS 組態：</span><span class="sxs-lookup"><span data-stu-id="d2a48-207">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="d2a48-208">針對下列副檔名設定 MIME 類型：</span><span class="sxs-lookup"><span data-stu-id="d2a48-208">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="d2a48-209">*.dll* &ndash; `application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="d2a48-209">*.dll* &ndash; `application/octet-stream`</span></span>
  * <span data-ttu-id="d2a48-210">*.json* &ndash; `application/json`</span><span class="sxs-lookup"><span data-stu-id="d2a48-210">*.json* &ndash; `application/json`</span></span>
  * <span data-ttu-id="d2a48-211">*.wasm* &ndash; `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="d2a48-211">*.wasm* &ndash; `application/wasm`</span></span>
  * <span data-ttu-id="d2a48-212">*.woff* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="d2a48-212">*.woff* &ndash; `application/font-woff`</span></span>
  * <span data-ttu-id="d2a48-213">*.woff2* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="d2a48-213">*.woff2* &ndash; `application/font-woff`</span></span>
* <span data-ttu-id="d2a48-214">針對下列 MIME 類型會啟用 HTTP 壓縮：</span><span class="sxs-lookup"><span data-stu-id="d2a48-214">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="d2a48-215">建立 URL Rewrite Module 規則：</span><span class="sxs-lookup"><span data-stu-id="d2a48-215">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="d2a48-216">提供應用程式靜態資產所在的子目錄 (*{組件名稱}/dist/{要求的路徑}*)。</span><span class="sxs-lookup"><span data-stu-id="d2a48-216">Serve the sub-directory where the app's static assets reside (*{ASSEMBLY NAME}/dist/{PATH REQUESTED}*).</span></span>
  * <span data-ttu-id="d2a48-217">建立 SPA 後援路由，讓非檔案資產要求重新導向至其靜態資產資料夾中的應用程式預設文件 (*{組件名稱}/dist/index.html*)。</span><span class="sxs-lookup"><span data-stu-id="d2a48-217">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*{ASSEMBLY NAME}/dist/index.html*).</span></span>

#### <a name="install-the-url-rewrite-module"></a><span data-ttu-id="d2a48-218">安裝 URL Rewrite 模組</span><span class="sxs-lookup"><span data-stu-id="d2a48-218">Install the URL Rewrite Module</span></span>

<span data-ttu-id="d2a48-219">需要 [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite)，才可重寫 URL。</span><span class="sxs-lookup"><span data-stu-id="d2a48-219">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="d2a48-220">預設不會安裝此模組，且其無法用來安裝為網頁伺服器 (IIS) 角色服務功能。</span><span class="sxs-lookup"><span data-stu-id="d2a48-220">The module isn't installed by default, and it isn't available for install as a Web Server (IIS) role service feature.</span></span> <span data-ttu-id="d2a48-221">必須從 IIS 網站下載模組。</span><span class="sxs-lookup"><span data-stu-id="d2a48-221">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="d2a48-222">請使用 Web Platform Installer 安裝模組：</span><span class="sxs-lookup"><span data-stu-id="d2a48-222">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="d2a48-223">在本機上，巡覽至 [URL Rewrite Module 下載頁面](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads)。</span><span class="sxs-lookup"><span data-stu-id="d2a48-223">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="d2a48-224">如需英文版，請選取 [WebPI] 下載 WebPI 安裝程式。</span><span class="sxs-lookup"><span data-stu-id="d2a48-224">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="d2a48-225">如需其他語言，請選取適當的伺服器架構 (x86 x64) 來下載安裝程式。</span><span class="sxs-lookup"><span data-stu-id="d2a48-225">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="d2a48-226">將安裝程式複製到伺服器。</span><span class="sxs-lookup"><span data-stu-id="d2a48-226">Copy the installer to the server.</span></span> <span data-ttu-id="d2a48-227">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="d2a48-227">Run the installer.</span></span> <span data-ttu-id="d2a48-228">選取 [安裝] 按鈕，並接受授權條款。</span><span class="sxs-lookup"><span data-stu-id="d2a48-228">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="d2a48-229">安裝完成之後，不需要重新啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="d2a48-229">A server restart isn't required after the install completes.</span></span>

#### <a name="configure-the-website"></a><span data-ttu-id="d2a48-230">設定網站</span><span class="sxs-lookup"><span data-stu-id="d2a48-230">Configure the website</span></span>

<span data-ttu-id="d2a48-231">將網站的 [實體路徑] 設為應用程式的資料夾。</span><span class="sxs-lookup"><span data-stu-id="d2a48-231">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="d2a48-232">此資料夾包含：</span><span class="sxs-lookup"><span data-stu-id="d2a48-232">The folder contains:</span></span>

* <span data-ttu-id="d2a48-233">IIS 用來設定網站的 *web.config* 檔，包括必要的重新導向規則和檔案內容類型。</span><span class="sxs-lookup"><span data-stu-id="d2a48-233">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="d2a48-234">應用程式的靜態資產資料夾。</span><span class="sxs-lookup"><span data-stu-id="d2a48-234">The app's static asset folder.</span></span>

#### <a name="troubleshooting"></a><span data-ttu-id="d2a48-235">疑難排解</span><span class="sxs-lookup"><span data-stu-id="d2a48-235">Troubleshooting</span></span>

<span data-ttu-id="d2a48-236">如果收到「500 - 內部伺服器錯誤」，且 IIS 管理員在嘗試存取網站設定時擲回錯誤，請確認是否已安裝 URL Rewrite 模組。</span><span class="sxs-lookup"><span data-stu-id="d2a48-236">If a *500 - Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="d2a48-237">未安裝此模組時，IIS 無法剖析 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="d2a48-237">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="d2a48-238">這導致 IIS 管理員無法載入網站的組態，且網站無法提供 Blazor 的靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="d2a48-238">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="d2a48-239">如需針對部署至 IIS 進行疑難排解的詳細資訊，請參閱 <xref:host-and-deploy/iis/troubleshoot>。</span><span class="sxs-lookup"><span data-stu-id="d2a48-239">For more information on troubleshooting deployments to IIS, see <xref:host-and-deploy/iis/troubleshoot>.</span></span>

### <a name="nginx"></a><span data-ttu-id="d2a48-240">Nginx</span><span class="sxs-lookup"><span data-stu-id="d2a48-240">Nginx</span></span>

<span data-ttu-id="d2a48-241">下列 *nginx.conf* 檔案已簡化，示範如何將 Nginx 設定為每當找不到磁碟上的對應檔案時，便傳送 *Index.html* 檔案。</span><span class="sxs-lookup"><span data-stu-id="d2a48-241">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *index.html* file whenever it can't find a corresponding file on disk.</span></span>

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

<span data-ttu-id="d2a48-242">如需生產環境 Nginx 網頁伺服器組態的詳細資訊，請參閱[建立 NGINX Plus 和 NGINX 組態檔](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/)。</span><span class="sxs-lookup"><span data-stu-id="d2a48-242">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

### <a name="nginx-in-docker"></a><span data-ttu-id="d2a48-243">Docker 中的 Nginx</span><span class="sxs-lookup"><span data-stu-id="d2a48-243">Nginx in Docker</span></span>

<span data-ttu-id="d2a48-244">若要在 Docker 中使用 Nginx 裝載 Blazor，請設定 Dockerfile 以使用以 Alpine 為基礎的 Nginx 映像。</span><span class="sxs-lookup"><span data-stu-id="d2a48-244">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="d2a48-245">更新 Dockerfile，將 *nginx.config* 檔案複製到容器內。</span><span class="sxs-lookup"><span data-stu-id="d2a48-245">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="d2a48-246">如下列範例所示，新增一行至 Dockerfile：</span><span class="sxs-lookup"><span data-stu-id="d2a48-246">Add one line to the Dockerfile, as shown in the following example:</span></span>

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="github-pages"></a><span data-ttu-id="d2a48-247">GitHub Pages</span><span class="sxs-lookup"><span data-stu-id="d2a48-247">GitHub Pages</span></span>

<span data-ttu-id="d2a48-248">若要處理 URL 重寫，請新增 *404.html* 檔，以及處理將要求重新導向至 *index.html* 頁面的指令碼。</span><span class="sxs-lookup"><span data-stu-id="d2a48-248">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="d2a48-249">如需社群所提供的範例實作，請參閱 [GitHub Pages 的單一頁面應用程式](http://spa-github-pages.rafrex.com/) (GitHub 上的 [rafrex/spa-github-pages](https://github.com/rafrex/spa-github-pages#readme))。</span><span class="sxs-lookup"><span data-stu-id="d2a48-249">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](http://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="d2a48-250">使用社群方法的範例可在 [GitHub 上的 blazor-demo/blazor-demo.github.io](https://github.com/blazor-demo/blazor-demo.github.io) ([即時網站](https://blazor-demo.github.io/)) 看到。</span><span class="sxs-lookup"><span data-stu-id="d2a48-250">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="d2a48-251">使用專案網站，而非組織網站時，請在 *index.html* 中新增或更新 `<base>` 標籤。</span><span class="sxs-lookup"><span data-stu-id="d2a48-251">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="d2a48-252">將 `href` 屬性值設定為 GitHub 存放庫名稱，並在尾端加上斜線 (例如 `my-repository/`)。</span><span class="sxs-lookup"><span data-stu-id="d2a48-252">Set the `href` attribute value to the GitHub repository name with a trailing slash (for example, `my-repository/`.</span></span>
