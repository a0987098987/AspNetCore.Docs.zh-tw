---
title: 配套並縮短在 ASP.NET Core 中的靜態資產
author: scottaddie
description: 了解如何藉由套用統合和縮製技術最佳化的 ASP.NET Core web 應用程式中的靜態資源。
ms.author: scaddie
ms.custom: mvc
ms.date: 05/10/2019
uid: client-side/bundling-and-minification
ms.openlocfilehash: ba01d365a25dfbd13fed89263d7489b2ce2a8771
ms.sourcegitcommit: ffe3ed7921ec6c7c70abaac1d10703ec9a43374c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/11/2019
ms.locfileid: "65535923"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a><span data-ttu-id="3453f-103">配套並縮短在 ASP.NET Core 中的靜態資產</span><span class="sxs-lookup"><span data-stu-id="3453f-103">Bundle and minify static assets in ASP.NET Core</span></span>

<span data-ttu-id="3453f-104">藉由[Scott Addie](https://twitter.com/Scott_Addie)和[David 松](https://twitter.com/davidpine7)</span><span class="sxs-lookup"><span data-stu-id="3453f-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [David Pine](https://twitter.com/davidpine7)</span></span>

<span data-ttu-id="3453f-105">這篇文章會說明套用統合和縮製，包括如何使用這些功能，ASP.NET Core web 應用程式的優點。</span><span class="sxs-lookup"><span data-stu-id="3453f-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="3453f-106">統合和縮製是什麼</span><span class="sxs-lookup"><span data-stu-id="3453f-106">What is bundling and minification</span></span>

<span data-ttu-id="3453f-107">統合和縮製是您可以套用 web 應用程式中的兩種獨特的效能最佳化。</span><span class="sxs-lookup"><span data-stu-id="3453f-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="3453f-108">搭配使用，統合和縮製改善效能降低的伺服器要求數目以及減少要求的靜態資產的大小。</span><span class="sxs-lookup"><span data-stu-id="3453f-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="3453f-109">統合和縮製主要改善的第一個頁面要求載入時間。</span><span class="sxs-lookup"><span data-stu-id="3453f-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="3453f-110">一旦要求的網頁上，瀏覽器快取靜態資產 （JavaScript、 CSS 和映像）。</span><span class="sxs-lookup"><span data-stu-id="3453f-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="3453f-111">因此，統合和縮製不改善效能時要求相同的頁面上或要求相同的資產位於相同站台上的頁面。</span><span class="sxs-lookup"><span data-stu-id="3453f-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="3453f-112">如果過期標頭未正確設定資產並不使用統合和縮製，如果瀏覽器的有效期限啟發學習法標記資產過時幾天後。</span><span class="sxs-lookup"><span data-stu-id="3453f-112">If the expires header isn't set correctly on the assets and if bundling and minification isn't used, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="3453f-113">此外，瀏覽器會針對每個資產需要將驗證要求。</span><span class="sxs-lookup"><span data-stu-id="3453f-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="3453f-114">在此情況下，統合和縮製提供效能改進，即使第一個網頁要求。</span><span class="sxs-lookup"><span data-stu-id="3453f-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="3453f-115">統合</span><span class="sxs-lookup"><span data-stu-id="3453f-115">Bundling</span></span>

<span data-ttu-id="3453f-116">統合將多個檔案結合成單一檔案。</span><span class="sxs-lookup"><span data-stu-id="3453f-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="3453f-117">統合可減少所需呈現的 web 資產，例如網頁伺服器要求的數目。</span><span class="sxs-lookup"><span data-stu-id="3453f-117">Bundling reduces the number of server requests that are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="3453f-118">您可以建立任意數目的個別的套件組合，專為 CSS、 JavaScript 等。較少的檔案表示更少的 HTTP 要求從瀏覽器到伺服器或服務，提供您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="3453f-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="3453f-119">這會導致改善第一次頁面載入效能。</span><span class="sxs-lookup"><span data-stu-id="3453f-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="3453f-120">縮製</span><span class="sxs-lookup"><span data-stu-id="3453f-120">Minification</span></span>

<span data-ttu-id="3453f-121">縮製從程式碼移除不必要的字元，而不需要變更的功能。</span><span class="sxs-lookup"><span data-stu-id="3453f-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="3453f-122">結果會是重大的大小減少要求的資產 （例如 CSS、 影像和 JavaScript 檔案）。</span><span class="sxs-lookup"><span data-stu-id="3453f-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="3453f-123">常見的縮製的副作用包括縮短至某一字元的變數名稱，並移除註解和不必要的空白字元。</span><span class="sxs-lookup"><span data-stu-id="3453f-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="3453f-124">請考慮下列的 JavaScript 函式：</span><span class="sxs-lookup"><span data-stu-id="3453f-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="3453f-125">縮製可減少函式，如下：</span><span class="sxs-lookup"><span data-stu-id="3453f-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="3453f-126">除了移除不必要的空白字元的註解，下列的參數和變數名稱已重新命名為，如下所示：</span><span class="sxs-lookup"><span data-stu-id="3453f-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="3453f-127">原始</span><span class="sxs-lookup"><span data-stu-id="3453f-127">Original</span></span> | <span data-ttu-id="3453f-128">已重新命名</span><span class="sxs-lookup"><span data-stu-id="3453f-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="3453f-129">統合和縮製的影響</span><span class="sxs-lookup"><span data-stu-id="3453f-129">Impact of bundling and minification</span></span>

<span data-ttu-id="3453f-130">下表概述個別載入資產和使用統合和縮製之間的差異：</span><span class="sxs-lookup"><span data-stu-id="3453f-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="3453f-131">動作</span><span class="sxs-lookup"><span data-stu-id="3453f-131">Action</span></span> | <span data-ttu-id="3453f-132">B/m</span><span class="sxs-lookup"><span data-stu-id="3453f-132">With B/M</span></span> | <span data-ttu-id="3453f-133">Without B/M</span><span class="sxs-lookup"><span data-stu-id="3453f-133">Without B/M</span></span> | <span data-ttu-id="3453f-134">變更</span><span class="sxs-lookup"><span data-stu-id="3453f-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="3453f-135">提出要求</span><span class="sxs-lookup"><span data-stu-id="3453f-135">File Requests</span></span>  | <span data-ttu-id="3453f-136">7</span><span class="sxs-lookup"><span data-stu-id="3453f-136">7</span></span>   | <span data-ttu-id="3453f-137">18</span><span class="sxs-lookup"><span data-stu-id="3453f-137">18</span></span>     | <span data-ttu-id="3453f-138">157%</span><span class="sxs-lookup"><span data-stu-id="3453f-138">157%</span></span>
<span data-ttu-id="3453f-139">傳送的 KB</span><span class="sxs-lookup"><span data-stu-id="3453f-139">KB Transferred</span></span> | <span data-ttu-id="3453f-140">156</span><span class="sxs-lookup"><span data-stu-id="3453f-140">156</span></span> | <span data-ttu-id="3453f-141">264.68</span><span class="sxs-lookup"><span data-stu-id="3453f-141">264.68</span></span> | <span data-ttu-id="3453f-142">70%</span><span class="sxs-lookup"><span data-stu-id="3453f-142">70%</span></span>
<span data-ttu-id="3453f-143">載入時間 （毫秒）</span><span class="sxs-lookup"><span data-stu-id="3453f-143">Load Time (ms)</span></span> | <span data-ttu-id="3453f-144">885</span><span class="sxs-lookup"><span data-stu-id="3453f-144">885</span></span> | <span data-ttu-id="3453f-145">2360</span><span class="sxs-lookup"><span data-stu-id="3453f-145">2360</span></span>   | <span data-ttu-id="3453f-146">167%</span><span class="sxs-lookup"><span data-stu-id="3453f-146">167%</span></span>

<span data-ttu-id="3453f-147">瀏覽器會相當詳細資訊，關於 HTTP 要求標頭。</span><span class="sxs-lookup"><span data-stu-id="3453f-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="3453f-148">所傳送的總位元組時將大幅降低所看到的計量。</span><span class="sxs-lookup"><span data-stu-id="3453f-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="3453f-149">載入時間會顯示有明顯的改進，不過此範例會在本機執行。</span><span class="sxs-lookup"><span data-stu-id="3453f-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="3453f-150">使用統合和縮製與資產透過網路傳輸時，會實現更高的效能提升。</span><span class="sxs-lookup"><span data-stu-id="3453f-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="3453f-151">選擇統合和縮製的策略</span><span class="sxs-lookup"><span data-stu-id="3453f-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="3453f-152">MVC 和 Razor 頁面專案範本提供統合和縮製 JSON 組態檔所組成的立即可用的解決方案。</span><span class="sxs-lookup"><span data-stu-id="3453f-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="3453f-153">第三方工具，例如[Grunt](xref:client-side/using-grunt)工作執行器中，完成更多的複雜度與相同的工作。</span><span class="sxs-lookup"><span data-stu-id="3453f-153">Third-party tools, such as the [Grunt](xref:client-side/using-grunt) task runner, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="3453f-154">第三方工具時，非常適合您的開發工作流程需要處理超過統合和縮製&mdash;linting 和映像最佳化等。</span><span class="sxs-lookup"><span data-stu-id="3453f-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="3453f-155">藉由使用的設計階段統合和縮製，縮短的檔案會在應用程式的部署之前建立。</span><span class="sxs-lookup"><span data-stu-id="3453f-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="3453f-156">統合及縮小部署之前提供的優點是減少的伺服器負載。</span><span class="sxs-lookup"><span data-stu-id="3453f-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="3453f-157">不過，務必要辨識該設計階段統合和縮製增組建的複雜度，並僅適用於靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="3453f-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="3453f-158">設定統合和縮製</span><span class="sxs-lookup"><span data-stu-id="3453f-158">Configure bundling and minification</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="3453f-159">在 ASP.NET Core 2.0 或更早版本，MVC 和 Razor 頁面專案範本會提供*bundleconfig.json*定義每一個套件組合的選項的組態檔：</span><span class="sxs-lookup"><span data-stu-id="3453f-159">In ASP.NET Core 2.0 or earlier, the MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file that defines the options for each bundle:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="3453f-160">在 ASP.NET Core 2.1 或更新版本中，加入新的 JSON 檔案，名為*bundleconfig.json*、 MVC 或 Razor 頁面專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="3453f-160">In ASP.NET Core 2.1 or later, add a new JSON file, named *bundleconfig.json*, to the MVC or Razor Pages project root.</span></span> <span data-ttu-id="3453f-161">做為起點，該檔案中包含下列 JSON:</span><span class="sxs-lookup"><span data-stu-id="3453f-161">Include the following JSON in that file as a starting point:</span></span>

::: moniker-end

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="3453f-162">*Bundleconfig.json*檔案會定義每一個套件組合的選項。</span><span class="sxs-lookup"><span data-stu-id="3453f-162">The *bundleconfig.json* file defines the options for each bundle.</span></span> <span data-ttu-id="3453f-163">在上述範例中，單一組合設定定義適用於自訂的 JavaScript (*wwwroot/js/site.js*) 和樣式表 (*wwwroot/css/site.css*) 檔案。</span><span class="sxs-lookup"><span data-stu-id="3453f-163">In the preceding example, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files.</span></span>

<span data-ttu-id="3453f-164">設定選項包括：</span><span class="sxs-lookup"><span data-stu-id="3453f-164">Configuration options include:</span></span>

* <span data-ttu-id="3453f-165">`outputFileName`：組合檔案輸出名稱。</span><span class="sxs-lookup"><span data-stu-id="3453f-165">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="3453f-166">可以包含相對路徑*bundleconfig.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="3453f-166">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="3453f-167">**required**</span><span class="sxs-lookup"><span data-stu-id="3453f-167">**required**</span></span>
* <span data-ttu-id="3453f-168">`inputFiles`：要組合在一起的檔案陣列。</span><span class="sxs-lookup"><span data-stu-id="3453f-168">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="3453f-169">這些是在組態檔的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="3453f-169">These are relative paths to the configuration file.</span></span> <span data-ttu-id="3453f-170">**選擇性**，\* 空的輸出檔案中的值是空的結果。</span><span class="sxs-lookup"><span data-stu-id="3453f-170">**optional**, \*an empty value results in an empty output file.</span></span> <span data-ttu-id="3453f-171">[萬用字元](http://www.tldp.org/LDP/abs/html/globbingref.html)支援模式。</span><span class="sxs-lookup"><span data-stu-id="3453f-171">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="3453f-172">`minify`：輸出類型縮製的選項。</span><span class="sxs-lookup"><span data-stu-id="3453f-172">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="3453f-173">**選擇性**，*預設值- `minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="3453f-173">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="3453f-174">每個輸出檔案類型有組態選項。</span><span class="sxs-lookup"><span data-stu-id="3453f-174">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="3453f-175">CSS 縮短程式</span><span class="sxs-lookup"><span data-stu-id="3453f-175">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="3453f-176">JavaScript 縮短程式</span><span class="sxs-lookup"><span data-stu-id="3453f-176">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="3453f-177">HTML 縮短程式</span><span class="sxs-lookup"><span data-stu-id="3453f-177">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="3453f-178">`includeInProject`：旗標表示是否要將產生的檔案新增至專案檔。</span><span class="sxs-lookup"><span data-stu-id="3453f-178">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="3453f-179">**選擇性**，*預設為 false*</span><span class="sxs-lookup"><span data-stu-id="3453f-179">**optional**, *default - false*</span></span>
* <span data-ttu-id="3453f-180">`sourceMap`：旗標，指出是否要產生搭售檔案的來源對應。</span><span class="sxs-lookup"><span data-stu-id="3453f-180">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="3453f-181">**選擇性**，*預設為 false*</span><span class="sxs-lookup"><span data-stu-id="3453f-181">**optional**, *default - false*</span></span>
* <span data-ttu-id="3453f-182">`sourceMapRootPath`：用於儲存產生的來源對應檔的根路徑。</span><span class="sxs-lookup"><span data-stu-id="3453f-182">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="3453f-183">統合和縮製的建置時間執行</span><span class="sxs-lookup"><span data-stu-id="3453f-183">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="3453f-184">[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/)執行統合和縮製在建置階段，可讓 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="3453f-184">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="3453f-185">封裝會插入[MSBuild 目標](/visualstudio/msbuild/msbuild-targets)執行在建置和清除的時間。</span><span class="sxs-lookup"><span data-stu-id="3453f-185">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="3453f-186">*Bundleconfig.json*建置程序，以產生根據定義的組態輸出檔案的分析檔案。</span><span class="sxs-lookup"><span data-stu-id="3453f-186">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="3453f-187">BuildBundlerMinifier 所屬的社群導向的 GitHub 上的專案，Microsoft 不提供支援。</span><span class="sxs-lookup"><span data-stu-id="3453f-187">BuildBundlerMinifier belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="3453f-188">問題應該提報[此處](https://github.com/madskristensen/BundlerMinifier/issues)。</span><span class="sxs-lookup"><span data-stu-id="3453f-188">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3453f-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3453f-189">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3453f-190">新增*BuildBundlerMinifier*封裝至您的專案。</span><span class="sxs-lookup"><span data-stu-id="3453f-190">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="3453f-191">建置專案。</span><span class="sxs-lookup"><span data-stu-id="3453f-191">Build the project.</span></span> <span data-ttu-id="3453f-192">在 [輸出] 視窗中顯示下列訊息：</span><span class="sxs-lookup"><span data-stu-id="3453f-192">The following appears in the Output window:</span></span>

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

<span data-ttu-id="3453f-193">清除專案。</span><span class="sxs-lookup"><span data-stu-id="3453f-193">Clean the project.</span></span> <span data-ttu-id="3453f-194">在 [輸出] 視窗中顯示下列訊息：</span><span class="sxs-lookup"><span data-stu-id="3453f-194">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3453f-195">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="3453f-195">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="3453f-196">新增*BuildBundlerMinifier*封裝至您的專案：</span><span class="sxs-lookup"><span data-stu-id="3453f-196">Add the *BuildBundlerMinifier* package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="3453f-197">如果使用 ASP.NET Core 1.x，還原新加入的封裝：</span><span class="sxs-lookup"><span data-stu-id="3453f-197">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```console
dotnet restore
```

<span data-ttu-id="3453f-198">建置專案：</span><span class="sxs-lookup"><span data-stu-id="3453f-198">Build the project:</span></span>

```console
dotnet build
```

<span data-ttu-id="3453f-199">顯示下列內容：</span><span class="sxs-lookup"><span data-stu-id="3453f-199">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="3453f-200">清除專案：</span><span class="sxs-lookup"><span data-stu-id="3453f-200">Clean the project:</span></span>

```console
dotnet clean
```

<span data-ttu-id="3453f-201">即會出現下列輸出：</span><span class="sxs-lookup"><span data-stu-id="3453f-201">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="3453f-202">統合和縮製的臨機操作執行</span><span class="sxs-lookup"><span data-stu-id="3453f-202">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="3453f-203">可以執行統合和縮製工作臨機操作，而不需建置專案。</span><span class="sxs-lookup"><span data-stu-id="3453f-203">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="3453f-204">新增[BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet 封裝加入您的專案：</span><span class="sxs-lookup"><span data-stu-id="3453f-204">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> <span data-ttu-id="3453f-205">BundlerMinifier.Core 所屬的社群導向的 GitHub 上的專案，Microsoft 不提供支援。</span><span class="sxs-lookup"><span data-stu-id="3453f-205">BundlerMinifier.Core belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="3453f-206">問題應該提報[此處](https://github.com/madskristensen/BundlerMinifier/issues)。</span><span class="sxs-lookup"><span data-stu-id="3453f-206">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="3453f-207">此套件會擴充以包含.NET Core CLI *dotnet 套件組合*工具。</span><span class="sxs-lookup"><span data-stu-id="3453f-207">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="3453f-208">在 套件管理員主控台 (PMC) 視窗中，或在命令殼層中，可以執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="3453f-208">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```console
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="3453f-209">NuGet 套件管理員將相依性新增至 \*.csproj 檔，做為`<PackageReference />`節點。</span><span class="sxs-lookup"><span data-stu-id="3453f-209">NuGet Package Manager adds dependencies to the \*.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="3453f-210">`dotnet bundle`命令會向.NET Core CLI 時，才`<DotNetCliToolReference />`節點可用。</span><span class="sxs-lookup"><span data-stu-id="3453f-210">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="3453f-211">據此修改 \*.csproj 檔案。</span><span class="sxs-lookup"><span data-stu-id="3453f-211">Modify the \*.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="3453f-212">將檔案新增至工作流程</span><span class="sxs-lookup"><span data-stu-id="3453f-212">Add files to workflow</span></span>

<span data-ttu-id="3453f-213">請考慮範例，其中額外*custom.css*檔案會加入類似於下列：</span><span class="sxs-lookup"><span data-stu-id="3453f-213">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="3453f-214">要縮短*custom.css*及組合與其*site.css*成*site.min.css*檔案中，新增的相對路徑*bundleconfig.json*:</span><span class="sxs-lookup"><span data-stu-id="3453f-214">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="3453f-215">或者，您可以使用下列通用慣例模式：</span><span class="sxs-lookup"><span data-stu-id="3453f-215">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css))"]
> ```
>
> <span data-ttu-id="3453f-216">這個萬用字元模式比對所有 CSS 檔案，並排除縮短的檔案模式。</span><span class="sxs-lookup"><span data-stu-id="3453f-216">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="3453f-217">建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="3453f-217">Build the application.</span></span> <span data-ttu-id="3453f-218">開啟*site.min.css* ，並注意內容*custom.css*附加至檔案結尾。</span><span class="sxs-lookup"><span data-stu-id="3453f-218">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="3453f-219">環境式統合和縮製</span><span class="sxs-lookup"><span data-stu-id="3453f-219">Environment-based bundling and minification</span></span>

<span data-ttu-id="3453f-220">最佳做法，您的應用程式的統合和縮製檔案應在生產環境中。</span><span class="sxs-lookup"><span data-stu-id="3453f-220">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="3453f-221">在開發期間，原始的檔案，請以利偵錯應用程式。</span><span class="sxs-lookup"><span data-stu-id="3453f-221">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="3453f-222">指定要使用包含在您的網頁中的檔案[環境標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)檢視中。</span><span class="sxs-lookup"><span data-stu-id="3453f-222">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="3453f-223">環境標籤協助程式只會呈現其內容，當執行在特定[環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="3453f-223">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="3453f-224">下列`environment`中執行時，標籤會呈現未處理的 CSS 檔案`Development`環境：</span><span class="sxs-lookup"><span data-stu-id="3453f-224">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

::: moniker-end

<span data-ttu-id="3453f-225">下列`environment`以外的其他環境中執行時，標籤會呈現統合和縮製的 CSS 檔案`Development`。</span><span class="sxs-lookup"><span data-stu-id="3453f-225">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="3453f-226">例如，在執行`Production`或`Staging`觸發這些樣式表呈現：</span><span class="sxs-lookup"><span data-stu-id="3453f-226">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

::: moniker-end

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="3453f-227">使用 Gulp 從 bundleconfig.json</span><span class="sxs-lookup"><span data-stu-id="3453f-227">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="3453f-228">有一些的情況中的應用程式統合和縮製工作流程需要額外的處理。</span><span class="sxs-lookup"><span data-stu-id="3453f-228">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="3453f-229">範例包括映像最佳化、 快取 busting，及 CDN 資產處理。</span><span class="sxs-lookup"><span data-stu-id="3453f-229">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="3453f-230">為了滿足這些需求，您可以將轉換使用 Gulp 的統合和縮製工作流程。</span><span class="sxs-lookup"><span data-stu-id="3453f-230">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="3453f-231">使用 Bundler & Minifier 擴充功能</span><span class="sxs-lookup"><span data-stu-id="3453f-231">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="3453f-232">Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier)擴充功能會處理轉換成 Gulp。</span><span class="sxs-lookup"><span data-stu-id="3453f-232">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="3453f-233">Microsoft 提供不支援的 GitHub 上的社群導向專案所屬 Bundler & Minifier 延伸模組。</span><span class="sxs-lookup"><span data-stu-id="3453f-233">The Bundler & Minifier extension belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="3453f-234">問題應該提報[此處](https://github.com/madskristensen/BundlerMinifier/issues)。</span><span class="sxs-lookup"><span data-stu-id="3453f-234">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="3453f-235">以滑鼠右鍵按一下*bundleconfig.json*方案總管 中的檔案，然後選取**Bundler & Minifier** > **轉換至 Gulp...**:</span><span class="sxs-lookup"><span data-stu-id="3453f-235">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![轉換至 Gulp 的操作功能表項目](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="3453f-237">*Gulpfile.js*並*package.json*檔案會新增至專案。</span><span class="sxs-lookup"><span data-stu-id="3453f-237">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="3453f-238">支援[npm](https://www.npmjs.com/)中所列封裝*package.json*檔案的`devDependencies`安裝一節。</span><span class="sxs-lookup"><span data-stu-id="3453f-238">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="3453f-239">若要安裝 Gulp CLI 做為全域的相依性的 PMC 視窗中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="3453f-239">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="3453f-240">*Gulpfile.js*檔案讀取*bundleconfig.json*輸入、 輸出和設定檔。</span><span class="sxs-lookup"><span data-stu-id="3453f-240">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="3453f-241">手動轉換</span><span class="sxs-lookup"><span data-stu-id="3453f-241">Convert manually</span></span>

<span data-ttu-id="3453f-242">如果 Visual Studio 和/或 Bundler & Minifier 擴充功能無法使用，以手動方式將轉換。</span><span class="sxs-lookup"><span data-stu-id="3453f-242">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="3453f-243">新增*package.json*檔案，以下列`devDependencies`，至專案根目錄：</span><span class="sxs-lookup"><span data-stu-id="3453f-243">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="3453f-244">位於相同層級中執行下列命令來安裝相依性*package.json*:</span><span class="sxs-lookup"><span data-stu-id="3453f-244">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="3453f-245">安裝 Gulp CLI 做為全域的相依性：</span><span class="sxs-lookup"><span data-stu-id="3453f-245">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="3453f-246">複製*gulpfile.js*專案根目錄的檔案如下：</span><span class="sxs-lookup"><span data-stu-id="3453f-246">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="3453f-247">執行 Gulp 工作</span><span class="sxs-lookup"><span data-stu-id="3453f-247">Run Gulp tasks</span></span>

<span data-ttu-id="3453f-248">若要觸發 Gulp 縮製工作，才能在 Visual Studio 中建置專案，新增下列[MSBuild 目標](/visualstudio/msbuild/msbuild-targets)\*.csproj 檔案：</span><span class="sxs-lookup"><span data-stu-id="3453f-248">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the \*.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="3453f-249">在此範例中，任何工作定義內`MyPreCompileTarget`目標之前預先定義執行`Build`目標。</span><span class="sxs-lookup"><span data-stu-id="3453f-249">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="3453f-250">如下所示的輸出會顯示在 Visual Studio 的 [輸出] 視窗中：</span><span class="sxs-lookup"><span data-stu-id="3453f-250">Output similar to the following appears in Visual Studio's Output window:</span></span>

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


## <a name="additional-resources"></a><span data-ttu-id="3453f-251">其他資源</span><span class="sxs-lookup"><span data-stu-id="3453f-251">Additional resources</span></span>

* [<span data-ttu-id="3453f-252">使用 Grunt</span><span class="sxs-lookup"><span data-stu-id="3453f-252">Use Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="3453f-253">使用多重環境</span><span class="sxs-lookup"><span data-stu-id="3453f-253">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="3453f-254">標記協助程式</span><span class="sxs-lookup"><span data-stu-id="3453f-254">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
