---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: "修改動畫從伺服器端 (VB) |Microsoft 文件"
author: wenz
description: "動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 也可能動畫..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: c5b23cce529be24157a8a3f9136de7ad7bafc1ea
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="modifying-animations-from-the-server-side-vb"></a>修改動畫從伺服器端 (VB)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)

> 動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 可能也會在伺服器端變更動畫


## <a name="overview"></a>概觀

動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 可能也會在伺服器端變更動畫

## <a name="steps"></a>步驟

首先，包括`ScriptManager`在頁面; 然後，ASP.NET AJAX 程式庫載入，因此您能夠使用控制項的工具組：

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

動畫會套用到面板中的文字看起來像這樣：

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

在 [面板] 中相關聯的 CSS 類別，定義好的背景色彩，也將面板固定的寬度設定：

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

程式碼的其餘部分會在伺服器端上執行，而不會使用標記，相反地，它使用程式碼建立`AnimationExtender`控制項：

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

不過，此控制項的工具組目前不提供如何建立個別的動畫 API 存取。 但它是可以設定`AnimationExtender`的動畫屬性設為字串包含時以宣告方式指定動畫所使用的 XML 標記。 若要建立不能包含 XML`<Animations>`您可以使用.NET Framework 的 XML 項目會支援，或如下列程式碼，只是提供字串：

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

最後，會加入`AnimationExtender`內目前的頁面，來控制`<form runat="server">`項目，並確認是否包含動畫，並執行：

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]


[![使用伺服器端 C# /VB 程式碼來建立動畫](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)

使用伺服器端 C# /VB 程式碼來建立動畫 ([按一下以檢視完整大小的影像](modifying-animations-from-the-server-side-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一頁](triggering-an-animation-in-another-control-vb.md)
[下一頁](executing-animations-using-client-side-code-vb.md)
