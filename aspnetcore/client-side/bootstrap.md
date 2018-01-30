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
ms.openlocfilehash: dfed5c7a8e103973048295b7607008ecc7e90eeb
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="building-beautiful-responsive-sites-with-bootstrap"></a>建置美麗、 可回應網站與啟動程序

<a name="bootstrap-index"></a>

由[Steve Smith](https://ardalis.com/)

最熱門的 web 架構開發能繼續回應的 web 應用程式的目前啟動載入器。 無論您是在前端的設計和開發或專家的新手，它會提供數種功能可以改善您的網站，您的使用者經驗的優點。 啟動安裝程式會部署為一組 CSS 和 JavaScript 檔案，設計用來協助您網站或應用程式的標尺有效率地從手機到桌上型電腦的平板電腦。

## <a name="getting-started"></a>使用者入門

有幾種方式來開始使用啟動程序。 如果您在 Visual Studio 中開始新的 web 應用程式，您可以選擇預設的入門範本適用於 ASP.NET Core，案例的啟動程序將會預先安裝：

![啟動載入入門範本方案檢視中](bootstrap/_static/bootstrap-in-starter-template.png)

啟動程序加入 ASP.NET Core 專案是簡單的將它加入至*bower.json*做為相依性：

[!code-json[Main](../common/samples/WebApplication1/bower.json?highlight=5)]

這是建議用來啟動程序加入 ASP.NET Core 專案。

您也可以安裝使用其中一種數個封裝管理員，例如 Bower、 npm 或 NuGet 的啟動程序。 在每個案例中，流程是基本上相同：

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
> 建議用來安裝用戶端相依性，例如透過 Bower 為啟動程序中 ASP.NET Core (使用*bower.json*，如上所示)。 Npm/NuGet 使用會顯示以示範如何輕鬆啟動程序可以加入其他種類的 web 應用程式，包括較早版本的 ASP.NET。

如果您正在參考您自己的啟動程序的本機版本，您必須在使用它的任何頁面中參考它們。 在生產環境中，您應該參考使用 CDN 的啟動程序。 在預設的 ASP.NET 網站範本， *_Layout.cshtml*檔案並因此如下所示：

[!code-html[Main](../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=9,13,51,59)]

> [!NOTE]
> 如果您要使用任何啟動程序的 jQuery 外掛程式，您也必須參考 jQuery。

## <a name="basic-templates-and-features"></a>基本的範本和功能

最基本的啟動程序範本看起來很像*_Layout.cshtml*檔案顯示更新的版本，並直接包含基本的功能表，用於導覽及呈現網頁的其餘的地方。

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

啟動程序設定站台的基本的印刷樣式、 色彩和格式化其 CSS 檔案中的連結。 這個 CSS 檔案中包含的資料表、 按鈕、 表單項目、 影像和多個預設樣式 ([進一步了解](http://getbootstrap.com/css/))。 一個特別有用的功能是格線版面配置系統，接下來涵蓋。

### <a name="grids"></a>格線

啟動程序的最受歡迎的功能之一是其格線版面配置系統。 現代化 web 應用程式應該避免使用`<table>`配置，請改為使用這個項目限制實際的表格式資料的標記。 相反地，資料行和資料列可配置使用的一系列`<div>`項目和適當的 CSS 類別。 有幾個這種方法，包括可調整的版面配置的方格顯示垂直上窄的畫面，例如手機上的好處。

[啟動程序的格線版面配置系統](http://getbootstrap.com/css/#grid)12 個資料欄為基礎。 這個數字已選擇，因為它可分為平均 1、 2、 3 或 4 的資料行和資料行的寬度可能會以 1 內/12 的垂直螢幕的寬度。 若要開始使用格線版面配置系統，您應該開始使用容器`<div>`然後再加入一個資料列`<div>`，如下所示：

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

永遠都會預設啟動程序至單一資料行配置，因此您只需要指定資料行，當您想要多個資料行。 您會想要明確指定的唯一時間`<div>`需要所有的 12 個資料行，就是覆寫較大的裝置層的行為。 當指定多個裝置層類別，您可能需要重設資料行的呈現特定時間點。 將只會顯示某些區內 clearfix div 可以達到這個目的，如下所示：

![窄和寬的檢視區方格](bootstrap/_static/narrow-and-wide-viewport-grid.png)

在上述範例中，一和二共用"md"版面配置中的資料列兩到三次共用"xs"版面配置中的資料列。 沒有 clearfix `<div>`，兩個和第三不會顯示正確"xs"檢視 （請注意，會顯示只有一個、 四和五個）：

![不使用 clearfix 方格](bootstrap/_static/grid-without-clearfix.png)

在此範例中，單一資料列`<div>`被使用，並且啟動程序仍然大部分未正確配置和資料行的堆疊方面的事。 一般而言，您應該指定一個資料列`<div>`每個水平資料列需要您的配置，以及當然您可以巢狀啟動程序內另一個方格。 當您這樣做時，每個巢狀的方格會佔用 100%的元件，它用來放置，可以再細分使用類別資料行的寬度。

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

我們的版面配置已包含標準的導覽列中，但啟動程序的佈景主題皆支援其他樣式選項。 我們也很容易可以選擇垂直顯示導覽列，而非水平如果有慣用的也會為新增的副導覽中的項目彈出式視窗功能表。 最上層的內建索引標籤帶狀線，例如簡單的瀏覽功能表 <ul> 項目。 可以建立非常簡單，只讓他們能夠以 CSS 類別 」 nav 」 和 「 瀏覽索引標籤 」:

![佈景主題 tabstrips](bootstrap/_static/theme-tabstrips.png)

Navbars 同樣地，內建，但會稍微更複雜。 開頭為`<nav>`或`<div>`"導覽列 」，在其中容器 div 保存的項目其餘部分的類別。 我們的頁面導覽列在其標頭中已包含 – 如下所示只會展開，新增下拉式功能表的支援：

![佈景主題 navbars](bootstrap/_static/theme-navbars.png)

### <a name="additional-elements"></a>其他項目

預設佈景主題也可用來呈現 HTML 表格格式樣式，包括支援等量的檢視中。 標籤一共有使用類似於按鈕的樣式。 您可以建立自訂支援標準 HTML 以外的其他樣式選項的下拉式功能表`<select>`項目，以及 Navbars 類似我們預設入門網站已在使用中。 如果您需要將進度列，有數種樣式，以供選擇，以及列出群組和包含標題與內容的面板。 瀏覽標準啟動程序主題這裡內的其他選項：

[http://getbootstrap.com/examples/theme/](http://getbootstrap.com/examples/theme/)

## <a name="more-themes"></a>更多主題

您可以擴充標準啟動程序的主題，藉由覆寫部分或所有其 CSS 調整色彩和樣式，以符合您自己的應用程式的需求。 如果您想要從現成的佈景主題開始，有數個主題組件庫提供線上，在啟動程序的佈景主題，例如 WrapBootstrap.com （這有各種不同的商業佈景主題） 和 Bootswatch.com （其提供可用的佈景主題） 中的特製化。 某些可用的付費範本提供許多基本的啟動程序佈景主題，例如豐富的支援系統管理功能表和儀表板頂端的功能豐富的圖表和量測計。 熱門付費範本的範例是 Inspinia，目前 $18，其中包含除了 AngularJS 和靜態的 HTML 版本的 ASP.NET MVC5 範本的銷售。 範例螢幕擷取畫面所示。

![範例主題 inspinia](bootstrap/_static/theme-inspinia.png)

如果您想要變更您的啟動程序主題，請將放*bootstrap.css*中您要的佈景主題檔案**wwwroot/css**資料夾，並變更中的參考*_Layout.cshtml*指向它。 變更用於所有環境的連結：

```html
<environment names="Development">
    <link rel="stylesheet" href="~/css/bootstrap.css" />
```

```html
<environment names="Staging,Production">
    <link rel="stylesheet" href="~/css/bootstrap.min.css" />
```

如果您想要建置您自己的儀表板，您可以從可用的範例可從這裡開始： [http://getbootstrap.com/examples/dashboard/](http://getbootstrap.com/examples/dashboard/)。

## <a name="components"></a>元件

除了已經討論過這些項目，啟動安裝程式包含支援各種[內建的 UI 元件](http://getbootstrap.com/components/)。

### <a name="glyphicons"></a>Glyphicons

啟動程序包括從 Glyphicons 圖示集 ([http://glyphicons.com](http://glyphicons.com))，有超過 200 圖示免費內啟動安裝程式啟用 web 應用程式使用。 以下是只是小型的範例：

![Glyphicons](bootstrap/_static/theme-glyphicons.png)

### <a name="input-groups"></a>輸入的群組

輸入的群組允許結合在一起的其他文字或按鈕與輸入項目，讓使用者更直覺的經驗：

![輸入的群組](bootstrap/_static/input-groups.png)

### <a name="breadcrumbs"></a>階層連結列

階層連結列會用來顯示使用者，其最新歷程記錄或網站的導覽階層內的深度的通用 UI 元件。 透過將"階層連結 」 類別套用至任何輕鬆地新增`<ol>`清單項目。 包含內建支援分頁上使用 「 重新編頁 」 類別`<ul>`內的項目`<nav>`。 將能繼續回應的內嵌投影片及視訊新增使用`<iframe>`， `<embed>`， `<video>`，或`<object>`項目，則啟動程序會自動設定樣式。 使用特定的類別，例如 「 內嵌-回應-16by9"來指定特定的長寬比。

## <a name="javascript-support"></a>JavaScript 支援

啟動安裝程式的 JavaScript 程式庫包含共用的元件，可讓您控制它們的行為，以程式設計方式在應用程式中的應用程式開發介面支援。 此外， *bootstrap.js*包含鋮自訂 jQuery 外掛程式，提供額外的功能，例如轉換之後，強制回應對話方塊中，捲動偵測 （更新樣式根據使用者已捲動文件中的位置），摺疊行為，可提領轉盤，並加至視窗，因此它們不捲動超出螢幕的功能表。 沒有足夠空間可以涵蓋所有的啟動程序 – 若要了解更多，請瀏覽內建 JavaScript 附加[http://getbootstrap.com/javascript/](http://getbootstrap.com/javascript/)。

## <a name="summary"></a>總結

啟動程序會提供一種 web 架構，可用來快速且有效率地配置和樣式各種不同的網站和應用程式。 其基本的印刷樣式和樣式提供愉快的外觀及操作，可輕鬆地透過自訂的佈景主題支援，可以對手工或購買商業上操作。 它支援的 web 元件在過去就已經要求昂貴的協力廠商控制項，來完成，同時支援現代化和開啟 web 標準的主機。
