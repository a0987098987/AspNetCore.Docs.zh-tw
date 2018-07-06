---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: 從伺服器端 (VB) 修改動畫 |Microsoft Docs
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 動畫也可能...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: 5ff8685b44e7d0c2261ea7b9f1ca6397ae05ce53
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811162"
---
<a name="modifying-animations-from-the-server-side-vb"></a>修改動畫從伺服器端 (VB)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)

> 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 可能也會在伺服器端變更的動畫


## <a name="overview"></a>總覽

動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 可能也會在伺服器端變更的動畫

## <a name="steps"></a>步驟

首先，包括`ScriptManager`單元頁面; 然後，ASP.NET AJAX 程式庫載入，因此能夠使用控制項工具組：

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

動畫將會套用至面板的文字看起來像這樣：

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

在 [面板] 中相關聯的 CSS 類別，定義好用的背景色彩和也設定面板的固定的寬度：

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

其餘的程式碼會在伺服器端上執行，而不使用標記，相反地，它使用的程式碼建立`AnimationExtender`控制項：

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

不過，此控制項工具組目前不提供的 API 存取權，建立個別的動畫。 但它是可以設定`AnimationExtender`的動畫屬性設為字串包含以宣告方式指定動畫時使用的 XML 標記。 若要建立不能包含 XML`<Animations>`您可以使用.NET Framework 的 XML 項目會支援，或如下列程式碼，只是提供的字串：

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

最後，新增`AnimationExtender`內目前的頁面，來控制`<form runat="server">`項目，並確定包含動畫，並執行：

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]


[![使用伺服器端 C# /VB 程式碼來建立動畫](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)

使用伺服器端 C# /VB 程式碼來建立動畫 ([按一下以檢視完整大小的影像](modifying-animations-from-the-server-side-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](triggering-an-animation-in-another-control-vb.md)
> [下一頁](executing-animations-using-client-side-code-vb.md)
