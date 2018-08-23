---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: 修改動畫從伺服器端 (C#) |Microsoft Docs
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 動畫也可能...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: ac2c2b1e9cfceba7f818f3f2dcbd719e94bea83e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835115"
---
<a name="modifying-animations-from-the-server-side-c"></a>修改動畫從伺服器端 (C#)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)

> 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 可能也會在伺服器端變更的動畫


## <a name="overview"></a>總覽

動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 可能也會在伺服器端變更的動畫

## <a name="steps"></a>步驟

首先，包括`ScriptManager`單元頁面; 然後，ASP.NET AJAX 程式庫載入，因此能夠使用控制項工具組：

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

動畫將會套用至面板的文字看起來像這樣：

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

在 [面板] 中相關聯的 CSS 類別，定義好用的背景色彩和也設定面板的固定的寬度：

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

其餘的程式碼會在伺服器端上執行，而不使用標記，相反地，它使用的程式碼建立`AnimationExtender`控制項：

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

不過，此控制項工具組目前不提供的 API 存取權，建立個別的動畫。 但它是可以設定`AnimationExtender`的動畫屬性設為字串包含以宣告方式指定動畫時使用的 XML 標記。 若要建立不能包含 XML`<Animations>`您可以使用.NET Framework 的 XML 項目會支援，或如下列程式碼，只是提供的字串：

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

最後，新增`AnimationExtender`內目前的頁面，來控制`<form runat="server">`項目，並確定包含動畫，並執行：

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]


[![使用伺服器端 C# /VB 程式碼來建立動畫](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)

使用伺服器端 C# /VB 程式碼來建立動畫 ([按一下以檢視完整大小的影像](modifying-animations-from-the-server-side-cs/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](triggering-an-animation-in-another-control-cs.md)
> [下一頁](executing-animations-using-client-side-code-cs.md)
