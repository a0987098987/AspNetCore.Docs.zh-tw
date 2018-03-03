---
title: "建置美麗、 可回應網站與啟動程序"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bootstrap
ms.openlocfilehash: e2ade6223cdc56a4f0f00ff0b985ceda4ab5484a
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2018
---
# <a name="building-beautiful-responsive-sites-with-bootstrap"></a><span data-ttu-id="a69df-102">建置美麗、 可回應網站與啟動程序</span><span class="sxs-lookup"><span data-stu-id="a69df-102">Building beautiful, responsive sites with Bootstrap</span></span>

<a name="bootstrap-index"></a>

<span data-ttu-id="a69df-103">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="a69df-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a69df-104">最熱門的 web 架構開發能繼續回應的 web 應用程式的目前啟動載入器。</span><span class="sxs-lookup"><span data-stu-id="a69df-104">Bootstrap is currently the most popular web framework for developing responsive web applications.</span></span> <span data-ttu-id="a69df-105">無論您是在前端的設計和開發或專家的新手，它會提供數種功能可以改善您的網站，您的使用者經驗的優點。</span><span class="sxs-lookup"><span data-stu-id="a69df-105">It offers a number of features and benefits that can improve your users' experience with your web site, whether you're a novice at front-end design and development or an expert.</span></span> <span data-ttu-id="a69df-106">啟動安裝程式會部署為一組 CSS 和 JavaScript 檔案，設計用來協助您網站或應用程式的標尺有效率地從手機到桌上型電腦的平板電腦。</span><span class="sxs-lookup"><span data-stu-id="a69df-106">Bootstrap is deployed as a set of CSS and JavaScript files, and is designed to help your website or application scale efficiently from phones to tablets to desktops.</span></span>

## <a name="getting-started"></a><span data-ttu-id="a69df-107">使用者入門</span><span class="sxs-lookup"><span data-stu-id="a69df-107">Getting started</span></span>

<span data-ttu-id="a69df-108">有幾種方式來開始使用啟動程序。</span><span class="sxs-lookup"><span data-stu-id="a69df-108">There are several ways to get started with Bootstrap.</span></span> <span data-ttu-id="a69df-109">如果您在 Visual Studio 中開始新的 web 應用程式，您可以選擇預設的入門範本適用於 ASP.NET Core，案例的啟動程序將會預先安裝：</span><span class="sxs-lookup"><span data-stu-id="a69df-109">If you're starting a new web application in Visual Studio, you can choose the default starter template for ASP.NET Core, in which case Bootstrap will come pre-installed:</span></span>

![啟動載入入門範本方案檢視中](bootstrap/_static/bootstrap-in-starter-template.png)

<span data-ttu-id="a69df-111">啟動程序加入 ASP.NET Core 專案是簡單的將它加入至*bower.json*做為相依性：</span><span class="sxs-lookup"><span data-stu-id="a69df-111">Adding Bootstrap to an ASP.NET Core project is simply a matter of adding it to *bower.json* as a dependency:</span></span>

[!code-json[](../common/samples/WebApplication1/bower.json?highlight=5)]

<span data-ttu-id="a69df-112">這是建議用來啟動程序加入 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="a69df-112">This is the recommended way to add Bootstrap to an ASP.NET Core project.</span></span>

<span data-ttu-id="a69df-113">您也可以安裝使用其中一種數個封裝管理員，例如 Bower、 npm 或 NuGet 的啟動程序。</span><span class="sxs-lookup"><span data-stu-id="a69df-113">You can also install bootstrap using one of several package managers, such as Bower, npm, or NuGet.</span></span> <span data-ttu-id="a69df-114">在每個案例中，流程是基本上相同：</span><span class="sxs-lookup"><span data-stu-id="a69df-114">In each case, the process is essentially the same:</span></span>

### <a name="bower"></a><span data-ttu-id="a69df-115">Bower</span><span class="sxs-lookup"><span data-stu-id="a69df-115">Bower</span></span>

```console
bower install bootstrap
```

### <a name="npm"></a><span data-ttu-id="a69df-116">npm</span><span class="sxs-lookup"><span data-stu-id="a69df-116">npm</span></span>

```console
npm install bootstrap
```

### <a name="nuget"></a><span data-ttu-id="a69df-117">NuGet</span><span class="sxs-lookup"><span data-stu-id="a69df-117">NuGet</span></span>

```console
Install-Package bootstrap
```

> [!NOTE]
> <span data-ttu-id="a69df-118">建議用來安裝用戶端相依性，例如透過 Bower 為啟動程序中 ASP.NET Core (使用*bower.json*，如上所示)。</span><span class="sxs-lookup"><span data-stu-id="a69df-118">The recommended way to install client-side dependencies like Bootstrap in ASP.NET Core is via Bower (using *bower.json*, as shown above).</span></span> <span data-ttu-id="a69df-119">Npm/NuGet 使用會顯示以示範如何輕鬆啟動程序可以加入其他種類的 web 應用程式，包括較早版本的 ASP.NET。</span><span class="sxs-lookup"><span data-stu-id="a69df-119">The use of npm/NuGet are shown to demonstrate how easily Bootstrap can be added to other kinds of web applications, including earlier versions of ASP.NET.</span></span>

<span data-ttu-id="a69df-120">如果您正在參考您自己的啟動程序的本機版本，您必須在使用它的任何頁面中參考它們。</span><span class="sxs-lookup"><span data-stu-id="a69df-120">If you're referencing your own local versions of Bootstrap, you'll need to reference them in any pages that will use it.</span></span> <span data-ttu-id="a69df-121">在生產環境中，您應該參考使用 CDN 的啟動程序。</span><span class="sxs-lookup"><span data-stu-id="a69df-121">In production you should reference bootstrap using a CDN.</span></span> <span data-ttu-id="a69df-122">在預設的 ASP.NET 網站範本， *_Layout.cshtml*檔案並因此如下所示：</span><span class="sxs-lookup"><span data-stu-id="a69df-122">In the default ASP.NET site template, the *_Layout.cshtml* file does so like this:</span></span>

[!code-html[](../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=9,13,51,59)]

> [!NOTE]
> <span data-ttu-id="a69df-123">如果您要使用任何啟動程序的 jQuery 外掛程式，您也必須參考 jQuery。</span><span class="sxs-lookup"><span data-stu-id="a69df-123">If you're going to be using any of Bootstrap's jQuery plugins, you will also need to reference jQuery.</span></span>

## <a name="basic-templates-and-features"></a><span data-ttu-id="a69df-124">基本的範本和功能</span><span class="sxs-lookup"><span data-stu-id="a69df-124">Basic templates and features</span></span>

<span data-ttu-id="a69df-125">最基本的啟動程序範本看起來很像*_Layout.cshtml*檔案顯示更新的版本，並直接包含基本的功能表，用於導覽及呈現網頁的其餘的地方。</span><span class="sxs-lookup"><span data-stu-id="a69df-125">The most basic Bootstrap template looks very much like the *_Layout.cshtml* file shown above, and simply includes a basic menu for navigation and a place to render the rest of the page.</span></span>

### <a name="basic-navigation"></a><span data-ttu-id="a69df-126">基本導覽</span><span class="sxs-lookup"><span data-stu-id="a69df-126">Basic navigation</span></span>

<span data-ttu-id="a69df-127">預設範本會使用一組`<div>`要呈現在上方導覽列和頁面的主體。</span><span class="sxs-lookup"><span data-stu-id="a69df-127">The default template uses a set of `<div>` elements to render a top navbar and the main body of the page.</span></span> <span data-ttu-id="a69df-128">如果您使用 HTML5，您可以取代第一個`<div>`標記`<nav>`標記來取得相同的效果，但有更精確的語意。</span><span class="sxs-lookup"><span data-stu-id="a69df-128">If you're using HTML5, you can replace the first `<div>` tag with a `<nav>` tag to get the same effect, but with more precise semantics.</span></span> <span data-ttu-id="a69df-129">在此第一個`<div>`您可以看到還有其他幾個。</span><span class="sxs-lookup"><span data-stu-id="a69df-129">Within this first `<div>` you can see there are several others.</span></span> <span data-ttu-id="a69df-130">首先， `<div>` "container"，然後在中，兩個類別`<div>`項目: 「 瀏覽列標頭 」 和 「 導覽列摺疊 」。</span><span class="sxs-lookup"><span data-stu-id="a69df-130">First, a `<div>` with a class of "container", and then within that, two more `<div>` elements: "navbar-header" and "navbar-collapse".</span></span> <span data-ttu-id="a69df-131">導覽列標頭 div 包含一個按鈕，會出現如下的某些最小寬度，顯示 3 水平線螢幕時 (所謂 「 漢堡圖示 」)。</span><span class="sxs-lookup"><span data-stu-id="a69df-131">The navbar-header div includes a button that will appear when the screen is below a certain minimum width, showing 3 horizontal lines (a so-called "hamburger icon").</span></span> <span data-ttu-id="a69df-132">使用純 HTML 和 CSS; 呈現圖示需要沒有映像。</span><span class="sxs-lookup"><span data-stu-id="a69df-132">The icon is rendered using pure HTML and CSS; no image is required.</span></span> <span data-ttu-id="a69df-133">這是會顯示圖示，與每個程式碼<span>標記呈現白色橫條的其中一個：</span><span class="sxs-lookup"><span data-stu-id="a69df-133">This is the code that displays the icon, with each of the <span> tags rendering one of the white bars:</span></span>

```html
<button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
</button>
```

<span data-ttu-id="a69df-134">它也包含應用程式名稱，它會出現在左上方。</span><span class="sxs-lookup"><span data-stu-id="a69df-134">It also includes the application name, which appears in the top left.</span></span> <span data-ttu-id="a69df-135">主瀏覽功能表呈現`<ul>`內第二個 div，項目，並包含以首頁、 關於和連絡人的連結。</span><span class="sxs-lookup"><span data-stu-id="a69df-135">The main navigation menu is rendered by the `<ul>` element within the second div, and includes links to Home, About, and Contact.</span></span> <span data-ttu-id="a69df-136">註冊和登入的其他連結會新增一行 29 _LoginPartial 列。</span><span class="sxs-lookup"><span data-stu-id="a69df-136">Additional links for Register and Login are added by the _LoginPartial line on line 29.</span></span> <span data-ttu-id="a69df-137">下方瀏覽，在另一個轉譯的每個頁面主體`<div>`、 標示為 「 容器 」 和 「 本文內容 」 類別。</span><span class="sxs-lookup"><span data-stu-id="a69df-137">Below the navigation, the main body of each page is rendered in another `<div>`, marked with the "container" and "body-content" classes.</span></span> <span data-ttu-id="a69df-138">簡單的預設 _Layout 檔案如下所示，在頁面的內容會轉譯頁面上，然後按一下 簡單與相關聯的特定檢視`<footer>`加入至結尾`<div>`項目。</span><span class="sxs-lookup"><span data-stu-id="a69df-138">In the simple default _Layout file shown here, the contents of the page are rendered by the specific View associated with the page, and then a simple `<footer>` is added to the end of the `<div>` element.</span></span> <span data-ttu-id="a69df-139">您可以看到有關頁面內建的顯示方式使用此範本：</span><span class="sxs-lookup"><span data-stu-id="a69df-139">You can see how the built-in About page appears using this template:</span></span>

![有關頁面](bootstrap/_static/about-page-wide.png)

<span data-ttu-id="a69df-141">視窗低於特定寬度時，會出現 摺疊導覽列中的，使用右上方，「 漢堡 」 按鈕：</span><span class="sxs-lookup"><span data-stu-id="a69df-141">The collapsed navbar, with "hamburger" button in the top right, appears when the window drops below a certain width:</span></span>

![關於漢堡 」 功能表與頁面](bootstrap/_static/about-page-hamburger.png)

<span data-ttu-id="a69df-143">按一下圖示會顯示在頁面頂端會向下滑動垂直 drawer 的功能表項目：</span><span class="sxs-lookup"><span data-stu-id="a69df-143">Clicking the icon reveals the menu items in a vertical drawer that slides down from the top of the page:</span></span>

![展開漢堡功能表與頁面相關](bootstrap/_static/about-page-hamburger-open.png)

### <a name="typography-and-links"></a><span data-ttu-id="a69df-145">印刷樣式和連結</span><span class="sxs-lookup"><span data-stu-id="a69df-145">Typography and links</span></span>

<span data-ttu-id="a69df-146">啟動程序設定站台的基本的印刷樣式、 色彩和格式化其 CSS 檔案中的連結。</span><span class="sxs-lookup"><span data-stu-id="a69df-146">Bootstrap sets up the site's basic typography, colors, and link formatting in its CSS file.</span></span> <span data-ttu-id="a69df-147">這個 CSS 檔案中包含的資料表、 按鈕、 表單項目、 影像和多個預設樣式 ([進一步了解](http://getbootstrap.com/css/))。</span><span class="sxs-lookup"><span data-stu-id="a69df-147">This CSS file includes default styles for tables, buttons, form elements, images, and more ([learn more](http://getbootstrap.com/css/)).</span></span> <span data-ttu-id="a69df-148">一個特別有用的功能是格線版面配置系統，接下來涵蓋。</span><span class="sxs-lookup"><span data-stu-id="a69df-148">One particularly useful feature is the grid layout system, covered next.</span></span>

### <a name="grids"></a><span data-ttu-id="a69df-149">格線</span><span class="sxs-lookup"><span data-stu-id="a69df-149">Grids</span></span>

<span data-ttu-id="a69df-150">啟動程序的最受歡迎的功能之一是其格線版面配置系統。</span><span class="sxs-lookup"><span data-stu-id="a69df-150">One of the most popular features of Bootstrap is its grid layout system.</span></span> <span data-ttu-id="a69df-151">現代化 web 應用程式應該避免使用`<table>`配置，請改為使用這個項目限制實際的表格式資料的標記。</span><span class="sxs-lookup"><span data-stu-id="a69df-151">Modern web applications should avoid using the `<table>` tag for layout, instead restricting the use of this element to actual tabular data.</span></span> <span data-ttu-id="a69df-152">相反地，資料行和資料列可配置使用的一系列`<div>`項目和適當的 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="a69df-152">Instead, columns and rows can be laid out using a series of `<div>` elements and the appropriate CSS classes.</span></span> <span data-ttu-id="a69df-153">有幾個這種方法，包括可調整的版面配置的方格顯示垂直上窄的畫面，例如手機上的好處。</span><span class="sxs-lookup"><span data-stu-id="a69df-153">There are several advantages to this approach, including the ability to adjust the layout of grids to display vertically on narrow screens, such as on phones.</span></span>

<span data-ttu-id="a69df-154">[啟動程序的格線版面配置系統](http://getbootstrap.com/css/#grid)12 個資料欄為基礎。</span><span class="sxs-lookup"><span data-stu-id="a69df-154">[Bootstrap's grid layout system](http://getbootstrap.com/css/#grid) is based on twelve columns.</span></span> <span data-ttu-id="a69df-155">這個數字已選擇，因為它可分為平均 1、 2、 3 或 4 的資料行和資料行的寬度可能會以 1 內/12 的垂直螢幕的寬度。</span><span class="sxs-lookup"><span data-stu-id="a69df-155">This number was chosen because it can be divided evenly into 1, 2, 3, or 4 columns, and column widths can vary to within 1/12th of the vertical width of the screen.</span></span> <span data-ttu-id="a69df-156">若要開始使用格線版面配置系統，您應該開始使用容器`<div>`然後再加入一個資料列`<div>`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="a69df-156">To start using the grid layout system, you should begin with a container `<div>` and then add a row `<div>`, as shown here:</span></span>

```html
<div class="container">
    <div class="row">
        ...
    </div>
</div>
```

<span data-ttu-id="a69df-157">接下來，加入 其他`<div>`項目，每個資料行，並指定資料行數目，`<div>`應佔據 （超出 12) 當做開頭為"col-md-"CSS 類別的一部分。</span><span class="sxs-lookup"><span data-stu-id="a69df-157">Next, add additional `<div>` elements for each column, and specify the number of columns that `<div>` should occupy (out of 12) as part of a CSS class starting with "col-md-".</span></span> <span data-ttu-id="a69df-158">比方說，如果您想要只需要相同大小的兩個資料行，您會針對每個使用"col-md-6"的類別。</span><span class="sxs-lookup"><span data-stu-id="a69df-158">For instance, if you want to simply have two columns of equal size, you would use a class of "col-md-6" for each one.</span></span> <span data-ttu-id="a69df-159">在此情況下"md"是簡稱為 「 中 」，而且參考到標準大小桌面的電腦的顯示大小。</span><span class="sxs-lookup"><span data-stu-id="a69df-159">In this case "md" is short for "medium" and refers to standard-sized desktop computer display sizes.</span></span> <span data-ttu-id="a69df-160">有四個不同的選項，您可以選擇，而每個將用於較高的寬度除非覆寫 （因此如果您想要修正不論螢幕寬度的配置，您可以只需要指定 xs 類別）。</span><span class="sxs-lookup"><span data-stu-id="a69df-160">There are four different options you can choose from, and each will be used for higher widths unless overridden (so if you want the layout to be fixed regardless of screen width, you can just specify xs classes).</span></span>

<span data-ttu-id="a69df-161">CSS 類別的前置詞</span><span class="sxs-lookup"><span data-stu-id="a69df-161">CSS Class Prefix</span></span> | <span data-ttu-id="a69df-162">裝置層</span><span class="sxs-lookup"><span data-stu-id="a69df-162">Device Tier</span></span> | <span data-ttu-id="a69df-163">寬度</span><span class="sxs-lookup"><span data-stu-id="a69df-163">Width</span></span>
:---: | :---: | :---:
<span data-ttu-id="a69df-164">col-xs-</span><span class="sxs-lookup"><span data-stu-id="a69df-164">col-xs-</span></span> | <span data-ttu-id="a69df-165">電話</span><span class="sxs-lookup"><span data-stu-id="a69df-165">Phones</span></span> | <span data-ttu-id="a69df-166">< 768px</span><span class="sxs-lookup"><span data-stu-id="a69df-166">< 768px</span></span>
<span data-ttu-id="a69df-167">col-sm-</span><span class="sxs-lookup"><span data-stu-id="a69df-167">col-sm-</span></span> | <span data-ttu-id="a69df-168">平板電腦</span><span class="sxs-lookup"><span data-stu-id="a69df-168">Tablets</span></span> | <span data-ttu-id="a69df-169">>= 768px</span><span class="sxs-lookup"><span data-stu-id="a69df-169">>= 768px</span></span>
<span data-ttu-id="a69df-170">col-md-</span><span class="sxs-lookup"><span data-stu-id="a69df-170">col-md-</span></span> | <span data-ttu-id="a69df-171">桌上型電腦</span><span class="sxs-lookup"><span data-stu-id="a69df-171">Desktops</span></span> | <span data-ttu-id="a69df-172">>= 992px</span><span class="sxs-lookup"><span data-stu-id="a69df-172">>= 992px</span></span>
<span data-ttu-id="a69df-173">col-lg-</span><span class="sxs-lookup"><span data-stu-id="a69df-173">col-lg-</span></span> | <span data-ttu-id="a69df-174">較大的桌面顯示</span><span class="sxs-lookup"><span data-stu-id="a69df-174">Larger Desktop Displays</span></span> | <span data-ttu-id="a69df-175">>= 1200px</span><span class="sxs-lookup"><span data-stu-id="a69df-175">>= 1200px</span></span>

<span data-ttu-id="a69df-176">當指定的兩個資料行都與 「 資料行-md-6 」 產生的配置將會在桌面的解析度上，兩個資料行，但這兩個資料行上較小裝置 （或較窄的瀏覽器視窗，在桌面上），讓使用者能夠輕鬆地檢視轉譯時就會垂直堆疊而不需要水平捲動內容。</span><span class="sxs-lookup"><span data-stu-id="a69df-176">When specifying two columns both with "col-md-6" the resulting layout will be two columns at desktop resolutions, but these two columns will stack vertically when rendered on smaller devices (or a narrower browser window on a desktop), allowing users to easily view content without the need to scroll horizontally.</span></span>

<span data-ttu-id="a69df-177">永遠都會預設啟動程序至單一資料行配置，因此您只需要指定資料行，當您想要多個資料行。</span><span class="sxs-lookup"><span data-stu-id="a69df-177">Bootstrap will always default to a single-column layout, so you only need to specify columns when you want more than one column.</span></span> <span data-ttu-id="a69df-178">您會想要明確指定的唯一時間`<div>`需要所有的 12 個資料行，就是覆寫較大的裝置層的行為。</span><span class="sxs-lookup"><span data-stu-id="a69df-178">The only time you would want to explicitly specify that a `<div>` take up all 12 columns would be to override the behavior of a larger device tier.</span></span> <span data-ttu-id="a69df-179">當指定多個裝置層類別，您可能需要重設資料行的呈現特定時間點。</span><span class="sxs-lookup"><span data-stu-id="a69df-179">When specifying multiple device tier classes, you may need to reset the column rendering at certain points.</span></span> <span data-ttu-id="a69df-180">將只會顯示某些區內 clearfix div 可以達到這個目的，如下所示：</span><span class="sxs-lookup"><span data-stu-id="a69df-180">Adding a clearfix div that's only visible within a certain viewport can achieve this, as shown here:</span></span>

![窄和寬的檢視區方格](bootstrap/_static/narrow-and-wide-viewport-grid.png)

<span data-ttu-id="a69df-182">在上述範例中，一和二共用"md"版面配置中的資料列兩到三次共用"xs"版面配置中的資料列。</span><span class="sxs-lookup"><span data-stu-id="a69df-182">In the above example, One and Two share a row in the "md" layout, while Two and Three share a row in the "xs" layout.</span></span> <span data-ttu-id="a69df-183">沒有 clearfix `<div>`，兩個和第三不會顯示正確"xs"檢視 （請注意，會顯示只有一個、 四和五個）：</span><span class="sxs-lookup"><span data-stu-id="a69df-183">Without the clearfix `<div>`, Two and Three are not shown correctly in the "xs" view (note that only One, Four, and Five are shown):</span></span>

![不使用 clearfix 方格](bootstrap/_static/grid-without-clearfix.png)

<span data-ttu-id="a69df-185">在此範例中，單一資料列`<div>`被使用，並且啟動程序仍然大部分未正確配置和資料行的堆疊方面的事。</span><span class="sxs-lookup"><span data-stu-id="a69df-185">In this example, only a single row `<div>` was used, and Bootstrap still mostly did the right thing with regard to the layout and stacking of the columns.</span></span> <span data-ttu-id="a69df-186">一般而言，您應該指定一個資料列`<div>`每個水平資料列需要您的配置，以及當然您可以巢狀啟動程序內另一個方格。</span><span class="sxs-lookup"><span data-stu-id="a69df-186">Typically, you should specify a row `<div>` for each horizontal row your layout requires, and of course you can nest Bootstrap grids within one another.</span></span> <span data-ttu-id="a69df-187">當您這樣做時，每個巢狀的方格會佔用 100%的元件，它用來放置，可以再細分使用類別資料行的寬度。</span><span class="sxs-lookup"><span data-stu-id="a69df-187">When you do, each nested grid will occupy 100% of the width of the element in which it's placed, which can then be subdivided using column classes.</span></span>

### <a name="jumbotron"></a><span data-ttu-id="a69df-188">Jumbotron</span><span class="sxs-lookup"><span data-stu-id="a69df-188">Jumbotron</span></span>

<span data-ttu-id="a69df-189">如果您已使用預設 ASP.NET MVC 範本在 Visual Studio 2012 或 2013年中，您可能已經看到 Jumbotron 作用中。</span><span class="sxs-lookup"><span data-stu-id="a69df-189">If you've used the default ASP.NET MVC templates in Visual Studio 2012 or 2013, you've probably seen the Jumbotron in action.</span></span> <span data-ttu-id="a69df-190">它是指大型全形頁面章節包含可以用來顯示大型的背景影像，呼叫動作、 旋轉或類似的項目。</span><span class="sxs-lookup"><span data-stu-id="a69df-190">It refers to a large full-width section of a page that can be used to display a large background image, a call to action, a rotator, or similar elements.</span></span> <span data-ttu-id="a69df-191">若要將 jumbotron 加入頁面中，只要加入`<div>`並為它提供一種"jumbotron"，然後放置在容器`<div>`內並加入您的內容。</span><span class="sxs-lookup"><span data-stu-id="a69df-191">To add a jumbotron to a page, simply add a `<div>` and give it a class of "jumbotron", then place a container `<div>` inside and add your content.</span></span> <span data-ttu-id="a69df-192">我們可以輕易地調整標準的 「 關於使用主要的標題，它會顯示 jumbotron 頁：</span><span class="sxs-lookup"><span data-stu-id="a69df-192">We can easily adjust the standard About page to use a jumbotron for the main headings it displays:</span></span>

![jumbotron 範例](bootstrap/_static/jumbotron.png)

### <a name="buttons"></a><span data-ttu-id="a69df-194">按鈕</span><span class="sxs-lookup"><span data-stu-id="a69df-194">Buttons</span></span>

<span data-ttu-id="a69df-195">預設按鈕的類別和其色彩會在下圖中顯示。</span><span class="sxs-lookup"><span data-stu-id="a69df-195">The default button classes and their colors are shown in the figure below.</span></span>

![佈景主題的按鈕](bootstrap/_static/theme-buttons.png)

### <a name="badges"></a><span data-ttu-id="a69df-197">徽章</span><span class="sxs-lookup"><span data-stu-id="a69df-197">Badges</span></span>

<span data-ttu-id="a69df-198">徽章是指瀏覽項目旁邊的小，通常是數值圖說文字。</span><span class="sxs-lookup"><span data-stu-id="a69df-198">Badges refer to small, usually numeric callouts next to a navigation item.</span></span> <span data-ttu-id="a69df-199">這可能是多個訊息或通知等待或更新的存在。</span><span class="sxs-lookup"><span data-stu-id="a69df-199">They can indicate a number of messages or notifications waiting, or the presence of updates.</span></span> <span data-ttu-id="a69df-200">指定這類徽章很簡單，只新增<span>包含文字，有 [徽章] 的類別：</span><span class="sxs-lookup"><span data-stu-id="a69df-200">Specifying such badges is as simple as adding a <span> containing the text, with a class of "badge":</span></span>

![佈景主題的徽章](bootstrap/_static/theme-badges.png)

### <a name="alerts"></a><span data-ttu-id="a69df-202">警示</span><span class="sxs-lookup"><span data-stu-id="a69df-202">Alerts</span></span>

<span data-ttu-id="a69df-203">您可能需要對應用程式的使用者顯示通知、 警示或錯誤訊息的某種。</span><span class="sxs-lookup"><span data-stu-id="a69df-203">You may need to display some kind of notification, alert, or error message to your application's users.</span></span> <span data-ttu-id="a69df-204">這是其中的標準警示類別會很有用。</span><span class="sxs-lookup"><span data-stu-id="a69df-204">That's where the standard alert classes are useful.</span></span> <span data-ttu-id="a69df-205">有四個不同的嚴重性層級與相關聯的色彩配置：</span><span class="sxs-lookup"><span data-stu-id="a69df-205">There are four different severity levels with associated color schemes:</span></span>

![佈景主題的警示](bootstrap/_static/theme-alerts.png)

### <a name="navbars-and-menus"></a><span data-ttu-id="a69df-207">Navbars 和功能表</span><span class="sxs-lookup"><span data-stu-id="a69df-207">Navbars and menus</span></span>

<span data-ttu-id="a69df-208">我們的版面配置已包含標準的導覽列中，但啟動程序的佈景主題皆支援其他樣式選項。</span><span class="sxs-lookup"><span data-stu-id="a69df-208">Our layout already includes a standard navbar, but the Bootstrap theme supports additional styling options.</span></span> <span data-ttu-id="a69df-209">我們也很容易可以選擇垂直顯示導覽列，而非水平如果有慣用的也會為新增的副導覽中的項目彈出式視窗功能表。</span><span class="sxs-lookup"><span data-stu-id="a69df-209">We can also easily opt to display the navbar vertically rather than horizontally if that's preferred, as well as adding sub-navigation items in flyout menus.</span></span> <span data-ttu-id="a69df-210">最上層的內建索引標籤帶狀線，例如簡單的瀏覽功能表</span><span class="sxs-lookup"><span data-stu-id="a69df-210">Simple navigation menus, like tab strips, are built on top of</span></span> <ul> <span data-ttu-id="a69df-211">項目。</span><span class="sxs-lookup"><span data-stu-id="a69df-211">elements.</span></span> <span data-ttu-id="a69df-212">可以建立非常簡單，只讓他們能夠以 CSS 類別 」 nav 」 和 「 瀏覽索引標籤 」:</span><span class="sxs-lookup"><span data-stu-id="a69df-212">These can be created very simply by just providing them with the CSS classes "nav" and "nav-tabs":</span></span>

![佈景主題 tabstrips](bootstrap/_static/theme-tabstrips.png)

<span data-ttu-id="a69df-214">Navbars 同樣地，內建，但會稍微更複雜。</span><span class="sxs-lookup"><span data-stu-id="a69df-214">Navbars are built similarly, but are a bit more complex.</span></span> <span data-ttu-id="a69df-215">開頭為`<nav>`或`<div>`"導覽列 」，在其中容器 div 保存的項目其餘部分的類別。</span><span class="sxs-lookup"><span data-stu-id="a69df-215">They start with a `<nav>` or `<div>` with a class of "navbar", within which a container div holds the rest of the elements.</span></span> <span data-ttu-id="a69df-216">我們的頁面導覽列在其標頭中已包含 – 如下所示只會展開，新增下拉式功能表的支援：</span><span class="sxs-lookup"><span data-stu-id="a69df-216">Our page includes a navbar in its header already – the one shown below simply expands on this, adding support for a dropdown menu:</span></span>

![佈景主題 navbars](bootstrap/_static/theme-navbars.png)

### <a name="additional-elements"></a><span data-ttu-id="a69df-218">其他項目</span><span class="sxs-lookup"><span data-stu-id="a69df-218">Additional elements</span></span>

<span data-ttu-id="a69df-219">預設佈景主題也可用來呈現 HTML 表格格式樣式，包括支援等量的檢視中。</span><span class="sxs-lookup"><span data-stu-id="a69df-219">The default theme can also be used to present HTML tables in a nicely formatted style, including support for striped views.</span></span> <span data-ttu-id="a69df-220">標籤一共有使用類似於按鈕的樣式。</span><span class="sxs-lookup"><span data-stu-id="a69df-220">There are labels with styles that are similar to those of the buttons.</span></span> <span data-ttu-id="a69df-221">您可以建立自訂支援標準 HTML 以外的其他樣式選項的下拉式功能表`<select>`項目，以及 Navbars 類似我們預設入門網站已在使用中。</span><span class="sxs-lookup"><span data-stu-id="a69df-221">You can create custom Dropdown menus that support additional styling options beyond the standard HTML `<select>` element, along with Navbars like the one our default starter site is already using.</span></span> <span data-ttu-id="a69df-222">如果您需要將進度列，有數種樣式，以供選擇，以及列出群組和包含標題與內容的面板。</span><span class="sxs-lookup"><span data-stu-id="a69df-222">If you need a progress bar, there are several styles to choose from, as well as List Groups and panels that include a title and content.</span></span> <span data-ttu-id="a69df-223">瀏覽標準啟動程序主題這裡內的其他選項：</span><span class="sxs-lookup"><span data-stu-id="a69df-223">Explore additional options within the standard Bootstrap Theme here:</span></span>

[<span data-ttu-id="a69df-224">http://getbootstrap.com/examples/theme/</span><span class="sxs-lookup"><span data-stu-id="a69df-224">http://getbootstrap.com/examples/theme/</span></span>](http://getbootstrap.com/examples/theme/)

## <a name="more-themes"></a><span data-ttu-id="a69df-225">更多主題</span><span class="sxs-lookup"><span data-stu-id="a69df-225">More themes</span></span>

<span data-ttu-id="a69df-226">您可以擴充標準啟動程序的主題，藉由覆寫部分或所有其 CSS 調整色彩和樣式，以符合您自己的應用程式的需求。</span><span class="sxs-lookup"><span data-stu-id="a69df-226">You can extend the standard Bootstrap theme by overriding some or all of its CSS, adjusting the colors and styles to suit your own application's needs.</span></span> <span data-ttu-id="a69df-227">如果您想要從現成的佈景主題開始，有數個主題組件庫提供線上，在啟動程序的佈景主題，例如 WrapBootstrap.com （這有各種不同的商業佈景主題） 和 Bootswatch.com （其提供可用的佈景主題） 中的特製化。</span><span class="sxs-lookup"><span data-stu-id="a69df-227">If you'd like to start from a ready-made theme, there are several theme galleries available online that specialize in Bootstrap themes, such as WrapBootstrap.com (which has a variety of commercial themes) and Bootswatch.com (which offers free themes).</span></span> <span data-ttu-id="a69df-228">某些可用的付費範本提供許多基本的啟動程序佈景主題，例如豐富的支援系統管理功能表和儀表板頂端的功能豐富的圖表和量測計。</span><span class="sxs-lookup"><span data-stu-id="a69df-228">Some of the paid templates available provide a great deal of functionality on top of the basic Bootstrap theme, such as rich support for administrative menus, and dashboards with rich charts and gauges.</span></span> <span data-ttu-id="a69df-229">熱門付費範本的範例是 Inspinia，目前 $18，其中包含除了 AngularJS 和靜態的 HTML 版本的 ASP.NET MVC5 範本的銷售。</span><span class="sxs-lookup"><span data-stu-id="a69df-229">An example of a popular paid template is Inspinia, currently for sale for $18, which includes an ASP.NET MVC5 template in addition to AngularJS and static HTML versions.</span></span> <span data-ttu-id="a69df-230">範例螢幕擷取畫面所示。</span><span class="sxs-lookup"><span data-stu-id="a69df-230">A sample screenshot is shown below.</span></span>

![範例主題 inspinia](bootstrap/_static/theme-inspinia.png)

<span data-ttu-id="a69df-232">如果您想要變更您的啟動程序主題，請將放*bootstrap.css*中您要的佈景主題檔案**wwwroot/css**資料夾，並變更中的參考*_Layout.cshtml*指向它。</span><span class="sxs-lookup"><span data-stu-id="a69df-232">If you want to change your Bootstrap theme, put the *bootstrap.css* file for the theme you want in the **wwwroot/css** folder and change the references in *_Layout.cshtml* to point it.</span></span> <span data-ttu-id="a69df-233">變更用於所有環境的連結：</span><span class="sxs-lookup"><span data-stu-id="a69df-233">Change the links for all environments:</span></span>

```html
<environment names="Development">
    <link rel="stylesheet" href="~/css/bootstrap.css" />
```

```html
<environment names="Staging,Production">
    <link rel="stylesheet" href="~/css/bootstrap.min.css" />
```

<span data-ttu-id="a69df-234">如果您想要建置您自己的儀表板，您可以從可用的範例可從這裡開始： [http://getbootstrap.com/examples/dashboard/](http://getbootstrap.com/examples/dashboard/)。</span><span class="sxs-lookup"><span data-stu-id="a69df-234">If you want to build your own dashboard, you can start from the free example available here: [http://getbootstrap.com/examples/dashboard/](http://getbootstrap.com/examples/dashboard/).</span></span>

## <a name="components"></a><span data-ttu-id="a69df-235">元件</span><span class="sxs-lookup"><span data-stu-id="a69df-235">Components</span></span>

<span data-ttu-id="a69df-236">除了已經討論過這些項目，啟動安裝程式包含支援各種[內建的 UI 元件](http://getbootstrap.com/components/)。</span><span class="sxs-lookup"><span data-stu-id="a69df-236">In addition to those elements already discussed, Bootstrap includes support for a variety of [built-in UI components](http://getbootstrap.com/components/).</span></span>

### <a name="glyphicons"></a><span data-ttu-id="a69df-237">Glyphicons</span><span class="sxs-lookup"><span data-stu-id="a69df-237">Glyphicons</span></span>

<span data-ttu-id="a69df-238">啟動程序包括從 Glyphicons 圖示集 ([http://glyphicons.com](http://glyphicons.com))，有超過 200 圖示免費內啟動安裝程式啟用 web 應用程式使用。</span><span class="sxs-lookup"><span data-stu-id="a69df-238">Bootstrap includes icon sets from Glyphicons ([http://glyphicons.com](http://glyphicons.com)), with over 200 icons freely available for use within your Bootstrap-enabled web application.</span></span> <span data-ttu-id="a69df-239">以下是只是小型的範例：</span><span class="sxs-lookup"><span data-stu-id="a69df-239">Here's just a small sample:</span></span>

![Glyphicons](bootstrap/_static/theme-glyphicons.png)

### <a name="input-groups"></a><span data-ttu-id="a69df-241">輸入的群組</span><span class="sxs-lookup"><span data-stu-id="a69df-241">Input groups</span></span>

<span data-ttu-id="a69df-242">輸入的群組允許結合在一起的其他文字或按鈕與輸入項目，讓使用者更直覺的經驗：</span><span class="sxs-lookup"><span data-stu-id="a69df-242">Input groups allow bundling of additional text or buttons with an input element, providing the user with a more intuitive experience:</span></span>

![輸入的群組](bootstrap/_static/input-groups.png)

### <a name="breadcrumbs"></a><span data-ttu-id="a69df-244">階層連結列</span><span class="sxs-lookup"><span data-stu-id="a69df-244">Breadcrumbs</span></span>

<span data-ttu-id="a69df-245">階層連結列會用來顯示使用者，其最新歷程記錄或網站的導覽階層內的深度的通用 UI 元件。</span><span class="sxs-lookup"><span data-stu-id="a69df-245">Breadcrumbs are a common UI component used to show a user their recent history or depth within a site's navigation hierarchy.</span></span> <span data-ttu-id="a69df-246">透過將"階層連結 」 類別套用至任何輕鬆地新增`<ol>`清單項目。</span><span class="sxs-lookup"><span data-stu-id="a69df-246">Add them easily by applying the "breadcrumb" class to any `<ol>` list element.</span></span> <span data-ttu-id="a69df-247">包含內建支援分頁上使用 「 重新編頁 」 類別`<ul>`內的項目`<nav>`。</span><span class="sxs-lookup"><span data-stu-id="a69df-247">Include built-in support for pagination by using the "pagination" class on a `<ul>` element within a `<nav>`.</span></span> <span data-ttu-id="a69df-248">將能繼續回應的內嵌投影片及視訊新增使用`<iframe>`， `<embed>`， `<video>`，或`<object>`項目，則啟動程序會自動設定樣式。</span><span class="sxs-lookup"><span data-stu-id="a69df-248">Add responsive embedded slideshows and video by using `<iframe>`, `<embed>`, `<video>`, or `<object>` elements, which Bootstrap will style automatically.</span></span> <span data-ttu-id="a69df-249">使用特定的類別，例如 「 內嵌-回應-16by9"來指定特定的長寬比。</span><span class="sxs-lookup"><span data-stu-id="a69df-249">Specify a particular aspect ratio by using specific classes like "embed-responsive-16by9".</span></span>

## <a name="javascript-support"></a><span data-ttu-id="a69df-250">JavaScript 支援</span><span class="sxs-lookup"><span data-stu-id="a69df-250">JavaScript support</span></span>

<span data-ttu-id="a69df-251">啟動安裝程式的 JavaScript 程式庫包含共用的元件，可讓您控制它們的行為，以程式設計方式在應用程式中的應用程式開發介面支援。</span><span class="sxs-lookup"><span data-stu-id="a69df-251">Bootstrap's JavaScript library includes API support for the included components, allowing you to control their behavior programmatically within your application.</span></span> <span data-ttu-id="a69df-252">此外， *bootstrap.js*包含鋮自訂 jQuery 外掛程式，提供額外的功能，例如轉換之後，強制回應對話方塊中，捲動偵測 （更新樣式根據使用者已捲動文件中的位置），摺疊行為，可提領轉盤，並加至視窗，因此它們不捲動超出螢幕的功能表。</span><span class="sxs-lookup"><span data-stu-id="a69df-252">In addition, *bootstrap.js* includes over a dozen custom jQuery plugins, providing additional features like transitions, modal dialogs, scroll detection (updating styles based on where the user has scrolled in the document), collapse behavior, carousels, and affixing menus to the window so they don't scroll off the screen.</span></span> <span data-ttu-id="a69df-253">沒有足夠空間可以涵蓋所有的啟動程序 – 若要了解更多，請瀏覽內建 JavaScript 附加[http://getbootstrap.com/javascript/](http://getbootstrap.com/javascript/)。</span><span class="sxs-lookup"><span data-stu-id="a69df-253">There's not sufficient room to cover all of the JavaScript add-ons built into Bootstrap – to learn more please visit [http://getbootstrap.com/javascript/](http://getbootstrap.com/javascript/).</span></span>

## <a name="summary"></a><span data-ttu-id="a69df-254">總結</span><span class="sxs-lookup"><span data-stu-id="a69df-254">Summary</span></span>

<span data-ttu-id="a69df-255">啟動程序會提供一種 web 架構，可用來快速且有效率地配置和樣式各種不同的網站和應用程式。</span><span class="sxs-lookup"><span data-stu-id="a69df-255">Bootstrap provides a web framework that can be used to quickly and productively lay out and style a wide variety of websites and applications.</span></span> <span data-ttu-id="a69df-256">其基本的印刷樣式和樣式提供愉快的外觀及操作，可輕鬆地透過自訂的佈景主題支援，可以對手工或購買商業上操作。</span><span class="sxs-lookup"><span data-stu-id="a69df-256">Its basic typography and styles provide a pleasant look and feel that can easily be manipulated through custom theme support, which can be hand-crafted or purchased commercially.</span></span> <span data-ttu-id="a69df-257">它支援的 web 元件在過去就已經要求昂貴的協力廠商控制項，來完成，同時支援現代化和開啟 web 標準的主機。</span><span class="sxs-lookup"><span data-stu-id="a69df-257">It supports a host of web components that in the past would've required expensive third-party controls to accomplish, while supporting modern and open web standards.</span></span>
