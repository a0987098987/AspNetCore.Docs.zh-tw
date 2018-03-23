---
title: 建立啟動程序與 ASP.NET Core 美觀、 可回應站台
author: ardalis
description: 了解如何開發 ASP.NET Core 與回應的 web 應用程式時，用於啟動程序。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bootstrap
ms.openlocfilehash: c3dfaa53e9e3277d025d014f65004e4c24a5acc4
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
---
# <a name="building-beautiful-responsive-sites-with-bootstrap-and-aspnet-core"></a>建立啟動程序與 ASP.NET Core 美觀、 可回應站台

<a name="bootstrap-index"></a>

作者：[Steve Smith](https://ardalis.com/)

Bootstrap 是目前用來開發響應式 Web 應用程式的最熱門 Web 架構。 無論您在前端的設計和開發方面是新手或專家，它都提供了數種功能可以協助您改善網站的使用者體驗。 Bootstrap 在部署的時候會是一組 CSS 和 JavaScript 檔案，這些檔案可以協助您讓網站和應用程式更有效率地延伸到手機、平板和桌上型電腦。

## <a name="get-started"></a>開始使用

有幾種方式來開始使用 Bootstrap。 如果您在 Visual Studio 中開始新的 web 應用程式，您可以選擇預設的入門範本適用於 ASP.NET Core，案例的 Bootstrap 將會預先安裝：

![啟動載入入門範本方案檢視中](bootstrap/_static/bootstrap-in-starter-template.png)

若要將 Bootstrap 加入 ASP.NET Core 專案，只要將它加入*bower.json* 做為相依性即可：

[!code-json[](../common/samples/WebApplication1/bower.json?highlight=5)]

建議採用這種方式來將 Bootstrap 加入 ASP.NET Core 專案。

您也可以使用 Bower、npm 或 NuGet 等套件管理員來安裝 Bootstrap。不管使用何種方式，流程基本上相同： 在每個案例中，流程是基本上相同：

### <a name="bower"></a>Bower

```console
bower install bootstrap
```

### <a name="npm"></a>npm

```console
npm install bootstrap
```

### <a name="nuget"></a>NuGet

```console
Install-Package bootstrap
```

> [!NOTE]
> 在 ASP.NET Core 中安裝如 Bootstrap 這類用戶端相依性的建議作法，是透過 Bower (使用*bower.json*，如上所示)。 Npm/NuGet 使用會顯示以示範如何輕鬆啟動程序可以加入其他種類的 web 應用程式，包括較早版本的 ASP.NET。

如果您正在參考您自己的 Bootstrap 本機版本，您必須在使用它的任何頁面中參考它們。 在生產環境中，您應該使用 CDN 來參考 Bootstrap。 在預設的 ASP.NET 網站範本， *_Layout.cshtml*檔案便是採用這種做法：

[!code-html[](../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=9,13,51,59)]

> [!NOTE]
> 如果您要使用任何 Bootstrap 的 jQuery 外掛程式，您也必須參考 jQuery。


## <a name="basic-templates-and-features"></a>基本的範本和功能

最基本的 Bootstrap 範本看起來很像上列*_Layout.cshtml*檔案，其中僅包含基本的導覽功能表，以及用於及呈現其餘網頁的空間。

### <a name="basic-navigation"></a>基本導覽

預設範本會使用一組`<div>`要呈現在上方導覽列和頁面的主體。 如果您使用 HTML5，您可以取代第一個`<div>`標記`<nav>`標記來取得相同的效果，但有更精確的語意。 在此第一個`<div>`您可以看到還有其他幾個。 首先， `<div>` "container"，然後在中，兩個類別`<div>`項目: 「 瀏覽列標頭 」 和 「 導覽列摺疊 」。 導覽列標頭 div 包含一個按鈕，會出現如下的某些最小寬度，顯示 3 水平線螢幕時 (所謂 「 漢堡圖示 」)。 使用純 HTML 和 CSS; 呈現圖示需要沒有映像。 這是會顯示圖示，與每個程式碼<span>標記呈現白色橫條的其中一個：

```html
<button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
</button>
```

它也包含應用程式名稱，它會出現在左上方。 主瀏覽功能表呈現`<ul>`內第二個 div，項目，並包含以首頁、 關於和連絡人的連結。 註冊和登入的其他連結會新增一行 29 _LoginPartial 列。 下方瀏覽，在另一個轉譯的每個頁面主體`<div>`、 標示為 「 容器 」 和 「 本文內容 」 類別。 簡單的預設 _Layout 檔案如下所示，在頁面的內容會轉譯頁面上，然後按一下 簡單與相關聯的特定檢視`<footer>`加入至結尾`<div>`項目。 您可以看到有關頁面內建的顯示方式使用此範本：

![有關頁面](bootstrap/_static/about-page-wide.png)

視窗低於特定寬度時，會出現 摺疊導覽列中的，使用右上方，「 漢堡 」 按鈕：

![關於漢堡 」 功能表與頁面](bootstrap/_static/about-page-hamburger.png)

按一下圖示會顯示在頁面頂端會向下滑動垂直 drawer 的功能表項目：

![展開漢堡功能表與頁面相關](bootstrap/_static/about-page-hamburger-open.png)

### <a name="typography-and-links"></a>印刷樣式和連結

Bootstrap 在其 CSS 檔案中設定了站台的基本印刷樣式、色彩和連結格式。 這個 CSS 檔案中包含了表格、按鈕、表單元素、影像等等的預設樣式 ([進一步了解](http://getbootstrap.com/css/))。 接下來將說明一個特別有用的功能，亦即格線版面配置系統。

### <a name="grids"></a>格線

Bootstrap 的最受歡迎的功能之一是其格線版面配置系統。 現代化 Web 應用程式應該避免使用 `<table>`版面配置，而只有針對實際的表格資料來使用此元素。 相反地，使用的一系列`<div>`元素和適當的 CSS 類別便能配置資料行和資料列。 此方法有數個優點，包括可以調整格線的版面配置，以便在手機等窄螢幕上垂直顯示。

[Bootstrap 的格線版面配置系統](http://getbootstrap.com/css/#grid)是以 12 個資料欄為基礎。 會選擇這個數字是因為它可被整除而成為 1、2、3 或 4 資料行，且資料行的寬度可以在螢幕垂直寬度的 1/12 內變化。 若要開始使用格線版面配置系統，您應該開始使用容器 `<div>`然後如下所示新增一個資料列`<div>`：

```html
<div class="container">
    <div class="row">
        ...
    </div>
</div>
```

接下來，加入 其他`<div>`項目，每個資料行，並指定資料行數目，`<div>`應佔據 （超出 12) 當做開頭為"col-md-"CSS 類別的一部分。 比方說，如果您想要只需要相同大小的兩個資料行，您會針對每個使用"col-md-6"的類別。 在此情況下"md"是簡稱為 「 中 」，而且參考到標準大小桌面的電腦的顯示大小。 有四個不同的選項，您可以選擇，而每個將用於較高的寬度除非覆寫 （因此如果您想要修正不論螢幕寬度的配置，您可以只需要指定 xs 類別）。

CSS 類別的前置詞 | 裝置層 | 寬度
:---: | :---: | :---:
col-xs- | 電話 | < 768px
col-sm- | 平板電腦 | >= 768px
col-md- | 桌上型電腦 | >= 992px
col-lg- | 較大的桌面顯示 | >= 1200px

當指定的兩個資料行都與 「 資料行-md-6 」 產生的配置將會在桌面的解析度上，兩個資料行，但這兩個資料行上較小裝置 （或較窄的瀏覽器視窗，在桌面上），讓使用者能夠輕鬆地檢視轉譯時就會垂直堆疊而不需要水平捲動內容。

Bootstrap 總是預設為單一資料行配置，因此只有當您想要多個資料行時，才需要指定資料行。 若要覆寫較大的裝置層行為，才需要明確指定 `<div>`需要所有 12 個資料行。 當指定多個裝置層類別時，可能需要在某些地方重設資料行的呈現。 如下所示，新增只能在特定檢視區中看到的 clearfix div，便可以達到這個目的：

![窄和寬的檢視區方格](bootstrap/_static/narrow-and-wide-viewport-grid.png)

在上述範例中，一和二共用"md"版面配置中的資料列兩到三次共用"xs"版面配置中的資料列。 沒有 clearfix `<div>`，兩個和第三不會顯示正確"xs"檢視 （請注意，會顯示只有一個、 四和五個）：

![不使用 clearfix 方格](bootstrap/_static/grid-without-clearfix.png)

在此範例中，只使用了單一資料列`<div>`且 Bootstrap 對於版面配置和資料行堆疊大致正確 一般而言，您應該為版面配置所需的每個水平列指定一個資料列`<div>`而且 Bootstrap 格線當然可以使用巢狀架構。 當您這樣做時，每個巢狀格線會佔用所在元素的 100% 寬度，其中可以再使用資料行類別加以分區。

### <a name="jumbotron"></a>Jumbotron

如果您已使用預設 ASP.NET MVC 範本在 Visual Studio 2012 或 2013年中，您可能已經看到 Jumbotron 作用中。 它是指大型全形頁面章節包含可以用來顯示大型的背景影像，呼叫動作、 旋轉或類似的項目。 若要將 jumbotron 加入頁面中，只要加入`<div>`並為它提供一種"jumbotron"，然後放置在容器`<div>`內並加入您的內容。 我們可以輕易地調整標準的 「 關於使用主要的標題，它會顯示 jumbotron 頁：

![jumbotron 範例](bootstrap/_static/jumbotron.png)

### <a name="buttons"></a>按鈕

預設按鈕的類別和其色彩會在下圖中顯示。

![佈景主題的按鈕](bootstrap/_static/theme-buttons.png)

### <a name="badges"></a>徽章

徽章是指瀏覽項目旁邊的小，通常是數值圖說文字。 這可能是多個訊息或通知等待或更新的存在。 指定這類徽章很簡單，只新增<span>包含文字，有 [徽章] 的類別：

![佈景主題的徽章](bootstrap/_static/theme-badges.png)

### <a name="alerts"></a>警示

您可能需要對應用程式的使用者顯示通知、 警示或錯誤訊息的某種。 這是其中的標準警示類別會很有用。 有四個不同的嚴重性層級與相關聯的色彩配置：

![佈景主題的警示](bootstrap/_static/theme-alerts.png)

### <a name="navbars-and-menus"></a>Navbars 和功能表

我們的版面配置已包含標準的導覽列，但 Bootstrap 的佈景主題支援額外的樣式選項。 我們也可以很容易地依偏好來選擇垂直顯示導覽列，而非水平顯示，同時也可以新增子導覽項目至彈出式視窗功能表。 索引標籤區域之類的簡單瀏覽功能表是建立在 <ul> 項目。 要建立十分容易，只要提供 CSS 類別 "nav" 和 "nav-tabs" 即可：

![佈景主題 tabstrips](bootstrap/_static/theme-tabstrips.png)

Navbars 同樣地，內建，但會稍微更複雜。 開頭為`<nav>`或`<div>`"導覽列 」，在其中容器 div 保存的項目其餘部分的類別。 我們的頁面導覽列在其標頭中已包含 – 如下所示只會展開，新增下拉式功能表的支援：

![佈景主題 navbars](bootstrap/_static/theme-navbars.png)

### <a name="additional-elements"></a>其他項目

預設佈景主題也可使用優美的格式來呈現 HTML 表格，包括條紋狀檢視。 還有具有樣式的標籤，類似於按鈕的樣式。 您可以建立自訂下拉式功能表，以支援超出標準 HTML`<select>`元素的額外樣式選項，以及類似我們預設入門網站所用的瀏覽列。 如果您需要將進度列，也有數種樣式可供選擇，以及包含標題與內容的清單群組及面板。 此處可以探索標準 Bootstrap 主題中的其他選項：

[http://getbootstrap.com/examples/theme/](http://getbootstrap.com/examples/theme/)

## <a name="more-themes"></a>更多主題

如果您可以覆寫部分或所有 Bootstrap 主題的 CSS、調整色彩和樣式以符合應用程式需求，藉此擴充標準 Bootstrap 主題。 您想要從現成的主題開始，網路上有數個針對 Bootstrap 主題的主題庫，例如 WrapBootstrap.com (有各種不同的商業主題) 和 Bootswatch.com (有免費主題)。 某些可用的付費範本提供許多基本的啟動程序佈景主題，例如豐富的支援系統管理功能表和儀表板頂端的功能豐富的圖表和量測計。 熱門付費範本之一便是 Inspinia，目前訂價 $18，其中包含 ASP.NET MVC5 範本，再加上 AngularJS 和靜態 HTML 版本。 範例螢幕擷取畫面所示。

![範例主題 inspinia](bootstrap/_static/theme-inspinia.png)

如果您想要變更您的 Bootstrap 主題，請將所要主題的 *bootstrap.css* 檔案放在**wwwroot/css**資料夾中，並變更*_Layout.cshtml* 中的參考以指向該檔案。 變更所有環境的連結： 變更用於所有環境的連結：

```html
<environment names="Development">
    <link rel="stylesheet" href="~/css/bootstrap.css" />
```

```html
<environment names="Staging,Production">
    <link rel="stylesheet" href="~/css/bootstrap.min.css" />
```

如果您想要建置您自己的儀表板，您可以從可用的範例可從這裡開始： [ http://getbootstrap.com/examples/dashboard/ ](http://getbootstrap.com/examples/dashboard/)。

## <a name="components"></a>元件

除了已經討論過這些項目，啟動安裝程式包含支援各種[內建的 UI 元件](http://getbootstrap.com/components/)。

### <a name="glyphicons"></a>Glyphicons

Bootstrap 包含一組來自 Glyphicons 的圖示集 ([http://glyphicons.com](http://glyphicons.com))，其中有超過 200 個圖示可以免費使用於您已啟用 Bootstrap 的 Web 應用程式。以下是一個小範例： 以下是只是小型的範例：

![Glyphicons](bootstrap/_static/theme-glyphicons.png)

### <a name="input-groups"></a>輸入的群組

輸入的群組允許結合在一起的其他文字或按鈕與輸入項目，讓使用者更直覺的經驗：

![輸入的群組](bootstrap/_static/input-groups.png)

### <a name="breadcrumbs"></a>階層連結列

階層連結列是常見的 UI 元件，可用來顯示使用者最近的歷程記錄或網站導覽階層的深度。 若要新增階層連結列，只要將 "breadcrumb" 類別套用至任何`<ol>`清單元素即可。 若要包含內建的分頁支援，請在`<ul>`中的 `<nav>`元素上使用  "pagination" 類別。 若要新增回應式內嵌投影片與視訊，請使用 `<iframe>`， `<embed>`， `<video>`，或`<object>`元素，Bootstrap 會自動設定這些元素的樣式。 若要指定特定的長寬比，請使用如 "embed-responsive-16by9" 的特定類別。

## <a name="javascript-support"></a>JavaScript 支援

Bootstrap 的 JavaScript 程式庫提供了所包含元件的 API 支援，可讓您在應用程式中以程式設計的方式來控制其行為。 此外， *bootstrap.js*包含十多個自訂 jQuery 外掛程式，提供額外的功能，例如轉換、強制回應對話方塊、捲動偵測 (根據使用者捲動文件的位置來更新樣式)、摺疊行為、浮動切換，以及將功能表固定至視窗，以免被捲動而超出螢幕。 此處無法詳述 Bootstrap 內建的 JavaScript 附加元件。若要深入了解，請瀏覽[ http://getbootstrap.com/javascript/ ](http://getbootstrap.com/javascript/)。

## <a name="summary"></a>總結

Bootstrap 提供了一種 Web 架構，可用來快速且有效率地為各種不同的網站和應用程式設定版面配置和樣式。 Bootstrap 的基本印刷樣式和樣式便能提供不錯的外觀，並可透過自行開發或購買現成的自訂主題來輕鬆地操控。 它支援許多以往需要昂貴的協力廠商控制項才能完成的 Web 元件，同時支援現代化和開放的 Web 標準。
