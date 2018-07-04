---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
title: 將驗證控制項新增至編輯和插入介面 (VB) |Microsoft Docs
author: rick-anderson
description: 在本教學課程中，我們會看到將驗證控制項新增至 EditItemTemplate 和 InsertItemTemplate Web 控制項的資料，以提供更是多麼...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: e3d7028a-7a22-4a4f-babe-d53afc41c0e2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
msc.type: authoredcontent
ms.openlocfilehash: a2fc00426022513c6e2adc49b0df30f943403302
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366225"
---
<a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-vb"></a>將驗證控制項新增至編輯和插入介面 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_VB.exe)或[下載 PDF](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/datatutorial19vb1.pdf)

> 在本教學課程中，我們會看到將驗證控制項新增至要提供更萬無一失的使用者介面的 EditItemTemplate 和 InsertItemTemplate 資料 Web 控制項的是多麼容易。


## <a name="introduction"></a>簡介

GridView 和 DetailsView 控制項範例中的我們探討了過去三個教學課程有所有已 BoundFields 和組成 CheckBoxFields （的欄位型別繫結至資料來源的 GridView 或 DetailsView 時，自動加入由 Visual Studio透過智慧標籤控制）。 當您編輯的 GridView 或 DetailsView 中的資料列，這些不是唯讀的 BoundFields 會轉換成文字方塊中，使用者可以從中修改現有的資料。 同樣地，當插入新記錄到 DetailsView 控制項，這些 BoundFields 其`InsertVisible`屬性設定為`True`（預設值） 會呈現為空白的文字方塊中，使用者可以在其中提供新資料錄的欄位值。 同樣地，CheckBoxFields，會停用標準的唯讀介面中，會轉換成已啟用的核取方塊中編輯和插入介面。

雖然預設編輯和插入介面 BoundField 及其可能很有幫助，介面會缺少任何類型的驗證。 如果使用者犯了資料項目-例如省略`ProductName`欄位，或輸入的值為 「 無效 」 `UnitsInStock` （例如-50) 將會引發例外狀況從應用程式架構的深度內。 雖然此處理例外狀況可以依正常程序中所示[先前的教學課程](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)，在理想情況下編輯，或插入使用者介面會包含驗證控制項，以防止使用者輸入中這類無效的資料第一個位置。

若要提供自訂的編輯或插入的介面，我們要取代為 TemplateField BoundField 或 CheckBoxField。 TemplateFields 所討論的主題中[使用 GridView 控制項中的 TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md)並[DetailsView 控制項中的使用 TemplateFields](../custom-formatting/using-templatefields-in-the-detailsview-control-vb.md)教學課程中，可以包含多個範本定義分隔不同的資料列狀態的介面。 TemplateField`ItemTemplate`時，用來呈現唯讀欄位或資料列，在 DetailsView 或 GridView 控制項中，而`EditItemTemplate`和`InsertItemTemplate`表示要進行編輯，然後分別插入模式中，使用的介面。

在本教學課程中我們會看到將驗證控制項新增至 TemplateField 是多麼`EditItemTemplate`和`InsertItemTemplate`提供更萬無一失的使用者介面。 具體來說，本教學課程需要中建立的範例[檢查與插入、 更新和刪除事件相關聯](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)教學課程和加強的編輯和插入介面以包含適當的驗證。

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deletingexamining-the-events-associated-with-inserting-updating-and-deleting-vbmd"></a>步驟 1： 複寫的範例[檢查的事件相關聯插入、 更新和刪除](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)

在 [檢查與插入、 更新和刪除事件相關聯](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)教學課程中我們建立所列出的名稱和價格的產品，可編輯的 GridView 內的頁面。 此外，頁面包含 DetailsView 其`DefaultMode`屬性設定為`Insert`，藉此一律呈現在插入模式。 從這個 DetailsView 中，使用者無法輸入新的產品名稱和價格、 按一下 插入，以及將它新增至系統 （請參閱 圖 1）。


[![上述範例可讓使用者加入新的產品，並編輯現有的](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image1.png)

**圖 1**: 上一個範例可讓使用者加入新的產品，並編輯現有的 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image3.png))


我們在本教學課程的目標是以增強 DetailsView 和 GridView，來提供驗證控制項。 特別是，我們將驗證邏輯將會：

- 需要插入或編輯產品時，提供名稱
- 需要插入一筆記錄; 時，提供價格當您編輯一筆記錄，我們仍需要價格，但將程式設計邏輯，在 GridView 的`RowUpdating`已經存在於較早的教學課程中的事件處理常式
- 請確定輸入的價格的值是有效的貨幣格式

我們來看看擴充前一個範例，以包含驗證之前，我們必須先將複寫的範例`DataModificationEvents.aspx`頁面，即可在此教學課程中，頁面`UIValidation.aspx`。 若要這麼做我們要同時複製`DataModificationEvents.aspx`頁面的宣告式標記和其原始程式碼。 第一次複製的宣告式標記執行下列步驟：

1. 開啟`DataModificationEvents.aspx`Visual Studio 中的頁面
2. 請移至頁面的宣告式標記 （在頁面底部的 [來源] 按鈕按一下）
3. 複製內的文字`<asp:Content>`和`</asp:Content>`標籤 （程式行 3 到 44），為所示的 圖 2。


[![複製文字內&lt;asp: Content&gt;控制項](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image4.png)

**圖 2**： 複製文字內`<asp:Content>`控制項 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image6.png))


1. 開啟`UIValidation.aspx`頁面
2. 請移至頁面的宣告式標記
3. 貼上文字內`<asp:Content>`控制項。

若要複製的原始碼，開啟`DataModificationEvents.aspx.vb`頁面上，並複製只是文字*內*`EditInsertDelete_DataModificationEvents`類別。 複製的三個事件處理常式 (`Page_Load`， `GridView1_RowUpdating`，並`ObjectDataSource1_Inserting`)，但不要**不**複製類別宣告。 貼上複製的文字*內*`EditInsertDelete_UIValidation`類別在`UIValidation.aspx.vb`。

移動內容，以及從程式碼之後`DataModificationEvents.aspx`至`UIValidation.aspx`，花點時間來測試您的瀏覽器中的進度。 您應該會看到相同的輸出和體驗相同的功能，在每個這兩個頁面中 (請參閱上一步 圖 1 的螢幕擷取畫面`DataModificationEvents.aspx`作用中)。

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>步驟 2： 將 BoundFields 轉換 TemplateFields

若要將驗證控制項加入編輯和插入介面，需要轉換成 TemplateFields BoundFields DetailsView 和 GridView 控制項所使用的。 若要這麼做，請分別按一下 編輯資料行和編輯欄位的連結，在 GridView 和 DetailsView 的智慧標籤。 選取每個 BoundFields，然後按一下 「 將這個欄位轉換為 TemplateField 」 連結。


[![每個的 GridView 與 DetailsView 的 BoundFields 轉換 TemplateFields](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image7.png)

**圖 3**： 轉換每個的 GridView 與 DetailsView 的 BoundFields 到 TemplateFields ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image9.png))


BoundField 轉換為 TemplateField 透過 [欄位] 對話方塊中，會產生展現相同的唯讀、 編輯和插入介面 BoundField 本身為 TemplateField。 下列標記顯示的宣告式語法`ProductName`DetailsView 轉換為 TemplateField 成欄位：

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample1.aspx)]

請注意此 TemplateField 有三個自動建立的範本`ItemTemplate`， `EditItemTemplate`，和`InsertItemTemplate`。 `ItemTemplate`會顯示單一資料欄位值 (`ProductName`) 使用 Label Web 控制項，而`EditItemTemplate`並`InsertItemTemplate`關聯的文字方塊中的資料欄位的文字方塊中的 Web 控制項中呈現資料欄位值`Text`使用雙向資料繫結的屬性。 因為我們只將此頁面中使用 DetailsView，您可能會移除`ItemTemplate`和`EditItemTemplate`從兩個的 TemplateFields，雖然沒有壞處，在將它們保留。

因為 GridView 不支援內建插入 DetailsView 的功能、 將轉換的 GridView `ProductName` TemplateField 欄位會導致只`ItemTemplate`和`EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample2.aspx)]

按一下"轉換為 TemplateField 到這個欄位 」，Visual Studio 建立的範本模擬之使用者介面的轉換 BoundField TemplateField。 您可以瀏覽此頁面，透過瀏覽器來確認。 您會發現的外觀和行為的 TemplateFields 是相同的體驗時 BoundFields 所使用。

> [!NOTE]
> 請隨意自訂中的範本所需的編輯介面。 比方說，我們可能會想要在文字方塊`UnitPrice`轉譯為較小的文字方塊，比 TemplateFields`ProductName`文字方塊中。 若要這麼做，您可以設定文字方塊的`Columns`屬性設為適當的值，或提供絕對的寬度，透過`Width`屬性。 在下一個教學課程中，我們會看到如何完全自訂編輯介面，以替代的資料輸入 Web 控制項取代文字方塊中。


## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>步驟 3： 將驗證控制項新增至 GridView 的`EditItemTemplate`s

當建構資料輸入表單，很重要，使用者輸入任何必要的欄位和所有提供的輸入是法律、 格式正確的值。 為了協助確保使用者的輸入都有效，ASP.NET 會提供五個內建的驗證控制項專為用來驗證單一的輸入控制項的值：

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx)可確保已提供的值
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx)驗證值與另一個 Web 控制項的值或常數的值，或可確保值的格式是合法的指定的資料型別
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx)確保值的值範圍內
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx)驗證值[規則運算式](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx)驗證值，以符合自訂的使用者定義的方法

如需有關這些五個控制項的詳細資訊，請參閱[驗證控制項 」 一節](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx)的[ASP.NET 快速入門教學課程](https://asp.net/QuickStart/aspnet/)。

本教學課程中，我們將需要使用在 DetailsView 和 GridView 的 RequiredFieldValidator `ProductName` TemplateFields 和 DetailsView 中的 RequiredFieldValidator `UnitPrice` TemplateField。 此外，我們需要加入這兩個控制項的 CompareValidator `UnitPrice` TemplateFields，以確保輸入的價格大於或等於 0 的值，並以有效的貨幣格式顯示。

> [!NOTE]
> 雖然 ASP.NET 1.x 有這些相同的五個驗證控制項、 ASP.NET 2.0 新增了一些改進功能、 主要兩位用戶端指令碼支援非 Internet Explorer 的瀏覽器和到頁面上的資料分割驗證控制項的能力驗證群組。 如需有關 2.0 中新的驗證控制項功能的詳細資訊，請參閱[剖析 ASP.NET 2.0 中的驗證控制項](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)。


讓我們開始新增必要的驗證控制項，以`EditItemTemplate`GridView 的 TemplateFields。 若要這麼做，按一下 GridView 的智慧標籤，即可啟動 [編輯範本] 介面中的 [編輯範本] 連結。 從這裡開始，您可以選取要從下拉式清單中編輯的範本。 因為我們想要擴充的編輯介面，我們需要將驗證控制項加入`ProductName`並`UnitPrice`的`EditItemTemplate`s。


[![我們需要擴充產品名稱和單價的 EditItemTemplates](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image10.png)

**圖 4**： 我們要擴充`ProductName`並`UnitPrice`的`EditItemTemplate`s ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image12.png))


在  `ProductName` `EditItemTemplate`，藉由將它從 工具箱 拖曳至範本的編輯介面，加入 RequiredFieldValidator 放後的文字方塊中。


[![RequiredFieldValidator 加入 ProductName EditItemTemplate](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image13.png)

**圖 5**： 新增至 RequiredFieldValidator `ProductName` `EditItemTemplate` ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image15.png))


所有驗證控制項來都處理驗證單一 ASP.NET Web 控制項的輸入。 因此，我們要指出我們剛才加入 RequiredFieldValidator 應該驗證在文字方塊`EditItemTemplate`; 這可以藉由設定驗證控制項[ControlToValidate 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx)至`ID`適當的 Web 控制項。 [] 文字方塊中目前有相當的描述性`ID`的`TextBox1`，但讓我們將它變更為更適當。 在文字方塊中，在範本上按一下，然後，從 [屬性] 視窗中，變更`ID`從`TextBox1`至`EditProductName`。


[![將文字方塊的識別碼變更為 EditProductName](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image16.png)

**圖 6**： 變更文字方塊`ID`要`EditProductName`([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image18.png))


接下來，設定 RequiredFieldValidator`ControlToValidate`屬性設`EditProductName`。 最後，設定[ErrorMessage 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx)「 您必須提供的產品名稱 」，[文字屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx)來 「\*"。 `Text`屬性值，如果提供，是在驗證失敗時，會將驗證控制項所顯示的文字。 `ErrorMessage`屬性值，也就是必要項目，由 ValidationSummary 控制項; 如果`Text`省略屬性值，則`ErrorMessage`屬性值也是無效的輸入上的驗證控制項所顯示的文字。

設定之後的 RequiredFieldValidator 這三個屬性，您的畫面看起來應該類似於 圖 7。


[![設定 RequiredFieldValidator ControlToValidate、 錯誤訊息，以及文字屬性](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image19.png)

**圖 7**： 設定 RequiredFieldValidator `ControlToValidate`， `ErrorMessage`，以及`Text`屬性 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image21.png))


使用新增至 RequiredFieldValidator `ProductName` `EditItemTemplate`，則所有剩下是加入必要的驗證，以`UnitPrice` `EditItemTemplate`。 因為我們已決定，此頁面上，針對`UnitPrice`選擇性當編輯一筆記錄，我們不需要新增 RequiredFieldValidator。 我們，不過，需要新增以確保 CompareValidator `UnitPrice`，提供，如果已正確格式化為貨幣，且大於或等於 0。

我們將新增至 CompareValidator 之前`UnitPrice` `EditItemTemplate`，讓我們先將變更從 TextBox Web 控制項的 ID`TextBox2`至`EditUnitPrice`。 完成此變更之後，新增 CompareValidator，設定其`ControlToValidate`屬性，以`EditUnitPrice`、 其`ErrorMessage`屬性，以 「 價格必須大於或等於零，而且不能包含貨幣符號"並將其`Text`屬性"\*".

表示`UnitPrice`值必須是大於或等於 0，設定的 CompareValidator[運算子屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx)要`GreaterThanEqual`、 其[ValueToCompare 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx)"0"，和其[型別屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx)至`Currency`。 下列宣告式語法示範`UnitPrice`TemplateField 的`EditItemTemplate`在進行這些變更之後：

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample3.aspx)]

進行這些變更之後，請在瀏覽器中開啟頁面。 如果您嘗試以省略名稱，或編輯產品時，請輸入不正確的價格值，則會在文字方塊旁邊出現星號。 如 [圖 8] 所示，包含貨幣符號，例如 $19.95 價格值會被視為無效。 CompareValidator `Currency` `Type`可讓數字分隔符號 （例如逗號或句號，根據文化特性設定而定） 和前置加號或減號，但*不*允許貨幣符號。 此行為可能 perplex 使用者，因為編輯介面目前呈現`UnitPrice`使用貨幣格式。

> [!NOTE]
> 請注意，在*與插入、 更新和刪除相關聯的事件*教學課程中，我們設定 BoundField`DataFormatString`屬性設`{0:c}`才能將它格式化為貨幣。 此外，我們會設定`ApplyFormatInEditMode`屬性設為 true，造成 GridView 的編輯介面，以格式化`UnitPrice`為貨幣。 Visual Studio 時轉換為 TemplateField BoundField，記下這些設定，並格式化文字方塊的`Text`屬性為貨幣，使用資料繫結語法`<%# Bind("UnitPrice", "{0:c}") %>`。


[![具有無效的輸入文字方塊旁邊出現星號](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image22.png)

**圖 8**: 星號顯示下一個具有無效的輸入文字方塊 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image24.png))


同時做為驗證運作方式-就是，使用者必須手動編輯記錄，這不是可接受時，移除的貨幣符號。 若要解決此問題，我們有三個選項：

1. 設定`EditItemTemplate`以便`UnitPrice`值不格式化為貨幣。
2. 可讓使用者藉由移除 CompareValidator RegularExpressionValidator 會正確檢查是格式正確的貨幣值，以取代輸入貨幣符號。 此處的問題是要驗證之貨幣值的規則運算式不是完美，需要撰寫程式碼，如果我們想要納入的文化特性設定。
3. 完全移除驗證控制項，並依賴伺服器端驗證邏輯，在 GridView 的`RowUpdating`事件處理常式。

讓我們針對此練習的選項 #1。 目前`UnitPrice`格式化為貨幣，因為文字方塊中的資料繫結運算式`EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`。 變更繫結陳述式來`Bind("UnitPrice", "{0:n2}")`，它將結果格式化為兩位數的有效位數的數字。 這可以直接透過宣告式語法，或從 編輯資料繫結 連結，即可完成`EditUnitPrice`中的文字方塊`UnitPrice`TemplateField 的`EditItemTemplate`（請參閱 圖 9 和 10）。


[![按一下文字方塊的 [編輯資料繫結] 連結](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image25.png)

**圖 9**： 按一下文字方塊的編輯資料繫結連結 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image27.png))


[![在 繫結陳述式中指定的格式規範](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image28.png)

**圖 10**： 指定中的格式規範`Bind`陳述式 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image30.png))


透過這項變更，編輯介面中格式化的價格包含做為群組分隔符號的逗號和句號當做十進位分隔符號，但缺乏的貨幣符號。

> [!NOTE]
> `UnitPrice` `EditItemTemplate`不包含 RequiredFieldValidator，允許以發生回傳和更新的邏輯，以開始進行。 不過，`RowUpdating`從複製的事件處理常式*檢查與插入、 更新和刪除事件相關聯*教學課程包含以程式設計方式檢查，可確保`UnitPrice`提供。 歡迎您移除此邏輯，請將它保留在為-，或新增至 RequiredFieldValidator `UnitPrice` `EditItemTemplate`。


## <a name="step-4-summarizing-data-entry-problems"></a>步驟 4： 彙總資料輸入問題

ASP.NET 包含五個驗證控制項，除了[ValidationSummary 控制項](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx)，用來顯示`ErrorMessage`之驗證控制項偵測到無效的資料。 此摘要的資料可以顯示為網頁上或透過強制回應的用戶端的訊息方塊的文字。 讓我們來增強本教學課程，包括用戶端 messagebox 進行任何驗證問題的摘要。

若要這麼做，請從 [工具箱] 拖曳至設計工具拖曳 ValidationSummary 控制項。 驗證控制項的位置並不很重要，因為我們要將它設定為只會顯示摘要為 messagebox。 加入控制項之後, 設定其[ShowSummary 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx)要`False`及其[ShowMessageBox 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx)到`True`。 此步驟中，任何驗證錯誤摘要說明在用戶端 messagebox 中。


[![驗證錯誤會摘要在用戶端 Messagebox](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image31.png)

**圖 11**： 用戶端 Messagebox 摘要說明的驗證錯誤 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image33.png))


## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>步驟 5： 將驗證控制項新增至 DetailsView`InsertItemTemplate`

所有在此教學課程是將驗證控制項新增至 DetailsView 插入的介面。 將驗證控制項新增至 DetailsView 範本的程序等同於在步驟 3 中，檢查因此，我們將快速瀏覽此步驟中的工作。 使用 GridView 的一樣`EditItemTemplate`s，建議您重新命名`ID`從描述性文字方塊之`TextBox1`和`TextBox2`來`InsertProductName`和`InsertUnitPrice`。

新增至 RequiredFieldValidator `ProductName` `InsertItemTemplate`。 設定`ControlToValidate`來`ID`的文字方塊中，在範本中，其`Text`屬性，以 「\*"並將其`ErrorMessage`「 您必須提供的產品名稱 」 屬性。

由於`UnitPrice`是當手寫筆新記錄時，所需的此頁面，新增至 RequiredFieldValidator `UnitPrice` `InsertItemTemplate`，將其`ControlToValidate`， `Text`，和`ErrorMessage`屬性適當地。 最後，新增至 CompareValidator `UnitPrice` `InsertItemTemplate` ，設定其`ControlToValidate`， `Text`， `ErrorMessage`， `Type`， `Operator`，以及`ValueToCompare`屬性就像我們一樣`UnitPrice`的 GridView 內的 CompareValidator `EditItemTemplate`。

之後新增這些驗證控制項，無法新增至系統，如果未提供其名稱，或其價格是負數或非法格式化一個新的產品。


[![驗證邏輯已新增至 DetailsView 插入介面](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image34.png)

**圖 12**： 已新增驗證邏輯至 DetailsView 插入介面 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image36.png))


## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>步驟 6： 將驗證控制項分割驗證群組

我們的頁面包含兩個以邏輯方式不同集合的驗證控制項： 與 GridView 的編輯介面，並與 DetailsView 的插入介面。 根據預設，當會回傳*所有*簽入驗證頁面上的控制項。 不過，編輯一筆記錄時我們不想要驗證的 DetailsView 插入介面的驗證控制項。 圖 13 說明我們目前的難題，當使用者編輯的產品，完全合法的值時，按一下 更新會造成驗證錯誤，因為插入的介面中的名稱和價格的值為空白。


[![更新產品會造成引發插入介面的驗證控制項](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image37.png)

**圖 13**： 更新產品會造成引發插入之介面的驗證控制項 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image39.png))


ASP.NET 2.0 中的驗證控制項可以分割成透過驗證群組其`ValidationGroup`屬性。 若要建立一組驗證控制項群組中的關聯，請設定其`ValidationGroup`屬性設為相同的值。 教學課程中，設定`ValidationGroup`GridView 的 TemplateFields，若要在驗證控制項的屬性`EditValidationControls`而`ValidationGroup`屬性以 DetailsView TemplateFields `InsertValidationControls`。 這些變更可直接在宣告式標記，或透過 [屬性] 視窗中使用設計工具時編輯 [範本] 介面。

除了驗證控制項、 按鈕和按鈕的相關的控制項，在 ASP.NET 2.0 也包含`ValidationGroup`屬性。 驗證群組的驗證程式會檢查其有效性只有當您引發具有同一個按鈕所回傳是，才`ValidationGroup`屬性設定。 例如，為了讓 DetailsView 插入按鈕來觸發`InsertValidationControls`驗證群組，我們需要設定 CommandField`ValidationGroup`屬性設`InsertValidationControls`（請參閱 圖 14）。 此外，設定 GridView 的 CommandField`ValidationGroup`屬性設`EditValidationControls`。


[![設定 DetailsView 的 InsertValidationControls CommandField 的 ValidationGroup 屬性](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image40.png)

**圖 14**： 設定 DetailsView CommandField 的`ValidationGroup`屬性設`InsertValidationControls`([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image42.png))


在這些變更之後, 的 GridView 與 DetailsView TemplateFields 和 CommandFields 看起來應該如下所示：

DetailsView TemplateFields 和 CommandField

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample4.aspx)]

GridView 的 CommandField 和 TemplateFields

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample5.aspx)]

編輯特定的驗證控制項此時按一下 DetailsView 的 插入 按鈕時，解決問題以 圖 13 反白顯示時，才會引發只有在按一下 GridView 的 更新 按鈕時才和插入特定的驗證控制項引發。 不過，這項變更我們 ValidationSummary 控制項不會再顯示在輸入無效的資料時。 ValidationSummary 控制項還包含`ValidationGroup`屬性，只顯示摘要資訊的驗證控制項，其驗證群組中。 因此，我們需要將兩個驗證控制項放在這個頁面上，一個用於`InsertValidationControls`驗證群組，一個用於`EditValidationControls`。

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample6.aspx)]

添加這在本教學課程已完成 ！

## <a name="summary"></a>總結

雖然 BoundFields 可以提供同時插入和編輯介面，該介面不是可自訂的。 通常，我們想要將驗證控制項新增至編輯和插入介面來確保使用者輸入必要的輸入，合法的格式。 若要這麼做，就必須轉換 TemplateFields BoundFields，並將驗證控制項新增至適當的範本。 在本教學課程中，我們擴充的範例*檢查與插入、 更新和刪除事件相關聯*教學課程中，將驗證控制項新增至這兩個 DetailsView 的插入介面和 GridView 的編輯介面。 此外，我們看到如何顯示使用 ValidationSummary 控制項的摘要驗證資訊以及如何分割成不同的驗證群組的頁面上的驗證控制項。

如我們在本教學課程中所見的 TemplateFields 允許予以增加以包含驗證控制項的編輯和插入介面。 TemplateFields 也可以擴充以包含額外的輸入的 Web 控制項，讓文字方塊來取代更適合的 Web 控制項。 在我們的下一個教學課程中，我們將看到如何使用資料繫結 DropDownList 控制項，也就是理想的編輯外部索引鍵時取代 TextBox 控制項 (例如`CategoryID`或是`SupplierID`在`Products`資料表)。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Liz Shulok 和 Zack Jones。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
> [下一頁](customizing-the-data-modification-interface-vb.md)
