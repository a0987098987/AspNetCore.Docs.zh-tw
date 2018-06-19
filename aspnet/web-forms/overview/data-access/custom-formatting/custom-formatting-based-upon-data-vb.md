---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
title: 自訂格式會根據資料 (VB) |Microsoft 文件
author: rick-anderson
description: 以多種方式可以完成調整 GridView、 DetailsView 或根據繫結至它的資料在 FormView 的格式。 在本教學課程中，我們會 l...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: df5a1525-386f-4632-972c-57b199870bc3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: a5c7f99b863697cc49a5bc9831dae861f51e129d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876939"
---
<a name="custom-formatting-based-upon-data-vb"></a>自訂格式會根據資料 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_11_VB.exe)或[下載 PDF](custom-formatting-based-upon-data-vb/_static/datatutorial11vb1.pdf)

> 以多種方式可以完成調整 GridView、 DetailsView 或根據繫結至它的資料在 FormView 的格式。 在此教學課程中我們將探討如何完成資料繫結透過資料繫結和 RowDataBound 事件處理常式使用的格式。


## <a name="introduction"></a>簡介

透過各種不同的樣式相關屬性，您可以自訂 GridView、 DetailsView 和 FormView 控制項的外觀。 屬性，例如`CssClass`， `Font`， `BorderWidth`， `BorderStyle`， `BorderColor`， `Width`，和`Height`，和其他項目，指定一般呈現的控制項的外觀。 屬性包括`HeaderStyle`， `RowStyle`， `AlternatingRowStyle`，和其他項目允許這些相同的樣式設定来套用至特定區段。 同樣地，可以在欄位層級套用這些樣式設定。

在許多情況下，格式需求取決於所顯示的資料值。 例如，若要強調的內建的產品，報告，列出產品資訊可能會設定背景色彩為黃色，這些產品的`UnitsInStock`和`UnitsOnOrder`欄位都等於 0。 反白顯示的價格的產品，我們可能會想要顯示成本超過 $75.00 以粗體字這些產品的價格。

以多種方式可以完成調整 GridView、 DetailsView 或根據繫結至它的資料在 FormView 的格式。 在此教學課程中我們會探討如何完成資料繫結透過使用的格式`DataBound`和`RowDataBound`事件處理常式。 在下一個教學課程中，我們將探討的替代方式。

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>使用 DetailsView 控制項`DataBound`事件處理常式

當資料繫結至 DetailsView，從資料來源控制項，或是透過程式設計方式將資料指派給控制項的`DataSource`屬性，並呼叫其`DataBind()`方法，下列步驟順序發生：

1. 資料 Web 控制項`DataBinding`事件引發。
2. 資料繫結至 Web 控制項的資料。
3. 資料 Web 控制項`DataBound`事件引發。

可透過事件處理常式的步驟 1 和 3 之後立即插入自訂邏輯。 藉由建立的事件處理常式`DataBound`我們以程式設計方式判斷已經過的資料繫結至資料 Web 控制項，並調整格式所需的事件。 為了說明這點讓我們來建立會列出產品的一般資訊，但將會顯示在 DetailsView`UnitPrice`值***粗體、 斜體字型***如果它超過 $75.00。

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>步驟 1： 在 DetailsView 中顯示的產品資訊。

開啟`CustomColors.aspx`頁面`CustomFormatting`資料夾中，將 DetailsView 控制項從工具箱拖曳至設計工具，設定其`ID`屬性值設定為`ExpensiveProductsPriceInBoldItalic`，並將它繫結至新的 ObjectDataSource 控制項叫用`ProductsBLL`類別的`GetProducts()`方法。 為求簡單明瞭，因為我們仔細檢查其先前的教學課程中完成此的詳細的步驟會這裡省略。

一旦您已經在 detailsview 結合 ObjectDataSource，請花一點時間修改的欄位清單。 我已選擇移除`ProductID`， `SupplierID`， `CategoryID`， `UnitsInStock`， `UnitsOnOrder`， `ReorderLevel`，和`Discontinued`BoundFields 和重新命名並重新格式化剩餘 BoundFields。 同時也清除`Width`和`Height`設定。 在 DetailsView 會顯示一筆記錄，因為我們需要啟用分頁，以便讓使用者檢視的所有產品。 這樣會檢查 DetailsView 的智慧標籤的 啟用分頁核取方塊。


[![圖 1： 檢查 DetailsView 的智慧標籤中啟用分頁核取方塊](custom-formatting-based-upon-data-vb/_static/image2.png)](custom-formatting-based-upon-data-vb/_static/image1.png)

**圖 1**： 圖 1： 檢查啟用分頁中的核取 DetailsView 的智慧標籤 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-vb/_static/image3.png))


這些變更之後，請將 DetailsView 標記：


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample1.aspx)]

請花一點時間來測試您的瀏覽器中的此頁面。


[![在 DetailsView 控制項一次顯示一個產品](custom-formatting-based-upon-data-vb/_static/image5.png)](custom-formatting-based-upon-data-vb/_static/image4.png)

**圖 2**: DetailsView 控制項顯示一個產品一次 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-vb/_static/image6.png))


## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>步驟 2： 以程式設計方式判斷此資料繫結事件處理常式中的資料值

若要顯示的價格以粗體、 斜體字型為這些產品的`UnitPrice`值超過 $75.00，我們必須要先能以程式設計方式判斷`UnitPrice`值。 為 DetailsView，這可以透過完成`DataBound`事件處理常式。 若要建立事件處理常式會 DetailsView 設計工具中按一下，然後瀏覽至 [屬性] 視窗。 按 F4，即可啟動，如果不是可見的或移至 [檢視] 功能表，然後選取 [屬性] 視窗的功能表選項。 屬性 視窗中，按一下 閃電圖示來列出在 DetailsView 的事件。 接下來，按兩下`DataBound`事件或輸入您想要建立此事件處理常式的名稱。


![資料繫結事件建立事件處理常式](custom-formatting-based-upon-data-vb/_static/image7.png)

**圖 3**： 建立事件處理常式`DataBound`事件


> [!NOTE]
> 您也可以建立事件處理常式從 ASP.NET 網頁的程式碼部分。 那里，您會發現在頁面頂端的兩個下拉式清單。 從左邊的下拉式清單中選取的物件，您想要從右邊下拉式清單和 Visual Studio 建立的處理常式的事件將會自動建立適當的事件處理常式。


這樣會自動建立事件處理常式，並帶您前往的程式碼部分加入其中。 此時您會看到：


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample2.vb)]

在 detailsview 繫結的資料可以透過存取`DataItem`屬性。 提醒您，我們會將我們將控制項繫結至強型別 DataTable，其中強型別 DataRow 執行個體的集合所組成。 在 DataTable 中的第一個 DataRow DataTable 繫結至 DetailsView，指派給 DetailsView 的`DataItem`屬性。 具體來說，`DataItem`屬性會被指派`DataRowView`物件。 我們可以使用`DataRowView`的`Row`屬性來存取基礎的 DataRow 物件是實際上`ProductsRow`執行個體。 一旦我們有此`ProductsRow`我們就可以決定我們只要檢查物件的屬性值的執行個體。

下列程式碼說明如何判斷是否`UnitPrice`DetailsView 控制項繫結的值大於 $75.00:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample3.vb)]

> [!NOTE]
> 因為`UnitPrice`可以有`NULL`值在資料庫中，我們會先檢查以確定我們正在未處理的`NULL`前存取值`ProductsRow`的`UnitPrice`屬性。 這項檢查，請務必因為如果我們嘗試存取`UnitPrice`屬性時，它有`NULL`值`ProductsRow`物件將會擲回[StrongTypingException 例外狀況](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx)。


## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>步驟 3： 格式化 UnitPrice 值 DetailsView 中

此時我們可以判斷是否`UnitPrice`繫結至 DetailsView 值超過 $75.00，但我們尚未以了解如何以程式設計方式調整 DetailsView 的據以格式化。 若要修改之格式設定的 DetailsView 中整個資料列，以程式設計方式存取資料列使用`DetailsViewID.Rows(index)`; 若要修改特定的資料格，存取使用`DetailsViewID.Rows(index).Cells(index)`。 一旦資料列或資料格的參考我們就可以設定其樣式相關屬性，然後調整其外觀。

您必須了解資料列的索引，從 0 開始，以程式設計方式存取資料列。 `UnitPrice`資料列會提供 4 個索引，並且讓它成為以程式設計方式存取 DetailsView 中的第五個資料列使用`ExpensiveProductsPriceInBoldItalic.Rows(4)`。 此時，我們可能使用下列程式碼顯示粗體、 斜體字型中的整個資料列內容：


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample4.vb)]

不過，這會讓*兩者*標籤 （價格） 和粗體和斜體的值。 如果我們想要只值粗體和斜體我們要將此套用的第二個資料格中的資料列，即可使用下列格式：


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample5.vb)]

由於教學課程到目前為止已使用的樣式表維持清楚分隔呈現的標記和樣式的相關資訊，而不是設定特定的樣式屬性，如上所示我們改為使用 CSS 類別。 開啟`Styles.css`樣式表和加入新的 CSS 類別，名為`ExpensivePriceEmphasis`具有下列定義：


[!code-css[Main](custom-formatting-based-upon-data-vb/samples/sample6.css)]

然後，在`DataBound`事件處理常式中，設定儲存格的`CssClass`屬性`ExpensivePriceEmphasis`。 下列程式碼會示範`DataBound`完整的事件處理常式：


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample7.vb)]

當檢視 Chai，成本小於 $75.00，價格會以一般字型顯示 （請參閱圖 4）。 不過，當檢視 Mishi Kobe Niku 有價格為 $97.00，價格會顯示在粗體、 斜體字型 （請參閱圖 5）。


[![價格小於 $75.00 會以一般字型顯示](custom-formatting-based-upon-data-vb/_static/image9.png)](custom-formatting-based-upon-data-vb/_static/image8.png)

**圖 4**： 價格小於 $75.00 會以一般字型顯示 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-vb/_static/image10.png))


[![昂貴的產品價格會顯示在粗體、 斜體字型](custom-formatting-based-upon-data-vb/_static/image12.png)](custom-formatting-based-upon-data-vb/_static/image11.png)

**圖 5**： 昂貴的產品價格會顯示在粗體、 斜體字型 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-vb/_static/image13.png))


## <a name="using-the-formview-controlsdataboundevent-handler"></a>使用 FormView 控制項`DataBound`事件處理常式

決定繫結至在 FormView 的基礎資料的步驟都與建立 DetailsView`DataBound`事件處理常式，轉換`DataItem`適當的物件類型的屬性繫結至控制項，並判斷要如何繼續執行。 在 FormView 和 DetailsView 有所不同，不過，在其使用者介面的外觀更新的方式。

在 FormView 不包含任何 BoundFields，因此缺少`Rows`集合。 相反地，在 FormView 組成的範本，其中可以包含靜態 HTML 混用，Web 控制項和資料繫結語法。 調整在 FormView 的樣式通常牽涉到調整一個或多個在 FormView 的範本中的 Web 控制項的樣式。

為了說明這點，讓我們使用產品清單 FormView 如同上述範例中，但這次我們顯示產品名稱和單位庫存顯示紅色字型，如果它小於或等於 10 的庫存單位。

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>步驟 4： 在 FormView 中顯示的產品資訊。

新增 FormView`CustomColors.aspx`頁面下方的 DetailsView 和設定其`ID`屬性`LowStockedProductsInRed`。 將在 FormView 的繫結至從上一個步驟建立 ObjectDataSource 控制項。 這會建立`ItemTemplate`， `EditItemTemplate`，和`InsertItemTemplate`在 FormView 的。 移除`EditItemTemplate`和`InsertItemTemplate`並簡化`ItemTemplate`包含只`ProductName`和`UnitsInStock`值，每個自己適當命名的 Label 控制項。 如同在 DetailsView 從先前的範例，也請檢查在 FormView 的智慧標籤的 啟用分頁核取方塊。

之後這些編輯 FormView 的標記看起來應該如下所示：


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample8.aspx)]

請注意，`ItemTemplate`包含：

- **靜態 HTML**文字 「 產品:"和"庫存單位:"連同`<br />`和`<b>`項目。
- **Web 控制項**兩個 Label 控制項，`ProductNameLabel`和`UnitsInStockLabel`。
- **資料繫結語法**`<%# Bind("ProductName") %>`和`<%# Bind("UnitsInStock") %>`語法，從這些欄位的值指定給 Label 控制項`Text`屬性。

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>步驟 5： 以程式設計方式判斷此資料繫結事件處理常式中的資料值

在 FormView 的標記完成下, 一個步驟是以程式設計方式判斷是否`UnitsInStock`值小於或等於 10。 這是與 DetailsView 達成完全相同的方式與在 FormView 中。 從建立在 FormView 的事件處理常式開始`DataBound`事件。


![建立資料繫結事件處理常式](custom-formatting-based-upon-data-vb/_static/image14.png)

**圖 6**： 建立`DataBound`事件處理常式


在事件處理常式轉換在 FormView 的`DataItem`屬性`ProductsRow`執行個體，並判斷是否`UnitsInPrice`值，這類我們需要顯示紅色字型。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample9.vb)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>步驟 6： 格式化在 FormView 的 ItemTemplate 的 UnitsInStockLabel 標籤控制項

最後一個步驟是格式化顯示`UnitsInStock`紅色字型中的值如果值為 10 或更少。 若要完成此我們需要以程式設計方式存取`UnitsInStockLabel`控制`ItemTemplate`並設定其樣式屬性，使其文字會以紅色顯示。 若要存取的 Web 控制項範本中，使用`FindControl("controlID")`方法如下：


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample10.vb)]

此範例中我們想要存取標籤控制項`ID`值是`UnitsInStockLabel`，因此我們會使用：


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample11.vb)]

一旦 Web 控制項的程式設計參考，我們可以視需要修改它的樣式相關屬性。 如先前範例中，我建立了中的 CSS 類別`Styles.css`名為`LowUnitsInStockEmphasis`。 若要套用此樣式的標籤 Web 控制項，設定其`CssClass`屬性據此。


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample12.vb)]

> [!NOTE]
> 格式設定範本，以程式設計方式存取 Web 控制項使用的語法`FindControl("controlID")`，然後設定其樣式相關屬性也可使用時[TemplateFields](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) DetailsView 或 GridView 中控制項。 在我們的下一個教學課程中，我們將檢驗 TemplateFields。


圖 7 顯示 FormView，檢視產品時其`UnitsInStock`值大於 10，而圖 8 中的產品有它的值小於 10。


[![針對產品與夠大 庫存單位，套用無自訂格式](custom-formatting-based-upon-data-vb/_static/image16.png)](custom-formatting-based-upon-data-vb/_static/image15.png)

**圖 7**： 套用的產品與夠大 庫存單位中，沒有自訂格式 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-vb/_static/image17.png))


[![庫存數字的單位數以紅色顯示的那些產品 With Values 10 倍或更少](custom-formatting-based-upon-data-vb/_static/image19.png)](custom-formatting-based-upon-data-vb/_static/image18.png)

**圖 8**: 單位庫存數字以紅色顯示的那些產品 With Values 10 倍或更少 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-vb/_static/image20.png))


## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>格式設定與 GridView`RowDataBound`事件

稍早我們檢查 DetailsView 步驟的順序和 FormView 控制項進行資料繫結期間。 讓我們透過下列步驟再次為重新整理程式。

1. 資料 Web 控制項`DataBinding`事件引發。
2. 資料繫結至 Web 控制項的資料。
3. 資料 Web 控制項`DataBound`事件引發。

這三個簡單步驟已足夠的 DetailsView 和 FormView，因為它們會顯示單一記錄。 Gridview，其中顯示*所有*記錄繫結它 （不只是第一個），步驟 2 會稍微更為複雜。

在步驟的 2 GridView 會列舉資料來源，以及每個記錄，建立`GridViewRow`執行個體，並繫結到它的目前記錄。 每個`GridViewRow`加入至 GridView，會引發兩個事件：

- **`RowCreated`** 之後，都會引發`GridViewRow`已建立
- **`RowDataBound`** 目前的記錄繫結至後引發`GridViewRow`。

Gridview，然後，資料繫結是更精確地分為下列一連串步驟：

1. GridView`DataBinding`事件引發。
2. 資料繫結至 GridView。   
  
   資料來源中的每一筆記錄 

    1. 建立`GridViewRow`物件
    2. 引發`RowCreated`事件
    3. 繫結至資料錄 `GridViewRow`
    4. 引發`RowDataBound`事件
    5. 新增`GridViewRow`至`Rows`集合
3. GridView`DataBound`事件引發。

若要自訂的 GridView 的個別記錄格式，然後，我們必須建立事件處理常式`RowDataBound`事件。 為了說明這點，讓我們加入 GridView`CustomColors.aspx`列出名稱、 類別目錄，以及這些產品的價格是不超過 $10.00 以黃色背景的色彩反白顯示的每個產品價格的頁面。

## <a name="step-7-displaying-product-information-in-a-gridview"></a>步驟 7： 在 GridView 中顯示產品資訊

在 FormView 下方的 GridView 加入前一個範例並設定其`ID`屬性`HighlightCheapProducts`。 因為我們已經在頁面上會傳回所有產品的 Sqldatasource，繫結的 GridView 的。 最後，編輯 GridView 的 BoundFields 以包含剛才產品的名稱、 分類和價格。 之後這些編輯 GridView 標記看起來應該像：


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample13.aspx)]

圖 9 顯示我們透過瀏覽器檢視時，此點的進度。


[![在 GridView 列出名稱、 類別和每項產品的價格](custom-formatting-based-upon-data-vb/_static/image22.png)](custom-formatting-based-upon-data-vb/_static/image21.png)

**圖 9**: GridView 列出名稱、 類別和每個產品的價格 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-vb/_static/image23.png))


## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>步驟 8： 以程式設計方式判斷 RowDataBound 事件處理常式中的資料值

當`ProductsDataTable`繫結至 GridView 其`ProductsRow`執行個體是列舉，以及對於每個`ProductsRow``GridViewRow`建立。 `GridViewRow`的`DataItem`屬性指派給特定`ProductRow`，其後 GridView`RowDataBound`事件處理常式，就會引發。 若要判斷`UnitPrice`每項產品的值繫結至 GridView，則我們需要建立事件處理常式的 GridView`RowDataBound`事件。 此事件處理常式中，我們可以檢查`UnitPrice`值目前`GridViewRow`做出格式化的決策，該資料列。

可以使用相同的一系列步驟，做為 DetailsView FormView 與建立此事件處理常式。


![GridView RowDataBound 事件建立事件處理常式](custom-formatting-based-upon-data-vb/_static/image24.png)

**圖 10**： 建立事件處理常式的 GridView`RowDataBound`事件


以這種方式建立事件處理常式，將導致下列的程式碼會自動加入至 ASP.NET 網頁的程式碼部分：


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample14.vb)]

當`RowDataBound`事件引發的事件處理常式會傳遞做為其第二個參數類型的物件`GridViewRowEventArgs`，其具有內容，名為`Row`。 這個屬性會傳回參考`GridViewRow`時只是資料繫結。 若要存取`ProductsRow`執行個體繫結至`GridViewRow`我們使用`DataItem`屬性如下所示：


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample15.vb)]

當使用`RowDataBound`務必記住 GridView 組成不同類型的資料列，而且此事件針對引發的事件處理常式*所有*資料列型別。 A`GridViewRow`的類型由其`RowType`屬性，且可以有一個可能的值：

- `DataRow` 繫結至資料錄從 GridView 的資料列 `DataSource`
- `EmptyDataRow` 如果顯示的資料列的 GridView`DataSource`是空的
- `Footer` 頁尾資料列。顯示的如果 GridView`ShowFooter`屬性設定為 `True`
- `Header` 標頭資料列，顯示是否 GridView ShowHeader 屬性會設定為`True`（預設值）
- `Pager` 會實作的 GridView 的分頁，顯示分頁介面的資料列
- `Separator` 不會用於 GridView，但使用`RowType`DataList 和中繼器內容控制項，兩個資料中，我們會討論在未來教學課程的 Web 控制項

因為`EmptyDataRow`， `Header`， `Footer`，和`Pager`與相關聯的資料列不是`DataSource`記錄，它們會一直有一個值的`Nothing`針對其`DataItem`屬性。 基於這個原因，然後再嘗試將工作與目前`GridViewRow`的`DataItem`屬性，我們必須先確定我們正在處理的`DataRow`。 這可以透過檢查`GridViewRow`的`RowType`屬性如下所示：


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample16.vb)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>步驟 9： 反白顯示資料列黃色時 UnitPrice 值是不超過 $10.00

最後一個步驟是以程式設計的方式反白顯示整個`GridViewRow`如果`UnitPrice`值該資料列是不超過 $10.00。 存取的 GridView 資料列或資料格的語法是如同 DetailsView`GridViewID.Rows(index)`存取整個資料列中，`GridViewID.Rows(index).Cells(index)`存取特定資料格。 不過，當`RowDataBound`事件處理常式就會引發資料繫結`GridViewRow`尚未加入至 GridView`Rows`集合。 因此，您無法存取目前`GridViewRow`的執行個體`RowDataBound`使用資料列集合的事件處理常式。

而不是`GridViewID.Rows(index)`，我們可以參考目前`GridViewRow`執行個體中`RowDataBound`事件處理常式使用`e.Row`。 也就是為了反白顯示目前`GridViewRow`的執行個體`RowDataBound`我們要使用的事件處理常式：


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample17.vb)]

而非設定`GridViewRow`的`BackColor`屬性直接管理，讓我們堅持使用 CSS 類別。 我已經建立名為的 CSS 類別`AffordablePriceEmphasis`所設定的背景色彩為黃色。 已完成`RowDataBound`事件處理常式會遵循：


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample18.vb)]


[![最合理的產品為黃色反白顯示](custom-formatting-based-upon-data-vb/_static/image26.png)](custom-formatting-based-upon-data-vb/_static/image25.png)

**圖 11**: 大部分價格合理的產品為黃色反白顯示 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-vb/_static/image27.png))


## <a name="summary"></a>總結

在本教學課程中，我們看到如何格式化 GridView、 DetailsView 和根據資料繫結至控制項的程式設計。 若要完成此我們建立的事件處理常式`DataBound`或`RowDataBound`其中基礎資料已檢查的格式設定的變更，以及視需要的事件。 若要存取的資料繫結至 DetailsView 或 FormView，我們使用`DataItem`屬性`DataBound`事件處理常式; GridView，每個`GridViewRow`執行個體的`DataItem`屬性包含的資料繫結至該資料列，其可於`RowDataBound`事件處理常式。

以程式設計方式調整資料 Web 控制項的格式設定的語法，取決於 Web 控制項和格式化資料的顯示方式。 DetailsView 和 GridView 控制項、 資料列和資料格可以透過序數索引。 FormView 中，會使用範本，如`FindControl("controlID")`方法通常用來找出範本內的 Web 控制項。

下一個教學課程中我們將探討如何使用 GridView 和 DetailsView 的範本。 此外，我們會看到另一個自訂的基礎資料的格式設定的技術。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 本教學課程總計 E.R.導致檢閱者 Gilmore，Dennis Patterson 和 Dan Jagers。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](displaying-summary-information-in-the-gridview-s-footer-cs.md)
> [下一頁](using-templatefields-in-the-gridview-control-vb.md)
