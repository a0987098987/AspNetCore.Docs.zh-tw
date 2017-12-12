---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: "挑選清單 (VB) 超出一個動畫 |Microsoft 文件"
author: wenz
description: "動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 架構也允許..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: dd2cfd512b03fa1d1f7754d9f86d080c1977695d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="picking-one-animation-out-of-a-list-vb"></a>挑選清單 (VB) 超出一個動畫
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)

> 動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 此架構也可讓程式設計人員挑選一個動畫超出清單的動畫，根據一些 JavaScript 程式碼評估。


## <a name="overview"></a>概觀

動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 此架構也可讓程式設計人員挑選一個動畫超出清單的動畫，根據一些 JavaScript 程式碼評估。

## <a name="steps"></a>步驟

首先，包括`ScriptManager`在頁面; 然後，ASP.NET AJAX 程式庫載入，因此您能夠使用控制項的工具組：

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

動畫會套用到面板中的文字看起來像這樣：

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

在 [面板] 中相關聯的 CSS 類別，定義好的背景色彩，也將面板固定的寬度設定：

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

接著，新增`AnimationExtender`到頁面上，提供`ID`、`TargetControlID`屬性和必要`runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

內`<Animations>`節點，請使用`<OnLoad>`頁面已完全載入後，執行動畫。 其中一個規則的動畫，而不是`<Case>`元素派上用場。 會評估其 SelectScript 屬性的值;傳回值必須是數字。 根據這個數字，其中一個內 subanimations&lt;案例&gt;執行。 比方說，如果 SelectScript 判斷值為 2，Control Toolkit 執行內的第三個動畫&lt;案例&gt;（從 0 開始）。

下列標記會定義三個 subanimations： 調整大小的寬度、 高度調整大小和淡出。JavaScript 程式碼 (`Math.floor(3 * Math.random())`) 然後挑選一個介於 0 和 2，使其中的三個動畫執行：

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]


[![其中一個可能的三個動畫： 面板變寬](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)

其中一個可能的三個動畫： 面板變寬 ([按一下以檢視完整大小的影像](picking-one-animation-out-of-a-list-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一頁](animation-depending-on-a-condition-vb.md)
[下一頁](animating-in-response-to-user-interaction-vb.md)
