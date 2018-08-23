---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: 挑選一張動畫 (VB) 的清單從 |Microsoft Docs
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 此架構也允許...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: 9c60d7cff7c841d23185fbdf07abf0e894b21cf5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826203"
---
<a name="picking-one-animation-out-of-a-list-vb"></a>挑選一張動畫從清單 (VB)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)

> 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 架構也可讓程式設計人員挑選一張動畫從清單的動畫，根據一些 JavaScript 程式碼的評估結果。


## <a name="overview"></a>總覽

動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 架構也可讓程式設計人員挑選一張動畫從清單的動畫，根據一些 JavaScript 程式碼的評估結果。

## <a name="steps"></a>步驟

首先，包括`ScriptManager`單元頁面; 然後，ASP.NET AJAX 程式庫載入，因此能夠使用控制項工具組：

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

動畫將會套用至面板的文字看起來像這樣：

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

在 [面板] 中相關聯的 CSS 類別，定義好用的背景色彩和也設定面板的固定的寬度：

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

然後，新增`AnimationExtender` 頁面上，以提供`ID`，則`TargetControlID`屬性和必要 `runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

內`<Animations>`節點，請使用`<OnLoad>`頁面完全載入後，執行動畫。 而不是一個一般的動畫，`<Case>`元素派上用場。 會評估其 SelectScript 屬性的值;傳回的值必須是數字。 根據此數字，其中一個內 subanimations&lt;案例&gt;執行。 比方說，如果 SelectScript 判斷值為 2，Control Toolkit 會執行內的第三個動畫&lt;案例&gt;（從 0 開始）。

下列標記會定義三個 subanimations： 調整大小的寬度、 調整大小的高度和淡出。JavaScript 程式碼 (`Math.floor(3 * Math.random())`)，讓其中的三種動畫會執行，然後選擇 0 和 2 之間的數字：

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]


[![其中一個可能的三個動畫: [面板] 中取得更多](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)

其中一個可能的三個動畫: [面板] 中取得更多 ([按一下以檢視完整大小的影像](picking-one-animation-out-of-a-list-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](animation-depending-on-a-condition-vb.md)
> [下一頁](animating-in-response-to-user-interaction-vb.md)
