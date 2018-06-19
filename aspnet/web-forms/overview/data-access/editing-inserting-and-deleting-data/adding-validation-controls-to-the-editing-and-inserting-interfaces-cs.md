---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs
title: 加入驗證控制項的編輯，並插入介面 (C#) |Microsoft 文件
author: rick-anderson
description: 在本教學課程中，我們會看到驗證控制項加入 EditItemTemplate 和 Web 控制項，將資料上，以提供更是多麼的輕鬆...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 2086cb1a-ab78-49ae-9c0b-03891c69776a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs
msc.type: authoredcontent
ms.openlocfilehash: 3df80984d15e13efddc52497d600d396d775d365
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888444"
---
<a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-c"></a>加入驗證控制項的編輯，並插入介面 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_CS.exe)或[下載 PDF](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/datatutorial19cs1.pdf)

> 在本教學課程中，我們會看到驗證控制項加入 EditItemTemplate 和 Web 控制項，將資料上，提供更容易的使用者介面是多麼的輕鬆。


## <a name="introduction"></a>簡介

我們已探索的 GridView 和 DetailsView 控制項範例中三個教學課程所有已撰寫 BoundFields 和 CheckBoxFields （的欄位型別繫結至資料來源的 GridView 或 DetailsView 時，自動加入由 Visual Studio 的過去透過智慧標籤控制項）。 當編輯 GridView 或 DetailsView 中的資料列，這些不是唯讀的 BoundFields 會轉換成等文字方塊中，終端使用者可以從中修改現有的資料。 同樣地，當插入新記錄變成 DetailsView 控制項，這些 BoundFields 其`InsertVisible`屬性設定為`true`（預設值） 會呈現為空白的文字方塊，其中使用者可以提供新的記錄欄位值。 同樣地，CheckBoxFields，標準的唯讀介面中停用時，會轉換為已啟用的核取方塊中編輯和插入介面。

雖然預設編輯和插入介面 BoundField 和 CheckBoxField 很有幫助，介面的任何類型的驗證。 如果使用者將資料輸入錯誤-例如省略`ProductName`欄位或輸入的值無效`UnitsInStock`（例如-50) 將會引發例外狀況從應用程式架構的深度內。 雖然此處理例外狀況可以依正常程序中所示[上一個教學課程](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)，理想的情況下編輯，或插入使用者介面會包含驗證控制項，以防止使用者輸入中這類無效的資料第一個位置。

若要提供自訂的編輯或插入的介面，我們要取代 BoundField 或 CheckBoxField 具有為 TemplateField。 TemplateFields，所討論的主題中[GridView 控制項中的使用 TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md)和[DetailsView 控制項中的使用 TemplateFields](../custom-formatting/using-templatefields-in-the-detailsview-control-cs.md)教學課程，可包含多個範本定義分隔不同的資料列狀態的介面。 TemplateField`ItemTemplate`時，用來呈現唯讀欄位或 DetailsView 或 GridView 控制項中的資料列而`EditItemTemplate`和`InsertItemTemplate`表示要進行編輯，然後分別插入模式中，使用的介面。

本教學課程中我們會看到驗證控制項加入 TemplateField 是多麼的輕鬆`EditItemTemplate`和`InsertItemTemplate`提供更容易的使用者介面。 具體來說，本教學課程會採用此範例中建立[檢查插入、 更新和刪除與事件相關聯](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)教學課程和加強編輯和插入介面以包含適當的驗證。

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deletingexamining-the-events-associated-with-inserting-updating-and-deleting-csmd"></a>步驟 1： 複製的範例中[檢查的事件相關聯插入、 更新和刪除](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)

在[檢查插入、 更新和刪除與事件相關聯](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)教學課程中我們建立了所列的名稱和的可編輯的 GridView 中的產品價格的頁面。 此外，網頁包含 DetailsView 其`DefaultMode`屬性設定為`Insert`，藉此一律呈現在插入模式。 從這個 DetailsView 中，使用者無法輸入新的產品名稱和價格、 按一下 [插入]，以及將它新增至系統 （請參閱圖 1）。


[![上述範例可讓使用者加入新的產品和編輯現有的](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image1.png)

**圖 1**: 先前範例可讓使用者加入新的產品，並編輯現有的 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image3.png))


我們在此教學課程的目標是以擴大來提供驗證控制項的 DetailsView 和 GridView。 特別是，我們將驗證邏輯將會：

- 需要插入或編輯產品時提供名稱
- 需要時插入記錄; 提供價格當編輯一筆記錄，我們仍會要求使用一個價格，但將在 GridView 中使用的程式設計邏輯`RowUpdating`事件處理常式已經存在，從較早的教學課程
- 請確定輸入的價格的值是有效的貨幣格式

我們來看看加強先前的範例包括驗證之前，我們必須先將複寫的範例中`DataModificationEvents.aspx`頁面，即可在此教學課程中，頁面`UIValidation.aspx`。 若要完成此我們需要透過同時複製`DataModificationEvents.aspx`網頁的宣告式標記和其原始碼。 第一次複製的宣告式標記執行下列步驟：

1. 開啟`DataModificationEvents.aspx`Visual Studio 中的頁面
2. 請移至網頁的宣告式標記 （在頁面底部的 [來源] 按鈕按一下）
3. 複製內的文字`<asp:Content>`和`</asp:Content>`標記 （線條 3 到 44） 中所顯示的圖 2。


[![複製文字內&lt;asp: Content&gt;控制項](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image4.png)

**圖 2**： 複製文字內`<asp:Content>`控制項 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image6.png))


1. 開啟`UIValidation.aspx`頁面
2. 請移至網頁的宣告式標記
3. 貼上文字內`<asp:Content>`控制項。

若要複製的原始碼，開啟`DataModificationEvents.aspx.cs`頁面上，並將複製文字*內*`EditInsertDelete_DataModificationEvents`類別。 複製的三個事件處理常式 (`Page_Load`， `GridView1_RowUpdating`，和`ObjectDataSource1_Inserting`)，但不要**不**複製類別宣告或`using`陳述式。 貼上複製的文字*內*`EditInsertDelete_UIValidation`類別`UIValidation.aspx.cs`。

透過內容和程式碼移動之後`DataModificationEvents.aspx`至`UIValidation.aspx`，花一點時間來測試您的瀏覽器中的進度。 您應該會看到相同的輸出和體驗相同的功能，在每個這兩個分頁 (請參閱上一步圖 1 的螢幕擷取畫面`DataModificationEvents.aspx`動作中)。

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>步驟 2： 將 BoundFields 轉換成 TemplateFields

若要加入驗證控制項的編輯和插入介面，需要轉換成 TemplateFields BoundFields DetailsView 和 GridView 控制項所使用的。 為了達成此目的，分別按一下 編輯資料行和編輯的欄位中連結的 GridView 和 DetailsView 的智慧標籤。 選取裝置，選取每個 BoundFields，然後按一下 「 將這個欄位轉換為 TemplateField 」 的連結。


[![每個的 DetailsView 的和 GridView 的 BoundFields 轉換 TemplateFields](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image7.png)

**圖 3**： 轉換每個的 DetailsView 的和 GridView 的 BoundFields 到 TemplateFields ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image9.png))


BoundField 轉換為 TemplateField 透過 [欄位] 對話方塊會產生表現相同的唯讀、 編輯及插入介面本身 BoundField 為 TemplateField。 下列標記顯示的宣告式語法`ProductName`之後已轉換為 TemplateField DetailsView 中的欄位：

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample1.aspx)]

請注意此 TemplateField 有三個自動建立的範本`ItemTemplate`， `EditItemTemplate`，和`InsertItemTemplate`。 `ItemTemplate`顯示單一資料欄位值 (`ProductName`) 使用標籤 Web 控制項，而`EditItemTemplate`和`InsertItemTemplate`使資料欄位與文字方塊中的文字方塊中的 Web 控制項中呈現資料欄位值`Text`使用雙向資料繫結的屬性。 因為我們只插入此頁面中使用 DetailsView，您可能會移除`ItemTemplate`和`EditItemTemplate`從兩個 TemplateFields，雖然沒有壞處中保留。

因為 GridView 不支援內建插入 DetailsView 的功能、 將轉換的 GridView `ProductName` TemplateField 欄位會導致只`ItemTemplate`和`EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample2.aspx)]

依序按一下"轉換為 TemplateField 到這個欄位 」，Visual Studio 已建立其範本模擬之使用者介面的轉換 BoundField TemplateField。 您可以瀏覽透過瀏覽器的此頁面來確認。 您會發現的外觀和行為 TemplateFields 是相同的體驗時 BoundFields 所使用。

> [!NOTE]
> 請隨意自訂所需之範本中的編輯介面。 例如，我們可能會想要在文字方塊中`UnitPrice`TemplateFields 轉譯為較小的文字方塊中，比`ProductName`文字方塊。 若要完成這項作業中，您可以設定文字方塊的`Columns`屬性設為適當的值，或提供透過絕對寬度`Width`屬性。 在下一個教學課程中我們會看到如何完全自訂 Web 控制項的替代資料項目以取代文字方塊中的 編輯介面。


## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>步驟 3： 將驗證控制項加入至 GridView`EditItemTemplate` s

建構資料輸入表單時, 很重要，使用者輸入任何必要的欄位和所有提供的輸入是合法、 格式正確的值。 為了協助確保使用者的輸入都有效，ASP.NET 提供專為用來驗證輸入控制項的單一值的五個內建的驗證控制項：

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx)可確保已提供的值
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx)驗證值與另一個 Web 控制項值或常數的值，或確保值的格式是合法的指定的資料型別
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx)確保值的值範圍內
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx)驗證值，以針對[規則運算式](http://en.wikipedia.org/wiki/Regular_expression)
- [一起](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx)驗證值，以符合自訂的使用者定義的方法

如需有關這些五個控制項的詳細資訊，請參閱[驗證控制項 」 一節](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx)的[ASP.NET 快速入門教學課程](https://asp.net/QuickStart/aspnet/)。

在教學課程中我們將需要使用在 DetailsView 和 GridView 的 RequiredFieldValidator `ProductName` TemplateFields 和在 DetailsView 的 RequiredFieldValidator `UnitPrice` TemplateField。 此外，我們需要加入這兩個控制項的 CompareValidator `UnitPrice` TemplateFields，以確保輸入的價格大於或等於 0 的值，並以有效的貨幣格式顯示。

> [!NOTE]
> 雖然 ASP.NET 1.x 具有這些相同的五個驗證控制項、 ASP.NET 2.0 已新增許多改進功能、 主要的用戶端指令碼的兩個支援非 Internet Explorer 的瀏覽器和到頁面上的資料分割驗證控制項的功能驗證群組。 如需 2.0 中的新驗證控制項功能的詳細資訊，請參閱[將 ASP.NET 2.0 中的驗證控制項的分解](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)。


我們先來新增必要的驗證控制項，以`EditItemTemplate`GridView TemplateFields。 若要完成此動作，按一下編輯樣板中的連結即可啟動 [編輯範本] 介面的 GridView 的智慧標籤。 從這裡，您可以選取要從下拉式清單中編輯的範本。 因為我們想要加強編輯介面，我們需要加入驗證控制項`ProductName`和`UnitPrice`的`EditItemTemplate`s。


[![我們要擴充的產品名稱和單價的 EditItemTemplates](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image10.png)

**圖 4**： 我們需要將延伸`ProductName`和`UnitPrice`的`EditItemTemplate`s ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image12.png))


在`ProductName` `EditItemTemplate`，將它從 [工具箱] 拖曳至範本編輯介面，加入 RequiredFieldValidator 放後的文字方塊。


[![加入 ProductName EditItemTemplate RequiredFieldValidator](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image13.png)

**圖 5**： 新增至 RequiredFieldValidator `ProductName` `EditItemTemplate` ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image15.png))


所有驗證控制項的都運作方式驗證單一 ASP.NET Web 控制項的輸入。 因此，我們需要表示我們剛才加入的 RequiredFieldValidator 應該驗證在文字方塊中`EditItemTemplate`; 這會透過設定驗證控制項的[ControlToValidate 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx)至`ID`適當的 Web 控制項。 文字方塊中目前有而描述性`ID`的`TextBox1`，但我們將它變更為更適當的項目。 在範本中的文字方塊中按一下，然後，從 [屬性] 視窗中，變更`ID`從`TextBox1`至`EditProductName`。


[![將文字方塊的識別碼變更為 EditProductName](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image16.png)

**圖 6**： 變更文字方塊的`ID`至`EditProductName`([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image18.png))


接下來，設定 RequiredFieldValidator`ControlToValidate`屬性`EditProductName`。 最後，設定[ErrorMessage 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx)至 「 您必須提供的產品名稱 」 和[Text 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx)至 「\*"。 `Text`屬性值，如果提供，是在驗證失敗時，會將驗證控制項所顯示的文字。 `ErrorMessage`屬性值，這是必要的由 ValidationSummary 控制項; 如果`Text`省略屬性值，則`ErrorMessage`屬性值也是無效的輸入上的驗證控制項所顯示的文字。

在設定後的 RequiredFieldValidator 這三個屬性，您的畫面看起來應該類似圖 7。


[![設定 RequiredFieldValidator ControlToValidate、 錯誤訊息和文字屬性](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image19.png)

**圖 7**： 設定 RequiredFieldValidator `ControlToValidate`， `ErrorMessage`，和`Text`屬性 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image21.png))


與加入 RequiredFieldValidator `ProductName` `EditItemTemplate`，則所有剩下是加入必要的驗證，以`UnitPrice` `EditItemTemplate`。 因為我們已經決定，此頁面上，針對`UnitPrice`選擇性當編輯記錄，我們不需要新增 RequiredFieldValidator。 我們，不過，需要手動新增，確保 CompareValidator `UnitPrice`，提供，如果已正確格式化為貨幣，且大於或等於 0。

我們新增至 CompareValidator 之前`UnitPrice` `EditItemTemplate`，讓我們先將變更從文字方塊中的 Web 控制項的 ID`TextBox2`至`EditUnitPrice`。 新增 CompareValidator，設定變更之後，其`ControlToValidate`屬性`EditUnitPrice`、 其`ErrorMessage`屬性設為 「 價格必須大於或等於零並不能包含貨幣符號 」，並將其`Text`屬性"\*".

表示`UnitPrice`值必須是大於或等於 0，且設定 CompareValidator[運算子屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx)至`GreaterThanEqual`、 其[ValueToCompare 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx)為"0"，而且其[型別屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx)至`Currency`。 下列宣告式語法示範`UnitPrice`TemplateField 的`EditItemTemplate`進行這些變更之後：

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample3.aspx)]

進行這些變更之後，請在瀏覽器中開啟頁面。 如果您嘗試以省略名稱，或輸入無效的價格值編輯產品時，則會在文字方塊旁邊出現星號。 如圖 8 所示，包含貨幣符號，例如 $19.95 的價格值會被視為無效。 CompareValidator `Currency` `Type`允許 （例如逗號或句號，根據文化特性設定而定） 的數字分隔符號的開頭的加號或減號，但是沒有*不*允許貨幣符號。 這種行為可能 perplex 使用者編輯介面目前轉譯`UnitPrice`使用貨幣格式。

> [!NOTE]
> 請記得，在*事件相關聯插入、 更新和刪除*教學課程中，我們設定 BoundField`DataFormatString`屬性`{0:c}`才能格式化為貨幣。 此外，我們設定`ApplyFormatInEditMode`屬性設定為 true，造成 GridView 的編輯介面，以格式化`UnitPrice`為貨幣。 Visual Studio 時轉換為 TemplateField BoundField，記下這些設定，並格式化文字方塊的`Text`屬性做為貨幣，使用資料繫結語法`<%# Bind("UnitPrice", "{0:c}") %>`。


[![具有無效的輸入的文字方塊旁邊出現星號](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image22.png)

**圖 8**: 星號會顯示下一步 以無效的輸入的文字方塊 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image24.png))


雖然做為驗證運作的方式-就是，使用者必須手動編輯記錄，這不是可接受時，移除的貨幣符號。 若要補救這種情況，我們有三個選項：

1. 設定`EditItemTemplate`以便`UnitPrice`值不格式化為貨幣。
2. 可讓使用者藉由移除 CompareValidator 取代正常會檢查有格式正確的貨幣值 RegularExpressionValidator 輸入貨幣符號。 此處的問題是要驗證之貨幣值的規則運算式不是完美和需要撰寫程式碼，如果我們想要納入的文化特性設定。
3. 完全移除驗證控制項，而且依賴 GridView 中的伺服器端驗證邏輯`RowUpdating`事件處理常式。

我們來使用針對此練習選項 #1。 目前`UnitPrice`格式化為貨幣，因為在文字方塊中的資料繫結運算式`EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`。 變更繫結陳述式， `Bind("UnitPrice", "{0:n2}")`，它將結果格式化為兩位數的有效位數的數字。 這可以透過宣告式語法直接或按一下 編輯資料繫結 連結，從完成`EditUnitPrice` 文字方塊中的`UnitPrice`TemplateField 的`EditItemTemplate`（請參閱圖 9 和 10）。


[![按一下文字方塊中編輯資料繫結連結](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image25.png)

**圖 9**： 按一下文字方塊中編輯資料繫結連結 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image27.png))


[![在繫結陳述式中指定的格式規範](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image28.png)

**圖 10**： 指定中的格式規範`Bind`陳述式 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image30.png))


這項變更，編輯介面中的格式化的價格包含做為群組分隔符號逗號和句號當做十進位分隔符號，但離開關閉貨幣符號。

> [!NOTE]
> `UnitPrice` `EditItemTemplate`不包含 RequiredFieldValidator，允許發生回傳，而以開始更新邏輯。 不過，`RowUpdating`從複製的事件處理常式*檢查插入、 更新和刪除與事件相關聯*教學課程包含以程式設計方式檢查，可確保`UnitPrice`提供。 歡迎移除此邏輯，讓它在做為-，或加入至 RequiredFieldValidator `UnitPrice` `EditItemTemplate`。


## <a name="step-4-summarizing-data-entry-problems"></a>步驟 4： 摘要資料輸入問題

除了五個驗證控制項，包含 ASP.NET [ValidationSummary 控制項](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx)，用來顯示`ErrorMessage`之偵測到無效的資料，這些驗證控制項。 此摘要的資料可以顯示為網頁上或透過強制回應，用戶端的訊息方塊的文字。 讓我們來提升這個教學課程，包括用戶端 messagebox 摘要的任何驗證問題。

若要達成此目的，拖曳到 ValidationSummary 控制項從 [工具箱] 拖曳至設計工具。 驗證控制項的位置不會真的很重要，因為我們會將它設定為只顯示摘要為 messagebox。 加入控制項之後, 設定其[ShowSummary 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx)至`false`及其[ShowMessageBox 屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx)至`true`。 此新增功能，使用任何驗證錯誤摘要說明在用戶端 messagebox 中。


[![驗證錯誤摘要說明在用戶端訊息方塊](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image31.png)

**圖 11**: 驗證錯誤摘要說明在用戶端 Messagebox ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image33.png))


## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>步驟 5： 將驗證控制項加入至 DetailsView 的`InsertItemTemplate`

所有會維持在此教學課程都會將驗證控制項新增至 DetailsView 的插入介面。 在 DetailsView 的範本中加入驗證控制項中的程序等同於步驟 3; 在檢查因此，我們將會快速瀏覽此步驟中的工作。 如同我們一樣 GridView `EditItemTemplate` s，建議您重新命名`ID`從描述性文字方塊之`TextBox1`和`TextBox2`至`InsertProductName`和`InsertUnitPrice`。

加入至 RequiredFieldValidator `ProductName` `InsertItemTemplate`。 設定`ControlToValidate`至`ID`在範本中，文字方塊中的其`Text`屬性 」\*"及其`ErrorMessage`「 您必須提供的產品名稱 」 的屬性。

因為`UnitPrice`是加入新的記錄時，此頁面所需，新增至 RequiredFieldValidator `UnitPrice` `InsertItemTemplate`，設定其`ControlToValidate`， `Text`，和`ErrorMessage`屬性適當地。 最後，會加入至 CompareValidator `UnitPrice` `InsertItemTemplate` ，設定其`ControlToValidate`， `Text`， `ErrorMessage`， `Type`， `Operator`，和`ValueToCompare`屬性，就像以`UnitPrice`的 GridView 中的 CompareValidator `EditItemTemplate`。

加入這些驗證控制項之後, 無法新增至系統，如果未提供其名稱，或其價格是負數或非法格式化新的產品。


[![在 DetailsView 的插入介面已加入驗證邏輯](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image34.png)

**圖 12**： 驗證邏輯已新增至 DetailsView 的插入介面 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image36.png))


## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>步驟 6： 將驗證控制項分割驗證群組

我們的頁面是由驗證控制項的兩個邏輯上是不同組所組成： 那些對應至 GridView 的編輯介面和那些對應到在 DetailsView 的插入介面。 根據預設，當回傳*所有*簽入驗證頁面上的控制項。 不過，編輯一筆記錄時，我們不希望在 DetailsView 的插入介面的驗證控制項來驗證。 圖 13 說明我們目前難題，當使用者在編輯產品中完全合法的值時，按一下 更新會導致驗證錯誤，因為插入介面中的名稱和價格值是空白。


[![更新產品會造成引發插入介面的驗證控制項](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image37.png)

**圖 13**： 更新的產品會造成引發插入介面的驗證控制項 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image39.png))


在 ASP.NET 2.0 驗證控制項可以分割成驗證群組透過其`ValidationGroup`屬性。 若要關聯的一組驗證控制項群組中，直接將其`ValidationGroup`屬性設為相同的值。 教學課程中，設定`ValidationGroup`GridView TemplateFields，若要在驗證控制項的內容`EditValidationControls`和`ValidationGroup`屬性至 DetailsView 的 TemplateFields `InsertValidationControls`。 這些變更可直接在宣告式標記中完成，或者透過使用在設計工具時，屬性 視窗編輯範本 介面。

除了驗證控制項、 按鈕和按鈕相關的控制項，在 ASP.NET 2.0 中也包含`ValidationGroup`屬性。 當檢查有效的驗證群組的驗證程式僅當回傳由所引發具有相同的按鈕`ValidationGroup`屬性設定。 例如，為了讓 DetailsView 的 [插入] 按鈕觸發`InsertValidationControls`驗證群組，所以我們需要將 CommandField`ValidationGroup`屬性`InsertValidationControls`（請參閱圖 14）。 此外，設定 GridView CommandField 的`ValidationGroup`屬性`EditValidationControls`。


[![設定在 DetailsView 的 InsertValidationControls CommandField 的 ValidationGroup 屬性](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image40.png)

**圖 14**： 設定 DetailsView 的 CommandField 的`ValidationGroup`屬性`InsertValidationControls`([按一下以檢視完整大小的影像](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image42.png))


這些變更之後，請的 DetailsView GridView 的 TemplateFields 和 CommandFields 看起來應該如下所示：

在 DetailsView 的 TemplateFields 和 CommandField

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample4.aspx)]

GridView CommandField 和 TemplateFields

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample5.aspx)]

編輯特定的驗證控制項此時按一下 DetailsView 的 [插入] 按鈕時，解決問題的圖 13 反白顯示時，才會引發只按 GridView 的更新按鈕時會和插入專屬驗證控制項引發。 不過，透過這項變更我們 ValidationSummary 控制項不會再顯示輸入無效的資料時。 ValidationSummary 控制項還包含`ValidationGroup`屬性，只顯示摘要資訊的這些群組中驗證控制項的驗證。 因此，我們必須在此頁面中，一個用於將兩個驗證控制項`InsertValidationControls`驗證群組，另一個用於`EditValidationControls`。

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample6.aspx)]

這個加入我們的教學課程已完成 ！

## <a name="summary"></a>總結

雖然 BoundFields 可以提供同時插入與編輯介面，介面不是可自訂的。 一般而言，我們想要加入驗證控制項來編輯和插入介面，以確保使用者在合法的格式輸入必要的輸入。 若要完成這項作業就必須轉換 TemplateFields BoundFields，並將驗證控制項加入至適當的範本。 在本教學課程中，我們擴充的範例中*檢查插入、 更新和刪除與事件相關聯*教學課程中，加入這兩個 DetailsView 驗證控制項的插入介面和 GridView編輯介面。 此外，我們可了解如何顯示使用 ValidationSummary 控制項的摘要驗證資訊以及如何分割成不同的驗證群組頁面上驗證的控制項。

因為我們在本教學課程中所見，TemplateFields 允許編輯和插入介面，即可予以增加以包含驗證控制項。 TemplateFields 也可以擴充以包含其他輸入的 Web 控制項，啟用以更適合的 Web 控制項取代文字方塊。 在我們的下一個教學課程中我們會看到如何使用資料繫結 DropDownList 控制項，也就是理想的編輯外部索引鍵時取代文字方塊控制項 (例如`CategoryID`或`SupplierID`中`Products`資料表)。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已 Liz Shulok 和 Zack Jones。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)
> [下一頁](customizing-the-data-modification-interface-cs.md)
