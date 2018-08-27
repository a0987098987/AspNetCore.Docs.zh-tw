---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
title: 主版頁面和 ASP.NET AJAX (C#) |Microsoft Docs
author: rick-anderson
description: 討論使用 ASP.NET AJAX 和主版頁面的選項。 探討使用 ScriptManagerProxy 類別;討論各種 JS 檔案會列印文件的載入，請 dependi...
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 0c55eb66-ba44-4d49-98e8-5c87fd9b1111
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
msc.type: authoredcontent
ms.openlocfilehash: 47201a0cfeb5d1e548721094d11488e9e804dc9c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826611"
---
<a name="master-pages-and-aspnet-ajax-c"></a>主版頁面和 ASP.NET AJAX (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_CS.zip)或[下載 PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_CS.pdf)

> 討論使用 ASP.NET AJAX 和主版頁面的選項。 探討使用 ScriptManagerProxy 類別;討論各種 JS 檔案會載入方式取決於是否使用 ScriptManager 在主頁面或內容頁面。


## <a name="introduction"></a>簡介

過去幾年來，越來越多的開發人員一直在建立[AJAX](http://en.wikipedia.org/wiki/Ajax_(programming))-啟用 web 應用程式。 啟用 AJAX 的網站使用多個相關的 web 技術，以提供回應速度更快的使用者體驗。 建立啟用 AJAX 的 ASP.NET 應用程式是 Microsoft 的感謝出乎意料的簡單[ASP.NET AJAX 架構](../../../../ajax/index.md)。 ASP.NET AJAX 是內建 ASP.NET 3.5 和 Visual Studio 2008;它也會提供個別下載 ASP.NET 2.0 應用程式的。

建置時使用 ASP.NET AJAX 架構啟用 AJAX 的網頁，您必須加入精確地說是一個[ScriptManager 控制項](https://msdn.microsoft.com/library/bb398863.aspx)使用 framework 的每個頁面。 正如其名，ScriptManager 會管理用戶端指令碼會在啟用 AJAX 的網頁。 最小值，ScriptManager 會發出 HTML，它會指示瀏覽器以該結構為 ASP.NET AJAX 用戶端程式庫下載 JavaScript 檔案。 它也可以用來註冊自訂的 JavaScript 檔案、 指令碼啟用網頁服務及自訂應用程式服務功能。

如果您的網站會使用主版頁面 （正常），您不一定需要將 ScriptManager 控制項新增至每個單一的內容頁面;相反地，您可以將 ScriptManager 控制項加入主版頁面。 本教學課程會示範如何將 ScriptManager 控制項新增至主版頁面。 它也會探討如何使用 ScriptManagerProxy 控制項特定的內容頁面中註冊自訂的指令碼和指令碼服務。

> [!NOTE]
> 設計或建置具有 ASP.NET AJAX 架構的啟用 AJAX 的 web 應用程式，就無法瀏覽本教學課程。 如需使用 AJAX 的詳細資訊，請參閱[ASP.NET AJAX 影片](../../../videos/aspnet-ajax/index.md)並[教學課程](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)，以及為本教學課程結尾處進一步閱讀 > 一節中列出這些資源。


## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>檢查送出 ScriptManager 控制項的標記

ScriptManager 控制項就會發出該結構為 ASP.NET AJAX 用戶端程式庫會指示瀏覽器來下載 JavaScript 檔案的標記。 也會增加一堆內嵌 JavaScript 初始化此程式庫的頁面。 下列標記會顯示新增至包含 ScriptManager 控制項的頁面呈現的輸出內容：


[!code-html[Main](master-pages-and-asp-net-ajax-cs/samples/sample1.html)]

`<script src="url"></script>`標記會指示瀏覽器下載並執行的 JavaScript 檔案*url*。 ScriptManager 會發出三個這類標記;其中一個參考的檔案`WebResource.axd`，而其他兩個參考檔案`ScriptResource.axd`。 這些檔案實際上不存在您的網站中的檔案。 相反地，當這些檔案的其中一個要求抵達 web 伺服器時，ASP.NET 引擎會檢查查詢字串，並傳回適當的 JavaScript 內容。 這三個的外部 JavaScript 檔案所提供的指令碼會構成 ASP.NET AJAX 架構的用戶端程式庫。 其他`<script>`ScriptManager 所發出的標記包括內嵌指令碼會初始化此程式庫。

外部指令碼參考和 ScriptManager 所發出的內嵌指令碼是針對使用 ASP.NET AJAX 架構，但不是使用 framework 的頁面不需要的頁面，基本屬性。 因此，您可能原因，最好只將 ScriptManager 加入至使用 ASP.NET AJAX 架構的頁面。 這已足夠，但如果您有許多網頁使用的架構，您就可以得到將 ScriptManager 控制項新增至所有頁面-重複性的工作中，這麼說。 或者，您可以將 ScriptManager 加入主版頁面，然後插入所有的內容頁面中的這個必要的指令碼。 使用此方法時，您不需要記得要使用 ASP.NET AJAX 架構，因為它已包含的主版頁面的新頁面加入 ScriptManager。 步驟 1 逐步 ScriptManager 加入主版頁面。

> [!NOTE]
> 如果您計劃，包括您的主版頁面的使用者介面中的 AJAX 功能，然後在問題中沒有任何選擇-您必須包含 ScriptManager 主版頁面中。


ScriptManager 加入主版頁面中的一項缺點是，上述指令碼中發出*每個*頁面上，無論其所需。 這顯然會造成浪費頻寬有 ScriptManager 包含 （透過主版頁面中），但不使用任何功能的 ASP.NET AJAX 架構的頁面。 但浪費頻寬的多少？

- ScriptManager （如上所示） 所發出的實際內容總計過 1 KB。
- 所參考的三個外部指令碼檔`<script>`項目，不過，包含大約 450 KB 的未壓縮的資料; 在使用 gzip 壓縮網站中，可以降低此總頻寬近乎 100 KB。 不過，這些指令碼檔案會由瀏覽器快取為一年，這表示它們只需要下載一次，則可以重複使用在網站上的其他頁面。

最好的情況下，然後，指令碼檔案會快取時，總成本是 1 KB 以下，這是微不足道。 在最糟的情況下，不過-這當指令碼檔案有尚未下載和 web 伺服器未使用任何形式的壓縮，-頻寬叫用為大約 450 KB，可以從第二個或兩個透過寬頻連線到最多一分鐘的任何位置加入 透過撥號數據機的使用者。 好消息是，因為外部指令碼檔案會由瀏覽器快取，此最糟糕的情況不常發生。

> [!NOTE]
> 如果您仍然認為只要不將 ScriptManager 控制項放在主版頁面項目，請考慮 Web Form (`<form runat="server">`主版頁面中的標記)。 每個 ASP.NET 網頁會回傳模型必須包含一個 Web 表單。 加入 Web Form 加入其他內容： 一個隱藏的表單欄位的數字`<form>`標記本身，以及必要時，JavaScript 函式的初始化指令碼從回傳。 此標記是不必要的未回傳的網頁。 移除主版頁面的 Web 表單，然後手動將它加入至其中一個需要每個內容頁面可以刪除此多餘的標記。 不過，Web Form 主版頁面中的效益會多於缺點的不必要地加入特定內容的頁面。


## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>步驟 1： 將 ScriptManager 控制項新增至主版頁面

使用 ASP.NET AJAX 架構的每個網頁必須包含一個 ScriptManager 控制項。 由於此需求，它通常可以合理地放置單一的 ScriptManager 控制項，在主版頁面，如此所有的內容頁面已自動包含 ScriptManager 控制項。 此外，ScriptManager 必須出現在任何 ASP.NET AJAX 伺服器控制項，例如 UpdatePanel 和 UpdateProgress 控制項之前。 因此，最好是將 ScriptManager 在 Web 表單內任何 ContentPlaceHolder 控制項之前。

開啟`Site.master`主版頁面，並將 ScriptManager 控制項新增至 Web 表單中的頁面之前`<div id="topContent">`項目 （請參閱 圖 1）。 如果您使用 Visual Web Developer 2008 或 Visual Studio 2008，ScriptManager 控制項位於工具箱的 [AJAX 延伸模組] 索引標籤。如果您使用 Visual Studio 2005，則您必須先安裝 ASP.NET AJAX 架構，並將控制項新增至工具箱。 請瀏覽[ASP.NET AJAX Wiki](https://github.com/DevExpress/AjaxControlToolkit/wiki) framework 取得 ASP.NET 2.0。

ScriptManager 加入網頁中之後, 變更其`ID`從`ScriptManager1`至`MyManager`。


[![ScriptManager 加入主版頁面](master-pages-and-asp-net-ajax-cs/_static/image2.png)](master-pages-and-asp-net-ajax-cs/_static/image1.png)

**圖 01**： 將 ScriptManager 加入至主版頁面 ([按一下以檢視完整大小的影像](master-pages-and-asp-net-ajax-cs/_static/image3.png))


## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>步驟 2： 使用 ASP.NET AJAX 架構，從內容頁面

以 ScriptManager 控制項加入至主版頁面中，我們現在可以任何內容的頁面，以新增 ASP.NET AJAX 架構功能。 讓我們建立新的 ASP.NET 頁面顯示的隨機選取的產品，從 Northwind 資料庫。 我們將使用 ASP.NET AJAX 架構的計時器控制項來更新此顯示每隔 15 秒，顯示新的產品。

藉由名為的根目錄中建立新的頁面開始`ShowRandomProduct.aspx`。 別忘了要繫結至這個新頁面`Site.master`主版頁面。


[![將新的 ASP.NET 網頁新增至網站](master-pages-and-asp-net-ajax-cs/_static/image5.png)](master-pages-and-asp-net-ajax-cs/_static/image4.png)

**圖 02**： 將新的 ASP.NET 網頁新增至網站 ([按一下以檢視完整大小的影像](master-pages-and-asp-net-ajax-cs/_static/image6.png))


請注意，在[*指定主版頁面的標題、 中繼標籤及其他 HTML 標頭*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)教學課程中我們建立名為自訂的基底頁面類別`BasePage`如果它已產生頁面的標題未明確設定。 移至`ShowRandomProduct.aspx`頁面的程式碼後置類別，並讓它衍生自`BasePage`(而不是從`System.Web.UI.Page`)。

最後，更新`Web.sitemap`檔案，以包含這一課中的項目。 加入下列標記下方`<siteMapNode>`主要服務內容的頁面互動課程：


[!code-xml[Main](master-pages-and-asp-net-ajax-cs/samples/sample2.xml)]

這個加法`<siteMapNode>`項目會反映在課程清單 （請參閱 [圖 5]）。

### <a name="displaying-a-randomly-selected-product"></a>顯示隨機選取的產品

返回`ShowRandomProduct.aspx`。 從設計工具中，拖曳 UpdatePanel 控制項從工具箱拖曳到`MainContent`內容控制項，並將其`ID`屬性設`ProductPanel`。 UpdatePanel 代表您可以透過部分頁面回傳以非同步方式更新畫面上的區域。

第一個工作是要顯示在 UpdatePanel 內的隨機選取產品的相關資訊。 開始將 DetailsView 控制項拖曳至 UpdatePanel。 將 DetailsView 控制項的`ID`屬性，以`ProductInfo`並清除其`Height`和`Width`屬性。 展開 DetailsView 的智慧標籤，然後從 選擇資料來源 下拉式清單中，選擇 繫結至新的 SqlDataSource 控制項，名為的 DetailsView `RandomProductDataSource`。


[![將 DetailsView 繫結至新的 SqlDataSource 控制項](master-pages-and-asp-net-ajax-cs/_static/image8.png)](master-pages-and-asp-net-ajax-cs/_static/image7.png)

**圖 03**： 將 DetailsView 繫結至新的 SqlDataSource 控制項 ([按一下以檢視完整大小的影像](master-pages-and-asp-net-ajax-cs/_static/image9.png))


設定 SqlDataSource 控制項連接至 Northwind 資料庫，透過`NorthwindConnectionString`(我們在中建立[*與主版頁面，從內容頁互動*](interacting-with-the-content-page-from-the-master-page-cs.md)教學課程)。 當設定 select 陳述式選擇指定自訂的 SQL 陳述式，然後再輸入下列查詢：


[!code-sql[Main](master-pages-and-asp-net-ajax-cs/samples/sample3.sql)]

`TOP 1`中的關鍵字`SELECT`子句會傳回只查詢所傳回的第一筆記錄。 [ `NEWID()`函式](https://msdn.microsoft.com/library/ms190348.aspx)產生新[全域唯一識別碼 (GUID) 的值](http://en.wikipedia.org/wiki/Globally_Unique_Identifier)而且可以用於`ORDER BY`子句，以隨機順序傳回資料表的記錄。


[![設定 SqlDataSource 傳回單一的隨機選取的記錄](master-pages-and-asp-net-ajax-cs/_static/image11.png)](master-pages-and-asp-net-ajax-cs/_static/image10.png)

**圖 04**： 設定要傳回一個隨機選取的記錄 SqlDataSource ([按一下以檢視完整大小的影像](master-pages-and-asp-net-ajax-cs/_static/image12.png))


完成精靈之後，Visual Studio 會建立兩個以上的查詢所傳回的資料行的 BoundField。 此時您頁面的宣告式標記看起來應該如下所示：


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample4.aspx)]

[圖 5] 顯示`ShowRandomProduct.aspx`頁面上透過瀏覽器檢視時。 按一下 瀏覽器的 重新整理 按鈕重新載入頁面;您應該會看到`ProductName`和`UnitPrice`新的隨機選取資料錄的值。


[![顯示隨機產品的名稱和價格](master-pages-and-asp-net-ajax-cs/_static/image14.png)](master-pages-and-asp-net-ajax-cs/_static/image13.png)

**圖 05**: 隨機產品的名稱和價格顯示 ([按一下以檢視完整大小的影像](master-pages-and-asp-net-ajax-cs/_static/image15.png))


### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>會自動顯示新的產品每隔 15 秒

ASP.NET AJAX 架構包含在指定的時間; 會執行回傳的計時器控制項在回傳計時器的`Tick`就會引發事件。 如果是在 UpdatePanel 放 Timer 控制項就會觸發部分頁面回傳，此時我們可以重新繫結至 DetailsView 來顯示新的隨機選取的產品的資料。

若要這麼做，計時器從 [工具箱] 拖放到 UpdatePanel。 變更的計時器`ID`從`Timer1`要`ProductTimer`並將其`Interval`60000 15000 的屬性。 `Interval`屬性會指出回傳之間的毫秒數; 將它設定為 15000，會導致計時器觸發程序的部分頁面回傳每隔 15 秒。 此時計時器的宣告式標記看起來應該如下所示：


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample5.aspx)]

建立事件處理常式，讓計時器的`Tick`事件。 這個事件處理常式中，我們需要重新繫結至 DetailsView 藉由呼叫 DetailsView`DataBind`方法。 這樣會指示重新擷取的資料，從其資料來源控制項 DetailsView 以選取並顯示新的隨機選取 （正如時重新載入頁面，按一下 瀏覽器的 重新整理 按鈕） 的記錄。


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample6.cs)]

這樣就完成了 ！ 再次瀏覽透過瀏覽器頁面。 一開始，會顯示隨機產品的資訊。 如果您耐心監看畫面，您會發現，15 秒之後，新產品的相關資訊神奇地取代現有的顯示。

若要進一步了解了解，讓我們加入一個 Label 控制項顯示的時間，顯示上次更新的 UpdatePanel。 新增在 UpdatePanel Label Web 控制項、 設定其`ID`要`LastUpdateTime`，並清除其`Text`屬性。 接下來，建立事件處理常式的 UpdatePanel`Load`事件與顯示在標籤中的目前時間。 (在 UpdatePanel`Load`在每個完整或部分頁面回傳時引發事件。)


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample7.cs)]

有了這個完成的變更，此頁面包含目前所顯示的產品已載入的時間。 [圖 6] 顯示頁面上，當第一次瀏覽。 圖 7 顯示頁面 15 秒之後 Timer 控制項的動作有 「 勾選 」 後 UpdatePanel 重新整理以顯示新的產品相關資訊。


[![隨機選取產品會顯示在頁面載入](master-pages-and-asp-net-ajax-cs/_static/image17.png)](master-pages-and-asp-net-ajax-cs/_static/image16.png)

**圖 06**: 隨機選取產品會顯示在頁面載入 ([按一下以檢視完整大小的影像](master-pages-and-asp-net-ajax-cs/_static/image18.png))


[![會顯示新隨機選取的產品，每隔 15 秒](master-pages-and-asp-net-ajax-cs/_static/image20.png)](master-pages-and-asp-net-ajax-cs/_static/image19.png)

**圖 07**： 每 15 秒會顯示新隨機選取的產品 ([按一下以檢視完整大小的影像](master-pages-and-asp-net-ajax-cs/_static/image21.png))


## <a name="step-3-using-the-scriptmanagerproxy-control"></a>步驟 3： 使用 ScriptManagerProxy 控制項

以及包括 ASP.NET AJAX 架構用戶端程式庫的必要指令碼，ScriptManager 也可以註冊自訂的 JavaScript 檔案，啟用指令碼的 Web 服務，以及自訂驗證、 授權和設定檔服務的參考。 通常這類自訂專屬於特定頁面。 不過，如果自訂指令碼檔案、 Web 服務參考或驗證、 授權或服務設定檔會參考在 ScriptManager 中的主版頁面中，則它們會包含在*所有*網站中的頁面。

若要加入 ScriptManager 相關頁面的頁面為基礎的自訂內容，請使用 ScriptManagerProxy 控制項。 您可以將 ScriptManagerProxy 新增至內容的頁面並再註冊自訂的 JavaScript 檔案、 Web 服務參考，或驗證、 授權或從 ScriptManagerProxy; 的設定檔服務這有註冊這些服務特定的內容頁面的效果。

> [!NOTE]
> ASP.NET 網頁中只能存在一個以上的 ScriptManager 控制項。 因此，您無法將 ScriptManager 控制項新增至內容頁面，如果 ScriptManager 控制項已定義主版頁面中。 ScriptManagerProxy 的唯一目的是提供開發人員可以在主版頁面中，定義 ScriptManager，但仍舊能夠加入 ScriptManager 的自訂內容頁的頁面為基礎的方式。


若要查看作用中的 ScriptManagerProxy 控制項，讓我們擴大在 UpdatePanel`ShowRandomProduct.aspx`包含暫停或繼續 Timer 控制項使用用戶端指令碼的按鈕。 Timer 控制項有三種可用來達成這項功能所需的用戶端方法：

- `_startTimer()` -啟動計時器控制項
- `_raiseTick()` -藉此使 「 刻度 」 計時器控制項回傳並引發其`Tick`伺服器上的事件
- `_stopTimer()` -停止計時器控制項

我們使用名為的變數來建立的 JavaScript 檔案`timerEnabled`和名為函式`ToggleTimer`。 `timerEnabled`變數會指出是否目前已啟用或停用計時器控制項; 它會預設為 true。 `ToggleTimer`函式會接受兩個輸入參數： 暫停/繼續按鈕和用戶端的參考`id`計時器控制項的值。 此函式會切換的值`timerEnabled`，則取得計時器控制項的參考、 啟動或停止計時器 (根據的值`timerEnabled`)，並更新 「 暫停 」 或 「 繼續 」 按鈕的顯示文字。 按一下 [暫停/繼續] 按鈕時，就會呼叫此函式。

開始建立新的資料夾中名為網站`Scripts`。 接下來，將新檔案新增至名為 [指令碼] 資料夾`TimerScript.js`是 JScript File 類型。


[![將新的 JavaScript 檔案加入至指令碼 資料夾](master-pages-and-asp-net-ajax-cs/_static/image23.png)](master-pages-and-asp-net-ajax-cs/_static/image22.png)

**圖 08**： 加入新的 JavaScript 檔案，以便`Scripts`資料夾 ([按一下以檢視完整大小的影像](master-pages-and-asp-net-ajax-cs/_static/image24.png))


[![新的 JavaScript 檔案新增網站](master-pages-and-asp-net-ajax-cs/_static/image26.png)](master-pages-and-asp-net-ajax-cs/_static/image25.png)

**圖 09**: 新的 JavaScript 檔案已新增至網站 ([按一下以檢視完整大小的影像](master-pages-and-asp-net-ajax-cs/_static/image27.png))


接下來，新增下列指令碼至 TimerScript.js 檔案：


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample8.cs)]

我們現在要註冊此自訂的 JavaScript 檔案中`ShowRandomProduct.aspx`。 返回`ShowRandomProduct.aspx`和 ScriptManagerProxy 控制項加入頁面，設定其`ID`至`MyManagerProxy`。 若要註冊自訂的 JavaScript 檔案會選取 ScriptManagerProxy 控制項設計工具中，然後移至 [屬性] 視窗。 其中一個屬性的標題為指令碼。 選取此屬性會顯示 [圖 10] 所示的指令碼參考集合編輯器。 按一下 [新增] 按鈕，以包含新的指令碼參考，然後再輸入 [路徑] 屬性中的指令碼檔案的路徑： `~/Scripts/TimerScript.js`。


[![新增 ScriptManagerProxy 控制項的指令碼參考](master-pages-and-asp-net-ajax-cs/_static/image29.png)](master-pages-and-asp-net-ajax-cs/_static/image28.png)

**圖 10**： 新增 ScriptManagerProxy 控制項的指令碼參考 ([按一下以檢視完整大小的影像](master-pages-and-asp-net-ajax-cs/_static/image30.png))


新增指令碼參考 ScriptManagerProxy 控制項的宣告式後的標記會更新以包含`<Scripts>`具有單一集合`ScriptReference`項目，做為標記中的下列程式碼片段說明：


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample9.aspx)]

`ScriptReference`項目會指示 ScriptManagerProxy 納入其呈現的標記中的 JavaScript 檔案的參考。 也就是註冊的自訂指令碼中 ScriptManagerProxy`ShowRandomProduct.aspx`頁面的轉譯的輸出現在包含另一個`<script src="url"></script>`標記： `<script src="Scripts/TimerScript.js" type="text/javascript"></script>`。

我們現在可以呼叫`ToggleTimer`中所定義的函式`TimerScript.js`中的用戶端指令碼從`ShowRandomProduct.aspx`頁面。 新增在 UpdatePanel 下列 HTML:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample10.aspx)]

這會顯示具有 「 暫停 」 的文字的按鈕。 只要按一下，JavaScript 函式`ToggleTimer`呼叫時，傳入的按鈕和 Timer 控制項的識別碼值的參考 (`ProductTimer`)。 請注意語法以取得`id`計時器控制項的值。 `<%=ProductTimer.ClientID%>` 值就會發出`ProductTimer`Timer 控制項`ClientID`屬性。 在  [*內容頁中的控制項識別碼命名*](control-id-naming-in-content-pages-cs.md)教學課程中我們所討論的伺服器端之間的差異`ID`值和產生的用戶端`id`值，以及如何`ClientID`會傳回用戶端`id`。

[圖 11] 顯示當第一次透過瀏覽器中瀏覽此頁面。 計時器目前正在執行，並更新顯示的產品資訊每隔 15 秒。 已按下 暫停 按鈕之後，圖 12 顯示的畫面。 按一下 [暫停] 按鈕時，會停止計時器，並更新 「 繼續 」 按鈕的文字。 產品資訊將會重新整理 （並繼續重新整理每隔 15 秒） 後使用者按下繼續。


[![按一下 [暫停] 按鈕來停止計時器控制項](master-pages-and-asp-net-ajax-cs/_static/image32.png)](master-pages-and-asp-net-ajax-cs/_static/image31.png)

**圖 11**： 按一下 停止計時器控制項的 [暫停] 按鈕 ([按一下以檢視完整大小的影像](master-pages-and-asp-net-ajax-cs/_static/image33.png))


[![按一下 [繼續] 按鈕，以重新啟動計時器](master-pages-and-asp-net-ajax-cs/_static/image35.png)](master-pages-and-asp-net-ajax-cs/_static/image34.png)

**圖 12**： 按一下 重新啟動計時器的 [繼續] 按鈕 ([按一下以檢視完整大小的影像](master-pages-and-asp-net-ajax-cs/_static/image36.png))


## <a name="summary"></a>總結

建置使用 ASP.NET AJAX 架構的啟用 AJAX 的 web 應用程式時務必每個啟用 AJAX 的網頁上包含 ScriptManager 控制項。 若要加速此程序，我們可以加入 ScriptManager 主版頁面，而不需要記得去 ScriptManager 加入每個內容頁面。 示範如何將 ScriptManager 加入主版頁面時查看在內容頁面中實作 AJAX 功能的步驟 2 的步驟 1。

如果您要新增自訂指令碼，指令碼啟用 Web 服務的參考，或自訂的驗證、 授權或特定的內容頁面上，設定檔服務 ScriptManagerProxy 控制項加入 [內容] 頁面並再設定那里自訂。 步驟 3 檢查如何使用 ScriptManagerProxy 特定的內容頁面中註冊自訂的 JavaScript 檔案。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ASP.NET AJAX 架構](../../../../ajax/index.md)
- [ASP.NET AJAX 教學課程](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [ASP.NET AJAX 影片](../../../videos/aspnet-ajax/index.md)
- [使用 Microsoft ASP.NET AJAX 建置互動式使用者介面](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [使用 NEWID 隨機排序記錄](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [使用 Timer 控制項](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多個 ASP 本書籍，他是 4GuysFromRolla.com 的創辦人，一直從事 Microsoft Web 技術自 1998 年。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 3.5 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)。 Scott 要聯絡[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在 [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](interacting-with-the-content-page-from-the-master-page-cs.md)
> [下一頁](specifying-the-master-page-programmatically-cs.md)
