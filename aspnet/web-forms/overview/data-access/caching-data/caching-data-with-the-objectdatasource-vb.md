---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-vb
title: 快取資料與 ObjectDataSource (VB) |Microsoft Docs
author: rick-anderson
description: 快取，可能表示緩慢和快速的 Web 應用程式之間的差異。 本教學課程會詳細看看在 ASP.NET 中快取的四個中的第一個...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 2e56a733-5512-48a6-9276-70a65bbe4d5d
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: baa6fd0c290c0b09cf137f12ce62f50bae52be23
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834679"
---
<a name="caching-data-with-the-objectdatasource-vb"></a>快取資料與 ObjectDataSource (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_VB.exe)或[下載 PDF](caching-data-with-the-objectdatasource-vb/_static/datatutorial58vb1.pdf)

> 快取，可能表示緩慢和快速的 Web 應用程式之間的差異。 本教學課程會詳細看看在 ASP.NET 中快取的四個中的第一個。 了解快取的重要概念以及如何將 ObjectDataSource 控制項透過展示層快取。


## <a name="introduction"></a>簡介

在電腦科學中，*快取*是取得資料或資訊會耗費大量資源取得，然後將其複本儲存在更快速地存取的位置的程序。 對於資料驅動的應用程式，大型且複雜的查詢通常會耗用大部分的應用程式的執行時間。 通常可以改善這類應用程式效能，然後藉由將耗費資源的資料庫查詢的結果儲存在應用程式的記憶體中。

ASP.NET 2.0 提供了各種不同的快取選項。 透過整個網頁或使用者控制項呈現的 s 標記快取可以*輸出快取*。 ObjectDataSource 和 SqlDataSource 控制項提供快取功能，藉此讓資料快取控制層級。 和 ASP.NET s*資料快取*提供豐富的快取 API，可讓網頁程式開發人員以程式設計方式快取物件。 在本教學課程，我們將檢驗使用 ObjectDataSource s 的下列三個快取功能，以及資料快取。 我們也會探討如何快取在啟動整個應用程式的資料以及如何將透過使用 SQL 快取相依性的全新快取的資料。 這些教學課程不會探索輸出快取。 深入了解輸出快取，請參閱 < [ASP.NET 2.0 中的輸出快取](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)。

快取可以套用在任何位置，在架構中，從資料存取層設定透過展示層。 在本教學課程中，我們將探討套用 ObjectDataSource 控制項透過展示層快取。 在下一個教學課程中我們將檢驗在快取資料的商務邏輯層。

## <a name="key-caching-concepts"></a>金鑰快取概念

快取可大幅提升應用程式 s 的整體效能和延展性會耗費大量資源來產生資料並將其複本儲存在可以更有效率地存取的位置。 因為快取會儲存只是實際的基礎資料的副本，它也可能會過期，或*過時*，如果基礎資料變更。 為了克服這點，為網頁開發人員可以指出快取項目將會用的準則*收回*從快取中，使用：

- **以時間為基礎的準則**絕對或滑動持續時間的快取可能會加入項目。 例如，網頁開發人員可能表示說 60 秒的期間。 使用絕對的持續時間，快取項目的收回 60 秒之後加入至快取，不論存取頻率。 滑動的持續時間，快取的項目會收回在上次存取後的 60 秒。
- **相依性為基礎的準則**相依性可新增至快取時，項目相關聯。 項目 s 相依性變更時收回快取中。 相依性可能是檔案、 另一個快取項目或兩者的組合。 ASP.NET 2.0 也可讓 SQL 快取相依性，可讓開發人員新增至快取的項目，並將它收回基礎資料庫資料變更時。 我們會檢查即將上市的 SQL 快取相依性[使用 SQL 快取相依性](using-sql-cache-dependencies-vb.md)教學課程。

快取中的項目可能會收回準則所指定的情況下，不論*清除*之前已符合以時間為基礎或相依性為基礎的準則。 如果快取已達其容量，就必須移除現有的項目才能加入新的。 因此，以程式設計方式使用快取資料時它 s 重要，您一律假設快取的資料可能不存在。 我們將探討在下一個教學課程中，以程式設計方式存取資料從快取時所要使用的模式*架構中的快取資料*。

快取提供經濟實惠的方法並從應用程式的更多效能。 作為[Steven Smith](http://aspadvice.com/blogs/ssmith/)指出在他的文章[ASP.NET 快取： 技巧和最佳作法](https://msdn.microsoft.com/library/aa478965.aspx):

快取可以取得良好足夠效能而不需要很長的時間和分析的好方法。 記憶體不耗費資源，因此如果您可以取得您需要的快取的輸出，30 秒，而不需要花費一天或週，嘗試最佳化您的程式碼或資料庫的效能，請執行快取的解決方案 （假設為第二個舊-30 的資料是 [確定]） 並繼續。 最後，不良的設計將可能趕上進度，因此當然，您應該試著正確地設計您的應用程式。 但如果您只需要取得良好的不足，無法在目前的效能，快取可能是絕佳 [方法]，購買重構您的應用程式，在日後當您有時間去做，您的時間。

雖然快取，可以提供明顯的效能增強功能，它並不適用於所有情況下，例如，使用即時、 經常更新的資料，或無法接受更短時間內存在的過時資料的應用程式使用。 但是，對於大部分的應用程式，快取應該使用。 如需更多有關 ASP.NET 2.0 中的快取的詳細背景，請參閱[快取的效能](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx)一節[ASP.NET 2.0 快速入門教學課程](https://quickstarts.asp.net/QuickStartv20/aspnet/)。

## <a name="step-1-creating-the-caching-web-pages"></a>步驟 1： 建立快取的網頁

我們開始探索的 ObjectDataSource s 快取功能之前，讓先花點時間，我們需要針對本教學課程和下列三個我們網站專案中建立 ASP.NET 網頁的 s。 藉由新增新的資料夾，名為啟動`Caching`。 接下來，新增到該資料夾，並確認其關聯與每個頁面的 下列 ASP.NET 網頁`Site.master`主版頁面：

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`


![加入 ASP.NET 網頁的快取相關的教學課程](caching-data-with-the-objectdatasource-vb/_static/image1.png)

**圖 1**： 加入 ASP.NET 網頁的快取相關的教學課程


在其他資料夾，例如`Default.aspx`在`Caching`資料夾會列出其一節中的教學課程。 請記得，`SectionLevelTutorialListing.ascx`使用者控制項提供這項功能。 因此，新增此使用者控制項`Default.aspx`從拖曳到頁面的設計 檢視中的 方案總管 中拖曳。


[![圖 2： 將 SectionLevelTutorialListing.ascx 使用者控制項新增至 Default.aspx](caching-data-with-the-objectdatasource-vb/_static/image3.png)](caching-data-with-the-objectdatasource-vb/_static/image2.png)

**圖 2**: 圖 2： 新增`SectionLevelTutorialListing.ascx`使用者控制項`Default.aspx`([按一下以檢視完整大小的影像](caching-data-with-the-objectdatasource-vb/_static/image4.png))


最後，將這些頁面新增項目為`Web.sitemap`檔案。 具體來說，使用二進位資料之後新增下列標記`<siteMapNode>`:


[!code-xml[Main](caching-data-with-the-objectdatasource-vb/samples/sample1.xml)]

在更新之後`Web.sitemap`，花點時間檢視教學課程網站，透過瀏覽器。 在左側功能表現在包含快取的教學課程中的項目。


![網站導覽現在包含快取的教學課程的項目](caching-data-with-the-objectdatasource-vb/_static/image5.png)

**圖 3**： 網站地圖現在包含快取的教學課程的項目


## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>步驟 2： 在網頁上顯示產品的清單

本教學課程會探討如何使用 ObjectDataSource 控制項 s 內建快取功能。 我們來看看這些功能之前，不過，我們首先需要能夠從頁面。 可讓 s 建立使用 GridView，以擷取從 ObjectDataSource 列出產品資訊網頁`ProductsBLL`類別。

首先開啟`ObjectDataSource.aspx`頁面中`Caching`資料夾。 從 工具箱 拖曳至設計工具拖曳的 GridView，設定其`ID`屬性，以`Products`，並從它的智慧標籤，選擇 繫結至新的 ObjectDataSource 控制項，名為`ProductsDataSource`。 設定使用 ObjectDataSource`ProductsBLL`類別。


[![設定使用 ProductsBLL 類別 ObjectDataSource](caching-data-with-the-objectdatasource-vb/_static/image7.png)](caching-data-with-the-objectdatasource-vb/_static/image6.png)

**圖 4**： 設定要使用 ObjectDataSource`ProductsBLL`類別 ([按一下以檢視完整大小的影像](caching-data-with-the-objectdatasource-vb/_static/image8.png))


此頁面上，讓建立可編輯的 GridView，以便我們可以檢查 ObjectDataSource 中快取資料修改透過 GridView 的介面時，會發生什麼事。 將下拉式清單中選取索引標籤上，並為其預設值，設定`GetProducts()`，變更至 [更新] 索引標籤中選取的項目，但`UpdateProduct`多載，接受`productName`， `unitPrice`，和`productID`做為輸入參數。


[![更新索引標籤的下拉式清單設適當 UpdateProduct 多載](caching-data-with-the-objectdatasource-vb/_static/image10.png)](caching-data-with-the-objectdatasource-vb/_static/image9.png)

**[圖 5**： 更新] 索引標籤的下拉式清單設合適`UpdateProduct`多載 ([按一下以檢視完整大小的影像](caching-data-with-the-objectdatasource-vb/_static/image11.png))


最後，設定為 （無） 插入和刪除索引標籤中的下拉式清單，然後按一下 完成。 完成後設定資料來源精靈，Visual Studio 設定 ObjectDataSource s`OldValuesParameterFormatString`屬性設`original_{0}`。 中所述[概觀的插入、 更新和刪除的資料](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)教學課程中，此屬性必須從宣告式語法中移除，或設回其預設值`{0}`，為了讓我們更新工作流程繼續進行，不會發生錯誤。

此外，在精靈完成 Visual Studio 將欄位加入至 GridView 的每個產品資料欄位。 移除以外的所有`ProductName`， `CategoryName`，和`UnitPrice`BoundFields。 接下來，更新`HeaderText`的這些 BoundFields 產品、 類別和價格以每個屬性分別。 由於`ProductName`是必填欄位，轉換為 TemplateField BoundField 和新增至 RequiredFieldValidator `EditItemTemplate`。 同樣地，將轉換`UnitPrice`到 TemplateField BoundField 和新增以確保使用者輸入的值是有效的貨幣值大於或等於零，s CompareValidator。 除了這些修改，任意執行任何美觀的變更，例如右側對齊`UnitPrice`值，或指定的格式`UnitPrice`的唯讀和編輯介面中的文字。

GridView s 智慧標籤的 啟用編輯核取方塊，讓 GridView 可進行編輯。 也請檢查啟用分頁，並啟用排序核取方塊。

> [!NOTE]
> 需要檢閱爧簏濻 GridView s 編輯介面嗎？ 如果是的話，請參閱上一步[自訂資料修改介面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)教學課程。


[![啟用適用於編輯、 排序和分頁 GridView 支援](caching-data-with-the-objectdatasource-vb/_static/image13.png)](caching-data-with-the-objectdatasource-vb/_static/image12.png)

**圖 6**： 啟用編輯，排序和分頁的 GridView 支援 ([按一下以檢視完整大小的影像](caching-data-with-the-objectdatasource-vb/_static/image14.png))


在這些 GridView 修改之後，GridView 和 ObjectDataSource s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](caching-data-with-the-objectdatasource-vb/samples/sample2.aspx)]

如 [圖 7] 所示，可編輯的 GridView 會列出名稱、 類別和每個資料庫中的產品的價格。 請花一點時間來測試頁面的功能排序結果逐頁查看它們，並編輯記錄。


[![每個產品名稱、 類別和價格會列在 可排序、 Pageable、 可編輯的 GridView](caching-data-with-the-objectdatasource-vb/_static/image16.png)](caching-data-with-the-objectdatasource-vb/_static/image15.png)

**圖 7**： 每個產品的名稱、 類別和價格會列在 可排序、 Pageable、 可編輯的 GridView ([按一下以檢視完整大小的影像](caching-data-with-the-objectdatasource-vb/_static/image17.png))


## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>步驟 3： 檢查時的 ObjectDataSource 會要求資料

`Products` GridView 擷取其資料，以顯示叫用`Select`方法`ProductsDataSource`ObjectDataSource。 這個 ObjectDataSource 建立商業邏輯層 s 的執行個體`ProductsBLL`類別並呼叫其`GetProducts()`方法，它接著會呼叫 s 中的資料存取層`ProductsTableAdapter`s`GetProducts()`方法。 DAL 方法連接至 Northwind 資料庫，並發出設定`SELECT`查詢。 這項資料會回到 DAL 中將它封裝起來`NorthwindDataTable`。 DataTable 物件傳回至 BLL，bll 回到 ObjectDataSource，則會傳回至 GridView。 接著會建立 GridView`GridViewRow`物件給每個`DataRow`datatable，而每個`GridViewRow`最終轉譯成已傳回給用戶端和訪客 s 瀏覽器顯示的 HTML。

這一系列的事件會發生每個 GridView 需要繫結至其基礎資料的時間。 會發生這種情況是當第一次瀏覽頁面時時排序 GridView，, 或修改透過內建的編輯或刪除介面的 GridView 的資料時，從一頁的資料移至另一個時。 GridView 的檢視狀態已停用，以及每個回傳上的 GridView 會被重新繫結。 GridView 可以也明確重新繫結至其資料藉由呼叫其`DataBind()`方法。

要充份鑒賞與從資料庫擷取資料的頻率，可讓 s 顯示訊息，指出當資料經過重新擷取。 新增名為 GridView 上方標籤 Web 控制項`ODSEvents`。 清除其`Text`屬性並設定其`EnableViewState`屬性設`False`。 下方的標籤，將 Button Web 控制項，並設定其`Text`回傳的屬性。


[![GridView 上方頁面中新增標籤和按鈕](caching-data-with-the-objectdatasource-vb/_static/image19.png)](caching-data-with-the-objectdatasource-vb/_static/image18.png)

**圖 8**: 頁面上方的 GridView 中新增標籤和按鈕 ([按一下以檢視完整大小的影像](caching-data-with-the-objectdatasource-vb/_static/image20.png))


在資料存取工作流程期間，ObjectDataSource 的`Selecting`事件引發之前建立的基礎物件和其設定的方法叫用。 建立此事件的事件處理常式，並新增下列程式碼：


[!code-vb[Main](caching-data-with-the-objectdatasource-vb/samples/sample3.vb)]

ObjectDataSource 的資料，架構會要求每次標籤會顯示文字選取事件引發。

請瀏覽此網頁瀏覽器中。 當第一次瀏覽頁面時，會顯示文字選取事件引發。 按一下回傳的按鈕，並請注意，文字就會消失 (假設 GridView s`EnableViewState`屬性設定為`True`，預設值)。 這是因為在回傳時，GridView 會重新建構從其檢視狀態，並因此不 t 向其資料的 ObjectDataSource。 排序、 分頁或編輯資料，不過，會導致重新繫結至其資料來源，GridView，因此選取事件引發的文字隨即再度出現。


[![只要 GridView 會重新繫結至其資料來源時，就會顯示選取事件引發](caching-data-with-the-objectdatasource-vb/_static/image22.png)](caching-data-with-the-objectdatasource-vb/_static/image21.png)

**圖 9**： 每當的 GridView 會重新繫結至其資料來源時，會顯示選取事件引發 ([按一下以檢視完整大小的影像](caching-data-with-the-objectdatasource-vb/_static/image23.png))


[![按一下回傳按鈕將會從其檢視狀態重新建構 GridView](caching-data-with-the-objectdatasource-vb/_static/image25.png)](caching-data-with-the-objectdatasource-vb/_static/image24.png)

**圖 10**： 按一下回傳的按鈕會導致從其檢視狀態重新建構 GridView ([按一下以檢視完整大小的影像](caching-data-with-the-objectdatasource-vb/_static/image26.png))


似乎浪費每次透過分頁或排序資料時擷取的資料庫資料。 畢竟，因為我們重新使用的預設分頁，ObjectDataSource 已擷取的所有記錄時顯示的第一頁。 即使 GridView 不提供排序和分頁支援，資料必須可從資料庫擷取任何使用者 （和在每個回傳時，如果已停用檢視狀態），頁面會先造訪每次。 但如果 GridView 會顯示相同的資料，所有使用者，這些額外的資料庫要求多餘。 為什麼不會快取傳回的結果`GetProducts()`方法，並繫結的 GridView 快取的結果？

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>步驟 4： 快取資料使用 ObjectDataSource

只要設定幾個屬性，則可以設定為自動快取的 ASP.NET 資料快取其擷取的資料的 ObjectDataSource。 下列清單摘要說明 ObjectDataSource 的快取相關屬性：

- [EnableCaching](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx)必須設為`True`來啟用快取。 預設為 `False`。
- [CacheDuration](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx)的時間 （秒），快取的資料量。 預設值為 0。 ObjectDataSource 會只快取的資料如果`EnableCaching`已`True`和`CacheDuration`設值小於或等於零。
- [CacheExpirationPolicy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx)可以設定為`Absolute`或`Sliding`。 如果`Absolute`，ObjectDataSource 快取其擷取的資料，以`CacheDuration`秒; 如果`Sliding`，資料過期時，只有在尚未針對存取之後，才`CacheDuration`秒。 預設為 `Absolute`。
- [CacheKeyDependency](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx)使用這個屬性，與現有的快取相依性產生關聯的 ObjectDataSource s 快取項目。 ObjectDataSource 的資料收回的項目可以是過早從快取過期及其相關聯`CacheKeyDependency`。 這個屬性通常用於關聯的 ObjectDataSource s 快取中的 SQL 快取相依性，主題會探討未來[使用 SQL 快取相依性](using-sql-cache-dependencies-vb.md)教學課程。

可讓設定 s `ProductsDataSource` ObjectDataSource 快取其資料，絕對比例上 30 秒。 設定 ObjectDataSource s`EnableCaching`屬性，以`True`及其`CacheDuration`到 30 之間的屬性。 離開`CacheExpirationPolicy`屬性設定為其預設`Absolute`。


[![設定快取其資料，30 秒內的 ObjectDataSource](caching-data-with-the-objectdatasource-vb/_static/image28.png)](caching-data-with-the-objectdatasource-vb/_static/image27.png)

**圖 11**： 設定要快取其資料的 30 秒的 ObjectDataSource ([按一下以檢視完整大小的影像](caching-data-with-the-objectdatasource-vb/_static/image29.png))


儲存變更並重新瀏覽此網頁瀏覽器中。 因為一開始的資料不是快取中，當您第一次造訪的頁面上，會出現選取引發的事件文字。 但後續的回傳中回傳的按鈕，排序，即可觸發的分頁，或按一下 [編輯] 或 [取消] 按鈕*不*重新顯示選取事件引發的文字。 這是因為`Selecting`ObjectDataSource 會從其基礎的物件，取得其資料時，只會引發事件`Selecting`如果會在將資料提取自資料快取將不會引發事件。

30 秒之後，資料將會從快取中收回。 資料將也從快取收回，如果 ObjectDataSource s `Insert`， `Update`，或`Delete`叫用方法。 因此，[更新] 按鈕已按下，排序、 分頁，或按一下 [編輯] 或 [取消] 按鈕會從其基礎的物件取得其資料 ObjectDataSource 或經過 30 秒之後，顯示選取事件引發的文字時`Selecting`引發事件。 這些傳回的結果會放回資料快取。

> [!NOTE]
> 如果您經常看到選取觸發事件的文字，即使您預期的 ObjectDataSource 會使用快取的資料，它可能是因為記憶體條件約束。 如果沒有足夠的可用記憶體，可能已清除的 ObjectDataSource 快取中加入的資料。 如果 ObjectDataSource 不 t 出現唯一的快取的資料正確快取資料偶關閉部分應用程式以釋放記憶體，並再試一次。


[圖 12] 說明快取的工作流程的 ObjectDataSource s。 選取事件引發時文字會出現在螢幕上，這是因為資料不是在快取中，且必須從基礎物件中擷取。 這段文字時遺失，不過，它 s 因為從快取可用的資料。 從快取，當傳回的資料那里 s 對基礎物件的任何呼叫，因此，任何資料庫的查詢執行。


![的 ObjectDataSource 會儲存及擷取資料的資料快取來源](caching-data-with-the-objectdatasource-vb/_static/image30.png)

**圖 12**: ObjectDataSource 會儲存並擷取其資料從資料快取


每個 ASP.NET 應用程式有自己的資料快取執行個體上所有頁面和訪客共用該 s。 這表示，資料儲存在資料快取中的 ObjectDataSource 會同樣地之間共用瀏覽網頁的所有使用者。 若要確認這點，開啟`ObjectDataSource.aspx`瀏覽器中的頁面。 當第一次瀏覽 頁面上，選取觸發事件文字會出現 （假設先前測試新增至快取的資料，到目前為止，已收回）。 開啟第二個瀏覽器執行個體，然後複製並貼上第二個從第一個瀏覽器執行個體的 URL。 在第二個瀏覽器執行個體中，選取觸發事件的文字不會顯示因為它使用相同快取的資料與第一個。

插入時擷取的資料快取，ObjectDataSource 會使用快取索引鍵的值，包括：`CacheDuration`和`CacheExpirationPolicy`屬性值; 正由 ObjectDataSource，指定基礎商務物件的類型透過[`TypeName`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.typename.aspx)(`ProductsBLL`，在此範例中); 的值`SelectMethod`屬性的名稱和參數的值`SelectParameters`集合和其值`StartRowIndex`並`MaximumRows`屬性，實作時，會使用[自訂分頁](../paging-and-sorting/paging-and-sorting-report-data-vb.md)。

製作的快取索引鍵值為這些屬性的組合可確保唯一的快取項目，因為這些值會變更。 例如，在過去的教學課程我們已討論過使用`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`，它會傳回指定分類的所有產品。 一位使用者可能會頁面並檢視的飲料，其中包含`CategoryID`為 1。 如果 ObjectDataSource 快取其結果，而不必考慮`SelectParameters`值，當另一位使用者的來源至頁面以檢視 「 調味品 」 飲料產品時所快取中時，他們 d 看到快取的飲料產品，而不是 「 調味品 」。 根據這些屬性改變快取索引鍵，其中包含的值`SelectParameters`，ObjectDataSource 會維護 beverages 和 「 調味品 」 的個別快取項目。

## <a name="stale-data-concerns"></a>過時的資料考量

ObjectDataSource 自動收回時有任何一種快取，其項目及其`Insert`， `Update`，或`Delete`方法叫用。 這有助於防止過時的資料，資料修改透過頁面時，清除快取項目。 不過，它可能會使用快取仍然顯示過時資料 ObjectDataSource。 在最簡單的情況下，它可能是因為在資料庫中直接變更的資料。 可能是資料庫管理員只會執行修改資料庫中記錄的一些指令碼。

這種情況下，也可以展開更細微的方式。 雖然 ObjectDataSource 收回其快取中的項目，其中一個資料修改方法呼叫時，快取中移除的項目會針對特定的屬性值組合 ObjectDataSource s (`CacheDuration`， `TypeName`， `SelectMethod`，等等）。 如果您有兩個使用不同的 ObjectDataSources`SelectMethods`或`SelectParameters`，但仍然可以更新相同的資料，則一個 ObjectDataSource 可能更新的資料列，並使它自己的快取項目，但對應的資料列的第二個 ObjectDataSource仍將會服務來自快取。 建議您建立頁面，以呈現這項功能。 建立會顯示可編輯的 GridView 會從 ObjectDataSource 會使用快取，而且已設定為從取得資料提取其資料的網頁`ProductsBLL`類別的`GetProducts()`方法。 新增另一個可編輯的 GridView 和 ObjectDataSource 到此頁面 （或另一個），而第二個 ObjectDataSource 讓它使用`GetProductsByCategoryID(categoryID)`方法。 因為兩個 ObjectDataSources`SelectMethod`屬性不同，它們將每個有自己的快的取值。 如果您編輯的產品，在一個方格中，的下次您將資料繫結回至其他資料格 （分頁、 排序和其他等等），它將仍提供舊的、 快取的資料，並不會反映從其他格線所做的變更。

簡單地說，如果只使用以時間為基礎的 expiries 您願意可能過時的資料，並使用較短的 expiries 案例其中的資料有效期限是很重要。 如果找不到可接受的過時資料，請放棄快取，或使用 SQL 快取相依性 (假設它資料庫資料您快取)。 在未來的教學課程中，我們將探討 SQL 快取相依性。

## <a name="summary"></a>總結

在本教學課程中，我們檢查 ObjectDataSource s 內建快取功能。 只要設定幾個屬性，我們可以指示要快取傳回從指定的結果 ObjectDataSource`SelectMethod`到 ASP.NET 資料快取。 `CacheDuration`和`CacheExpirationPolicy`屬性會指出項目會被快取的持續時間，以及它是否是絕對或滑動期限。 `CacheKeyDependency`屬性會將所有的 ObjectDataSource s 快取項目關聯現有的快取相依性。 這可用來收回 ObjectDataSource s 項目，從快取之前的時間為基礎的到期日達到，而且通常可搭配 SQL 快取相依性。

ObjectDataSource 只會快取的資料快取其值，因為我們無法以程式設計方式將複寫的 ObjectDataSource s 的內建功能。 它不 t 意義來執行這項操作在展示層，因為 ObjectDataSource 提供這項功能內建的但我們可以在獨立的圖層的架構來實作快取功能。 若要這樣做，我們必須重複相同的邏輯使用 ObjectDataSource。 我們將探討如何以程式設計方式使用我們的下一個教學課程中的 從架構中的資料快取的。

快樂地寫程式 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ASP.NET 快取： 技巧和最佳作法](https://msdn.microsoft.com/library/aa478965.aspx)
- [.NET Framework 應用程式的快取架構指南](https://msdn.microsoft.com/library/ee817645.aspx)
- [ASP.NET 2.0 中的輸出快取](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Teresa murphy 徹底檢驗了。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](using-sql-cache-dependencies-cs.md)
> [下一頁](caching-data-in-the-architecture-vb.md)
