---
title: "統合及縮製中 ASP.NET Core"
author: spboyer
description: 
keywords: "ASP.NET Core 組合和縮製、 CSS、 JavaScript、 縮短，BuildBundlerMinifier"
ms.author: riande
manager: wpickett
ms.date: 02/28/2017
ms.topic: article
ms.assetid: d54230f9-8e5f-4861-a29c-1d3a14e0b0d9
ms.technology: aspnet
ms.prod: aspnet-core
uid: client-side/bundling-and-minification
ms.openlocfilehash: 11528cb2067ced79a09845f9ff78d897da033438
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2017
---
# <a name="bundling-and-minification-in-aspnet-core"></a><span data-ttu-id="fd495-103">統合及縮製中 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fd495-103">Bundling and minification in ASP.NET Core</span></span>

<span data-ttu-id="fd495-104">統合及縮製是兩項技術可用於 ASP.NET 網頁載入效能改善 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="fd495-104">Bundling and minification are two techniques you can use in ASP.NET to improve page load performance for your web application.</span></span> <span data-ttu-id="fd495-105">結合在一起將多個檔案合併成單一檔案。</span><span class="sxs-lookup"><span data-stu-id="fd495-105">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="fd495-106">縮製執行各種不同的程式碼最佳化，以指令碼和 CSS，這會產生較小的裝載。</span><span class="sxs-lookup"><span data-stu-id="fd495-106">Minification performs a variety of different code optimizations to scripts and CSS, which results in smaller payloads.</span></span> <span data-ttu-id="fd495-107">一起使用，統合及縮製載入時間效能透過減少伺服器的要求數目，以及改進降低要求資產 （例如 CSS 和 JavaScript 檔案） 的大小。</span><span class="sxs-lookup"><span data-stu-id="fd495-107">Used together, bundling and minification improves load time performance by reducing the number of requests to the server and reducing the size of the requested assets (such as CSS and JavaScript files).</span></span>

<span data-ttu-id="fd495-108">本文件說明使用統合及縮製，包括如何使用這些功能，與 ASP.NET Core 應用程式的優點。</span><span class="sxs-lookup"><span data-stu-id="fd495-108">This article explains the benefits of using bundling and minification, including how these features can be used with ASP.NET Core applications.</span></span>

## <a name="overview"></a><span data-ttu-id="fd495-109">概觀</span><span class="sxs-lookup"><span data-stu-id="fd495-109">Overview</span></span>

<span data-ttu-id="fd495-110">在 ASP.NET Core 應用程式，有多個統合及縮小用戶端資源選項。</span><span class="sxs-lookup"><span data-stu-id="fd495-110">In ASP.NET Core apps, there are multiple options for bundling and minifying client-side resources.</span></span> <span data-ttu-id="fd495-111">MVC 的核心範本提供的方塊外解決方案使用組態檔和 BuildBundlerMinifier NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="fd495-111">The core templates for MVC provide an out-of-the-box solution using a configuration file and BuildBundlerMinifier NuGet package.</span></span> <span data-ttu-id="fd495-112">協力廠商工具，例如[Gulp](using-gulp.md)和[Grunt](using-grunt.md)也可用來完成相同的工作，應該您的程序需要額外的工作流程或變得複雜。</span><span class="sxs-lookup"><span data-stu-id="fd495-112">Third party tools, such as [Gulp](using-gulp.md) and [Grunt](using-grunt.md) are also available to accomplish the same tasks should your processes require additional workflow or complexities.</span></span> <span data-ttu-id="fd495-113">使用設計階段組合和縮製，應用程式的部署之前，先建立這些縮短的檔案。</span><span class="sxs-lookup"><span data-stu-id="fd495-113">By using design-time bundling and minification, the minified files are created prior to the application’s deployment.</span></span> <span data-ttu-id="fd495-114">統合及縮小部署之前提供的優點減少的伺服器負載。</span><span class="sxs-lookup"><span data-stu-id="fd495-114">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="fd495-115">不過，務必辨識該設計階段組合和縮製增加建置複雜，而且只適用於靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="fd495-115">However, it’s important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

<span data-ttu-id="fd495-116">統合及縮製主要改善第一個頁面要求載入時間。</span><span class="sxs-lookup"><span data-stu-id="fd495-116">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="fd495-117">一旦要求的網頁上，瀏覽器快取資產 （JavaScript、 CSS 和圖像） 因此統合及縮製將不提供任何提升效能，當要求相同的頁面上，或在相同的網頁站台要求相同的資產。</span><span class="sxs-lookup"><span data-stu-id="fd495-117">Once a web page has been requested, the browser caches the assets (JavaScript, CSS and images) so bundling and minification won’t provide any performance boost when requesting the same page, or pages on the same site requesting the same assets.</span></span> <span data-ttu-id="fd495-118">如果您不需要設定到期標頭，在您的資產上正確和不使用統合及縮製、 瀏覽器的有效期限啟發學習法會將標示為資產過時幾天之後和瀏覽器將會需要為每個資產的驗證要求。</span><span class="sxs-lookup"><span data-stu-id="fd495-118">If you don’t set the expires header correctly on your assets, and you don’t use bundling and minification, the browser's freshness heuristics will mark the assets stale after a few days and the browser will require a validation request for each asset.</span></span> <span data-ttu-id="fd495-119">在此情況下，統合及縮製即使在第一個頁面要求後提供的效能提升。</span><span class="sxs-lookup"><span data-stu-id="fd495-119">In this case, bundling and minification provide a performance increase even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="fd495-120">結合在一起</span><span class="sxs-lookup"><span data-stu-id="fd495-120">Bundling</span></span>

<span data-ttu-id="fd495-121">結合在一起是功能可讓您輕鬆地結合或多個檔案配套成單一檔案。</span><span class="sxs-lookup"><span data-stu-id="fd495-121">Bundling is a feature that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="fd495-122">結合在一起時，會將多個檔案結合成單一檔案，因為它可減少擷取及顯示 web 資產，例如網頁所需的伺服器的要求數目。</span><span class="sxs-lookup"><span data-stu-id="fd495-122">Because bundling combines multiple files into a single file, it reduces the number of requests to the server that are required to retrieve and display a web asset, such as a web page.</span></span> <span data-ttu-id="fd495-123">您可以建立 CSS、 JavaScript 和其他組合。</span><span class="sxs-lookup"><span data-stu-id="fd495-123">You can create CSS, JavaScript and other bundles.</span></span> <span data-ttu-id="fd495-124">較少的檔案表示更少的 HTTP 要求從瀏覽器，以在伺服器或提供您的應用程式的服務。</span><span class="sxs-lookup"><span data-stu-id="fd495-124">Fewer files means fewer HTTP requests from your browser to the server or from the service providing your application.</span></span> <span data-ttu-id="fd495-125">這會導致更佳的第一個頁面負載效能。</span><span class="sxs-lookup"><span data-stu-id="fd495-125">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="fd495-126">縮小</span><span class="sxs-lookup"><span data-stu-id="fd495-126">Minification</span></span>

<span data-ttu-id="fd495-127">縮製執行各種不同的程式碼最佳化，以降低要求資產 （例如 CSS、 影像、 JavaScript 檔案） 的大小。</span><span class="sxs-lookup"><span data-stu-id="fd495-127">Minification performs a variety of different code optimizations to reduce the size of requested assets (such as CSS, images, JavaScript files).</span></span> <span data-ttu-id="fd495-128">常見的縮製的結果包含移除不必要的空白字元和註解，並縮短成一個字元的變數名稱。</span><span class="sxs-lookup"><span data-stu-id="fd495-128">Common results of minification include removing unnecessary white space and comments, and shortening variable names to one character.</span></span>

<span data-ttu-id="fd495-129">請考慮下列的 JavaScript 函式：</span><span class="sxs-lookup"><span data-stu-id="fd495-129">Consider the following JavaScript function:</span></span>

```javascript
AddAltToImg = function (imageTagAndImageID, imageContext) {
  ///<signature>
  ///<summary> Adds an alt tab to the image
  // </summary>
  //<param name="imgElement" type="String">The image selector.</param>
  //<param name="ContextForImage" type="String">The image context.</param>
  ///</signature>
  var imageElement = $(imageTagAndImageID, imageContext);
  imageElement.attr('alt', imageElement.attr('id').replace(/ID/, ''));
}
```

<span data-ttu-id="fd495-130">之後縮製，函式會減少所示：</span><span class="sxs-lookup"><span data-stu-id="fd495-130">After minification, the function is reduced to the following:</span></span>

```javascript
AddAltToImg=function(t,a){var r=$(t,a);r.attr("alt",r.attr("id").replace(/ID/,""))};
```

<span data-ttu-id="fd495-131">移除註解和不必要的空白字元，除了下列參數和變數名稱已重新命名 （縮短），如下所示：</span><span class="sxs-lookup"><span data-stu-id="fd495-131">In addition to removing the comments and unnecessary whitespace, the following parameters and variable names were renamed (shortened) as follows:</span></span>

<span data-ttu-id="fd495-132">原始</span><span class="sxs-lookup"><span data-stu-id="fd495-132">Original</span></span> | <span data-ttu-id="fd495-133">已重新命名</span><span class="sxs-lookup"><span data-stu-id="fd495-133">Renamed</span></span>
--- | :---:
<span data-ttu-id="fd495-134">imageTagAndImageID</span><span class="sxs-lookup"><span data-stu-id="fd495-134">imageTagAndImageID</span></span> | <span data-ttu-id="fd495-135">t</span><span class="sxs-lookup"><span data-stu-id="fd495-135">t</span></span>
<span data-ttu-id="fd495-136">imageContext</span><span class="sxs-lookup"><span data-stu-id="fd495-136">imageContext</span></span> | <span data-ttu-id="fd495-137">一個</span><span class="sxs-lookup"><span data-stu-id="fd495-137">a</span></span>
<span data-ttu-id="fd495-138">imageElement</span><span class="sxs-lookup"><span data-stu-id="fd495-138">imageElement</span></span> | <span data-ttu-id="fd495-139">r</span><span class="sxs-lookup"><span data-stu-id="fd495-139">r</span></span>

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="fd495-140">統合及縮製的影響</span><span class="sxs-lookup"><span data-stu-id="fd495-140">Impact of bundling and minification</span></span>

<span data-ttu-id="fd495-141">下表顯示個別列出所有資產，並使用簡單的 web 網頁上的統合及縮製之間的數個重要差異：</span><span class="sxs-lookup"><span data-stu-id="fd495-141">The following table shows several important differences between listing all the assets individually and using bundling and minification on a simple web page:</span></span>

<span data-ttu-id="fd495-142">動作</span><span class="sxs-lookup"><span data-stu-id="fd495-142">Action</span></span> | <span data-ttu-id="fd495-143">使用 B/M</span><span class="sxs-lookup"><span data-stu-id="fd495-143">With B/M</span></span> | <span data-ttu-id="fd495-144">沒有 B/M</span><span class="sxs-lookup"><span data-stu-id="fd495-144">Without B/M</span></span> | <span data-ttu-id="fd495-145">變更</span><span class="sxs-lookup"><span data-stu-id="fd495-145">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="fd495-146">檔案要求</span><span class="sxs-lookup"><span data-stu-id="fd495-146">File Requests</span></span> |<span data-ttu-id="fd495-147">7</span><span class="sxs-lookup"><span data-stu-id="fd495-147">7</span></span> | <span data-ttu-id="fd495-148">18</span><span class="sxs-lookup"><span data-stu-id="fd495-148">18</span></span> | <span data-ttu-id="fd495-149">157%</span><span class="sxs-lookup"><span data-stu-id="fd495-149">157%</span></span>
<span data-ttu-id="fd495-150">傳送的 KB</span><span class="sxs-lookup"><span data-stu-id="fd495-150">KB Transferred</span></span> | <span data-ttu-id="fd495-151">156</span><span class="sxs-lookup"><span data-stu-id="fd495-151">156</span></span> | <span data-ttu-id="fd495-152">264.68</span><span class="sxs-lookup"><span data-stu-id="fd495-152">264.68</span></span> | <span data-ttu-id="fd495-153">70%</span><span class="sxs-lookup"><span data-stu-id="fd495-153">70%</span></span>
<span data-ttu-id="fd495-154">載入時間 （毫秒）</span><span class="sxs-lookup"><span data-stu-id="fd495-154">Load Time (MS)</span></span> | <span data-ttu-id="fd495-155">885</span><span class="sxs-lookup"><span data-stu-id="fd495-155">885</span></span> | <span data-ttu-id="fd495-156">2360</span><span class="sxs-lookup"><span data-stu-id="fd495-156">2360</span></span> | <span data-ttu-id="fd495-157">167%</span><span class="sxs-lookup"><span data-stu-id="fd495-157">167%</span></span>

<span data-ttu-id="fd495-158">傳送的位元組有大幅降低與結合在一起的瀏覽器相當詳細資訊，以套用要求的 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="fd495-158">The bytes sent had a significant reduction with bundling as browsers are fairly verbose with the HTTP headers that they apply on requests.</span></span> <span data-ttu-id="fd495-159">載入時間顯示大改進，但此範例已在本機執行。</span><span class="sxs-lookup"><span data-stu-id="fd495-159">The load time shows a big improvement, however this example was run locally.</span></span> <span data-ttu-id="fd495-160">統合及縮製使用資產透過網路傳輸時，您會得到更提升效能。</span><span class="sxs-lookup"><span data-stu-id="fd495-160">You will get greater gains in performance when using bundling and minification with assets transferred over a network.</span></span>

## <a name="using-bundling-and-minification-in-a-project"></a><span data-ttu-id="fd495-161">在專案中使用統合及縮製</span><span class="sxs-lookup"><span data-stu-id="fd495-161">Using bundling and minification in a project</span></span>

<span data-ttu-id="fd495-162">MVC 專案範本提供`bundleconfig.json`組態檔會定義每個組合的選項。</span><span class="sxs-lookup"><span data-stu-id="fd495-162">The MVC project template provides a `bundleconfig.json` configuration file which defines the options for each bundle.</span></span> <span data-ttu-id="fd495-163">依預設，單一組合組態定義的自訂 javascript (`wwwroot/js/site.js`) 和樣式表 (`wwwroot/css/site.css`) 檔案。</span><span class="sxs-lookup"><span data-stu-id="fd495-163">By default, a single bundle configuration is defined for the custom JavaScript (`wwwroot/js/site.js`) and Stylesheet (`wwwroot/css/site.css`) files.</span></span>

[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig.json)]

<span data-ttu-id="fd495-164">組合的選項包括：</span><span class="sxs-lookup"><span data-stu-id="fd495-164">Bundle options include:</span></span>

* <span data-ttu-id="fd495-165">outputFileName-要輸出的組合檔案的名稱。</span><span class="sxs-lookup"><span data-stu-id="fd495-165">outputFileName - name of the bundle file to output.</span></span> <span data-ttu-id="fd495-166">可包含相對路徑`bundleconfig.json`檔案。</span><span class="sxs-lookup"><span data-stu-id="fd495-166">Can contain a relative path from the `bundleconfig.json` file.</span></span> <span data-ttu-id="fd495-167">**所需**</span><span class="sxs-lookup"><span data-stu-id="fd495-167">**required**</span></span>
* <span data-ttu-id="fd495-168">inputFiles-要配套起來的檔案陣列。</span><span class="sxs-lookup"><span data-stu-id="fd495-168">inputFiles - array of files to bundle together.</span></span> <span data-ttu-id="fd495-169">這些是在組態檔的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="fd495-169">These are relative paths to the configuration file.</span></span> <span data-ttu-id="fd495-170">**選擇性**，* 空值會導致空的輸出檔案。</span><span class="sxs-lookup"><span data-stu-id="fd495-170">**optional**, *an empty value results in an empty output file.</span></span> <span data-ttu-id="fd495-171">[通用慣例](http://www.tldp.org/LDP/abs/html/globbingref.html)支援的模式。</span><span class="sxs-lookup"><span data-stu-id="fd495-171">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="fd495-172">縮短-縮小選項的輸出類型。</span><span class="sxs-lookup"><span data-stu-id="fd495-172">minify - minification options for the output type.</span></span> <span data-ttu-id="fd495-173">**選擇性**，*預設值-`minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="fd495-173">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="fd495-174">每個輸出檔案類型有組態選項。</span><span class="sxs-lookup"><span data-stu-id="fd495-174">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="fd495-175">CSS 縮短程式</span><span class="sxs-lookup"><span data-stu-id="fd495-175">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="fd495-176">JavaScript 縮短程式</span><span class="sxs-lookup"><span data-stu-id="fd495-176">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
    * [<span data-ttu-id="fd495-177">HTML 縮短程式</span><span class="sxs-lookup"><span data-stu-id="fd495-177">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="fd495-178">includeInProject-將產生的檔案加入至專案檔。</span><span class="sxs-lookup"><span data-stu-id="fd495-178">includeInProject - add generated files to project file.</span></span> <span data-ttu-id="fd495-179">**選擇性**，*預設為 false*</span><span class="sxs-lookup"><span data-stu-id="fd495-179">**optional**, *default - false*</span></span>
* <span data-ttu-id="fd495-180">Sourcemap-產生將配套的檔案的來源對應。</span><span class="sxs-lookup"><span data-stu-id="fd495-180">sourceMaps - generate source maps for the bundled file.</span></span> <span data-ttu-id="fd495-181">**選擇性**，*預設為 false*</span><span class="sxs-lookup"><span data-stu-id="fd495-181">**optional**, *default - false*</span></span>

### <a name="visual-studio-2015--2017"></a><span data-ttu-id="fd495-182">Visual Studio 2015 / 2017年</span><span class="sxs-lookup"><span data-stu-id="fd495-182">Visual Studio 2015 / 2017</span></span>

<span data-ttu-id="fd495-183">開啟`bundleconfig.json`在 Visual Studio 中，如果您的環境沒有安裝; 此擴充提示會建議是否都有一個可以協助進行這種檔案類型。</span><span class="sxs-lookup"><span data-stu-id="fd495-183">Open `bundleconfig.json` in Visual Studio, if your environment does not have the extension installed; a prompt is presented suggesting that there is one that could assist with this file type.</span></span>

![BuildBundlerMinifier 擴充功能的建議](../client-side/bundling-and-minification/_static/bundler-extension-suggestion.png)

<span data-ttu-id="fd495-185">選取檢視擴充功能，並安裝**搭配程式 （& s) 縮短程式**擴充功能 （需要 Visual Studio 重新啟動）。</span><span class="sxs-lookup"><span data-stu-id="fd495-185">Select View Extensions, and install the **Bundler & Minifier** extension (Requires Visual Studio restart).</span></span>

![BuildBundlerMinifier 擴充功能的建議](../client-side/bundling-and-minification/_static/view-extension.png)

<span data-ttu-id="fd495-187">重新啟動完成時，您需要設定要執行的處理程序縮短和結合在一起的用戶端資產的組建。</span><span class="sxs-lookup"><span data-stu-id="fd495-187">When the restart is complete, you need to configure the build to run the processes of minifying and bundling the client-side assets.</span></span> <span data-ttu-id="fd495-188">以滑鼠右鍵按一下`bundleconfig.json`檔案，然後選取*啟用組合建置...*.</span><span class="sxs-lookup"><span data-stu-id="fd495-188">Right-click the `bundleconfig.json` file and select *Enable bundle on build...*.</span></span>

<span data-ttu-id="fd495-189">建置專案，而`bundleconfig.json`包含在建置程序，以產生根據組態的輸出檔。</span><span class="sxs-lookup"><span data-stu-id="fd495-189">Build the project, and the `bundleconfig.json` is included in the build process to produce the output files based on the configuration.</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierExample, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierExample -> C:\BuildBundlerMinifierExample\bin\Debug\netcoreapp1.1\BuildBundlerMinifierExample.dll
========== Build: 1 succeeded or up-to-date, 0 failed, 0 skipped ==========
```

### <a name="visual-studio-code-or-command-line"></a><span data-ttu-id="fd495-190">Visual Studio 程式碼或命令列</span><span class="sxs-lookup"><span data-stu-id="fd495-190">Visual Studio Code or Command Line</span></span>

<span data-ttu-id="fd495-191">Visual Studio 的擴充功能結合在一起的磁碟機和縮製程序使用 GUI 手勢。不過，相同的功能可與`dotnet`CLI 和 BuildBundlerMinifier NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="fd495-191">Visual Studio and the extension drive the bundling and minification process using GUI gestures; however, the same capabilities are available with the `dotnet` CLI and BuildBundlerMinifier NuGet package.</span></span>

<span data-ttu-id="fd495-192">將 NuGet 封裝加入至您的專案：</span><span class="sxs-lookup"><span data-stu-id="fd495-192">Add the NuGet package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="fd495-193">還原的相依性：</span><span class="sxs-lookup"><span data-stu-id="fd495-193">Restore the dependencies:</span></span>

```console
dotnet restore
```

<span data-ttu-id="fd495-194">建置應用程式：</span><span class="sxs-lookup"><span data-stu-id="fd495-194">Build the app:</span></span>

```console
dotnet build
```

<span data-ttu-id="fd495-195">建置命令的輸出顯示縮製及/或根據設定結合在一起的結果。</span><span class="sxs-lookup"><span data-stu-id="fd495-195">The output from the build command shows the results of the minification and/or bundling according to what is configured.</span></span>

```console
Microsoft (R) Build Engine version 15.1.545.13942
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Begin processing bundleconfig.json
     Minified wwwroot/css/site.min.css
  Bundler: Done processing bundleconfig.json
  BuildBundlerMinifierExample -> /BuildBundlerMinifierExample/bin/Debug/netcoreapp1.0/BuildBundlerMinifierExample.dll
```

## <a name="adding-files"></a><span data-ttu-id="fd495-196">新增檔案</span><span class="sxs-lookup"><span data-stu-id="fd495-196">Adding files</span></span>

<span data-ttu-id="fd495-197">在此範例中，其他的 CSS 檔案會加入稱為`custom.css`而且設定為統合及縮製與`site.css`，並產生單一`site.min.css`。</span><span class="sxs-lookup"><span data-stu-id="fd495-197">In this example, an additional CSS file is added called `custom.css` and configured for bundling and minification with `site.css`, resulting in a single `site.min.css`.</span></span>

<span data-ttu-id="fd495-198">custom.css</span><span class="sxs-lookup"><span data-stu-id="fd495-198">custom.css</span></span>

```css
.about, [role=main], [role=complementary]
{
    margin-top: 60px;
}

footer
{
    margin-top: 10px;
}
```

<span data-ttu-id="fd495-199">新增的相對路徑`bundleconfig.json`。</span><span class="sxs-lookup"><span data-stu-id="fd495-199">Add the relative path to `bundleconfig.json`.</span></span>

[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig2.json)]

> [!NOTE]
> <span data-ttu-id="fd495-200">或者，您無法使用通用慣例模式-`"inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]`此 cmdlet 會取得所有 CSS 檔，並排除縮短的檔案模式。</span><span class="sxs-lookup"><span data-stu-id="fd495-200">Alternatively, the globbing pattern could be used - `"inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]` which gets all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="fd495-201">建置應用程式，如果您開啟`site.min.css`，您會立即注意的內容`custom.css`已附加至檔案結尾。</span><span class="sxs-lookup"><span data-stu-id="fd495-201">Build the application and if you open `site.min.css`, you'll now notice that contents of `custom.css` has been appended to the end of the file.</span></span>

## <a name="controlling-bundling-and-minification"></a><span data-ttu-id="fd495-202">控制統合及縮製</span><span class="sxs-lookup"><span data-stu-id="fd495-202">Controlling bundling and minification</span></span>

<span data-ttu-id="fd495-203">一般情況下，您要使用您的應用程式只會在實際執行環境的配套和縮短檔。</span><span class="sxs-lookup"><span data-stu-id="fd495-203">In general, you want to use the bundled and minified files of your app only in a production environment.</span></span> <span data-ttu-id="fd495-204">在開發期間，您要使用原始的檔案，因此您的應用程式偵錯更容易。</span><span class="sxs-lookup"><span data-stu-id="fd495-204">During development, you want to use your original files so your app is easier to debug.</span></span>

<span data-ttu-id="fd495-205">您可以指定哪些指令碼和 CSS 檔案，以包含在您使用環境標記協助程式在版面配置頁面的頁面 (請參閱[標記協助程式](../mvc/views/tag-helpers/index.md))。</span><span class="sxs-lookup"><span data-stu-id="fd495-205">You can specify which scripts and CSS files to include in your pages using the environment tag helper in your layout pages (see [Tag Helpers](../mvc/views/tag-helpers/index.md)).</span></span> <span data-ttu-id="fd495-206">在特定的環境中執行時，環境標記協助程式只會呈現其內容。</span><span class="sxs-lookup"><span data-stu-id="fd495-206">The environment tag helper will only render its contents when running in specific environments.</span></span> <span data-ttu-id="fd495-207">請參閱[使用多個環境](../fundamentals/environments.md)如指定目前環境的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="fd495-207">See [Working with Multiple Environments](../fundamentals/environments.md) for details on specifying the current environment.</span></span>

<span data-ttu-id="fd495-208">下列環境標記將會呈現未處理的 CSS 檔案中執行時`Development`環境：</span><span class="sxs-lookup"><span data-stu-id="fd495-208">The following environment tag will render the unprocessed CSS files when running in the `Development` environment:</span></span>

[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=3&range=9-12)]

<span data-ttu-id="fd495-209">只有在執行時，此環境標記會轉譯配套並縮短的 CSS 檔案`Production`或`Staging`:</span><span class="sxs-lookup"><span data-stu-id="fd495-209">This environment tag will render the bundled and minified CSS files only when running in `Production` or `Staging`:</span></span>

[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=5&range=13-18)]

## <a name="consuming-bundleconfigjson-from-gulp"></a><span data-ttu-id="fd495-210">使用從 Gulp bundleconfig.json</span><span class="sxs-lookup"><span data-stu-id="fd495-210">Consuming bundleconfig.json from Gulp</span></span>

<span data-ttu-id="fd495-211">如果您的應用程式統合及縮製工作流程需要額外的處理序，例如映像處理、 快取 busting、 CDN assest 處理 」 等，您可以將組合和 Minify 程序轉換成 Gulp。</span><span class="sxs-lookup"><span data-stu-id="fd495-211">If your app bundling and minification workflow requires additional processes such as image processing, cache busting, CDN assest processing, etc., then you can convert the Bundle and Minify process to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="fd495-212">僅適用於 Visual Studio 2015 和 2017年 [轉換] 選項。</span><span class="sxs-lookup"><span data-stu-id="fd495-212">Conversion option only available in Visual Studio 2015 and 2017.</span></span>

<span data-ttu-id="fd495-213">以滑鼠右鍵按一下`bundleconfig.json`選取**Gulp 轉換成...**.這會產生`gulpfile.js`並安裝必要的 npm 封裝。</span><span class="sxs-lookup"><span data-stu-id="fd495-213">Right-click the `bundleconfig.json` and select **Convert to Gulp...**. This will generate the `gulpfile.js` and install the necessary npm packages.</span></span>

![將轉換成 Gulp](../client-side/bundling-and-minification/_static/convert-togulp.png)

<span data-ttu-id="fd495-215">`gulpfile.js`產生讀取`bundleconfig.json`檔案設定，因此它可以繼續使用輸入/輸出和設定。</span><span class="sxs-lookup"><span data-stu-id="fd495-215">The `gulpfile.js` produced reads the `bundleconfig.json` file for the configuration, therefore it can continue to be used for the inputs/outputs and settings.</span></span>

[!code-javascript[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/gulpfile.js)]

<span data-ttu-id="fd495-216">若要在 Visual Studio 2017 建置專案時，請啟用 Gulp，請先 *.csproj 檔案中加入下列：</span><span class="sxs-lookup"><span data-stu-id="fd495-216">To enable Gulp when the project builds in Visual Studio 2017, add the following to the *.csproj file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
    <Exec Command="gulp min" />
</Target>
```

<span data-ttu-id="fd495-217">若要啟用 Gulp，Visual Studio 2015 中建置專案時，將下列內容加入`project.json`檔案：</span><span class="sxs-lookup"><span data-stu-id="fd495-217">To enable Gulp when the project builds in Visual Studio 2015, add the following to the `project.json` file:</span></span>

```json
"scripts": {
    "precompile": "gulp min"
}
```

## <a name="additional-resources"></a><span data-ttu-id="fd495-218">其他資源</span><span class="sxs-lookup"><span data-stu-id="fd495-218">Additional resources</span></span>

* [<span data-ttu-id="fd495-219">使用 Gulp</span><span class="sxs-lookup"><span data-stu-id="fd495-219">Using Gulp</span></span>](using-gulp.md)
* [<span data-ttu-id="fd495-220">使用 Grunt</span><span class="sxs-lookup"><span data-stu-id="fd495-220">Using Grunt</span></span>](using-grunt.md)
* [<span data-ttu-id="fd495-221">使用多個環境</span><span class="sxs-lookup"><span data-stu-id="fd495-221">Working with Multiple Environments</span></span>](../fundamentals/environments.md)
* [<span data-ttu-id="fd495-222">標記協助程式</span><span class="sxs-lookup"><span data-stu-id="fd495-222">Tag Helpers</span></span>](../mvc/views/tag-helpers/index.md)
