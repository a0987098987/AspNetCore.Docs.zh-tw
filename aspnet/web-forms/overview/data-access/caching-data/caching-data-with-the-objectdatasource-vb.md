---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-vb
title: "快取資料與 ObjectDataSource (VB) |Microsoft 文件"
author: rick-anderson
description: "快取，可能表示緩慢和快速的 Web 應用程式之間的差異。 本教學課程會詳細看看 ASP.NET 中的快取的四個中的第一個..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 2e56a733-5512-48a6-9276-70a65bbe4d5d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: fa0a0f1f80a407f8f68d5fe081b5b144e2945700
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="caching-data-with-the-objectdatasource-vb"></a>快取資料與 ObjectDataSource (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_VB.exe)或[下載 PDF](caching-data-with-the-objectdatasource-vb/_static/datatutorial58vb1.pdf)

> 快取，可能表示緩慢和快速的 Web 應用程式之間的差異。 本教學課程會詳細看看 ASP.NET 中的快取的四個中的第一個。 了解快取的重要概念以及如何將套用至展示層 ObjectDataSource 控制項透過快取。


## <a name="introduction"></a>簡介

在電腦科學中，*快取*是取得資料或會高度耗費資源取得的資訊，然後將它的複本儲存在更快速地進行存取的位置的程序。 資料驅動應用程式的大型且複雜的查詢通常會耗用大部分的應用程式的執行時間。 這類應用程式 s，然後通常可改善效能將高度耗費資源的資料庫查詢的結果儲存在應用程式的記憶體中。

ASP.NET 2.0 提供各種不同的快取選項。 整個網頁或使用者控制項 s 呈現標記可以快取透過*輸出快取*。 ObjectDataSource 和 SqlDataSource 控制項都會提供快取功能以及，藉此讓資料快取控制層級。 和 ASP.NET s*資料快取*提供了豐富的快取 API，可讓網頁開發人員以程式設計方式快取物件。 在本教學課程中，我們將檢驗使用 ObjectDataSource s 下一個三個快取功能，以及資料快取。 我們也將探討如何快取在啟動應用程式層級資料以及如何保持透過 SQL 快取相依性使用最新的快取的資料。 這些教學課程不會探索輸出快取。 如需在輸出快取，請參閱[輸出快取在 ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)。

快取可以在任何地方在架構中，資料存取層的向上透過套用展示層。 在本教學課程，我們將探討套用快取以透過 ObjectDataSource 控制項展示層。 在下一個教學課程中我們將檢驗快取資料在商務邏輯層。

## <a name="key-caching-concepts"></a>金鑰快取概念

快取可大幅改善應用程式 s 的整體效能和延展性，會產生高度耗費資源的資料，以及將它的複本儲存在可以更有效率地存取的位置。 快取會儲存 just 實際、 基礎資料的複本，因為它也可能會過期，或*過時*，如果基礎資料變更。 若要避免此問題，網頁開發人員可以指出快取項目將會依據準則*收回*從快取中，使用：

- **以時間為基礎的準則**項目可能會加入至快取中的絕對或滑動持續時間。 例如，網頁開發人員可能表示，例如，60 秒的持續時間。 絕對的持續時間，與快取的項目會將它收回 60 秒之後已將它加入快取中，不論它存取的頻率。 滑動持續時間，快取的項目會收回在上次存取後的 60 秒。
- **相依性為基礎的準則**相依性可能是相關聯的項目加入至快取時。 項目 s 相依性變更時就會從快取中收回。 相依性可能是檔案、 另一個快取項目或兩者的組合。 ASP.NET 2.0 也可讓 SQL 快取相依性，可讓開發人員加入至快取的項目，並讓它收回基礎資料庫資料變更時。 我們會檢查 SQL 快取相依性中即將[使用 SQL 快取相依性](using-sql-cache-dependencies-vb.md)教學課程。

快取中的項目可能是指定的收回條件，不論*清除*之前符合時間或相依性為基礎的準則。 如果快取已達到其容量，就必須移除現有的項目才能加入新的。 因此，當以程式設計方式使用快取的資料可 s 重要，您一律假設快取的資料可能不存在。 我們將在下一個教學課程中，以程式設計方式存取資料從快取時要使用的模式*架構中的快取資料*。

快取提供經濟的方式並從應用程式效能。 做為[Steven Smith](http://aspadvice.com/blogs/ssmith/) articulates 在他的文章[ASP.NET 快取： 技巧和最佳作法](https://msdn.microsoft.com/en-us/library/aa478965.aspx):

快取可以取得良好足夠效能而不需要很長的時間和分析的好方法。 記憶體便宜，因此您可以取得您需要的快取輸出，而不需花費一天或週，嘗試最佳化您的程式碼或資料庫 30 秒的效能，如果不要快取方案 （假設為第二個舊-30 資料為 [確定]），且將上。 最後，不良的設計將可能跟上，因此的課程中，您應該試著正確地設計您的應用程式。 但如果您只需要取得良好不足，無法在目前的效能，快取可以是一個絕佳 [方法]，購買您有時間重構您的應用程式，在日後當您有時間，若要這樣做。

雖然快取，可以提供明顯的效能增強功能，它並不適用於所有情況下，例如使用即時、 經常更新的資料，或是無法接受更短時間內存在的過時資料的應用程式。 但是，對於大部分的應用程式，快取應該使用。 如需快取在 ASP.NET 2.0 的詳細背景，請參閱[快取效能](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx)區段[ASP.NET 2.0 快速入門教學課程](https://quickstarts.asp.net/QuickStartv20/aspnet/)。

## <a name="step-1-creating-the-caching-web-pages"></a>步驟 1： 建立快取的 Web 網頁

我們一開始我們的 ObjectDataSource s 快取功能的瀏覽之前，可讓 s 先花點時間我們網站專案中，我們需要針對此教學課程中，下一步的三個建立的 ASP.NET 網頁。 加入新的資料夾，名為啟動`Caching`。 接下來，將下列 ASP.NET 網頁新增至該資料夾中，務必要建立關聯的每一頁`Site.master`主版頁面：

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`


![加入 ASP.NET 網頁的快取相關的教學課程](caching-data-with-the-objectdatasource-vb/_static/image1.png)

**圖 1**： 加入 ASP.NET 網頁的快取相關的教學課程


如同在其他資料夾，`Default.aspx`中`Caching`資料夾會列出這些教學課程 > 一節中。 請記得，`SectionLevelTutorialListing.ascx`使用者控制項提供這項功能。 因此，新增此使用者控制項加入`Default.aspx`拖曳到頁面的設計 檢視中的 方案總管 中。


[![圖 2： 將 SectionLevelTutorialListing.ascx 使用者控制項加入至 Default.aspx](caching-data-with-the-objectdatasource-vb/_static/image3.png)](caching-data-with-the-objectdatasource-vb/_static/image2.png)

**圖 2**： 圖 2： 新增`SectionLevelTutorialListing.ascx`使用者控制項加入`Default.aspx`([按一下以檢視完整大小的影像](caching-data-with-the-objectdatasource-vb/_static/image4.png))


最後，將這些頁面加入做為項目至`Web.sitemap`檔案。 具體而言，二進位資料搭配使用之後加入下列標記`<siteMapNode>`:


[!code-xml[Main](caching-data-with-the-objectdatasource-vb/samples/sample1.xml)]

在更新之後， `Web.sitemap`，花一點時間來檢視此教學課程網站，透過瀏覽器。 左側功能表現在包含項目，為快取的教學課程。


![如需快取的教學課程網站導覽現在包含的項目](caching-data-with-the-objectdatasource-vb/_static/image5.png)

**圖 3**： 網站導覽現在包含項目，為快取的教學課程


## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>步驟 2： 在網頁中顯示產品的清單

本教學課程會探討如何使用 ObjectDataSource 控制項 s 內建快取功能。 我們來看看這些功能之前，不過，我們先必須從工作頁面。 可讓 s 建立使用以列出產品資訊從 ObjectDataSource 所擷取的 GridView 的網頁`ProductsBLL`類別。

先開啟`ObjectDataSource.aspx`頁面`Caching`資料夾。 從 工具箱 拖曳至設計工具拖曳 GridView、 設定其`ID`屬性`Products`，並從其智慧標籤上，選擇 繫結至新的 ObjectDataSource 控制項，名為`ProductsDataSource`。 設定用於 ObjectDataSource`ProductsBLL`類別。


[![設定使用 ProductsBLL 類別 ObjectDataSource](caching-data-with-the-objectdatasource-vb/_static/image7.png)](caching-data-with-the-objectdatasource-vb/_static/image6.png)

**圖 4**： 設定要使用 ObjectDataSource`ProductsBLL`類別 ([按一下以檢視完整大小的影像](caching-data-with-the-objectdatasource-vb/_static/image8.png))


此頁面，可讓建立可編輯的 GridView，以便我們可以檢查在 ObjectDataSource 快取的資料修改透過 GridView 的介面時，會發生什麼事。 保留下拉式清單中選取索引標籤上，並為其預設設定`GetProducts()`，但變更更新索引標籤中選取的項目`UpdateProduct`多載會接受`productName`， `unitPrice`，和`productID`做為其輸入參數。


[![將更新索引標籤的下拉式清單設為適當的 UpdateProduct 多載](caching-data-with-the-objectdatasource-vb/_static/image10.png)](caching-data-with-the-objectdatasource-vb/_static/image9.png)

**圖 5**： 更新 索引標籤的下拉式清單設合適`UpdateProduct`多載 ([按一下以檢視完整大小的影像](caching-data-with-the-objectdatasource-vb/_static/image11.png))


最後，設定為 （無） 插入和刪除索引標籤中的下拉式清單，然後按一下 完成。 完成設定資料來源精靈，Visual Studio 設定 ObjectDataSource s`OldValuesParameterFormatString`屬性`original_{0}`。 中所述[概觀的插入、 更新和刪除資料](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)教學課程中，若要移除的宣告式語法或設為預設值，這個屬性需要`{0}`，為了讓我們更新工作流程繼續進行，不會發生錯誤。

此外，在精靈完成 Visual Studio 將欄位加入至 GridView 每個產品資料欄位。 移除以外的所有`ProductName`， `CategoryName`，和`UnitPrice`BoundFields。 接下來，更新`HeaderText`屬性的每個產品、 分類和價格，這些 BoundFields 分別。 因為`ProductName`是必填欄位、 BoundField 轉換為 TemplateField 和新增至 RequiredFieldValidator `EditItemTemplate`。 同樣地，將轉換`UnitPrice`到 TemplateField BoundField 並加入 CompareValidator 以確保使用者所輸入的值是有效的貨幣值 s 大於或等於零。 除了這些修改，則可以自由執行任何美觀的變更，例如右對齊`UnitPrice`值或指定的格式`UnitPrice`其唯讀和編輯介面中的文字。

請檢查 GridView s 智慧標籤的 啟用編輯核取方塊可編輯 GridView。 另請檢查啟用分頁和啟用排序核取方塊。

> [!NOTE]
> 需要檢閱如何自訂 GridView s 編輯介面嗎？ 若是如此，請參閱上一步[自訂的資料修改介面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)教學課程。


[![啟用編輯、 排序和分頁的 GridView 支援](caching-data-with-the-objectdatasource-vb/_static/image13.png)](caching-data-with-the-objectdatasource-vb/_static/image12.png)

**圖 6**： 啟用編輯排序和分頁的 GridView 支援 ([按一下以檢視完整大小的影像](caching-data-with-the-objectdatasource-vb/_static/image14.png))


在這些 GridView 修改後，GridView 和 ObjectDataSource s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](caching-data-with-the-objectdatasource-vb/samples/sample2.aspx)]

如圖 7 所示，可編輯的 GridView 會列出名稱、 類別和每個資料庫中的產品的價格。 請花一點時間頁面的功能排序測試結果中，瀏覽，並編輯一筆記錄。


[![每個產品名稱、 類別和價格列為 Sortable、 Pageable、 可編輯的 GridView](caching-data-with-the-objectdatasource-vb/_static/image16.png)](caching-data-with-the-objectdatasource-vb/_static/image15.png)

**圖 7**： 每個產品的名稱、 類別和價格列為 Sortable、 Pageable、 可編輯 GridView ([按一下以檢視完整大小的影像](caching-data-with-the-objectdatasource-vb/_static/image17.png))


## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>步驟 3： 檢查時 ObjectDataSource 會要求資料

`Products` GridView 擷取要顯示叫用其資料`Select`方法`ProductsDataSource`ObjectDataSource。 此 ObjectDataSource 建立執行個體的商務邏輯層 s`ProductsBLL`類別並呼叫其`GetProducts()`方法，也就會呼叫 s 的資料存取層`ProductsTableAdapter`s`GetProducts()`方法。 DAL 方法會連接到 Northwind 資料庫，並會發出設定`SELECT`查詢。 這項資料會回到 DAL 中封裝起來`NorthwindDataTable`。 DataTable 物件會傳回到 BLL，回到 ObjectDataSource，則會傳回至 GridView。 接著會建立 GridView`GridViewRow`物件給每個`DataRow`DataTable 中與每個`GridViewRow`最後會轉譯為 HTML 是傳回至用戶端，並顯示在訪客 s 瀏覽器。

此事件的順序就會發生 GridView 需要繫結至其基礎資料的每個階段。 發生這種情況時第一次瀏覽頁面時，排序 GridView 中時，或修改 GridView 的資料，透過內建的編輯或刪除介面時，從一頁的資料移至另一個時。 GridView 的檢視狀態已停用，以及每個回傳的 GridView 會被重新繫結。 在 GridView 可以也重新明確繫結至其資料藉由呼叫其`DataBind()`方法。

若要充分運用獲得與從資料庫擷取資料的頻率，可讓 s 顯示訊息，指出當資料重新擷取。 加入標籤 Web 控制項上方名為 GridView `ODSEvents`。 清除其`Text`屬性並設定其`EnableViewState`屬性`False`。 下方的標籤，將按鈕 Web 控制項，並設定其`Text`回傳的屬性。


[![加入 GridView 上方頁面上的標籤和按鈕](caching-data-with-the-objectdatasource-vb/_static/image19.png)](caching-data-with-the-objectdatasource-vb/_static/image18.png)

**圖 8**： 將標籤和按鈕加入至頁面上方 GridView ([按一下以檢視完整大小的影像](caching-data-with-the-objectdatasource-vb/_static/image20.png))


在資料存取工作流程期間，ObjectDataSource 的`Selecting`事件引發之前建立基礎物件和叫用其設定的方法。 建立此事件的事件處理常式，並加入下列程式碼：


[!code-vb[Main](caching-data-with-the-objectdatasource-vb/samples/sample3.vb)]

ObjectDataSource 資料，架構就會要求每次標籤會顯示在文字 Selecting 事件引發。

請瀏覽這個瀏覽器中。 當第一次瀏覽頁面時，會顯示在文字 Selecting 事件引發。 按一下 [回傳] 按鈕，並記下文字就會消失 (假設 GridView s`EnableViewState`屬性設定為`True`，預設值)。 這是因為在回傳時，GridView 會重新建構從其檢視狀態，因此未開啟選項，以用於其資料 ObjectDataSource。 排序、 分頁或編輯資料，不過，會導致重新繫結至其資料來源，GridView，因此在 Selecting 事件引發文字隨即再度出現。


[![每當 GridView 重新繫結至其資料來源時，不會顯示選取事件引發](caching-data-with-the-objectdatasource-vb/_static/image22.png)](caching-data-with-the-objectdatasource-vb/_static/image21.png)

**圖 9**： 每當 GridView 重新繫結至其資料來源時，會顯示 選取事件引發 ([按一下以檢視完整大小的影像](caching-data-with-the-objectdatasource-vb/_static/image23.png))


[![按一下回傳按鈕將會從其檢視狀態重新建構 GridView](caching-data-with-the-objectdatasource-vb/_static/image25.png)](caching-data-with-the-objectdatasource-vb/_static/image24.png)

**圖 10**： 按一下回傳按鈕會導致重新建構其檢視狀態從 GridView ([按一下以檢視完整大小的影像](caching-data-with-the-objectdatasource-vb/_static/image26.png))


它可能看起來很浪費資料庫資料在每次擷取的資料是透過分頁，或排序。 畢竟，因為我們重新使用的預設分頁 ObjectDataSource 已擷取的所有記錄時顯示的第一頁。 即使 GridView 不提供排序和分頁支援，資料必須是從資料庫擷取每次第一次瀏覽頁面時的任何使用者 （和在每次回傳，如果已停用檢視狀態）。 但如果 GridView 會顯示相同的資料，所有使用者，這些額外的資料庫要求過多。 為什麼不會快取從傳回的結果`GetProducts()`方法和繫結的 GridView 快取結果？

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>步驟 4： 快取的資料，使用 ObjectDataSource

只需要設定一些屬性，ObjectDataSource 可以設定為自動快取的 ASP.NET 資料快取其擷取的資料。 下列清單摘要說明 ObjectDataSource 的快取相關屬性：

- [EnableCaching](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx)必須設為`True`啟用快取。 預設為 `False`。
- [CacheDuration](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx)的時間 （秒），快取資料量。 預設值為 0。 ObjectDataSource 將只能快取資料如果`EnableCaching`是`True`和`CacheDuration`設定的值小於或等於零。
- [CacheExpirationPolicy](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx)可設為`Absolute`或`Sliding`。 如果`Absolute`，ObjectDataSource 快取其擷取的資料，以`CacheDuration`秒; 如果`Sliding`的資料過期未經存取的之後，才`CacheDuration`秒。 預設為 `Absolute`。
- [CacheKeyDependency](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx) ObjectDataSource s 快取項目與現有的快取相依性中使用這個屬性。 ObjectDataSource 的資料項目可以提前收回從快取設定為已過期及其相關聯`CacheKeyDependency`。 這個屬性通常用於關聯 ObjectDataSource s 快取中的 SQL 快取相依性、 主題我們將探討未來[使用 SQL 快取相依性](using-sql-cache-dependencies-vb.md)教學課程。

可讓設定 s `ProductsDataSource` ObjectDataSource 絕對標尺上 30 秒快取其資料。 設定 ObjectDataSource s`EnableCaching`屬性`True`及其`CacheDuration`屬性設為 30。 保留`CacheExpirationPolicy`屬性設定為其預設`Absolute`。


[![設定要快取其資料的 30 秒 ObjectDataSource](caching-data-with-the-objectdatasource-vb/_static/image28.png)](caching-data-with-the-objectdatasource-vb/_static/image27.png)

**圖 11**： 設定要快取其資料的 30 秒 ObjectDataSource ([按一下以檢視完整大小的影像](caching-data-with-the-objectdatasource-vb/_static/image29.png))


儲存變更並再次瀏覽這個瀏覽器中。 因為一開始的資料已不在快取中，當您第一次瀏覽 頁面上，會顯示選取引發的事件文字。 但後續回傳時回傳的按鈕，排序，按一下所觸發的分頁，或按一下 [編輯] 或 [取消] 5d; 按鈕*不*顯示在 Selecting 事件引發文字。 這是因為`Selecting`僅就會引發事件時 ObjectDataSource 取得其資料從其基礎的物件;`Selecting`如果資料取自資料快取將不會引發事件。

30 秒之後，資料將會收回快取。 資料將也會在從快取收回，如果 ObjectDataSource s `Insert`， `Update`，或`Delete`叫用方法。 因此，[更新] 按鈕已按下，排序、 分頁，或按一下 [編輯] 或 [取消] 5d; 按鈕會從其基礎物件取得其資料 ObjectDataSource 或已超過 30 秒之後，顯示在 Selecting 事件引發文字時`Selecting`事件引發。 這些傳回的結果會放回資料快取。

> [!NOTE]
> 如果您經常看到選定引發的事件文字，即使您預期會使用快取資料，ObjectDataSource 它可能是因為記憶體條件約束。 如果沒有足夠的可用記憶體，可能已清除 objectdatasource 新增至快取的資料。 如果 ObjectDataSource 規定 t 出現或只快取的資料正確快取資料偶發性，關閉一些應用程式以釋放記憶體，並再試一次。


圖 12 說明 ObjectDataSource s 中的快取工作流程。 在 Selecting 事件引發時文字會出現在螢幕上，這是因為資料不是在快取中，且必須從基礎物件中擷取。 當此文字遺漏時，不過，它 s 因為從快取所使用的資料。 從快取傳回的資料時那里 s 沒有基礎物件的呼叫，因此，沒有資料庫的查詢執行。


![ObjectDataSource 存放區和擷取其資料從資料快取](caching-data-with-the-objectdatasource-vb/_static/image30.png)

**圖 12**: ObjectDataSource 儲存和擷取其資料從資料快取


每個 ASP.NET 應用程式有自己的資料快取執行個體上所有頁面和訪客共用該 s。 這表示 objectdatasource 資料快取中儲存的資料同樣橫跨所有使用者瀏覽的頁面。 若要確認這種情況，請開啟`ObjectDataSource.aspx`瀏覽器中的。 當第一次瀏覽頁面，選取引發事件會以文字 （假設先前的測試加入至快取的資料，現在已收回）。 開啟第二個瀏覽器執行個體，然後複製並貼上第二個從第一個瀏覽器執行個體的 URL。 第二個瀏覽器執行個體中選取事件引發文字不會顯示因為它多個使用相同快取的資料與第一個。

ObjectDataSource 插入時擷取的資料快取，會使用快取索引鍵的值，其中包含：`CacheDuration`和`CacheExpirationPolicy`屬性值; ObjectDataSource，指定正在使用的基礎商務物件的類型透過[`TypeName`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.typename.aspx)(`ProductsBLL`，在此範例中); 的值`SelectMethod`屬性的名稱和值的參數中`SelectParameters`集合和其值`StartRowIndex`和`MaximumRows`屬性實作時，使用[自訂分頁](../paging-and-sorting/paging-and-sorting-report-data-vb.md)。

製作的快取索引鍵值為這些屬性的組合可確保唯一快取項目，這些值會變更。 例如，在過去的教學課程中我們我們探討了使用`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`，它會傳回指定分類的所有產品。 一位使用者可能會頁面並檢視飲料具有`CategoryID`為 1。 如果 ObjectDataSource 快取而不考慮其結果`SelectParameters`值，則當其他使用者頁面以檢視 「 調味品 」 飲料產品時快取中，d 他們會看到快取的飲料產品，而不是 「 調味品 」。 改變這些屬性的快取索引鍵，其中包含的值`SelectParameters`，ObjectDataSource 維護 beverages 和 「 調味品 」 的不同的快取項目。

## <a name="stale-data-concerns"></a>過時的資料考量

ObjectDataSource 自動收回其項目從快取的任何一個其`Insert`， `Update`，或`Delete`方法叫用。 這有助於防止過時的資料是透過 [] 頁面修改的資料清除快取項目。 不過，它可能會使用快取仍會顯示過時資料 ObjectDataSource。 在最簡單的情況下，它可能是因為在資料庫中直接變更的資料。 可能是資料庫管理員只會執行修改資料庫中記錄的某些指令碼。

這種情況下，也無法展開以更細微的方式。 快取中移除的項目時 ObjectDataSource 收回從快取其項目，其資料的修改方法的其中一個呼叫時，會針對特定的屬性值組合 ObjectDataSource s (`CacheDuration`， `TypeName`， `SelectMethod`，等等）。 如果您有使用不同的兩個 ObjectDataSources`SelectMethods`或`SelectParameters`，但仍然可以更新相同的資料，則一個 ObjectDataSource 可能更新一個資料列，並使它自己的快取項目，但對應的資料列的第二個 ObjectDataSource仍將會服務來自快取。 建議您將建立至展現這項功能的網頁。 建立可顯示從 ObjectDataSource 使用快取，會設定用於取得資料提取其資料的可編輯 GridView 的頁面`ProductsBLL`類別的`GetProducts()`方法。 加入另一個可編輯 GridView 以及此頁面 （或另一個），但是針對這個第二個 ObjectDataSource ObjectDataSource 讓它使用`GetProductsByCategoryID(categoryID)`方法。 因為兩個 ObjectDataSources`SelectMethod`屬性不同，它們每個 ll 擁有其快取的值。 如果您編輯的產品，在一個方格中，的下次您將資料繫結至其他方格 （分頁、 排序和其他等等），它將仍可作為舊、 快取資料，並不會反映從其他資料格所做的變更。

簡單地說，如果只使用以時間為基礎的 expiries 您願意可能會過時的資料，並使用較短的 expiries 案例，其中資料的有效期限是重要。 如果無法接受資料失效，請放棄快取，或使用 SQL 快取相依性 (假設它資料庫的資料快取作為您)。 在未來的教學課程中，我們將探討 SQL 快取相依性。

## <a name="summary"></a>總結

在本教學課程，我們會檢查 ObjectDataSource s 內建快取功能。 只需要設定一些屬性，我們可以指示快取傳回從指定的結果 ObjectDataSource`SelectMethod`到 ASP.NET 資料快取。 `CacheDuration`和`CacheExpirationPolicy`屬性會指出項目會快取的持續時間，以及它是否是絕對或滑動期限。 `CacheKeyDependency`屬性會將所有 ObjectDataSource s 快取項目關聯現有的快取相依性。 這可以用於 ObjectDataSource s 項目，從快取中收回之前的時間為基礎的到期日達到，而且通常會搭配 SQL 快取相依性。

ObjectDataSource 只會快取資料快取其值，因為我們無法以程式設計的方式複寫 ObjectDataSource s 內建功能。 它規定 t 合理的做法是在展示層，因為 ObjectDataSource 提供這項功能，根據預設，但是我們可以在個別圖層的架構實作快取功能。 若要這樣做，我們需要重複 ObjectDataSource 所使用的相同邏輯。 我們將探討如何以程式設計的方式從架構中的資料快取在我們的下一個教學課程。

祝您程式設計 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ASP.NET 快取： 技巧和最佳作法](https://msdn.microsoft.com/en-us/library/aa478965.aspx)
- [.NET Framework 應用程式的快取架構指南](https://msdn.microsoft.com/en-us/library/ee817645.aspx)
- [ASP.NET 2.0 中的輸出快取](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已本文菲。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一頁](using-sql-cache-dependencies-cs.md)
[下一頁](caching-data-in-the-architecture-vb.md)
