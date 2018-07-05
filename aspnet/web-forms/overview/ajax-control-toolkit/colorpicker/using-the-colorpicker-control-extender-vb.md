---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
title: 使用 ColorPicker 控制項擴充項 (VB) |Microsoft Docs
author: microsoft
description: ColorPicker 是提供 UI 中的用戶端色彩挑選功能，在快顯視窗控制項中的 ASP.NET AJAX 擴充項。 可以將它附加至任何 ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 577ae07b-a872-4818-a804-bca489b40ad0
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: cad012dd1ce93714ecb127bf3543d5c65803aba9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374175"
---
<a name="using-the-colorpicker-control-extender-vb"></a>使用 ColorPicker 控制項擴充項 (VB)
====================
by [Microsoft](https://github.com/microsoft)

> ColorPicker 是提供 UI 中的用戶端色彩挑選功能，在快顯視窗控制項中的 ASP.NET AJAX 擴充項。 可以將它附加至任何 ASP.NET TextBox 控制項。 它。


本教學課程的目標是要說明如何使用 AJAX 控制項工具組 ColorPicker 控制項擴充項。 ColorPicker 控制項擴充項會顯示快顯對話方塊中，可讓您選取色彩。 ColorPicker 適合，每當您想要提供直覺式使用者介面來選擇色彩的使用者。

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>擴充 ColorPicker 控制項擴充項與 TextBox 控制項

例如，假設您想要建立網站，可讓訪客建立自訂的名片。 訪客可以輸入文字的名片，挑選色彩。 列表 1 中的 [ASP.NET] 頁面包含兩個名為 txtCardText 和 txtCardColor 的 TextBox 控制項。 當您提交表單時，會顯示選取的值 （請參閱 圖 1）。


[![簡單的表單建立名片](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)

**圖 01**： 建立名片的簡單表單 ([按一下以檢視完整大小的影像](using-the-colorpicker-control-extender-vb/_static/image2.png))


**列表 1-CreateCard.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample1.aspx)]

表單的程式碼範例 1 的運作方式，但它不提供絕佳的使用者體驗。 使用者必須在文字方塊中輸入一種色彩。 如果使用者想要的特製化的色彩-比方說，只是右陰影的尖峰綠-然後使用者必須找出 HTML 色彩代碼，而不需任何協助。

若要建立較佳使用者體驗，您可以使用 ColorPicker 控制項擴充項。 當您將焦點移至 TextBox 控制項，ColorPicker 顯示色彩對話方塊 （請參閱 圖 2）。


[![ColorPicker 控制項擴充項](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)

**圖 02**: ColorPicker 控制項擴充項 ([按一下以檢視完整大小的影像](using-the-colorpicker-control-extender-vb/_static/image4.png))


您需要完成 ColorPicker 控制項擴充項搭配列表 1 中的表單的兩個步驟：

1. ScriptManager 控制項加入頁面
2. ColorPicker 控制項擴充項新增至頁面

您可以使用 ColorPicker 之前，必須以您的頁面加入 ScriptManager。 開啟伺服器端權限低於將 ScriptManager 的好地方&lt;表單&gt;標記。 您可以拖曳到頁面的 ScriptManager，從工具箱 （ScriptManager 位於 AJAX 延伸模組 索引標籤下）。 或者，您可以輸入下列標記原始碼檢視下開啟伺服器端表單標記：

&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;

ColorPicker 控制項擴充項新增至頁面的最簡單方式是在設計檢視中。 如果您將滑鼠停留 txtCardColor 文字方塊時，智慧工作選項，將會出現可讓您將新增 extender （請參閱 [圖 3]）。 如果您選擇此選項時，擴充性精靈] 隨即出現 （請參閱 [圖 4）。


[![新增 extender](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)

**圖 03**： 加入擴充項 ([按一下以檢視完整大小的影像](using-the-colorpicker-control-extender-vb/_static/image6.png))


[![選取 使用 Extender 精靈控制項擴充項](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)

**圖 04**： 選取使用 Extender 精靈控制項擴充項 ([按一下以檢視完整大小的影像](using-the-colorpicker-control-extender-vb/_static/image8.png))


您可以挑選 ColorPicker 擴充項，來擴充 txtCardColor 文字方塊與 ColorPicker 擴充項。 按一下 [確定] 以關閉對話方塊。

進行這些變更之後，來源頁面看起來像列表 2 中。

**列表 2-CreateCard.aspx （含 ColorPicker)**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample2.aspx)]

請注意頁面現在包含 ColorPickerExtender 控制項 txtCardColor TextBox 控制項的正下方顯示。 ColorPickerExtender 控制延伸 txtCardColor 控制項，讓它會顯示色彩選擇器對話方塊。

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>使用按鈕來啟動色彩選擇器對話方塊

ColorPicker extender 支援下列屬性：

- 可利用 PopupButtonId-會導致出現色彩選擇器對話方塊頁面上的按鈕的識別碼。
- PopupPosition-的位置，相對於目標控制項的 [色彩選擇器] 對話方塊。 可能值為 Absolute、 中心、 BottomLeft、 BottomRight、 TopLeft、 右上方和左側 （預設為 BottomLeft）。
- SampleControlId-顯示所選取之的色彩的控制項的 ID。
- SelectedColor-ColorPicker 所選取的初始色彩。

您可以使用這些屬性來自訂顯示色彩選擇器對話方塊的方式，以及所選取之的色彩的顯示方式。 [列表 3] 頁面會說明您如何使用數個這些屬性。

**列表 3-CreateCardButton.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample3.aspx)]

[列表 3] 頁面包含挑選色彩按鈕 （請參閱 [圖 5]）。 當您按一下此按鈕時，色彩選擇器對話方塊上方文字方塊中。 如果您選取色彩對話方塊中選取的色彩就會顯示為 lblSample Label 控制項的背景色彩。

ColorPicker 可利用 PopupButtonID 屬性用來關聯 ColorPicker 擴充項中的 [挑選顏色] 按鈕。 當您提供可利用 PopupButtonID 屬性的值時，色彩選擇器對話方塊不會再出現目標控制項有焦點時。 您必須按一下按鈕以顯示對話方塊。

SampleControlID 屬性用來將會顯示所選取的色彩與 ColorPicker 控制項產生關聯。 ColorPicker 會將此控制項的背景色彩變成目前選取的色彩。


[![顯示具有按鈕的色彩選擇器對話方塊](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)

**圖 05**： 顯示具有按鈕的色彩選擇器對話方塊 ([按一下以檢視完整大小的影像](using-the-colorpicker-control-extender-vb/_static/image10.png))


## <a name="summary"></a>總結

在本教學課程中，您已了解如何使用 ColorPicker 控制項擴充項顯示快顯色彩選擇器對話方塊。 首先，我們會檢查焦點會移至 TextBox 控制項時，如何顯示對話方塊。 接下來，您已了解如何建立按鈕，按一下按鈕時，會顯示 [色彩選擇器] 對話方塊。

> [!div class="step-by-step"]
> [上一步](using-the-colorpicker-control-extender-cs.md)
