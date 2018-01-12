---
title: "統合及縮製中 ASP.NET Core"
author: scottaddie
description: "了解如何最佳化 ASP.NET Core web 應用程式中的靜態資源套用統合及縮製的技術。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bundling-and-minification
ms.openlocfilehash: ac8e7fee7600dabb8f4970b5bf87ad7a57ebf17f
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/11/2018
---
# <a name="bundling-and-minification"></a><span data-ttu-id="97fa2-103">統合及縮製</span><span class="sxs-lookup"><span data-stu-id="97fa2-103">Bundling and minification</span></span>

<span data-ttu-id="97fa2-104">作者：[Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="97fa2-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="97fa2-105">這篇文章會說明套用統合及縮製，包括如何使用這些功能，與 ASP.NET Core web 應用程式的優點。</span><span class="sxs-lookup"><span data-stu-id="97fa2-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="97fa2-106">統合及縮製為何？</span><span class="sxs-lookup"><span data-stu-id="97fa2-106">What is bundling and minification?</span></span>

<span data-ttu-id="97fa2-107">組合和縮製都可套用 web 應用程式中的兩個不同的效能最佳化。</span><span class="sxs-lookup"><span data-stu-id="97fa2-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="97fa2-108">一起使用，統合及縮製改善效能降低伺服器的要求數目以及減少要求的靜態資產的大小。</span><span class="sxs-lookup"><span data-stu-id="97fa2-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="97fa2-109">統合及縮製主要改善第一個頁面要求載入時間。</span><span class="sxs-lookup"><span data-stu-id="97fa2-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="97fa2-110">一旦要求的網頁上，瀏覽器快取靜態資產 （JavaScript、 CSS 和映像）。</span><span class="sxs-lookup"><span data-stu-id="97fa2-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="97fa2-111">因此，統合及縮製不改善效能時要求相同的頁面或頁面，要求相同的資產在相同網站。</span><span class="sxs-lookup"><span data-stu-id="97fa2-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="97fa2-112">如果您不需要設定到期在您的資產上正確的標頭並如果您不使用統合及縮製，瀏覽器的有效期限啟發學習法標示資產過時之後幾天。</span><span class="sxs-lookup"><span data-stu-id="97fa2-112">If you don't set the expires header correctly on your assets, and if you don’t use bundling and minification, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="97fa2-113">此外，瀏覽器會需要為每個資產的驗證要求。</span><span class="sxs-lookup"><span data-stu-id="97fa2-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="97fa2-114">在此情況下，統合及縮製提供改進效能，即使第一個頁面要求。</span><span class="sxs-lookup"><span data-stu-id="97fa2-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="97fa2-115">結合在一起</span><span class="sxs-lookup"><span data-stu-id="97fa2-115">Bundling</span></span>

<span data-ttu-id="97fa2-116">結合在一起將多個檔案合併成單一檔案。</span><span class="sxs-lookup"><span data-stu-id="97fa2-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="97fa2-117">結合在一起，減少所需呈現 web 資產，例如網頁伺服器要求的數目。</span><span class="sxs-lookup"><span data-stu-id="97fa2-117">Bundling reduces the number of server requests which are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="97fa2-118">您可以建立任意數目的個別組合，專為 CSS、 JavaScript 等。較少的檔案表示更少的 HTTP 要求從瀏覽器至伺服器或提供您的應用程式的服務。</span><span class="sxs-lookup"><span data-stu-id="97fa2-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="97fa2-119">這會導致更佳的第一個頁面負載效能。</span><span class="sxs-lookup"><span data-stu-id="97fa2-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="97fa2-120">縮小</span><span class="sxs-lookup"><span data-stu-id="97fa2-120">Minification</span></span>

<span data-ttu-id="97fa2-121">縮製從程式碼移除不必要的字元，而不需變更功能。</span><span class="sxs-lookup"><span data-stu-id="97fa2-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="97fa2-122">結果會是重要的大小降低要求資產 （例如 CSS、 影像和 JavaScript 檔案）。</span><span class="sxs-lookup"><span data-stu-id="97fa2-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="97fa2-123">常見的縮製的副作用包括縮短成一個字元的變數名稱，以及移除註解和不必要的空白字元。</span><span class="sxs-lookup"><span data-stu-id="97fa2-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="97fa2-124">請考慮下列的 JavaScript 函式：</span><span class="sxs-lookup"><span data-stu-id="97fa2-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="97fa2-125">縮製減少函式所示：</span><span class="sxs-lookup"><span data-stu-id="97fa2-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="97fa2-126">移除註解和不必要的空白字元，除了下列的參數和變數名稱已命名，如下所示：</span><span class="sxs-lookup"><span data-stu-id="97fa2-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="97fa2-127">原始</span><span class="sxs-lookup"><span data-stu-id="97fa2-127">Original</span></span> | <span data-ttu-id="97fa2-128">已重新命名</span><span class="sxs-lookup"><span data-stu-id="97fa2-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="97fa2-129">統合及縮製的影響</span><span class="sxs-lookup"><span data-stu-id="97fa2-129">Impact of bundling and minification</span></span>

<span data-ttu-id="97fa2-130">下表摘要列出個別資產的載入和使用統合及縮製之間的差異：</span><span class="sxs-lookup"><span data-stu-id="97fa2-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="97fa2-131">動作</span><span class="sxs-lookup"><span data-stu-id="97fa2-131">Action</span></span> | <span data-ttu-id="97fa2-132">使用 B/M</span><span class="sxs-lookup"><span data-stu-id="97fa2-132">With B/M</span></span> | <span data-ttu-id="97fa2-133">沒有 B/M</span><span class="sxs-lookup"><span data-stu-id="97fa2-133">Without B/M</span></span> | <span data-ttu-id="97fa2-134">變更</span><span class="sxs-lookup"><span data-stu-id="97fa2-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="97fa2-135">檔案要求</span><span class="sxs-lookup"><span data-stu-id="97fa2-135">File Requests</span></span>  | <span data-ttu-id="97fa2-136">7</span><span class="sxs-lookup"><span data-stu-id="97fa2-136">7</span></span>   | <span data-ttu-id="97fa2-137">18</span><span class="sxs-lookup"><span data-stu-id="97fa2-137">18</span></span>     | <span data-ttu-id="97fa2-138">157%</span><span class="sxs-lookup"><span data-stu-id="97fa2-138">157%</span></span>
<span data-ttu-id="97fa2-139">傳送的 KB</span><span class="sxs-lookup"><span data-stu-id="97fa2-139">KB Transferred</span></span> | <span data-ttu-id="97fa2-140">156</span><span class="sxs-lookup"><span data-stu-id="97fa2-140">156</span></span> | <span data-ttu-id="97fa2-141">264.68</span><span class="sxs-lookup"><span data-stu-id="97fa2-141">264.68</span></span> | <span data-ttu-id="97fa2-142">70%</span><span class="sxs-lookup"><span data-stu-id="97fa2-142">70%</span></span>
<span data-ttu-id="97fa2-143">載入時間 （毫秒）</span><span class="sxs-lookup"><span data-stu-id="97fa2-143">Load Time (ms)</span></span> | <span data-ttu-id="97fa2-144">885</span><span class="sxs-lookup"><span data-stu-id="97fa2-144">885</span></span> | <span data-ttu-id="97fa2-145">2360</span><span class="sxs-lookup"><span data-stu-id="97fa2-145">2360</span></span>   | <span data-ttu-id="97fa2-146">167%</span><span class="sxs-lookup"><span data-stu-id="97fa2-146">167%</span></span>

<span data-ttu-id="97fa2-147">瀏覽器會相當詳細資訊，關於 HTTP 要求標頭。</span><span class="sxs-lookup"><span data-stu-id="97fa2-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="97fa2-148">所傳送的總位元組度量所看到的大幅降低結合在一起。</span><span class="sxs-lookup"><span data-stu-id="97fa2-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="97fa2-149">載入時間顯示有明顯的改進，不過此範例會在本機執行。</span><span class="sxs-lookup"><span data-stu-id="97fa2-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="97fa2-150">統合及縮製使用資產透過網路傳輸時，會實現更高的效能提升。</span><span class="sxs-lookup"><span data-stu-id="97fa2-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="97fa2-151">選擇統合及縮製的策略</span><span class="sxs-lookup"><span data-stu-id="97fa2-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="97fa2-152">MVC 和 Razor 頁面的專案範本提供的方塊外方案統合及縮製的 JSON 組態檔所組成。</span><span class="sxs-lookup"><span data-stu-id="97fa2-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="97fa2-153">協力廠商工具，例如[Gulp](xref:client-side/using-gulp)和[Grunt](xref:client-side/using-grunt)工作的說明，完成更多的複雜性與相同的工作。</span><span class="sxs-lookup"><span data-stu-id="97fa2-153">Third-party tools, such as the [Gulp](xref:client-side/using-gulp) and [Grunt](xref:client-side/using-grunt) task runners, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="97fa2-154">協力廠商工具的絕佳符合時是您的開發工作流程需要處理超出統合及縮製&mdash;例如 linting 和映像最佳化。</span><span class="sxs-lookup"><span data-stu-id="97fa2-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="97fa2-155">使用設計階段組合和縮製，應用程式的部署之前，先建立這些縮短的檔案。</span><span class="sxs-lookup"><span data-stu-id="97fa2-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="97fa2-156">統合及縮小部署之前提供的優點減少的伺服器負載。</span><span class="sxs-lookup"><span data-stu-id="97fa2-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="97fa2-157">不過，務必辨識該設計階段組合和縮製增加建置複雜，而且只適用於靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="97fa2-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="97fa2-158">統合及縮製的設定</span><span class="sxs-lookup"><span data-stu-id="97fa2-158">Configure bundling and minification</span></span>

<span data-ttu-id="97fa2-159">MVC 和 Razor 頁面 專案範本提供*bundleconfig.json*組態檔會定義每個組合的選項。</span><span class="sxs-lookup"><span data-stu-id="97fa2-159">The MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file which defines the options for each bundle.</span></span> <span data-ttu-id="97fa2-160">依預設，單一組合組態定義的自訂 javascript (*wwwroot/js/site.js*) 和樣式表 (*wwwroot/css/site.css*) 檔案：</span><span class="sxs-lookup"><span data-stu-id="97fa2-160">By default, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="97fa2-161">設定選項包括：</span><span class="sxs-lookup"><span data-stu-id="97fa2-161">Configuration options include:</span></span>

* <span data-ttu-id="97fa2-162">`outputFileName`: 要輸出的組合檔案的名稱。</span><span class="sxs-lookup"><span data-stu-id="97fa2-162">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="97fa2-163">可包含相對路徑*bundleconfig.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="97fa2-163">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="97fa2-164">**所需**</span><span class="sxs-lookup"><span data-stu-id="97fa2-164">**required**</span></span>
* <span data-ttu-id="97fa2-165">`inputFiles`： 要配套起來的檔案陣列。</span><span class="sxs-lookup"><span data-stu-id="97fa2-165">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="97fa2-166">這些是在組態檔的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="97fa2-166">These are relative paths to the configuration file.</span></span> <span data-ttu-id="97fa2-167">**選擇性**，\* 空值會導致空的輸出檔案。</span><span class="sxs-lookup"><span data-stu-id="97fa2-167">**optional**, \*an empty value results in an empty output file.</span></span> <span data-ttu-id="97fa2-168">[通用慣例](http://www.tldp.org/LDP/abs/html/globbingref.html)支援的模式。</span><span class="sxs-lookup"><span data-stu-id="97fa2-168">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="97fa2-169">`minify`： 輸出型別縮製選項。</span><span class="sxs-lookup"><span data-stu-id="97fa2-169">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="97fa2-170">**選擇性**，*預設值-`minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="97fa2-170">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="97fa2-171">每個輸出檔案類型有組態選項。</span><span class="sxs-lookup"><span data-stu-id="97fa2-171">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="97fa2-172">CSS 縮短程式</span><span class="sxs-lookup"><span data-stu-id="97fa2-172">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="97fa2-173">JavaScript 縮短程式</span><span class="sxs-lookup"><span data-stu-id="97fa2-173">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="97fa2-174">HTML 縮短程式</span><span class="sxs-lookup"><span data-stu-id="97fa2-174">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="97fa2-175">`includeInProject`： 旗標，指出是否要將產生的檔案加入至專案檔。</span><span class="sxs-lookup"><span data-stu-id="97fa2-175">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="97fa2-176">**選擇性**，*預設為 false*</span><span class="sxs-lookup"><span data-stu-id="97fa2-176">**optional**, *default - false*</span></span>
* <span data-ttu-id="97fa2-177">`sourceMap`： 指出是否要產生將配套的檔案的來源對應的旗標。</span><span class="sxs-lookup"><span data-stu-id="97fa2-177">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="97fa2-178">**選擇性**，*預設為 false*</span><span class="sxs-lookup"><span data-stu-id="97fa2-178">**optional**, *default - false*</span></span>
* <span data-ttu-id="97fa2-179">`sourceMapRootPath`： 用於儲存產生的來源對應檔的根路徑。</span><span class="sxs-lookup"><span data-stu-id="97fa2-179">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="97fa2-180">建置時間執行的統合及縮製</span><span class="sxs-lookup"><span data-stu-id="97fa2-180">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="97fa2-181">[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet 封裝啟用的結合在一起執行，以及在建置階段縮製。</span><span class="sxs-lookup"><span data-stu-id="97fa2-181">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="97fa2-182">封裝會插入[MSBuild 目標](/visualstudio/msbuild/msbuild-targets)在建置和清除的時間執行的。</span><span class="sxs-lookup"><span data-stu-id="97fa2-182">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="97fa2-183">*Bundleconfig.json*建置程序來產生輸出檔案，根據定義的組態分析檔案。</span><span class="sxs-lookup"><span data-stu-id="97fa2-183">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="97fa2-184">BuildBundlerMinifier 所屬的社群導向的專案，Microsoft 不提供任何支援的 GitHub 上。</span><span class="sxs-lookup"><span data-stu-id="97fa2-184">BuildBundlerMinifier belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="97fa2-185">問題應必填欄位[這裡](https://github.com/madskristensen/BundlerMinifier/issues)。</span><span class="sxs-lookup"><span data-stu-id="97fa2-185">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="97fa2-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="97fa2-186">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="97fa2-187">新增*BuildBundlerMinifier*封裝至您的專案。</span><span class="sxs-lookup"><span data-stu-id="97fa2-187">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="97fa2-188">建置專案。</span><span class="sxs-lookup"><span data-stu-id="97fa2-188">Build the project.</span></span> <span data-ttu-id="97fa2-189">在 [輸出] 視窗中顯示下列訊息：</span><span class="sxs-lookup"><span data-stu-id="97fa2-189">The following appears in the Output window:</span></span>

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

<span data-ttu-id="97fa2-190">清除專案。</span><span class="sxs-lookup"><span data-stu-id="97fa2-190">Clean the project.</span></span> <span data-ttu-id="97fa2-191">在 [輸出] 視窗中顯示下列訊息：</span><span class="sxs-lookup"><span data-stu-id="97fa2-191">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="97fa2-192">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="97fa2-192">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="97fa2-193">新增*BuildBundlerMinifier*封裝至您的專案：</span><span class="sxs-lookup"><span data-stu-id="97fa2-193">Add the *BuildBundlerMinifier* package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="97fa2-194">如果使用 ASP.NET Core 1.x，還原新加入的封裝：</span><span class="sxs-lookup"><span data-stu-id="97fa2-194">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```console
dotnet restore
```

<span data-ttu-id="97fa2-195">建置專案：</span><span class="sxs-lookup"><span data-stu-id="97fa2-195">Build the project:</span></span>

```console
dotnet build
```

<span data-ttu-id="97fa2-196">顯示下列內容：</span><span class="sxs-lookup"><span data-stu-id="97fa2-196">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="97fa2-197">清除專案：</span><span class="sxs-lookup"><span data-stu-id="97fa2-197">Clean the project:</span></span>

```console
dotnet clean
```

<span data-ttu-id="97fa2-198">出現下列輸出：</span><span class="sxs-lookup"><span data-stu-id="97fa2-198">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="97fa2-199">統合及縮製的臨機操作執行</span><span class="sxs-lookup"><span data-stu-id="97fa2-199">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="97fa2-200">您可根據臨機操作，執行統合及縮製的工作，而不需建置專案。</span><span class="sxs-lookup"><span data-stu-id="97fa2-200">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="97fa2-201">新增[BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet 封裝加入您的專案：</span><span class="sxs-lookup"><span data-stu-id="97fa2-201">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> <span data-ttu-id="97fa2-202">BundlerMinifier.Core 所屬的社群導向的專案，Microsoft 不提供任何支援的 GitHub 上。</span><span class="sxs-lookup"><span data-stu-id="97fa2-202">BundlerMinifier.Core belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="97fa2-203">問題應必填欄位[這裡](https://github.com/madskristensen/BundlerMinifier/issues)。</span><span class="sxs-lookup"><span data-stu-id="97fa2-203">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="97fa2-204">此套件會擴充以包含.NET Core CLI *dotnet 配套*工具。</span><span class="sxs-lookup"><span data-stu-id="97fa2-204">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="97fa2-205">在 封裝管理員主控台 (PMC) 視窗或在命令殼層中，可以執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="97fa2-205">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```console
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="97fa2-206">NuGet 套件管理員將相依性加入至 \*.csproj 檔案做為`<PackageReference />`節點。</span><span class="sxs-lookup"><span data-stu-id="97fa2-206">NuGet Package Manager adds dependencies to the \*.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="97fa2-207">`dotnet bundle`命令已向.NET Core CLI 時，才`<DotNetCliToolReference />` 節點可用。</span><span class="sxs-lookup"><span data-stu-id="97fa2-207">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="97fa2-208">適當地修改 \*.csproj 檔案。</span><span class="sxs-lookup"><span data-stu-id="97fa2-208">Modify the \*.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="97fa2-209">將檔案加入至工作流程</span><span class="sxs-lookup"><span data-stu-id="97fa2-209">Add files to workflow</span></span>

<span data-ttu-id="97fa2-210">請考慮使用範例中的額外*custom.css*檔案會加入與下列類似下列：</span><span class="sxs-lookup"><span data-stu-id="97fa2-210">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="97fa2-211">要縮短*custom.css*和組合使用*site.css*到*site.min.css*檔案中，加入相對路徑*bundleconfig.json*:</span><span class="sxs-lookup"><span data-stu-id="97fa2-211">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="97fa2-212">或者，您可以使用下列通用慣例模式：</span><span class="sxs-lookup"><span data-stu-id="97fa2-212">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]
> ```
>
> <span data-ttu-id="97fa2-213">此通用慣例模式比對所有 CSS 檔案，但不包括縮短的檔案模式。</span><span class="sxs-lookup"><span data-stu-id="97fa2-213">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="97fa2-214">建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="97fa2-214">Build the application.</span></span> <span data-ttu-id="97fa2-215">開啟*site.min.css*注意到的內容和*custom.css*附加至檔案結尾。</span><span class="sxs-lookup"><span data-stu-id="97fa2-215">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="97fa2-216">環境架構統合及縮製</span><span class="sxs-lookup"><span data-stu-id="97fa2-216">Environment-based bundling and minification</span></span>

<span data-ttu-id="97fa2-217">最佳做法，您的應用程式的配套和縮短檔應該用於實際執行環境。</span><span class="sxs-lookup"><span data-stu-id="97fa2-217">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="97fa2-218">在開發期間，原始的檔案，請為方便偵錯應用程式。</span><span class="sxs-lookup"><span data-stu-id="97fa2-218">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="97fa2-219">指定要包含在您的網頁中使用哪些檔案[環境標記協助程式](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)在檢視中。</span><span class="sxs-lookup"><span data-stu-id="97fa2-219">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="97fa2-220">環境標記協助程式只會呈現其內容，在特定中執行時[環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="97fa2-220">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="97fa2-221">下列`environment`標記會呈現未處理的 CSS 檔案中執行時`Development`環境：</span><span class="sxs-lookup"><span data-stu-id="97fa2-221">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="97fa2-222">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="97fa2-222">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="97fa2-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="97fa2-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

---

<span data-ttu-id="97fa2-224">下列`environment`以外的環境中執行時，標記會轉譯配套並縮短的 CSS 檔案`Development`。</span><span class="sxs-lookup"><span data-stu-id="97fa2-224">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="97fa2-225">例如，在執行`Production`或`Staging`呈現這些樣式表的觸發程序：</span><span class="sxs-lookup"><span data-stu-id="97fa2-225">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="97fa2-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="97fa2-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="97fa2-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="97fa2-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

---

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="97fa2-228">使用從 Gulp bundleconfig.json</span><span class="sxs-lookup"><span data-stu-id="97fa2-228">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="97fa2-229">一些情況下，應用程式的統合及縮製的工作流程需要額外的處理。</span><span class="sxs-lookup"><span data-stu-id="97fa2-229">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="97fa2-230">範例包括映像最佳化、 快取 busting，及 CDN 資產處理。</span><span class="sxs-lookup"><span data-stu-id="97fa2-230">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="97fa2-231">為了滿足這些需求，您可以將轉換使用 Gulp 統合及縮製工作流程。</span><span class="sxs-lookup"><span data-stu-id="97fa2-231">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="97fa2-232">使用搭配程式 （& s） 縮短程式延伸模組</span><span class="sxs-lookup"><span data-stu-id="97fa2-232">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="97fa2-233">Visual Studio[搭配程式 （& s) 縮短程式](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier)延伸模組會處理 Gulp 的轉換。</span><span class="sxs-lookup"><span data-stu-id="97fa2-233">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="97fa2-234">搭配程式 （& s） 縮短程式副檔名所屬的社群導向的專案，Microsoft 不提供任何支援的 GitHub 上。</span><span class="sxs-lookup"><span data-stu-id="97fa2-234">The Bundler & Minifier extension belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="97fa2-235">問題應必填欄位[這裡](https://github.com/madskristensen/BundlerMinifier/issues)。</span><span class="sxs-lookup"><span data-stu-id="97fa2-235">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="97fa2-236">以滑鼠右鍵按一下*bundleconfig.json*檔案在 [方案總管]，然後選取**搭配程式 （& s) 縮短程式** > **轉換至 Gulp...**:</span><span class="sxs-lookup"><span data-stu-id="97fa2-236">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![轉換至 Gulp 的操作功能表項目](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="97fa2-238">*Gulpfile.js*和*package.json*檔案會新增至專案。</span><span class="sxs-lookup"><span data-stu-id="97fa2-238">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="97fa2-239">支援[npm](https://www.npmjs.com/)封裝中所列*package.json*檔案的`devDependencies`安裝 > 一節。</span><span class="sxs-lookup"><span data-stu-id="97fa2-239">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="97fa2-240">若要安裝的 Gulp CLI 為全域的相依性 PMC 視窗執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="97fa2-240">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="97fa2-241">*Gulpfile.js*檔案讀取*bundleconfig.json*輸入、 輸出和設定檔。</span><span class="sxs-lookup"><span data-stu-id="97fa2-241">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="97fa2-242">手動轉換</span><span class="sxs-lookup"><span data-stu-id="97fa2-242">Convert manually</span></span>

<span data-ttu-id="97fa2-243">如果 Visual Studio 和 （或） 搭配程式 （& s） 縮短程式擴充功能無法使用，手動轉換。</span><span class="sxs-lookup"><span data-stu-id="97fa2-243">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="97fa2-244">新增*package.json*檔案，以下列`devDependencies`，專案根目錄：</span><span class="sxs-lookup"><span data-stu-id="97fa2-244">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="97fa2-245">在相同的層級執行下列命令來安裝相依性*package.json*:</span><span class="sxs-lookup"><span data-stu-id="97fa2-245">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="97fa2-246">做為全域的相依性安裝的 Gulp CLI:</span><span class="sxs-lookup"><span data-stu-id="97fa2-246">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="97fa2-247">複製*gulpfile.js*專案根目錄底下的檔案：</span><span class="sxs-lookup"><span data-stu-id="97fa2-247">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="97fa2-248">執行 Gulp 工作</span><span class="sxs-lookup"><span data-stu-id="97fa2-248">Run Gulp tasks</span></span>

<span data-ttu-id="97fa2-249">若要觸發 Gulp 縮製工作，在專案之前建置 Visual Studio 中，加入下列[MSBuild 目標](/visualstudio/msbuild/msbuild-targets)\*.csproj 檔案：</span><span class="sxs-lookup"><span data-stu-id="97fa2-249">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the \*.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="97fa2-250">在此範例中，任何工作定義內`MyPreCompileTarget`執行之前的預先定義的目標`Build`目標。</span><span class="sxs-lookup"><span data-stu-id="97fa2-250">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="97fa2-251">Visual Studio 的 [輸出] 視窗中會出現類似下面的輸出：</span><span class="sxs-lookup"><span data-stu-id="97fa2-251">Output similar to the following appears in Visual Studio's Output window:</span></span>

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

<span data-ttu-id="97fa2-252">或者，Visual Studio 工作執行器總管可能用來將 Gulp 工作繫結至特定的 Visual Studio 事件。</span><span class="sxs-lookup"><span data-stu-id="97fa2-252">Alternatively, Visual Studio's Task Runner Explorer may be used to bind Gulp tasks to specific Visual Studio events.</span></span> <span data-ttu-id="97fa2-253">請參閱[執行預設工作](xref:client-side/using-gulp#running-default-tasks)如需這麼做可指示。</span><span class="sxs-lookup"><span data-stu-id="97fa2-253">See [Running default tasks](xref:client-side/using-gulp#running-default-tasks) for instructions on doing that.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="97fa2-254">其他資源</span><span class="sxs-lookup"><span data-stu-id="97fa2-254">Additional resources</span></span>

* [<span data-ttu-id="97fa2-255">使用 Gulp</span><span class="sxs-lookup"><span data-stu-id="97fa2-255">Using Gulp</span></span>](xref:client-side/using-gulp)
* [<span data-ttu-id="97fa2-256">使用 Grunt</span><span class="sxs-lookup"><span data-stu-id="97fa2-256">Using Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="97fa2-257">使用多個環境</span><span class="sxs-lookup"><span data-stu-id="97fa2-257">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="97fa2-258">標記協助程式</span><span class="sxs-lookup"><span data-stu-id="97fa2-258">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
