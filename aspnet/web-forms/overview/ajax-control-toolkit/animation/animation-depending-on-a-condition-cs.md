---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: "根據條件 (C#) 動畫 |Microsoft 文件"
author: wenz
description: "動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 動畫的是否..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: 13366a86be01f41e27db1869b93192520190387a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="animation-depending-on-a-condition-c"></a>動畫根據條件 (C#)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)

> 動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 動畫執行時或不可以也會取決於一些 JavaScript 程式碼的表單中的條件。


## <a name="overview"></a>概觀

動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 動畫執行時或不可以也會取決於一些 JavaScript 程式碼的表單中的條件。

## <a name="steps"></a>步驟

首先，包括`ScriptManager`在頁面; 然後，ASP.NET AJAX 程式庫載入，因此您能夠使用控制項的工具組：

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

動畫會套用到面板中的文字看起來像這樣：

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

在 [面板] 中相關聯的 CSS 類別，定義好的背景色彩，也將面板固定的寬度設定：

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

接著，新增`AnimationExtender`到頁面上，提供`ID`、`TargetControlID`屬性和必要`runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

內`<Animations>`節點，請使用`<OnLoad>`頁面已完全載入後，執行動畫。 其中一個規則的動畫，而不是`<Condition>`元素派上用場。 提供的值為 JavaScript 程式碼`ConditionScript`屬性會在執行階段執行。 如果評估為 true，動畫會執行，否則不。 下列標記會提供兩個動畫，每個以 50%的隨機時的情況下執行。 因為只能有一個動畫內`<OnLoad>`，兩個`<Condition>`動畫聯結在一起使用`<Sequence>`項目：

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

請注意，小於符號 (`<`) 中`ConditionScript`屬性必須是逸出 （）。 當您執行此指令碼，可能是沒有動畫執行時，或兩者的其中一個存在，或同時執行。


[![面板淡出沒有調整大小，因此第二個動畫執行時，第一個未](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)

面板淡出沒有調整大小，因此未在第二個動畫執行，第一個 ([按一下以檢視完整大小的影像](animation-depending-on-a-condition-cs/_static/image3.png))

>[!div class="step-by-step"]
[上一頁](executing-several-animations-after-each-other-cs.md)
[下一頁](picking-one-animation-out-of-a-list-cs.md)
