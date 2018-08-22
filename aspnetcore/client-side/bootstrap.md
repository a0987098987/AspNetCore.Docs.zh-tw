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
# <a name="build-beautiful-responsive-sites-with-bootstrap-and-aspnet-core"></a>建置美觀的回應式網站啟動程序和 ASP.NET Core

<a name="bootstrap-index"></a>

作者：[Steve Smith](https://ardalis.com/)

Bootstrap 是目前用來開發響應式 Web 應用程式的最熱門 Web 架構。 無論您在前端的設計和開發方面是新手或專家，它都提供了數種功能可以協助您改善網站的使用者體驗。 Bootstrap 在部署的時候會是一組 CSS 和 JavaScript 檔案，這些檔案可以協助您讓網站和應用程式更有效率地延伸到手機、平板和桌上型電腦。

## <a name="get-started"></a>開始使用

有幾種方式來開始使用 Bootstrap。 如果您在 Visual Studio 中開始新的 web 應用程式，您可以選擇預設的入門範本適用於 ASP.NET Core，案例的 Bootstrap 將會預先安裝：

![在入門範本方案 檢視啟動程序](bootstrap/_static/bootstrap-in-starter-template.png)

若要將 Bootstrap 加入 ASP.NET Core 專案，只要將它加入*bower.json* 做為相依性即可：

[!code-json[](../common/samples/WebApplication1/bower.json?highlight=5)]

建議採用這種方式來將 Bootstrap 加入 ASP.NET Core 專案。

您也可以使用 Bower、npm 或 NuGet 等套件管理員來安裝 Bootstrap。不管使用何種方式，流程基本上相同： 在每個案例中，處理程序基本上都是一樣：

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
> 在 ASP.NET Core 中安裝如 Bootstrap 這類用戶端相依性的建議作法，是透過 Bower (使用*bower.json*，如上所示)。 使用 npm/NuGet 會顯示可示範如何輕鬆地啟動程序可以加入其他類型的 web 應用程式，包括舊版的 ASP.NET。

如果您正在參考您自己的 Bootstrap 本機版本，您必須在使用它的任何頁面中參考它們。 在生產環境中，您應該使用 CDN 來參考 Bootstrap。 在預設 ASP.NET Core 網站範本中， *_Layout.cshtml*檔案因此像這樣：

[!code-html[](../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=9,13,51,59)]

> [!NOTE]
> 如果您要使用任何 Bootstrap 的 jQuery 外掛程式，您也必須參考 jQuery。


## <a name="basic-templates-and-features"></a>基本的範本和功能

最基本的 Bootstrap 範本看起來很像上列 *_Layout.cshtml*檔案，其中僅包含基本的導覽功能表，以及用於及呈現其餘網頁的空間。

### <a name="basic-navigation"></a>基本導覽

預設範本會使用一組`<div>`要呈現在上方導覽列和頁面的主體。 如果您使用 HTML5，您可以取代第一個`<div>`標記`<nav>`標記來取得相同的效果，但具有更精確的語意。 在此第一個`<div>`您可以看到有幾個其他人。 首先，`<div>`的 [容器]，然後內的兩個以上的類別`<div>`項目: 「 navbar 標頭 」 和 「 導覽列摺疊 」。 導覽列標題 div 包含某些最小寬度，顯示 3 個水平線下方螢幕時，會出現一個按鈕 (所謂 「 漢堡圖示 」)。 使用純 HTML 和 CSS; 呈現圖示需要任何映像不。 這是會顯示圖示，與每個程式碼<span>轉譯其中一個白色橫條的標記：

```html
<button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
</button>
```

它也包含應用程式名稱，就會出現在左上方。 主導覽功能表呈現`<ul>`在第二個的 div 內的項目，並包含以首頁、 關於和連絡人的連結。 下方瀏覽，每個頁面的主體會轉譯在另一個`<div>`、 標示與 「 容器 」 和 「 內容 」 類別。 在預設的簡單\_版面配置檔案處所，頁面上，然後按一下 簡單相關聯的特定檢視所呈現頁面的內容`<footer>`新增至結尾`<div>`項目。 您可以看到關於頁面內建的顯示方式使用此範本：

![關於頁面](bootstrap/_static/about-page-wide.png)

視窗低於特定寬度時，會出現 摺疊導覽列中的，使用右上方，「 漢堡 」 按鈕：

![有關 「 漢堡功能表 」 頁面](bootstrap/_static/about-page-hamburger.png)

按一下圖示即可顯示功能表項目，從頁面頂端會向下滑動垂直下拉式清單中：

![有關 「 擴充的漢堡功能表 」 頁面](bootstrap/_static/about-page-hamburger-open.png)

### <a name="typography-and-links"></a>印刷樣式和連結

啟動程序會設定站台的基本的印刷樣式、 色彩和格式化其 CSS 檔案中的連結。 此 CSS 檔案包含預設樣式，如資料表、 按鈕、 表單項目、 影像和多個 ([了解更多](http://getbootstrap.com/css/))。 接下來將說明一個特別有用的功能，亦即格線版面配置系統。

### <a name="grids"></a>方格

Bootstrap 的最受歡迎的功能之一是其格線版面配置系統。 現代化 Web 應用程式應該避免使用 `<table>`版面配置，而只有針對實際的表格資料來使用此元素。 相反地，使用的一系列`<div>`元素和適當的 CSS 類別便能配置資料行和資料列。 此方法有數個優點，包括可以調整格線的版面配置，以便在手機等窄螢幕上垂直顯示。

[Bootstrap 的格線版面配置系統](http://getbootstrap.com/css/#grid)是以 12 個資料欄為基礎。 會選擇這個數字是因為它可被整除而成為 1、2、3 或 4 資料行，且資料行的寬度可以在螢幕垂直寬度的 1/12 內變化。 若要開始使用格線版面配置系統，您應該開始使用容器 `<div>`然後如下所示新增一個資料列`<div>`：

```html
<div class="container">
    <div class="row">
        ...
    </div>
</div>
```

接下來，新增 額外`<div>`項目，每個資料行，並指定資料行數目，`<div>`開頭 」 資料行-md-」 的 CSS 類別的一部分 （共 12) 應佔據。 比方說，如果您想要只有相同大小的兩個資料行，您就會針對每個使用"資料行-md-6"的類別。 在此情況下"md"會簡稱為 「 中 」，而是指標準大小的桌上電腦的顯示大小。 有四個不同的選項，您可以從中選擇，以及每個將用於較高的寬度除非覆寫 （因此如果您想要不論螢幕寬度修正版面配置，您可以只指定 xs 類別）。

CSS 類別的前置詞 | 裝置層 | 寬度
:---: | :---: | :---:
col-xs- | 手機 | < 768px
資料行-sm- | 平板電腦 | >= 768px
資料行-md- | 桌上型電腦 | >= 992px
資料行-lg- | 較大的桌面顯示 | > = 1200px

當指定兩個資料行都使用 「 資料行-md-6 」 產生的版面配置將會在桌面的解析度的兩個資料行，但這些兩個資料行上較小的裝置 （或較窄的瀏覽器視窗，在桌上型電腦上），可讓使用者輕鬆地檢視轉譯時就會垂直堆疊而不需要以水平捲動的內容。

Bootstrap 總是預設為單一資料行配置，因此只有當您想要多個資料行時，才需要指定資料行。 若要覆寫較大的裝置層行為，才需要明確指定 `<div>`需要所有 12 個資料行。 當指定多個裝置層類別時，可能需要在某些地方重設資料行的呈現。 如下所示，新增只能在特定檢視區中看到的 clearfix div，便可以達到這個目的：

![窄和寬的檢視區方格](bootstrap/_static/narrow-and-wide-viewport-grid.png)

在上述範例中，一和二共用"md"版面配置中的資料列時顯示三個兩個共用"xs"版面配置中的資料列。 沒有 clearfix `<div>`，二和三不會顯示正確"xs"檢視 （請注意，會顯示只有一個、 四和五個）：

![不使用 clearfix 方格](bootstrap/_static/grid-without-clearfix.png)

在此範例中，只使用了單一資料列`<div>`且 Bootstrap 對於版面配置和資料行堆疊大致正確 一般而言，您應該為版面配置所需的每個水平列指定一個資料列`<div>`而且 Bootstrap 格線當然可以使用巢狀架構。 當您這樣做時，每個巢狀的方格會佔用 100%的元件在其中，它會放置，可以再細分使用類別資料行的寬度。

### <a name="jumbotron"></a>Jumbotron

如果您已使用預設 ASP.NET Core MVC 範本，在 Visual Studio 2012 或 2013年中，您可能已經發現 Jumbotron 作用中。 它是指大型的全形區域，可用來顯示大型的背景影像，動作、 輪值表或類似的項目呼叫的頁面。 若要將 jumbotron 加入頁面，只要加入`<div>`並為它提供的類別 「 jumbotron"，然後將容器`<div>`內，並新增您的內容。 我們可以輕鬆地調整標準的 「 關於使用主要的標題，它會顯示 jumbotron 頁：

![jumbotron 範例](bootstrap/_static/jumbotron.png)

### <a name="buttons"></a>按鈕

預設按鈕的類別和其色彩會在下圖中顯示。

![佈景主題的按鈕](bootstrap/_static/theme-buttons.png)

### <a name="badges"></a>徽章

徽章是指瀏覽項目旁邊的小，通常是數值圖說文字。 它們可以表示多個訊息或通知等候或更新的目前狀態。 指定這類徽章很簡單，只要新增`<span>`包含文字，具有 「 徽章 」 的類別：

![佈景主題的徽章](bootstrap/_static/theme-badges.png)

### <a name="alerts"></a>警示

您可能需要對您的應用程式使用者顯示某種類型的通知、 警示或錯誤訊息。 這是標準的警示類別適合的位置。 有四個不同的嚴重性層級與相關聯的色彩配置：

![佈景主題的警示](bootstrap/_static/theme-alerts.png)

### <a name="navbars-and-menus"></a>導覽列和功能表

我們的版面配置已包含標準的導覽列，但 Bootstrap 的佈景主題支援額外的樣式選項。 我們也可以很容易地依偏好來選擇垂直顯示導覽列，而非水平顯示，同時也可以新增子導覽項目至彈出式視窗功能表。 簡單的導覽功能表，例如 索引標籤區域建立最上層的`<ul>`項目。 要建立十分容易，只要提供 CSS 類別 "nav" 和 "nav-tabs" 即可：

![佈景主題的索引](bootstrap/_static/theme-tabstrips.png)

導覽列大致上很類似，但會比較複雜一點。 他們開始`<nav>`或`<div>`與 「 導覽列 」，在其中容器 div 保留其餘的項目類別。 我們的網頁瀏覽列標頭中已包含-如下所示只會展開，新增的下拉式功能表中的支援：

![佈景主題的導覽列](bootstrap/_static/theme-navbars.png)

### <a name="additional-elements"></a>其他項目

預設佈景主題也可用來正確格式化樣式，包括支援等量的檢視中呈現 HTML 表格。 還有具有樣式的標籤，類似於按鈕的樣式。 您可以建立自訂下拉式功能表，以支援超出標準 HTML`<select>`元素的額外樣式選項，以及類似我們預設入門網站所用的瀏覽列。 如果您需要一個進度列時，有數個可供選擇，以及列出群組和納入標題和內容的面板的樣式。 此處可以探索標準 Bootstrap 主題中的其他選項：

[http://getbootstrap.com/examples/theme/](http://getbootstrap.com/examples/theme/)

## <a name="more-themes"></a>更多佈景主題

如果您可以覆寫部分或所有 Bootstrap 主題的 CSS、調整色彩和樣式以符合應用程式需求，藉此擴充標準 Bootstrap 主題。 您想要從現成的主題開始，網路上有數個針對 Bootstrap 主題的主題庫，例如 WrapBootstrap.com (有各種不同的商業主題) 和 Bootswatch.com (有免費主題)。 有些付費的範本可提供豐富的圖表與量測計來大量基本的 Bootstrap 佈景主題，例如豐富的支援系統管理功能表和儀表板頂端的功能。 熱門付費的範例是範本的 Inspinia，下列螢幕擷取畫面所示：

![範例主題 inspinia](bootstrap/_static/theme-inspinia.png)

如果您想要變更您的 Bootstrap 主題，請將所要主題的 *bootstrap.css* 檔案放在**wwwroot/css**資料夾中，並變更 *_Layout.cshtml* 中的參考以指向該檔案。 變更所有環境的連結： 變更連結以取得所有環境：

```html
<environment names="Development">
    <link rel="stylesheet" href="~/css/bootstrap.css" />
```

```html
<environment names="Staging,Production">
    <link rel="stylesheet" href="~/css/bootstrap.min.css" />
```

如果您想要建置您自己的儀表板，您可以從可用的免費範例從這裡開始： [ http://getbootstrap.com/examples/dashboard/ ](http://getbootstrap.com/examples/dashboard/)。

## <a name="components"></a>元件

除了已經討論過這些項目，還會使用啟動程序包含支援各種[內建的 UI 元件](http://getbootstrap.com/components/)。

### <a name="glyphicons"></a>Glyphicons

Bootstrap 包含一組來自 Glyphicons 的圖示集 ([http://glyphicons.com](http://glyphicons.com))，其中有超過 200 個圖示可以免費使用於您已啟用 Bootstrap 的 Web 應用程式。以下是一個小範例： 以下是只是一小部分的範例：

![Glyphicons](bootstrap/_static/theme-glyphicons.png)

### <a name="input-groups"></a>輸入的群組

輸入的群組可讓統合的其他文字或按鈕與輸入的項目，讓使用者更直覺式的體驗：

![輸入的群組](bootstrap/_static/input-groups.png)

### <a name="breadcrumbs"></a>階層連結

階層連結列是常見的 UI 元件，可用來顯示使用者最近的歷程記錄或網站導覽階層的深度。 若要新增階層連結列，只要將 "breadcrumb" 類別套用至任何`<ol>`清單元素即可。 若要包含內建的分頁支援，請在`<ul>`中的 `<nav>`元素上使用  "pagination" 類別。 若要新增回應式內嵌投影片與視訊，請使用 `<iframe>`， `<embed>`， `<video>`，或`<object>`元素，Bootstrap 會自動設定這些元素的樣式。 若要指定特定的長寬比，請使用如 "embed-responsive-16by9" 的特定類別。

## <a name="javascript-support"></a>JavaScript 支援

Bootstrap 的 JavaScript 程式庫提供了所包含元件的 API 支援，可讓您在應用程式中以程式設計的方式來控制其行為。 此外， *bootstrap.js*包含十多個自訂 jQuery 外掛程式，提供額外的功能，例如轉換、強制回應對話方塊、捲動偵測 (根據使用者捲動文件的位置來更新樣式)、摺疊行為、浮動切換，以及將功能表固定至視窗，以免被捲動而超出螢幕。 此處無法詳述 Bootstrap 內建的 JavaScript 附加元件。若要深入了解，請瀏覽[ http://getbootstrap.com/javascript/ ](http://getbootstrap.com/javascript/)。

## <a name="summary"></a>總結

Bootstrap 提供了一種 Web 架構，可用來快速且有效率地為各種不同的網站和應用程式設定版面配置和樣式。 Bootstrap 的基本印刷樣式和樣式便能提供不錯的外觀，並可透過自行開發或購買現成的自訂主題來輕鬆地操控。 它支援許多以往需要昂貴的協力廠商控制項才能完成的 Web 元件，同時支援現代化和開放的 Web 標準。
