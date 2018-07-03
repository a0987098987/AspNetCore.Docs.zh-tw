---
uid: single-page-application/overview/templates/hottowel-template
title: 熱 Towel 範本 |Microsoft Docs
author: madskristensen
description: HotTowel 範本
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: de81f12f57d7f2fb7c6478bfa1f3a278ae905a39
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388534"
---
<a name="hot-towel-template"></a><span data-ttu-id="e5e4e-103">Hot Towel 範本</span><span class="sxs-lookup"><span data-stu-id="e5e4e-103">Hot Towel template</span></span>
====================
<span data-ttu-id="e5e4e-104">藉由[Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="e5e4e-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="e5e4e-105">最忙碌的毛巾 MVC 範本作者 John Papa</span><span class="sxs-lookup"><span data-stu-id="e5e4e-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="e5e4e-106">選擇要下載的版本：</span><span class="sxs-lookup"><span data-stu-id="e5e4e-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="e5e4e-107">適用於 Visual Studio 2012 的最忙碌的毛巾 MVC 範本</span><span class="sxs-lookup"><span data-stu-id="e5e4e-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="e5e4e-108">Visual Studio 2013 的最忙碌的毛巾 MVC 範本</span><span class="sxs-lookup"><span data-stu-id="e5e4e-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> <span data-ttu-id="e5e4e-109">熱毛巾： 因為您不想要移至其中一個情況下 SPA ！</span><span class="sxs-lookup"><span data-stu-id="e5e4e-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>


<span data-ttu-id="e5e4e-110">要建置 SPA，但無法決定要從何處開始？</span><span class="sxs-lookup"><span data-stu-id="e5e4e-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="e5e4e-111">使用熱門的毛巾，並以秒為單位，您將有 SPA 並在其上建置所需的所有工具 ！</span><span class="sxs-lookup"><span data-stu-id="e5e4e-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="e5e4e-112">最忙碌的毛巾建立好的切入點，來建置使用 ASP.NET 單一頁面應用程式 (SPA)。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="e5e4e-113">根據預設，您它提供模組化結構程式碼、 檢視瀏覽、 資料繫結、 豐富的資料管理和簡單但簡潔樣式。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="e5e4e-114">最忙碌的毛巾提供您要建置 SPA，讓您可以專注於您的應用程式未連結的所有項目。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="e5e4e-115">了解如何建置從 SPA [John Papa 的影片、 教學課程和 Pluralsight 課程](http://johnpapa.net/spa?vsix)。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>


## <a name="application-structure"></a><span data-ttu-id="e5e4e-116">應用程式結構</span><span class="sxs-lookup"><span data-stu-id="e5e4e-116">Application Structure</span></span>

<span data-ttu-id="e5e4e-117">最忙碌的毛巾 SPA 提供應用程式資料夾，其中包含定義您的應用程式的 JavaScript 和 HTML 檔案。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="e5e4e-118">在 [應用程式] 資料夾中：</span><span class="sxs-lookup"><span data-stu-id="e5e4e-118">Inside the App folder:</span></span>

- <span data-ttu-id="e5e4e-119">durandal</span><span class="sxs-lookup"><span data-stu-id="e5e4e-119">durandal</span></span>
- <span data-ttu-id="e5e4e-120">服務</span><span class="sxs-lookup"><span data-stu-id="e5e4e-120">services</span></span>
- <span data-ttu-id="e5e4e-121">viewmodel</span><span class="sxs-lookup"><span data-stu-id="e5e4e-121">viewmodels</span></span>
- <span data-ttu-id="e5e4e-122">檢視</span><span class="sxs-lookup"><span data-stu-id="e5e4e-122">views</span></span>

<span data-ttu-id="e5e4e-123">應用程式資料夾中包含模組的集合。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="e5e4e-124">這些模組會封裝功能，並宣告對其他模組的相依性。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="e5e4e-125">[Views] 資料夾包含您的應用程式的 HTML 和 viewmodel 資料夾包含檢視 （一般 MVVM 模式） 的展示邏輯。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="e5e4e-126">[服務] 資料夾是適合用來裝載您的應用程式可能需要 HTTP 資料擷取或本機儲存體互動等任何通用服務。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="e5e4e-127">很常見的多個 viewmodel 重複使用從服務模組的程式碼。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="e5e4e-128">ASP.NET MVC 伺服器端應用程式結構</span><span class="sxs-lookup"><span data-stu-id="e5e4e-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="e5e4e-129">最忙碌的毛巾熟悉且功能強大的 ASP.NET MVC 結構為基礎。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="e5e4e-130">應用程式\_開始</span><span class="sxs-lookup"><span data-stu-id="e5e4e-130">App\_Start</span></span>
- <span data-ttu-id="e5e4e-131">內容</span><span class="sxs-lookup"><span data-stu-id="e5e4e-131">Content</span></span>
- <span data-ttu-id="e5e4e-132">Controllers</span><span class="sxs-lookup"><span data-stu-id="e5e4e-132">Controllers</span></span>
- <span data-ttu-id="e5e4e-133">模型</span><span class="sxs-lookup"><span data-stu-id="e5e4e-133">Models</span></span>
- <span data-ttu-id="e5e4e-134">指令碼</span><span class="sxs-lookup"><span data-stu-id="e5e4e-134">Scripts</span></span>
- <span data-ttu-id="e5e4e-135">檢視</span><span class="sxs-lookup"><span data-stu-id="e5e4e-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="e5e4e-136">精選文件庫</span><span class="sxs-lookup"><span data-stu-id="e5e4e-136">Featured Libraries</span></span>

- <span data-ttu-id="e5e4e-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="e5e4e-137">ASP.NET MVC</span></span>
- <span data-ttu-id="e5e4e-138">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="e5e4e-138">ASP.NET Web API</span></span>
- <span data-ttu-id="e5e4e-139">ASP.NET Web 最佳化-統合和縮製</span><span class="sxs-lookup"><span data-stu-id="e5e4e-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="e5e4e-140">[Breeze.js](http://Breezejs.com) -豐富的資料管理</span><span class="sxs-lookup"><span data-stu-id="e5e4e-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="e5e4e-141">[Durandal.js](http://Durandaljs.com) -瀏覽和檢視構成要素</span><span class="sxs-lookup"><span data-stu-id="e5e4e-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="e5e4e-142">[Knockout.js](http://Knockoutjs.com) -資料繫結</span><span class="sxs-lookup"><span data-stu-id="e5e4e-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="e5e4e-143">[Require.js](http://requirejs.org) -使用 AMD 和最佳化的模組化</span><span class="sxs-lookup"><span data-stu-id="e5e4e-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="e5e4e-144">[Toastr.js](http://jpapa.me/c7toastr) -快顯訊息</span><span class="sxs-lookup"><span data-stu-id="e5e4e-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="e5e4e-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) -強大的 CSS 樣式</span><span class="sxs-lookup"><span data-stu-id="e5e4e-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="e5e4e-146">透過 Visual Studio 2012 的最忙碌的毛巾 SPA 範本安裝</span><span class="sxs-lookup"><span data-stu-id="e5e4e-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="e5e4e-147">最忙碌的毛巾可以安裝為 Visual Studio 2012 的範本。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="e5e4e-148">只要按一下`File`  |  `New Project` ，然後選擇  `ASP.NET MVC 4 Web Application`。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="e5e4e-149">然後選取 ' 熱毛巾單一頁面應用程式 」 範本，並執行 ！</span><span class="sxs-lookup"><span data-stu-id="e5e4e-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="e5e4e-150">透過 NuGet 套件安裝</span><span class="sxs-lookup"><span data-stu-id="e5e4e-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="e5e4e-151">最忙碌的毛巾也是加強現有的空白 ASP.NET MVC 專案的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="e5e4e-152">只使用 Nuget 來安裝，然後再執行 ！</span><span class="sxs-lookup"><span data-stu-id="e5e4e-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="e5e4e-153">如何建置在熱門的毛巾？</span><span class="sxs-lookup"><span data-stu-id="e5e4e-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="e5e4e-154">只要開始加入程式碼 ！</span><span class="sxs-lookup"><span data-stu-id="e5e4e-154">Simply start adding code!</span></span>

1. <span data-ttu-id="e5e4e-155">新增您自己伺服器端程式碼，最好是 Entity Framework 和 WebAPI （這會真正顯現 Breeze.js 與）</span><span class="sxs-lookup"><span data-stu-id="e5e4e-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="e5e4e-156">加入檢視，以`App/views`資料夾</span><span class="sxs-lookup"><span data-stu-id="e5e4e-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="e5e4e-157">新增到 viewmodel`App/viewmodels`資料夾</span><span class="sxs-lookup"><span data-stu-id="e5e4e-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="e5e4e-158">將 HTML 和 Knockout 資料繫結新增至您新的檢視</span><span class="sxs-lookup"><span data-stu-id="e5e4e-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="e5e4e-159">更新中的瀏覽路由 `shell.js`</span><span class="sxs-lookup"><span data-stu-id="e5e4e-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="e5e4e-160">逐步解說中的 HTML/JavaScript</span><span class="sxs-lookup"><span data-stu-id="e5e4e-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="e5e4e-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="e5e4e-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="e5e4e-162">index.cshtml 是開始的路由和檢視的 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="e5e4e-163">它包含所有標準的中繼標記、 css 連結及您所預期的 JavaScript 參考。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="e5e4e-164">本文包含單一`<div>`這是所有內容 （檢視） 將放置的位置要求時。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="e5e4e-165">`@Scripts.Render`用以執行應用程式的程式碼中，包含此功能的進入點 Require.js`main.js`檔案。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="e5e4e-166">示範如何建立啟動顯示畫面，您的應用程式載入時提供啟動顯示畫面。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="e5e4e-167">App/main.js</span><span class="sxs-lookup"><span data-stu-id="e5e4e-167">App/main.js</span></span>

<span data-ttu-id="e5e4e-168">`main.js`檔案包含已載入您的應用程式時，立即將執行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="e5e4e-169">這是您想要用來定義您的瀏覽路由、 設定檢視，您啟動及執行任何安裝/啟動程序等預備您的應用程式資料。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="e5e4e-170">`main.js`檔案會定義數個 durandal 的模組，以幫助啟動應用程式執行。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="e5e4e-171">定義陳述式可協助解決模組相依性，讓它們可供函式。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="e5e4e-172">第一次會啟用偵錯訊息，其中傳送應用程式的主控台視窗中執行何種事件的相關訊息。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="e5e4e-173">App.start 程式碼會告訴 durandal 架構，來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="e5e4e-174">使 durandal 知道所有檢視和 viewmodels 會分別包含在相同的已命名資料夾中，設定慣例。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="e5e4e-175">最後，`app.setRoot`一開始會載入`shell`使用預先定義`entrance`動畫。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="e5e4e-176">檢視</span><span class="sxs-lookup"><span data-stu-id="e5e4e-176">Views</span></span>

<span data-ttu-id="e5e4e-177">檢視位於`App/views`資料夾。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="e5e4e-178">shell.html</span><span class="sxs-lookup"><span data-stu-id="e5e4e-178">shell.html</span></span>

<span data-ttu-id="e5e4e-179">`shell.html`包含 HTML 的主要配置。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="e5e4e-180">所有其他程式檢視端將某處包含您`shell`檢視。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="e5e4e-181">最忙碌的毛巾提供`shell`與三個這類區域： 標頭、 內容區域和頁尾。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="e5e4e-182">每一個地區已載入內容形成要求時的其他檢視。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="e5e4e-183">`compose`頁首和頁尾的繫結是硬式編碼中指向的熱毛巾`nav`和`footer`分別檢視。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="e5e4e-184">Compose 繫結區段`#content`繫結至`router`模組的作用中的項目。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="e5e4e-185">換句話說，當您按一下 瀏覽連結，它是對應的檢視會載入此區域中。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="e5e4e-186">nav.html</span><span class="sxs-lookup"><span data-stu-id="e5e4e-186">nav.html</span></span>

<span data-ttu-id="e5e4e-187">`nav.html`包含為了讓 SPA 的導覽連結。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="e5e4e-188">這是功能表結構可放置的位置，例如。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="e5e4e-189">通常這是資料繫結 （使用 Knockout） 為了`router`模組，以顯示在中所定義的導覽`shell.js`。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="e5e4e-190">油墨廓清會尋找資料繫結屬性，並將它們加入繫結`shell`viewmodel 顯示瀏覽路線，並顯示進度列 （使用 Twitter Bootstrap） 如果`router`模組正在進行瀏覽不同的檢視，另一個 （請參閱`router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="e5e4e-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="e5e4e-191">home.html 和 details.html</span><span class="sxs-lookup"><span data-stu-id="e5e4e-191">home.html and details.html</span></span>

<span data-ttu-id="e5e4e-192">這些檢視包含自訂檢視的 HTML。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="e5e4e-193">當`home`連結`nav`按一下檢視的功能表時，`home`檢視將會放置在內容區域的`shell`檢視。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="e5e4e-194">這些檢視可以增強或取代您自己的自訂檢視。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="e5e4e-195">footer.html</span><span class="sxs-lookup"><span data-stu-id="e5e4e-195">footer.html</span></span>

<span data-ttu-id="e5e4e-196">`footer.html`包含出現在頁尾中，在底部的 HTML`shell`檢視。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="e5e4e-197">ViewModels</span><span class="sxs-lookup"><span data-stu-id="e5e4e-197">ViewModels</span></span>

<span data-ttu-id="e5e4e-198">Viewmodel 位於`App/viewmodels`資料夾。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="e5e4e-199">shell.js</span><span class="sxs-lookup"><span data-stu-id="e5e4e-199">shell.js</span></span>

<span data-ttu-id="e5e4e-200">`shell` Viewmodel 包含屬性和繫結至的函式`shell`檢視。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="e5e4e-201">通常這是位於功能表瀏覽繫結 (請參閱`router.mapNav`邏輯)。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="e5e4e-202">home.js 並 details.js</span><span class="sxs-lookup"><span data-stu-id="e5e4e-202">home.js and details.js</span></span>

<span data-ttu-id="e5e4e-203">這些 viewmodel 包含的屬性和繫結至的函式`home`檢視。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="e5e4e-204">它也包含展示邏輯檢視，並且資料和檢視之間的連結。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="e5e4e-205">服務</span><span class="sxs-lookup"><span data-stu-id="e5e4e-205">Services</span></span>

<span data-ttu-id="e5e4e-206">服務應用程式/服務資料夾中找到。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="e5e4e-207">在理想情況下您未來的服務，例如 dataservice 模組，也就是負責取得和張貼遠端資料，無法放置。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="e5e4e-208">logger.js</span><span class="sxs-lookup"><span data-stu-id="e5e4e-208">logger.js</span></span>

<span data-ttu-id="e5e4e-209">最忙碌的毛巾提供`logger`模組服務 資料夾中的。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="e5e4e-210">`logger`模組適合用來記錄訊息至主控台，以及在快顯通知快顯中的使用者。</span><span class="sxs-lookup"><span data-stu-id="e5e4e-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>
