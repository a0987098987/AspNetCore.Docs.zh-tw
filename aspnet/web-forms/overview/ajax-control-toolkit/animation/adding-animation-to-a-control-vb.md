---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: 將動畫加入至控制項 (VB) |Microsoft Docs
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 本教學課程示範如何...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 9392b1bab2289d886baf308d05644afbdc42a13a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832252"
---
<a name="adding-animation-to-a-control-vb"></a>將動畫加入至控制項 (VB)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)

> 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 本教學課程會示範如何設定這類動畫。


## <a name="overview"></a>總覽

動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 本教學課程會示範如何設定這類動畫。

## <a name="steps"></a>步驟

如往常般的第一個步驟是加入`ScriptManager`頁面中，讓 ASP.NET AJAX 程式庫已載入，並控制工具組可以用於：

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

在此案例中的動畫將會套用至面板的文字看起來像這樣：

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

面板相關聯的 CSS 類別定義的背景色彩和寬度：

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

接下來，我們需要`AnimationExtender`。 提供之後`ID`和一般`runat="server"`，則`TargetControlID`屬性必須設定為控制項，以在我們的案例中，[面板] 中建立動畫：

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

整個套用該動畫以宣告方式，使用 XML 語法，不幸的是目前不完全受到 Visual Studio's IntelliSense。 根節點是`<Animations>;`此節點中，內有多個事件允許用來決定當 animation(s) take(s) 位置：

- `OnClick` （按一下滑鼠）
- `OnHoverOut` （當滑鼠離開控制項）
- `OnHoverOver` (當滑鼠停留在控制項時，停止`OnHoverOut`動畫)
- `OnLoad` （當頁面已載入）
- `OnMouseOut` （當滑鼠離開控制項）
- `OnMouseOver` (當滑鼠停留在控制項時，不會 」 停止`OnMouseOut`動畫)

此架構隨附一組動畫，由它自己的 XML 項目表示每一個。 以下是選取項目：

- `<Color>` （變更色彩）
- `<FadeIn>` （淡入）
- `<FadeOut>` （淡出）
- `<Property>` （變更控制項的屬性）
- `<Pulse>` (pulsating)
- `<Resize>` （變更大小）
- `<Scale>` （按比例變更大小）

在此範例中，[面板] 中應該會淡出。動畫應採用 1.5 秒 (`Duration`屬性)，顯示每秒 24 畫面格數 （動畫步驟） (`Fps` attributs)。 以下是完成標記`AnimationExtender`控制項：

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

當您執行此指令碼時，面板會顯示，而且在一個半秒內淡出。


[![面板淡出](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)

面板淡出 ([按一下以檢視完整大小的影像](adding-animation-to-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](dynamically-controlling-updatepanel-animations-cs.md)
> [下一頁](executing-several-animations-at-the-same-time-vb.md)
