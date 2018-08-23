---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-cs
title: 將驗證控制項新增至 DataList 的編輯介面 (C#) |Microsoft Docs
author: rick-anderson
description: 在本教學課程中，我們會看到以提供更萬無一失的編輯使用者 int 時，將驗證控制項新增至 DataList 的 EditItemTemplate 是多麼...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 3ecc21c5-da0e-40ab-abb4-fac1e47398ad
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 2fe85d6513a229f11b3aad7c7cc6c7124c94d70f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832021"
---
<a name="adding-validation-controls-to-the-datalists-editing-interface-c"></a>將驗證控制項新增至 DataList 的編輯介面 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_CS.exe)或[下載 PDF](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/datatutorial39cs1.pdf)

> 在本教學課程中，我們會看到以提供更萬無一失的編輯使用者介面時，將驗證控制項新增至 DataList 的 EditItemTemplate 是多麼容易。


## <a name="introduction"></a>簡介

在 DataList 編輯教學課程到目前為止，編輯介面 DataLists 並未包含任何的主動式的使用者輸入的驗證即使無效的使用者輸入，例如遺漏的產品名稱或負的價格會產生例外狀況。 在 [前述教學課程](handling-bll-and-dal-level-exceptions-cs.md)中，我們檢查如何新增例外狀況處理程式碼以 DataList 的`UpdateCommand`事件處理常式，以便攔截，並依正常程序顯示任何所引發的例外狀況的相關資訊。 在理想情況下，不過，編輯介面會包含驗證控制項，以防止使用者在第一時間輸入這類無效的資料。

在本教學課程中我們會看到將驗證控制項新增至 DataList s 是多麼`EditItemTemplate`以提供更萬無一失的編輯使用者介面。 具體來說，本教學課程會採用先前的教學課程中建立的範例，並擴大編輯介面，以包含適當的驗證。

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptionshandling-bll-and-dal-level-exceptions-csmd"></a>步驟 1： 複寫的範例[處理 BLL 和 DAL 層級的例外狀況](handling-bll-and-dal-level-exceptions-cs.md)

在 [處理 BLL 和 DAL 層級例外狀況](handling-bll-and-dal-level-exceptions-cs.md)教學課程中我們建立的頁面，列出名稱及兩個資料行中，可以讓您編輯 DataList 中產品的價格。 我們在本教學課程的目標是要加強 DataList s 編輯介面，以加入驗證控制項。 特別是，我們將驗證邏輯將會：

- 需要提供產品的名稱
- 請確定輸入的價格的值是有效的貨幣格式
- 請確認輸入的價格是大於或等於零，因為負`UnitPrice`值是不合法

我們來看看擴充前一個範例，以包含驗證之前，我們必須先將複寫的範例`ErrorHandling.aspx`頁面中`EditDeleteDataList`資料夾，以在此教學課程中，頁面`UIValidation.aspx`。 若要這麼做我們要同時複製`ErrorHandling.aspx`頁面 s 宣告式標記和其原始程式碼。 第一次複製的宣告式標記執行下列步驟：

1. 開啟`ErrorHandling.aspx`Visual Studio 中的頁面
2. 請移至頁面 s 宣告式標記 （在頁面底部的 [來源] 按鈕按一下）
3. 複製內的文字`<asp:Content>`和`</asp:Content>`標籤 （程式行 3 到 32），為所示的 圖 1。


[![複製文字內&lt;asp: Content&gt;控制項](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image1.png)

**圖 1**： 複製文字內`<asp:Content>`控制項 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image3.png))


1. 開啟`UIValidation.aspx`頁面
2. 請移至頁面 s 宣告式標記
3. 貼上文字內`<asp:Content>`控制項。

若要複製的原始碼，開啟`ErrorHandling.aspx.vb`頁面上，並複製只是文字*內*`EditDeleteDataList_ErrorHandling`類別。 複製的三個事件處理常式 (`Products_EditCommand`， `Products_CancelCommand`，並`Products_UpdateCommand`) 連同`DisplayExceptionDetails`方法，但不要**不**複製類別宣告或`using`陳述式。 貼上複製的文字*內*`EditDeleteDataList_UIValidation`類別在`UIValidation.aspx.vb`。

移動內容，以及從程式碼之後`ErrorHandling.aspx`至`UIValidation.aspx`，花點時間來測試瀏覽器中的頁面。 您應該會看到相同的輸出，並體驗相同的功能，在每個 （請參閱 圖 2） 這兩個頁面。


[![UIValidation.aspx 頁面模擬 ErrorHandling.aspx 中的功能](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image4.png)

**圖 2**:`UIValidation.aspx`頁面會模擬中的功能`ErrorHandling.aspx`([按一下以檢視完整大小的影像](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image6.png))


## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>步驟 2： 將驗證控制項新增至 DataList 的 EditItemTemplate

當建構資料輸入表單，很重要，使用者輸入任何必要的欄位，其提供的輸入是法律、 格式正確的值。 為了協助確保使用者的輸入有效值，ASP.NET 會提供五個內建的驗證控制項是設計用來驗證單一輸入 Web 控制項的值：

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx)可確保已提供的值
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx)驗證值與另一個 Web 控制項的值或常數的值，或可確保值的格式不合法的指定的資料型別
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx)確保值的值範圍內
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx)驗證值[規則運算式](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx)驗證值，以符合自訂的使用者定義的方法

如需這些五個控制項的詳細資訊請參閱上一步[將驗證控制項加入編輯和插入介面](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)簽出或教學課程[驗證控制項區段](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx)的[ASP.NET 快速入門教學課程](https://quickstarts.asp.net)。

本教學課程中，我們必須使用以確保已提供的產品名稱的值 RequiredFieldValidator 和 CompareValidator，以確保輸入的價格大於或等於 0 的值，並以有效的貨幣格式顯示。

> [!NOTE]
> 雖然 ASP.NET 1.x 有這些相同的五個驗證控制項、 ASP.NET 2.0 新增了一些改進功能、 主要兩位用戶端指令碼支援除了 Internet Explorer 的瀏覽器和到頁面上的資料分割驗證控制項的能力驗證群組。 如需有關 2.0 中新的驗證控制項功能的詳細資訊，請參閱[剖析 ASP.NET 2.0 中的驗證控制項](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)。


讓著手將必要的驗證控制項新增至 DataList s s `EditItemTemplate`。 透過設計工具中，依序按一下 [編輯範本] 連結，從 DataList s 智慧標籤，或透過宣告式語法，可以執行這項工作。 可讓 s 步驟，透過使用 [編輯範本] 選項，從 [設計] 檢視的程序。 選擇編輯 DataList s 之後`EditItemTemplate`，從 [工具箱] 拖曳至範本的編輯介面，加入 RequiredFieldValidator 放之後`ProductName`文字方塊中。


[![[ProductName] 文字方塊之後新增的 EditItemTemplate RequiredFieldValidator](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image7.png)

**圖 3**： 新增至 RequiredFieldValidator `EditItemTemplate After` `ProductName`文字方塊中 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image9.png))


所有驗證控制項來都處理驗證單一 ASP.NET Web 控制項的輸入。 因此，我們需要以指出應該驗證我們剛才加入 RequiredFieldValidator`ProductName`文字方塊中，這是藉由設定驗證控制項 s [ `ControlToValidate`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx)到`ID`的適當的 Web 控制項 (`ProductName`，這個執行個體中)。 接下來，設定[`ErrorMessage`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx)您必須提供產品的名稱並[`Text`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx)至\*。 `Text`屬性值，如果提供，是在驗證失敗時，會將驗證控制項所顯示的文字。 `ErrorMessage`屬性值，也就是必要項目，由 ValidationSummary 控制項; 如果`Text`省略屬性值，則`ErrorMessage`上輸入無效的驗證控制項所顯示屬性值。

設定之後的 RequiredFieldValidator 這三個屬性，您的畫面看起來應該類似於 圖 4。


[![設定 RequiredFieldValidator 的 ControlToValidate、 錯誤訊息，以及文字屬性](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image10.png)

**圖 4**： 設定 RequiredFieldValidator s `ControlToValidate`， `ErrorMessage`，以及`Text`屬性 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image12.png))


使用新增至 RequiredFieldValidator `EditItemTemplate`，則所有剩下是加入必要的驗證產品 s 價格文字方塊中。 因為`UnitPrice`為選擇性時編輯記錄，我們不將 RequiredFieldValidator t 需求。 我們，不過，需要新增以確保 CompareValidator `UnitPrice`，提供，如果已正確格式化為貨幣，且大於或等於 0。

新增到 CompareValidator`EditItemTemplate`並將其`ControlToValidate`屬性設`UnitPrice`、 其`ErrorMessage`價格的屬性必須大於或等於零，而且不能包含貨幣符號，並將其`Text`屬性\*. 表示`UnitPrice`值必須是大於或等於 0，設定 CompareValidator s [ `Operator`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx)來`GreaterThanEqual`、 其[`ValueToCompare`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx)為 0，及其[`Type`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx)至`Currency`。

在之後加入下列兩個驗證的控制項，DataList 的`EditItemTemplate`s 宣告式語法看起來應該如下所示：


[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-cs/samples/sample1.aspx)]

進行這些變更之後，請在瀏覽器中開啟頁面。 如果您嘗試以省略名稱，或編輯產品時，請輸入不正確的價格值，則會在文字方塊旁邊出現星號。 如 [圖 5] 所示，包含貨幣符號，例如 $19.95 價格值會被視為無效。 CompareValidator s `Currency` `Type`可讓數字分隔符號 （例如逗號或句號，根據文化特性設定而定） 和前置加號或減號，但*不*允許貨幣符號。 此行為可能 perplex 使用者，因為編輯介面目前呈現`UnitPrice`使用貨幣格式。


[![具有無效的輸入文字方塊旁邊出現星號](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image13.png)

**圖 5**: 星號顯示下一個具有無效的輸入文字方塊 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image15.png))


同時做為驗證運作方式-就是，使用者必須手動編輯記錄，這不是可接受時，移除的貨幣符號。 此外，如果無效輸入中的編輯介面未更新或 [取消] 按鈕時，按下時，將會叫用的回傳。 在理想情況下，[取消] 按鈕會回到 DataList 其預先編輯的狀態，不論使用者的輸入的有效性。 此外，我們需要確保頁面的資料是否有效，再更新 DataList s 中的產品資訊`UpdateCommand`事件處理常式，可以略過用戶端邏輯，由其瀏覽器不支援 JavaScript，或有的使用者的驗證控制項停用其支援。

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>移除 EditItemTemplate 的 UnitPrice 文字方塊中的貨幣符號

當使用 CompareValidator 的`Currency``Type`，正在驗證的輸入不能包含任何貨幣符號。 這類符號的存在會導致將標示為無效輸入 CompareValidator。 不過，我們的編輯介面目前包含貨幣符號在`UnitPrice`文字方塊中，這表示使用者必須明確移除之前將其變更的貨幣符號。 若要解決這個問題，我們會有三個選項：

1. 設定`EditItemTemplate`以便`UnitPrice`文字方塊值不格式化為貨幣。
2. 可讓使用者藉由移除 CompareValidator 取代會檢查有格式正確的貨幣值 RegularExpressionValidator 輸入貨幣符號。 此處的挑戰是要驗證之貨幣值的規則運算式並不一樣直接 CompareValidator，而且需要撰寫程式碼，如果我們想要納入的文化特性設定。
3. 完全移除驗證控制項，並依賴 GridView s 中的自訂伺服器端驗證邏輯`RowUpdating`事件處理常式。

可讓 s 隨附此教學課程中的選項 1。 目前`UnitPrice`格式化為貨幣值，因為文字方塊中的資料繫結運算式`EditItemTemplate`: `<%# Eval("UnitPrice", "{0:c}") %>`。 變更`Eval`陳述式來`Eval("UnitPrice", "{0:n2}")`，它將結果格式化為兩位數的有效位數的數字。 這可以直接透過宣告式語法，或從 [編輯資料繫結] 連結，即可完成`UnitPrice`DataList s 中的文字方塊`EditItemTemplate`。

透過這項變更，編輯介面中格式化的價格包含做為群組分隔符號的逗號和句號當做十進位分隔符號，但缺乏的貨幣符號。

> [!NOTE]
> 當移除的可編輯的介面中的貨幣格式，我發現很有幫助將貨幣符號為外部文字方塊的文字。 這可做為提示給使用者，他們不需要提供的貨幣符號。


## <a name="fixing-the-cancel-button"></a>修正 [取消] 按鈕

根據預設，驗證 Web 控制項發出用戶端上執行驗證的 JavaScript。 按一下按鈕、 LinkButton 或 ImageButton 時，會發生回傳之前，先檢查頁面上的驗證控制項用戶端。 如果沒有任何無效的資料，便會取消回傳。 針對特定按鈕，不過，資料的有效性可能多少都沒關係;在此情況下，因為無效的資料而取消回傳是有點惹人討厭。

[取消] 按鈕是這類範例。 想像一下，使用者輸入無效的資料，例如省略 s 產品名稱，然後決定她不 t 想要在所有儲存的產品並按下 [取消] 按鈕。 目前，[取消] 按鈕就會觸發驗證控制項在頁面上，回報的產品名稱遺漏，並防止回傳。 我們的使用者必須輸入一些文字`ProductName`為了取消編輯程序的文字方塊。

幸運的是，按鈕、 LinkButton 和 ImageButton 已[`CausesValidation`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.causesvalidation.aspx)，可表示是否按一下按鈕應該會開始驗證邏輯 (預設為`True`)。 設定 [取消] 按鈕 s`CausesValidation`屬性設`False`。

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>確保輸入會在 UpdateCommand 事件處理常式中有效

由於用戶端指令碼發出的驗證控制項中，如果使用者輸入無效的輸入驗證控制項取消按鈕，LinkButton，所起始的任何回傳或 ImageButton 控制項`CausesValidation`屬性是`True`(預設值）。 不過，如果使用者造訪古的瀏覽器或其中一個支援的 JavaScript 已停用，將不會執行的用戶端驗證檢查。

ASP.NET 驗證控制項的所有重複回傳時，立即其驗證邏輯，並報告頁面的輸入，透過整體有效性[`Page.IsValid`屬性](https://msdn.microsoft.com/library/system.web.ui.page.isvalid.aspx)。 不過，未中斷或停止任何方式為基礎的值中的頁面流程`Page.IsValid`。 身為開發人員，很確定我們有責任`Page.IsValid`屬性的值為`True`假設有效的程式碼進行輸入資料之前。

如果使用者已停用 JavaScript，瀏覽我們的網頁、 編輯產品、 進入的價格值太昂貴，然後按一下 [更新] 按鈕，用戶端驗證將會略過和回傳接踵而至。 在回傳時，ASP.NET 頁面 s`UpdateCommand`事件處理常式執行，並會引發例外狀況，當嘗試剖析太難`Decimal`。 因為我們有例外狀況處理、 會依正常程序，處理這類例外狀況，但我們無法防止進度落後透過一開始只會繼續使用無效的資料`UpdateCommand`事件處理常式如果`Page.IsValid`的值為`True`。

將下列程式碼新增至開頭`UpdateCommand`事件處理常式之前，立即`Try`區塊：


[!code-csharp[Main](adding-validation-controls-to-the-datalist-s-editing-interface-cs/samples/sample2.cs)]

此步驟中，產品會嘗試送出的資料無效時，才會更新。 贏得 t 的大部分使用者都能夠回傳無效的資料，因為驗證控制項的用戶端指令碼，但其瀏覽器不支援 JavaScript，或有支援 JavaScript 的使用者停用，可以略過用戶端檢查和送出無效的資料。

> [!NOTE]
> 使用 GridView，更新資料時，精明的讀者會記得，我們不需要明確檢查`Page.IsValid`我們頁面 s 程式碼後置類別中的屬性。 這是因為 GridView 會查閱`Page.IsValid`屬性只會繼續更新才會傳回值和`True`。


## <a name="step-3-summarizing-data-entry-problems"></a>步驟 3： 彙總資料輸入問題

ASP.NET 包含五個驗證控制項，除了[ValidationSummary 控制項](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx)，用來顯示`ErrorMessage`之驗證控制項偵測到無效的資料。 此摘要的資料可以顯示為網頁上或透過強制回應的用戶端的訊息方塊的文字。 可讓增強本教學課程中要包含摘要的任何驗證問題的用戶端 messagebox s。

若要這麼做，請從 [工具箱] 拖曳至設計工具拖曳 ValidationSummary 控制項。 ValidationSummary 控制項不 t 的位置很重要，因為我們要將它設定為只會顯示摘要為 messagebox 重新。 加入控制項之後, 設定其[`ShowSummary`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx)要`False`及其[`ShowMessageBox`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx)到`True`。 此步驟中，任何驗證錯誤會摘要在用戶端 messagebox （請參閱 圖 6）。


[![驗證錯誤會摘要在用戶端 Messagebox](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image16.png)

**圖 6**： 用戶端 Messagebox 摘要說明的驗證錯誤 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image18.png))


## <a name="summary"></a>總結

在本教學課程中，我們看到如何以減少所使用驗證控制項來主動確保我們的使用者輸入再嘗試更新工作流程中使用它們都是有效的例外狀況的可能性。 ASP.NET 提供五種驗證 Web 控制項是設計用來檢查特定的 Web 控制項 s 輸入，並回報輸入的 s 有效性。 在本教學課程中我們使用這些五個控制項的兩個 RequiredFieldValidator 和 CompareValidator 以確保已提供 產品的名稱且價格必須大於或等於零的值與貨幣格式。

將驗證控制項新增到 DataList 編輯介面很簡單，只要拖放到`EditItemTemplate`從 [工具箱] 設定之少數屬性。 根據預設，驗證控制項都會自動發出用戶端驗證指令碼;它們也會在回傳時，儲存中的累計結果提供伺服器端驗證`Page.IsValid`屬性。 按一下按鈕、 LinkButton 或 ImageButton 時，請略過用戶端驗證，設定 [s] 按鈕`CausesValidation`屬性設`False`。 此外，執行任何工作之前在回傳上送出的資料，請確認`Page.IsValid`屬性會傳回`True`。

DataList 編輯教學課程的所有我們 ve 檢查到目前為止已經非常簡單的編輯介面，文字方塊中的，產品 s 名稱，而另一個價格。 不過，編輯介面，可以包含各種不同的 Web 控制項，例如 dropdownlist 進行、 行事曆、 選項按鈕、 核取方塊，等等。 在我們的下一個教學課程中，我們將探討建置使用不同的 Web 控制項的介面。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Dennis Patterson、 Ken Pespisa 和 Liz Shulok。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](handling-bll-and-dal-level-exceptions-cs.md)
> [下一頁](customizing-the-datalist-s-editing-interface-cs.md)
