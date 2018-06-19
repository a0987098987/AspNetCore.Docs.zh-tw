---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: 使用多個快顯視窗控制項 (VB) |Microsoft 文件
author: wenz
description: AJAX Control Toolkit PopupControl extender 提供簡單的方式來啟動任何其他控制項時，觸發快顯視窗。 此外，也可以使用 m...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 7c57aab3ecf2c02a8488b5ea4e3e0ed33ac5e7fe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869318"
---
<a name="using-multiple-popup-controls-vb"></a>使用多個快顯視窗控制項 (VB)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)

> AJAX Control Toolkit PopupControl extender 提供簡單的方式來啟動任何其他控制項時，觸發快顯視窗。 它也可用於在單一頁面上的多個 popup 控制項。


## <a name="overview"></a>總覽

AJAX Control Toolkit PopupControl extender 提供簡單的方式來啟動任何其他控制項時，觸發快顯視窗。 它也可用於在單一頁面上的多個 popup 控制項。

## <a name="steps"></a>步驟

為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`控制項必須任意位置放置在頁面 (但內部`<form>`項目):

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

接著，將會做為快顯視窗的面板。 在目前的案例中，包含面板`Calendar`控制項。 為了避免因行事曆的回傳的重新整理頁面，[面板] 中皆會置於`UpdatePanel`控制項：

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

這個頁面也包含兩個文字方塊。 針對每個文字方塊中，當啟動時在文字方塊中，應該會出現快顯行事曆。

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

現在擴充的兩個文字方塊與每個`PopupControlExtender`。 `TargetControlID`屬性提供的繫結至擴充項控制項的識別碼。 `PopupControlID`屬性包含快顯面板的識別碼。 在此情況下，這兩種 extender 顯示相同的面板中，但也是可行的不同的面板。

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

現在每當您按一下文字欄位中，就會出現日曆以下欄位，可讓您選取的日期。 （選取的日期取回在文字方塊中會涵蓋不同的教學課程中。）


[![當使用者按一下的文字方塊中，就會出現日曆](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)

當使用者按一下的文字方塊中，就會出現日曆 ([按一下以檢視完整大小的影像](using-multiple-popup-controls-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [下一頁](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
