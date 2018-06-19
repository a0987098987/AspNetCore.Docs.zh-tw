---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
title: 在相同的時間 (C#) 執行數個動畫 |Microsoft 文件
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 它可讓執行 severa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 219149e1-3ee9-4b79-8fe4-7433f6b7d15b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
msc.type: authoredcontent
ms.openlocfilehash: 6ecd822f7fa00a24e97b9aa14888798825624617
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869357"
---
<a name="executing-several-animations-at-the-same-time-c"></a>執行數個動畫在相同的時間 (C#)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)

> 動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 它可讓以平行方式執行數個動畫。


## <a name="overview"></a>總覽

動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 它可讓以平行方式執行數個動畫。

## <a name="steps"></a>步驟

首先，包括`ScriptManager`在頁面; 然後，ASP.NET AJAX 程式庫載入，因此您能夠使用控制項的工具組：

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample1.aspx)]

動畫會套用到面板中的文字看起來像這樣：

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample2.aspx)]

在 [面板] 中相關聯的 CSS 類別，定義好的背景色彩，也將面板固定的寬度設定：

[!code-css[Main](executing-several-animations-at-the-same-time-cs/samples/sample3.css)]

接著，新增`AnimationExtender`到頁面上，提供`ID`、`TargetControlID`屬性和必要`runat="server"`:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample4.aspx)]

內`<Animations>`節點，請使用`<OnLoad>`頁面已完全載入後，執行動畫。 一般而言，`<OnLoad>`只接受一個動畫。 動畫架構可讓您加入到另一種使用數個動畫`<Parallel>`項目。 中的所有動畫`<Parallel>`會執行一次。

以下是可能的標記為`AnimationExtender`控制項淡出並調整面板大小相同的時間：

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample5.aspx)]

和確實： 當您執行此指令碼，面板隨即出現，然後調整大小時 (超過 threefolding 其寬度和 halfing 高度) 和淡出相同的時間。


[![面板是淡出並調整大小 （包括其內容，這點受惠瀏覽器的轉譯引擎）](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)

面板是淡出並調整大小 （包括其內容，這點受惠瀏覽器的轉譯引擎） ([按一下以檢視完整大小的影像](executing-several-animations-at-the-same-time-cs/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](adding-animation-to-a-control-cs.md)
> [下一頁](executing-several-animations-after-each-other-cs.md)
