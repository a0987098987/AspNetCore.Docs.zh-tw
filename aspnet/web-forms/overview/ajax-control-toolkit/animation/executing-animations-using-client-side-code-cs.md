---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
title: 執行動畫使用用戶端程式碼 (C#) |Microsoft Docs
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 動畫執行...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 0270e0df-6fde-4a8f-a2cb-2cacc55143f2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: ae8c4df3c5852e9ee95f6184a86059859c56b128
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824554"
---
<a name="executing-animations-using-client-side-code-c"></a>執行動畫使用用戶端程式碼 (C#)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)

> 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 動畫執行也可能使用自訂用戶端 JavaScript 程式碼會觸發。


## <a name="overview"></a>總覽

動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 動畫執行也可能使用自訂用戶端 JavaScript 程式碼會觸發。

## <a name="steps"></a>步驟

首先，包括`ScriptManager`單元頁面; 然後，ASP.NET AJAX 程式庫載入，因此能夠使用控制項工具組：

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample1.aspx)]

動畫將會套用至面板的文字看起來像這樣：

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample2.aspx)]

在 [面板] 中相關聯的 CSS 類別，定義好用的背景色彩和也設定面板的固定的寬度：

[!code-css[Main](executing-animations-using-client-side-code-cs/samples/sample3.css)]

然後，新增`AnimationExtender` 頁面上，以提供`ID`，則`TargetControlID`屬性和必要`runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample4.aspx)]

內`<Animations>`節點，請使用`<OnClick>`執行動畫一次使用者按一下面板上。 新增兩個動畫 parallelly 執行：

[!code-xml[Main](executing-animations-using-client-side-code-cs/samples/sample5.xml)]

為了示範，這個動畫 （和任何其他使用 Control Toolkit 建立的動畫） 執行之後執行網頁時，使用 JavaScript 程式碼。 首先我們必須存取`AnimationExtender`控制項。 ASP.NET AJAX 程式庫提供`$find()`函式，這項工作：

[!code-csharp[Main](executing-animations-using-client-side-code-cs/samples/sample6.cs)]

`AnimationExtender`控制項會公開豐富的 API，包括具有名稱相同的 XML 標記中所使用的事件處理常式的方法： `OnClick()`， `OnLoad()`，依此類推。 比方說，呼叫`OnClick()`方法會執行內的動畫`<OnClick>`項目`AnimationExtender`控制項：

[!code-javascript[Main](executing-animations-using-client-side-code-cs/samples/sample7.js)]

以下是 頁面完全載入後，模擬面板上的按一下 完成用戶端 JavaScript 程式碼，請注意，`pageLoad()`函式名稱可由呼叫 ASP.NET AJAX 一次頁面和所有包含的 JavaScript 程式庫已載入。

[!code-html[Main](executing-animations-using-client-side-code-cs/samples/sample8.html)]


[![動畫會立即執行，而不需要按下滑鼠](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)

動畫會立即執行，沒有滑鼠點選 ([按一下以檢視完整大小的影像](executing-animations-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](modifying-animations-from-the-server-side-cs.md)
> [下一頁](changing-an-animation-using-client-side-code-cs.md)
