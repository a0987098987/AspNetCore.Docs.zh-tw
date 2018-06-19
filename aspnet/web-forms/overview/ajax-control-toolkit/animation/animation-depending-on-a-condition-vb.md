---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: 根據條件 (VB) 動畫 |Microsoft 文件
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 動畫的是否...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: d3a648ff8299c9720e9f34522f271595ab1b9bc9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868148"
---
<a name="animation-depending-on-a-condition-vb"></a>根據條件 (VB) 動畫
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)

> 動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 動畫執行時或不可以也會取決於一些 JavaScript 程式碼的表單中的條件。


## <a name="overview"></a>總覽

動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 動畫執行時或不可以也會取決於一些 JavaScript 程式碼的表單中的條件。

## <a name="steps"></a>步驟

首先，包括`ScriptManager`在頁面; 然後，ASP.NET AJAX 程式庫載入，因此您能夠使用控制項的工具組：

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

動畫會套用到面板中的文字看起來像這樣：

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

在 [面板] 中相關聯的 CSS 類別，定義好的背景色彩，也將面板固定的寬度設定：

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

接著，新增`AnimationExtender`到頁面上，提供`ID`、`TargetControlID`屬性和必要 `runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

內`<Animations>`節點，請使用`<OnLoad>`頁面已完全載入後，執行動畫。 其中一個規則的動畫，而不是`<Condition>`元素派上用場。 提供的值為 JavaScript 程式碼`ConditionScript`屬性會在執行階段執行。 如果評估為 true，動畫會執行，否則不。 下列標記會提供兩個動畫，每個以 50%的隨機時的情況下執行。 因為只能有一個動畫內`<OnLoad>`，兩個`<Condition>`動畫聯結在一起使用`<Sequence>`項目：

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

請注意，小於符號 (`<`) 中`ConditionScript`屬性必須是逸出 （）。 當您執行此指令碼，可能是沒有動畫執行時，或兩者的其中一個存在，或同時執行。


[![面板淡出沒有調整大小，因此第二個動畫執行時，第一個未](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)

面板淡出沒有調整大小，因此未在第二個動畫執行，第一個 ([按一下以檢視完整大小的影像](animation-depending-on-a-condition-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](executing-several-animations-after-each-other-vb.md)
> [下一頁](picking-one-animation-out-of-a-list-vb.md)
