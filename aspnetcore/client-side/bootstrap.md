---
title: 建置美觀的回應式網站啟動程序和 ASP.NET Core
author: ardalis
description: 了解如何開發使用 ASP.NET Core 回應靈敏的 web 應用程式時，用於啟動程序。
ms.author: riande
ms.date: 10/14/2016
uid: client-side/bootstrap
ms.openlocfilehash: 1ccf33b299739f5aa963a53feb70b44b290443ca
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827260"
---
# <a name="build-beautiful-responsive-sites-with-bootstrap-and-aspnet-core"></a><span data-ttu-id="e70f7-103">建置美觀的回應式網站啟動程序和 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e70f7-103">Build beautiful, responsive sites with Bootstrap and ASP.NET Core</span></span>

<a name="bootstrap-index"></a>

<span data-ttu-id="e70f7-104">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e70f7-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e70f7-105">Bootstrap 是目前用來開發響應式 Web 應用程式的最熱門 Web 架構。</span><span class="sxs-lookup"><span data-stu-id="e70f7-105">Bootstrap is currently the most popular web framework for developing responsive web applications.</span></span> <span data-ttu-id="e70f7-106">無論您在前端的設計和開發方面是新手或專家，它都提供了數種功能可以協助您改善網站的使用者體驗。</span><span class="sxs-lookup"><span data-stu-id="e70f7-106">It offers a number of features and benefits that can improve your users' experience with your web site, whether you're a novice at front-end design and development or an expert.</span></span> <span data-ttu-id="e70f7-107">Bootstrap 在部署的時候會是一組 CSS 和 JavaScript 檔案，這些檔案可以協助您讓網站和應用程式更有效率地延伸到手機、平板和桌上型電腦。</span><span class="sxs-lookup"><span data-stu-id="e70f7-107">Bootstrap is deployed as a set of CSS and JavaScript files, and is designed to help your website or application scale efficiently from phones to tablets to desktops.</span></span>

## <a name="get-started"></a><span data-ttu-id="e70f7-108">開始使用</span><span class="sxs-lookup"><span data-stu-id="e70f7-108">Get started</span></span>

<span data-ttu-id="e70f7-109">有幾種方式來開始使用 Bootstrap。</span><span class="sxs-lookup"><span data-stu-id="e70f7-109">There are several ways to get started with Bootstrap.</span></span> <span data-ttu-id="e70f7-110">如果您在 Visual Studio 中開始新的 web 應用程式，您可以選擇預設的入門範本適用於 ASP.NET Core，案例的 Bootstrap 將會預先安裝：</span><span class="sxs-lookup"><span data-stu-id="e70f7-110">If you're starting a new web application in Visual Studio, you can choose the default starter template for ASP.NET Core, in which case Bootstrap will come pre-installed:</span></span>

![在入門範本方案 檢視啟動程序](bootstrap/_static/bootstrap-in-starter-template.png)

<span data-ttu-id="e70f7-112">若要將 Bootstrap 加入 ASP.NET Core 專案，只要將它加入*bower.json* 做為相依性即可：</span><span class="sxs-lookup"><span data-stu-id="e70f7-112">Adding Bootstrap to an ASP.NET Core project is simply a matter of adding it to *bower.json* as a dependency:</span></span>

[!code-json[](../common/samples/WebApplication1/bower.json?highlight=5)]

<span data-ttu-id="e70f7-113">建議採用這種方式來將 Bootstrap 加入 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="e70f7-113">This is the recommended way to add Bootstrap to an ASP.NET Core project.</span></span>

<span data-ttu-id="e70f7-114">您也可以使用 Bower、npm 或 NuGet 等套件管理員來安裝 Bootstrap。不管使用何種方式，流程基本上相同：</span><span class="sxs-lookup"><span data-stu-id="e70f7-114">You can also install bootstrap using one of several package managers, such as Bower, npm, or NuGet.</span></span> <span data-ttu-id="e70f7-115">在每個案例中，處理程序基本上都是一樣：</span><span class="sxs-lookup"><span data-stu-id="e70f7-115">In each case, the process is essentially the same:</span></span>

### <a name="bower"></a><span data-ttu-id="e70f7-116">Bower</span><span class="sxs-lookup"><span data-stu-id="e70f7-116">Bower</span></span>

```console
bower install bootstrap
```

### <a name="npm"></a><span data-ttu-id="e70f7-117">npm</span><span class="sxs-lookup"><span data-stu-id="e70f7-117">npm</span></span>

```console
npm install bootstrap
```

### <a name="nuget"></a><span data-ttu-id="e70f7-118">NuGet</span><span class="sxs-lookup"><span data-stu-id="e70f7-118">NuGet</span></span>

```console
Install-Package bootstrap
```

> [!NOTE]
> <span data-ttu-id="e70f7-119">在 ASP.NET Core 中安裝如 Bootstrap 這類用戶端相依性的建議作法，是透過 Bower (使用*bower.json*，如上所示)。</span><span class="sxs-lookup"><span data-stu-id="e70f7-119">The recommended way to install client-side dependencies like Bootstrap in ASP.NET Core is via Bower (using *bower.json*, as shown above).</span></span> <span data-ttu-id="e70f7-120">使用 npm/NuGet 會顯示可示範如何輕鬆地啟動程序可以加入其他類型的 web 應用程式，包括舊版的 ASP.NET。</span><span class="sxs-lookup"><span data-stu-id="e70f7-120">The use of npm/NuGet are shown to demonstrate how easily Bootstrap can be added to other kinds of web applications, including earlier versions of ASP.NET.</span></span>

<span data-ttu-id="e70f7-121">如果您正在參考您自己的 Bootstrap 本機版本，您必須在使用它的任何頁面中參考它們。</span><span class="sxs-lookup"><span data-stu-id="e70f7-121">If you're referencing your own local versions of Bootstrap, you'll need to reference them in any pages that will use it.</span></span> <span data-ttu-id="e70f7-122">在生產環境中，您應該使用 CDN 來參考 Bootstrap。</span><span class="sxs-lookup"><span data-stu-id="e70f7-122">In production you should reference bootstrap using a CDN.</span></span> <span data-ttu-id="e70f7-123">在預設 ASP.NET Core 網站範本中， *_Layout.cshtml*檔案因此像這樣：</span><span class="sxs-lookup"><span data-stu-id="e70f7-123">In the default ASP.NET Core site template, the *_Layout.cshtml* file does so like this:</span></span>

[!code-html[](../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=9,13,51,59)]

> [!NOTE]
> <span data-ttu-id="e70f7-124">如果您要使用任何 Bootstrap 的 jQuery 外掛程式，您也必須參考 jQuery。
</span><span class="sxs-lookup"><span data-stu-id="e70f7-124">If you're going to be using any of Bootstrap's jQuery plugins, you will also need to reference jQuery.</span></span>

## <a name="basic-templates-and-features"></a><span data-ttu-id="e70f7-125">基本的範本和功能</span><span class="sxs-lookup"><span data-stu-id="e70f7-125">Basic templates and features</span></span>

<span data-ttu-id="e70f7-126">最基本的 Bootstrap 範本看起來很像上列 *_Layout.cshtml*檔案，其中僅包含基本的導覽功能表，以及用於及呈現其餘網頁的空間。</span><span class="sxs-lookup"><span data-stu-id="e70f7-126">The most basic Bootstrap template looks very much like the *_Layout.cshtml* file shown above, and simply includes a basic menu for navigation and a place to render the rest of the page.</span></span>

### <a name="basic-navigation"></a><span data-ttu-id="e70f7-127">基本導覽</span><span class="sxs-lookup"><span data-stu-id="e70f7-127">Basic navigation</span></span>

<span data-ttu-id="e70f7-128">預設範本會使用一組`<div>`要呈現在上方導覽列和頁面的主體。</span><span class="sxs-lookup"><span data-stu-id="e70f7-128">The default template uses a set of `<div>` elements to render a top navbar and the main body of the page.</span></span> <span data-ttu-id="e70f7-129">如果您使用 HTML5，您可以取代第一個`<div>`標記`<nav>`標記來取得相同的效果，但具有更精確的語意。</span><span class="sxs-lookup"><span data-stu-id="e70f7-129">If you're using HTML5, you can replace the first `<div>` tag with a `<nav>` tag to get the same effect, but with more precise semantics.</span></span> <span data-ttu-id="e70f7-130">在此第一個`<div>`您可以看到有幾個其他人。</span><span class="sxs-lookup"><span data-stu-id="e70f7-130">Within this first `<div>` you can see there are several others.</span></span> <span data-ttu-id="e70f7-131">首先，`<div>`的 [容器]，然後內的兩個以上的類別`<div>`項目: 「 navbar 標頭 」 和 「 導覽列摺疊 」。</span><span class="sxs-lookup"><span data-stu-id="e70f7-131">First, a `<div>` with a class of "container", and then within that, two more `<div>` elements: "navbar-header" and "navbar-collapse".</span></span> <span data-ttu-id="e70f7-132">導覽列標題 div 包含某些最小寬度，顯示 3 個水平線下方螢幕時，會出現一個按鈕 (所謂 「 漢堡圖示 」)。</span><span class="sxs-lookup"><span data-stu-id="e70f7-132">The navbar-header div includes a button that will appear when the screen is below a certain minimum width, showing 3 horizontal lines (a so-called "hamburger icon").</span></span> <span data-ttu-id="e70f7-133">使用純 HTML 和 CSS; 呈現圖示需要任何映像不。</span><span class="sxs-lookup"><span data-stu-id="e70f7-133">The icon is rendered using pure HTML and CSS; no image is required.</span></span> <span data-ttu-id="e70f7-134">這是會顯示圖示，與每個程式碼<span>轉譯其中一個白色橫條的標記：</span><span class="sxs-lookup"><span data-stu-id="e70f7-134">This is the code that displays the icon, with each of the <span> tags rendering one of the white bars:</span></span>

```html
<button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
</button>
```

<span data-ttu-id="e70f7-135">它也包含應用程式名稱，就會出現在左上方。</span><span class="sxs-lookup"><span data-stu-id="e70f7-135">It also includes the application name, which appears in the top left.</span></span> <span data-ttu-id="e70f7-136">主導覽功能表呈現`<ul>`在第二個的 div 內的項目，並包含以首頁、 關於和連絡人的連結。</span><span class="sxs-lookup"><span data-stu-id="e70f7-136">The main navigation menu is rendered by the `<ul>` element within the second div, and includes links to Home, About, and Contact.</span></span> <span data-ttu-id="e70f7-137">下方瀏覽，每個頁面的主體會轉譯在另一個`<div>`、 標示與 「 容器 」 和 「 內容 」 類別。</span><span class="sxs-lookup"><span data-stu-id="e70f7-137">Below the navigation, the main body of each page is rendered in another `<div>`, marked with the "container" and "body-content" classes.</span></span> <span data-ttu-id="e70f7-138">在預設的簡單\_版面配置檔案處所，頁面上，然後按一下 簡單相關聯的特定檢視所呈現頁面的內容`<footer>`新增至結尾`<div>`項目。</span><span class="sxs-lookup"><span data-stu-id="e70f7-138">In the simple default \_Layout file shown here, the contents of the page are rendered by the specific View associated with the page, and then a simple `<footer>` is added to the end of the `<div>` element.</span></span> <span data-ttu-id="e70f7-139">您可以看到關於頁面內建的顯示方式使用此範本：</span><span class="sxs-lookup"><span data-stu-id="e70f7-139">You can see how the built-in About page appears using this template:</span></span>

![關於頁面](bootstrap/_static/about-page-wide.png)

<span data-ttu-id="e70f7-141">視窗低於特定寬度時，會出現 摺疊導覽列中的，使用右上方，「 漢堡 」 按鈕：</span><span class="sxs-lookup"><span data-stu-id="e70f7-141">The collapsed navbar, with "hamburger" button in the top right, appears when the window drops below a certain width:</span></span>

![有關 「 漢堡功能表 」 頁面](bootstrap/_static/about-page-hamburger.png)

<span data-ttu-id="e70f7-143">按一下圖示即可顯示功能表項目，從頁面頂端會向下滑動垂直下拉式清單中：</span><span class="sxs-lookup"><span data-stu-id="e70f7-143">Clicking the icon reveals the menu items in a vertical drawer that slides down from the top of the page:</span></span>

![有關 「 擴充的漢堡功能表 」 頁面](bootstrap/_static/about-page-hamburger-open.png)

### <a name="typography-and-links"></a><span data-ttu-id="e70f7-145">印刷樣式和連結</span><span class="sxs-lookup"><span data-stu-id="e70f7-145">Typography and links</span></span>

<span data-ttu-id="e70f7-146">啟動程序會設定站台的基本的印刷樣式、 色彩和格式化其 CSS 檔案中的連結。</span><span class="sxs-lookup"><span data-stu-id="e70f7-146">Bootstrap sets up the site's basic typography, colors, and link formatting in its CSS file.</span></span> <span data-ttu-id="e70f7-147">此 CSS 檔案包含預設樣式，如資料表、 按鈕、 表單項目、 影像和多個 ([了解更多](http://getbootstrap.com/css/))。</span><span class="sxs-lookup"><span data-stu-id="e70f7-147">This CSS file includes default styles for tables, buttons, form elements, images, and more ([learn more](http://getbootstrap.com/css/)).</span></span> <span data-ttu-id="e70f7-148">接下來將說明一個特別有用的功能，亦即格線版面配置系統。</span><span class="sxs-lookup"><span data-stu-id="e70f7-148">One particularly useful feature is the grid layout system, covered next.</span></span>

### <a name="grids"></a><span data-ttu-id="e70f7-149">方格</span><span class="sxs-lookup"><span data-stu-id="e70f7-149">Grids</span></span>

<span data-ttu-id="e70f7-150">Bootstrap 的最受歡迎的功能之一是其格線版面配置系統。</span><span class="sxs-lookup"><span data-stu-id="e70f7-150">One of the most popular features of Bootstrap is its grid layout system.</span></span> <span data-ttu-id="e70f7-151">現代化 Web 應用程式應該避免使用 `<table>`版面配置，而只有針對實際的表格資料來使用此元素。</span><span class="sxs-lookup"><span data-stu-id="e70f7-151">Modern web applications should avoid using the `<table>` tag for layout, instead restricting the use of this element to actual tabular data.</span></span> <span data-ttu-id="e70f7-152">相反地，使用的一系列`<div>`元素和適當的 CSS 類別便能配置資料行和資料列。</span><span class="sxs-lookup"><span data-stu-id="e70f7-152">Instead, columns and rows can be laid out using a series of `<div>` elements and the appropriate CSS classes.</span></span> <span data-ttu-id="e70f7-153">此方法有數個優點，包括可以調整格線的版面配置，以便在手機等窄螢幕上垂直顯示。</span><span class="sxs-lookup"><span data-stu-id="e70f7-153">There are several advantages to this approach, including the ability to adjust the layout of grids to display vertically on narrow screens, such as on phones.</span></span>

<span data-ttu-id="e70f7-154">[Bootstrap 的格線版面配置系統](http://getbootstrap.com/css/#grid)是以 12 個資料欄為基礎。</span><span class="sxs-lookup"><span data-stu-id="e70f7-154">[Bootstrap's grid layout system](http://getbootstrap.com/css/#grid) is based on twelve columns.</span></span> <span data-ttu-id="e70f7-155">會選擇這個數字是因為它可被整除而成為 1、2、3 或 4 資料行，且資料行的寬度可以在螢幕垂直寬度的 1/12 內變化。</span><span class="sxs-lookup"><span data-stu-id="e70f7-155">This number was chosen because it can be divided evenly into 1, 2, 3, or 4 columns, and column widths can vary to within 1/12th of the vertical width of the screen.</span></span> <span data-ttu-id="e70f7-156">若要開始使用格線版面配置系統，您應該開始使用容器 `<div>`然後如下所示新增一個資料列`<div>`：</span><span class="sxs-lookup"><span data-stu-id="e70f7-156">To start using the grid layout system, you should begin with a container `<div>` and then add a row `<div>`, as shown here:</span></span>

```html
<div class="container">
    <div class="row">
        ...
    </div>
</div>
```

<span data-ttu-id="e70f7-157">接下來，新增 額外`<div>`項目，每個資料行，並指定資料行數目，`<div>`開頭 」 資料行-md-」 的 CSS 類別的一部分 （共 12) 應佔據。</span><span class="sxs-lookup"><span data-stu-id="e70f7-157">Next, add additional `<div>` elements for each column, and specify the number of columns that `<div>` should occupy (out of 12) as part of a CSS class starting with "col-md-".</span></span> <span data-ttu-id="e70f7-158">比方說，如果您想要只有相同大小的兩個資料行，您就會針對每個使用"資料行-md-6"的類別。</span><span class="sxs-lookup"><span data-stu-id="e70f7-158">For instance, if you want to simply have two columns of equal size, you would use a class of "col-md-6" for each one.</span></span> <span data-ttu-id="e70f7-159">在此情況下"md"會簡稱為 「 中 」，而是指標準大小的桌上電腦的顯示大小。</span><span class="sxs-lookup"><span data-stu-id="e70f7-159">In this case "md" is short for "medium" and refers to standard-sized desktop computer display sizes.</span></span> <span data-ttu-id="e70f7-160">有四個不同的選項，您可以從中選擇，以及每個將用於較高的寬度除非覆寫 （因此如果您想要不論螢幕寬度修正版面配置，您可以只指定 xs 類別）。</span><span class="sxs-lookup"><span data-stu-id="e70f7-160">There are four different options you can choose from, and each will be used for higher widths unless overridden (so if you want the layout to be fixed regardless of screen width, you can just specify xs classes).</span></span>

<span data-ttu-id="e70f7-161">CSS 類別的前置詞</span><span class="sxs-lookup"><span data-stu-id="e70f7-161">CSS Class Prefix</span></span> | <span data-ttu-id="e70f7-162">裝置層</span><span class="sxs-lookup"><span data-stu-id="e70f7-162">Device Tier</span></span> | <span data-ttu-id="e70f7-163">寬度</span><span class="sxs-lookup"><span data-stu-id="e70f7-163">Width</span></span>
:---: | :---: | :---:
<span data-ttu-id="e70f7-164">col-xs-</span><span class="sxs-lookup"><span data-stu-id="e70f7-164">col-xs-</span></span> | <span data-ttu-id="e70f7-165">手機</span><span class="sxs-lookup"><span data-stu-id="e70f7-165">Phones</span></span> | <span data-ttu-id="e70f7-166">< 768px</span><span class="sxs-lookup"><span data-stu-id="e70f7-166">< 768px</span></span>
<span data-ttu-id="e70f7-167">資料行-sm-</span><span class="sxs-lookup"><span data-stu-id="e70f7-167">col-sm-</span></span> | <span data-ttu-id="e70f7-168">平板電腦</span><span class="sxs-lookup"><span data-stu-id="e70f7-168">Tablets</span></span> | <span data-ttu-id="e70f7-169">>= 768px</span><span class="sxs-lookup"><span data-stu-id="e70f7-169">>= 768px</span></span>
<span data-ttu-id="e70f7-170">資料行-md-</span><span class="sxs-lookup"><span data-stu-id="e70f7-170">col-md-</span></span> | <span data-ttu-id="e70f7-171">桌上型電腦</span><span class="sxs-lookup"><span data-stu-id="e70f7-171">Desktops</span></span> | <span data-ttu-id="e70f7-172">>= 992px</span><span class="sxs-lookup"><span data-stu-id="e70f7-172">>= 992px</span></span>
<span data-ttu-id="e70f7-173">資料行-lg-</span><span class="sxs-lookup"><span data-stu-id="e70f7-173">col-lg-</span></span> | <span data-ttu-id="e70f7-174">較大的桌面顯示</span><span class="sxs-lookup"><span data-stu-id="e70f7-174">Larger Desktop Displays</span></span> | <span data-ttu-id="e70f7-175">> = 1200px</span><span class="sxs-lookup"><span data-stu-id="e70f7-175">>= 1200px</span></span>

<span data-ttu-id="e70f7-176">當指定兩個資料行都使用 「 資料行-md-6 」 產生的版面配置將會在桌面的解析度的兩個資料行，但這些兩個資料行上較小的裝置 （或較窄的瀏覽器視窗，在桌上型電腦上），可讓使用者輕鬆地檢視轉譯時就會垂直堆疊而不需要以水平捲動的內容。</span><span class="sxs-lookup"><span data-stu-id="e70f7-176">When specifying two columns both with "col-md-6" the resulting layout will be two columns at desktop resolutions, but these two columns will stack vertically when rendered on smaller devices (or a narrower browser window on a desktop), allowing users to easily view content without the need to scroll horizontally.</span></span>

<span data-ttu-id="e70f7-177">Bootstrap 總是預設為單一資料行配置，因此只有當您想要多個資料行時，才需要指定資料行。</span><span class="sxs-lookup"><span data-stu-id="e70f7-177">Bootstrap will always default to a single-column layout, so you only need to specify columns when you want more than one column.</span></span> <span data-ttu-id="e70f7-178">若要覆寫較大的裝置層行為，才需要明確指定 `<div>`需要所有 12 個資料行。</span><span class="sxs-lookup"><span data-stu-id="e70f7-178">The only time you would want to explicitly specify that a `<div>` take up all 12 columns would be to override the behavior of a larger device tier.</span></span> <span data-ttu-id="e70f7-179">當指定多個裝置層類別時，可能需要在某些地方重設資料行的呈現。</span><span class="sxs-lookup"><span data-stu-id="e70f7-179">When specifying multiple device tier classes, you may need to reset the column rendering at certain points.</span></span> <span data-ttu-id="e70f7-180">如下所示，新增只能在特定檢視區中看到的 clearfix div，便可以達到這個目的：</span><span class="sxs-lookup"><span data-stu-id="e70f7-180">Adding a clearfix div that's only visible within a certain viewport can achieve this, as shown here:</span></span>

![窄和寬的檢視區方格](bootstrap/_static/narrow-and-wide-viewport-grid.png)

<span data-ttu-id="e70f7-182">在上述範例中，一和二共用"md"版面配置中的資料列時顯示三個兩個共用"xs"版面配置中的資料列。</span><span class="sxs-lookup"><span data-stu-id="e70f7-182">In the above example, One and Two share a row in the "md" layout, while Two and Three share a row in the "xs" layout.</span></span> <span data-ttu-id="e70f7-183">沒有 clearfix `<div>`，二和三不會顯示正確"xs"檢視 （請注意，會顯示只有一個、 四和五個）：</span><span class="sxs-lookup"><span data-stu-id="e70f7-183">Without the clearfix `<div>`, Two and Three are not shown correctly in the "xs" view (note that only One, Four, and Five are shown):</span></span>

![不使用 clearfix 方格](bootstrap/_static/grid-without-clearfix.png)

<span data-ttu-id="e70f7-185">在此範例中，只使用了單一資料列`<div>`且 Bootstrap 對於版面配置和資料行堆疊大致正確</span><span class="sxs-lookup"><span data-stu-id="e70f7-185">In this example, only a single row `<div>` was used, and Bootstrap still mostly did the right thing with regard to the layout and stacking of the columns.</span></span> <span data-ttu-id="e70f7-186">一般而言，您應該為版面配置所需的每個水平列指定一個資料列`<div>`而且 Bootstrap 格線當然可以使用巢狀架構。</span><span class="sxs-lookup"><span data-stu-id="e70f7-186">Typically, you should specify a row `<div>` for each horizontal row your layout requires, and of course you can nest Bootstrap grids within one another.</span></span> <span data-ttu-id="e70f7-187">當您這樣做時，每個巢狀的方格會佔用 100%的元件在其中，它會放置，可以再細分使用類別資料行的寬度。</span><span class="sxs-lookup"><span data-stu-id="e70f7-187">When you do, each nested grid will occupy 100% of the width of the element in which it's placed, which can then be subdivided using column classes.</span></span>

### <a name="jumbotron"></a><span data-ttu-id="e70f7-188">Jumbotron</span><span class="sxs-lookup"><span data-stu-id="e70f7-188">Jumbotron</span></span>

<span data-ttu-id="e70f7-189">如果您已使用預設 ASP.NET Core MVC 範本，在 Visual Studio 2012 或 2013年中，您可能已經發現 Jumbotron 作用中。</span><span class="sxs-lookup"><span data-stu-id="e70f7-189">If you've used the default ASP.NET Core MVC templates in Visual Studio 2012 or 2013, you've probably seen the Jumbotron in action.</span></span> <span data-ttu-id="e70f7-190">它是指大型的全形區域，可用來顯示大型的背景影像，動作、 輪值表或類似的項目呼叫的頁面。</span><span class="sxs-lookup"><span data-stu-id="e70f7-190">It refers to a large full-width section of a page that can be used to display a large background image, a call to action, a rotator, or similar elements.</span></span> <span data-ttu-id="e70f7-191">若要將 jumbotron 加入頁面，只要加入`<div>`並為它提供的類別 「 jumbotron"，然後將容器`<div>`內，並新增您的內容。</span><span class="sxs-lookup"><span data-stu-id="e70f7-191">To add a jumbotron to a page, simply add a `<div>` and give it a class of "jumbotron", then place a container `<div>` inside and add your content.</span></span> <span data-ttu-id="e70f7-192">我們可以輕鬆地調整標準的 「 關於使用主要的標題，它會顯示 jumbotron 頁：</span><span class="sxs-lookup"><span data-stu-id="e70f7-192">We can easily adjust the standard About page to use a jumbotron for the main headings it displays:</span></span>

![jumbotron 範例](bootstrap/_static/jumbotron.png)

### <a name="buttons"></a><span data-ttu-id="e70f7-194">按鈕</span><span class="sxs-lookup"><span data-stu-id="e70f7-194">Buttons</span></span>

<span data-ttu-id="e70f7-195">預設按鈕的類別和其色彩會在下圖中顯示。</span><span class="sxs-lookup"><span data-stu-id="e70f7-195">The default button classes and their colors are shown in the figure below.</span></span>

![佈景主題的按鈕](bootstrap/_static/theme-buttons.png)

### <a name="badges"></a><span data-ttu-id="e70f7-197">徽章</span><span class="sxs-lookup"><span data-stu-id="e70f7-197">Badges</span></span>

<span data-ttu-id="e70f7-198">徽章是指瀏覽項目旁邊的小，通常是數值圖說文字。</span><span class="sxs-lookup"><span data-stu-id="e70f7-198">Badges refer to small, usually numeric callouts next to a navigation item.</span></span> <span data-ttu-id="e70f7-199">它們可以表示多個訊息或通知等候或更新的目前狀態。</span><span class="sxs-lookup"><span data-stu-id="e70f7-199">They can indicate a number of messages or notifications waiting, or the presence of updates.</span></span> <span data-ttu-id="e70f7-200">指定這類徽章很簡單，只要新增`<span>`包含文字，具有 「 徽章 」 的類別：</span><span class="sxs-lookup"><span data-stu-id="e70f7-200">Specifying such badges is as simple as adding a `<span>` containing the text, with a class of "badge":</span></span>

![佈景主題的徽章](bootstrap/_static/theme-badges.png)

### <a name="alerts"></a><span data-ttu-id="e70f7-202">警示</span><span class="sxs-lookup"><span data-stu-id="e70f7-202">Alerts</span></span>

<span data-ttu-id="e70f7-203">您可能需要對您的應用程式使用者顯示某種類型的通知、 警示或錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="e70f7-203">You may need to display some kind of notification, alert, or error message to your application's users.</span></span> <span data-ttu-id="e70f7-204">這是標準的警示類別適合的位置。</span><span class="sxs-lookup"><span data-stu-id="e70f7-204">That's where the standard alert classes are useful.</span></span> <span data-ttu-id="e70f7-205">有四個不同的嚴重性層級與相關聯的色彩配置：</span><span class="sxs-lookup"><span data-stu-id="e70f7-205">There are four different severity levels with associated color schemes:</span></span>

![佈景主題的警示](bootstrap/_static/theme-alerts.png)

### <a name="navbars-and-menus"></a><span data-ttu-id="e70f7-207">導覽列和功能表</span><span class="sxs-lookup"><span data-stu-id="e70f7-207">Navbars and menus</span></span>

<span data-ttu-id="e70f7-208">我們的版面配置已包含標準的導覽列，但 Bootstrap 的佈景主題支援額外的樣式選項。</span><span class="sxs-lookup"><span data-stu-id="e70f7-208">Our layout already includes a standard navbar, but the Bootstrap theme supports additional styling options.</span></span> <span data-ttu-id="e70f7-209">我們也可以很容易地依偏好來選擇垂直顯示導覽列，而非水平顯示，同時也可以新增子導覽項目至彈出式視窗功能表。</span><span class="sxs-lookup"><span data-stu-id="e70f7-209">We can also easily opt to display the navbar vertically rather than horizontally if that's preferred, as well as adding sub-navigation items in flyout menus.</span></span> <span data-ttu-id="e70f7-210">簡單的導覽功能表，例如 索引標籤區域建立最上層的`<ul>`項目。</span><span class="sxs-lookup"><span data-stu-id="e70f7-210">Simple navigation menus, like tab strips, are built on top of `<ul>` elements.</span></span> <span data-ttu-id="e70f7-211">要建立十分容易，只要提供 CSS 類別 "nav" 和 "nav-tabs" 即可：</span><span class="sxs-lookup"><span data-stu-id="e70f7-211">These can be created very simply by just providing them with the CSS classes "nav" and "nav-tabs":</span></span>

![佈景主題的索引](bootstrap/_static/theme-tabstrips.png)

<span data-ttu-id="e70f7-213">導覽列大致上很類似，但會比較複雜一點。</span><span class="sxs-lookup"><span data-stu-id="e70f7-213">Navbars are built similarly, but are a bit more complex.</span></span> <span data-ttu-id="e70f7-214">他們開始`<nav>`或`<div>`與 「 導覽列 」，在其中容器 div 保留其餘的項目類別。</span><span class="sxs-lookup"><span data-stu-id="e70f7-214">They start with a `<nav>` or `<div>` with a class of "navbar", within which a container div holds the rest of the elements.</span></span> <span data-ttu-id="e70f7-215">我們的網頁瀏覽列標頭中已包含-如下所示只會展開，新增的下拉式功能表中的支援：</span><span class="sxs-lookup"><span data-stu-id="e70f7-215">Our page includes a navbar in its header already – the one shown below simply expands on this, adding support for a dropdown menu:</span></span>

![佈景主題的導覽列](bootstrap/_static/theme-navbars.png)

### <a name="additional-elements"></a><span data-ttu-id="e70f7-217">其他項目</span><span class="sxs-lookup"><span data-stu-id="e70f7-217">Additional elements</span></span>

<span data-ttu-id="e70f7-218">預設佈景主題也可用來正確格式化樣式，包括支援等量的檢視中呈現 HTML 表格。</span><span class="sxs-lookup"><span data-stu-id="e70f7-218">The default theme can also be used to present HTML tables in a nicely formatted style, including support for striped views.</span></span> <span data-ttu-id="e70f7-219">還有具有樣式的標籤，類似於按鈕的樣式。</span><span class="sxs-lookup"><span data-stu-id="e70f7-219">There are labels with styles that are similar to those of the buttons.</span></span> <span data-ttu-id="e70f7-220">您可以建立自訂下拉式功能表，以支援超出標準 HTML`<select>`元素的額外樣式選項，以及類似我們預設入門網站所用的瀏覽列。</span><span class="sxs-lookup"><span data-stu-id="e70f7-220">You can create custom Dropdown menus that support additional styling options beyond the standard HTML `<select>` element, along with Navbars like the one our default starter site is already using.</span></span> <span data-ttu-id="e70f7-221">如果您需要一個進度列時，有數個可供選擇，以及列出群組和納入標題和內容的面板的樣式。</span><span class="sxs-lookup"><span data-stu-id="e70f7-221">If you need a progress bar, there are several styles to choose from, as well as List Groups and panels that include a title and content.</span></span> <span data-ttu-id="e70f7-222">此處可以探索標準 Bootstrap 主題中的其他選項：</span><span class="sxs-lookup"><span data-stu-id="e70f7-222">Explore additional options within the standard Bootstrap Theme here:</span></span>

[http://getbootstrap.com/examples/theme/](http://getbootstrap.com/examples/theme/)

## <a name="more-themes"></a><span data-ttu-id="e70f7-223">更多佈景主題</span><span class="sxs-lookup"><span data-stu-id="e70f7-223">More themes</span></span>

<span data-ttu-id="e70f7-224">如果您可以覆寫部分或所有 Bootstrap 主題的 CSS、調整色彩和樣式以符合應用程式需求，藉此擴充標準 Bootstrap 主題。</span><span class="sxs-lookup"><span data-stu-id="e70f7-224">You can extend the standard Bootstrap theme by overriding some or all of its CSS, adjusting the colors and styles to suit your own application's needs.</span></span> <span data-ttu-id="e70f7-225">您想要從現成的主題開始，網路上有數個針對 Bootstrap 主題的主題庫，例如 WrapBootstrap.com (有各種不同的商業主題) 和 Bootswatch.com (有免費主題)。</span><span class="sxs-lookup"><span data-stu-id="e70f7-225">If you'd like to start from a ready-made theme, there are several theme galleries available online that specialize in Bootstrap themes, such as WrapBootstrap.com (which has a variety of commercial themes) and Bootswatch.com (which offers free themes).</span></span> <span data-ttu-id="e70f7-226">有些付費的範本可提供豐富的圖表與量測計來大量基本的 Bootstrap 佈景主題，例如豐富的支援系統管理功能表和儀表板頂端的功能。</span><span class="sxs-lookup"><span data-stu-id="e70f7-226">Some of the paid templates available provide a great deal of functionality on top of the basic Bootstrap theme, such as rich support for administrative menus, and dashboards with rich charts and gauges.</span></span> <span data-ttu-id="e70f7-227">熱門付費的範例是範本的 Inspinia，下列螢幕擷取畫面所示：</span><span class="sxs-lookup"><span data-stu-id="e70f7-227">An example of a popular paid template is Inspinia, which is shown in the following screenshot:</span></span>

![範例主題 inspinia](bootstrap/_static/theme-inspinia.png)

<span data-ttu-id="e70f7-229">如果您想要變更您的 Bootstrap 主題，請將所要主題的 *bootstrap.css* 檔案放在**wwwroot/css**資料夾中，並變更 *_Layout.cshtml* 中的參考以指向該檔案。 變更所有環境的連結：</span><span class="sxs-lookup"><span data-stu-id="e70f7-229">If you want to change your Bootstrap theme, put the *bootstrap.css* file for the theme you want in the **wwwroot/css** folder and change the references in *_Layout.cshtml* to point it.</span></span> <span data-ttu-id="e70f7-230">變更連結以取得所有環境：</span><span class="sxs-lookup"><span data-stu-id="e70f7-230">Change the links for all environments:</span></span>

```html
<environment names="Development">
    <link rel="stylesheet" href="~/css/bootstrap.css" />
```

```html
<environment names="Staging,Production">
    <link rel="stylesheet" href="~/css/bootstrap.min.css" />
```

<span data-ttu-id="e70f7-231">如果您想要建置您自己的儀表板，您可以從可用的免費範例從這裡開始： [ http://getbootstrap.com/examples/dashboard/ ](http://getbootstrap.com/examples/dashboard/)。</span><span class="sxs-lookup"><span data-stu-id="e70f7-231">If you want to build your own dashboard, you can start from the free example available here: [http://getbootstrap.com/examples/dashboard/](http://getbootstrap.com/examples/dashboard/).</span></span>

## <a name="components"></a><span data-ttu-id="e70f7-232">元件</span><span class="sxs-lookup"><span data-stu-id="e70f7-232">Components</span></span>

<span data-ttu-id="e70f7-233">除了已經討論過這些項目，還會使用啟動程序包含支援各種[內建的 UI 元件](http://getbootstrap.com/components/)。</span><span class="sxs-lookup"><span data-stu-id="e70f7-233">In addition to those elements already discussed, Bootstrap includes support for a variety of [built-in UI components](http://getbootstrap.com/components/).</span></span>

### <a name="glyphicons"></a><span data-ttu-id="e70f7-234">Glyphicons</span><span class="sxs-lookup"><span data-stu-id="e70f7-234">Glyphicons</span></span>

<span data-ttu-id="e70f7-235">Bootstrap 包含一組來自 Glyphicons 的圖示集 ([http://glyphicons.com](http://glyphicons.com))，其中有超過 200 個圖示可以免費使用於您已啟用 Bootstrap 的 Web 應用程式。以下是一個小範例：</span><span class="sxs-lookup"><span data-stu-id="e70f7-235">Bootstrap includes icon sets from Glyphicons ([http://glyphicons.com](http://glyphicons.com)), with over 200 icons freely available for use within your Bootstrap-enabled web application.</span></span> <span data-ttu-id="e70f7-236">以下是只是一小部分的範例：</span><span class="sxs-lookup"><span data-stu-id="e70f7-236">Here's just a small sample:</span></span>

![Glyphicons](bootstrap/_static/theme-glyphicons.png)

### <a name="input-groups"></a><span data-ttu-id="e70f7-238">輸入的群組</span><span class="sxs-lookup"><span data-stu-id="e70f7-238">Input groups</span></span>

<span data-ttu-id="e70f7-239">輸入的群組可讓統合的其他文字或按鈕與輸入的項目，讓使用者更直覺式的體驗：</span><span class="sxs-lookup"><span data-stu-id="e70f7-239">Input groups allow bundling of additional text or buttons with an input element, providing the user with a more intuitive experience:</span></span>

![輸入的群組](bootstrap/_static/input-groups.png)

### <a name="breadcrumbs"></a><span data-ttu-id="e70f7-241">階層連結</span><span class="sxs-lookup"><span data-stu-id="e70f7-241">Breadcrumbs</span></span>

<span data-ttu-id="e70f7-242">階層連結列是常見的 UI 元件，可用來顯示使用者最近的歷程記錄或網站導覽階層的深度。</span><span class="sxs-lookup"><span data-stu-id="e70f7-242">Breadcrumbs are a common UI component used to show a user their recent history or depth within a site's navigation hierarchy.</span></span> <span data-ttu-id="e70f7-243">若要新增階層連結列，只要將 "breadcrumb" 類別套用至任何`<ol>`清單元素即可。</span><span class="sxs-lookup"><span data-stu-id="e70f7-243">Add them easily by applying the "breadcrumb" class to any `<ol>` list element.</span></span> <span data-ttu-id="e70f7-244">若要包含內建的分頁支援，請在`<ul>`中的 `<nav>`元素上使用  "pagination" 類別。</span><span class="sxs-lookup"><span data-stu-id="e70f7-244">Include built-in support for pagination by using the "pagination" class on a `<ul>` element within a `<nav>`.</span></span> <span data-ttu-id="e70f7-245">若要新增回應式內嵌投影片與視訊，請使用 `<iframe>`， `<embed>`， `<video>`，或`<object>`元素，Bootstrap 會自動設定這些元素的樣式。</span><span class="sxs-lookup"><span data-stu-id="e70f7-245">Add responsive embedded slideshows and video by using `<iframe>`, `<embed>`, `<video>`, or `<object>` elements, which Bootstrap will style automatically.</span></span> <span data-ttu-id="e70f7-246">若要指定特定的長寬比，請使用如 "embed-responsive-16by9" 的特定類別。</span><span class="sxs-lookup"><span data-stu-id="e70f7-246">Specify a particular aspect ratio by using specific classes like "embed-responsive-16by9".</span></span>

## <a name="javascript-support"></a><span data-ttu-id="e70f7-247">JavaScript 支援</span><span class="sxs-lookup"><span data-stu-id="e70f7-247">JavaScript support</span></span>

<span data-ttu-id="e70f7-248">Bootstrap 的 JavaScript 程式庫提供了所包含元件的 API 支援，可讓您在應用程式中以程式設計的方式來控制其行為。</span><span class="sxs-lookup"><span data-stu-id="e70f7-248">Bootstrap's JavaScript library includes API support for the included components, allowing you to control their behavior programmatically within your application.</span></span> <span data-ttu-id="e70f7-249">此外， *bootstrap.js*包含十多個自訂 jQuery 外掛程式，提供額外的功能，例如轉換、強制回應對話方塊、捲動偵測 (根據使用者捲動文件的位置來更新樣式)、摺疊行為、浮動切換，以及將功能表固定至視窗，以免被捲動而超出螢幕。</span><span class="sxs-lookup"><span data-stu-id="e70f7-249">In addition, *bootstrap.js* includes over a dozen custom jQuery plugins, providing additional features like transitions, modal dialogs, scroll detection (updating styles based on where the user has scrolled in the document), collapse behavior, carousels, and affixing menus to the window so they don't scroll off the screen.</span></span> <span data-ttu-id="e70f7-250">此處無法詳述 Bootstrap 內建的 JavaScript 附加元件。若要深入了解，請瀏覽[ http://getbootstrap.com/javascript/ ](http://getbootstrap.com/javascript/)。</span><span class="sxs-lookup"><span data-stu-id="e70f7-250">There's not sufficient room to cover all of the JavaScript add-ons built into Bootstrap – to learn more please visit [http://getbootstrap.com/javascript/](http://getbootstrap.com/javascript/).</span></span>

## <a name="summary"></a><span data-ttu-id="e70f7-251">總結</span><span class="sxs-lookup"><span data-stu-id="e70f7-251">Summary</span></span>

<span data-ttu-id="e70f7-252">Bootstrap 提供了一種 Web 架構，可用來快速且有效率地為各種不同的網站和應用程式設定版面配置和樣式。</span><span class="sxs-lookup"><span data-stu-id="e70f7-252">Bootstrap provides a web framework that can be used to quickly and productively lay out and style a wide variety of websites and applications.</span></span> <span data-ttu-id="e70f7-253">Bootstrap 的基本印刷樣式和樣式便能提供不錯的外觀，並可透過自行開發或購買現成的自訂主題來輕鬆地操控。</span><span class="sxs-lookup"><span data-stu-id="e70f7-253">Its basic typography and styles provide a pleasant look and feel that can easily be manipulated through custom theme support, which can be hand-crafted or purchased commercially.</span></span> <span data-ttu-id="e70f7-254">它支援許多以往需要昂貴的協力廠商控制項才能完成的 Web 元件，同時支援現代化和開放的 Web 標準。</span><span class="sxs-lookup"><span data-stu-id="e70f7-254">It supports a host of web components that in the past would've required expensive third-party controls to accomplish, while supporting modern and open web standards.</span></span>
