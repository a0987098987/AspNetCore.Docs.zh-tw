---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: "將動畫加入至控制項 (C#) |Microsoft 文件"
author: wenz
description: "動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 本教學課程示範如何..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7016ae3c92c665136579a8588818e6e4179a102a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="adding-animation-to-a-control-c"></a>將動畫加入至控制項 (C#)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)

> 動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 本教學課程會示範如何設定這類動畫。


## <a name="overview"></a>概觀

動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 本教學課程會示範如何設定這類動畫。

## <a name="steps"></a>步驟

第一個步驟是包含如往常般`ScriptManager`頁面中，而 ASP.NET AJAX library 載入 Control Toolkit 可用：

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

在此案例中動畫會套用到面板中的文字看起來像這樣：

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

面板的相關聯的 CSS 類別定義的背景色彩和寬度：

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

接下來，我們需要`AnimationExtender`。 提供後`ID`和一般`runat="server"`、`TargetControlID`屬性必須設為控制項，以動畫方式顯示在此案例中的面板：

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

整個動畫套用瞬間以宣告方式，使用 XML 語法，不幸的是 Visual Studio intellisense 目前不完全支援。 根節點是`<Animations>;`在此節點中，數個事件會允許用來決定當動畫 take(s) 位置：

- `OnClick`（按一下滑鼠）
- `OnHoverOut`（當滑鼠離開控制項）
- `OnHoverOver`(當滑鼠停留在控制項上，停止`OnHoverOut`動畫)
- `OnLoad`（當已經載入頁面）
- `OnMouseOut`（當滑鼠離開控制項）
- `OnMouseOver`(當滑鼠停留在控制項上時，不停止`OnMouseOut`動畫)

架構隨附一組自己的 XML 元素所代表的每個動畫。 以下是選取項目：

- `<Color>`（變更一種色彩）
- `<FadeIn>`（淡入）
- `<FadeOut>`（淡出）
- `<Property>`（變更控制項的屬性）
- `<Pulse>`(pulsating)
- `<Resize>`（變更大小）
- `<Scale>`（按比例變更大小）

在此範例中，[面板] 中應該淡出。動畫應採用 1.5 秒 (`Duration`屬性)，顯示每秒 24 畫面格數 （動畫步驟） (`Fps` attributs)。 以下是完整標記`AnimationExtender`控制項：

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

當您執行此指令碼時，面板會顯示和淡出一個半秒。


[![面板淡出](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)

面板淡出 ([按一下以檢視完整大小的影像](adding-animation-to-a-control-cs/_static/image3.png))

>[!div class="step-by-step"]
[下一步](executing-several-animations-at-the-same-time-cs.md)
