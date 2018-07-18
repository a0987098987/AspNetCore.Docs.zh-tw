---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
title: 格式化 DataList 和 Repeater 根據資料 (VB) |Microsoft Docs
author: rick-anderson
description: 在本教學課程，我們要逐步執行範例的我們如何格式化 DataList 和 Repeater 控制項，藉由使用具有格式設定函式的外觀...
ms.author: aspnetcontent
ms.date: 09/13/2006
ms.assetid: e2f401ae-37bb-4b19-aa97-d6b385d40f88
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 428438b2bae062c09d13c002f4729c3c394975a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807706"
---
<a name="formatting-the-datalist-and-repeater-based-upon-data-vb"></a>格式化 DataList 和 Repeater 根據資料 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_VB.exe)或[下載 PDF](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/datatutorial30vb1.pdf)

> 在本教學課程，我們將逐步執行的我們如何格式化 DataList 和 Repeater 控制項，使用在範本內的格式化功能，或處理資料繫結事件的外觀範例。


## <a name="introduction"></a>簡介

如我們所見在先前的教學課程中，DataList 會提供一些會影響其外觀的樣式相關屬性。 特別是，我們了解如何將預設的 CSS 類別指派給 DataList s `HeaderStyle`， `ItemStyle`， `AlternatingItemStyle`，和`SelectedItemStyle`屬性。 除了這四個屬性，DataList 包含數個其他樣式相關的屬性，例如`Font`， `ForeColor`， `BackColor`，和`BorderWidth`、 等等。 在 Repeater 控制項不包含任何的樣式相關屬性。 直接在中繼器的範本中的標記內，都必須進行任何這類的樣式設定。

通常，不過，格式化資料的方式取決於資料本身。 比方說，列出產品時我們可能會想要以淺灰色字型色彩顯示產品資訊，它已停止，則我們可能想要反白顯示`UnitsInStock`值為零。 我們在先前的教學課程中所見的因為 GridView、 DetailsView 和 FormView 提供了兩個不同的方式，把它們根據其資料的外觀：

- **`DataBound`事件**建立適當的事件處理常式`DataBound`，所引發的事件資料已繫結至每個項目之後 (它是 gridview`RowDataBound`事件; DataList 與重複項對於`ItemDataBound`事件)。 在該事件處理常式，繫結的資料只要可以檢查進行格式化的決策。 我們檢查此技巧[自訂格式化時資料](../custom-formatting/custom-formatting-based-upon-data-vb.md)教學課程。
- **格式函式範本中的**時 DetailsView 或 GridView 控制項或 FormView 控制項中的範本中使用 TemplateFields，我們可以將格式化函式新增至 ASP.NET 頁面 s 程式碼後置類別、 商務邏輯層，或任何可從 web 應用程式存取其他類別庫。 此格式的函式可以接受任意數目的輸入參數，但必須傳回要在範本中所呈現之 HTML。 格式函式第一次檢查[使用 GridView 控制項中的 TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md)教學課程。

這兩種格式的技術都可使用 DataList 與重複項控制項。 在本教學課程，我們將逐步執行這兩個控制項使用這兩種技巧的範例。

## <a name="using-theitemdataboundevent-handler"></a>使用`ItemDataBound`事件處理常式

當資料繫結至 DataList 中，從資料來源控制項，或是透過程式設計方式將資料指派給控制項 s`DataSource`屬性，並呼叫其`DataBind()`方法中，DataList 的`DataBinding`事件引發時，資料來源列舉而且，每個資料記錄繫結至 DataList。 資料來源中的每一筆記錄，建立 DataList [ `DataListItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.aspx)也就是物件，然後繫結至目前的記錄。 在此過程中，DataList 會引發兩個事件：

- **`ItemCreated`** 之後，就會引發`DataListItem`已建立
- **`ItemDataBound`** 已繫結至的目前記錄之後，就會引發 `DataListItem`

下列步驟概述 DataList 控制項的資料繫結程序。

1. DataList s [ `DataBinding`事件](https://msdn.microsoft.com/library/system.web.ui.control.databinding.aspx)引發
2. 資料繫結至 DataList  
  
   資料來源中的每一筆記錄 

    1. 建立`DataListItem`物件
    2. 火焰[`ItemCreated`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemcreated.aspx)
    3. 繫結至資料錄 `DataListItem`
    4. 火焰[`ItemDataBound`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx)
    5. 新增`DataListItem`至`Items`集合

當資料繫結至 Repeater 控制項，在各步驟的確切的相同順序。 唯一的差別在於，而不是`DataListItem`所建立的執行個體，會使用 Repeater [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)s。

> [!NOTE]
> 精明的讀者可能已經發現的 DataList 與重複項繫結至資料與 GridView 繫結至資料時會的步驟順序之間的些微異常。 在資料繫結程序的結尾結束時，引發 GridView`DataBound`事件; 不過，收錄 DataList 或 Repeater 控制項有這類事件。 這是因為之前的前置和後置的層級的事件處理常式模式已成為一般在 ASP.NET 1.x 時間範圍內，建立 DataList 與重複項控制項。


像是 GridView、 格式化以資料為依據的其中一個選項是建立的事件處理常式`ItemDataBound`事件。 這個事件處理常式會檢查有只繫結到的資料`DataListItem`或`RepeaterItem`並且影響視控制項格式。

DataList 控制項，格式為整個項目可以使用實作的變更`DataListItem`s 與樣式相關屬性，其中包含標準`Font`， `ForeColor`， `BackColor`， `CssClass`，依此類推。 若要影響的 DataList 的範本內的特定 Web 控制項格式，我們需要以程式設計方式存取和修改這些 Web 控制項的樣式。 我們了解如何完成此回溯*自訂格式化時資料*教學課程。 例如中繼器控制項中，`RepeaterItem`類別有任何與樣式相關屬性; 因此，所有與樣式相關的變更會對`RepeaterItem`在`ItemDataBound`事件處理常式必須以程式設計方式存取及更新中的 Web 控制項範本中。

因為`ItemDataBound`格式化 DataList 和 Repeater 幾乎都相同，我們的範例著重於使用 DataList 項技術。

## <a name="step-1-displaying-product-information-in-the-datalist"></a>步驟 1: DataList 中顯示產品資訊

我們會擔心格式之前，讓 s 首先會建立用來顯示產品資訊的 DataList 的頁面。 在 [先前的教學課程](displaying-data-with-the-datalist-and-repeater-controls-vb.md)我們建立了 DataList 其`ItemTemplate`顯示每個產品的名稱、 類別、 供應商、 每個單位和價格的數量。 可讓重複這項功能在此本教學課程中的 s。 若要這麼做，您可以 DataList 和其 ObjectDataSource 從頭情況下，可能是重新建立，或者您可以從先前的教學課程中建立的頁面複製這些控制項 (`Basics.aspx`) 將其貼到頁面中，在本教學課程 (`Formatting.aspx`)。

一旦您已複寫的 DataList 與 ObjectDataSource 的功能，從`Basics.aspx`成`Formatting.aspx`，請花一點時間變更 DataList s`ID`屬性從`DataList1`以更具描述性`ItemDataBoundFormattingExample`。 接下來，在瀏覽器中檢視 DataList。 如 [圖 1] 所示，唯一格式化每個產品之間的差別的背景色彩會交替出現。


[![DataList 控制項中所列的產品](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image1.png)

**圖 1**: 產品會列在 DataList 控制項 ([按一下以檢視完整大小的影像](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image3.png))


本教學課程中，讓格式化 DataList，使得標價小於 $ 20.00 美元的任何產品會有它的名稱，而單位價格反白顯示的黃色的 s。

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>步驟 2： 以程式設計方式判斷 ItemDataBound 事件處理常式中的資料值

由於這些產品價格下 $ 20.00 美元會有套用的自訂格式，我們必須能夠判斷每個產品的價格。 當資料繫結至 DataList，DataList 列舉其資料來源中的記錄，然後針對每一筆記錄，會建立`DataListItem`執行個體，繫結至資料來源記錄`DataListItem`。 在特定的資料錄 s 後資料繫結至目前`DataListItem`物件，DataList 的`ItemDataBound`引發事件。 我們可以建立這個事件，以查看目前的資料值的事件處理常式`DataListItem`並根據這些值，進行所需的任何格式的變更。

建立`ItemDataBound`DataList 的事件，並新增下列程式碼：


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample1.vb)]

概念和 DataList s 背後的語意時`ItemDataBound`事件處理常式會與所使用的 GridView s 相同`RowDataBound`中的事件處理常式*自訂格式設定在資料*教學課程中，語法之間差異稍微。 當`ItemDataBound`事件引發時，`DataListItem`只是繫結至資料會傳遞至透過對應的事件處理常式`e.Item`(而非`e.Row`，就像使用 GridView 的`RowDataBound`事件處理常式)。 DataList s`ItemDataBound`針對觸發的事件處理常式*每個*新增至 DataList 和包括標頭資料列，頁尾資料列分隔符號資料列的資料列。 不過，產品資訊只會繫結至資料列。 因此，當使用`ItemDataBound`事件，以檢查的資料繫結至 DataList 中，我們需要先確定我們重新處理資料的項目。 這可藉由檢查`DataListItem`s [ `ItemType`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx)，且可以包含的其中一個[下列八個值](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listitemtype.aspx):

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

兩者`Item`和`AlternatingItem``DataListItem`的結構 DataList s 中的資料項目。 假設我們再次使用`Item`或是`AlternatingItem`，我們存取實際`ProductsRow`繫結至目前的執行個體`DataListItem`。 `DataListItem` s [ `DataItem`屬性](https://msdn.microsoft.com/system.web.ui.webcontrols.datalistitem.dataitem.aspx)包含的參考`DataRowView`物件，其`Row`屬性提供的實際參考`ProductsRow`物件。

接下來，我們會檢查`ProductsRow`執行個體的`UnitPrice`屬性。 因為產品資料表 s`UnitPrice`欄位可讓`NULL`值，然後再嘗試存取`UnitPrice`屬性我們應該先檢查以查看是否有`NULL`值使用`IsUnitPriceNull()`方法。 如果`UnitPrice`值不是`NULL`，我們接著查看是否它 s 小於 $ 20.00 美元。 如果它確實會在 $ 20.00 美元，我們需要套用自訂格式。

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>步驟 3： 反白顯示的產品名稱和價格

一旦我們知道產品的價格是小於為 $ 20.00 美元，全都是反白顯示它的名稱和價格。 若要達成此目的，我們必須先以程式設計方式參考中的標籤控制項`ItemTemplate`，顯示產品的名稱和價格。 接下來，我們需要將它們顯示為黃色背景。 此格式的資訊可以直接修改標籤套用`BackColor`屬性 (`LabelID.BackColor = Color.Yellow`); 在理想情況下，不過，會透過階層式樣式表中表示所有顯示相關的事物。 事實上，我們已經有提供所要的格式中所定義的樣式表`Styles.css`  -  `AffordablePriceEmphasis`，已建立和討論*自訂格式化時資料*教學課程。

若要套用格式設定，請設定兩個 Label Web 控制項`CssClass`屬性，以`AffordablePriceEmphasis`，如下列程式碼所示：


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample2.vb)]

具有`ItemDataBound`事件處理常式已完成、 重新瀏覽`Formatting.aspx`瀏覽器中的頁面。 圖 2 所示，在 $ 20.00 美元價格的產品有其名稱 」 和 「 反白顯示的價格。


[![這些產品小於 $ 20.00 美元會反白顯示](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image4.png)

**圖 2**： 這些產品小於 $ 20.00 美元會反白顯示 ([按一下以檢視完整大小的影像](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image6.png))


> [!NOTE]
> 因為 DataList 會轉譯為 HTML `<table>`、 其`DataListItem`執行個體都可以將特定的樣式套用至整個項目設定的樣式相關屬性。 比方說，如果我們想要反白顯示*整個*其價格的時間小於為 $ 20.00 美元，則項目黃色，我們可能已取代的程式碼參考的標籤，並將其`CssClass`具有下列程式碼行的屬性： `e.Item.CssClass = "AffordablePriceEmphasis"`（請參閱 圖 3）。


`RepeaterItem`構成 Repeater 控制項，不過，don t s 提供這類樣式層級屬性。 因此，套用自訂格式 Repeater 需要應用程式樣式屬性的 Web 控制項 Repeater s 在範本內，就像我們在圖 2 中。


[![整個產品項目會反白顯示的產品在 $ 20.00 美元](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image7.png)

**圖 3**: 整個產品項目會反白顯示的產品在 $ 20.00 美元 ([按一下以檢視完整大小的影像](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image9.png))


## <a name="using-formatting-functions-from-within-the-template"></a>使用 從範本內的格式化功能

在 *使用 GridView 控制項中的 TemplateFields*教學課程中我們了解如何使用 GridView TemplateField 內格式化函式來套用自訂格式，根據資料繫結至 GridView s 資料列。 格式函式是方法，可以從範本中叫用，並傳回在其位置，就會發出 HTML。 格式函式可以位於 ASP.NET 頁面 s 程式碼後置類別，或可以集中式中的類別檔案到`App_Code`資料夾或個別的類別庫專案中。 將格式化的函式，從 ASP.NET 頁面 s 程式碼後置類別移是理想，如果您計劃在多個 ASP.NET 網頁或其他 ASP.NET web 應用程式中使用相同的格式設定函式。

為了示範格式化功能，可讓 s 有產品資訊包含產品的名稱旁邊的 [DISCONTINUED] 的文字，如果它已停止的 s。 此外，可讓 s 有價格反白顯示黃色如果它小於 $ 20.00 美元 s (如同我們在`ItemDataBound`事件處理常式範例); 如果價格是 $ 20.00 美元或更高版本，可讓 s 不會顯示實際的價格，但價格報價的文字，請改為呼叫。 [圖 4] 顯示的螢幕擷取畫面列出套用這些格式化規則的產品。


[![適用於昂貴的產品，價格會取代文字，請呼叫價格報價](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image10.png)

**圖 4**： 適用於昂貴的產品，價格會取代文字，請呼叫價格報價 ([按一下以檢視完整大小的影像](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image12.png))


## <a name="step-1-create-the-formatting-functions"></a>步驟 1： 建立格式化的函式

此範例中我們需要兩種格式化函式，其中會顯示產品名稱與文字 [DISCONTINUED]，如有需要和另一個顯示其中一個反白顯示的價格，如果它小於 $ 20.00 美元或該文字，請呼叫價格報價，否則為 s。 可讓 ASP.NET 頁面 s 程式碼後置類別中建立這些函數及它們的名稱`DisplayProductNameAndDiscontinuedStatus`和`DisplayPrice`。 這兩種方法需要傳回的 HTML 字串形式呈現，而且兩者都必須標示`Protected`(或`Public`) 才能叫用從 ASP.NET 頁面 s 宣告式語法部分。 這兩種方法的程式碼如下：


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample3.vb)]

請注意，`DisplayProductNameAndDiscontinuedStatus`方法可接受的值`productName`並`discontinued`資料欄位做為純量值，而`DisplayPrice`方法會接受`ProductsRow`執行個體 (而非`unitPrice`純量值)。 兩種方法將運作;不過，如果格式化的函式使用可以包含資料庫的純量值`NULL`值 (這類`UnitPrice`; 不`ProductName`也不`Discontinued`允許`NULL`值)，特別小心處理這些純量輸入。

特別是，輸入的參數必須屬於型別`Object`連入的值可能是因為`DBNull`而不是預期的資料類型的執行個體。 此外，必須進行檢查以判斷輸入的值是否為資料庫`NULL`值。 也就是說，如果我們想`DisplayPrice`一定要使用下列程式碼的方法，以接受價格為純量值時，我們的 d:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample4.vb)]

請注意，`unitPrice`輸入的參數的類型是`Object`並已修改過的條件陳述式來確定如果`unitPrice`是`DBNull`與否。 此外，因為`unitPrice`當做傳入輸入的參數`Object`，必須將它轉換成十進位值。

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>步驟 2： 從 DataList 的 ItemTemplate 呼叫格式化函式

格式函式新增到 ASP.NET 頁面 s 程式碼後置類別中，剩下的就是叫用這些格式化 DataList s 函式`ItemTemplate`。 若要從範本呼叫格式化函式，放入該函式呼叫內的資料繫結語法：


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample5.aspx)]

在 DataList s `ItemTemplate` `ProductNameLabel` Label Web 控制項目前有顯示產品的名稱指派其`Text`屬性結果的`<%# Eval("ProductName") %>`。 為了讓它顯示名稱加上文字 [DISCONTINUED]，如有需要更新的宣告式語法，使它改為將指派`Text`屬性值的`DisplayProductNameAndDiscontinuedStatus`方法。 當這麼做，我們必須傳遞 s 產品名稱和已停止使用的值`Eval("columnName")`語法。 `Eval` 傳回值的型別`Object`，但`DisplayProductNameAndDiscontinuedStatus`方法預期的輸入的參數的型別`String`並`Boolean`; 因此，我們必須轉換所傳回的值`Eval`方法預期的輸入的參數類型，就像這樣：


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample6.aspx)]

若要顯示的價格，我們可以直接設定`UnitPriceLabel`標籤 s`Text`屬性所傳回的值為`DisplayPrice`方法，就像我們未顯示產品的名稱和 [停用] 文字。 不過，而不是傳入`UnitPrice`做為純量的輸入參數，我們改為傳入整個`ProductsRow`執行個體：


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample7.aspx)]

就地格式化函式呼叫，請花一點時間瀏覽器中檢視進度。 您的畫面看起來應該類似圖 5 已停止的產品，包含文字 [DISCONTINUED] 而且成本超過 $ 20.00 美元具有其價格的產品以取代文字請價格報價的呼叫。


[![適用於昂貴的產品，價格會取代文字，請呼叫價格報價](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image13.png)

**圖 5**： 適用於昂貴的產品，價格會取代文字，請呼叫價格報價 ([按一下以檢視完整大小的影像](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image15.png))


## <a name="summary"></a>總結

格式化 DataList 或 Repeater 控制項資料為基礎的內容可以透過兩種技術。 第一個技巧是要建立的事件處理常式`ItemDataBound`引發的事件，因為資料來源中的每一筆記錄會繫結至新`DataListItem`或`RepeaterItem`。 在 `ItemDataBound`事件處理常式，可檢查目前的項目的資料，然後格式化可以套用至內容的範本，或針對`DataListItem`s，整個項目本身。

或者，自訂格式可以實現透過格式設定函式。 可以從 DataList 中叫用方法或 Repeater 的範本，傳回要在其位置中發出的 HTML 格式的函式。 格式函式所傳回的 HTML，通常取決於繫結至目前的項目值。 這些值可以傳遞到格式化的函式，做為純量值或傳遞整個物件繫結至項目 (例如`ProductsRow`執行個體)。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Yaakov Ellis、 Randy Schmidt 和 Liz Shulok。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
> [下一頁](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
