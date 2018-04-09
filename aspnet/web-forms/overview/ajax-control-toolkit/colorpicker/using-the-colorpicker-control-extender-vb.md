---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
title: 使用 ColorPicker 控制項擴充項 (VB) |Microsoft 文件
author: microsoft
description: ColorPicker 是用戶端色彩挑選功能提供 UI，快顯功能表控制項中的 ASP.NET AJAX 擴充項。 可以將它附加至任何 ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 577ae07b-a872-4818-a804-bca489b40ad0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 3411119f85d7f5c26703b7df40cff24fdf30b81d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="using-the-colorpicker-control-extender-vb"></a>使用 ColorPicker 控制項擴充項 (VB)
====================
by [Microsoft](https://github.com/microsoft)

> ColorPicker 是用戶端色彩挑選功能提供 UI，快顯功能表控制項中的 ASP.NET AJAX 擴充項。 可以將它附加至任何 ASP.NET 文字方塊控制項。 它。


本教學課程的目標是要說明如何使用 AJAX 控制項 Toolkit ColorPicker 控制項擴充項。 ColorPicker 控制項擴充項會顯示快顯對話方塊中，可讓您選取的色彩。 每當您想要提供直覺式使用者介面來選擇色彩的使用者，ColorPicker 很有用。

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>擴充 ColorPicker 控制項擴充項的 TextBox 控制項

例如，假設您想要建立網站，讓建立自訂的名片的訪客。 訪客可以商務名片輸入的文字，並選取色彩。 程式碼範例 1 中的 ASP.NET 網頁包含名為 txtCardText 和 txtCardColor 的兩個文字方塊控制項。 當您送出表單時，會顯示選取的值 （請參閱圖 1）。


[![簡單的表單建立名片](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)

**圖 01**： 簡單的表單建立名片 ([按一下以檢視完整大小的影像](using-the-colorpicker-control-extender-vb/_static/image2.png))


**Listing 1 - CreateCard.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample1.aspx)]

表單的程式碼範例 1 是有效的但它不提供絕佳的使用者體驗。 使用者必須輸入的文字方塊中的色彩。 如果使用者想要特製化的色彩： 例如，只要右陰影 pea 綠色-然後使用者必須找出 HTML 色彩代碼，而不需任何協助。

若要建立較佳使用者體驗，您可以使用 ColorPicker 控制項擴充項。 當您將焦點移至 TextBox 控制項 ColorPicker 會顯示色彩對話方塊 （請參閱圖 2）。


[![ColorPicker 控制項擴充項](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)

**圖 02**: ColorPicker 控制項擴充項 ([按一下以檢視完整大小的影像](using-the-colorpicker-control-extender-vb/_static/image4.png))


您必須完成表單的程式碼範例 1 與使用 ColorPicker 控制項擴充項的兩個步驟：

1. ScriptManager 控制項加入網頁
2. 頁面上新增 ColorPicker 控制項擴充項

您可以使用 ColorPicker 之前，您必須將 ScriptManager 加入您的頁面。 若要加入 ScriptManager 最好先開啟伺服器端的正下方&lt;表單&gt;標記。 您可以拖曳到頁面的 ScriptManager，從工具箱 （ScriptManager 位於 AJAX 擴充功能 索引標籤）。 或者，您可以輸入下列標記來源檢視下方開頭伺服器端表單的標記：

&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;

ColorPicker 控制項擴充項新增至頁面的最簡單方式是在設計檢視中。 如果您將滑鼠停留 txtCardColor 文字方塊中，智慧工作選項會顯示可讓您將擴充項 （請參閱圖 3）。 如果您選擇此選項時，擴充性精靈 隨即出現 （請參閱圖 4）。


[![新增 extender](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)

**圖 03**： 加入擴充項 ([按一下以檢視完整大小的影像](using-the-colorpicker-control-extender-vb/_static/image6.png))


[![選取的控制項擴充項使用擴充性精靈](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)

**圖 04**： 選取 使用 Extender 精靈控制項擴充項 ([按一下以檢視完整大小的影像](using-the-colorpicker-control-extender-vb/_static/image8.png))


您可以挑選 ColorPicker extender 來延伸與 ColorPicker extender txtCardColor 文字方塊。 按一下 [確定] 以關閉對話方塊。

進行這些變更之後，頁面來源看起來像列表 2 中。

**列出 2-CreateCard.aspx （具有 ColorPicker)**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample2.aspx)]

請注意，頁面現在已包含 ColorPickerExtender 控制項 txtCardColor TextBox 控制項的正下方出現。 ColorPickerExtender 控制擴充 txtCardColor 控制項使其顯示色彩選擇器對話方塊。

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>使用按鈕以啟動色彩選擇器對話方塊

ColorPicker extender 支援下列屬性：

- PopupButtonId-會導致出現色彩選擇器對話方塊頁面上的按鈕的識別碼。
- PopupPosition-的位置，相對於目標控制項的 [色彩選擇器] 對話方塊。 可能的值為絕對、 中心、 BottomLeft、 BottomRight、 TopLeft、 TopRight 權限和左側 （預設為 BottomLeft）。
- SampleControlId-顯示所選取之色彩的控制項識別碼。
- SelectedColor-ColorPicker 所選取的初始色彩。

您可以使用這些屬性來自訂顯示色彩選擇器 對話方塊的方式，以及所選取之色彩的顯示方式。 [] 頁面中列出的 3 說明您如何使用這些屬性。

**Listing 3 - CreateCardButton.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample3.aspx)]

[] 頁面中列出的 3 包含挑選顏色按鈕 （請參閱圖 5）。 當您按一下這個按鈕時，色彩選擇器對話方塊上方的文字方塊中。 如果您選取色彩對話方塊中選取的色彩就會顯示為 lblSample Label 控制項的背景色彩。

ColorPicker PopupButtonID 屬性用來與 ColorPicker extender 挑選顏色 按鈕。 當您提供 PopupButtonID 屬性的值時，色彩選擇器對話方塊不會顯示目標控制項有焦點時。 您必須按一下按鈕以顯示對話方塊。

SampleControlID 屬性用來將會顯示所選取之色彩 ColorPicker 與控制項產生關聯。 ColorPicker 變更這個控制項的背景色彩為目前選取的色彩。


[![顯示色彩選擇器 對話方塊，使用按鈕](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)

**圖 05**： 顯示色彩選擇器 對話方塊，使用按鈕 ([按一下以檢視完整大小的影像](using-the-colorpicker-control-extender-vb/_static/image10.png))


## <a name="summary"></a>總結

在本教學課程中，您將學會如何顯示快顯色彩選擇器對話方塊使用 ColorPicker 控制項擴充項。 首先，我們會檢查當焦點移至 TextBox 控制項時，如何顯示對話方塊。 接下來，您會學到如何建立顯示色彩選擇器對話方塊時按下按鈕的按鈕。

> [!div class="step-by-step"]
> [上一步](using-the-colorpicker-control-extender-cs.md)
