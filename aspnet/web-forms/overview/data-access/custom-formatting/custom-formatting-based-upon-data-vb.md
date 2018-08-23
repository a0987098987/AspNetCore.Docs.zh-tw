---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
title: 根據自訂格式設定資料 (VB) |Microsoft Docs
author: rick-anderson
description: 以多種方式可以完成 GridView、 DetailsView 或 FormView 繫結至該資料為基礎的格式調整。 在本教學課程中，我們將 l...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: df5a1525-386f-4632-972c-57b199870bc3
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: d902fd6d042783c036bb42a11b7e469f6dd2b5b6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832552"
---
<a name="custom-formatting-based-upon-data-vb"></a>根據自訂格式設定資料 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_11_VB.exe)或[下載 PDF](custom-formatting-based-upon-data-vb/_static/datatutorial11vb1.pdf)

> 以多種方式可以完成 GridView、 DetailsView 或 FormView 繫結至該資料為基礎的格式調整。 在本教學課程中我們將探討如何完成資料繫結格式透過使用 DataBound 和 RowDataBound 事件處理常式。


## <a name="introduction"></a>簡介

透過大量的樣式相關屬性，您可以自訂 GridView、 DetailsView 和 FormView 控制項的外觀。 屬性，例如`CssClass`， `Font`， `BorderWidth`， `BorderStyle`， `BorderColor`， `Width`，和`Height`，其他項目，指定一般轉譯的控制項的外觀。 屬性，包括`HeaderStyle`， `RowStyle`， `AlternatingRowStyle`，和其他項目允許這些相同的樣式設定来套用至特定區段。 同樣地，可以在欄位層級套用這些樣式設定。

在許多情況下，格式需求取決於所顯示資料的值。 例如，以強調出的內建的產品，報告，列出產品資訊可能會設定背景色彩設為黃色，這些產品的`UnitsInStock`和`UnitsOnOrder`欄位都等於 0。 反白顯示的昂貴的產品，我們可能想要顯示的成本超過美金 75.00 以粗體字這些產品的價格。

以多種方式可以完成 GridView、 DetailsView 或 FormView 繫結至該資料為基礎的格式調整。 在本教學課程中我們將探討如何完成資料繫結使用的格式`DataBound`和`RowDataBound`事件處理常式。 在下一個教學課程中，我們將探討一個替代方法。

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>使用 DetailsView 控制項的`DataBound`事件處理常式

當資料繫結至 DetailsView 中，從資料來源控制項，或是透過程式設計方式將資料指派給控制項的`DataSource`屬性，並呼叫其`DataBind()`方法，下列步驟順序發生：

1. 資料 Web 控制項`DataBinding`引發事件。
2. 資料繫結至資料 Web 控制項。
3. 資料 Web 控制項`DataBound`引發事件。

可透過事件處理常式的步驟 1 和 3 之後，立即插入自訂邏輯。 藉由建立的事件處理常式`DataBound`我們以程式設計方式決定已資料繫結至資料 Web 控制項和調整格式所需的事件。 為了說明這點我們建立會列出產品的一般資訊，但會顯示 DetailsView`UnitPrice`中的值***粗體、 斜體字型***如果它超過美金 75.00。

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>步驟 1： 在 DetailsView 中顯示的產品資訊

開啟`CustomColors.aspx`頁面中`CustomFormatting`資料夾中，將 DetailsView 控制項從工具箱拖曳至設計工具，將其`ID`屬性值設`ExpensiveProductsPriceInBoldItalic`，並將它繫結至叫用的新 ObjectDataSource 控制項`ProductsBLL`類別的`GetProducts()`方法。 為求簡潔，因為我們這些詳細檢查的先前教學課程中完成這項作業的詳細的步驟會這裡省略。

一旦您已受限於 DetailsView ObjectDataSource，花點時間修改的欄位清單。 我選擇移除`ProductID`， `SupplierID`， `CategoryID`， `UnitsInStock`， `UnitsOnOrder`， `ReorderLevel`，和`Discontinued`BoundFields 和重新命名，並重新格式化剩餘 BoundFields。 我也清除`Width`和`Height`設定。 DetailsView 顯示單一記錄，因為我們需要啟用分頁功能，以允許使用者檢視的所有產品。 達到此目的的 DetailsView 的智慧標籤的 啟用分頁核取方塊。


[![圖 1： 檢查 DetailsView 的智慧標籤啟用分頁核取方塊](custom-formatting-based-upon-data-vb/_static/image2.png)](custom-formatting-based-upon-data-vb/_static/image1.png)

**圖 1**: 圖 1： 檢查啟用分頁中的核取 DetailsView 的智慧標籤 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-vb/_static/image3.png))


這些變更之後，請將 DetailsView 標記：


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample1.aspx)]

請花一點時間來測試您的瀏覽器中的此頁面。


[![在 DetailsView 控制項一次顯示一項產品](custom-formatting-based-upon-data-vb/_static/image5.png)](custom-formatting-based-upon-data-vb/_static/image4.png)

**圖 2**: DetailsView 控制項顯示一個產品一次 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-vb/_static/image6.png))


## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>步驟 2： 以程式設計方式判斷資料繫結事件處理常式中的資料值

若要顯示的價格以粗體、 斜體的字型，這些產品的`UnitPrice`值超過美金 75.00，我們必須先能夠以程式設計方式判斷`UnitPrice`值。 針對 DetailsView，這可以透過完成`DataBound`事件處理常式。 若要建立事件處理常式會 DetailsView 設計工具中按一下，然後瀏覽至 [屬性] 視窗。 按 f4 鍵以啟動它，如果它不是可見的或移至 [檢視] 功能表，然後選取 [屬性視窗] 功能表選項。 從 [屬性] 視窗中，按一下閃電圖示來列出 DetailsView 的事件。 接下來，請按兩下`DataBound`事件或輸入您想要建立的事件處理常式的名稱。


![建立資料繫結事件的事件處理常式](custom-formatting-based-upon-data-vb/_static/image7.png)

**圖 3**： 建立事件處理常式`DataBound`事件


> [!NOTE]
> 您也可以從 ASP.NET 頁面的程式碼部分，來建立事件處理常式。 您將在這裡找到在頁面頂端的兩個下拉式清單。 從左邊的下拉式清單中選取物件，以及您想要從權限下拉式清單和 Visual Studio 中建立的處理常式的事件會自動建立適當的事件處理常式。


這樣會自動建立事件處理常式，並帶您前往的程式碼部分加入其中。 此時您會看到：


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample2.vb)]

DetailsView 繫結的資料可透過存取`DataItem`屬性。 回想一下，我們會將我們的控制項繫結至強型別的 DataTable，是由強型別 DataRow 執行個體的集合所組成。 DataTable 繫結至 DetailsView，第一個 DataRow 的 DataTable 中會指派給 DetailsView`DataItem`屬性。 具體而言，`DataItem`屬性會被指派`DataRowView`物件。 我們可以使用`DataRowView`的`Row`屬性來取得存取基礎的 DataRow 物件，也就是實際上`ProductsRow`執行個體。 一旦我們有了這個`ProductsRow`我們可以讓我們的決策，只要檢查物件的屬性值的執行個體。

下列程式碼說明如何判斷是否`UnitPrice`繫結至 DetailsView 控制項的值是否大於美金 75.00:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample3.vb)]

> [!NOTE]
> 由於`UnitPrice`可以有`NULL`資料庫中的值，我們先檢查以確定我們要未處理的`NULL`值，才能存取`ProductsRow`的`UnitPrice`屬性。 這項檢查，請務必因為如果我們嘗試存取`UnitPrice`屬性具有時`NULL`值`ProductsRow`物件將會擲回[StrongTypingException 例外狀況](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx)。


## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>步驟 3： 設定格式化的 DetailsView UnitPrice 值

此時我們可以判斷是否`UnitPrice`繫結至 DetailsView 值超過美金 75.00，但我們尚未以了解如何以程式設計方式調整 DetailsView 的據以格式化。 若要修改在 DetailsView 中整個資料列的格式，以程式設計方式存取資料列使用`DetailsViewID.Rows(index)`; 若要修改特定的資料格，存取使用`DetailsViewID.Rows(index).Cells(index)`。 一旦我們擁有之資料列或資料格的參考我們接著可以藉由設定其樣式相關屬性來調整它的外觀。

以程式設計方式存取資料列，您需要知道的資料列的索引，從 0 開始。 `UnitPrice` DetailsView，提供 4 的索引，並且讓它以程式設計方式存取的第五個資料列資料列為使用`ExpensiveProductsPriceInBoldItalic.Rows(4)`。 此時我們可以使用下列程式碼顯示粗體、 斜體字型中的整個資料列的內容：


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample4.vb)]

不過，這會讓*兩者*標籤 （價格） 和粗體和斜體的值。 如果我們想要只是值粗體和斜體我們要將此套用的第二個資料格中的資料列，即可使用下列格式：


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample5.vb)]

由於我們的教學課程到目前為止已使用樣式表維持清楚的分隔，呈現的標記與樣式相關的資訊之間，而不是設定特定的樣式屬性，如上所示讓我們改為使用 CSS 類別。 開啟`Styles.css`樣式表並加入新的 CSS 類別，名為`ExpensivePriceEmphasis`具有下列定義：


[!code-css[Main](custom-formatting-based-upon-data-vb/samples/sample6.css)]

然後，在`DataBound`事件處理常式中設定的儲存格`CssClass`屬性設`ExpensivePriceEmphasis`。 下列程式碼示範`DataBound`完整的事件處理常式：


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample7.vb)]

時檢視 Chai，成本小於 $75.00，價格會以一般字型顯示 （請參閱 圖 4）。 不過，當檢視 Mishi Kobe Niku，其具有價格為美金 97.00，價格會顯示在粗體、 斜體字型 （請參閱 [圖 5]）。


[![價格低於美金 75.00 會以一般字型顯示](custom-formatting-based-upon-data-vb/_static/image9.png)](custom-formatting-based-upon-data-vb/_static/image8.png)

**圖 4**： 價格低於美金 75.00 會以一般字型顯示 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-vb/_static/image10.png))


[![昂貴的產品價格會顯示在粗體、 斜體字型](custom-formatting-based-upon-data-vb/_static/image12.png)](custom-formatting-based-upon-data-vb/_static/image11.png)

**圖 5**： 昂貴的產品價格會顯示在粗體、 斜體字型 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-vb/_static/image13.png))


## <a name="using-the-formview-controlsdataboundevent-handler"></a>使用 FormView 控制項`DataBound`事件處理常式

DetailsView 建立，會完全一樣的步驟，以判斷基礎資料繫結至 FormView`DataBound`事件處理常式，轉型`DataItem`適當的物件類型的屬性繫結至控制項，並判斷如何繼續進行。 FormView 和 DetailsView 不同，不過，在更新其使用者介面的外觀的方式。

FormView 不包含任何 BoundFields，因此缺少`Rows`集合。 相反地，FormView 組成範本，其中可以包含混合的靜態 HTML，Web 控制項及資料繫結語法。 調整的 FormView 樣式通常牽涉到調整一或多個 FormView 的範本內的 Web 控制項的樣式。

為了說明這點，讓我們使用 FormView 產品清單類似在上述範例中，但這次我們顯示產品名稱和單位庫存與紅色字型顯示，如果它小於或等於 10 的庫存單位數。

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>步驟 4： 在 FormView 中顯示的產品資訊

新增至 FormView`CustomColors.aspx`頁面下方的 DetailsView 和設定其`ID`屬性設`LowStockedProductsInRed`。 將 FormView 的繫結至從上一個步驟建立的 ObjectDataSource 控制項。 這會建立`ItemTemplate`， `EditItemTemplate`，和`InsertItemTemplate`FormView 的。 移除`EditItemTemplate`並`InsertItemTemplate`並簡化`ItemTemplate`包含只`ProductName`和`UnitsInStock`值各自位於自己適當地命名為 Label 控制項。 如同 DetailsView 來自先前範例中，也請檢查 FormView 的智慧標籤的 啟用分頁核取方塊。

這些編輯之後 FormView 的標記看起來應該如下所示：


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample8.aspx)]

請注意，`ItemTemplate`包含：

- **靜態 HTML**文字 」 產品: 」 及 「 庫存單位數:"連同`<br />`和`<b>`項目。
- **Web 控制項**兩個 Label 控制項中，`ProductNameLabel`和`UnitsInStockLabel`。
- **資料繫結語法**`<%# Bind("ProductName") %>`並`<%# Bind("UnitsInStock") %>`語法，從這些欄位的值指定給 Label 控制項`Text`屬性。

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>步驟 5： 以程式設計方式判斷資料繫結事件處理常式中的資料值

使用 FormView 的標記完成下, 一個步驟是以程式設計方式判斷如果`UnitsInStock`值是否小於或等於 10。 這是與 DetailsView 完成使用 FormView 完全相同的方式。 開始建立事件處理常式，如 FormView 的`DataBound`事件。


![建立資料繫結事件處理常式](custom-formatting-based-upon-data-vb/_static/image14.png)

**圖 6**： 建立`DataBound`事件處理常式


在事件處理常式轉型的 FormView`DataItem`屬性，以`ProductsRow`執行個體，並判斷是否`UnitsInPrice`值是，我們要顯示紅色字型。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample9.vb)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>步驟 6： 設定格式化的 FormView 的 itemtemplate UnitsInStockLabel Label 控制項

最後一個步驟是設定格式顯示`UnitsInStock`紅色字型值，如果值為 10 或更少。 若要這麼做我們要以程式設計方式存取`UnitsInStockLabel`控制在`ItemTemplate`並設定其樣式屬性，使其文字會以紅色顯示。 若要存取 Web 控制項範本中的，使用`FindControl("controlID")`方法如下：


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample10.vb)]

針對我們的範例中我們想要存取標籤控制項`ID`值是`UnitsInStockLabel`，因此，我們會使用：


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample11.vb)]

一旦我們擁有 Web 控制項的程式設計參考，我們可以視需要修改它的樣式相關屬性。 如先前範例中，我建立了中的 CSS 類別`Styles.css`名為`LowUnitsInStockEmphasis`。 若要將此樣式套用至標籤 Web 控制項中，設定其`CssClass`屬性據此。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample12.vb)]

> [!NOTE]
> 格式化以程式設計方式存取 使用您建立 Web 控制項範本的語法`FindControl("controlID")`，然後設定其樣式相關屬性也可用時使用[TemplateFields](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) DetailsView 或 GridView控制項。 在我們的下一個教學課程中，我們將檢驗 TemplateFields。


圖 7] 顯示 FormView 檢視產品時其`UnitsInStock`值大於 10，而 [圖 8 中的產品有其值小於 10。


[![針對產品使用夠大庫存單位，套用無自訂格式](custom-formatting-based-upon-data-vb/_static/image16.png)](custom-formatting-based-upon-data-vb/_static/image15.png)

**圖 7**： 套用的產品使用夠大庫存單位中，沒有自訂格式 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-vb/_static/image17.png))


[![內建數字的單位是以紅色顯示這些產品與值的 10 或更少](custom-formatting-based-upon-data-vb/_static/image19.png)](custom-formatting-based-upon-data-vb/_static/image18.png)

**圖 8**： 在庫存數字的單位以紅色顯示的那些產品 With Values 10 或更少的 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-vb/_static/image20.png))


## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>使用 GridView 的格式化`RowDataBound`事件

稍早我們檢查一連串步驟 DetailsView 和 FormView 控制項進行資料繫結期間。 讓我們透過下列步驟再次複習一下。

1. 資料 Web 控制項`DataBinding`引發事件。
2. 資料繫結至資料 Web 控制項。
3. 資料 Web 控制項`DataBound`引發事件。

這三個簡單步驟已足夠完成 DetailsView 和 FormView 的因為它們會顯示單一記錄。 Gridview，其中顯示*所有*記錄繫結至該 （不只是第一個），步驟 2 會稍微複雜一點。

在步驟的 2 的 GridView 會列舉資料來源，並每一筆記錄，建立`GridViewRow`執行個體，並繫結至它的目前資料錄。 每個`GridViewRow`新增至 GridView，會引發兩個事件：

- **`RowCreated`** 之後，就會引發`GridViewRow`已建立
- **`RowDataBound`** 已繫結至的目前記錄之後，就會引發`GridViewRow`。

Gridview，接著，資料繫結是更精確地將描述下列一連串步驟：

1. GridView 的`DataBinding`引發事件。
2. 資料繫結至 GridView。   
  
   資料來源中的每一筆記錄 

    1. 建立`GridViewRow`物件
    2. 引發`RowCreated`事件
    3. 繫結至資料錄 `GridViewRow`
    4. 引發`RowDataBound`事件
    5. 新增`GridViewRow`至`Rows`集合
3. GridView 的`DataBound`引發事件。

若要自訂的 GridView 的個別記錄格式，然後，我們需要建立的事件處理常式`RowDataBound`事件。 為了說明這點，讓我們新增到 GridView`CustomColors.aspx`列出名稱、 類別和每個產品，反白顯示的價格不超過 $10.00 以黃色背景色彩的產品價格頁面。

## <a name="step-7-displaying-product-information-in-a-gridview"></a>步驟 7: GridView 中顯示產品資訊

新增 GridView 下方 FormView 上一個範例中並設定其`ID`屬性設`HighlightCheapProducts`。 由於我們已經有 ObjectDataSource 會傳回所有產品頁面上，將該繫結 GridView。 最後，編輯的 GridView BoundFields 包含只是產品的名稱、 分類和價格。 這些編輯之後 GridView 的標記應該看起來像：


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample13.aspx)]

[圖 9] 顯示我們到目前為止透過瀏覽器檢視時的進度。


[![GridView 會列出名稱、 類別和每項產品的價格](custom-formatting-based-upon-data-vb/_static/image22.png)](custom-formatting-based-upon-data-vb/_static/image21.png)

**圖 9**: GridView 會列出名稱、 類別和每個產品的價格 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-vb/_static/image23.png))


## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>步驟 8： 以程式設計方式判斷 RowDataBound 事件處理常式中的資料值

當`ProductsDataTable`繫結至 GridView 其`ProductsRow`執行個體均列舉的且每個`ProductsRow``GridViewRow`建立。 `GridViewRow`的`DataItem`屬性會指派給特定`ProductRow`之後, GridView 的`RowDataBound`事件處理常式，就會引發。 若要判斷`UnitPrice`針對每個產品的值繫結至 GridView，則我們需要建立事件處理常式，如 GridView 的`RowDataBound`事件。 這個事件處理常式中，我們可以檢查`UnitPrice`目前的值`GridViewRow`和格式化決定該資料列。

可以使用 FormView 和 DetailsView 中使用相同的一系列步驟為建立這個事件處理常式。


![GridView 的 RowDataBound 事件建立事件處理常式](custom-formatting-based-upon-data-vb/_static/image24.png)

**圖 10**： 建立事件處理常式，如 GridView 的`RowDataBound`事件


以這種方式建立事件處理常式將會導致下列的程式碼，以自動新增至 ASP.NET 網頁的程式碼部分：


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample14.vb)]

當`RowDataBound`事件引發時，事件處理常式傳遞做為其第二個參數類型的物件`GridViewRowEventArgs`，其具有名為`Row`。 這個屬性會傳回參考`GridViewRow`是只是資料繫結。 若要存取`ProductsRow`執行個體繫結至`GridViewRow`我們會使用`DataItem`屬性就像這樣：


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample15.vb)]

使用時`RowDataBound`很重要要牢記在心 GridView 由不同類型的資料列所組成且為引發此事件的事件處理常式*所有*資料列型別。 A`GridViewRow`的型別由其`RowType`屬性，而且可以有其中一個可能的值：

- `DataRow` 從 GridView 的繫結至資料錄的資料列 `DataSource`
- `EmptyDataRow` 如果顯示的資料列的 GridView`DataSource`是空的
- `Footer` 頁尾資料列;顯示的如果 GridView 的`ShowFooter`屬性設定為 `True`
- `Header` 標頭資料列，顯示是否 GridView 的 ShowHeader 屬性會設定為`True`（預設值）
- `Pager` 如 GridView 的實作分頁，顯示分頁介面的資料列
- `Separator` 不使用 GridView，但是所使用`RowType`屬性 DataList 與重複項控制項、 兩個資料 Web 控制項，我們在未來將討論教學課程

由於`EmptyDataRow`， `Header`， `Footer`，和`Pager`與相關聯的資料列不`DataSource`記錄，它們一定會針對`Nothing`針對其`DataItem`屬性。 基於這個理由，然後再嘗試使用目前`GridViewRow`的`DataItem` 屬性中，我們首先必須確定我們要處理的`DataRow`。 這可藉由檢查`GridViewRow`的`RowType`屬性就像這樣：


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample16.vb)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>步驟 9： 反白顯示黃色時 UnitPrice 資料列值會小於 $10.00

最後一個步驟是以程式設計的方式反白顯示整個`GridViewRow`如果`UnitPrice`該資料列是不超過 $10.00 的值。 存取 GridView 的資料列或資料格的語法是如同 DetailsView`GridViewID.Rows(index)`存取整個資料列中，`GridViewID.Rows(index).Cells(index)`存取特定的資料格。 不過，當`RowDataBound`事件處理常式，就會引發資料繫結`GridViewRow`尚未加入至 GridView 的`Rows`集合。 因此，您無法存取目前`GridViewRow`執行個體`RowDataBound`使用資料列集合的事件處理常式。

而不是`GridViewID.Rows(index)`，我們可以參考目前`GridViewRow`執行個體中`RowDataBound`事件處理常式使用`e.Row`。 也就是為了反白顯示目前`GridViewRow`執行個體`RowDataBound`我們要使用的事件處理常式：


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample17.vb)]

而不是設定`GridViewRow`的`BackColor`屬性直接，讓我們繼續使用 CSS 類別。 我建立了名為的 CSS 類別`AffordablePriceEmphasis`所設定的背景色彩為黃色。 已完成`RowDataBound`事件處理常式會遵循：


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample18.vb)]


[![最實惠的產品為黃色反白顯示](custom-formatting-based-upon-data-vb/_static/image26.png)](custom-formatting-based-upon-data-vb/_static/image25.png)

**圖 11**： 最經濟實惠的產品為黃色反白顯示 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-vb/_static/image27.png))


## <a name="summary"></a>總結

在本教學課程中，我們看到如何設定 GridView、 DetailsView 和 FormView 控制項繫結的資料為基礎的格式。 若要這麼做我們建立的事件處理常式`DataBound`或`RowDataBound`其中基礎資料已檢查的格式設定的變更，以及如有需要的事件。 若要存取的資料繫結至 DetailsView 或 FormView，我們使用`DataItem`中的屬性`DataBound`事件處理常式; 如 GridView，每個`GridViewRow`執行個體的`DataItem`屬性包含該資料列位於繫結的資料`RowDataBound`事件處理常式。

以程式設計方式調整資料 Web 控制項格式的語法，取決於 Web 控制項和格式化資料的顯示方式。 DetailsView 和 GridView 控制項、 資料列和資料格可以存取由循序的索引。 FormView，它會使用範本，如`FindControl("controlID")`方法通常用來找出範本內的 Web 控制項。

在下一個教學課程中我們將探討如何使用 GridView 和 DetailsView 中使用範本。 此外，我們會看到自訂根據基礎資料的格式設定的另一種技術。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 E.R. Gilmore，Dennis Patterson 和 Dan Jagers。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](displaying-summary-information-in-the-gridview-s-footer-cs.md)
> [下一頁](using-templatefields-in-the-gridview-control-vb.md)
