---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb
title: 插入、 更新和刪除資料 (VB) 的概觀 |Microsoft 文件
author: rick-anderson
description: 在本教學課程中，我們會看到如何將對應 ObjectDataSource insert、 update （)，和 BLL 方法 delete （） 方法的類別，以及如何使用組態...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 35b40b8f-2ca8-4ab3-9c19-f361a91a3647
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb
msc.type: authoredcontent
ms.openlocfilehash: db77d9ec5b0d4b27259023363e786b26fe736d7b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890817"
---
<a name="an-overview-of-inserting-updating-and-deleting-data-vb"></a>插入、 更新和刪除資料 (VB) 的概觀
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_VB.exe)或[下載 PDF](an-overview-of-inserting-updating-and-deleting-data-vb/_static/datatutorial16vb1.pdf)

> 在此教學課程中我們會看到如何將對應 ObjectDataSource insert、 update （)，和 BLL 方法 delete （） 方法的類別，以及如何設定以提供資料修改功能的 GridView、 DetailsView 和 FormView 控制項。


## <a name="introduction"></a>簡介

在過去幾個教學課程中，我們已經檢查過如何使用 GridView、 DetailsView 和 FormView 控制項之 ASP.NET 網頁中顯示資料。 這些控制項只會使用提供給它們的資料。 通常，這些控制項存取透過資料來源控制項，例如 ObjectDataSource 使用的資料。 我們已看到如何 ObjectDataSource 做為 ASP.NET 網頁與基礎資料之間的 proxy。 GridView 需要時顯示的資料，它會叫用其 ObjectDataSource`Select()`方法，它會接著叫用的方法從我們商務邏輯層 (BLL)，會呼叫的方法中適當資料存取圖層 (DAL) 的 TableAdapter，繼而再傳送`SELECT` Northwind 資料庫的查詢。

當我們建立 Tableadapter 中 DAL 中時，請記得，[我們第一個教學課程](../introduction/creating-a-data-access-layer-cs.md)、 Visual Studio 自動加入方法來插入、 更新和刪除的資料從基礎資料庫資料表。 此外，在[建立商務邏輯層](../introduction/creating-a-business-logic-layer-vb.md)我們在這些資料修改 DAL 方法設計中向下呼叫 BLL 方法。

除了其`Select()`方法，也有 ObjectDataSource `Insert()`， `Update()`，和`Delete()`方法。 像`Select()`方法，這三個方法可以對應至基礎物件中的方法。 當設定為插入、 更新或刪除資料，GridView、 DetailsView 和 FormView 控制項提供使用者介面修改基礎資料。 此使用者介面呼叫`Insert()`， `Update()`，和`Delete()`ObjectDataSource，方法就會再叫用基礎物件的關聯 （請參閱圖 1） 的方法。


[![ObjectDataSource insert、 update （） 和 delete （） 方法做為 Proxy BLL 到](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image1.png)

**圖 1**: ObjectDataSource `Insert()`， `Update()`，和`Delete()`方法做為 BLL 到 Proxy ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image3.png))


在本教學課程中，我們會看到如何將對應 ObjectDataSource `Insert()`， `Update()`，和`Delete()`BLL，以及如何設定來提供資料修改的 GridView、 DetailsView 和 FormView 控制項中類別方法的方法功能。

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>步驟 1： 建立 Insert、 Update 和 Delete 教學課程 Web 網頁

我們可以開始探索如何插入、 更新和刪除資料之前，讓我們先花一點時間在我們的網站專案，我們需要本教學課程和下一步的數個項目中建立 ASP.NET 網頁。 加入新的資料夾，名為啟動`EditInsertDelete`。 接下來，將下列 ASP.NET 網頁新增至該資料夾中，務必要建立關聯的每一頁`Site.master`主版頁面：

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![加入 ASP.NET 網頁，針對資料修改相關教學課程](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image4.png)

**圖 2**： 加入 ASP.NET 網頁，針對資料修改相關教學課程


如同在其他資料夾，`Default.aspx`中`EditInsertDelete`資料夾會列出這些教學課程 > 一節中。 請記得，`SectionLevelTutorialListing.ascx`使用者控制項提供這項功能。 因此，新增此使用者控制項加入`Default.aspx`拖曳到頁面的 [設計] 檢視 [方案總管] 中。


[![將 SectionLevelTutorialListing.ascx 使用者控制項加入至 Default.aspx](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image5.png)

**圖 3**： 新增`SectionLevelTutorialListing.ascx`使用者控制項加入`Default.aspx`([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image7.png))


最後，將頁面加入做為項目至`Web.sitemap`檔案。 具體而言，在自訂格式化之後加入下列標記`<siteMapNode>`:


[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample1.xml)]

在更新之後， `Web.sitemap`，花一點時間來檢視此教學課程網站，透過瀏覽器。 在左側功能表現在包含編輯、 插入和刪除教學課程的項目。


![編輯、 插入和刪除教學課程的站台地圖現在包含的項目](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image8.png)

**圖 4**： 網站導覽現在包含項目編輯、 插入和刪除教學課程


## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>步驟 2： 加入和設定 ObjectDataSource 控制項

之後的 GridView、 DetailsView 和 FormView 每個不同資料修改功能和版面配置中，讓我們來討論每個個別。 而不是有使用它自己 ObjectDataSource 每個控制項，不過，我們已建立所有的三個控制項範例可以共用單一 ObjectDataSource。

開啟`Basics.aspx`頁面上，從 工具箱 拖曳至設計工具中，拖曳 ObjectDataSource，按一下 設定資料來源連結，從其智慧標籤。 因為`ProductsBLL`是編輯、 插入和刪除的方法，設定要使用這個類別 ObjectDataSource 所提供的唯一 BLL 類別。


[![設定使用 ProductsBLL 類別 ObjectDataSource](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image9.png)

**圖 5**： 設定要使用 ObjectDataSource`ProductsBLL`類別 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image11.png))


在下一個畫面中，我們可以指定哪些方法的`ProductsBLL`類別會對應到 ObjectDataSource `Select()`， `Insert()`， `Update()`，和`Delete()`選取適當的索引標籤，然後從下拉式清單中選擇的方法。 圖 6，這應該看起來很熟悉到目前為止，對應 ObjectDataSource`Select()`方法`ProductsBLL`類別的`GetProducts()`方法。 `Insert()`， `Update()`，和`Delete()`可以設定方法，沿著上方清單中選取適當的索引標籤。


[![ObjectDataSource 已傳回所有產品](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image12.png)

**圖 6**： 有 ObjectDataSource 傳回所有產品的 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image14.png))


圖 7、 8 和 9 ObjectDataSource UPDATE、 INSERT 和 DELETE 的顯示索引標籤。 設定這些索引標籤，讓`Insert()`， `Update()`，和`Delete()`方法叫用`ProductsBLL`類別的`UpdateProduct`， `AddProduct`，和`DeleteProduct`方法，分別。


[![ObjectDataSource update （） 方法對應至 ProductBLL 類別 UpdateProduct 方法](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image15.png)

**圖 7**： 對應 ObjectDataSource`Update()`方法`ProductBLL`類別的`UpdateProduct`方法 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image17.png))


[![ObjectDataSource insert （） 方法對應至 ProductBLL 類別 AddProduct 方法](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image18.png)

**圖 8**： 對應 ObjectDataSource`Insert()`方法`ProductBLL`類別的新增`Product`方法 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image20.png))


[![ObjectDataSource delete （） 方法對應至 ProductBLL 類別 DeleteProduct 方法](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image21.png)

**圖 9**： 對應 ObjectDataSource`Delete()`方法`ProductBLL`類別的`DeleteProduct`方法 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image23.png))


您可能已注意到下拉式清單，在 INSERT、 UPDATE 和 DELETE 的索引標籤中已經有選取這些方法。 這是由於我們使用`DataObjectMethodAttribute`裝飾的方法`ProducstBLL`。 例如，DeleteProduct 方法具有下列簽章：


[!code-vb[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample2.vb)]

`DataObjectMethodAttribute`屬性會指出每個方法的目的，它是適用於選取、 插入、 更新或刪除及它是否不是預設值。 如果建立 BLL 類別時，您可以省略這些屬性，您將需要手動更新中，選取方法插入和刪除索引標籤。

在確定適當`ProductsBLL`方法對應到 ObjectDataSource `Insert()`， `Update()`，和`Delete()`方法中，按一下 [完成] 以完成精靈。

## <a name="examining-the-objectdatasources-markup"></a>檢查 ObjectDataSource 標記

設定之後透過其精靈 ObjectDataSource，移至來源檢視來檢查產生的宣告式標記。 `<asp:ObjectDataSource>`標記指定的基礎物件和叫用的方法。 此外，還有`DeleteParameters`， `UpdateParameters`，和`InsertParameters`會對應到輸入參數`ProductsBLL`類別的`AddProduct`， `UpdateProduct`，和`DeleteProduct`方法：


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample3.aspx)]

ObjectDataSource 包含參數的每個輸入參數為其相關聯的方法，就像一份`SelectParameter`s 時的存在 ObjectDataSource 呼叫選取的方法需要輸入的參數的設定時 (例如`GetProductsByCategoryID(categoryID)`). 我們稍後將會看到這些值`DeleteParameters`， `UpdateParameters`，和`InsertParameters`會自動設定的 GridView、 DetailsView 和 FormView 再叫用 ObjectDataSource `Insert()`， `Update()`，或`Delete()`方法。 這些值也可以設定以程式設計的方式如有需要因為在未來的教學課程中，我們將討論。

它的一個副作用使用精靈來設定 ObjectDataSource 是 Visual Studio 設定[Where 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx)至`original_{0}`。 這個屬性值用於包含正在編輯之資料的原始值，並在兩個案例中很有用：

- 如果編輯記錄，使用者就可以變更主索引鍵值。 在此情況下，新的主要金鑰值和原始主索引鍵值必須提供使原始的主索引鍵值的記錄可以找到並據以更新其值。
- 當使用開放式並行存取。 開放式並行存取是一種技術，確定兩個同時連接的使用者不會覆寫彼此的變更，以及主題未來教學課程。

`OldValuesParameterFormatString`屬性指示基礎物件的更新和刪除的原始值的方法中輸入參數的名稱。 我們將討論這個屬性，而且它的目的更詳細地探討開放式並行存取時。 我啟動它，不過，因為我們 BLL 方法不會預期的原始值，因此是很重要，我們會移除這個屬性。 離開`OldValuesParameterFormatString`屬性設定為預設值以外的任何項目 (`{0}`) 會造成錯誤，當資料 Web 控制項嘗試叫用 ObjectDataSource`Update()`或`Delete()`方法 ObjectDataSource 會因為嘗試在傳入`UpdateParameters`或`DeleteParameters`以及原始值參數所指定。

如果這不是那麼清楚在此時，別擔心，我們將檢驗這個屬性，而且其公用程式，在未來的教學課程。 現在，只是某些宣告式語法中完全移除這個屬性宣告，或將值設定為預設值 （{0}）。

> [!NOTE]
> 如果您只要清除`OldValuesParameterFormatString`屬性值從 [屬性] 視窗，在設計檢視中，屬性仍會存在於宣告式語法，但被設定為空字串。 這樣一來，不幸的是，仍會相同上面所討論的問題。 因此，移除屬性完全宣告式語法，或從 [屬性] 視窗中，將值設定為預設值， `{0}`。


## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>步驟 3： 加入資料 Web 控制項，並設定用於資料修改

一旦 ObjectDataSource 已加入至頁面，並設定，我們準備資料的 Web 控制項加入頁面，即可同時顯示的資料，並提供使用者才能修改它的方法。 我們會探討 GridView、 DetailsView 和 FormView 分開，因為其資料修改功能和組態中的 Web 控制項的這些資料不同。

我們會看到此篇文章的其餘部分將非常基本的編輯、 插入和刪除的 GridView DetailsView，透過支援以及 FormView 控制是真的越簡單越檢查幾個核取方塊。 有許多微妙和真實世界中的邊緣案例，請提供更為複雜，只要點，然後按一下比這類功能。 本教學課程中，不過，完全著重證明最簡單的資料修改功能。 未來的教學課程會檢查毋庸置疑地，在真實世界設定將會發生的問題。

## <a name="deleting-data-from-the-gridview"></a>在 GridView 中刪除資料

開始從工具箱拖曳至設計工具拖曳 GridView。 接下來，將繫結 ObjectDataSource 至 GridView 從 GridView 的智慧標籤在下拉式清單中選取它。 此時會是 GridView 的宣告式標記：


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample4.aspx)]

繫結至其智慧標籤透過 ObjectDataSource 的 GridView 有兩個好處：

- BoundFields 和 CheckBoxFields 會自動建立每個 ObjectDataSource 所傳回的欄位。 此外，BoundField 和 CheckBoxField 的屬性會根據設定的基礎欄位中繼資料。 例如， `ProductID`， `CategoryName`，和`SupplierName`欄位標示為以唯讀狀態中`ProductsDataTable`，因此不應該是可更新編輯時。 若要配合此，這些 BoundFields' [ReadOnly 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx)設為`True`。
- [DataKeyNames 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx)指派給基礎物件的主索引鍵欄位。 這是必要時使用 GridView 編輯或刪除資料，因為在進行這個屬性會指出欄位 （或一組欄位），唯一識別每一筆記錄。 如需有關`DataKeyNames`屬性，會回頭[主要/詳細說明可選取的主要 GridView 使用詳細資料 DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)教學課程。

雖然 GridView 可透過 [屬性] 視窗或宣告式語法 ObjectDataSource 繫結，這樣需要您手動加入適當的 BoundField 和`DataKeyNames`標記。

GridView 控制項提供資料列層級編輯和刪除的內建支援。 設定支援刪除的 GridView 加入刪除按鈕的資料行。 當使用者按一下特定的資料列的 [刪除] 按鈕時，回傳展示和 GridView 執行下列步驟：

1. ObjectDataSource`DeleteParameters`指派值
2. ObjectDataSource`Delete()`叫用方法時，刪除指定的記錄
3. 在 GridView 重新繫結本身 ObjectDataSource 來叫用其`Select()`方法

值指派給`DeleteParameters`的值`DataKeyNames`欄位的 [刪除] 按鈕已按下的資料列。 因此就很重要的 GridView`DataKeyNames`屬性正確設定。 如果遺失`DeleteParameters`將值指派給`Nothing`在步驟 1 中，這又不會導致在步驟 2 中任何已刪除的記錄。

> [!NOTE]
> `DataKeys`集合會儲存在 GridView 的控制項狀態，表示`DataKeys`值會記住在回傳，即使已停用 GridView 的檢視狀態。 不過，它是非常重要的檢視狀態的支援編輯或刪除 （預設行為） 的 GridViews 保持啟用。 如果您將 GridView s`EnableViewState`屬性`false`、 編輯和刪除行為會正常顯示為單一使用者，但如果沒有刪除資料的並行使用者，有這些並行的使用者可能會不小心的可能性刪除或編輯的記錄，確定它們 professionals t 想。 請參閱我部落格文章：[警告： 已停用並行問題與 ASP.NET 2.0 GridViews/DetailsView/FormViews 該支援編輯和/或刪除與的檢視狀態](http://scottonwriting.net/sowblog/posts/10054.aspx)，如需詳細資訊。


這個相同警告亦適用於 DetailsViews 和 FormViews。

若要新增的 GridView 刪除功能，只需移至其智慧標籤和選取啟用刪除的核取方塊。


![核取 啟用刪除核取方塊](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image24.png)

**圖 10**： 核取 啟用刪除核取方塊


檢查啟用刪除 核取方塊的智慧標籤將 CommandField 加入至 GridView。 CommandField 轉譯按鈕，執行一或多個下列工作的 GridView 中的資料行： 選取資料錄、 編輯記錄，以及刪除記錄。 我們先前所見以選取資料錄中的動作中 CommandField[主要/詳細說明可選取的主要 GridView 使用詳細資料 DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)教學課程。

CommandField 包含數個`ShowXButton`指出哪些系列的按鈕會顯示在 CommandField 屬性。 藉由檢查啟用刪除核取方塊 CommandField 其`ShowDeleteButton`屬性是`True`已新增至 GridView 的資料行集合。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample5.aspx)]

此時，信不，我們已順利完成刪除支援新增至 GridView ！ 圖 11 顯示，因為瀏覽透過瀏覽器的刪除按鈕資料行的這個頁面出現時。


[![CommandField 加入資料行的刪除按鈕](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image25.png)

**圖 11**: CommandField 加入資料行的刪除按鈕 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image27.png))


如果您建立本教學課程從無到有自己，在測試此頁面，按一下 [刪除] 按鈕就會引發例外狀況。 請繼續閱讀，若要了解這些例外狀況發生原因和如何加以修正。

> [!NOTE]
> 如果您要遵照沿著使用下載隨附此教學課程中，這些問題有已經尚未指明可以被。 不過，建議您閱讀下面所列，以協助識別可能發生的問題和適當的因應措施的詳細資料。


如果您嘗試刪除產品，您會取得例外狀況的訊息會類似於 「*ObjectDataSource 'ObjectDataSource1' 找不到非泛型方法 'DeleteProduct' 具有參數： productID，原始\_ProductID*，「 您可能忘記移除`OldValuesParameterFormatString`從 ObjectDataSource 屬性。 與`OldValuesParameterFormatString`指定屬性，傳入同時嘗試 ObjectDataSource`productID`和`original_ProductID`輸入參數來`DeleteProduct`方法。 `DeleteProduct`不過，只接受單一輸入的參數，因此例外狀況。 移除`OldValuesParameterFormatString`屬性 (或將它設定為`{0}`) 會指示要嘗試的原始的輸入參數中傳遞 ObjectDataSource。


[![確認已清除 參數屬性](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image28.png)

**圖 12**： 請確定`OldValuesParameterFormatString`屬性已被清除 Out ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image30.png))


即使您已移除`OldValuesParameterFormatString`屬性，將時仍然出現例外狀況嘗試刪除的產品與訊息: 「*與參考條件約束衝突的 DELETE 陳述式 ' FK\_順序\_詳細資料\_產品的*。 」Northwind 資料庫所包含的外部索引鍵條件約束之間`Order Details`和`Products`資料表，這表示從系統無法刪除的產品，如果有一或多個記錄中的`Order Details`資料表。 因為 Northwind 資料庫中的每一個產品有至少一筆記錄`Order Details`，我們先刪除產品的相關聯的訂單詳細資料記錄之前，我們無法刪除任何產品。


[![外部索引鍵條件約束會禁止刪除產品](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image31.png)

**圖 13**： 的 Foreign Key 條件約束會禁止刪除產品 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image33.png))


教學課程中，我們只要刪除所有記錄的`Order Details`資料表。 在真實世界應用程式中，我們需要為：

- 有另一個螢幕，來管理訂單詳細資料資訊
- 加強`DeleteProduct`方法，將包含邏輯，可刪除指定的產品訂單詳細資料
- 修改 SQL 查詢的 TableAdapter 用於包含刪除指定的產品訂單詳細資料

讓我們只要刪除所有記錄的`Order Details`規避 foreign key 條件約束的資料表。 請移至 Visual Studio 中的 [伺服器總管]，以滑鼠右鍵按一下`NORTHWND.MDF`] 節點，然後選擇 [新的查詢。 然後，在查詢視窗中，執行下列 SQL 陳述式： `DELETE FROM [Order Details]`


[![從訂單詳細資料資料表刪除所有記錄](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image34.png)

**圖 14**： 刪除所有記錄`Order Details`資料表 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image36.png))


在之後清除`Order Details`按一下 [刪除] 按鈕上的資料表將會刪除產品不會發生錯誤。 如果按一下 [刪除] 按鈕不會刪除產品，請檢查以確保 GridView 的`DataKeyNames`屬性設定為主要索引鍵欄位 (`ProductID`)。

> [!NOTE]
> 按一下 [刪除] 按鈕時回傳展示並刪除該記錄。 這可能十分危險，因為很容易就不小心按的錯誤資料列的 [刪除] 按鈕。 在未來的教學課程中我們會看到如何將用戶端確認刪除記錄時。


## <a name="editing-data-with-the-gridview"></a>編輯 GridView 的資料

以及刪除 GridView 控制項也提供內建的資料列層級編輯的支援。 設定為支援編輯 GridView 加入編輯按鈕的資料行。 從使用者的觀點來看，按一下變成可編輯的資料列的資料列的編輯按鈕原因將開啟到包含現有的值，取代更新，[編輯] 按鈕和 [取消] 按鈕的文字方塊的資料格。 在其所需的變更後，終端使用者可以按一下以認可變更的 [更新] 按鈕或 [取消] 按鈕，以捨棄它們。 在任一情況下後按一下 [更新] 或 [取消] 5d; GridView 回到其預先編輯狀態。

從我們的觀點來看，為網頁開發人員，當使用者按一下特定的資料列的 [編輯] 按鈕，回傳展示和 GridView 執行下列步驟：

1. GridView`EditItemIndex`屬性指派給其編輯 按鈕已按下資料列的索引
2. 在 GridView 重新繫結本身 ObjectDataSource 來叫用其`Select()`方法
3. 比對的資料列索引`EditItemIndex`呈現在 「 編輯模式 」。 此模式中，編輯 按鈕會被取代更新 和 取消 5d; 按鈕和 BoundFields 其`ReadOnly`屬性為 False （預設值） 會轉譯文字方塊的 Web 控制項的`Text`屬性指派給資料欄位的值。

此時標記會傳回給瀏覽器中，允許使用者變更資料列的資料。 當使用者按一下 [更新] 按鈕時，就會發生回傳和 GridView 執行下列步驟：

1. ObjectDataSource`UpdateParameters`值會指派 GridView 的編輯介面中輸入使用者的值
2. ObjectDataSource`Update()`叫用方法時，更新指定的記錄
3. 在 GridView 重新繫結本身 ObjectDataSource 來叫用其`Select()`方法

主索引鍵值指派給`UpdateParameters`在步驟 1 中來自中指定的值`DataKeyNames`屬性，而非主索引鍵值來自中編輯資料列的文字方塊中的 Web 控制項的文字。 為刪除，很重要的 GridView`DataKeyNames`屬性正確設定。 如果遺失`UpdateParameters`主索引鍵值將值指派給`Nothing`在步驟 1 中，這又不會導致在步驟 2 中的任何更新記錄。

藉由只檢查 GridView 的智慧標籤的 啟用編輯核取方塊，就可以啟動編輯功能。


![核取 [啟用編輯] 核取方塊](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image37.png)

**圖 15**： 核取 [啟用編輯] 核取方塊


檢查 [啟用編輯] 核取方塊，將 CommandField （如有需要） 並設定其`ShowEditButton`屬性`True`。 如稍早所見，CommandField 包含數個`ShowXButton`指出哪些系列的按鈕會顯示在 CommandField 屬性。 檢查 [啟用編輯] 核取方塊新增`ShowEditButton`現有 CommandField 屬性：


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample6.aspx)]

這就是新增基本編輯的支援。 粗糙編輯介面是 Figure16 所示，每個 BoundField 其`ReadOnly`屬性設定為`False`（預設值） 會轉譯為文字方塊。 這包括欄位`CategoryID`和`SupplierID`，這是與其他資料表的索引鍵。


[![按一下 [Chai 的編輯] 按鈕顯示的資料列處於編輯模式](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image38.png)

**圖 16**： 按一下 Chai 的編輯 按鈕以編輯模式中顯示的資料列 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image40.png))


除了詢問使用者直接編輯外部索引鍵值，編輯介面的介面缺少下列方式：

- 如果使用者輸入`CategoryID`或`SupplierID`不在資料庫中，存在`UPDATE`將違反的外部索引鍵條件約束，造成引發例外狀況。
- 編輯介面不包含任何驗證。 如果您未提供必要的值 (例如`ProductName`)，或輸入一個數字值 （例如，輸入 「 太多 ！"的預期的字串值 到`UnitPrice`文字方塊)，將會擲回例外狀況。 未來的教學課程將會檢驗如何加入驗證控制項來編輯的使用者介面。
- 目前，*所有*不是唯讀的產品欄位必須包含在 GridView。 如果我們從 GridView 移除欄位，說出`UnitPrice`，當更新在 GridView 的資料就不應設定`UnitPrice``UpdateParameters`值，也可能會變更資料庫記錄`UnitPrice`至`NULL`值。 同樣地，如果必要的欄位，例如`ProductName`，會移除從 GridView，更新將會失敗的相同 「*資料行 'ProductName' 不允許 null*"上面提到的例外狀況。
- 編輯介面格式讓人滿意。 `UnitPrice`顯示四個小數位數。 在理想情況下`CategoryID`和`SupplierID`值會包含在系統中列出的類別目錄與供應商的 DropDownLists。

這些是所有的缺點，我們必須具備即時現在，但將在未來的教學課程中獲得解決。

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>插入、 編輯和刪除資料與 DetailsView

如同我們在先前的教學課程中，DetailsView 控制項一次和 like GridView 會顯示一筆記錄，允許編輯和刪除目前所顯示的記錄。 這兩個使用者的經驗與編輯和刪除項目從 DetailsView 和從 ASP.NET 端工作流程是相同的 GridView。 在 DetailsView 與 GridView 不同是它也提供內建插入的支援。

為了示範 GridView 的資料修改功能，開始請先新增至 DetailsView`Basics.aspx`現有 GridView 上方頁面上，並將它繫結至現有的 ObjectDataSource 透過 DetailsView 的智慧標籤。 下一步]，清除 DetailsView 的`Height`和`Width`屬性，並檢查智慧標籤的 [啟用分頁選項。 若要啟用編輯，插入和刪除的支援，只會檢查啟用編輯、 啟用插入和刪除啟用核取方塊中的智慧標籤。


![設定為支援編輯、 插入和刪除 DetailsView](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image41.png)

**圖 17**： 設定為支援編輯、 插入和刪除 DetailsView


做為 GridView 中加入編輯、 插入或刪除支援新增 CommandField detailsview，如下所示的宣告式語法：


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample7.aspx)]

請注意，為 DetailsView CommandField 依預設會出現在資料行集合的結尾。 在 DetailsView 的欄位會轉譯為資料列，CommandField 會顯示為一個資料列具有插入，因為編輯和刪除 DetailsView 底部的按鈕。


[![設定為支援編輯、 插入和刪除 DetailsView](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image42.png)

**圖 18**： 設定來支援編輯、 插入和刪除 DetailsView ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image44.png))


按一下 [刪除] 按鈕啟動事件的相同順序，如同 GridView： 的回傳。後面接著填入其 ObjectDataSource DetailsView`DeleteParameters`根據`DataKeyNames`值;，並已完成，但在呼叫其 ObjectDataSource`Delete()`方法，從資料庫中實際移除產品。 編輯 DetailsView 中也能運作，以相同的 GridView 的方式。

插入，使用者會看見新按鈕，按一下時，會呈現在 DetailsView 中 「 插入模式 」。 插入模式 使用 新增 按鈕和會取代插入 和 取消 5d; 按鈕只有那些 BoundFields 其`InsertVisible`屬性設定為`True`（預設值） 會顯示。 識別做為自動遞增的欄位，例如那些資料欄位`ProductID`，有其[InsertVisible 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx)設`False`繫結至資料來源透過智慧標籤的 DetailsView 時。

當繫結的 DetailsView 透過智慧標籤的資料來源，Visual Studio 設定`InsertVisible`屬性`False`只適用於自動遞增的欄位。 唯讀欄位，例如`CategoryName`和`SupplierName`，將會顯示在 「 插入模式 」 使用者介面中，除非其`InsertVisible`屬性明確設定為`False`。 請花一點時間來設定這兩個欄位`InsertVisible`屬性`False`，透過在 DetailsView 的宣告式語法或透過編輯的欄位中的智慧標籤連結。 圖 19 顯示設定`InsertVisible`屬性`False`編輯欄位上按一下連結。


[![Northwind Traders 現在提供 Acme 茶杯](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image45.png)

**圖 19**: Northwind Traders 現在提供 Acme 茶杯 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image47.png))


設定之後`InsertVisible`屬性、 檢視`Basics.aspx`網頁瀏覽器中，按一下 [新增] 按鈕。 圖 20 會顯示在 DetailsView 加入新的飲料時, Acme 茶杯，到我們的產品線。


[![Northwind Traders 現在提供 Acme 茶杯](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image48.png)

**圖 20**: Northwind Traders 現在提供 Acme 茶杯 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image50.png))


回傳展示之後 Acme 茶杯的詳細資料的輸入，然後按一下 [插入] 按鈕，並加入新的記錄`Products`資料庫資料表。 因為此 DetailsView 按照順序列出產品與其存在於資料庫資料表，我們必須頁面上的最後一個產品才能看到新的產品。


[![Acme 茶杯的詳細資料](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image51.png)

**圖 21**: Acme 茶杯的詳細資料 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image53.png))


> [!NOTE]
> 在 DetailsView 的[CurrentMode 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx)指出所顯示的介面，而且可以是下列值之一： `Edit`， `Insert`，或`ReadOnly`。 [DefaultMode 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx)指出的模式 DetailsView 返回編輯之後，或插入已經完成，並且適用於顯示的 DetailsView 永久處於編輯，或插入模式。


的點，並按一下插入及編輯 DetailsView 的功能會受困於從 GridView 相同的限制： 使用者必須輸入現有`CategoryID`和`SupplierID`到文字方塊中的值; 介面缺少任何驗證邏輯中，不允許的產品欄位`NULL`值沒有預設值或在資料庫層級指定的值必須包含在插入介面，依此類推。

我們將來擴充和增強 GridView 的編輯介面在未來的發行項可以套用至 DetailsView 控制項的編輯和插入介面以及檢視技術。

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>在 FormView 使用更具彈性的資料修改使用者介面

在 FormView 提供內建支援插入、 編輯和刪除資料，但因為它會使用範本，而不是欄位沒有將 BoundFields 或 CommandField GridView 和 DetailsView 控制項用來提供的資料位置修改介面。 相反地，此介面來收集使用者的 Web 控制項加入新項目時所輸入或編輯現有的 fgpp 以及 [新增]，編輯、 刪除、 插入、 更新和取消按鈕必須手動新增至適當的範本。 幸運的是，當繫結至資料來源透過下拉式清單中的智慧標籤的 FormView Visual Studio 會自動建立所需的介面。

為了說明這些技術，開始請先新增 FormView`Basics.aspx`頁面上，並在 FormView 的智慧標籤上，從繫結到已建立 ObjectDataSource。 這會產生`EditItemTemplate`， `InsertItemTemplate`，和`ItemTemplate`收集新增使用者的輸入和按鈕 Web 控制項的 TextBox Web 控制項與 FormView，編輯、 刪除、 插入、 更新以及 [取消] 按鈕。 此外，在 FormView 的`DataKeyNames`屬性設定為主要索引鍵欄位 (`ProductID`) ObjectDataSource 所傳回的物件。 最後，檢查在 FormView 的智慧標籤的 [啟用分頁] 選項。

下圖顯示的宣告式標記為在 FormView 的`ItemTemplate`FormView 已經繫結至 ObjectDataSource 之後。 根據預設，每個非布林值的產品欄位繫結至`Text`時每個布林值欄位的標籤 Web 控制項的屬性 (`Discontinued`) 繫結至`Checked`已停用核取方塊 Web 控制項的屬性。 為了新增、 編輯和刪除按鈕來觸發特定 FormView 行為，在按下時，務必，其`CommandName`值設定為`New`， `Edit`，和`Delete`分別。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample8.aspx)]

圖 22 顯示在 FormView 的`ItemTemplate`透過瀏覽器檢視時。 每個產品欄位會列出在底部的新增、 編輯和刪除按鈕。


[![預設 FormView ItemTemplate 列出每個產品欄位，以及新增、 編輯和刪除按鈕](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image54.png)

**圖 22**: 預設 FormView`ItemTemplate`列出每個產品欄位沿著以新增、 編輯和刪除 按鈕 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image56.png))


例如 GridView 和 DetailsView，按一下 [刪除] 按鈕，或任何按鈕、 LinkButton 或 ImageButton 其`CommandName`屬性是設定為刪除，會導致回傳，並填入 ObjectDataSource`DeleteParameters`根據在 FormView 的`DataKeyNames`值，而且會叫用 ObjectDataSource`Delete()`方法。

按一下 [編輯] 按鈕時回傳展示和資料重新繫結至`EditItemTemplate`，這是負責呈現編輯介面。 這個介面包含 Web 控制項編輯資料，以及更新 和 取消 5d; 按鈕。 預設值`EditItemTemplate`產生的 Visual Studio 包含任何自動遞增的欄位標籤 (`ProductID`)、 的文字方塊中，每個非布林值欄位，以及每個布林值欄位的核取方塊。 此行為是非常類似於自動產生 BoundFields GridView 和 DetailsView 控制項中。

> [!NOTE]
> 在 FormView 的自動產生的一個小問題`EditItemTemplate`是它呈現文字方塊中的 Web 控制項是唯讀，例如這些欄位`CategoryName`和`SupplierName`。 我們會看到如何將此列入考量很快。


TextBox 控制項中`EditItemTemplate`有其`Text`屬性繫結至其對應的資料欄位使用的值*雙向資料繫結*。 雙向資料繫結，以表示`<%# Bind("dataField") %>`，執行資料繫結這兩個範本繫結資料時，並填入 ObjectDataSource 參數插入或編輯資料錄。 也就是說，當使用者按一下 [編輯] 按鈕時，才`ItemTemplate`、`Bind()`方法會傳回指定的資料欄位值。 使用者進行變更，並按一下 更新之後，這些值回傳對應到使用指定的資料欄位`Bind()`會套用到 ObjectDataSource `UpdateParameters`。 單向資料繫結，或者，以表示`<%# Eval("dataField") %>`，只會將資料繫結至範本時所擷取的資料欄位值，並*不*返回資料來源的參數中的使用者輸入的值，在回傳。

下列宣告式標記會顯示在 FormView 的`EditItemTemplate`。 請注意，`Bind()`方法用於中的資料繫結語法，並更新和取消按鈕 Web 控制項有其`CommandName`隨之設定的屬性。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample9.aspx)]

我們`EditItemTemplate`，這點，如果我們嘗試使用它擲回例外狀況。 問題在於`CategoryName`和`SupplierName`TextBox Web 控制項中，會呈現欄位`EditItemTemplate`。 我們可能需要變更這些文字方塊的標籤或完全移除它們。 讓我們只要刪除它們完全從`EditItemTemplate`。

圖 23 瀏覽器中顯示 FormView 之後會在按下 Chai [編輯] 按鈕。 請注意，`SupplierName`和`CategoryName`欄位中顯示`ItemTemplate`不再存在，因為我們只移除從`EditItemTemplate`。 按一下 更新 按鈕時 FormView 就會繼續進行 GridView 和 DetailsView 控制項相同的步驟序列。


[![依預設，EditItemTemplate 顯示每個可編輯的產品欄位文字方塊或核取方塊](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image57.png)

**圖 23**： 根據預設`EditItemTemplate`顯示每個可編輯產品欄位做為文字方塊或核取方塊 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image59.png))


當按一下 [插入] 按鈕在 FormView 的`ItemTemplate`回傳展示。 不過，沒有資料繫結至 FormView 因為正在加入新的記錄。 `InsertItemTemplate`介面包含 Web 控制項，加入新的記錄，以及 [插入] 和 [取消] 5d; 按鈕。 預設值`InsertItemTemplate`產生的 Visual Studio 包含每個非布林值欄位的文字方塊和核取方塊，每個布林值欄位，類似於自動產生`EditItemTemplate`的介面。 TextBox 控制項有其`Text`屬性繫結使用雙向資料繫結及其對應資料欄位的值。

下列宣告式標記會顯示在 FormView 的`InsertItemTemplate`。 請注意，`Bind()`方法用於的資料繫結語法，而插入和取消按鈕 Web 控制項有其`CommandName`隨之設定的屬性。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample10.aspx)]

沒有在 FormView 的自動產生的微妙的地方`InsertItemTemplate`。 即使是唯讀，例如這些欄位的文字方塊中的 Web 控制項建立的具體來說，`CategoryName`和`SupplierName`。 例如`EditItemTemplate`，我們要移除這些文字方塊從`InsertItemTemplate`。

圖 24 顯示 FormView 瀏覽器中，加入新的產品，Acme 咖啡時。 請注意，`SupplierName`和`CategoryName`欄位中顯示`ItemTemplate`不再存在，因為我們只移除。 [插入] 按鈕按一下時透過 DetailsView 控制項相同的步驟序列 FormView 會繼續進行，加入新的記錄，到`Products`資料表。 圖 25 FormView 中顯示 Acme 咖啡產品詳細資料之後已插入。


[![上規定在 FormView 的插入介面](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image60.png)

**圖 24**:`InsertItemTemplate`規定在 FormView 的插入介面 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image62.png))


[![在 FormView 中會顯示新的產品，Acme 咖啡詳細資料](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image63.png)

**圖 25**： 新的產品，Acme 咖啡的詳細資料會顯示在 FormView ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image65.png))


藉由分隔的唯讀時，編輯，並將介面插入三個不同的範本，在 FormView 允許比 DetailsView 和 GridView 更好的控制這些介面。

> [!NOTE]
> 要在 DetailsView，在 FormView 的`CurrentMode`屬性會指出所顯示的介面和其`DefaultMode`屬性指出模式 FormView 返回編輯之後，或插入已經完成。


## <a name="summary"></a>總結

在本教學課程，我們會檢查插入、 編輯和刪除資料使用 GridView、 DetailsView 和在 FormView 的基本概念。 所有這三種控制項提供某種程度的內建的資料修改功能可以用不需要撰寫一行程式碼，這點受惠 Web 控制項的資料和 ObjectDataSource ASP.NET 網頁中。 不過，簡單點，然後按一下 相當身軀技術呈現和貝氏資料修改使用者介面。 若要提供的驗證，將以程式設計方式的值、 順暢地處理例外狀況、 自訂使用者介面，和等等，我們需要依賴 first 技術，將在下一步的幾個教學課程會討論。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

> [!div class="step-by-step"]
> [上一頁](limiting-data-modification-functionality-based-on-the-user-cs.md)
> [下一頁](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
