---
title: 捆綁和整理 ASP.NET 核心中的靜態資產
author: scottaddie
description: 瞭解如何透過應用捆綁和最小化技術來優化ASP.NET核心 Web 應用程式中的靜態資源。
ms.author: scaddie
ms.custom: mvc
ms.date: 04/15/2020
uid: client-side/bundling-and-minification
ms.openlocfilehash: 670ac6a96c3affd2b2ac699836f536aea7d85ff3
ms.sourcegitcommit: 77c046331f3d633d7cc247ba77e58b89e254f487
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "81488685"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a><span data-ttu-id="ecf7c-103">捆綁和整理 ASP.NET 核心中的靜態資產</span><span class="sxs-lookup"><span data-stu-id="ecf7c-103">Bundle and minify static assets in ASP.NET Core</span></span>

<span data-ttu-id="ecf7c-104">由[斯科特·阿迪](https://twitter.com/Scott_Addie)[和大衛·派恩](https://twitter.com/davidpine7)</span><span class="sxs-lookup"><span data-stu-id="ecf7c-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [David Pine](https://twitter.com/davidpine7)</span></span>

<span data-ttu-id="ecf7c-105">本文介紹了應用捆綁和小化的好處,包括這些功能如何與ASP.NET核心 Web 應用一起使用。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="ecf7c-106">什麼是捆綁和小化</span><span class="sxs-lookup"><span data-stu-id="ecf7c-106">What is bundling and minification</span></span>

<span data-ttu-id="ecf7c-107">捆綁和分小是兩種不同的性能優化,您可以應用於 Web 應用。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="ecf7c-108">捆綁和縮小相結合,通過減少伺服器請求的數量和減少請求的靜態資產的大小來提高性能。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="ecf7c-109">捆綁和小化主要改善第一頁請求載入時間。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="ecf7c-110">請求網頁後,瀏覽器將緩存靜態資產(JAVAScript、CSS 和圖像)。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="ecf7c-111">因此,在同一網站上請求同一頁面或頁面時,捆綁和小化不會提高性能。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="ecf7c-112">如果過期標頭未正確設置在資產上,並且不使用捆綁和小化,瀏覽器的新鮮感啟發式將標記幾天后資產過時。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-112">If the expires header isn't set correctly on the assets and if bundling and minification isn't used, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="ecf7c-113">此外,瀏覽器需要每個資產的驗證請求。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="ecf7c-114">在這種情況下,捆綁和分明即使在第一頁請求后也能提高性能。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="ecf7c-115">捆綁</span><span class="sxs-lookup"><span data-stu-id="ecf7c-115">Bundling</span></span>

<span data-ttu-id="ecf7c-116">統合可將多個檔案合併成單一檔案。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="ecf7c-117">捆綁減少了呈現 Web 資產(如網頁)所需的伺服器請求數。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-117">Bundling reduces the number of server requests that are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="ecf7c-118">您可以為 CSS、JavaScript 等創建任意數量的單個捆綁包。檔越少,從瀏覽器到伺服器或提供應用程式的服務的 HTTP 請求更少。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="ecf7c-119">這提高了第一頁的載入性能。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="ecf7c-120">縮製</span><span class="sxs-lookup"><span data-stu-id="ecf7c-120">Minification</span></span>

<span data-ttu-id="ecf7c-121">敏化可在不更改功能的情況下從代碼中刪除不必要的字元。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="ecf7c-122">結果是請求的資產(如 CSS、映射和 JAVAScript 檔)的大小顯著縮減。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="ecf7c-123">最小化的常見副作用包括將變數名稱縮短為一個字元,並刪除註釋和不必要的空格。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="ecf7c-124">請考慮以下 JavaScript 函數:</span><span class="sxs-lookup"><span data-stu-id="ecf7c-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="ecf7c-125">最小化將功能減少到以下內容:</span><span class="sxs-lookup"><span data-stu-id="ecf7c-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="ecf7c-126">除了刪除註解和不必要的空格外,以下參數和變數名稱也重命名如下:</span><span class="sxs-lookup"><span data-stu-id="ecf7c-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="ecf7c-127">原始</span><span class="sxs-lookup"><span data-stu-id="ecf7c-127">Original</span></span> | <span data-ttu-id="ecf7c-128">已重新命名</span><span class="sxs-lookup"><span data-stu-id="ecf7c-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="ecf7c-129">捆綁和小化的影響</span><span class="sxs-lookup"><span data-stu-id="ecf7c-129">Impact of bundling and minification</span></span>

<span data-ttu-id="ecf7c-130">下表概述了單獨載入資產和使用捆綁和最小化之間的區別:</span><span class="sxs-lookup"><span data-stu-id="ecf7c-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="ecf7c-131">動作</span><span class="sxs-lookup"><span data-stu-id="ecf7c-131">Action</span></span> | <span data-ttu-id="ecf7c-132">帶 B/M</span><span class="sxs-lookup"><span data-stu-id="ecf7c-132">With B/M</span></span> | <span data-ttu-id="ecf7c-133">無 B/M</span><span class="sxs-lookup"><span data-stu-id="ecf7c-133">Without B/M</span></span> | <span data-ttu-id="ecf7c-134">變更</span><span class="sxs-lookup"><span data-stu-id="ecf7c-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="ecf7c-135">檔案要求</span><span class="sxs-lookup"><span data-stu-id="ecf7c-135">File Requests</span></span>  | <span data-ttu-id="ecf7c-136">7</span><span class="sxs-lookup"><span data-stu-id="ecf7c-136">7</span></span>   | <span data-ttu-id="ecf7c-137">18</span><span class="sxs-lookup"><span data-stu-id="ecf7c-137">18</span></span>     | <span data-ttu-id="ecf7c-138">157%</span><span class="sxs-lookup"><span data-stu-id="ecf7c-138">157%</span></span>
<span data-ttu-id="ecf7c-139">KB 已傳輸</span><span class="sxs-lookup"><span data-stu-id="ecf7c-139">KB Transferred</span></span> | <span data-ttu-id="ecf7c-140">156</span><span class="sxs-lookup"><span data-stu-id="ecf7c-140">156</span></span> | <span data-ttu-id="ecf7c-141">264.68</span><span class="sxs-lookup"><span data-stu-id="ecf7c-141">264.68</span></span> | <span data-ttu-id="ecf7c-142">70%</span><span class="sxs-lookup"><span data-stu-id="ecf7c-142">70%</span></span>
<span data-ttu-id="ecf7c-143">載入時間 (毫秒)</span><span class="sxs-lookup"><span data-stu-id="ecf7c-143">Load Time (ms)</span></span> | <span data-ttu-id="ecf7c-144">885</span><span class="sxs-lookup"><span data-stu-id="ecf7c-144">885</span></span> | <span data-ttu-id="ecf7c-145">2360</span><span class="sxs-lookup"><span data-stu-id="ecf7c-145">2360</span></span>   | <span data-ttu-id="ecf7c-146">167%</span><span class="sxs-lookup"><span data-stu-id="ecf7c-146">167%</span></span>

<span data-ttu-id="ecf7c-147">瀏覽器在 HTTP 請求標頭方面相當詳細。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="ecf7c-148">捆綁時發送的位元組總數指標顯著減少。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="ecf7c-149">載入時間顯示顯著改進,但此範例在本機運行。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="ecf7c-150">使用捆綁和計量與通過網路傳輸的資產進行捆綁和計量時,可實現更大的性能提升。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="ecf7c-151">選擇捆綁與最小化原則</span><span class="sxs-lookup"><span data-stu-id="ecf7c-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="ecf7c-152">MVC 和 Razor Pages 專案範本提供了一個解決方案,用於捆綁和整理 JSON 配置檔。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-152">The MVC and Razor Pages project templates provide a solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="ecf7c-153">第三方工具(如[Grunt](xref:client-side/using-grunt)任務運行者)以稍微複雜一點完成相同的任務。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-153">Third-party tools, such as the [Grunt](xref:client-side/using-grunt) task runner, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="ecf7c-154">當您的開發工作流需要超越捆綁和縮編&mdash;(如鑲合和圖像優化)的處理時,第三方工具非常合適。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="ecf7c-155">通過使用設計時捆綁和小化,在應用部署之前創建已創建已化的檔。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="ecf7c-156">部署前捆綁和簡化可提供減少伺服器負載的優勢。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="ecf7c-157">但是,請務必認識到,設計時捆綁和小化會增加構建複雜性,並且僅適用於靜態檔。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="ecf7c-158">設定捆繫與最小化</span><span class="sxs-lookup"><span data-stu-id="ecf7c-158">Configure bundling and minification</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="ecf7c-159">在ASP.NET Core 2.0 或更早版本中,MVC 和 Razor Pages 專案範本提供一個*bundleconfig.json*設定檔,用於定義每個捆綁套件的選項:</span><span class="sxs-lookup"><span data-stu-id="ecf7c-159">In ASP.NET Core 2.0 or earlier, the MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file that defines the options for each bundle:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ecf7c-160">在ASP.NET Core 2.1 或更高版本中,向MVC或Razor Pages專案根添加新的 JSON 檔,名為*bundleconfig.json。*</span><span class="sxs-lookup"><span data-stu-id="ecf7c-160">In ASP.NET Core 2.1 or later, add a new JSON file, named *bundleconfig.json*, to the MVC or Razor Pages project root.</span></span> <span data-ttu-id="ecf7c-161">將以下 JSON 作為起點在該檔中:</span><span class="sxs-lookup"><span data-stu-id="ecf7c-161">Include the following JSON in that file as a starting point:</span></span>

::: moniker-end

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="ecf7c-162">*bundleconfig.json*檔定義了每個捆綁包的選項。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-162">The *bundleconfig.json* file defines the options for each bundle.</span></span> <span data-ttu-id="ecf7c-163">在前面的範例中,為自定義 JavaScript *(wwwroot/js/site.js)* 和樣式表 (*wwwroot/css/site.css)* 檔定義了單個捆綁包配置。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-163">In the preceding example, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files.</span></span>

<span data-ttu-id="ecf7c-164">設定選項包括：</span><span class="sxs-lookup"><span data-stu-id="ecf7c-164">Configuration options include:</span></span>

* <span data-ttu-id="ecf7c-165">`outputFileName`:要輸出的捆綁檔的名稱。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-165">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="ecf7c-166">可以包含*來自 bundleconfig.json*檔中的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-166">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="ecf7c-167">**必填**</span><span class="sxs-lookup"><span data-stu-id="ecf7c-167">**required**</span></span>
* <span data-ttu-id="ecf7c-168">`inputFiles`:要捆綁在一起的文件陣組。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-168">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="ecf7c-169">這些是配置檔的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-169">These are relative paths to the configuration file.</span></span> <span data-ttu-id="ecf7c-170">**可選**,\一個空值會導致輸出檔為空。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-170">**optional**, \*an empty value results in an empty output file.</span></span> <span data-ttu-id="ecf7c-171">支援[globing](https://www.tldp.org/LDP/abs/html/globbingref.html)模式。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-171">[globbing](https://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="ecf7c-172">`minify`:輸出類型的最小化選項。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-172">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="ecf7c-173">\**選擇 ,\*\*\*預設`minify: { enabled: true }`-*</span><span class="sxs-lookup"><span data-stu-id="ecf7c-173">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="ecf7c-174">配置選項每個輸出檔類型都可用。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-174">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="ecf7c-175">CSS 最小值器</span><span class="sxs-lookup"><span data-stu-id="ecf7c-175">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="ecf7c-176">JavaScript 分明器</span><span class="sxs-lookup"><span data-stu-id="ecf7c-176">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="ecf7c-177">HTML 最小值器</span><span class="sxs-lookup"><span data-stu-id="ecf7c-177">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="ecf7c-178">`includeInProject`:指示是否將生成的檔添加到專案檔的標誌。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-178">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="ecf7c-179">**選擇**,*預設 - 假*</span><span class="sxs-lookup"><span data-stu-id="ecf7c-179">**optional**, *default - false*</span></span>
* <span data-ttu-id="ecf7c-180">`sourceMap`:指示是否為捆綁檔生成源映射的標誌。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-180">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="ecf7c-181">**選擇**,*預設 - 假*</span><span class="sxs-lookup"><span data-stu-id="ecf7c-181">**optional**, *default - false*</span></span>
* <span data-ttu-id="ecf7c-182">`sourceMapRootPath`:用於存儲生成的源映射檔的根路徑。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-182">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="ecf7c-183">新增檔案到工作流</span><span class="sxs-lookup"><span data-stu-id="ecf7c-183">Add files to workflow</span></span>

<span data-ttu-id="ecf7c-184">考慮新增類似以下內容的其他*自訂.css*檔案的範例:</span><span class="sxs-lookup"><span data-stu-id="ecf7c-184">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="ecf7c-185">要將*自訂.css*進行小化並將其與*site.css*捆綁到*site.min.css*檔中,將相對路徑新增到*bundleconfig.json*:</span><span class="sxs-lookup"><span data-stu-id="ecf7c-185">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="ecf7c-186">或者,可以使用以下 globing 模式:</span><span class="sxs-lookup"><span data-stu-id="ecf7c-186">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/!(*.min).css" ]
> ```
>
> <span data-ttu-id="ecf7c-187">此 globing 模式匹配所有 CSS 檔,並排除已小化檔模式。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-187">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="ecf7c-188">建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-188">Build the application.</span></span> <span data-ttu-id="ecf7c-189">打開*網站.min.css*並注意到*自訂.css*的內容追加到文件的末尾。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-189">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="ecf7c-190">基於環境的捆綁和小化</span><span class="sxs-lookup"><span data-stu-id="ecf7c-190">Environment-based bundling and minification</span></span>

<span data-ttu-id="ecf7c-191">最佳做法是,應用的捆綁和微化檔應在生產環境中使用。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-191">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="ecf7c-192">在開發過程中,原始檔使應用程式的調試更加容易。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-192">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="ecf7c-193">在檢視中使用[環境標記説明程式](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)指定要包含在頁面中的檔。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-193">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="ecf7c-194">環境標記説明程式僅在特定[環境中](xref:fundamentals/environments)運行時呈現其內容。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-194">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="ecf7c-195">在環境中`environment`執行時,以下標記呈現未處理`Development`的 CSS 檔:</span><span class="sxs-lookup"><span data-stu-id="ecf7c-195">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

::: moniker-end

<span data-ttu-id="ecf7c-196">以下`environment`標記在`Development`非環境中運行時呈現捆綁和已掃描的 CSS 檔。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-196">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="ecf7c-197">例如,在`Production`中`Staging`運行 或觸發這些樣式表的呈現:</span><span class="sxs-lookup"><span data-stu-id="ecf7c-197">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

::: moniker-end

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="ecf7c-198">使用來自 Gulp 的捆綁配置.json</span><span class="sxs-lookup"><span data-stu-id="ecf7c-198">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="ecf7c-199">在某些情況下,應用的捆綁和小化工作流需要額外的處理。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-199">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="ecf7c-200">示例包括圖像優化、緩存破壞和CDN資產處理。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-200">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="ecf7c-201">為了滿足這些要求,您可以將捆綁和最小化工作流轉換為使用 Gulp。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-201">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="manually-convert-the-bundling-and-minification-workflow-to-use-gulp"></a><span data-ttu-id="ecf7c-202">手動轉換捆綁和最小化工作流以使用 Gulp</span><span class="sxs-lookup"><span data-stu-id="ecf7c-202">Manually convert the bundling and minification workflow to use Gulp</span></span>

<span data-ttu-id="ecf7c-203">將包含以下內容`devDependencies`的*套件.json*檔加入到專案根:</span><span class="sxs-lookup"><span data-stu-id="ecf7c-203">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

> [!WARNING]
> <span data-ttu-id="ecf7c-204">該`gulp-uglify`模組不支援 ECMAScript (ES) 2015 / ES6 及更高版本。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-204">The `gulp-uglify` module doesn't support ECMAScript (ES) 2015 / ES6 and later.</span></span> <span data-ttu-id="ecf7c-205">安裝[口槽](https://www.npmjs.com/package/gulp-terser),`gulp-uglify`而不是使用 ES2015 / ES6 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-205">Install [gulp-terser](https://www.npmjs.com/package/gulp-terser) instead of `gulp-uglify` to use ES2015 / ES6 or later.</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="ecf7c-206">透過*在 與套件*相同的等級執行以下指令來安裝依賴項:</span><span class="sxs-lookup"><span data-stu-id="ecf7c-206">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="ecf7c-207">將 Gulp CLI 安裝為全域相依項:</span><span class="sxs-lookup"><span data-stu-id="ecf7c-207">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="ecf7c-208">將下面的*gulpfile.js*複製到專案根目錄:</span><span class="sxs-lookup"><span data-stu-id="ecf7c-208">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="ecf7c-209">執行 Gulp 工作</span><span class="sxs-lookup"><span data-stu-id="ecf7c-209">Run Gulp tasks</span></span>

<span data-ttu-id="ecf7c-210">在 Visual Studio 中產生專案之前觸發 Gulp 小化任務,將以下[MSBuild 目標](/visualstudio/msbuild/msbuild-targets)新增到 @.csproj 檔中:</span><span class="sxs-lookup"><span data-stu-id="ecf7c-210">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the \*.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="ecf7c-211">在此範例中,在`MyPreCompileTarget`目標內定義的任何任務都運行在預定義`Build`目標之前。</span><span class="sxs-lookup"><span data-stu-id="ecf7c-211">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="ecf7c-212">類似於以下內容的輸出將顯示在視覺化工作室的輸出視窗中:</span><span class="sxs-lookup"><span data-stu-id="ecf7c-212">Output similar to the following appears in Visual Studio's Output window:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="ecf7c-213">其他資源</span><span class="sxs-lookup"><span data-stu-id="ecf7c-213">Additional resources</span></span>

* [<span data-ttu-id="ecf7c-214">使用 Grunt</span><span class="sxs-lookup"><span data-stu-id="ecf7c-214">Use Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="ecf7c-215">使用多重環境</span><span class="sxs-lookup"><span data-stu-id="ecf7c-215">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="ecf7c-216">標記說明器</span><span class="sxs-lookup"><span data-stu-id="ecf7c-216">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
