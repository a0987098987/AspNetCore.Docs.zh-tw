---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
title: "主版頁面和 ASP.NET AJAX (C#) |Microsoft 文件"
author: rick-anderson
description: "討論使用 ASP.NET AJAX 和主版頁面的選項。 查看使用 ScriptManagerProxy 類別; 事件類別討論如何載入各種 JS 檔案 dependi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 0c55eb66-ba44-4d49-98e8-5c87fd9b1111
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
msc.type: authoredcontent
ms.openlocfilehash: 6e09951be5483ed098b8cab6517335f9962a5d95
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="master-pages-and-aspnet-ajax-c"></a>主版頁面和 ASP.NET AJAX (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_CS.zip)或[下載 PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_CS.pdf)

> 討論使用 ASP.NET AJAX 和主版頁面的選項。 查看使用 ScriptManagerProxy 類別; 事件類別討論各種 JS 檔案會載入方式取決於是否使用 ScriptManager 在主頁面或內容頁面。


## <a name="introduction"></a>簡介

過去幾年來，越來越多的開發人員有建置[AJAX](http://en.wikipedia.org/wiki/Ajax_(programming))-啟用 web 應用程式。 啟用 AJAX 的網站使用多個相關的 web 技術，以提供更能有效回應的使用者體驗。 建立啟用 AJAX 的 ASP.NET 應用程式是非常簡單的感謝您至 Microsoft 的[ASP.NET AJAX framework](../../../../ajax/index.md)。 ASP.NET AJAX 內建 ASP.NET 3.5 和 Visual Studio 2008。它也會提供個別下載 ASP.NET 2.0 應用程式。

建置時使用 ASP.NET AJAX framework 啟用 AJAX 的網頁，您必須加入精確一個[ScriptManager 控制項](https://msdn.microsoft.com/library/bb398863.aspx)使用 framework 的每個頁面。 正如其名，ScriptManager 會管理已啟用 AJAX 的網頁中使用的用戶端指令碼。 最少 ScriptManager 會發出會指示瀏覽器下載 JavaScript 檔案，該結構 ASP.NET AJAX 用戶端程式庫的 HTML。 它也可以用來註冊自訂的 JavaScript 檔案、 指令碼啟用 web 服務和自訂應用程式服務功能。

如果您的站台會使用主版頁面 （如同它），您不一定需要 ScriptManager 控制項加入每一個單一的內容頁面。相反地，您可以將 ScriptManager 控制項加入主版頁面。 本教學課程會示範如何將 ScriptManager 控制項加入至主版頁面。 它也會查看如何使用 ScriptManagerProxy 控制項特定的內容頁面中註冊自訂指令碼和指令碼服務。

> [!NOTE]
> 設計或建置 ASP.NET AJAX 架構與啟用 AJAX 的 web 應用程式，就無法瀏覽本教學課程。 如需使用 AJAX 詳細資訊，請參閱[ASP.NET AJAX 視訊](../../../videos/aspnet-ajax/index.md)和[教學課程](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)，以及做為本教學課程結尾處進一步閱讀區段中列出這些資源。


## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>檢查發出 ScriptManager 控制項的標記

ScriptManager 控制項發出該結構 ASP.NET AJAX 用戶端程式庫會指示瀏覽器下載 JavaScript 檔案的標記。 它也會初始化此文件庫的頁面加入的位元的內嵌 JavaScript。 下列標記會顯示加入至網頁，其中包含 ScriptManager 控制項的轉譯的輸出內容：


[!code-html[Main](master-pages-and-asp-net-ajax-cs/samples/sample1.html)]

`<script src="url"></script>`標記指示下載並執行 JavaScript 檔案在瀏覽器*url*。 ScriptManager 會發出這類三個標記。其中一個參考檔`WebResource.axd`，而其他兩個參考檔案`ScriptResource.axd`。 這些檔案實際上不存在您的網站中的檔案。 相反地，當這些檔案的其中一個要求到達網頁伺服器時，ASP.NET 引擎會檢查查詢字串並傳回適當的 JavaScript 內容。 這三個外部的 JavaScript 檔案所提供的指令碼會構成 ASP.NET AJAX 架構的用戶端程式庫。 其他`<script>`ScriptManager 所發出的標記包括內嵌指令碼，以初始化此文件庫。

外部指令碼參考和內嵌指令碼發出 ScriptManager 不可或缺的頁面會使用 ASP.NET AJAX 架構，但不需要使用未使用 framework 的頁面。 因此，您可能原因是適合用來只使用 ASP.NET AJAX framework 這些頁加入 ScriptManager。 這就足夠，但如果您有許多網頁使用 framework 最後將 ScriptManager 控制項加入至所有頁面-重複的工作中，至少。 或者，您可以加入 ScriptManager 主版頁面，然後將必要的指令碼插入至所有的內容頁面。 使用此方法時，您不需要記得至使用 ASP.NET AJAX 架構，因為它已包含在主版頁面的新頁面加入 ScriptManager。 步驟 1 逐步解說主版頁面中加入 ScriptManager。

> [!NOTE]
> 如果您計劃包括主版頁面的使用者介面中的 AJAX 功能，然後在內容中沒有任何選擇-您必須在主版頁面包含 ScriptManager。


ScriptManager 加入主版頁面中的一個缺點是，上述指令碼會在中發出*每*頁面上，不論其所需。 清楚地帶來浪費頻寬有 ScriptManager 包含 （透過主版頁面），但不使用任何 ASP.NET AJAX 架構的功能，這些頁面。 但只要多少頻寬會作廢嗎？

- ScriptManager （如上所示） 所發出的實際內容加總的方式稍有超過 1 KB。
- 所參考的三個外部指令碼檔案`<script>`項目，不過，構成大約需要 450 KB 的資料進行壓縮; 在使用 gzip 壓縮的網站，可以減少這個總頻寬接近 100 KB。 不過，這些指令碼檔案會由瀏覽器快取為一年，也就是說，它們只需要下載一次，則可以重複使用站台上的其他頁面中。

最好的情況下，然後快取的指令碼檔案時的總成本過 1 KB，也就是可以忽略。 在最差的情況下，不過-這當指令碼檔案有尚未下載和 web 伺服器未使用任何形式的壓縮-頻寬叫用是大約需要 450 KB，可將從第二個或兩個透過寬頻連線到最多一分鐘的任何位置新增 透過撥號數據機的使用者。 好消息是，外部指令碼檔案會由瀏覽器快取，因為這個最差的情況不常發生。

> [!NOTE]
> 如果您仍然認為感到不舒服 ScriptManager 控制項置於主版頁面，請考慮 Web 表單 (`<form runat="server">`主版頁面中的標記)。 使用回傳模型中每個 ASP.NET 網頁必須包含一個精確的 Web 表單。 加入 Web Form 加入額外的內容： 一個隱藏的表單欄位的數字`<form>`標記本身，並在必要時，JavaScript 函式初始化回傳從指令碼。 此標記是不必要不回傳的頁面。 無法從主版頁面移除 Web 表單，並手動將它加入至每個需要的內容頁面排除此多餘的標記。 不過，讓 Web Form 中的主版頁面的優點多於不必要地加入一些內容頁面的缺點。


## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>步驟 1： 將 ScriptManager 控制項加入至主版頁面

每個網頁使用 ASP.NET AJAX framework 必須包含一個精確 ScriptManager 控制項。 由於此需求，因此通常才會將單一 ScriptManager 控制項在主版頁面上的，所有的內容頁面需要 ScriptManager 控制項自動包含。 此外，必須出現 ScriptManager，才能進行任何 ASP.NET AJAX 伺服器控制項，例如 UpdatePanel 和 UpdateProgress 控制項。 因此，最好將 ScriptManager Web 表單內任何 ContentPlaceHolder 控制項之前。

開啟`Site.master`主版頁面及前將 ScriptManager 控制項加入至 Web 表單頁面`<div id="topContent">`項目 （請參閱圖 1）。 如果您使用 Visual Web Developer 2008 或 Visual Studio 2008，ScriptManager 控制項位於工具箱的 [AJAX 擴充功能] 索引標籤。如果您使用 Visual Studio 2005，則您必須先安裝 ASP.NET AJAX 架構，並將控制項加入 [工具箱]。 請瀏覽[ASP.NET AJAX Wiki](https://github.com/DevExpress/AjaxControlToolkit/wiki)取得 ASP.NET 2.0 的架構。

在頁面中加入 ScriptManager 之後, 變更其`ID`從`ScriptManager1`至`MyManager`。


[![ScriptManager 加入主版頁面](master-pages-and-asp-net-ajax-cs/_static/image2.png)](master-pages-and-asp-net-ajax-cs/_static/image1.png)

**圖 01**: ScriptManager 加入主版頁面 ([按一下以檢視完整大小的影像](master-pages-and-asp-net-ajax-cs/_static/image3.png))


## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>步驟 2： 使用 ASP.NET AJAX Framework 從內容頁面

與 ScriptManager 控制項加入至主版頁面，我們現在可以新增 ASP.NET AJAX framework 功能任何內容的頁面。 讓我們來建立新的 ASP.NET 頁面會顯示來自 Northwind 資料庫的隨機選取的產品。 我們會使用 ASP.NET AJAX 架構的計時器控制項來更新此顯示中顯示新的產品每隔 15 秒。

藉由名為的根目錄中建立新的頁面開始`ShowRandomProduct.aspx`。 別忘了繫結至這個新頁面`Site.master`主版頁面。


[![將新的 ASP.NET 網頁新增至網站](master-pages-and-asp-net-ajax-cs/_static/image5.png)](master-pages-and-asp-net-ajax-cs/_static/image4.png)

**圖 02**： 將新的 ASP.NET 網頁新增至網站 ([按一下以檢視完整大小的影像](master-pages-and-asp-net-ajax-cs/_static/image6.png))


請記得，在[*主版頁面中指定的標題、 Meta 標記和其他 HTML 標頭*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)教學課程中我們建立名為基礎的自訂頁面類別`BasePage`它產生頁面的標題未明確設定。 移至`ShowRandomProduct.aspx`網頁的程式碼後置類別，並讓它衍生自`BasePage`(而不是從`System.Web.UI.Page`)。

最後，更新`Web.sitemap`檔案至這一課中加入一個項目。 加入下列標記下方`<siteMapNode>`母片內容的頁面互動課：


[!code-xml[Main](master-pages-and-asp-net-ajax-cs/samples/sample2.xml)]

這個加法`<siteMapNode>`項目會反映在課程清單 （請參閱圖 5）。

### <a name="displaying-a-randomly-selected-product"></a>顯示隨機選取的產品

返回`ShowRandomProduct.aspx`。 從設計工具中，拖曳 UpdatePanel 控制項從工具箱拖曳到`MainContent`內容控制項並設定其`ID`屬性`ProductPanel`。 UpdatePanel 表示會以非同步方式更新部分網頁回傳透過在螢幕上的區域。

我們第一項工作是顯示在 UpdatePanel 內隨機選取產品的相關資訊。 開始將 DetailsView 控制項拖曳至 UpdatePanel。 將 DetailsView 控制項的`ID`屬性`ProductInfo`和清除其`Height`和`Width`屬性。 展開 DetailsView 的智慧標籤，然後從 [選擇資料來源] 下拉式清單中選擇繫結至新的 SqlDataSource 控制項，名為的 DetailsView `RandomProductDataSource`。


[![將 DetailsView 繫結至新的 SqlDataSource 控制項](master-pages-and-asp-net-ajax-cs/_static/image8.png)](master-pages-and-asp-net-ajax-cs/_static/image7.png)

**圖 03**： 繫結至新的 SqlDataSource 控制項的 DetailsView ([按一下以檢視完整大小的影像](master-pages-and-asp-net-ajax-cs/_static/image9.png))


設定要透過連接至 Northwind 資料庫的 SqlDataSource 控制項`NorthwindConnectionString`(我們在中建立[*互動主版頁面內容的頁面從*](interacting-with-the-content-page-from-the-master-page-cs.md)教學課程)。 當設定 select 陳述式選擇指定自訂的 SQL 陳述式，然後輸入下列查詢：


[!code-sql[Main](master-pages-and-asp-net-ajax-cs/samples/sample3.sql)]

`TOP 1`關鍵字`SELECT`子句會傳回只由查詢所傳回的第一筆記錄。 [ `NEWID()`函式](https://msdn.microsoft.com/library/ms190348.aspx)會產生新[全域唯一識別碼 (GUID) 的值](http://en.wikipedia.org/wiki/Globally_Unique_Identifier)而且可以用於`ORDER BY`子句，以隨機順序傳回資料表的記錄。


[![設定傳回單一的隨機選取的記錄 SqlDataSource](master-pages-and-asp-net-ajax-cs/_static/image11.png)](master-pages-and-asp-net-ajax-cs/_static/image10.png)

**圖 04**： 設定要傳回的單一、 隨機選取的記錄 SqlDataSource ([按一下以檢視完整大小的影像](master-pages-and-asp-net-ajax-cs/_static/image12.png))


完成精靈之後，Visual Studio 會建立 BoundField 上述查詢所傳回的兩個資料行。 此時網頁的宣告式標記看起來應該如下所示：


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample4.aspx)]

圖 5 顯示`ShowRandomProduct.aspx`頁面上透過瀏覽器檢視時。 按一下您的瀏覽器重新整理 按鈕重新載入該頁面。您應該會看到`ProductName`和`UnitPrice`新的隨機選取資料錄的值。


[![顯示隨機產品的名稱和價格](master-pages-and-asp-net-ajax-cs/_static/image14.png)](master-pages-and-asp-net-ajax-cs/_static/image13.png)

**圖 05**： 顯示隨機產品的名稱和價格 ([按一下以檢視完整大小的影像](master-pages-and-asp-net-ajax-cs/_static/image15.png))


### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>會自動顯示新的產品每 15 秒

ASP.NET AJAX 架構包含在指定的時間; 執行回傳的計時器控制項在回傳計時器的`Tick`就會引發事件。 如果計時器控制項置於 UpdatePanel 觸發部分頁面回傳時，在我們可以重新繫結以顯示新的隨機選取的產品 DetailsView 的資料。

若要完成這項作業，從 [工具箱] 拖曳一個計時器並拖放到 UpdatePanel。 變更計時器的`ID`從`Timer1`至`ProductTimer`及其`Interval`60000 15000 的屬性。 `Interval`屬性指出回傳之間的毫秒數; 將它設定為 15000，會使計時器觸發程序的部分頁面回傳每 15 秒。 此時計時器的宣告式標記看起來應該如下所示：


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample5.aspx)]

建立事件處理常式的計時器的`Tick`事件。 此事件處理常式中，我們需要重新繫結至 DetailsView 藉由呼叫 DetailsView 的`DataBind`方法。 這樣會指示重新擷取其資料來源控制項，資料在 DetailsView 以選取並顯示新的隨機選取記錄 （如同時重新載入該頁面，依序按一下 [瀏覽器的重新整理] 按鈕）。


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample6.cs)]

這就是這麼簡單 ！ 再次瀏覽透過瀏覽器頁面。 一開始，會顯示隨機產品的資訊。 如果您耐心觀看螢幕，您會發現，15 秒之後，新的產品資訊神奇地取代現有的顯示。

若要進一步了解以下情況，我們將標籤控制項加入顯示顯示上次更新的時間的 UpdatePanel。 新增標籤 Web 控制項 UpdatePanel 內、 設定其`ID`至`LastUpdateTime`，並清除其`Text`屬性。 接下來，建立事件處理常式的 UpdatePanel`Load`事件和顯示標籤中的目前時間。 (UpdatePanel`Load`事件會在每次的完整或部分網頁回傳。)


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample7.cs)]

透過這項變更完成，此頁面包含目前所顯示的產品已載入的時間。 圖 6 顯示當第一次瀏覽的頁面。 圖 7 顯示頁面稍後 15 秒之後計時器控制項具有"核"和 UpdatePanel 重新整理以顯示新的產品資訊。


[![載入的頁面顯示隨機選取產品](master-pages-and-asp-net-ajax-cs/_static/image17.png)](master-pages-and-asp-net-ajax-cs/_static/image16.png)

**圖 06**: 隨機選取產品會顯示在頁面載入 ([按一下以檢視完整大小的影像](master-pages-and-asp-net-ajax-cs/_static/image18.png))


[![會顯示新隨機選取產品每 15 秒](master-pages-and-asp-net-ajax-cs/_static/image20.png)](master-pages-and-asp-net-ajax-cs/_static/image19.png)

**圖 07**： 新隨機選取的產品會顯示每隔 15 秒 ([按一下以檢視完整大小的影像](master-pages-and-asp-net-ajax-cs/_static/image21.png))


## <a name="step-3-using-the-scriptmanagerproxy-control"></a>步驟 3： 使用 ScriptManagerProxy 控制項

包含必要的指令碼，適用於 ASP.NET AJAX framework 用戶端程式庫，以及 ScriptManager 也可以註冊自訂的 JavaScript 檔案，啟用指令碼的 Web 服務和自訂驗證、 授權和設定檔服務的參考。 通常這類自訂專屬於特定頁面。 不過，如果自訂指令碼檔案、 Web 服務參考或驗證、 授權或服務設定檔所參考主版頁面 scriptmanager 則它們會包含在*所有*網站中的網頁。

若要加入 ScriptManager 相關以頁面的頁面為基礎的自訂內容使用 ScriptManagerProxy 控制項。 您可以新增 ScriptManagerProxy 內容頁面，並再註冊自訂的 JavaScript 檔案、 Web 服務參考，或驗證、 授權或從 ScriptManagerProxy; 的設定檔服務這有註冊特定的內容頁面上的這些服務的效果。

> [!NOTE]
> ASP.NET 網頁只能存在一個以上的 ScriptManager 控制項。 因此，如果您無法加入 ScriptManager 控制項的內容頁面的主版頁面中已定義 ScriptManager 控制項。 ScriptManagerProxy 的唯一目的是提供開發人員定義 ScriptManager 在主版頁面中，但仍有能力加入 ScriptManager 的自訂內容頁的頁面為基礎的方式。


若要查看 ScriptManagerProxy 控制項中的動作，讓我們來加強在 UpdatePanel`ShowRandomProduct.aspx`包括使用用戶端指令碼來暫停或繼續計時器控制項的按鈕。 Timer 控制項有三種可用來達成這項功能所需的用戶端方法：

- `_startTimer()`-啟動計時器控制項
- `_raiseTick()`-回傳和引發，進而導致 「 刻度 」，計時器控制項及其`Tick`的伺服器上事件
- `_stopTimer()`-停止計時器控制項

讓我們來建立名為的變數的 JavaScript 檔案`timerEnabled`和名為函式`ToggleTimer`。 `timerEnabled`變數會指出是否目前啟用或停用計時器控制項; 它預設為 true。 `ToggleTimer`函式接受兩個輸入參數： 暫停/繼續 按鈕和用戶端的參考`id`計時器控制項的值。 此函式的值會切換`timerEnabled`、 取得計時器控制項的參考、 啟動或停止計時器 (根據的值`timerEnabled`)，並更新 「 暫停 」 或 「 繼續 」 按鈕的顯示文字。 按一下 [暫停/繼續] 按鈕時，會呼叫此函式。

藉由建立新的資料夾中名為 「 網站啟動`Scripts`。 接下來，將新檔案加入至指令碼 資料夾，名為`TimerScript.js`是 JScript File 類型。


[![將新的 JavaScript 檔案加入至指令碼 資料夾](master-pages-and-asp-net-ajax-cs/_static/image23.png)](master-pages-and-asp-net-ajax-cs/_static/image22.png)

**圖 08**： 加入新的 JavaScript 檔案`Scripts`資料夾 ([按一下以檢視完整大小的影像](master-pages-and-asp-net-ajax-cs/_static/image24.png))


[![新的 JavaScript 檔案有尚未加入網站](master-pages-and-asp-net-ajax-cs/_static/image26.png)](master-pages-and-asp-net-ajax-cs/_static/image25.png)

**圖 09**: 新的 JavaScript 檔案已加入至網站 ([按一下以檢視完整大小的影像](master-pages-and-asp-net-ajax-cs/_static/image27.png))


接下來，將下列指令碼加入至 TimerScript.js 檔案：


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample8.cs)]

我們現在需要註冊這個自訂的 JavaScript 檔案在`ShowRandomProduct.aspx`。 返回`ShowRandomProduct.aspx`ScriptManagerProxy 控制項加入頁面之後，設定其`ID`至`MyManagerProxy`。 若要註冊自訂的 JavaScript 檔案 ScriptManagerProxy 控制項設計工具中選取，然後移至 [屬性] 視窗。 其中一個屬性的標題為指令碼。 選取這個屬性會顯示在圖 10 顯示的指令碼參考集合編輯器。 按一下 [新增] 按鈕，以包含新的指令碼參考，然後再輸入指令碼中的檔案路徑屬性的路徑： `~/Scripts/TimerScript.js`。


[![新增 ScriptManagerProxy 控制項的指令碼參考](master-pages-and-asp-net-ajax-cs/_static/image29.png)](master-pages-and-asp-net-ajax-cs/_static/image28.png)

**圖 10**： 新增 ScriptManagerProxy 控制項的指令碼參考 ([按一下以檢視完整大小的影像](master-pages-and-asp-net-ajax-cs/_static/image30.png))


標記之後新增指令碼參考 ScriptManagerProxy 控制項的宣告式會更新為包含`<Scripts>`集合具有單一`ScriptReference`項目，做為標記中的下列程式碼片段說明：


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample9.aspx)]

`ScriptReference`項目會指示要併入其呈現標記中的 JavaScript 檔案的參考 ScriptManagerProxy。 也就是註冊的自訂指令碼中 ScriptManagerProxy`ShowRandomProduct.aspx`頁面的轉譯的輸出，現在包含另一個`<script src="url"></script>`標記： `<script src="Scripts/TimerScript.js" type="text/javascript"></script>`。

現在我們可以呼叫`ToggleTimer`函式定義於`TimerScript.js`中的用戶端指令碼從`ShowRandomProduct.aspx`頁面。 加入下列 HTML UpdatePanel 內：


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample10.aspx)]

這會顯示 「 暫停 」 的文字的按鈕。 只要按一下，JavaScript 函式`ToggleTimer`呼叫時，傳入按鈕和計時器控制項的識別碼值的參考 (`ProductTimer`)。 請注意取得語法`id`計時器控制項的值。 `<%=ProductTimer.ClientID%>`值就會發出`ProductTimer`計時器控制項`ClientID`屬性。 在[*內容頁面中的控制項 ID 命名*](control-id-naming-in-content-pages-cs.md)教學課程中我們將討論伺服器端之間的差異`ID`值和產生的用戶端`id`值，以及如何`ClientID`傳回用戶端`id`。

圖 11 顯示此頁面，當第一次瀏覽透過瀏覽器。 計時器目前正在執行，並更新顯示的產品資訊每隔 15 秒。 已按下 [暫停] 按鈕之後，圖 12 顯示畫面。 按一下 [暫停] 按鈕會停止計時器，並更新 「 繼續 」 按鈕的文字。 產品資訊將會重新整理 （並繼續重新整理每 15 秒） 之後，使用者按下繼續。


[![按一下 [暫停] 按鈕，以停止計時器控制項](master-pages-and-asp-net-ajax-cs/_static/image32.png)](master-pages-and-asp-net-ajax-cs/_static/image31.png)

**圖 11**： 按一下 [暫停] 按鈕，以停止計時器控制項 ([按一下以檢視完整大小的影像](master-pages-and-asp-net-ajax-cs/_static/image33.png))


[![按一下 [繼續] 按鈕，重新啟動計時器](master-pages-and-asp-net-ajax-cs/_static/image35.png)](master-pages-and-asp-net-ajax-cs/_static/image34.png)

**圖 12**： 按一下 [繼續] 按鈕，重新啟動計時器 ([按一下以檢視完整大小的影像](master-pages-and-asp-net-ajax-cs/_static/image36.png))


## <a name="summary"></a>總結

建立啟用 AJAX 的 web 應用程式使用 ASP.NET AJAX framework 時務必每個已啟用 AJAX 的網頁上包含 ScriptManager 控制項。 若要簡化此程序，我們可以加入 ScriptManager 主版頁面，而不需要記得去 ScriptManager 加入每個內容頁面。 示範如何將 ScriptManager 加入主版頁面時查看實作的內容頁面的 AJAX 功能的步驟 2 至步驟 1。

如果您要新增自訂指令碼，指令碼啟用 Web 服務的參考，或自訂的驗證、 授權或與特定的內容頁面的設定檔服務 ScriptManagerProxy 控制項加入 [內容] 頁面，然後設定那里自訂內容。 步驟 3 檢查如何使用 ScriptManagerProxy 特定的內容頁面中註冊自訂的 JavaScript 檔案。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ASP.NET AJAX 架構](../../../../ajax/index.md)
- [ASP.NET AJAX 教學課程](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [ASP.NET AJAX 影片](../../../videos/aspnet-ajax/index.md)
- [Microsoft ASP.NET AJAX 的建置互動式的使用者介面](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [使用 NEWID 隨機排序記錄](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [使用計時器控制項](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多個 ASP/ASP.NET 書籍和 4GuysFromRolla.com 的創辦，目前正在使用 Microsoft Web 技術從 1998 年。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 3.5 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)。 在可到達 Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或透過在他的部落格[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一頁](interacting-with-the-content-page-from-the-master-page-cs.md)
[下一頁](specifying-the-master-page-programmatically-cs.md)
