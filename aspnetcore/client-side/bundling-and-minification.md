---
title: 在 ASP.NET Core 中組合及縮小靜態資產
author: scottaddie
description: 瞭解如何藉由套用捆綁和縮制技術，將 ASP.NET Core web 應用程式中的靜態資源優化。
ms.author: scaddie
ms.custom: mvc
ms.date: 06/17/2019
uid: client-side/bundling-and-minification
ms.openlocfilehash: 7499381a24a2513a4fbd1205f245e624c86647c3
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080564"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a><span data-ttu-id="b939d-103">在 ASP.NET Core 中組合及縮小靜態資產</span><span class="sxs-lookup"><span data-stu-id="b939d-103">Bundle and minify static assets in ASP.NET Core</span></span>

<span data-ttu-id="b939d-104">由[Scott Addie](https://twitter.com/Scott_Addie)和[David 松樹](https://twitter.com/davidpine7)</span><span class="sxs-lookup"><span data-stu-id="b939d-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [David Pine](https://twitter.com/davidpine7)</span></span>

<span data-ttu-id="b939d-105">本文說明套用配套和縮制的優點，包括如何將這些功能與 ASP.NET Core web 應用程式搭配使用。</span><span class="sxs-lookup"><span data-stu-id="b939d-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="b939d-106">什麼是捆綁和縮制</span><span class="sxs-lookup"><span data-stu-id="b939d-106">What is bundling and minification</span></span>

<span data-ttu-id="b939d-107">捆綁和縮制是您可以在 web 應用程式中套用的兩個不同的效能優化。</span><span class="sxs-lookup"><span data-stu-id="b939d-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="b939d-108">搭配使用、結合和縮制可減少伺服器要求的數目，以及減少所要求靜態資產的大小，進而提升效能。</span><span class="sxs-lookup"><span data-stu-id="b939d-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="b939d-109">組合和縮制主要會改善第一頁的要求載入時間。</span><span class="sxs-lookup"><span data-stu-id="b939d-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="b939d-110">一旦要求網頁之後，瀏覽器就會快取靜態資產（JavaScript、CSS 和影像）。</span><span class="sxs-lookup"><span data-stu-id="b939d-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="b939d-111">因此，在要求相同資產的相同網站上要求相同頁面（或頁面）時，配套和縮制並不會改善效能。</span><span class="sxs-lookup"><span data-stu-id="b939d-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="b939d-112">如果資產上的 expires 標頭未正確設定，且未使用配套和縮制，則瀏覽器的有效性啟發式會將幾天後的資產過時。</span><span class="sxs-lookup"><span data-stu-id="b939d-112">If the expires header isn't set correctly on the assets and if bundling and minification isn't used, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="b939d-113">此外，瀏覽器需要每個資產的驗證要求。</span><span class="sxs-lookup"><span data-stu-id="b939d-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="b939d-114">在此情況下，即使在第一頁要求之後，配套和縮制也可提供效能改進。</span><span class="sxs-lookup"><span data-stu-id="b939d-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="b939d-115">統合</span><span class="sxs-lookup"><span data-stu-id="b939d-115">Bundling</span></span>

<span data-ttu-id="b939d-116">配套會將多個檔案結合成單一檔案。</span><span class="sxs-lookup"><span data-stu-id="b939d-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="b939d-117">「組合」可減少呈現 web 資產（例如網頁）所需的伺服器要求數目。</span><span class="sxs-lookup"><span data-stu-id="b939d-117">Bundling reduces the number of server requests that are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="b939d-118">您可以針對 CSS、JavaScript 等建立任意數目的個別組合。較少的檔案表示從瀏覽器到伺服器的 HTTP 要求較少，或提供您應用程式的服務。</span><span class="sxs-lookup"><span data-stu-id="b939d-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="b939d-119">這會導致第一頁載入效能改進。</span><span class="sxs-lookup"><span data-stu-id="b939d-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="b939d-120">縮制</span><span class="sxs-lookup"><span data-stu-id="b939d-120">Minification</span></span>

<span data-ttu-id="b939d-121">縮制會從程式碼移除不必要的字元，而不會改變功能。</span><span class="sxs-lookup"><span data-stu-id="b939d-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="b939d-122">結果會大幅減少要求的資產（例如 CSS、影像和 JavaScript 檔案）的大小。</span><span class="sxs-lookup"><span data-stu-id="b939d-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="b939d-123">縮制的常見副作用包括縮短變數名稱為一個字元，以及移除批註和不必要的空白字元。</span><span class="sxs-lookup"><span data-stu-id="b939d-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="b939d-124">請考慮下列 JavaScript 函數：</span><span class="sxs-lookup"><span data-stu-id="b939d-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="b939d-125">縮制會將函數縮減為下列內容：</span><span class="sxs-lookup"><span data-stu-id="b939d-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="b939d-126">除了移除批註和不必要的空白字元以外，下列參數和變數名稱的重新命名方式如下：</span><span class="sxs-lookup"><span data-stu-id="b939d-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="b939d-127">原始</span><span class="sxs-lookup"><span data-stu-id="b939d-127">Original</span></span> | <span data-ttu-id="b939d-128">已重新命名</span><span class="sxs-lookup"><span data-stu-id="b939d-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="b939d-129">捆綁與縮制的影響</span><span class="sxs-lookup"><span data-stu-id="b939d-129">Impact of bundling and minification</span></span>

<span data-ttu-id="b939d-130">下表概述個別載入資產與使用配套和縮制之間的差異：</span><span class="sxs-lookup"><span data-stu-id="b939d-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="b939d-131">動作</span><span class="sxs-lookup"><span data-stu-id="b939d-131">Action</span></span> | <span data-ttu-id="b939d-132">使用 B/M</span><span class="sxs-lookup"><span data-stu-id="b939d-132">With B/M</span></span> | <span data-ttu-id="b939d-133">不含 B/M 的</span><span class="sxs-lookup"><span data-stu-id="b939d-133">Without B/M</span></span> | <span data-ttu-id="b939d-134">變更</span><span class="sxs-lookup"><span data-stu-id="b939d-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="b939d-135">檔案要求</span><span class="sxs-lookup"><span data-stu-id="b939d-135">File Requests</span></span>  | <span data-ttu-id="b939d-136">7</span><span class="sxs-lookup"><span data-stu-id="b939d-136">7</span></span>   | <span data-ttu-id="b939d-137">18</span><span class="sxs-lookup"><span data-stu-id="b939d-137">18</span></span>     | <span data-ttu-id="b939d-138">157%</span><span class="sxs-lookup"><span data-stu-id="b939d-138">157%</span></span>
<span data-ttu-id="b939d-139">已傳輸 KB</span><span class="sxs-lookup"><span data-stu-id="b939d-139">KB Transferred</span></span> | <span data-ttu-id="b939d-140">156</span><span class="sxs-lookup"><span data-stu-id="b939d-140">156</span></span> | <span data-ttu-id="b939d-141">264.68</span><span class="sxs-lookup"><span data-stu-id="b939d-141">264.68</span></span> | <span data-ttu-id="b939d-142">70%</span><span class="sxs-lookup"><span data-stu-id="b939d-142">70%</span></span>
<span data-ttu-id="b939d-143">載入時間（毫秒）</span><span class="sxs-lookup"><span data-stu-id="b939d-143">Load Time (ms)</span></span> | <span data-ttu-id="b939d-144">885</span><span class="sxs-lookup"><span data-stu-id="b939d-144">885</span></span> | <span data-ttu-id="b939d-145">2360</span><span class="sxs-lookup"><span data-stu-id="b939d-145">2360</span></span>   | <span data-ttu-id="b939d-146">167%</span><span class="sxs-lookup"><span data-stu-id="b939d-146">167%</span></span>

<span data-ttu-id="b939d-147">瀏覽器對於 HTTP 要求標頭而言相當冗長。</span><span class="sxs-lookup"><span data-stu-id="b939d-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="b939d-148">[已傳送的位元組總數] 計量在進行捆綁時明顯降低。</span><span class="sxs-lookup"><span data-stu-id="b939d-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="b939d-149">載入時間顯示顯著的改進，但此範例會在本機執行。</span><span class="sxs-lookup"><span data-stu-id="b939d-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="b939d-150">搭配透過網路傳輸的資產使用配套和縮制時，就可以獲得更佳的效能提升。</span><span class="sxs-lookup"><span data-stu-id="b939d-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="b939d-151">選擇捆綁和縮制策略</span><span class="sxs-lookup"><span data-stu-id="b939d-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="b939d-152">MVC 和 Razor Pages 專案範本提供現成的解決方案，可供包含 JSON 設定檔的組合和縮制使用。</span><span class="sxs-lookup"><span data-stu-id="b939d-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="b939d-153">協力廠商工具（例如[Grunt](xref:client-side/using-grunt)工作執行器）會以更複雜的方式來完成相同的工作。</span><span class="sxs-lookup"><span data-stu-id="b939d-153">Third-party tools, such as the [Grunt](xref:client-side/using-grunt) task runner, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="b939d-154">當您的開發工作流程需要處理超出配套和縮制&mdash;（例如 linting 和影像優化）時，協力廠商工具是很好的組合。</span><span class="sxs-lookup"><span data-stu-id="b939d-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="b939d-155">藉由使用設計階段組合和縮制，縮減檔案會在應用程式部署之前建立。</span><span class="sxs-lookup"><span data-stu-id="b939d-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="b939d-156">部署之前的捆綁與縮小提供伺服器負載降低的優點。</span><span class="sxs-lookup"><span data-stu-id="b939d-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="b939d-157">不過，請務必辨識設計階段的組合和縮制會增加組建複雜度，而且只適用于靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="b939d-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="b939d-158">設定捆綁和縮制</span><span class="sxs-lookup"><span data-stu-id="b939d-158">Configure bundling and minification</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="b939d-159">在 ASP.NET Core 2.0 或更早版本中，MVC 和 Razor Pages 專案範本會提供*bundleconfig.json*檔案，以定義每個組合的選項：</span><span class="sxs-lookup"><span data-stu-id="b939d-159">In ASP.NET Core 2.0 or earlier, the MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file that defines the options for each bundle:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b939d-160">在 ASP.NET Core 2.1 或更新版本中，將名為*bundleconfig.json*的新 JSON 檔案新增至 MVC 或 Razor Pages 專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="b939d-160">In ASP.NET Core 2.1 or later, add a new JSON file, named *bundleconfig.json*, to the MVC or Razor Pages project root.</span></span> <span data-ttu-id="b939d-161">在該檔案中包含下列 JSON 做為起點：</span><span class="sxs-lookup"><span data-stu-id="b939d-161">Include the following JSON in that file as a starting point:</span></span>

::: moniker-end

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="b939d-162">*Bundleconfig.json*會定義每個配套的選項。</span><span class="sxs-lookup"><span data-stu-id="b939d-162">The *bundleconfig.json* file defines the options for each bundle.</span></span> <span data-ttu-id="b939d-163">在上述範例中，會針對自訂 JavaScript （*wwwroot/js/* node.js）和樣式表單（*wwwroot/css/.css*）檔案定義單一組合設定。</span><span class="sxs-lookup"><span data-stu-id="b939d-163">In the preceding example, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files.</span></span>

<span data-ttu-id="b939d-164">設定選項包括:</span><span class="sxs-lookup"><span data-stu-id="b939d-164">Configuration options include:</span></span>

* <span data-ttu-id="b939d-165">`outputFileName`：要輸出的組合檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="b939d-165">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="b939d-166">可以包含來自*bundleconfig.json*的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="b939d-166">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="b939d-167">**必填**</span><span class="sxs-lookup"><span data-stu-id="b939d-167">**required**</span></span>
* <span data-ttu-id="b939d-168">`inputFiles`：要組合在一起的檔案陣列。</span><span class="sxs-lookup"><span data-stu-id="b939d-168">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="b939d-169">這些是設定檔的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="b939d-169">These are relative paths to the configuration file.</span></span> <span data-ttu-id="b939d-170">（**選擇性**） \* 空白值會產生空的輸出檔。</span><span class="sxs-lookup"><span data-stu-id="b939d-170">**optional**, \*an empty value results in an empty output file.</span></span> <span data-ttu-id="b939d-171">支援[萬用字元模式。](https://www.tldp.org/LDP/abs/html/globbingref.html)</span><span class="sxs-lookup"><span data-stu-id="b939d-171">[globbing](https://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="b939d-172">`minify`：輸出類型的縮制選項。</span><span class="sxs-lookup"><span data-stu-id="b939d-172">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="b939d-173">**選擇性**，*預設值`minify: { enabled: true }` -*</span><span class="sxs-lookup"><span data-stu-id="b939d-173">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="b939d-174">每個輸出檔案類型都有可用的設定選項。</span><span class="sxs-lookup"><span data-stu-id="b939d-174">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="b939d-175">CSS Minifier</span><span class="sxs-lookup"><span data-stu-id="b939d-175">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="b939d-176">JavaScript Minifier</span><span class="sxs-lookup"><span data-stu-id="b939d-176">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="b939d-177">HTML Minifier</span><span class="sxs-lookup"><span data-stu-id="b939d-177">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="b939d-178">`includeInProject`：表示是否將產生的檔案加入至專案檔的旗標。</span><span class="sxs-lookup"><span data-stu-id="b939d-178">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="b939d-179">**選擇性**，*預設值-false*</span><span class="sxs-lookup"><span data-stu-id="b939d-179">**optional**, *default - false*</span></span>
* <span data-ttu-id="b939d-180">`sourceMap`：表示是否要產生配套檔案之來源對應的旗標。</span><span class="sxs-lookup"><span data-stu-id="b939d-180">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="b939d-181">**選擇性**，*預設值-false*</span><span class="sxs-lookup"><span data-stu-id="b939d-181">**optional**, *default - false*</span></span>
* <span data-ttu-id="b939d-182">`sourceMapRootPath`：用來儲存所產生之來源對應檔的根路徑。</span><span class="sxs-lookup"><span data-stu-id="b939d-182">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="b939d-183">組合和縮制的組建時間執行</span><span class="sxs-lookup"><span data-stu-id="b939d-183">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="b939d-184">[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet 套件可讓您在組建階段執行封裝和縮制。</span><span class="sxs-lookup"><span data-stu-id="b939d-184">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="b939d-185">封裝會插入在組建和清除時間執行的[MSBuild 目標](/visualstudio/msbuild/msbuild-targets)。</span><span class="sxs-lookup"><span data-stu-id="b939d-185">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="b939d-186">建立程式會分析*bundleconfig.json*檔案，以根據定義的設定產生輸出檔。</span><span class="sxs-lookup"><span data-stu-id="b939d-186">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="b939d-187">BuildBundlerMinifier 屬於 GitHub 上的社區驅動專案，Microsoft 不提供任何支援。</span><span class="sxs-lookup"><span data-stu-id="b939d-187">BuildBundlerMinifier belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="b939d-188">問題應在[此](https://github.com/madskristensen/BundlerMinifier/issues)提出。</span><span class="sxs-lookup"><span data-stu-id="b939d-188">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b939d-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b939d-189">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b939d-190">將*BuildBundlerMinifier*套件新增至您的專案。</span><span class="sxs-lookup"><span data-stu-id="b939d-190">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="b939d-191">建置專案。</span><span class="sxs-lookup"><span data-stu-id="b939d-191">Build the project.</span></span> <span data-ttu-id="b939d-192">[輸出] 視窗中會顯示下列內容：</span><span class="sxs-lookup"><span data-stu-id="b939d-192">The following appears in the Output window:</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>  Minified wwwroot/css/site.min.css
1>  Minified wwwroot/js/site.min.js
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

<span data-ttu-id="b939d-193">清除專案。</span><span class="sxs-lookup"><span data-stu-id="b939d-193">Clean the project.</span></span> <span data-ttu-id="b939d-194">[輸出] 視窗中會顯示下列內容：</span><span class="sxs-lookup"><span data-stu-id="b939d-194">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b939d-195">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b939d-195">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b939d-196">將*BuildBundlerMinifier*套件新增至您的專案：</span><span class="sxs-lookup"><span data-stu-id="b939d-196">Add the *BuildBundlerMinifier* package to your project:</span></span>

```dotnetcli
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="b939d-197">如果使用 ASP.NET Core 1.x，請還原新加入的套件：</span><span class="sxs-lookup"><span data-stu-id="b939d-197">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```dotnetcli
dotnet restore
```

<span data-ttu-id="b939d-198">建立專案：</span><span class="sxs-lookup"><span data-stu-id="b939d-198">Build the project:</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="b939d-199">如下所示：</span><span class="sxs-lookup"><span data-stu-id="b939d-199">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="b939d-200">清除專案：</span><span class="sxs-lookup"><span data-stu-id="b939d-200">Clean the project:</span></span>

```dotnetcli
dotnet clean
```

<span data-ttu-id="b939d-201">即會出現下列輸出：</span><span class="sxs-lookup"><span data-stu-id="b939d-201">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="b939d-202">臨機操作執行組合和縮制</span><span class="sxs-lookup"><span data-stu-id="b939d-202">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="b939d-203">您可以臨機操作方式執行系結和縮制工作，而不需要建立專案。</span><span class="sxs-lookup"><span data-stu-id="b939d-203">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="b939d-204">將[BundlerMinifier](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet 套件新增至您的專案：</span><span class="sxs-lookup"><span data-stu-id="b939d-204">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> <span data-ttu-id="b939d-205">BundlerMinifier 屬於 GitHub 上的社區驅動專案，Microsoft 不提供任何支援。</span><span class="sxs-lookup"><span data-stu-id="b939d-205">BundlerMinifier.Core belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="b939d-206">問題應在[此](https://github.com/madskristensen/BundlerMinifier/issues)提出。</span><span class="sxs-lookup"><span data-stu-id="b939d-206">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="b939d-207">此套件會擴充 .NET Core CLI 以包含*dotnet*組合工具。</span><span class="sxs-lookup"><span data-stu-id="b939d-207">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="b939d-208">您可以在 [套件管理員主控台] （PMC）視窗或命令介面中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="b939d-208">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```dotnetcli
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="b939d-209">NuGet 套件管理員會將 \* .csproj 檔案的相依性新增`<PackageReference />`為節點。</span><span class="sxs-lookup"><span data-stu-id="b939d-209">NuGet Package Manager adds dependencies to the \*.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="b939d-210">只有`dotnet bundle`在使用`<DotNetCliToolReference />`節點時，才會向 .NET Core CLI 註冊此命令。</span><span class="sxs-lookup"><span data-stu-id="b939d-210">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="b939d-211">據以修改 \* .csproj 檔案。</span><span class="sxs-lookup"><span data-stu-id="b939d-211">Modify the \*.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="b939d-212">將檔案新增至工作流程</span><span class="sxs-lookup"><span data-stu-id="b939d-212">Add files to workflow</span></span>

<span data-ttu-id="b939d-213">假設有一個範例，其中加入了類似下列的其他*自訂 .css*檔案：</span><span class="sxs-lookup"><span data-stu-id="b939d-213">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="b939d-214">若要縮小*自訂的 .css* ，並將其與 *.css*結合在*網站. 最小 .css*檔案中，請新增*bundleconfig.json*的相對路徑：</span><span class="sxs-lookup"><span data-stu-id="b939d-214">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="b939d-215">或者，也可以使用下列萬用字元模式：</span><span class="sxs-lookup"><span data-stu-id="b939d-215">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css))"]
> ```
>
> <span data-ttu-id="b939d-216">這個萬用字元模式會比對所有 CSS 檔案，並排除縮減檔案模式。</span><span class="sxs-lookup"><span data-stu-id="b939d-216">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="b939d-217">建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="b939d-217">Build the application.</span></span> <span data-ttu-id="b939d-218">開啟 [*網站*]，並注意 [*自訂 .css* ] 的內容會附加至檔案結尾。</span><span class="sxs-lookup"><span data-stu-id="b939d-218">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="b939d-219">以環境為基礎的捆綁和縮制</span><span class="sxs-lookup"><span data-stu-id="b939d-219">Environment-based bundling and minification</span></span>

<span data-ttu-id="b939d-220">最佳做法是，應用程式的配套和縮減檔案應該在生產環境中使用。</span><span class="sxs-lookup"><span data-stu-id="b939d-220">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="b939d-221">在開發期間，原始檔案可讓您更輕鬆地對應用程式進行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="b939d-221">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="b939d-222">使用您的瀏覽器中的[環境](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)標籤協助程式，指定要包含在頁面中的檔案。</span><span class="sxs-lookup"><span data-stu-id="b939d-222">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="b939d-223">環境標籤協助程式只會在特定[環境](xref:fundamentals/environments)中執行時呈現其內容。</span><span class="sxs-lookup"><span data-stu-id="b939d-223">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="b939d-224">下列`environment`標記會在`Development`環境中執行時呈現未處理的 CSS 檔案：</span><span class="sxs-lookup"><span data-stu-id="b939d-224">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

::: moniker-end

<span data-ttu-id="b939d-225">在以外`environment`的環境`Development`中執行時，下列標記會轉譯配套和縮減的 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="b939d-225">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="b939d-226">例如，在或`Production` `Staging`中執行時，會觸發這些樣式表單的呈現：</span><span class="sxs-lookup"><span data-stu-id="b939d-226">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

::: moniker-end

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="b939d-227">使用 Gulp 的 bundleconfig.json</span><span class="sxs-lookup"><span data-stu-id="b939d-227">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="b939d-228">在某些情況下，應用程式的捆綁與縮制工作流程需要額外的處理。</span><span class="sxs-lookup"><span data-stu-id="b939d-228">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="b939d-229">範例包括影像優化、快取破壞和 CDN 資產處理。</span><span class="sxs-lookup"><span data-stu-id="b939d-229">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="b939d-230">若要滿足這些需求，您可以將包裝和縮制工作流程轉換為使用 Gulp。</span><span class="sxs-lookup"><span data-stu-id="b939d-230">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="b939d-231">使用搭配程式 & Minifier 延伸模組</span><span class="sxs-lookup"><span data-stu-id="b939d-231">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="b939d-232">Visual Studio[搭配程式 & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier)延伸模組會處理 Gulp 的轉換。</span><span class="sxs-lookup"><span data-stu-id="b939d-232">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="b939d-233">搭配程式 & Minifier 延伸模組屬於 GitHub 上的社區驅動專案，Microsoft 不提供任何支援。</span><span class="sxs-lookup"><span data-stu-id="b939d-233">The Bundler & Minifier extension belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="b939d-234">問題應在[此](https://github.com/madskristensen/BundlerMinifier/issues)提出。</span><span class="sxs-lookup"><span data-stu-id="b939d-234">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="b939d-235">以滑鼠右鍵按一下方案總管中的*bundleconfig.json* ，然後選取 [**搭配程式 & Minifier**  > **轉換成 Gulp ...** ]：</span><span class="sxs-lookup"><span data-stu-id="b939d-235">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![轉換成 Gulp 內容功能表項目](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="b939d-237">*Gulpfile.js*會將檔案新增至*專案。*</span><span class="sxs-lookup"><span data-stu-id="b939d-237">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="b939d-238">已安裝*package. json*檔案的`devDependencies`區段中所列的支援[npm](https://www.npmjs.com/)套件。</span><span class="sxs-lookup"><span data-stu-id="b939d-238">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="b939d-239">在 [PMC] 視窗中執行下列命令，以將 Gulp CLI 安裝為全域相依性：</span><span class="sxs-lookup"><span data-stu-id="b939d-239">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="b939d-240">*Gulpfile.js*會讀取輸入、輸出和設定的*bundleconfig.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="b939d-240">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="b939d-241">手動轉換</span><span class="sxs-lookup"><span data-stu-id="b939d-241">Convert manually</span></span>

<span data-ttu-id="b939d-242">如果 Visual Studio 和/或搭配程式 & Minifier 延伸模組無法使用，請以手動方式轉換。</span><span class="sxs-lookup"><span data-stu-id="b939d-242">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="b939d-243">將包含下列`devDependencies`的*package. json*檔案新增至專案根目錄：</span><span class="sxs-lookup"><span data-stu-id="b939d-243">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

> [!WARNING]
> <span data-ttu-id="b939d-244">`gulp-uglify`模組不支援 ECMAScript （ES） 2015/ES6 和更新版本。</span><span class="sxs-lookup"><span data-stu-id="b939d-244">The `gulp-uglify` module doesn't support ECMAScript (ES) 2015 / ES6 and later.</span></span> <span data-ttu-id="b939d-245">安裝[gulp-terser](https://www.npmjs.com/package/gulp-terser) ，而`gulp-uglify`不是使用 ES2015/ES6 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="b939d-245">Install [gulp-terser](https://www.npmjs.com/package/gulp-terser) instead of `gulp-uglify` to use ES2015 / ES6 or later.</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="b939d-246">藉由在與 package 相同的層級執行下列命令來安裝相依性 *。 json*：</span><span class="sxs-lookup"><span data-stu-id="b939d-246">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="b939d-247">將 Gulp CLI 安裝為全域相依性：</span><span class="sxs-lookup"><span data-stu-id="b939d-247">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="b939d-248">將下列*gulpfile.js*複製到專案根目錄：</span><span class="sxs-lookup"><span data-stu-id="b939d-248">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="b939d-249">執行 Gulp 工作</span><span class="sxs-lookup"><span data-stu-id="b939d-249">Run Gulp tasks</span></span>

<span data-ttu-id="b939d-250">若要在 Visual Studio 中建立專案之前觸發 Gulp 縮制工作，請將下列[MSBuild 目標](/visualstudio/msbuild/msbuild-targets)新增至 \* .csproj 檔案：</span><span class="sxs-lookup"><span data-stu-id="b939d-250">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the \*.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="b939d-251">在此範例中， `MyPreCompileTarget`目標內定義的任何工作都會在預先定義`Build`的目標之前執行。</span><span class="sxs-lookup"><span data-stu-id="b939d-251">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="b939d-252">與下列類似的輸出會出現在 Visual Studio 的 [輸出] 視窗中：</span><span class="sxs-lookup"><span data-stu-id="b939d-252">Output similar to the following appears in Visual Studio's Output window:</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
1>[14:17:49] Using gulpfile C:\BuildBundlerMinifierApp\gulpfile.js
1>[14:17:49] Starting 'min:js'...
1>[14:17:49] Starting 'min:css'...
1>[14:17:49] Starting 'min:html'...
1>[14:17:49] Finished 'min:js' after 83 ms
1>[14:17:49] Finished 'min:css' after 88 ms
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```


## <a name="additional-resources"></a><span data-ttu-id="b939d-253">其他資源</span><span class="sxs-lookup"><span data-stu-id="b939d-253">Additional resources</span></span>

* [<span data-ttu-id="b939d-254">使用 Grunt</span><span class="sxs-lookup"><span data-stu-id="b939d-254">Use Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="b939d-255">使用多重環境</span><span class="sxs-lookup"><span data-stu-id="b939d-255">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="b939d-256">標記協助程式</span><span class="sxs-lookup"><span data-stu-id="b939d-256">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
