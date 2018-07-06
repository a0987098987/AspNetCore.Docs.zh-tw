---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: 依據條件 (C#) 的動畫 |Microsoft Docs
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 動畫是否是...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: cb08c330d6fbc86035a2f21ad382cc009411bcd6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808420"
---
<a name="animation-depending-on-a-condition-c"></a>依據條件 (C#) 的動畫
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)

> 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 動畫執行時是否也取決於一些 JavaScript 程式碼的表單中的條件。


## <a name="overview"></a>總覽

動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 動畫執行時是否也取決於一些 JavaScript 程式碼的表單中的條件。

## <a name="steps"></a>步驟

首先，包括`ScriptManager`單元頁面; 然後，ASP.NET AJAX 程式庫載入，因此能夠使用控制項工具組：

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

動畫將會套用至面板的文字看起來像這樣：

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

在 [面板] 中相關聯的 CSS 類別，定義好用的背景色彩和也設定面板的固定的寬度：

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

然後，新增`AnimationExtender` 頁面上，以提供`ID`，則`TargetControlID`屬性和必要 `runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

內`<Animations>`節點，請使用`<OnLoad>`頁面完全載入後，執行動畫。 而不是一個一般的動畫，`<Condition>`元素派上用場。 提供的值為 JavaScript 程式碼`ConditionScript`屬性會在執行階段執行。 如果評估為 true，動畫會執行，否則不。 下列標記會提供兩個動畫，每個正在執行 50%的隨機時的案例。 因為只能有一張動畫內`<OnLoad>`，這兩個`<Condition>`動畫會聯結在一起使用`<Sequence>`項目：

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

請注意，為小於符號 (`<`) 中`ConditionScript`屬性必須逸出 （）。 當您執行此指令碼，可能是沒有動畫執行時，或兩者的其中一個存在，或兩個的作用。


[![面板淡出但不調整大小，因此未在第二個動畫執行後的第一個](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)

面板淡出但不調整大小，因此未在第二個動畫執行後的第一個 ([按一下以檢視完整大小的影像](animation-depending-on-a-condition-cs/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](executing-several-animations-after-each-other-cs.md)
> [下一頁](picking-one-animation-out-of-a-list-cs.md)
