---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
title: 執行動畫使用用戶端程式碼 (C#) |Microsoft 文件
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 動畫執行...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0270e0df-6fde-4a8f-a2cb-2cacc55143f2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: cfaed745b4c547d04033ee89d2c6549c5989cb73
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="executing-animations-using-client-side-code-c"></a>執行動畫使用用戶端程式碼 (C#)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)

> 動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 動畫執行也可能使用自訂用戶端 JavaScript 程式碼會觸發。


## <a name="overview"></a>總覽

動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 動畫執行也可能使用自訂用戶端 JavaScript 程式碼會觸發。

## <a name="steps"></a>步驟

首先，包括`ScriptManager`在頁面; 然後，ASP.NET AJAX 程式庫載入，因此您能夠使用控制項的工具組：

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample1.aspx)]

動畫會套用到面板中的文字看起來像這樣：

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample2.aspx)]

在 [面板] 中相關聯的 CSS 類別，定義好的背景色彩，也將面板固定的寬度設定：

[!code-css[Main](executing-animations-using-client-side-code-cs/samples/sample3.css)]

接著，新增`AnimationExtender`到頁面上，提供`ID`、`TargetControlID`屬性和必要`runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample4.aspx)]

內`<Animations>`節點，請使用`<OnClick>`執行動畫一次使用者按一下面板上。 加入兩個動畫 parallelly 執行：

[!code-xml[Main](executing-animations-using-client-side-code-cs/samples/sample5.xml)]

為了示範，這個動畫 （及其他使用此控制項的工具組建立的動畫） 會執行使用 JavaScript 程式碼之後執行網頁。 我們第一次，所有需要存取`AnimationExtender`控制項。 ASP.NET AJAX 程式庫提供`$find()`函式，這項工作：

[!code-csharp[Main](executing-animations-using-client-side-code-cs/samples/sample6.cs)]

`AnimationExtender`控制項不僅會公開一個豐富的 API，包括具有名稱相同的 XML 標記中使用的事件處理常式的方法： `OnClick()`， `OnLoad()`，依此類推。 比方說，呼叫`OnClick()`方法執行內動畫`<OnClick>`元素`AnimationExtender`控制項：

[!code-javascript[Main](executing-animations-using-client-side-code-cs/samples/sample7.js)]

以下是完整用戶端 JavaScript 程式碼，來模擬面板上的按一下頁面已完全載入後，請注意，`pageLoad()`函式的名稱會由呼叫 ASP.NET AJAX 一次頁面和 JavaScript 程式庫已包含所有載入。

[!code-html[Main](executing-animations-using-client-side-code-cs/samples/sample8.html)]


[![動畫便會立即執行，不按滑鼠](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)

動畫會立即執行，而按一下滑鼠 ([按一下以檢視完整大小的影像](executing-animations-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](modifying-animations-from-the-server-side-cs.md)
> [下一頁](changing-an-animation-using-client-side-code-cs.md)
