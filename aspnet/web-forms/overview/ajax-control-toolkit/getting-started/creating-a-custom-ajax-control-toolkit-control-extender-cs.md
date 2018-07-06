---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
title: 建立自訂的 AJAX 控制項 Toolkit 控制項擴充項 (C#) |Microsoft Docs
author: microsoft
description: 自訂擴充項可讓您自訂和擴充 ASP.NET 控制項的功能，而不需要建立新的類別。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 96b56eca-a892-45a4-96b4-67e61178650a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 0dd243ec1709324174ff48b52e7dfb5185d3aaa2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390949"
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-c"></a>建立自訂的 AJAX Control Toolkit 控制項擴充項 (C#)
====================
by [Microsoft](https://github.com/microsoft)

> 自訂擴充項可讓您自訂和擴充 ASP.NET 控制項的功能，而不需要建立新的類別。


在本教學課程中，您將了解如何建立自訂的 AJAX Control Toolkit 控制項擴充項。 我們會建立一項簡單、 但變更按鈕的狀態從停用，以啟用在文字方塊中輸入文字時的實用、 新的擴充項。 閱讀本教學課程中之後, 您將能夠擴充 ASP.NET AJAX 工具組，使用您自己的控制項擴充項。

您可以建立使用 Visual Studio 或 Visual Web Developer 的自訂控制項擴充項 （請確定您有最新版的 Visual Web Developer）。

## <a name="overview-of-the-disabledbutton-extender"></a>DisabledButton 擴充項的概觀

我們新的控制項擴充項是名為 DisabledButton 擴充項。 此擴充項將會有三個屬性：

- TargetControlID-擴充控制項的文字方塊。
- TargetButtonIID-已停用或啟用的按鈕。
- DisabledText-最初顯示在按鈕的文字。 當您開始輸入時，按鈕會顯示按鈕文字屬性的值。

您連接至文字方塊和按鈕控制項 DisabledButton 擴充項。 輸入任何文字之前，會停用按鈕和文字方塊和按鈕看起來像這樣：


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image1.png)

([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))


在您開始輸入文字之後，已啟用 按鈕和文字方塊和按鈕看起來像這樣：


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image4.png)

([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))


若要建立我們控制擴充項，我們需要建立下列三個檔案：

- DisabledButtonExtender.cs-這個檔案是伺服器端控制項類別，會管理建立您的擴充項，並可讓您在設計階段設定的屬性。 它也會定義可以設定在 extender 的屬性。 這些屬性可透過程式碼，並在設計階段，而且符合 DisableButtonBehavior.js 檔案中定義的屬性。
- DisabledButtonBehavior.js-這個檔案是會加入所有的用戶端指令碼邏輯的位置。
- DisabledButtonDesigner.cs-此類別可讓設計階段功能。 如果您想要控制擴充項，才能正確地使用 Visual Studio/Visual Web Developer 設計工具，您會需要此類別。

因此控制項擴充項所組成的伺服器端控制項、 用戶端行為，以及伺服器端的設計工具類別。 您了解如何建立下列各節中的所有三個檔案。

## <a name="creating-the-custom-extender-website-and-project"></a>建立自訂擴充項網站和專案

第一個步驟是在 Visual Studio/Visual Web Developer 中建立類別庫專案和網站。 我們將在類別庫專案中建立自訂的擴充性和網站中測試自訂的擴充性。

可讓網站啟動的 s。 請遵循下列步驟來建立網站：

1. 選取功能表選項**檔案中，新的網站**。
2. 選取  **ASP.NET 網站**範本。
3. 命名新的網站*Website1*。
4. 按一下 [**確定**] 按鈕。

接下來，我們需要建立類別庫專案將包含控制項擴充項的程式碼：

1. 選取功能表選項**檔案中，加入新的專案**。
2. 選取 **類別庫**範本。
3. 命名新的類別程式庫同名**CustomExtenders**。
4. 按一下 [**確定**] 按鈕。

完成這些步驟之後，您的方案總管 視窗看起來應該像圖 1。


[![與網站和類別庫專案的方案](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)

**圖 01**： 網站和類別程式庫專案的方案 ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))


接下來，您需要新增所有必要的組件的參考類別庫專案：

1. CustomExtenders 專案上按一下滑鼠右鍵，然後選取功能表選項**加入參考**。
2. 選取 [.NET] 索引標籤。
3. 加入下列組件的參考：

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. 選取 [瀏覽] 索引標籤。
5. 新增 AjaxControlToolkit.dll 組件的參考。 這個組件位於您用來下載 AJAX Control Toolkit 中的資料夾。

完成這些步驟之後，您的類別庫專案參考資料夾看起來應該像圖 2。


[![必要的參考，[參考] 資料夾](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)

**圖 02**： 必要的參考，[參考] 資料夾 ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))


## <a name="creating-the-custom-control-extender"></a>建立自訂控制項擴充項

既然我們已經有類別庫，我們可以開始建置我們的擴充項控制項。 可讓開始使用最基本的自訂擴充項控制項類別 （請參閱列表 1） s。

**列表 1-MyCustomExtender.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample1.cs)]

有幾件事，您會注意到關於清單 1 控制項擴充項類別。 首先，請注意，類別可以繼承自基底 ExtenderControlBase 類別。 所有的 AJAX Control Toolkit 擴充項控制項是衍生自這個基底類別。 比方說，基底類別包含必要的屬性，每個控制項擴充項的 TargetID 屬性。

接下來，要請您注意的是類別，包含用戶端指令碼相關的下列兩個屬性：

- WebResource-會使要做為內嵌資源的組件中包含的檔案。
- ClientScriptResource-會導致指令碼資源要擷取的組件。

WebResource 屬性用來將 MyControlBehavior.js JavaScript 檔案內嵌至組件中，自訂的擴充性在編譯時。 ClientScriptResource 屬性用來在網頁中使用自訂的擴充性時，從組件擷取 MyControlBehavior.js 指令碼。


為了 WebResource 與 ClientScriptResource 屬性運作，您必須編譯 JavaScript 檔案做為內嵌資源。 在 [方案總管] 視窗中選取的檔案、 開啟屬性工作表及值指派*內嵌的資源*要**建置動作**屬性。


請注意，控制項擴充項也會包含 TargetControlType 屬性。 這個屬性用來指定擴充控制項擴充項的控制項型別。 列表 1，如果控制項擴充項用來擴充的文字方塊中。

最後，要請您注意的是自訂的擴充項包含名為 MyProperty 的屬性。 屬性標記著 ExtenderControlProperty 屬性。 GetPropertyValue() 和 SetPropertyValue() 方法用來從伺服器端控制項擴充項的屬性值傳遞到用戶端的行為。

可讓繼續及實作我們 DisabledButton 擴充項的程式碼。 可以在 列表 2 中找到這個擴充項的程式碼。

**列表 2-DisabledButtonExtender.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample2.cs)]

DisabledButton 擴充項列表 2 中的會有兩個名為 TargetButtonID 和 DisabledText 的屬性。 套用至 TargetButtonID 屬性 IDReferenceProperty 會防止您將按鈕控制項的 ID 以外的任何項目指派給這個屬性。

WebResource 與 ClientScriptResource 屬性建立關聯位於名為 DisabledButtonBehavior.js 這個擴充項檔案中的用戶端行為。 我們會討論這個 JavaScript 檔案下, 一節。

## <a name="creating-the-custom-extender-behavior"></a>建立自訂的擴充項行為

控制項擴充項的用戶端端元件稱為 「 行為 」。 停用和啟用按鈕的實際邏輯都包含在 DisabledButton 行為。 在 列表 3 包含行為的 JavaScript 程式碼。

**列表 3-DisabledButton.js**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample3.js)]

在 列表 3 中的 JavaScript 檔案包含名為 DisabledButtonBehavior 的用戶端類別。 像其伺服器端對應項中，此類別包含兩個屬性，名為 TargetButtonID 和取得您可以使用來存取 DisabledText\_TargetButtonID/set\_TargetButtonID 並取得\_DisabledText/set\_DisabledText。

Initialize （） 方法會將 keyup 事件處理常式關聯的目標項目行為。 每當您在這項行為，與相關文字方塊中輸入字母的 keyup 處理常式執行。 Keyup 處理常式會啟用或停用根據與行為相關聯的文字方塊是否包含任何文字的按鈕。

請記住，您必須編譯 JavaScript 檔案做為內嵌資源的列表 3。 在 [方案總管] 視窗中選取的檔案、 開啟屬性工作表及值指派*內嵌資源*要**建置動作**屬性 （請參閱 [圖 3]）。 Visual Studio 和 Visual Web Developer 中使用此選項。


[![新增 JavaScript 檔案做為內嵌資源](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)

**圖 03**： 加入 JavaScript 檔案做為內嵌資源 ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))


## <a name="creating-the-custom-extender-designer"></a>建立自訂的擴充性設計工具

沒有一個我們需要建立完成我們的擴充項的最後一個類別。 我們需要在 列表 4 中建立設計工具類別。 這個類別，才能讓正常運作，Visual Studio/Visual Web Developer 設計工具與擴充項。

**列表 4-DisabledButtonDesigner.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample4.cs)]

列表 4 中的設計工具關聯 DisabledButton 擴充項與設計工具屬性上。您需要將設計工具屬性套用至 DisabledButtonExtender 類別就像這樣：

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample5.cs)]

## <a name="using-the-custom-extender"></a>使用自訂的擴充性

既然我們已經完成建立 DisabledButton 控制項擴充項，就可以在我們的 ASP.NET 網站中使用它。 首先，我們需要將自訂的擴充性加入至工具箱。 請依照下列步驟：

1. 按兩下 [方案總管] 視窗中的頁面，以開啟 ASP.NET 網頁。
2. 以滑鼠右鍵按一下 [工具箱]，然後選取功能表選項**選擇項目**。
3. 在 [選擇工具箱項目] 對話方塊中，瀏覽至 CustomExtenders.dll 組件。
4. 按一下 **確定**按鈕以關閉對話方塊。

完成這些步驟之後，DisabledButton 控制項擴充項應該會出現在工具箱] 中 （請參閱 [圖 4）。


[![在 [工具箱] 的 DisabledButton](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)

**圖 04**： 在 [工具箱] 的 DisabledButton ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))


接下來，我們需要建立新的 ASP.NET 頁面。 請依照下列步驟：

1. 建立名為 ShowDisabledButton.aspx 的新 ASP.NET 頁面。
2. 將 ScriptManager 拖曳至頁面。
3. 將 TextBox 控制項拖曳至頁面。
4. 將按鈕控制項拖曳至頁面。
5. 在 屬性 視窗中，將按鈕的 ID 屬性變更的值<em>btnSave</em>和 文字屬性設為值*儲存\**。
  

我們建立使用標準 ASP.NET 文字方塊和按鈕控制項的頁面。

接下來，我們需要擴充 DisabledButton 擴充項與 TextBox 控制項：

1. 選取 **加入擴充項**工作選項來開啟 擴充性精靈 對話方塊 （請參閱 圖 5）。 請注意對話方塊包含我們自訂的 DisabledButton 擴充性。
2. 選取 DisabledButton 擴充項，然後按一下**確定** 按鈕。


[![擴充項精靈對話方塊](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)

**圖 05**: [擴充性精靈] 對話方塊 ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))


最後，我們可以設定 DisabledButton 擴充項的屬性。 您可以修改 TextBox 控制項的屬性來修改 DisabledButton 擴充項的屬性：

1. 在設計工具選取文字方塊。
2. 在 屬性 視窗中，展開 Extender 節點 （請參閱 圖 6）。
3. 將值指派*儲存*DisabledText 屬性和值*btnSave* TargetButtonID 屬性。


[![設定擴充項屬性](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)

**圖 06**： 設定擴充項屬性 ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))


當您執行的頁面 （藉由按下 F5） 時，一開始會停用的按鈕控制項。 一旦您開始在文字方塊中輸入文字，控制項的按鈕會啟用 （請參閱 圖 7）。


[![在 動作 DisabledButton 擴充項](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)

**圖 07**: DisabledButton 作用中的擴充項 ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))


## <a name="summary"></a>總結

本教學課程的目標是要說明如何擴充 AJAX Control Toolkit 之自訂擴充項控制項。 在本教學課程中，我們會建立簡單的 DisabledButton 控制項擴充項。 我們透過建立 DisabledButtonExtender 類別、 DisabledButtonBehavior JavaScript 行為和 DisabledButtonDesigner 類別實作這個擴充項。 每當您建立自訂控制項擴充項時，您就會遵循一組類似的步驟。

> [!div class="step-by-step"]
> [上一頁](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
> [下一頁](get-started-with-the-ajax-control-toolkit-vb.md)
