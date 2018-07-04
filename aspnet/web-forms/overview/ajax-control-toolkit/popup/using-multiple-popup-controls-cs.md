---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: 使用多個快顯視窗控制項 (C#) |Microsoft Docs
author: wenz
description: PopupControl 擴充項在 AJAX Control Toolkit 提供簡單的方式，來啟動任何其他控制項時，觸發快顯視窗。 此外，也可以使用 m...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 5b00f720b66e6826c29f51690ab3361958aa8677
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364811"
---
<a name="using-multiple-popup-controls-c"></a>使用多個快顯視窗控制項 (C#)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)

> PopupControl 擴充項在 AJAX Control Toolkit 提供簡單的方式，來啟動任何其他控制項時，觸發快顯視窗。 它也可使用一個以上的快顯視窗控制項，在單一頁面上。


## <a name="overview"></a>總覽

PopupControl 擴充項在 AJAX Control Toolkit 提供簡單的方式，來啟動任何其他控制項時，觸發快顯視窗。 它也可使用一個以上的快顯視窗控制項，在單一頁面上。

## <a name="steps"></a>步驟

若要啟動的 ASP.NET AJAX Control Toolkit 中，功能`ScriptManager`控制項必須放置在任何位置上 (但在`<form>`項目):

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

接下來，新增一個面板，以做為快顯視窗。 在目前的案例中，包含面板`Calendar`控制項。 為了避免因行事曆的回傳的重新整理頁面，面板會放在`UpdatePanel`控制項：

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

此頁面也包含兩個文字方塊。 針對每個文字方塊中，一旦文字方塊就會啟動，應該會出現快顯行事曆。

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

現在來擴充每個使用中的兩個文字方塊`PopupControlExtender`。 `TargetControlID`屬性提供繫結至擴充項控制項的 ID。 `PopupControlID`屬性包含的快顯面板識別碼。 在此情況下，這兩個擴充項會顯示相同的窗格中，但也是可行的不同的面板。

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

現在每當您按一下文字欄位中，行事曆會顯示以下欄位，可讓您選取的日期。 （回到選取的日期文字方塊將會說明在不同的教學課程。）


[![當使用者按一下文字方塊，即出現日曆](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)

當使用者按一下文字方塊，即出現日曆 ([按一下以檢視完整大小的影像](using-multiple-popup-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [下一步](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
