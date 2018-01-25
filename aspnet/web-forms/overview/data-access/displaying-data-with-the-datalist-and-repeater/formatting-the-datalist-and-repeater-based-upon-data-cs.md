---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs
title: "格式化 DataList 和中繼器根據資料 (C#) |Microsoft 文件"
author: rick-anderson
description: "在本教學課程，我們將逐步完成我們如何格式化 DataList 和中繼器控制項，藉由使用格式化的功能與外觀的範例..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 83e3d759-82b8-41e6-8d62-f0f4b3edec41
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 604aa63919a881e828b6a3620360c3d1133c5830
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="formatting-the-datalist-and-repeater-based-upon-data-c"></a>格式化 DataList 和中繼器根據資料 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_CS.exe)或[下載 PDF](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/datatutorial30cs1.pdf)

> 在本教學課程，我們將逐步執行的我們如何格式化 DataList 和中繼器控制項，使用範本中的格式化功能，或處理資料繫結事件的外觀範例。


## <a name="introduction"></a>簡介

如我們所見前述教學課程中，DataList 提供了一些會影響其外觀的樣式相關屬性。 特別是，我們可了解如何將預設的 CSS 類別指派給 DataList s `HeaderStyle`， `ItemStyle`， `AlternatingItemStyle`，和`SelectedItemStyle`屬性。 除了這四個屬性，DataList 包含數個其他樣式相關的屬性，例如`Font`， `ForeColor`， `BackColor`，和`BorderWidth`、 等等。 在中繼器控制項不包含任何樣式相關屬性。 必須直接在中繼器的範本中的標記內進行任何這類樣式設定。

通常，不過，格式化資料的方式取決於資料本身。 例如，列出產品時我們可能會想要顯示產品資訊的淺灰色的字型色彩已停用，或者我們想要反白顯示`UnitsInStock`值為零。 我們在先前的教學課程中所見，GridView、 DetailsView 和 FormView 會提供兩個不同的方式來格式化根據其資料的外觀：

- **`DataBound`事件**建立適當的事件處理常式`DataBound`資料已繫結至每個項目之後引發的事件 (的 gridview`RowDataBound`事件; DataList 和中繼器對於`ItemDataBound`事件)。 在該事件處理常式，繫結的資料就可加以檢查進行格式化的決策。 我們會檢查這項技巧[自訂格式化時資料](../custom-formatting/custom-formatting-based-upon-data-cs.md)教學課程。
- **格式函式範本中的**時使用 TemplateFields DetailsView 或 GridView 控制項或在 FormView 控制項中的範本中，我們可以將格式化的函式加入 ASP.NET 網頁 s 程式碼後置類別、 商務邏輯層或任何從 web 應用程式可以存取其他類別庫。 此種格式化函式可以接受任意數目的輸入參數，但必須傳回範本中呈現 HTML。 格式函式已先檢查[GridView 控制項中的使用 TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md)教學課程。

這兩種格式的技術都會提供 DataList 和中繼器控制項。 在本教學課程，我們將逐步執行這兩個控制項中使用這兩種技術的範例。

## <a name="using-theitemdataboundevent-handler"></a>使用`ItemDataBound`事件處理常式

當資料繫結至資料清單，從資料來源控制項，或是透過程式設計方式將資料指派給控制項 s`DataSource`屬性，並呼叫其`DataBind()`方法中，DataList 的`DataBinding`事件引發時，資料來源列舉與每個資料記錄繫結至資料清單。 資料來源中的每一筆記錄，建立在 DataList [ `DataListItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.aspx)也就是物件，然後繫結至目前的記錄。 在此過程中，DataList 會引發兩個事件：

- **`ItemCreated`**之後，都會引發`DataListItem`已建立
- **`ItemDataBound`**目前的記錄繫結至後引發`DataListItem`

下列步驟概述 DataList 控制項的資料繫結程序。

1. DataList s [ `DataBinding`事件](https://msdn.microsoft.com/library/system.web.ui.control.databinding.aspx)引發
2. 資料繫結至資料清單  
  
 資料來源中的每一筆記錄 

    1. 建立`DataListItem`物件
    2. 引發[`ItemCreated`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemcreated.aspx)
    3. 繫結至資料錄`DataListItem`
    4. 引發[`ItemDataBound`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx)
    5. 新增`DataListItem`至`Items`集合

資料繫結至在中繼器控制項中，當執行時透過完全相同的步驟序列。 唯一的差別，而不是`DataListItem`所建立的執行個體，會使用中繼器[ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)s。

> [!NOTE]
> 精明讀取器可能已注意到有些微的異常狀況之間的時會發生 DataList 中繼器會繫結至資料和 GridView 繫結至資料時的步驟順序。 在資料繫結程序的結尾結束時，引發 GridView`DataBound`事件; 不過，DataList 都中繼器控制項有這類事件。 這是因為之前的前置和後置層級的事件處理常式模式變得常用回中 ASP.NET 1.x 時間範圍內，建立 DataList 和中繼器控制項。


Like 與 GridView 中的格式化資料為基礎的其中一個選項是建立事件處理常式`ItemDataBound`事件。 這個事件處理常式會檢查有只繫結至資料`DataListItem`或`RepeaterItem`並且影響視控制項格式。

DataList 控制項，格式變更為整個項目可以使用實作`DataListItem`s 樣式相關屬性，其中包括標準`Font`， `ForeColor`， `BackColor`， `CssClass`，依此類推。 DataList 的範本內的特定 Web 控制項的格式設定的影響，我們需要以程式設計方式存取和修改這些 Web 控制項的樣式。 我們了解如何完成此回*自訂格式化時資料*教學課程。 要在中繼器控制項中，`RepeaterItem`類別具有任何樣式相關屬性，因此，所有樣式相關變更`RepeaterItem`中`ItemDataBound`事件處理常式必須以程式設計方式存取及更新中的 Web 控制項範本。

因為`ItemDataBound`DataList 和中繼器的幾乎完全相同，我們的範例將著重在使用 DataList 格式的技術。

## <a name="step-1-displaying-product-information-in-the-datalist"></a>步驟 1： 在資料清單中顯示產品資訊

我們會擔心格式之前，o d e s 會先建立用來顯示產品資訊的資料清單的頁面。 在[上一個教學課程](displaying-data-with-the-datalist-and-repeater-controls-cs.md)我們建立 DataList 其`ItemTemplate`顯示每個產品的名稱、 類別、 供應商、 每個單位和價格的數量。 可讓 s 在本教學課程中的重複以下這項功能。 若要完成這項作業，您可以在 DataList 和其 ObjectDataSource 從頭情況下，可能是重新建立，或您可以從上一個教學課程中建立的頁面上的這些控制複製 (`Basics.aspx`) 並將它們貼至網頁中，在此教學課程 (`Formatting.aspx`)。

一旦您已複寫的 DataList 和 ObjectDataSource 功能`Basics.aspx`到`Formatting.aspx`，請花一點時間變更 DataList s`ID`屬性從`DataList1`以更具描述性`ItemDataBoundFormattingExample`。 接下來，在瀏覽器中檢視資料清單。 如圖 1 所示，每項產品的唯一格式差別是背景色彩替代項目。


[![DataList 控制項中所列出的產品](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image1.png)

**圖 1**: 產品會列在 DataList 控制項 ([按一下以檢視完整大小的影像](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image3.png))


本教學課程，可讓任何產品的標價小於 $20.00 會有它的名稱和單位價格反白顯示的黃色的格式化 DataList s。

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>步驟 2： 以程式設計方式判斷 ItemDataBound 事件處理常式中的資料值

因為這些產品的價格在 $20.00 會有套用的自訂格式，所以我們必須能夠判斷每個產品的價格。 當資料繫結至資料清單中，DataList 列舉其資料來源中的記錄，然後會針對每一筆記錄，建立`DataListItem`執行個體，繫結至資料來源記錄`DataListItem`。 在特定資料錄 s 後資料已繫結至目前`DataListItem`物件，DataList 的`ItemDataBound`引發事件。 我們可以建立此事件，以檢查目前的資料值的事件處理常式`DataListItem`並根據這些值，進行必要的格式設定的任何變更。

建立`ItemDataBound`DataList 事件並加入下列程式碼：


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample1.cs)]

概念和 DataList s 背後的語意時`ItemDataBound`事件處理常式會與所使用的 GridView s 相同`RowDataBound`中的事件處理常式*自訂格式化時資料*教學課程中，語法與稍微。 當`ItemDataBound`事件引發`DataListItem`剛才對應的事件處理常式，透過傳入繫結至資料`e.Item`(而不是`e.Row`，就像使用 GridView 的`RowDataBound`事件處理常式)。 DataList s`ItemDataBound`事件處理常式引發的*每個*DataList，包括標頭資料列、 頁尾資料列，以及分隔符號資料列加入資料列。 不過，產品資訊只繫結至資料列。 因此，當使用`ItemDataBound`事件，以檢查的資料繫結至資料清單中，我們需要先確定我們重新處理資料的項目。 這可以透過檢查`DataListItem`s [ `ItemType`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx)，且可以包含的其中一個[下列八個值](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listitemtype.aspx):

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

同時`Item`和`AlternatingItem``DataListItem`的結構 DataList 的資料項目。 假設我們重新使用`Item`或`AlternatingItem`，所以我們存取實際`ProductsRow`已繫結至目前的執行個體`DataListItem`。 `DataListItem` s [ `DataItem`屬性](https://msdn.microsoft.com/system.web.ui.webcontrols.datalistitem.dataitem.aspx)包含參考`DataRowView`物件，其`Row`屬性提供參考實際`ProductsRow`物件。

接下來，我們會檢查`ProductsRow`執行個體的`UnitPrice`屬性。 因為 Products 資料表 s`UnitPrice`欄位可讓`NULL`值，再嘗試存取`UnitPrice`屬性我們應該先檢查是否有`NULL`值使用`IsUnitPriceNull()`方法。 如果`UnitPrice`值不是`NULL`，我們接著查看是否它小於 $20.00 s。 如果它確實會在 $20.00，我們需要套用自訂格式。

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>步驟 3： 反白顯示，產品的名稱和價格

我們已了解產品的價格是小於 $20.00，剩下的就是以反白顯示其名稱和價格。 若要達成此目的，我們必須先以程式設計方式參考中的標籤控制項`ItemTemplate`顯示產品的名稱和價格。 接下來，我們需要將它們顯示為黃色背景。 此格式的資訊可以直接修改標籤套用`BackColor`屬性 (`LabelID.BackColor = Color.Yellow`); 在理想情況下，不過，所有顯示相關的重要應該透過階層式樣式表來都表示。 實際上，我們已經有提供所要的格式中定義的樣式表`Styles.css`  -  `AffordablePriceEmphasis`，已建立和討論*自訂格式化時資料*教學課程。

若要套用的格式，直接將兩個標籤 Web 控制項`CssClass`屬性`AffordablePriceEmphasis`，如下列程式碼所示：


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample2.cs)]

與`ItemDataBound`事件處理常式已完成、 重新瀏覽`Formatting.aspx`瀏覽器中的。 如圖 2 所示，這些產品的價格在 $20.00 同時具有其名稱及反白顯示的價格。


[![這些產品小於 $20.00 會反白顯示](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image4.png)

**圖 2**： 那些產品小於 $20.00 會反白顯示 ([按一下以檢視完整大小的影像](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image6.png))


> [!NOTE]
> 因為在 DataList 會轉譯為 HTML `<table>`、 其`DataListItem`執行個體具有可以將特定樣式套用至整個項目設定的樣式相關屬性。 比方說，如果我們想要反白顯示*整個*項目黃色其價格小於 $20.00 時，我們無法已取代的程式碼參考的標籤，並設定其`CssClass`與下列程式碼行的屬性： `e.Item.CssClass = "AffordablePriceEmphasis"`（請參閱圖 3）。


`RepeaterItem`構成中繼器控制項中，不過，不要 t s 提供這類層級的樣式屬性。 因此，套用自訂格式設定中繼器需要應用程式樣式屬性的 Web 控制項內中繼器的範本，如同我們在圖 2 中。


[![整個產品項目會反白顯示的產品下 $20.00](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image7.png)

**圖 3**： 整個產品項目會反白顯示的產品下 $20.00 ([按一下以檢視完整大小的影像](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image9.png))


## <a name="using-formatting-functions-from-within-the-template"></a>使用從範本中的格式化功能

在*GridView 控制項中的使用 TemplateFields*教學課程中我們可了解如何使用 GridView TemplateField 內的格式化函式來套用自訂格式，根據的資料繫結至 GridView s 資料列。 格式函式是方法，可以叫用從範本，並傳回在其所在位置中發出的 HTML。 格式函式可以位於 ASP.NET 頁面 s 程式碼後置類別或類別中的檔案可以集中式`App_Code`資料夾或個別的類別庫專案中。 如果您打算使用相同的格式函式，在多個 ASP.NET 網頁或其他的 ASP.NET web 應用程式的理想移動格式化函式，從 ASP.NET 頁面 s 程式碼後置類別。

為了示範格式化功能，讓 s 有產品資訊包含產品的名稱旁的 [DISCONTINUED] 的文字，如果它已停止 s。 此外，讓 s 有價格反白顯示黃色 if 它小於 $20.00 s (如同`ItemDataBound`事件處理常式範例); 如果價格為 $20.00 或更高版本，讓 s 無法顯示實際的價格，但是針對價格引號的文字，請改為呼叫。 圖 4 顯示這些套用的格式化規則所列出的產品的螢幕擷取畫面。


[![適用於高度耗費資源的產品，價格會取代文字，請呼叫價格報價](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image10.png)

**圖 4**： 昂貴的產品，價格會取代文字，請呼叫價格報價 ([按一下以檢視完整大小的影像](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image12.png))


## <a name="step-1-create-the-formatting-functions"></a>步驟 1： 建立格式化的功能

此範例中我們需要兩個格式函式，其中顯示的文字 [DISCONTINUED]，以及產品名稱，如有需要和另一個顯示反白顯示的價格，如果它小於 $20.00 或該文字，請呼叫價格報價，否則為 s。 可讓 ASP.NET 頁面 s 程式碼後置類別中建立這些函式，並加以命名 s`DisplayProductNameAndDiscontinuedStatus`和`DisplayPrice`。 這兩種方法需要傳回以字串形式呈現的 HTML，兩者必須標示`Protected`(或`Public`) 若要從 ASP.NET 頁面 s 宣告式語法部分叫用。 這兩種方法的程式碼如下：


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample3.cs)]

請注意，`DisplayProductNameAndDiscontinuedStatus`方法可接受的值`productName`和`discontinued`資料欄位做為純量值，而`DisplayPrice`方法會接受`ProductsRow`執行個體 (而非`unitPrice`純量值)。 其中一個方法才有作用。不過，格式化函式使用可以包含資料庫的純量值`NULL`值 (例如`UnitPrice`; 都不`ProductName`也`Discontinued`允許`NULL`值)，必須採取特別注意，在處理這些純量輸入。

特別是，輸入的參數必須屬於型別`Object`傳入的值可能會因為`DBNull`而不是預期的資料類型的執行個體。 此外，必須進行檢查以判斷輸入的值是否為資料庫`NULL`值。 也就是說，如果我們想`DisplayPrice`方法，以接受價格為純量值時，我們 d 必須使用下列程式碼：


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample4.cs)]

請注意，`unitPrice`輸入的參數的類型是`Object`和已修改條件陳述式，以確定如果`unitPrice`是`DBNull`與否。 此外，因為`unitPrice`當做傳入輸入的參數`Object`，必須將它轉換成十進位值。

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>步驟 2： 從 DataList s ItemTemplate 呼叫格式化函式

格式函式加入至我們的 ASP.NET 頁面 s 程式碼後置類別中，剩下的就是要叫用這些格式化函式從 DataList 的`ItemTemplate`。 若要從範本呼叫格式化函式，放入該函式呼叫內的資料繫結語法：


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample5.aspx)]

在 DataList s `ItemTemplate` `ProductNameLabel`標籤 Web 控制項目前顯示的產品的名稱藉由指派其`Text`屬性結果的`<%# Eval("ProductName") %>`。 若要讓它顯示的名稱加上的文字 [DISCONTINUED]，如有需要更新宣告式語法，使改為將指派`Text`屬性值的`DisplayProductNameAndDiscontinuedStatus`方法。 當此情況下，我們就必須傳入 s 產品名稱和已停止的值使用`Eval("columnName")`語法。 `Eval`傳回值的型別`Object`，但`DisplayProductNameAndDiscontinuedStatus`方法預期輸入的參數的型別`String`和`Boolean`; 因此，我們必須將傳回的值轉型`Eval`方法預期的輸入的參數類型，就像這樣：


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample6.aspx)]

若要顯示的價格，我們可以直接將`UnitPriceLabel`標籤 s`Text`屬性所傳回的值為`DisplayPrice`方法，如同我們並未顯示產品的名稱，而且 [停用] 文字。 不過，而不是傳入`UnitPrice`做為純量的輸入參數，而是傳遞整個`ProductsRow`執行個體：


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample7.aspx)]

透過就地格式的函式呼叫，請花一點時間瀏覽器中檢視進度。 您的畫面看起來應該類似圖 5 已停止的產品，包括 [DISCONTINUED] 的文字和成本超過 $20.00 需其價格的產品取代為文字，請呼叫報價。


[![適用於高度耗費資源的產品，價格會取代文字，請呼叫價格報價](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image13.png)

**圖 5**： 昂貴的產品，價格會取代文字，請呼叫價格報價 ([按一下以檢視完整大小的影像](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image15.png))


## <a name="summary"></a>總結

格式設定 DataList 或中繼器控制項的資料為基礎的內容可以透過兩種技術。 第一種技術是建立事件處理常式`ItemDataBound`引發的事件，因為資料來源中的每一筆記錄會繫結至新`DataListItem`或`RepeaterItem`。 在`ItemDataBound`事件處理常式，可檢查目前的項目的資料和格式可以套用至內容的範本，或如`DataListItem`距整個項目本身。

或者，自訂格式可以實現透過格式函式。 在 DataList 叫用的方法或中繼器的範本傳回發出其所在位置的 HTML 格式的函式。 格式函式所傳回的 HTML，通常取決於所繫結至目前的項目值。 這些值可以傳遞到格式的函式、 純量值，或是透過傳入繫結項目至整個物件 (例如`ProductsRow`執行個體)。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 本教學課程中的前導檢閱者已 Yaakov Ellis、 袁 Schmidt 和 Liz Shulok。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一頁](displaying-data-with-the-datalist-and-repeater-controls-cs.md)
[下一頁](showing-multiple-records-per-row-with-the-datalist-control-cs.md)
