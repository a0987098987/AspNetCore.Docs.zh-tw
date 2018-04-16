---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb
title: 檢查事件相關聯插入、 更新和刪除 (VB) |Microsoft 文件
author: rick-anderson
description: 在本教學課程，我們將檢驗使用事件發生之前、 期間和之後插入、 更新或刪除 ASP.NET 資料 Web 控制項的作業。 W...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: c9bd10a7-eff8-4d8c-bec9-963c2aef2d6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 9c6a0ff85567b6e41a62feddc58672f38ad0d75b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="examining-the-events-associated-with-inserting-updating-and-deleting-vb"></a>檢查與插入、 更新和刪除 (VB) 相關聯的事件
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_VB.exe)或[下載 PDF](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/datatutorial17vb1.pdf)

> 在本教學課程，我們將檢驗使用事件發生之前、 期間和之後插入、 更新或刪除 ASP.NET 資料 Web 控制項的作業。 我們也會看到如何自訂編輯介面只更新的產品欄位的子集。


## <a name="introduction"></a>簡介

當使用內建的插入、 編輯或刪除 GridView、 DetailsView 或在 FormView 控制項的功能時，各種步驟時會發生使用者完成 加入新的記錄或更新或刪除現有記錄的程序。 如我們所述[上一個教學課程](an-overview-of-inserting-updating-and-deleting-data-vb.md)、 在 [更新] 和 [取消] 5d; 按鈕和文字方塊 BoundFields 變成會取代 [編輯] 按鈕在 GridView 中編輯資料列時。 終端使用者更新的資料，並按一下 更新之後，會在回傳時執行下列步驟：

1. 在 GridView 填入其 ObjectDataSource`UpdateParameters`與編輯的記錄的唯一識別欄位 (透過`DataKeyNames`屬性) 以及使用者所輸入的值
2. GridView 會叫用其 ObjectDataSource`Update()`方法，它會接著叫用適當的方法，在基礎物件 (`ProductsDAL.UpdateProduct`，在我們的上一個教學課程)
3. 基礎資料，現在包含更新的變更，是重新繫結至 GridView

在這一系列的步驟，有多個事件引發，讓我們來建立事件處理常式，將自訂邏輯加入所需。 例如前步驟 1 中，在 GridView 的,`RowUpdating`事件引發。 此時，可以某些驗證錯誤時取消更新要求。 當`Update()`叫用方法時，ObjectDataSource`Updating`事件引發，若要加入或自訂的任何值的機會`UpdateParameters`。 ObjectDataSource 的基礎物件的方法完成後執行，ObjectDataSource`Updated`就會引發事件。 事件處理常式`Updated`事件可以檢查有關更新作業，例如多少資料列受到影響，以及是否會發生例外狀況的詳細資料。 最後，在步驟 2 中，在 GridView 的之後`RowUpdated`事件引發; 的事件處理常式事件，可能會檢查執行只是更新作業的相關額外資訊。

圖 1 描述這一系列的事件和步驟，更新 GridView 時。 圖 1 中的事件模式不是更新與 GridView 唯一的。 插入、 更新或刪除從 GridView 的資料，DetailsView 或在 FormView precipitates 相同資料的 Web 控制項與 ObjectDataSource 的前置和後置層級事件的順序。


[![系列的前置和後的事件引發時更新 GridView 中的資料](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image1.png)

**圖 1**: A 系列的前置和後置事件引發時更新中的資料 GridView ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image3.png))


在本教學課程，我們將檢驗使用這些事件，以擴充內建的插入、 更新和刪除功能的 ASP.NET 資料 Web 控制項。 我們也會看到如何自訂編輯介面只更新的產品欄位的子集。

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>步驟 1： 更新的產品`ProductName`和`UnitPrice`欄位

編輯介面，從上一個教學課程中*所有*未唯讀狀態必須是包含的產品欄位。 如果要移除欄位而使 GridView-說`QuantityPerUnit`-當更新資料的 Web 控制項的資料就不應設定 ObjectDataSource `QuantityPerUnit` `UpdateParameters`值。 ObjectDataSource 然後會傳送值為`Nothing`到`UpdateProduct`商務邏輯層 (BLL) 方法，也可能會變更已編輯的資料庫記錄的`QuantityPerUnit`欄`NULL`值。 同樣地，如果必要的欄位，例如`ProductName`，會移除從編輯介面，更新將會失敗並"*資料行 'ProductName' 不允許 null*」 例外狀況。 此行為的原因，是因為 ObjectDataSource 的設定來呼叫`ProductsBLL`類別的`UpdateProduct`方法，每個產品欄位預期輸入的參數。 因此，ObjectDataSource 的`UpdateParameters`集合包含參數之方法的每個輸入參數。

如果我們想要提供資料的 Web 控制項，可讓使用者只會更新子集合的欄位，則我們需要以程式設計方式設定遺漏`UpdateParameters`值在 ObjectDataSource`Updating`事件處理常式或建立，並呼叫 BLL 方法預期欄位的子集。 讓我們來探索此第二種方法。

具體來說，讓我們來建立顯示的頁面，只要`ProductName`和`UnitPrice`中可編輯的 GridView 的欄位。 此 GridView 編輯介面只會允許使用者更新兩個顯示的欄位，`ProductName`和`UnitPrice`。 此編輯介面只提供的產品欄位的子集，因為我們需要建立會使用現有的 BLL ObjectDataSource`UpdateProduct`方法，並有遺漏的產品欄位值設定以程式設計方式在其`Updating`事件處理常式，或我們需要建立新的 BLL 方法預期在 GridView 中定義之欄位的子集。 此教學課程中，我們使用第二種選項，並建立的多載`UpdateProduct`方法，會採用只有三個輸入參數中的一個： `productName`， `unitPrice`，和`productID`:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample1.vb)]

例如原始`UpdateProduct`方法，這個多載會啟動，檢查是否有一項產品中具有指定資料庫`ProductID`。 如果沒有，它會傳回`False`，表示要更新的產品資訊的要求失敗。 否則它會更新現有的產品記錄`ProductName`和`UnitPrice`據此欄位，並藉由呼叫 TableAdpater 認可更新`Update()`方法，傳入`ProductsRow`執行個體。

與此新增功能至我們`ProductsBLL`類別中，我們就可以準備建立簡化的 GridView 介面。 開啟`DataModificationEvents.aspx`中`EditInsertDelete`資料夾並加入至頁面的 GridView。 建立新的 ObjectDataSource，並將它設定為使用`ProductsBLL`類別其`Select()`方法對應至`GetProducts`及其`Update()`方法對應至`UpdateProduct`只接受中的多載`productName`， `unitPrice`，和`productID`輸入參數。 圖 2 顯示建立資料來源精靈，當對應 ObjectDataSource`Update()`方法`ProductsBLL`類別的新`UpdateProduct`方法多載。


[![ObjectDataSource update （） 方法對應至新的 UpdateProduct 多載](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image4.png)

**圖 2**： 對應 ObjectDataSource`Update()`方法來新增`UpdateProduct`多載 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image6.png))


因為我們的範例一開始只需要編輯資料，但不是能插入或刪除記錄的功能，請花一點時間明確表明 ObjectDataSource`Insert()`和`Delete()`方法不應對應至任何`ProductsBLL`移至 插入和刪除索引標籤，然後從下拉式清單中選擇 （無） 類別的方法。


[![從下拉式清單中，插入和刪除索引標籤中選擇 （無）](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image7.png)

**圖 3**： 選擇 （無） 從下拉式清單插入和刪除的索引標籤 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image9.png))


此精靈完成後檢查啟用編輯 核取方塊 GridView 的智慧標籤。

完成建立資料來源精靈] 和 [繫結至 GridView，與 Visual Studio 已經建立兩個控制項的宣告式語法。 請移至來源檢視，來檢查 ObjectDataSource 的宣告式標記，其中如下所示：


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample2.aspx)]

因為沒有對應的 ObjectDataSource`Insert()`和`Delete()`方法，沒有任何`InsertParameters`或`DeleteParameters`區段。 此外，因為`Update()`方法對應到`UpdateProduct`只接受三個的輸入的參數的方法多載`UpdateParameters`區段具有三`Parameter`執行個體。

請注意，ObjectDataSource`OldValuesParameterFormatString`屬性設定為`original_{0}`。 使用設定資料來源精靈 時，這個屬性會自動設定由 Visual Studio。 不過，由於我們 BLL 方法不需要原始`ProductID`中進行傳遞，請從 ObjectDataSource 的宣告式語法完全移除此屬性指派值。

> [!NOTE]
> 如果您只要清除`OldValuesParameterFormatString`屬性值從 [屬性] 視窗，在設計檢視中，屬性仍會存在於宣告式語法，但將設定為空字串。 移除屬性完全宣告式語法，或從 [屬性] 視窗中，將值設定為預設值， `{0}`。


而 ObjectDataSource 只`UpdateParameters`產品的名稱、 價格和識別碼，如 Visual Studio 已新增 BoundField 或 CheckBoxField GridView 中每個產品的欄位。


[![在 GridView BoundField 或 CheckBoxField 包含每個產品的欄位](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image10.png)

**圖 4**: GridView BoundField 或 CheckBoxField 包含每個產品的欄位 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image12.png))


當終端使用者編輯產品，然後按一下其 [更新] 按鈕時，GridView 會列舉不是唯讀的欄位。 然後將之對應參數的值在 ObjectDataSource`UpdateParameters`使用者所輸入的值集合。 如果沒有對應的參數，在 GridView 加入至該集合。 因此，如果我們 GridView 包含 BoundFields 和 CheckBoxFields 所有產品的欄位，ObjectDataSource，最後將會叫用`UpdateProduct`在所有這些參數，儘管會採用多載，ObjectDataSource宣告式標記指定只有三個輸入的參數 （請參閱圖 5）。 同樣地，如果沒有非唯讀的某種組合產品中的欄位未對應到輸入參數的 GridView`UpdateProduct`多載，嘗試更新時，就會引發例外狀況。


[![GridView 會將參數加入至 ObjectDataSource UpdateParameters 集合](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image13.png)

**圖 5**: GridView 會將參數加入至 ObjectDataSource`UpdateParameters`集合 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image15.png))


若要確保 ObjectDataSource 叫用`UpdateProduct`多載，會在只產品的名稱、 價格和識別碼，我們需要有可編輯的欄位限制 GridView 只`ProductName`和`UnitPrice`。 這可藉由移除其他 BoundFields 和 CheckBoxFields，藉由設定這些其他欄位的`ReadOnly`屬性`True`，或兩者的一些組合。 此教學課程中我們只是移除所有 GridView 欄位除外`ProductName`和`UnitPrice`BoundFields，其後 GridView 的宣告式標記看起來像：


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample3.aspx)]

即使`UpdateProduct`多載必須有三個輸入的參數，我們在我們的 GridView 中只能有兩個 BoundFields。 這是因為`productID`輸入的參數是主索引鍵值，和傳遞的值`DataKeyNames`編輯資料列的屬性。

我們的 GridView，連同`UpdateProduct`多載，可讓使用者編輯而不會遺失的任何其他產品欄位的名稱和產品的價格。


[![介面可讓您編輯只要產品的名稱與價格](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image16.png)

**圖 6**： 編輯只要產品的名稱與價格介面允許 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image18.png))


> [!NOTE]
> 上一個教學課程所述，它是非常重要的 GridView 的檢視狀態是啟用 （預設行為）。 如果您將 GridView s`EnableViewState`屬性`false`，執行並行的使用者不小心刪除或編輯記錄的風險。 請參閱[警告： 已停用並行問題與 ASP.NET 2.0 GridViews/DetailsView/FormViews 該支援編輯和/或刪除與的檢視狀態](http://scottonwriting.net/sowblog/posts/10054.aspx)如需詳細資訊。


## <a name="improving-theunitpriceformatting"></a>改善`UnitPrice`格式

雖然圖 6 是有效的在顯示的 GridView 範例`UnitPrice`欄位的格式不完全，導致缺少任何貨幣價格顯示符號和有四個小數位數。 若要套用貨幣格式不可編輯的資料列，只要設定`UnitPrice`BoundField 的`DataFormatString`屬性`{0:c}`及其`HtmlEncode`屬性`False`。


[![據以設定單價 DataFormatString 和 HtmlEncode 屬性](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image19.png)

**圖 7**： 設定`UnitPrice`的`DataFormatString`和`HtmlEncode`據以屬性 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image21.png))


使用這項變更，不可編輯的資料列的格式價格為貨幣。編輯資料列，不過，仍會顯示不含貨幣符號，利用四位數的值。


[![非可編輯的資料列會現在格式化成貨幣值](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image22.png)

**圖 8**: 非可編輯的資料列會立即將其格式化為貨幣值 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image24.png))


中指定的格式設定指示`DataFormatString`屬性可以套用至編輯介面，藉由設定 BoundField`ApplyFormatInEditMode`屬性`True`(預設值是`False`)。 請花一點時間將此屬性設定為`True`。


[![UnitPrice BoundField 的 ApplyFormatInEditMode 屬性設為 True](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image25.png)

**圖 9**： 設定`UnitPrice`BoundField 的`ApplyFormatInEditMode`屬性`True`([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image27.png))


這項變更的值與`UnitPrice`顯示在已編輯資料列也會格式化為貨幣。


[![編輯資料列的 UnitPrice 值是現在格式化貨幣](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image28.png)

**圖 10**: 編輯資料列的`UnitPrice`值是現在格式化為貨幣 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image30.png))


不過，更新的產品以貨幣符號的文字方塊中，例如 $19.00 擲回`FormatException`。 GridView 嘗試將使用者提供的值指派給 ObjectDataSource`UpdateParameters`集合無法轉換`UnitPrice`字串"$19.00"`Decimal`所需的參數 （請參閱圖 11）。 若要補救這種情況我們可以建立事件處理常式的 GridView`RowUpdating`事件，並讓它剖析使用者提供`UnitPrice`做為貨幣格式化`Decimal`。

GridView`RowUpdating`做為其第二個參數接受事件類型的物件[GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx)，其中包括`NewValues`字典，因為其中一個保留準備好的使用者提供值的屬性指派給 ObjectDataSource`UpdateParameters`集合。 我們可以覆寫現有`UnitPrice`值`NewValues`使用貨幣格式具有下列幾行中的程式碼的集合，具有十進位值剖析`RowUpdating`事件處理常式：


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample4.vb)]

如果使用者已經提供`UnitPrice`值 （例如"$19.00")，這個值會覆寫所計算的十進位值[Decimal.Parse](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx)，剖析為貨幣值。 這將會正確地剖析小數點萬一任何貨幣符號、 逗號、 小數位數，等等，並使用[NumberStyles 列舉](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx)中[System.Globalization](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx)命名空間。

圖 11 顯示這兩個貨幣符號在使用者提供所造成的問題`UnitPrice`，以及如何 GridView`RowUpdating`事件處理常式可以用來正確地剖析這類的輸入。


[![編輯資料列的 UnitPrice 值是現在格式化貨幣](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image31.png)

**圖 11**: 編輯資料列的`UnitPrice`值是現在格式化為貨幣 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image33.png))


## <a name="step-2-prohibitingnull-unitprices"></a>步驟 2： 禁止`NULL UnitPrices`

當資料庫設定為允許`NULL`值`Products`資料表的`UnitPrice`我們可能會想要防止使用者瀏覽這個特定的頁面，從指定的資料行， `NULL` `UnitPrice`值。 也就是說，如果使用者在輸入`UnitPrice`時編輯產品的資料列的值，而不是將結果儲存至我們想要顯示訊息，通知使用者此頁面上，透過已編輯的任何產品必須有指定價格的資料庫。

`GridViewUpdateEventArgs`物件傳遞至 GridView`RowUpdating`事件處理常式包含`Cancel`屬性，如果設定為`True`，會終止在更新程序。 讓我們來擴充`RowUpdating`事件處理常式，以設定`e.Cancel`至`True`並顯示訊息，說明原因，如果`UnitPrice`值`NewValues`集合的值為`Nothing`。

將標籤 Web 控制項加入至名為頁面開始`MustProvideUnitPriceMessage`。 如果使用者無法指定，就會顯示此標籤控制項`UnitPrice`更新產品時的值。 設定標籤`Text`屬性，以 「 您必須提供價格的產品 」。 我也已經建立新的 CSS 類別中`Styles.css`名為`Warning`具有下列定義：


[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample5.css)]

最後，設定標籤的`CssClass`屬性`Warning`。 在此時設計工具應該會顯示警告訊息中的紅色、 粗體、 斜體、 上方 GridView 中的超大型字型大小圖 12 中所示。


[![已加入標籤上方的 GridView](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image34.png)

**圖 12**: 標籤已被加入上述 GridView ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image36.png))


根據預設，此標籤應該隱藏，因此，設定其`Visible`屬性`False`中`Page_Load`事件處理常式：


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample6.vb)]

如果使用者嘗試更新產品，而不指定`UnitPrice`，我們想要取消更新並顯示警告標籤。 加強的 GridView`RowUpdating`事件處理常式，如下所示：


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample7.vb)]

如果使用者嘗試儲存未指定價格的產品，已取消更新，而且很有幫助的訊息會顯示。 資料庫 （和商務邏輯） 時允許`NULL` `UnitPrice` s，此特定的 ASP.NET 頁面並不會。


[![使用者無法離開 UnitPrice 空白](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image37.png)

**圖 13**: 使用者無法離開`UnitPrice`空白 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image39.png))


到目前為止我們已經看到如何使用 GridView`RowUpdating`事件來以程式設計方式變更參數值指派給 ObjectDataSource`UpdateParameters`集合以及如何取消更新完全處理。 這些概念會延續到 DetailsView 和 FormView 控制項，以及也適用於插入及刪除。

這些工作還可以在 ObjectDataSource 層級的事件處理常式透過其`Inserting`， `Updating`，和`Deleting`事件。 這些事件引發之前叫用的基礎物件相關聯的方法，並提供修改輸入的參數集合，或取消作業徹底的最後機會機會。 這三個事件的事件處理常式會傳遞類型的物件[ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx)具有感興趣的兩個屬性：

- [取消](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx)，而如果設定為`True`、 取消正在執行的作業
- [在這](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx)，這是集合`InsertParameters`， `UpdateParameters`，或`DeleteParameters`，取決於事件處理常式是否為`Inserting`， `Updating`，或`Deleting`事件

為了說明使用 ObjectDataSource 層級的參數值，讓我們來包含 DetailsView 中我們讓使用者能夠加入新的產品的頁面。 此 DetailsView 將用來提供介面來迅速將新的產品加入至資料庫。 若要保持一致的使用者介面，讓我們加入新的產品可讓使用者只輸入的值時`ProductName`和`UnitPrice`欄位。 根據預設，在 DetailsView 的插入介面中不提供這些值會設定為`NULL`資料庫值。 不過，我們可以使用 ObjectDataSource`Inserting`来插入不同的預設值，我們很快就會看到事件。

## <a name="step-3-providing-an-interface-to-add-new-products"></a>步驟 3： 提供介面來將新的產品

從 [工具箱] 拖曳至上方 GridView 中設計工具拖曳 DetailsView，清除其`Height`和`Width`屬性和繫結到 ObjectDataSource 尚未出現在頁面上。 這會新增 BoundField 或 CheckBoxField 每個產品的欄位。 由於我們想要使用此 DetailsView 加入新的產品，我們必須核取 啟用插入選項從智慧標籤。不過，沒有任何這類選項因為 ObjectDataSource`Insert()`方法未對應中的方法`ProductsBLL`類別 （請記得，我們設定此對應 （無） 時設定資料來源，請參閱圖 3）。

若要設定 ObjectDataSource，選取 設定資料來源連結從其智慧標籤，啟動精靈。 第一個畫面可讓您變更基礎物件 ObjectDataSource 繫結至;將它設定為`ProductsBLL`。 下一個畫面會列出從 ObjectDataSource 方法基礎物件的對應。 即使明確指出，`Insert()`和`Delete()`方法不應對應至任何方法，如果您移至 插入和刪除索引標籤會看到對應是否有。 這是因為`ProductsBLL`的`AddProduct`和`DeleteProduct`方法使用`DataObjectMethodAttribute`屬性，表示它們是預設方法`Insert()`和`Delete()`分別。 因此，ObjectDataSource 精靈會選取這些每次您執行精靈，除非明確指定的其他值。

保留`Insert()`方法指向`AddProduct`方法，但一次設定為 （無） 的 刪除 索引標籤的下拉式清單。


[![[插入] 索引標籤的下拉式清單設 AddProduct 方法](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image40.png)

**圖 14**: [插入] 索引標籤的下拉式清單設定為`AddProduct`方法 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image42.png))


[![設定 [刪除] 索引標籤的下拉式清單，為 （無）](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image43.png)

**圖 15**： 刪除索引標籤的下拉式清單 （無） ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image45.png))


進行這些變更之後，ObjectDataSource 的宣告式語法會展開成包含`InsertParameters`集合，如下所示：


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample8.aspx)]

重新執行精靈加回`OldValuesParameterFormatString`屬性。 請花一點時間來清除此屬性設定為預設值 (`{0}`) 或移除它完全宣告式語法。

提供插入功能 ObjectDataSource]，以在 DetailsView 的智慧標籤現在包含啟用插入核取方塊。返回 [設計工具，並核取此選項。 接下來，使其只具有兩個 BoundFields-削減 DetailsView`ProductName`和`UnitPrice`-和 CommandField。 此時在 DetailsView 的宣告式語法看起來應該像：


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample9.aspx)]

圖 16 顯示此時透過瀏覽器檢視時，此頁面。 如您所見，DetailsView 列出 (Chai) 的第一個產品價格與名稱。 不過，我們要是插入可提供方法，以快速在資料庫中加入新的產品使用者介面。


[![在 DetailsView 是目前在唯讀模式中轉譯](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image46.png)

**圖 16**: DetailsView 是目前在唯讀模式中呈現 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image48.png))


若要顯示在 DetailsView 中所以我們需要將其插入模式`DefaultMode`屬性`Inserting`。 這會轉譯在插入模式下，當第一次瀏覽 DetailsView 並且那里它之後，插入新的記錄。 圖 17 示，這類 DetailsView 提供快速的介面加入新的記錄。


[![在 DetailsView 為了迅速將加入新的產品提供的介面](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image49.png)

**圖 17**: DetailsView 提供介面以迅速將加入新的產品 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image51.png))


當使用者輸入的產品名稱和價格 （例如"Acme Water"和 1.99，如圖 17 所示），然後按一下 插入時，回傳展示並插入工作流程，在開始新的產品記錄被新增到資料庫中累積。 在 DetailsView 會維護其插入的介面和 GridView 會自動重新繫結至其資料來源以便加入新的產品，如圖 18 所示。


![產品](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image52.png)

**圖 18**： 產品"Acme Water"加入資料庫


雖然圖 18 GridView 不會顯示，缺少 DetailsView 介面中的產品欄位`CategoryID`， `SupplierID`，`QuantityPerUnit`等等指派`NULL`資料庫值。 您可以看到這藉由執行下列步驟：

1. 請移至 Visual Studio 中的 [伺服器總管]
2. 展開`NORTHWND.MDF`資料庫節點
3. 以滑鼠右鍵按一下`Products`資料庫資料表節點
4. 選取 顯示資料表資料

這樣將會列出所有在記錄`Products`資料表。 如圖 19 所示，所有新產品的資料行以外`ProductID`， `ProductName`，和`UnitPrice`有`NULL`值。


[![在 DetailsView 中產品欄位未提供會指派 NULL 值](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image53.png)

**圖 19**: 產品欄位中未提供 DetailsView 指派`NULL`值 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image55.png))


我們可能會想要提供的預設值以外`NULL`的一個或多個資料行值，也就是因為`NULL`不是最佳的預設選項，或因為本身的資料庫資料行不允許`NULL`s。 若要完成此我們可以透過程式設計方式設定的 DetailsView 的參數值`InputParameters`集合。 這項指派可以在進行可能是在事件處理常式在 DetailsView 的`ItemInserting`事件或 ObjectDataSource`Inserting`事件。 由於我們已經討論在使用前置和後置層級的事件資料 Web 控制層級，讓我們來探索目前使用 ObjectDataSource 的事件。

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>步驟 4： 指派值給`CategoryID`和`SupplierID`參數

此教學課程中我們假設，加入新的產品，透過這個介面時，我們應用程式應該指派`CategoryID`和`SupplierID`值為 1。 如先前所述，ObjectDataSource 有一組前置和後置層級的事件引發資料修改程序期間。 當其`Insert()`叫用方法時，會先引發 ObjectDataSource 其`Inserting`事件，然後呼叫的方法，其`Insert()`方法都已經對應至，並最後引發`Inserted`事件。 `Inserting`事件處理常式可以提供給我們調整輸入的參數，或取消作業徹底的最後機會。

> [!NOTE]
> 在真實世界應用程式，您可能會想要讓使用者指定的類別目錄和供應商或會選取這個值，根據一些準則或商務邏輯 （而非盲目地選取識別碼為 1）。 無論如何，此範例說明如何以程式設計方式設定輸入參數的值從 ObjectDataSource 預先層級事件。


請花一點時間來建立事件處理常式，用於 ObjectDataSource`Inserting`事件。 請注意，事件處理常式的第二個輸入的參數類型的物件`ObjectDataSourceMethodEventArgs`，其中包含要存取參數集合的屬性 (`InputParameters`) 和取消作業的屬性 (`Cancel`)。


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample10.vb)]

此時，`InputParameters`屬性包含 ObjectDataSource `InsertParameters` DetailsView 從指派的值集合。 若要變更其中一個參數的值，只要使用： `e.InputParameters("paramName") = value`。 因此，若要設定`CategoryID`和`SupplierID`值為 1，來調整`Inserting`事件處理常式，如下所示：


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample11.vb)]

此時間加入新的產品 （例如 Acme Soda) 時`CategoryID`和`SupplierID`新產品的資料行都設定為 1 （請參閱圖 20）。


[![新產品，現在有其 CategoryID 和 SupplierID 值設定為 1](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image56.png)

**圖 20**： 新產品現在有其`CategoryID`和`SupplierID`值設為 1 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image58.png))


## <a name="summary"></a>總結

在編輯、 插入和刪除處理程序，資料 Web 控制項和 ObjectDataSource 繼續執行的前置和後置層級的事件數目。 在此教學課程中我們會檢查預先層級的事件，可了解如何使用這些自訂的輸入的參數，或取消資料修改作業完全同時從資料 Web 控制項和 ObjectDataSource 的事件。 在下一個教學課程中我們將探討建立及用於後續層級的事件的事件處理常式。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已傑克 Goor 和 Liz Shulok。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](an-overview-of-inserting-updating-and-deleting-data-vb.md)
> [下一頁](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
