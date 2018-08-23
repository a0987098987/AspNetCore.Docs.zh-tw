---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
title: 概觀插入、 更新和刪除資料 (C#) |Microsoft Docs
author: rick-anderson
description: 在本教學課程中，我們會看到如何將 ObjectDataSource 的 insert （），update （），對應和方法的 BLL delete （） 方法的類別，以及如何組態...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: b651dc58-93c7-4f83-a74e-3b99f6d60848
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 82f1127b01c211a2af91623d4df7ca10dcad6d8a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833466"
---
<a name="an-overview-of-inserting-updating-and-deleting-data-c"></a>插入、 更新和刪除資料 (C#) 的概觀
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_CS.exe)或[下載 PDF](an-overview-of-inserting-updating-and-deleting-data-cs/_static/datatutorial16cs1.pdf)

> 在本教學課程中我們將了解如何將 ObjectDataSource 的 insert （），update （），對應，方法的 BLL delete （） 方法的類別，以及如何設定 GridView、 DetailsView 和 FormView 控制項，可提供資料修改功能。


## <a name="introduction"></a>簡介

移轉過去的幾個教學課程中，我們已討論過如何在 ASP.NET 網頁使用 GridView、 DetailsView 和 FormView 控制項中顯示資料。 這些控制項只會使用提供給它們的資料。 通常，這些控制項存取透過 ObjectDataSource 等資料來源控制項使用的資料。 我們已了解如何將 ObjectDataSource 做為 ASP.NET 網頁和基礎資料之間的 proxy。 當 GridView 需要顯示資料時，它會叫用其 ObjectDataSource`Select()`方法，它會接著叫用的方法從我們商務邏輯層 (BLL)，會呼叫的方法，在適當的資料存取圖層 (DAL) 中的 TableAdapter，繼而再傳送`SELECT` Northwind 資料庫的查詢。

請注意，當我們在 DAL 中建立 TableAdapters[我們的第一個教學課程](../introduction/creating-a-data-access-layer-cs.md)、 Visual Studio 會自動加入方法來插入、 更新和刪除的資料，從基礎資料庫資料表。 此外，在[建立商業邏輯層](../introduction/creating-a-business-logic-layer-cs.md)我們設計方法呼叫向下到 BLL 中的用到這些資料修改 DAL 方法。

除了其`Select()`方法，也有 ObjectDataSource `Insert()`， `Update()`，和`Delete()`方法。 例如`Select()`方法，這三種方法可以對應至基礎物件中的方法。 當設定為插入、 更新或刪除資料，GridView、 DetailsView 和 FormView 控制項提供使用者介面修改基礎資料。 此使用者介面會呼叫`Insert()`， `Update()`，和`Delete()`方法的 ObjectDataSource]，然後叫用基礎物件的相關聯 （請參閱 [圖 1） 的方法。


[![ObjectDataSource 的 insert （）、 update （） 和 delete （） 方法做為 Proxy 到 BLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image1.png)

**圖 1**: ObjectDataSource `Insert()`， `Update()`，和`Delete()`方法做為到 BLL Proxy ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image3.png))


在本教學課程中，我們會看到如何將對應的 ObjectDataSource `Insert()`， `Update()`，和`Delete()`BLL，以及如何設定 GridView、 DetailsView 和 FormView 控制項來提供資料修改類別的方法的方法功能。

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>步驟 1： 建立 Insert、 Update 和 Delete 的教學課程的網頁

我們開始探索如何插入、 更新和刪除資料之前，讓我們先花一點時間在本教學課程的下一步 的數個工具，我們將需要我們網站專案中建立 ASP.NET 網頁。 藉由新增新的資料夾，名為啟動`EditInsertDelete`。 接下來，新增到該資料夾，並確認其關聯與每個頁面的 下列 ASP.NET 網頁`Site.master`主版頁面：

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![加入 ASP.NET 網頁的資料修改相關教學課程](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image4.png)

**圖 2**： 加入 ASP.NET 網頁的資料修改相關教學課程


在其他資料夾，例如`Default.aspx`在`EditInsertDelete`資料夾會列出其一節中的教學課程。 請記得，`SectionLevelTutorialListing.ascx`使用者控制項提供這項功能。 因此，新增此使用者控制項`Default.aspx`藉由將它拖曳到頁面的 [設計] 檢視上的 [方案總管] 中。


[![將 SectionLevelTutorialListing.ascx 使用者控制項新增至 Default.aspx](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image5.png)

**圖 3**： 新增`SectionLevelTutorialListing.ascx`使用者控制項`Default.aspx`([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image7.png))


最後，將頁面新增項目，以作為`Web.sitemap`檔案。 具體來說，自訂格式化之後新增下列標記`<siteMapNode>`:


[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample1.xml)]

在更新之後`Web.sitemap`，花點時間檢視教學課程網站，透過瀏覽器。 在左側功能表現在包含編輯、 插入及刪除教學課程的項目。


![網站導覽現在包含項目編輯、 插入及刪除教學課程](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image8.png)

**圖 4**： 網站地圖現在包含項目編輯、 插入及刪除教學課程


## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>步驟 2： 加入和設定 ObjectDataSource 控制項

因為 GridView、 DetailsView 和 FormView 其資料修改功能和版面配置的每個不同，讓我們檢查每個個別。 而不是有使用它自己的 ObjectDataSource 的每個控制項，不過，我們就來建立所有的三個控制項範例可以共用單一 ObjectDataSource。

開啟`Basics.aspx`頁面上，從 [工具箱] 拖曳至設計工具中，拖曳 ObjectDataSource，然後按一下 [設定資料來源] 連結，從它的智慧標籤。 因為`ProductsBLL`是唯一提供編輯、 插入和刪除方法，設定要使用這個類別的 ObjectDataSource 的 BLL 類別。


[![設定使用 ProductsBLL 類別 ObjectDataSource](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image9.png)

**圖 5**： 設定要使用 ObjectDataSource`ProductsBLL`類別 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image11.png))


在下一個畫面中，我們可以指定哪些方法的`ProductsBLL`類別會對應到 ObjectDataSource `Select()`， `Insert()`， `Update()`，和`Delete()`選取適當的索引標籤，然後從下拉式清單中選擇的方法。 圖 6，這應該看起來很熟悉到目前為止，對應的 ObjectDataSource`Select()`方法，以`ProductsBLL`類別的`GetProducts()`方法。 `Insert()`， `Update()`，和`Delete()`來設定方法，請從上方清單中選取適當的索引標籤。


[![ObjectDataSource 已傳回的所有產品](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image12.png)

**圖 6**： 有 ObjectDataSource 傳回所有的產品 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image14.png))


圖 7、 8 和 9 顯示 ObjectDataSource 的更新、 插入和刪除索引標籤。 設定這些索引標籤，讓`Insert()`， `Update()`，並`Delete()`方法叫用`ProductsBLL`類別的`UpdateProduct`， `AddProduct`，和`DeleteProduct`方法，分別。


[![將 ObjectDataSource 的 update （） 方法對應至 ProductBLL 類別的 UpdateProduct 方法](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image15.png)

**圖 7**： 對應的 ObjectDataSource`Update()`方法來`ProductBLL`類別的`UpdateProduct`方法 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image17.png))


[![將 ObjectDataSource 的 insert （） 方法對應至 ProductBLL 類別的 AddProduct 方法](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image18.png)

**圖 8**： 對應的 ObjectDataSource`Insert()`方法來`ProductBLL`類別的 Add`Product`方法 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image20.png))


[![將 ObjectDataSource 的 delete （） 方法對應至 ProductBLL 類別的 DeleteProduct 方法](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image21.png)

**圖 9**： 對應的 ObjectDataSource`Delete()`方法來`ProductBLL`類別的`DeleteProduct`方法 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image23.png))


您可能已經注意到在更新、 插入和刪除索引標籤的下拉式清單中已經有這些選取的方法。 這是因為我們使用`DataObjectMethodAttribute`裝飾的方法`ProducstBLL`。 比方說，DeleteProduct 方法具有下列簽章：


[!code-csharp[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample2.cs)]

`DataObjectMethodAttribute`屬性會指出每個方法的目的，不管它是用於選取、 插入、 更新或刪除和它是否不是預設值。 如果您省略這些屬性，建立您的 BLL 類別時，您將需要以手動方式更新，從選取的方法插入和刪除索引標籤。

在確定適當`ProductsBLL`方法會對應到 ObjectDataSource `Insert()`， `Update()`，和`Delete()`方法中，按一下 [完成] 以完成精靈。

## <a name="examining-the-objectdatasources-markup"></a>檢查 ObjectDataSource 的標記

設定之後透過其精靈 ObjectDataSource，移至來源檢視來檢查產生的宣告式標記。 `<asp:ObjectDataSource>`標記會指定基礎物件和方法來叫用。 此外，還有`DeleteParameters`， `UpdateParameters`，並`InsertParameters`會對應到輸入參數`ProductsBLL`類別的`AddProduct`， `UpdateProduct`，和`DeleteProduct`方法：


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample3.aspx)]

ObjectDataSource 包含參數的每一個輸入參數，為其相關聯的方法，就像一份`SelectParameter`s 是的存在 ObjectDataSource 呼叫選取的方法所預期的輸入的參數的設定時 (例如`GetProductsByCategoryID(categoryID)`). 誠如所見，這些值`DeleteParameters`， `UpdateParameters`，並`InsertParameters`會自動設定 GridView、 DetailsView 和 FormView 之前叫用的 ObjectDataSource `Insert()`， `Update()`，或`Delete()`方法。 這些值也可以設定以程式設計的方式如有需要我們將在未來的教學課程中討論。

使用精靈設定 ObjectDataSource 的一個副作用是，Visual Studio 會將[Where 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx)至`original_{0}`。 這個屬性值用來包含所編輯之資料的原始值，以及兩個案例中很有用：

- 如果當您編輯一筆記錄，使用者就能夠變更主索引鍵值的。 在此情況下，新的主要金鑰值和原始的主索引鍵值都必須提供，讓原始的主索引鍵值的記錄可以找到並據以更新其值。
- 當使用開放式並行存取。 開放式並行存取是一種技術，以確保兩個同時連接的使用者不會覆寫他人的變更，並為未來的教學課程中的主題。

`OldValuesParameterFormatString`屬性表示基礎物件的 update 和 delete 方法之原始值的輸入參數的名稱。 當我們瀏覽開放式並行存取時，我們會討論這個屬性和詳細說明其用途。 我啟動它，不過，因為我們的 BLL 方法不會預期的原始值，因此是很重要，我們會移除這個屬性。 離開`OldValuesParameterFormatString`屬性設定為預設值以外的任何項目 (`{0}`) 會造成錯誤，當資料 Web 控制項嘗試叫用的 ObjectDataSource`Update()`或`Delete()`方法因為 ObjectDataSource嘗試在傳遞`UpdateParameters`或`DeleteParameters`指定，以及原始值的參數。

如果這不是那麼清楚在此時，別擔心，我們將在未來的教學課程中檢查這個屬性和其公用程式。 目前，只為特定的宣告式語法中完全移除此屬性宣告，或將值設定為預設值 ({0})。

> [!NOTE]
> 如果您只需清除`OldValuesParameterFormatString`屬性值從 [屬性] 視窗，在 [設計] 檢視中，屬性仍會存在於宣告式語法中，但設定為空字串。 這樣一來，不幸的是，仍會相同上面所討論的問題。 因此，請移除屬性完全宣告式語法，或從 [屬性] 視窗中，將值設定為預設值， `{0}`。


## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>步驟 3： 加入資料 Web 控制項，並為資料修改設定

一旦已新增至頁面並設定 ObjectDataSource，我們就要資料 Web 控制項加入頁面，即可同時顯示資料，並提供方法，讓使用者進行修改。 我們將探討 GridView、 DetailsView 和 FormView 分開，因為這些資料 Web 控制項的差異在於其資料修改功能和組態。

因為我們會看到這篇文章其餘部分將非常基本的編輯、 插入及刪除支援 GridView、 DetailsView 和 FormView 控制項是很簡單，只要檢查幾個核取方塊。 有許多微妙之處和真實世界中的邊緣案例，讓提供更為複雜，比只是點，再按一下這類功能。 本教學課程中，不過，完全著重證明簡單的資料修改功能。 未來的教學課程將探討在真實世界設定中發生的問題。

## <a name="deleting-data-from-the-gridview"></a>從 GridView 中刪除資料

開始拖曳的 GridView，從 [工具箱] 拖曳至設計工具。 接下來，繫結 ObjectDataSource 至 GridView 藉由從 GridView 的智慧標籤中的下拉式清單中選取。 此時，系統將會是 GridView 的宣告式標記：


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample4.aspx)]

繫結至它的智慧標籤透過 ObjectDataSource 的 GridView 有兩個優點：

- 自動為每個欄位傳回的 ObjectDataSource 建立 BoundFields 與 CheckBoxFields。 此外，BoundField 及其的屬性會根據設定的基礎欄位中繼資料。 例如， `ProductID`， `CategoryName`，並`SupplierName`欄位會標示為唯讀模式中`ProductsDataTable`，因此不應該是可更新編輯時。 若要容納此，這些 BoundFields'[唯讀屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx)設定為`true`。
- [DataKeyNames 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx)指派給基礎物件的主索引鍵欄位。 這是不可或缺的時機為編輯或刪除資料，使用 GridView，因為這個屬性指出欄位 （或一組欄位），唯一識別每一筆記錄。 如需詳細資訊`DataKeyNames`屬性，請參閱上一步[主要/詳細說明使用具有詳細資料 detailview 之可選取主要 GridView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)教學課程。

GridView，都可以繫結至的 ObjectDataSource 透過 [屬性] 視窗或宣告式語法，雖然這種方式需要您手動加入適當的 BoundField 和`DataKeyNames`標記。

GridView 控制項提供資料列層級編輯和刪除的內建支援。 設定 GridView，以支援刪除加入刪除按鈕資料的行。 當使用者按一下特定的資料列的 [刪除] 按鈕時，回傳是兩邊彼此乾瞪眼和 GridView 會執行下列步驟：

1. ObjectDataSource 的`DeleteParameters`指派值
2. ObjectDataSource 的`Delete()`叫用方法時，刪除指定的記錄
3. GridView 重新繫結本身到 ObjectDataSource 叫用其`Select()`方法

若要指派的值`DeleteParameters`的值，`DataKeyNames`按下的 [刪除] 按鈕的資料列的欄位。 因此很重要的 GridView`DataKeyNames`正確設定屬性。 如果遺失`DeleteParameters`會指派`null`值在步驟 1，又不會導致任何已刪除的步驟 2 中的記錄。

> [!NOTE]
> `DataKeys`集合會儲存在 GridView 的控制項狀態，表示`DataKeys`值將會記住跨越回傳，即使已停用的 GridView 的檢視狀態。 不過，它是非常重要的檢視狀態的支援編輯或刪除 （預設行為） 的 Gridview 會維持啟用。 如果您將設定 GridView s`EnableViewState`屬性設`false`、 編輯和刪除行為將會正常運作的單一使用者，但如果沒有刪除資料的並行使用者，有可能發生這些並行的使用者可能不小心刪除或編輯記錄他們嘛 t 想。 請參閱我的部落格項目[警告： 已停用並行處理問題與 ASP.NET 2.0 Gridview/DetailsView/FormViews 該支援編輯和/或刪除與的 View State](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx)，如需詳細資訊。


這個相同的警告也適用於 DetailsViews 和 FormViews。

若要刪除的功能加入 GridView，只要移至它的智慧標籤，並核取方塊啟用刪除。


![核取 啟用刪除核取方塊](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image24.png)

**圖 10**： 核取 啟用刪除核取方塊


正在檢查啟用刪除 核取方塊的智慧標籤將 CommandField 加入至 GridView。 CommandField 呈現按鈕執行下列其中一個或多個下列工作與 GridView 中的資料行： 選取的記錄、 編輯記錄，並刪除記錄。 我們先前曾看過在選取資料錄中的動作中 CommandField[主要/詳細說明使用具有詳細資料 detailview 之可選取主要 GridView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)教學課程。

CommandField 包含數個`ShowXButton`指出哪些系列的按鈕會顯示在 CommandField 屬性。 核取啟用刪除 CommandField 其`ShowDeleteButton`屬性是`true`已新增至 GridView 的資料行集合。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample5.aspx)]

此時，信不信由您，我們已完成刪除的支援新增至 GridView ！ 如 [圖 11] 所示，瀏覽此頁面，透過瀏覽器的刪除按鈕資料行時出現。


[![CommandField 加入資料行的刪除按鈕](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image25.png)

**圖 11**: CommandField 加入資料行的刪除按鈕 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image27.png))


如果您建立本教學課程從頭自行測試此頁面，按一下 [刪除] 按鈕將會引發例外狀況。 繼續閱讀以了解為什麼引發這些例外狀況，以及如何加以修正。

> [!NOTE]
> 如果您有依照上述指示使用下載隨附本教學課程中，這些問題有已被佔用。 不過，建議您閱讀下面所列，以協助識別可能發生之問題和適當的因應措施的詳細資料。


如果您嘗試將刪除產品時，您會收到例外狀況的訊息會類似於 「*ObjectDataSource 'ObjectDataSource1' 找不到 přepisuje neobecnou metodu 具有參數的 ' DeleteProduct': productID，原始\_ProductID*，「 您可能忘了移除`OldValuesParameterFormatString`從 ObjectDataSource 的屬性。 與`OldValuesParameterFormatString`屬性指定，ObjectDataSource 會嘗試傳入兩者`productID`並`original_ProductID`輸入參數`DeleteProduct`方法。 `DeleteProduct`不過，只接受單一輸入的參數，因此例外狀況。 移除`OldValuesParameterFormatString`屬性 (或將它設定為`{0}`) 會嘗試不在原始的輸入參數中傳遞的 ObjectDataSource 的指示。


[![請確定已清除 [Where] 屬性](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image28.png)

**圖 12**： 請確認`OldValuesParameterFormatString`屬性已被清除 Out ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image30.png))


即使您已移除`OldValuesParameterFormatString`屬性，仍會得到例外狀況嘗試刪除具有訊息之產品的: 「*DELETE 陳述式與參考條件約束 ' FK\_順序\_詳細資料\_產品的*。 」Northwind 資料庫包含之間的外部索引鍵條件約束`Order Details`並`Products`資料表，這表示從系統無法刪除的產品，如果有一或多個記錄中的`Order Details`資料表。 因為 Northwind 資料庫中的每個產品具有至少一個記錄`Order Details`，直到我們第一次刪除產品的相關聯的訂單詳細資料記錄，所以無法刪除任何產品。


[![外部索引鍵條件約束會禁止刪除產品](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image31.png)

**圖 13**： 使用 Foreign Key 條件約束會禁止刪除的產品 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image33.png))


教學課程中，我們只是刪除所有記錄的`Order Details`資料表。 在真實世界應用程式中，我們必須為：

- 有另一個畫面來管理訂單詳細資料資訊
- 加強`DeleteProduct`方法來包含邏輯，以刪除指定的產品訂單詳細資料
- 修改 SQL 查詢的 TableAdapter 用於包含刪除指定的產品訂單詳細資料

讓我們只要刪除所有記錄的`Order Details`規避的外部索引鍵條件約束的資料表。 請移至 Visual Studio 中的 [伺服器總管] 中，以滑鼠右鍵按一下`NORTHWND.MDF`] 節點，然後選擇 [新增查詢。 然後，在 [查詢] 視窗中，執行下列 SQL 陳述式： `DELETE FROM [Order Details]`


[![從訂單明細資料表刪除所有記錄](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image34.png)

**圖 14**： 刪除所有記錄`Order Details`資料表 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image36.png))


之後清除`Order Details`資料表按一下 [刪除] 按鈕將會刪除不會發生錯誤的產品。 如果按一下 [刪除] 按鈕不會刪除產品，請檢查以確定的 GridView`DataKeyNames`屬性設定為主要索引鍵欄位 (`ProductID`)。

> [!NOTE]
> 按一下 [刪除] 按鈕時回傳是兩邊彼此乾瞪眼並刪除該記錄。 這可能會造成危險，因為很容易不小心按錯誤資料列的 [刪除] 按鈕。 在未來的教學課程中，我們會看到如何刪除記錄時，加入用戶端確認。


## <a name="editing-data-with-the-gridview"></a>編輯資料的 GridView

以及刪除 GridView 控制項也提供內建的資料列層級編輯支援。 設定 GridView，以支援編輯新增編輯按鈕資料的行。 從使用者的觀點來看，按一下變成可編輯的資料列的資料列的編輯按鈕原因轉換包含現有的值，並取代的更新，[編輯] 按鈕和 [取消] 按鈕的文字方塊中的資料格。 進行其所需的變更之後，使用者可以按一下以認可變更的 [更新] 按鈕或 [取消] 按鈕，以捨棄它們。 在任一情況下，按一下 [更新] 或 [取消] 之後 GridView 回到其預先編輯狀態。

從我們的觀點來看，為頁面開發人員，當使用者按一下特定的資料列的 [編輯] 按鈕，回傳是兩邊彼此乾瞪眼和 GridView 會執行下列步驟：

1. GridView 的`EditItemIndex`屬性指派給其編輯按鈕已按下的資料列的索引
2. GridView 重新繫結本身到 ObjectDataSource 叫用其`Select()`方法
3. 比對的資料列索引`EditItemIndex`呈現在 「 編輯模式 」。 在此模式中，編輯 按鈕已取代更新 和 取消 按鈕和 BoundFields 其`ReadOnly`屬性為 False （預設值） 會轉譯為 TextBox Web 控制項`Text`屬性指派給資料欄位的值。

此時的標記會傳回至瀏覽器，讓使用者對資料列的資料進行任何變更。 當使用者按一下 [更新] 按鈕時，會發生回傳和 GridView 會執行下列步驟：

1. ObjectDataSource 的`UpdateParameters`值指派至 GridView 的編輯介面由使用者所輸入的值
2. ObjectDataSource 的`Update()`叫用方法時，更新指定的記錄
3. GridView 重新繫結本身到 ObjectDataSource 叫用其`Select()`方法

若要指派的主索引鍵值`UpdateParameters`步驟 1 中來自中指定的值`DataKeyNames`屬性，而非主索引鍵值是來自已編輯的資料列的 TextBox Web 控制項中的文字。 因為刪除，它是不可或缺的 GridView`DataKeyNames`正確設定屬性。 如果遺失`UpdateParameters`主索引鍵值會指派`null`中步驟 1，又不會產生任何值更新步驟 2 中的記錄。

只要選取 [啟用編輯] 核取方塊在 GridView 的智慧標籤，就可以啟動編輯功能。


![核取 [啟用編輯] 核取方塊](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image37.png)

**圖 15**： 核取 啟用編輯 核取方塊


檢查 [啟用編輯] 核取方塊 （如有需要），將會新增 CommandField 和設定其`ShowEditButton`屬性設`true`。 如稍早所見，CommandField 包含數個`ShowXButton`指出哪些系列的按鈕會顯示在 CommandField 屬性。 檢查 [啟用編輯] 核取方塊加入`ShowEditButton`現有 CommandField 屬性：


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample6.aspx)]

這就是沒有新增基本編輯的支援。 Figure16 示編輯介面也很粗糙每 BoundField 其`ReadOnly`屬性設定為`false`（預設值） 會轉譯為文字方塊。 這包括像是欄位`CategoryID`和`SupplierID`，這是與其他資料表的索引鍵。


[![按一下 [Chai 的編輯] 按鈕顯示的資料列處於編輯模式](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image38.png)

**圖 16**： 按一下 Chai s [編輯] 按鈕會顯示在編輯模式中的資料列 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image40.png))


除了會要求使用者直接編輯外部索引鍵值，編輯介面的介面缺少下列方式：

- 如果使用者輸入`CategoryID`或是`SupplierID`不存在資料庫中`UPDATE`將違反外部索引鍵條件約束，造成引發例外狀況。
- 編輯介面不包含任何驗證。 如果您未提供必要的值 (例如`ProductName`)，或輸入字串值，其中必須是數值 （例如，輸入 「 太多 ！ 」 到`UnitPrice`文字方塊)，將會擲回例外狀況。 未來的教學課程將探討如何將驗證控制項新增至 編輯使用者介面。
- 目前，*所有*不是唯讀的產品欄位必須包含在 GridView。 如果我們從 GridView 移除欄位，說出`UnitPrice`更新資料的 GridView 會未設定時， `UnitPrice` `UpdateParameters`的值，也會變更資料庫記錄`UnitPrice`到`NULL`值。 同樣地，如果必要的欄位，這類`ProductName`，會移除從 GridView，更新將會失敗的相同 「*資料行 'ProductName' 不允許 null*"前面所提到的例外狀況。
- 編輯介面格式離開令人滿意。 `UnitPrice`顯示四個小數位數。 在理想情況下`CategoryID`和`SupplierID`值會包含在系統中列出的類別和供應商的 dropdownlist 進行。

這些是所有的缺點，我們必須忍受的到目前為止，但也會在未來的教學課程中獲得解決。

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>插入、 編輯和刪除資料與 DetailsView

如我們在先前的教學課程中所見，DetailsView 控制項一次，並如 GridView 會顯示一筆記錄，用來編輯和刪除目前顯示的資料錄。 編輯與刪除項目從 DetailsView 和 ASP.NET 側邊的工作流程的這兩個使用者的經驗都是一樣的 GridView。 DetailsView 其中與 GridView 不同處是它也提供內建的插入支援。

若要示範 GridView 的資料修改功能，先新增至 DetailsView`Basics.aspx`現有 GridView 上方頁面上，並將它繫結至現有的 ObjectDataSource 透過 DetailsView 的智慧標籤。 下一步，清除 DetailsView`Height`和`Width`屬性，並檢查的智慧標籤的 啟用分頁 選項。 若要啟用編輯，插入及刪除支援，只會檢查中的智慧標籤，啟用編輯、 啟用插入及啟用刪除核取方塊。


![設定支援編輯、 插入及刪除 DetailsView](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image41.png)

**圖 17**： 設定為支援編輯、 插入及刪除 DetailsView


做為使用 GridView，新增編輯、 插入或刪除支援加入 CommandField DetailsView，下列宣告式語法所示：


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample7.aspx)]

請注意，DetailsView CommandField 依預設會出現在資料行集合的結尾。 因為 DetailsView 的欄位會轉譯為資料列，CommandField 顯示為資料列，使用 Insert、 編輯和刪除 DetailsView 底部的按鈕。


[![設定支援編輯、 插入及刪除 DetailsView](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image42.png)

**圖 18**： 設定支援編輯、 插入及刪除 DetailsView ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image44.png))


按一下 [刪除] 按鈕啟動事件的相同順序如同 GridView: a 回傳;後面接著填入其 ObjectDataSource DetailsView`DeleteParameters`根據`DataKeyNames`值，並已完成，但呼叫其 ObjectDataSource`Delete()`方法，這個方法會實際從資料庫移除產品。 在 DetailsView 中編輯也適用於以相同的 GridView 的方式。

插入，終端使用者會看到新按鈕，按一下時，會呈現在 DetailsView 中 「 插入模式 」。 使用 「 插入模式 」 的新按鈕取代插入 和 取消 按鈕，只有這些 BoundFields 其`InsertVisible`屬性設定為`true`（預設值） 會顯示。 這類識別為自動遞增欄位，這些資料欄位`ProductID`，有其[InsertVisible 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx)設定為`false`繫結至資料來源 」、 「 智慧標籤的 DetailsView 時。

當繫結 DetailsView 透過智慧標籤的資料來源，Visual Studio 會將`InsertVisible`屬性設`false`只適用於自動遞增欄位。 唯讀欄位，例如`CategoryName`並`SupplierName`，將會顯示在 「 插入模式 」 使用者介面中，除非他們`InsertVisible`屬性明確設定為`false`。 請花一點時間來設定這兩個欄位`InsertVisible`屬性，以`false`，透過 DetailsView 的宣告式語法，或透過 [編輯] 欄位中的智慧標籤連結。 [圖 19] 顯示設定`InsertVisible`屬性，以`false`的編輯欄位上按一下連結。


[![Northwind Traders 現在提供 Acme 茶](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image45.png)

**圖 19**: Northwind Traders 現在提供 Acme 茶 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image47.png))


在設定後`InsertVisible`屬性、 檢視`Basics.aspx`頁面在瀏覽器，然後按一下 [新增] 按鈕。 [圖 20] 顯示 DetailsView 時加入新的飲料，Acme 茶，至我們的產品線。


[![Northwind Traders 現在提供 Acme 茶](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image48.png)

**圖 20**: Northwind Traders 現在提供 Acme 茶 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image50.png))


之後如 Acme 茶輸入詳細資料，並按一下 [插入] 按鈕，回傳是兩邊彼此乾瞪眼和新的記錄新增至`Products`資料庫資料表。 因為此 DetailsView 列出的順序與它們存在於資料庫資料表中的產品，我們必須頁面上的最後一個產品才能看到新的產品。


[![Acme 茶的詳細資料](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image51.png)

**圖 21**: Acme 茶的詳細資料 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image53.png))


> [!NOTE]
> DetailsView [CurrentMode 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx)指出所顯示的介面，而且可以是下列值之一： `Edit`， `Insert`，或`ReadOnly`。 [DefaultMode 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx)指出 DetailsView 之後編輯傳回，或插入的模式已完成，而且是適用於顯示 DetailsView 永久處於編輯或插入模式。


點並按一下 插入和編輯功能 DetailsView 苦 GridView 相同的限制： 使用者必須輸入現有`CategoryID`和`SupplierID`透過文字方塊的值; 介面缺少任何驗證邏輯，不允許的產品欄位`NULL`值，或沒有預設值在資料庫層級指定的值必須包含在插入介面，依此類推。

我們將擴充和增強 GridView 的編輯介面在未來的發行項可以套用至 DetailsView 控制項的編輯和插入介面以及檢視技術。

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>使用 FormView 更有彈性的資料修改的使用者介面

FormView 提供內建支援插入、 編輯和刪除資料，但因為它會使用範本，而不是欄位沒有將 BoundFields 或用來提供資料的 GridView 和 DetailsView 控制項 CommandField 位置修改介面。 相反地，此介面的 Web 控制項收集使用者輸入時新增項目或編輯現有的 fgpp，以及 [新增]，編輯、 刪除、 插入、 更新和取消按鈕必須手動新增至適當的範本。 幸運的是，繫結至資料來源透過下拉式清單中，在它的智慧標籤的 FormView 時 Visual Studio 會自動建立所需的介面。

為了說明這些技術，啟動 加到 FormView`Basics.aspx`頁面上，並從 FormView 的智慧標籤，將它繫結至已建立的 ObjectDataSource。 這會產生`EditItemTemplate`， `InsertItemTemplate`，和`ItemTemplate`FormView TextBox Web 控制項的新收集使用者的輸入和 Button Web 控制項的編輯、 刪除、 插入、 更新和 [取消] 按鈕。 此外，FormView 的`DataKeyNames`屬性設定為主要索引鍵欄位 (`ProductID`) 的 ObjectDataSource 所傳回的物件。 最後，檢查 FormView 的智慧標籤的 [啟用分頁] 選項。

以下顯示的宣告式標記為 FormView 的`ItemTemplate`FormView 已經繫結到 ObjectDataSource 之後。 根據預設，每個非布林值的產品 欄位繫結至`Text`Label Web 控制項時每個布林值欄位的屬性 (`Discontinued`) 繫結至`Checked`已停用的核取方塊 Web 控制項的屬性。 為了新增、 編輯和刪除按鈕來觸發特定 FormView 行為，按下時，務必，其`CommandName`值設定為`New`， `Edit`，和`Delete`分別。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample8.aspx)]

[圖 22] 顯示 FormView 的`ItemTemplate`透過瀏覽器檢視時。 每個產品 欄位會列出底部新增、 編輯和刪除按鈕。


[![預設 FormView ItemTemplate 列出每個產品 欄位，以及新增、 編輯和刪除按鈕](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image54.png)

**圖 22**: 預設 FormView`ItemTemplate`列出每個產品欄位以及新增、 編輯及刪除按鈕 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image56.png))


使用 GridView 和 DetailsView，按一下 [刪除] 按鈕，或任何按鈕、 LinkButton 或 ImageButton 其`CommandName`屬性設定為刪除，導致回傳，填入 ObjectDataSource 的`DeleteParameters`根據 FormView 的`DataKeyNames`值，並叫用的 ObjectDataSource`Delete()`方法。

按一下 [編輯] 按鈕時回傳是兩邊彼此乾瞪眼和資料重新繫結至`EditItemTemplate`，它會負責呈現編輯介面。 這個介面包含 Web 控制項來編輯資料，以及 [更新] 和 [取消] 按鈕。 預設值`EditItemTemplate`產生的 Visual Studio 包含的任何自動遞增欄位的標籤 (`ProductID`)、 一個的文字方塊中的每個非布林值欄位，以及每個布林值欄位的核取方塊。 此行為是非常類似於自動產生 BoundFields GridView 和 DetailsView 控制項中。

> [!NOTE]
> FormView 的自動產生的一個小問題`EditItemTemplate`是它呈現 TextBox Web 控制項是唯讀，例如這些欄位`CategoryName`和`SupplierName`。 我們會了解如何說明這一點短時間內。


TextBox 控制項中`EditItemTemplate`有其`Text`屬性繫結至其對應的資料欄位使用的值*雙向資料繫結*。 雙向資料繫結，以表示`<%# Bind("dataField") %>`，當資料繫結至範本，並填入插入或編輯記錄的 ObjectDataSource 的參數時，會執行資料繫結這兩個。 也就是說，當使用者按一下 [編輯] 按鈕時，才`ItemTemplate`，則`Bind()`方法會傳回指定的資料欄位值。 值的使用者進行變更，然後按一下 更新之後，張貼至使用指定的資料欄位對應的後`Bind()`會套用到 ObjectDataSource 的`UpdateParameters`。 單向資料繫結，或者，以表示`<%# Eval("dataField") %>`只擷取資料欄位值，資料繫結至範本時，未*不*返回資料來源的參數中的使用者輸入的值，在回傳。

下列宣告式標記會顯示 FormView 的`EditItemTemplate`。 請注意，`Bind()`方法是否會在資料繫結語法，以及更新和取消按鈕 Web 控制項有其`CommandName`據以設定的屬性。

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample9.aspx)]

我們`EditItemTemplate`，在這點，如果我們嘗試使用它擲回例外狀況。 問題在於`CategoryName`並`SupplierName`TextBox Web 控制項中，會呈現欄位`EditItemTemplate`。 我們需要變更這些文字方塊的標籤，或完全移除它們。 讓我們只要刪除它們完全從`EditItemTemplate`。

[圖 23] 顯示 FormView 瀏覽器中之後 Chai 的已按下 [編輯] 按鈕。 請注意，`SupplierName`並`CategoryName`欄位中所示`ItemTemplate`不再存在，因為我們只是移除從`EditItemTemplate`。 按一下 [更新] 按鈕時 FormView 會流經的 GridView 和 DetailsView 控制項相同的步驟順序。


[![根據預設 EditItemTemplate 會顯示為文字方塊或核取方塊的每個可編輯的產品 欄位](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image57.png)

**圖 23**： 依預設`EditItemTemplate`會顯示每個可編輯產品的欄位做為文字方塊或核取方塊 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image59.png))


[插入] 按鈕按一下 FormView 的時`ItemTemplate`回傳是兩邊彼此乾瞪眼。 不過，沒有資料繫結至 FormView 由於正在加入新的記錄。 `InsertItemTemplate`介面包含 Web 控制項，加入新的記錄，以及 [插入] 和 [取消] 按鈕。 預設值`InsertItemTemplate`產生的 Visual Studio 包含每個非布林值欄位的文字方塊和核取方塊，每個布林值欄位，類似於自動產生`EditItemTemplate`的介面。 將文字方塊控制項有其`Text`屬性繫結至其對應的資料欄位，使用雙向資料繫結的值。

下列宣告式標記會顯示 FormView 的`InsertItemTemplate`。 請注意，`Bind()`方法是否會在資料繫結語法，以及 Insert 和取消按鈕 Web 控制項有其`CommandName`據以設定的屬性。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample10.aspx)]

會使用 FormView 的自動產生的一些微妙的差異`InsertItemTemplate`。 具體來說，TextBox Web 控制項會針對這些欄位是唯讀，例如，甚至`CategoryName`和`SupplierName`。 如同`EditItemTemplate`，我們需要移除從這些文字方塊`InsertItemTemplate`。

[圖 24] 顯示 FormView 瀏覽器中新增新的產品，Acme 咖啡時。 請注意，`SupplierName`並`CategoryName`欄位中所示`ItemTemplate`不再存在，因為我們只是移除。 按一下 插入 按鈕時透過 DetailsView 控制項相同的步驟順序 FormView 進行，新增至記錄`Products`資料表。 圖 25 在 FormView 中顯示 Acme 咖啡產品詳細資料之後已插入。


[![InsertItemTemplate 規定 FormView 的插入介面](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image60.png)

**圖 24**:`InsertItemTemplate`規定 FormView 的插入介面 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image62.png))


[![在 FormView 中顯示新的產品，Acme 咖啡的詳細資料](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image63.png)

**圖 25**： 新的產品，Acme 咖啡的詳細資料會顯示在 FormView 中 ([按一下以檢視完整大小的影像](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image65.png))


藉由分隔出唯讀，編輯和介面插入三個不同的範本，FormView 用來控制這些介面比 GridView 與 DetailsView 精細的細微程度。

> [!NOTE]
> 例如 DetailsView FormView 的`CurrentMode`屬性會指出所顯示的介面和其`DefaultMode`屬性會指出模式 FormView 返回編輯之後，或已完成插入。


## <a name="summary"></a>總結

在本教學課程中，我們會檢查插入、 編輯和刪除資料使用 GridView、 DetailsView 和 FormView 的基本概念。 所有這三種控制項提供某種程度的內建的資料修改功能，可加以利用，而不需要撰寫一行程式碼，在 ASP.NET 頁面中，由於資料 Web 控制項和 ObjectDataSource。 不過，簡單點，然後按一下 技術轉譯相當身軀和貝氏資料修改使用者介面。 若要提供驗證，插入以程式設計方式的值、 依正常程序處理例外狀況、 自訂使用者介面，和等等，我們需要依賴一系列的技術，將會透過下一步 的數個教學課程中討論。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

> [!div class="step-by-step"]
> [下一步](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
