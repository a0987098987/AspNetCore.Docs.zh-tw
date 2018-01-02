---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
title: "建立自訂的 AJAX 控制項 Toolkit 控制項擴充項 (VB) |Microsoft 文件"
author: microsoft
description: "自訂擴充項可讓您自訂和擴充 ASP.NET 控制項的功能，而不必建立新的類別。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 18b29834-c991-4e0c-b533-44d358fbfc9c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 3e8fceb3c7570aa1bf085c8e1037736254e74ef9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-vb"></a>建立自訂的 AJAX 控制項 Toolkit 控制項擴充項 (VB)
====================
由[Microsoft](https://github.com/microsoft)

> 自訂擴充項可讓您自訂和擴充 ASP.NET 控制項的功能，而不必建立新的類別。


在本教學課程中，您會學習如何建立自訂的 AJAX Control Toolkit 控制項擴充項。 我們會建立簡單，但很有用，新的按鈕狀態變更從停用，以您在文字方塊中輸入文字時，啟用的擴充項。 閱讀本教學課程之後, 您可以擴充 ASP.NET AJAX 工具組，使用您自己的控制項擴充項。

您可以建立使用 Visual Studio 或 Visual Web Developer 中的自訂控制項擴充項 （請確定您有最新版本的 Visual Web Developer）。

## <a name="overview-of-the-disabledbutton-extender"></a>DisabledButton Extender 的概觀

我們新的控制項擴充項名為 DisabledButton 擴充項。 此擴充項會有三個屬性：

- TargetControlID-擴充控制項的文字方塊。
- TargetButtonIID-已停用或啟用的按鈕。
- DisabledText-最初顯示在按鈕的文字。 當您開始輸入時，按鈕會顯示按鈕文字屬性的值。

您連接至文字方塊和按鈕控制項 DisabledButton extender。 您輸入的任何文字之前，會停用按鈕和文字方塊和按鈕看起來像這樣：


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image1.png)

([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))


您開始輸入文字後，已啟用 按鈕和文字方塊和按鈕看起來像這樣：


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image4.png)

([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))


若要建立我們控制項擴充項，我們需要建立下列三個檔案：

- DisabledButtonExtender.vb-這個檔案是伺服器端控制項類別會管理建立您的擴充性，並可讓您在設計階段設定的屬性。 它也會定義可以在 extender 設定的屬性。 這些屬性是透過程式碼和設計階段存取，且比對 DisableButtonBehavior.js 檔案中定義的屬性。
- DisabledButtonBehavior.js-這個檔案是供您用來新增所有您的用戶端指令碼邏輯。
- DisabledButtonDesigner.vb-此類別可讓設計階段功能。 如果您想控制項擴充項 Visual Studio/Visual Web Developer 設計工具中正常運作，您會需要此類別。

因此控制項擴充項組成的伺服器端控制項、 用戶端行為，以及伺服器端的設計工具類別。 您了解如何在下列各節中建立這些檔案的所有三個。

## <a name="creating-the-custom-extender-website-and-project"></a>建立自訂 Extender 網站和專案

第一個步驟是在 Visual Studio/Visual Web Developer 中建立類別庫專案和網站。 我們 ll 類別庫專案中建立自訂的擴充性，並在網站中測試自訂的擴充性。

可讓網站啟動 s。 請遵循下列步驟來建立網站：

1. 選取功能表選項**檔案、 新的網站**。
2. 選取**ASP.NET 網站**範本。
3. 命名新的網站*Website1*。
4. 按一下**確定** 按鈕。

接下來，我們需要建立類別庫專案將包含控制項擴充項的程式碼：

1. 選取功能表選項**檔案，加入新的專案**。
2. 選取**類別庫**範本。
3. 命名新的類別庫的名稱**CustomExtenders**。
4. 按一下**確定** 按鈕。

完成這些步驟之後，您的方案總管 視窗看起來應該像圖 1。


[![網站和類別庫專案與方案](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)

**圖 01**： 與網站和類別庫專案的方案 ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))


接下來，您要新增的所有必要的組件參考類別庫專案：

1. CustomExtenders 專案上按一下滑鼠右鍵，然後選取功能表選項**加入參考**。
2. 選取 [.NET] 索引標籤。
3. 加入下列組件的參考：

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. 選取 [瀏覽] 索引標籤。
5. 新增 AjaxControlToolkit.dll 組件的參考。 這個組件位於您下載 AJAX Control Toolkit 所在的資料夾。

您可以確認您已用滑鼠右鍵按一下您的專案、 選取屬性，然後按一下 [參考] 索引標籤新增所有正確的參考 （請參閱圖 2）。


[![必要的參考，[參考] 資料夾](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)

**圖 02**： 所需的參考，[參考] 資料夾 ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))


## <a name="creating-the-custom-control-extender"></a>建立自訂控制項擴充項

現在我們有我們類別程式庫，我們可以開始建置我們擴充項控制項。 可讓 s 以最基本自訂的擴充項控制項類別 （請參閱清單 1） 的開頭。

**列出 1-MyCustomExtender.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample1.vb)]

有幾件事，您會注意到控制項擴充項類別清單 1 中的相關。 首先，請注意，類別可以繼承自基底 ExtenderControlBase 類別。 所有的 AJAX Control Toolkit 擴充項控制項衍生自這個基底類別。 例如，基底類別包含 TargetID 屬性是必要的每個控制項擴充項屬性。

接下來，請注意此類別包括下列用戶端指令碼相關的兩個屬性：

- WebResource-會使要做為內嵌資源的組件中包含的檔案。
- ClientScriptResource-會使指令碼資源要擷取的組件。

WebResource 屬性用來編譯自訂 extender 時 MyControlBehavior.js JavaScript 檔案嵌入組件。 ClientScriptResource 屬性用來在網頁中使用自訂的擴充性時，從組件擷取 MyControlBehavior.js 指令碼。


為了讓 WebResource 和 ClientScriptResource 屬性運作，您必須做為內嵌資源編譯 JavaScript 檔案。 在 [方案總管] 視窗中選取檔案、 開啟屬性工作表和指派值*內嵌資源*至**建置動作**屬性。


請注意控制項擴充項也包含 TargetControlType 屬性。 這個屬性用來指定所擴充的控制項擴充項控制項類型。 在程式碼範例 1，控制項擴充項用於擴充的文字方塊。

最後，請注意，自訂的擴充性包含名為 MyProperty 的屬性。 屬性會標示 ExtenderControlProperty 屬性。 GetPropertyValue() 和 SetPropertyValue() 方法可用來從伺服器端控制項擴充項的屬性值傳遞到用戶端的行為。

可讓 s 請繼續並實作我們 DisabledButton 擴充項的程式碼。 此程式碼可以列表 2 中找到。

**列出 2-DisabledButtonExtender.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample2.vb)]

DisabledButton 擴充項清單 2 中的有兩個名為 TargetButtonID 和 DisabledText 的屬性。 套用至 TargetButtonID 屬性 IDReferenceProperty 會防止您將按鈕控制項的識別碼以外的任何項目指派給這個屬性。

WebResource 和 ClientScriptResource 屬性產生關聯名 DisabledButtonBehavior.js 為此擴充項的檔案中的用戶端行為。 我們將討論這個 JavaScript 檔案，在下一節。

## <a name="creating-the-custom-extender-behavior"></a>建立自訂的擴充性行為

控制項擴充項的用戶端元件會呼叫行為。 停用和啟用按鈕的實際邏輯會包含在 DisabledButton 行為。 列出的 3 包含行為的 JavaScript 程式碼。

**列出 3-DisabledButton.js**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample3.js)]

範例 3 中的 JavaScript 檔案包含名為 DisabledButtonBehavior 的用戶端類別。 這個類別，例如其伺服器端兩個，包含名為 TargetButtonID 的兩個屬性，並可以使用存取 DisabledText 取得\_TargetButtonID/set\_TargetButtonID 並取得\_DisabledText/set\_DisabledText。

Initialize （） 方法會將 keyup 事件處理常式關聯之行為的目標項目。 每當您在這種行為，與相關聯的文字方塊中輸入的字母 keyup 處理常式會執行。 Keyup 處理常式會啟用或停用根據與行為相關聯的文字方塊中是否包含任何文字按鈕。

請記住，您必須編譯 JavaScript 檔案中列出的 3，做為內嵌資源。 在 [方案總管] 視窗中選取檔案、 開啟屬性工作表和指派值*內嵌資源*至**建置動作**屬性 （請參閱圖 3）。 在 Visual Studio 和 Visual Web Developer 中使用此選項。


[![若要加入 JavaScript 檔案做為內嵌資源](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)

**圖 03**： 加入 JavaScript 檔案做為內嵌資源 ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))


## <a name="creating-the-custom-extender-designer"></a>建立自訂的擴充性設計工具

沒有一個我們需要建立完成我們 extender 的最後一個類別。 我們需要在列表 4 中建立設計工具類別。 這個類別才能讓正常運作，與 Visual Studio/Visual Web Developer 設計工具的擴充項。

**列出 4-DisabledButtonDesigner.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample4.vb)]

您可以在設計工具中列出的 4 關聯 DisabledButton extender 使用 Designer 屬性。您需要將設計工具屬性套用至 DisabledButtonExtender 類別就像這樣：

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample5.vb)]

## <a name="using-the-custom-extender"></a>使用自訂的擴充性

既然我們已經完成建立 DisabledButton 控制項擴充項，就可以使用我們的 ASP.NET 網站中開始。 首先，我們需要 [工具箱] 加入自訂的擴充性。 請依照下列步驟：

1. 按兩下 [方案總管] 視窗中的頁面中開啟 ASP.NET 頁面。
2. 以滑鼠右鍵按一下 [工具箱]，然後選取功能表選項**選擇項目**。
3. 在 [選擇工具箱項目] 對話方塊中，瀏覽至 CustomExtenders.dll 組件。
4. 按一下**確定**按鈕以關閉對話方塊。

完成這些步驟之後，DisabledButton 控制項擴充項應該會出現在工具箱中 （請參閱圖 4）。


[![在 [工具箱] 的 DisabledButton](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)

**圖 04**： 在 [工具箱] 的 DisabledButton ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))


接下來，我們需要建立新的 ASP.NET 網頁。 請依照下列步驟：

1. 建立名為 ShowDisabledButton.aspx 的新 ASP.NET 頁面。
2. 將 ScriptManager 拖曳至頁面。
3. 將文字方塊控制項拖曳至頁面。
4. 將按鈕控制項拖曳至頁面。
5. 在 [屬性] 視窗中變更的按鈕 ID 屬性的值*btnSave*和文字屬性的值*儲存\**。
  

我們建立標準 ASP.NET 文字方塊和按鈕控制項的頁面。

接下來，我們必須擴充 DisabledButton extender 與文字方塊控制項：

1. 選取**加入擴充項**工作以開啟 [擴充性精靈] 對話方塊的選項 （請參閱圖 5）。 請注意，對話方塊會包含自訂的 DisabledButton 擴充性。
2. 選取 DisabledButton 擴充性，然後按一下**確定** 按鈕。


[![[擴充性精靈] 對話方塊](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)

**圖 05**: Extender 精靈對話方塊 ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))


最後，我們可以設定 DisabledButton 擴充項的屬性。 您可以藉由修改文字方塊控制項的內容修改 DisabledButton 擴充項的屬性：

1. 在設計工具選取文字方塊。
2. 在 屬性 視窗中，展開 Extender 節點 （請參閱圖 6）。
3. 將值指派*儲存*DisabledText 屬性和值*btnSave* TargetButtonID 屬性。


[![設定擴充項屬性](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)

**圖 06**： 設定擴充項屬性 ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))


當您執行的頁面 （藉由按下 F5） 時，一開始會停用按鈕控制項。 當您啟動的文字方塊中輸入文字時，控制項是這個按鈕會啟用 （請參閱圖 7）。


[![DisabledButton 擴充項中的動作](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)

**圖 07**: DisabledButton extender 中動作 ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))


## <a name="summary"></a>總結

本教學課程的目的是要說明如何擴充 AJAX Control Toolkit，含有自訂的擴充項控制項。 在本教學課程中，我們會建立簡單的 DisabledButton 控制項擴充項。 我們透過建立 DisabledButtonExtender 類別、 DisabledButtonBehavior JavaScript 行為，以及 DisabledButtonDesigner 類別實作此擴充項。 每當您建立自訂控制項擴充項時，您可以遵循一組類似的步驟。

>[!div class="step-by-step"]
[上一步](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
