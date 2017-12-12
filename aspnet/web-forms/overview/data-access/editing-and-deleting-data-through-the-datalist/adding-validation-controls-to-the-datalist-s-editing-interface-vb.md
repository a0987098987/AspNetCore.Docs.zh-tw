---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
title: "在 DataList 中加入驗證控制項的編輯介面 (VB) |Microsoft 文件"
author: rick-anderson
description: "在本教學課程中，我們會看到它是驗證控制項加入 DataList EditItemTemplate，為了提供更容易的編輯使用者 int 多麼容易..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 6b073fc6-524d-453d-be7c-0c30986de391
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: b720c7704a9c44e60ed8a9ad1479558376fb5402
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="adding-validation-controls-to-the-datalists-editing-interface-vb"></a>驗證控制項加入至 DataList 編輯介面 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_VB.exe)或[下載 PDF](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/datatutorial39vb1.pdf)

> 在本教學課程中，我們會看到將驗證控制項加入至 DataList EditItemTemplate，以便提供更容易的編輯使用者介面是多麼的輕鬆。


## <a name="introduction"></a>簡介

在 DataList 為止編輯教學課程時，在編輯介面 DataLists 並未包含任何主動式使用者輸入的驗證即使無效的使用者輸入，例如遺漏的產品名稱或負的價格會產生例外狀況。 在[前述教學課程](handling-bll-and-dal-level-exceptions-vb.md)我們檢驗如何新增例外狀況處理程式碼，以在 DataList 的`UpdateCommand`才能攔截，並依正常程序顯示任何所引發的例外狀況的相關資訊的事件處理常式。 在理想情況下，不過，編輯介面會包含驗證控制項，以防止使用者在第一次進入這類無效的資料。

本教學課程中我們會看到驗證控制項加入 DataList s 是多麼的輕鬆`EditItemTemplate`為了提供更容易的編輯使用者介面。 具體來說，本教學課程採用前述教學課程中建立的範例，並擴大要包含適當驗證的編輯介面。

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptionshandling-bll-and-dal-level-exceptions-vbmd"></a>步驟 1： 複製的範例中[BLL 和 DAL 層級例外狀況處理](handling-bll-and-dal-level-exceptions-vb.md)

在[處理 BLL-以及 DAL 層級例外狀況](handling-bll-and-dal-level-exceptions-vb.md)教學課程中我們建立一個頁面，列出名稱及兩個資料行，可以編輯 DataList 中產品的價格。 我們在此教學課程的目標是以擴大 DataList s 編輯介面，包含驗證的控制項。 特別是，我們將驗證邏輯將會：

- 需要提供產品的名稱
- 請確定輸入的價格的值是有效的貨幣格式
- 請確認輸入的價格大於或等於零，因為為負數的值`UnitPrice`值是不合法

我們來看看加強先前的範例包括驗證之前，我們必須先將複寫的範例中`ErrorHandling.aspx`頁面`EditDeleteDataList`資料夾，以在此教學課程中，頁面`UIValidation.aspx`。 若要達成我們需要透過同時複製這`ErrorHandling.aspx`s 宣告式標記和其原始碼頁。 第一次複製的宣告式標記執行下列步驟：

1. 開啟`ErrorHandling.aspx`Visual Studio 中的頁面
2. 請移至頁面 s 宣告式標記 （在頁面底部的 [來源] 按鈕按一下）
3. 複製內的文字`<asp:Content>`和`</asp:Content>`圖 1 為標記 （線條 3 到 32）。


[![複製文字內&lt;asp: Content&gt;控制項](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image1.png)

**圖 2**： 複製文字內`<asp:Content>`控制項 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image3.png))


1. 開啟`UIValidation.aspx`頁面
2. 請移至頁面 s 宣告式標記
3. 貼上文字內`<asp:Content>`控制項。

若要複製的原始碼，開啟`ErrorHandling.aspx.vb`頁面上，並將複製文字*內*`EditDeleteDataList_ErrorHandling`類別。 複製的三個事件處理常式 (`Products_EditCommand`， `Products_CancelCommand`，和`Products_UpdateCommand`) 連同`DisplayExceptionDetails`方法，但不要**不**複製類別宣告或`using`陳述式。 貼上複製的文字*內*`EditDeleteDataList_UIValidation`類別`UIValidation.aspx.vb`。

透過內容和程式碼移動之後`ErrorHandling.aspx`至`UIValidation.aspx`，花一點時間，若要測試的瀏覽器中的頁面。 您應該會看到相同的輸出及體驗中每個網頁 （請參閱圖 2） 兩個相同的功能。


[![UIValidation.aspx 頁面都是模仿 ErrorHandling.aspx 中的功能](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image4.png)

**圖 2**:`UIValidation.aspx`頁面模擬中的功能`ErrorHandling.aspx`([按一下以檢視完整大小的影像](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image6.png))


## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>步驟 2： 將驗證控制項加入至 DataList 的 EditItemTemplate

建構資料輸入表單時, 很重要，使用者輸入任何必要的欄位和所有其提供的輸入是合法、 格式正確的值。 為協助確保使用者的輸入有效值，ASP.NET 會提供五個內建的驗證控制項，設計來驗證單一輸入 Web 控制項的值：

- [RequiredFieldValidator](https://msdn.microsoft.com/en-us/library/5hbw267h(VS.80).aspx)可確保已提供的值
- [CompareValidator](https://msdn.microsoft.com/en-us/library/db330ayw(VS.80).aspx)驗證值與另一個 Web 控制項值或常數的值，或確保值的格式不合法的指定的資料型別
- [RangeValidator](https://msdn.microsoft.com/en-us/library/f70d09xt.aspx)確保值的值範圍內
- [RegularExpressionValidator](https://msdn.microsoft.com/en-US/library/eahwtc9e.aspx)驗證值，以針對[規則運算式](http://en.wikipedia.org/wiki/Regular_expression)
- [一起](https://msdn.microsoft.com/en-us/library/9eee01cx(VS.80).aspx)驗證值，以符合自訂的使用者定義的方法

如需有關這些五個控制項回頭參考[新增驗證控制項的編輯和插入介面](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)教學課程或簽出[驗證控制項 」 一節](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx)的[ASP.NET 快速入門教學課程](https://quickstarts.asp.net)。

在教學課程中我們需要使用以確保產品名稱的值已提供 RequiredFieldValidator 和 CompareValidator 以確保輸入的價格大於或等於 0 的值，而且會以正確的貨幣格式顯示。

> [!NOTE]
> 雖然 ASP.NET 1.x 具有這些相同的五個驗證控制項、 ASP.NET 2.0 已新增許多改進功能、 主要的用戶端指令碼的兩個支援除了 Internet Explorer 的瀏覽器和到頁面上的資料分割驗證控制項的功能驗證群組。 如需 2.0 中的新驗證控制項功能的詳細資訊，請參閱[將 ASP.NET 2.0 中的驗證控制項的分解](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)。


可讓啟動必要的驗證控制項加入 DataList s s `EditItemTemplate`。 透過設計工具，依序按一下 [編輯樣板] 連結，從 DataList s 智慧標籤，或透過宣告式語法，可以執行這項工作。 可讓 s 步驟透過使用 [編輯樣板] 選項，從 [設計] 檢視的程序。 選擇編輯 DataList s 後`EditItemTemplate`，將它從 [工具箱] 拖曳至範本編輯介面，加入 RequiredFieldValidator 放之後`ProductName`文字方塊。


[![[ProductName] 文字方塊中之後，新增至 EditItemTemplate 的 RequiredFieldValidator](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image7.png)

**圖 3**： 新增至 RequiredFieldValidator `EditItemTemplate After` `ProductName`文字方塊中 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image9.png))


所有驗證控制項的都運作方式驗證單一 ASP.NET Web 控制項的輸入。 因此，我們需要表示我們剛才加入的 RequiredFieldValidator 應該驗證`ProductName`文字方塊中; 這是藉由設定驗證控制項 s [ `ControlToValidate`屬性](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx)至`ID`的適當的 Web 控制項 (`ProductName`，這個執行個體中)。 接下來，設定[`ErrorMessage`屬性](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx)您必須提供的產品的名稱和[`Text`屬性](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx)至\*。 `Text`屬性值，如果提供，是在驗證失敗時，會將驗證控制項所顯示的文字。 `ErrorMessage`屬性值，這是必要的由 ValidationSummary 控制項; 如果`Text`省略屬性值，則`ErrorMessage`屬性值會顯示無效的輸入上的驗證控制項。

設定後的 RequiredFieldValidator 這三個屬性，請您的畫面看起來應該類似於圖 4。


[![設定 RequiredFieldValidator 的 ControlToValidate、 錯誤訊息，以及文字屬性](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image10.png)

**圖 4**： 設定 RequiredFieldValidator s `ControlToValidate`， `ErrorMessage`，和`Text`屬性 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image12.png))


與加入 RequiredFieldValidator `EditItemTemplate`，則所有剩下是加入必要的驗證產品 s 價格文字方塊。 因為`UnitPrice`是選擇性的編輯時記錄，我們不 t 需要加入 RequiredFieldValidator。 我們，不過，需要手動新增，確保 CompareValidator `UnitPrice`，提供，如果已正確格式化為貨幣，且大於或等於 0。

新增到 CompareValidator`EditItemTemplate`並設定其`ControlToValidate`屬性`UnitPrice`、 其`ErrorMessage`價格屬性必須是大於或等於零，而且不能包含貨幣符號，且其`Text`屬性\*. 表示`UnitPrice`值必須是大於或等於 0，且設定 CompareValidator s [ `Operator`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx)至`GreaterThanEqual`、 其[`ValueToCompare`屬性](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx)為 0，並其[`Type`屬性](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx)至`Currency`。

新增這些兩個驗證控制項中，DataList s 後`EditItemTemplate`s 宣告式語法看起來應該類似下列：


[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

進行這些變更之後，請在瀏覽器中開啟頁面。 如果您嘗試以省略名稱，或輸入無效的價格值編輯產品時，則會在文字方塊旁邊出現星號。 如圖 5 所示，包含貨幣符號，例如 $19.95 的價格值會被視為無效。 CompareValidator s `Currency` `Type`允許 （例如逗號或句號，根據文化特性設定而定） 的數字分隔符號的開頭的加號或減號，但是沒有*不*允許貨幣符號。 這種行為可能 perplex 使用者編輯介面目前轉譯`UnitPrice`使用貨幣格式。


[![具有無效的輸入的文字方塊旁邊出現星號](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image13.png)

**圖 5**: 星號會顯示下一步 以無效的輸入的文字方塊 ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image15.png))


雖然做為驗證運作的方式-就是，使用者必須手動編輯記錄，這不是可接受時，移除的貨幣符號。 此外，如果無效輸入中編輯介面都沒有更新或 [取消] 按鈕時，按下時，將會叫用回傳。 在理想情況下，[取消] 按鈕就會回到 DataList 其預先編輯的狀態，不論使用者的輸入的有效性。 此外，我們需要確保頁面的資料是否有效，然後再更新產品中的資訊 DataList 的`UpdateCommand`做為用戶端邏輯可以由使用者的瀏覽器不支援 JavaScript，或已略過驗證控制項的事件處理常式，停用的支援。

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>移除 EditItemTemplate 的 UnitPrice 文字方塊中的貨幣符號

當使用 CompareValidator 的`Currency``Type`，正在驗證的輸入不能包含任何貨幣符號。 這類符號存在，會導致將標示為無效的輸入 CompareValidator。 不過，我們編輯介面目前包含中的貨幣符號`UnitPrice`文字方塊中，這表示使用者必須明確地移除之前將其變更的貨幣符號。 若要補救這種情況中，我們有三個選項：

1. 設定`EditItemTemplate`以便`UnitPrice`TextBox 值不格式化為貨幣。
2. 可讓使用者藉由移除 CompareValidator 取代會檢查有格式正確的貨幣值 RegularExpressionValidator 輸入貨幣符號。 此處的挑戰是要驗證之貨幣值的規則運算式不像是那樣 CompareValidator 和需要撰寫程式碼，如果我們想要納入的文化特性設定。
3. 完全移除驗證控制項，而且依賴 GridView s 中的自訂伺服器端驗證邏輯`RowUpdating`事件處理常式。

可讓 s 隨附此教學課程中的選項 1。 目前`UnitPrice`格式化為貨幣值，因為在文字方塊中的資料繫結運算式`EditItemTemplate`: `<%# Eval("UnitPrice", "{0:c}") %>`。 變更`Eval`陳述式來`Eval("UnitPrice", "{0:n2}")`，它將結果格式化為兩位數的有效位數的數字。 這可以透過宣告式語法直接或按一下 [編輯資料繫結] 連結，從完成`UnitPrice`DataList s 中的 TextBox `EditItemTemplate`。

這項變更，編輯介面中的格式化的價格包含做為群組分隔符號逗號和句號當做十進位分隔符號，但離開關閉貨幣符號。

> [!NOTE]
> 當從可編輯介面移除貨幣格式，我發現很有幫助將貨幣符號為外部文字方塊的文字。 這可做為提示使用者，他們不需要提供貨幣符號。


## <a name="fixing-the-cancel-button"></a>修正 [取消] 按鈕

根據預設，驗證 Web 控制項發出 JavaScript 用戶端上執行驗證。 按一下按鈕、 LinkButton 或 ImageButton 時，會發生回傳之前，先檢查驗證控制項在頁面上用戶端。 如果沒有任何無效的資料，則會取消回傳。 某些按鈕，不過，資料的有效性可能多少都沒關係。在這種情況下，因為無效的資料而取消回傳是來進行騷擾。

[取消] 按鈕是這類範例。 想像一下，使用者輸入無效的資料，例如省略 s 產品名稱，然後決定她不想要在所有儲存產品並點擊 [取消] 按鈕。 目前，[取消] 按鈕會觸發驗證控制項在頁面上，執行中的報表產品名稱遺漏，防止回傳。 我們的使用者可以輸入一些文字到`ProductName`文字方塊中，只是為了取消編輯程序。

幸運的是，按鈕、 LinkButton 和 ImageButton 已[`CausesValidation`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.button.causesvalidation.aspx)，可以指出是否按一下按鈕應該起始驗證邏輯 (預設值為`True`)。 設定 [取消] 按鈕 s`CausesValidation`屬性`False`。

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>確保輸入都有效 UpdateCommand 的事件處理常式

因為驗證控制項中，所發出的用戶端指令碼如果使用者輸入無效的輸入驗證控制項取消按鈕，LinkButton，由起始任何回傳或 ImageButton 控制其`CausesValidation`屬性`True`(預設值）。 不過，如果使用者造訪用的瀏覽器或其中一個已停用其 JavaScript 支援時，不會執行的用戶端驗證檢查。

ASP.NET 驗證控制項的所有重複回傳時立即其驗證邏輯，並報告整體透過頁輸入的有效性[`Page.IsValid`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.page.isvalid.aspx)。 不過，未中斷或停止任何方式為基礎的值頁面流程`Page.IsValid`。 身為開發人員，負責我們確定`Page.IsValid`屬性的值為`True`之前繼續執行程式碼假設有效輸入資料。

如果使用者已停用 JavaScript，造訪我們的頁面，編輯產品，進入價格的值太昂貴，並按一下 [更新] 按鈕，將會略過用戶端驗證，並將發生回傳。 在回傳時，ASP.NET 頁面 s`UpdateCommand`執行事件處理常式，並嘗試剖析太時引發例外狀況，是相當費時`Decimal`。 因為我們有例外狀況處理，會依正常程序，處理這類例外狀況，不過就可以防止無效的資料在由只能繼續使用在第一次機會通過`UpdateCommand`事件處理常式如果`Page.IsValid`的值為`True`。

開頭加入下列程式碼`UpdateCommand`事件處理常式之前，立即`Try`區塊：


[!code-vb[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample2.vb)]

與此新增功能，產品會嘗試送出的資料無效時，才會更新。 贏了 t 的大部分使用者都可以回傳無效的資料，因為驗證控制項的用戶端指令碼，但使用者的瀏覽器不支援 JavaScript，或有支援的 JavaScript 停用，可以略過用戶端檢查和送出無效的資料。

> [!NOTE]
> GridView 中使用更新資料時，精明讀取器可以回想，我們 professionals t 需要明確檢查`Page.IsValid`我們頁面 s 程式碼後置類別中的屬性。 這是因為 GridView 參照`Page.IsValid`屬性，並繼續更新才會傳回值`True`。


## <a name="step-3-summarizing-data-entry-problems"></a>步驟 3： 摘要資料輸入問題

除了五個驗證控制項，包含 ASP.NET [ValidationSummary 控制項](https://msdn.microsoft.com/en-US/library/f9h59855(VS.80).aspx)，用來顯示`ErrorMessage`之偵測到無效的資料，這些驗證控制項。 此摘要的資料可以顯示為網頁上或透過強制回應，用戶端的訊息方塊的文字。 可讓 s 強化這個教學課程，包括用戶端 messagebox 摘要的任何驗證問題。

若要達成此目的，拖曳到 ValidationSummary 控制項從 [工具箱] 拖曳至設計工具。 ValidationSummary 控制項規定 t 位置真的很重要，因為我們重新打算將它設定為只顯示摘要為 messagebox。 加入控制項之後, 設定其[`ShowSummary`屬性](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx)至`False`及其[`ShowMessageBox`屬性](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx)至`True`。 此新增功能，使用任何驗證錯誤會摘要在用戶端 messagebox （請參閱圖 6）。


[![驗證錯誤摘要說明在用戶端訊息方塊](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image16.png)

**圖 6**: 驗證錯誤摘要說明在用戶端 Messagebox ([按一下以檢視完整大小的影像](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image18.png))


## <a name="summary"></a>總結

在此教學課程中我們可了解如何使用以主動確保我們的使用者輸入之前嘗試更新的工作流程中使用它們都是有效的驗證控制項來減少例外狀況的可能性。 ASP.NET 提供五個，設計來檢查特定 Web 驗證 Web 控制項控制 s 輸入，並回報輸入的 s 有效性。 在本教學課程中我們使用這些五個控制項的兩個 RequiredFieldValidator 和 CompareValidator 以確保提供的產品的名稱且價格必須大於或等於零的值與貨幣格式。

驗證控制項加入編輯介面 DataList s 很簡單，拖曳到`EditItemTemplate`從 [工具箱] 會設定之少數屬性。 根據預設，驗證控制項自動發出用戶端驗證指令碼。它們也提供伺服器端驗證在回傳時，儲存中的累計結果`Page.IsValid`屬性。 如果要略過用戶端驗證 按鈕、 LinkButton 或 imagebutton 已按下時，設定按鈕 s`CausesValidation`屬性`False`。 此外，然後再執行任何工作在回傳時送出的資料，請確定`Page.IsValid`屬性會傳回`True`。

所有的編輯教學課程 DataList 我們檢查到目前為止我們有非常簡單的編輯介面，產品 s 名稱文字方塊中，而另一個的價格。 編輯介面，不過，可以包含不同的 Web 控制項，例如 DropDownLists、 行事曆、 選項按鈕、 核取方塊，等等的混合。 在我們的下一個教學課程中我們將探討建置使用不同的 Web 控制項的介面。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已 Dennis Patterson、 Ken Pespisa 和 Liz Shulok。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一頁](handling-bll-and-dal-level-exceptions-vb.md)
[下一頁](customizing-the-datalist-s-editing-interface-vb.md)
