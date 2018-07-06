---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
title: 檢查事件相關聯插入、 更新和刪除 (C#) |Microsoft Docs
author: rick-anderson
description: 在本教學課程，我們將檢驗使用事件發生之前、 期間和之後插入、 更新或刪除作業在 ASP.NET 資料 Web 控制項。 中的...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: dab291a0-a8b5-46fa-9dd8-3d35b249201f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: cec25809038cbe6a114b0e36ef7265d84bbe9b92
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363296"
---
<a name="examining-the-events-associated-with-inserting-updating-and-deleting-c"></a>檢查與插入、 更新和刪除 (C#) 相關聯的事件
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_CS.exe)或[下載 PDF](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/datatutorial17cs1.pdf)

> 在本教學課程，我們將檢驗使用事件發生之前、 期間和之後插入、 更新或刪除作業在 ASP.NET 資料 Web 控制項。 我們也將了解如何自訂編輯介面只更新產品欄位的子集。


## <a name="introduction"></a>簡介

當使用內建的插入、 編輯或刪除 GridView、 DetailsView 或 FormView 控制項功能時，各種不同的步驟時會終端使用者完成將新記錄新增或更新或刪除現有記錄的程序。 如我們所述[先前的教學課程](an-overview-of-inserting-updating-and-deleting-data-cs.md)、 在 gridview 裡 [編輯] 按鈕會取代的更新] 和 [取消] 按鈕以及 [BoundFields 開啟在文字方塊中編輯資料列時。 終端使用者更新資料，然後按一下 更新之後，會在回傳中執行下列步驟：

1. GridView 會填入其 ObjectDataSource`UpdateParameters`以編輯的記錄的唯一識別欄位 (透過`DataKeyNames`屬性) 以及使用者所輸入的值
2. GridView 會叫用其 ObjectDataSource`Update()`方法，它會接著叫用適當的方法基礎物件中 (`ProductsDAL.UpdateProduct`，我們先前的教學課程中)
3. 基礎資料，現在包含更新後的變更，是重新繫結至 GridView

在這一系列的步驟，有多個事件引發，讓我們建立事件處理常式，來將自訂邏輯加入在需要時。 例如，步驟 1 之前，GridView 的`RowUpdating`引發事件。 某些驗證錯誤時，我們到目前為止，可以取消更新要求。 當`Update()`方法會叫用，ObjectDataSource`Updating`事件引發時，提供加入或自訂的任何值的機會`UpdateParameters`。 ObjectDataSource 的基礎物件的方法完成後執行，ObjectDataSource 的`Updated`就會引發事件。 事件處理常式`Updated`事件可以檢查更新作業，例如多少資料列受到影響，以及是否發生例外狀況的相關詳細資料。 最後，在步驟 2 中，GridView 的`RowUpdated`引發事件，檢查執行只是更新作業的相關額外資訊的事件來將事件處理常式。

圖 1 說明這一系列的事件和步驟，更新 GridView 時。 [圖 1] 中的事件模式不是更新與 GridView 唯一的。 插入、 更新或刪除資料的 GridView、 DetailsView 或 FormView precipitates 相同資料 Web 控制項和 ObjectDataSource 的前置和後置的層級事件的順序。


[![一系列的預先和後續的事件引發時更新 GridView 中的資料](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image1.png)

**圖 1**: A 系列的預先和後續的事件引發時更新資料 GridView 中 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image3.png))


在本教學課程，我們將檢驗使用這些事件來擴充內建的插入、 更新和刪除功能的 ASP.NET 資料 Web 控制項。 我們也將了解如何自訂編輯介面只更新產品欄位的子集。

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>步驟 1： 更新的產品`ProductName`和`UnitPrice`欄位

在上一個教學課程中的編輯介面*所有*不是唯讀狀態必須是包含的產品欄位。 如果我們要將欄位從 GridView-移除說`QuantityPerUnit`-當更新資料 Web 控制項的資料就不應設定 ObjectDataSource `QuantityPerUnit` `UpdateParameters`值。 ObjectDataSource 會接著傳入`null`值貼`UpdateProduct`商務邏輯層 (BLL) 方法，它會變更已編輯的資料庫記錄`QuantityPerUnit`資料行`NULL`值。 同樣地，如果必要的欄位，例如`ProductName`，會移除從編輯介面中，更新將會失敗並 「*資料行 'ProductName' 不允許 null*」 例外狀況。 此行為的原因是因為呼叫設定 ObjectDataSource`ProductsBLL`類別的`UpdateProduct`方法，每個產品欄位預期輸入的參數。 因此，ObjectDataSource 的`UpdateParameters`集合包含參數之方法的每個輸入參數。

如果我們想要提供資料 Web 控制項，可讓使用者只會更新欄位的子集，則我們需要以程式設計方式設定遺漏`UpdateParameters`ObjectDataSource 的中值`Updating`事件處理常式或建立，並呼叫 BLL 方法預期欄位的子集。 讓我們來探索此第二種方法。

具體來說，讓我們來建立頁面，其中顯示剛`ProductName`和`UnitPrice`可編輯的 GridView 內的欄位。 此 GridView 的編輯介面只允許使用者更新兩個顯示的欄位`ProductName`和`UnitPrice`。 因為此編輯介面只提供產品的欄位子集，所以我們需要建立會使用現有的 BLL ObjectDataSource`UpdateProduct`方法，並有遺漏的產品欄位值設定以程式設計方式在其`Updating`事件處理常式，或我們需要建立新的 BLL 方法預期 GridView 中定義的欄位子集。 本教學課程中，讓我們使用後面這個選項，並建立的多載`UpdateProduct`方法中，只需要三個輸入參數會採用的其中一個： `productName`， `unitPrice`，和`productID`:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample1.cs)]

像是原始`UpdateProduct`方法，會先檢查以查看是否有一項產品中具有指定之資料庫的這個多載`ProductID`。 如果沒有，它會傳回`false`，指出更新的產品資訊的要求失敗。 否則它會更新現有的產品記錄`ProductName`並`UnitPrice`據此欄位，並藉由呼叫 TableAdpater 認可更新`Update()`方法並傳入`ProductsRow`執行個體。

這與我們`ProductsBLL`類別中，我們已經準備好建立簡化的 GridView 介面。 開啟`DataModificationEvents.aspx`在`EditInsertDelete`資料夾，並新增至頁面的 GridView。 建立新的 ObjectDataSource，並將它設定為使用`ProductsBLL`類別及其`Select()`方法對應至`GetProducts`及其`Update()`方法對應至`UpdateProduct`只接受中的多載`productName`， `unitPrice`，和`productID`輸入參數。 圖 2 顯示建立資料來源精靈，當對應的 ObjectDataSource`Update()`方法，以`ProductsBLL`類別的新`UpdateProduct`方法多載。


[![對應至新的 UpdateProduct 多載的 ObjectDataSource 的 update （） 方法](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image4.png)

**圖 2**： 對應的 ObjectDataSource`Update()`方法來新增`UpdateProduct`多載 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image6.png))


因為我們的範例一開始只需要編輯資料，但是不能插入或刪除記錄的能力，花點時間明確表明 ObjectDataSource`Insert()`並`Delete()`方法不應對應至任何`ProductsBLL`移至 插入和刪除索引標籤，然後從下拉式清單中選擇 （無） 類別的方法。


[![從下拉式清單中，以便進行插入和刪除索引標籤中選擇 （無）](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image7.png)

**圖 3**： 選擇 （無） 從下拉式清單以便進行插入和刪除索引標籤 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image9.png))


此精靈完成後檢查 [啟用編輯] 核取方塊 GridView 的智慧標籤。

使用 建立資料來源精靈和繫結至 GridView 完成後，Visual Studio 建立兩個控制項的宣告式語法。 請移至來源檢視，來檢查 ObjectDataSource 的宣告式標記，會如下所示：


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample2.aspx)]

因為沒有為 ObjectDataSource 的對應`Insert()`並`Delete()`方法，有沒有`InsertParameters`或`DeleteParameters`區段。 此外，因為`Update()`方法會對應至`UpdateProduct`只接受三個的輸入的參數的方法多載`UpdateParameters` 區段會有三個只`Parameter`執行個體。

請注意，ObjectDataSource`OldValuesParameterFormatString`屬性設定為`original_{0}`。 使用設定資料來源精靈 時，這個屬性會自動設定由 Visual Studio。 不過，由於我們 BLL 方法不應接受原始`ProductID`傳遞，請從 ObjectDataSource 的宣告式語法中完全移除此屬性指派的值。

> [!NOTE]
> 如果您只需清除`OldValuesParameterFormatString`屬性值從 [屬性] 視窗，在 [設計] 檢視中，屬性仍會存在於宣告式語法，但會設為空字串。 移除屬性完全宣告式語法，或從 [屬性] 視窗中，將值設定為預設值， `{0}`。


雖然 ObjectDataSource 只有`UpdateParameters`產品的名稱、 價格和識別碼，Visual Studio 新增 BoundField 或 CheckBoxField GridView 裡每個產品的欄位。


[![GridView 會在每個產品的欄位包含 「 BoundField 或 CheckBoxField](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image10.png)

**圖 4**: GridView 會在每個產品的欄位包含 「 BoundField 或 CheckBoxField ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image12.png))


當使用者編輯產品，然後按一下其 [更新] 按鈕時，GridView 會列舉不是唯讀的欄位。 它接著設定 ObjectDataSource 的對應參數的值`UpdateParameters`使用者所輸入的值的集合。 如果沒有對應的參數，GridView 會新增至該集合。 因此，如果我們 GridView 包含 BoundFields 和 CheckBoxFields 所有產品的欄位，則 ObjectDataSource 會叫用結束`UpdateProduct`所有這些參數，但其實會採用的多載的 ObjectDataSource 的宣告式標記會指定只有三個輸入的參數 （請參閱 [圖 5]）。 同樣地，如果沒有非唯讀的某種組合產品中的欄位未對應到輸入參數的 GridView`UpdateProduct`多載，當您嘗試更新時，就會引發例外狀況。


[![GridView 會將參數加入至 ObjectDataSource 的 UpdateParameters 集合](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image13.png)

**圖 5**: GridView 會將參數加入至的 ObjectDataSource`UpdateParameters`集合 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image15.png))


若要確保 ObjectDataSource 會叫用`UpdateProduct`多載，只是產品的名稱、 價格和識別碼，我們需要有可編輯欄位限制 GridView 只`ProductName`和`UnitPrice`。 這可藉由移除其他 BoundFields 和 CheckBoxFields，藉由設定這些其他的欄位`ReadOnly`屬性設`true`，或兩者的一些組合。 本教學課程中我們只是移除所有 GridView 欄位除外`ProductName`和`UnitPrice`BoundFields，其後 GridView 的宣告式標記看起來像：


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample3.aspx)]

即使`UpdateProduct`多載必須要有三個輸入的參數，我們只需要兩個 BoundFields 我們 GridView 內。 這是因為`productID`輸入的參數是主索引鍵值，而且在傳遞的值`DataKeyNames`編輯資料列的屬性。

我們的 GridView，連同`UpdateProduct`多載，可讓使用者編輯而不會遺失的任何其他產品欄位的名稱和產品的價格。


[![此介面可讓編輯只是產品的名稱和價格](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image16.png)

**圖 6**： 編輯只是產品的名稱和價格的介面允許 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image18.png))


> [!NOTE]
> 如先前的教學課程中所述，它是非常重要的 GridView 的檢視狀態是啟用 （預設行為）。 如果您將設定 GridView s`EnableViewState`屬性設`false`，執行並行的使用者不小心刪除或編輯記錄的風險。 請參閱[警告： 已停用並行處理問題與 ASP.NET 2.0 Gridview/DetailsView/FormViews 該支援編輯和/或刪除與的 View State](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx)如需詳細資訊。


## <a name="improving-theunitpriceformatting"></a>改善`UnitPrice`格式化

雖然 圖 6 的運作方式，在顯示的 GridView 範例`UnitPrice`欄位的格式不完全，導致缺少任何貨幣的價格顯示符號和有四個小數位數。 若要套用貨幣格式不可編輯的資料列，請設定`UnitPrice`BoundField 的`DataFormatString`屬性設`{0:c}`及其`HtmlEncode`屬性設`false`。


[![據以設定單價 DataFormatString 和 HtmlEncode 屬性](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image19.png)

**圖 7**： 設定`UnitPrice`的`DataFormatString`並`HtmlEncode`據以屬性 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image21.png))


透過這項變更，非可編輯的資料列，請格式化成貨幣; 的價格編輯資料列，不過，仍會顯示不含貨幣符號，利用四位數的值。


[![非可編輯的資料列做為貨幣值的 現在的格式](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image22.png)

**圖 8**： 非可編輯的資料列會立即將其格式化為貨幣值 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image24.png))


中指定的格式設定指示`DataFormatString`屬性可以套用至編輯介面，藉由設定 BoundField`ApplyFormatInEditMode`屬性設`true`(預設值是`false`)。 花點時間，將此屬性設定為`true`。


[![UnitPrice BoundField 的 ApplyFormatInEditMode 屬性設定為 true](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image25.png)

**圖 9**： 設定`UnitPrice`BoundField 的`ApplyFormatInEditMode`屬性設`true`([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image27.png))


這項變更的值與`UnitPrice`顯示在 編輯資料列也會格式化為貨幣。


[![編輯資料列的 UnitPrice 值為現在格式化貨幣](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image28.png)

**圖 10**: 編輯資料列的`UnitPrice`值是現在格式化為貨幣 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image30.png))


不過，更新 [] 文字方塊中出現貨幣符號與產品，例如 $19.00 會擲回`FormatException`。 當嘗試將使用者提供的值指派給 ObjectDataSource 的 GridView`UpdateParameters`無法轉換的集合`UnitPrice`字串 「 $19.00 」 成`decimal`參數所需 （請參閱 圖 11）。 若要解決這個問題我們可以建立事件處理常式的 GridView`RowUpdating`事件，並將它剖析使用者提供`UnitPrice`做為貨幣格式化`decimal`。

GridView`RowUpdating`接受做為其第二個參數的型別物件的事件[GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx)，其中包括`NewValues`字典表示成其中一個保留準備好的使用者提供值的屬性指派給 ObjectDataSource 的`UpdateParameters`集合。 我們可以覆寫現有`UnitPrice`中的值`NewValues`具有十進位值的集合可讓您剖析下列幾行程式碼中使用的貨幣格式`RowUpdating`事件處理常式：


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample4.cs)]

如果使用者已經提供`UnitPrice`值 （例如"$19.00")，這個值會覆寫所計算的十進位值[Decimal.Parse](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx)，剖析為貨幣值。 這將會正確地剖析小數點發生任何貨幣符號、 逗號、 小數位數，並依此類推，並使用[NumberStyles 列舉](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx)中[System.Globalization](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx)命名空間。

[圖 11] 顯示這兩個中的使用者提供的貨幣符號所造成的問題`UnitPrice`，以及如何 GridView 的`RowUpdating`事件處理常式可以用來正確地剖析這類的輸入。


[![編輯資料列的 UnitPrice 值為現在格式化貨幣](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image31.png)

**圖 11**: 編輯資料列的`UnitPrice`值是現在格式化為貨幣 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image33.png))


## <a name="step-2-prohibitingnull-unitprices"></a>步驟 2： 禁止`NULL UnitPrices`

而資料庫已設定為允許`NULL`中的值`Products`資料表的`UnitPrice`資料行中，我們可能想要防止瀏覽此特定的頁面，從指定的使用者`NULL``UnitPrice`值。 也就是說，如果使用者無法輸入`UnitPrice`時編輯產品的資料列的值，而不是將結果儲存至我們想要顯示訊息，通知使用者此頁面上，透過任何已編輯的產品必須有指定價格的資料庫。

`GridViewUpdateEventArgs`物件傳遞至 GridView`RowUpdating`事件處理常式包含`Cancel`屬性，如果設定為`true`，終止更新程序。 讓我們來擴充`RowUpdating`若要設定的事件處理常式`e.Cancel`要`true`，並顯示訊息，說明原因，如果`UnitPrice`中的值`NewValues`集合是`null`。

藉由將名為頁面中的標籤 Web 控制項開始`MustProvideUnitPriceMessage`。 如果使用者無法指定，則會顯示此標籤控制項`UnitPrice`更新產品時的值。 將標籤的`Text`屬性 「 您必須提供價格的產品 」。 我也建立了新的 CSS 類別中`Styles.css`名為`Warning`具有下列定義：


[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample5.css)]

最後，設定標籤`CssClass`屬性設`Warning`。 在此時設計工具應該會顯示警告訊息中紅色、 粗體、 斜體、 超大的字型大小，上述 GridView，如 圖 12 所示。


[![已新增標籤上方的 GridView](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image34.png)

**圖 12**: 標籤具有已加入上述的 GridView ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image36.png))


根據預設，此標籤應該隱藏，因此，請將其`Visible`屬性，以`false`在`Page_Load`事件處理常式：


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample6.cs)]

如果使用者嘗試更新產品，而不指定`UnitPrice`，我們想要取消更新並顯示 [警告] 標籤。 增強 GridView 的`RowUpdating`事件處理常式，如下所示：


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample7.cs)]

如果使用者嘗試儲存未指定價格的產品時，更新已取消，而且很有幫助的訊息會顯示。 雖然資料庫 （和商務邏輯） 是用來`NULL` `UnitPrice` s，這個特定的 ASP.NET 網頁則否。


[![使用者不能離開 UnitPrice 空白](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image37.png)

**圖 13**: 使用者不能離開`UnitPrice`空白 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image39.png))


到目前為止我們已了解如何使用 GridView`RowUpdating`來以程式設計方式變更指派給 ObjectDataSource 的參數值的事件`UpdateParameters`集合以及如何取消更新完全處理。 這些概念會延續到 DetailsView 和 FormView 控制項，也適用於插入及刪除。

這些工作還可以在 ObjectDataSource 層級事件處理常式，如透過其`Inserting`， `Updating`，和`Deleting`事件。 這些事件引發之前叫用的基礎物件相關聯的方法，並提供最後機會修改輸入的參數集合，或取消作業直接。 這三個事件的事件處理常式傳遞類型的物件[ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx)具有感興趣的兩個屬性：

- [取消](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx)，而如果設定為`true`，取消正在執行的作業
- [InputParameters](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx)，這是收集`InsertParameters`， `UpdateParameters`，或`DeleteParameters`，取決於事件處理常式是否針對`Inserting`， `Updating`，或`Deleting`事件

為了說明使用 ObjectDataSource 層級的參數值，讓我們納入 DetailsView 我們可讓使用者將新產品的頁面。 此 DetailsView 將用來提供介面來快速地將新的產品加入資料庫中。 若要讓我們加入一個新的產品可讓使用者只輸入的值時，保持一致的使用者介面`ProductName`和`UnitPrice`欄位。 根據預設，DetailsView 插入介面中不提供這些值會設定為`NULL`資料庫值。 不過，我們可以使用 ObjectDataSource 的`Inserting`插入不同的預設值，如我們稍後就會看到的事件。

## <a name="step-3-providing-an-interface-to-add-new-products"></a>步驟 3： 提供的介面來新增新的產品

從 [工具箱] 拖曳至上方 gridview 裡，設計工具拖曳 DetailsView，清除其`Height`和`Width`屬性和繫結到 ObjectDataSource 在頁面中已存在。 這會新增 BoundField 或 CheckBoxField，每個產品的欄位。 因為我們想要用於此 DetailsView 新增新的產品，我們必須檢查的智慧標籤，啟用插入選項不過，沒有任何這類選項因為 ObjectDataSource`Insert()`方法不會對應到方法，以在`ProductsBLL`類別 （請記得，我們設定此對應 （無） 時設定資料來源，請參閱 圖 3）。

若要設定 ObjectDataSource，選取 設定資料來源連結，從它的智慧標籤，啟動精靈。 第一個畫面可讓您變更基礎物件的 ObjectDataSource 會繫結至;將它設定為`ProductsBLL`。 下一個畫面中列出的基礎物件的 從 ObjectDataSource 的方法的對應。 我們會明確指出，雖然`Insert()`和`Delete()`方法不應對應至任何方法，如果您移至 插入和刪除索引標籤會看到對應是否有。 這是因為`ProductsBLL`的`AddProduct`並`DeleteProduct`方法會使用`DataObjectMethodAttribute`屬性，表示它們是預設方法，如`Insert()`和`Delete()`分別。 因此，ObjectDataSource 精靈會選取這些每次您執行精靈，除非有明確指定其他值。

離開`Insert()`指向的方法`AddProduct`方法，但一次設定為 （無） 的 刪除 索引標籤的下拉式清單。


[![將 [插入] 索引標籤的下拉式清單設定為 [AddProduct 方法](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image40.png)

**圖 14**： 若要設定 [插入] 索引標籤下拉式清單`AddProduct`方法 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image42.png))


[![[刪除] 索引標籤的下拉式清單 （無）](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image43.png)

**圖 15**： 設定為 [（無） 的 [刪除] 索引標籤下拉式清單 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image45.png))


進行這些變更之後，ObjectDataSource 的宣告式語法會展開成包含`InsertParameters`集合，如下所示：


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample8.aspx)]

重新執行精靈加回`OldValuesParameterFormatString`屬性。 請花一點時間來清除此屬性設定為預設值 (`{0}`) 或移除它完全宣告式語法。

使用 ObjectDataSource 提供插入功能，DetailsView 的智慧標籤現在會包含啟用插入核取方塊;返回 設計工具，並核取此選項。 接下來，使其僅具有兩個 BoundFields-削減 DetailsView`ProductName`和`UnitPrice`-和 CommandField。 此時 DetailsView 的宣告式語法看起來應該像：


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample9.aspx)]

[圖 16] 顯示此時檢視透過瀏覽器時，此頁面。 如您所見，DetailsView 會列出第一個產品 (Chai) 的價格與名稱。 不過，我們要是提供方法，讓使用者快速加入資料庫中的新產品插入介面。


[![DetailsView 是目前在唯讀模式中呈現](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image46.png)

**圖 16**: DetailsView 是目前呈現在唯讀模式中 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image48.png))


若要在我們需要將其插入模式中顯示 DetailsView`DefaultMode`屬性設`Inserting`。 這會呈現在插入模式下，當第一次瀏覽 DetailsView，並讓它那里之後插入新記錄。 如 [圖 17] 所示，這類 DetailsView 提供快速的介面加入新記錄。


[![DetailsView 快速加入一個新的產品提供的介面](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image49.png)

**圖 17**: DetailsView 提供的介面來快速地加入新的產品 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image51.png))


當使用者輸入的產品名稱和價格 （例如"Acme Water"和 1.99，如 圖 17 所示），並按 Insert 時，回傳是兩邊彼此乾瞪眼，並插入工作流程開始，已累積可新增至資料庫的新產品記錄。 DetailsView 會維護其插入的介面和 GridView 會自動重新繫結至其資料來源，在其中加入新的產品，如 圖 18 所示。


![產品](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image52.png)

**圖 18**： 產品"Acme Water"加入資料庫


雖然 圖 18 GridView 不會顯示，產品欄位從 DetailsView 介面缺乏`CategoryID`， `SupplierID`，`QuantityPerUnit`等指派`NULL`資料庫值。 您可以看到這藉由執行下列步驟：

1. 請移至 Visual Studio 中的 [伺服器總管]
2. 展開`NORTHWND.MDF`資料庫節點
3. 以滑鼠右鍵按一下`Products`資料庫資料表節點
4. 選取 顯示資料表資料

這會列出所有在記錄`Products`資料表。 如圖 19 所示，所有新產品的資料行以外`ProductID`， `ProductName`，並`UnitPrice`有`NULL`值。


[![在 DetailsView 中產品欄位未提供會指派 NULL 值](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image53.png)

**圖 19**: 產品欄位中未提供 DetailsView 指派`NULL`的值 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image55.png))


我們可能想要提供的預設值以外`NULL`的一個或多個資料行值，可能是因為`NULL`不是最佳的預設選項，或因為本身的資料庫資料行不允許`NULL`s。 若要這麼做我們可以以程式設計方式設定的 DetailsView 的參數值`InputParameters`集合。 這項指派可以在進行可能是在事件處理常式的 DetailsView`ItemInserting`事件或 ObjectDataSource 的`Inserting`事件。 因為我們已經討論在使用的前置和後置的層級的事件資料 Web 控制層級，讓我們將探討這次使用 ObjectDataSource 的事件。

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>步驟 4： 指派值給`CategoryID`和`SupplierID`參數

在本教學課程假設，我們將新增新的產品，透過這個介面時的應用程式應該將它指派`CategoryID`和`SupplierID`值為 1。 如先前所述，ObjectDataSource 有一組前置和後置的層級事件的引發資料修改程序期間。 當它`Insert()`叫用方法時，會先引發 ObjectDataSource 其`Inserting`事件，然後呼叫的方法，其`Insert()`方法都已經對應至，並最後引發`Inserted`事件。 `Inserting`事件處理常式會提供我們一個最後的機會，可以調整輸入的參數，或取消作業直接。

> [!NOTE]
> 在真實世界應用程式，您可能會想要讓使用者指定的類別和供應商或會挑選此值，根據一些準則或商務邏輯 （而非盲目地選取識別碼為 1）。 不論如何，此範例說明如何以程式設計方式設定 ObjectDataSource 的預先層級事件的 輸入參數的值。


請花一點時間來建立事件處理常式的 ObjectDataSource 的`Inserting`事件。 請注意，事件處理常式的第二個輸入的參數類型的物件`ObjectDataSourceMethodEventArgs`，其中包含要存取的參數集合的屬性 (`InputParameters`) 和取消作業的屬性 (`Cancel`)。


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample10.cs)]

在此時`InputParameters`屬性包含 ObjectDataSource 的`InsertParameters`DetailsView 從指派的值的集合。 若要變更其中一個參數的值，只要使用： `e.InputParameters["paramName"] = value`。 因此，若要設定`CategoryID`並`SupplierID`為 1 的值，調整`Inserting`事件處理常式，如下所示：


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample11.cs)]

此時間加入新的產品 （例如 Acme Soda) 時`CategoryID`和`SupplierID`新產品的資料行都設為 1 （請參閱圖 20）。


[![新的產品現在有其 CategoryID 和 SupplierID 值設定為 1](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image56.png)

**圖 20**： 新的產品現在有其`CategoryID`並`SupplierID`值設為 1 ([按一下以檢視完整大小的影像](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image58.png))


## <a name="summary"></a>總結

在編輯、 插入和刪除處理程序，資料 Web 控制項和 ObjectDataSource 進行的前置和後置的層級的事件數目。 在本教學課程中我們會檢查預先層級的事件，並可了解如何使用這些自訂的輸入的參數，或取消資料修改作業完全是同時從資料 Web 控制項和 ObjectDataSource 的事件。 在下一個教學課程中我們將探討建立和使用後置的層級的事件的事件處理常式。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Jackie Goor 和 Liz Shulok。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](an-overview-of-inserting-updating-and-deleting-data-cs.md)
> [下一頁](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)
