---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: 使用用戶端程式碼 (VB) 變更動畫 |Microsoft Docs
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 動畫也可以...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: cc8ca2c962c5ebe5e0c45d5b575031ada3e64acd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386743"
---
<a name="changing-an-animation-using-client-side-code-vb"></a>變更動畫使用用戶端程式碼 (VB)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)

> 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 動畫也可以使用自訂用戶端 JavaScript 程式碼會變更。


## <a name="overview"></a>總覽

動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 動畫也可以使用自訂用戶端 JavaScript 程式碼會變更。

## <a name="steps"></a>步驟

首先，包括`ScriptManager`單元頁面; 然後，ASP.NET AJAX 程式庫載入，因此能夠使用控制項工具組：

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

動畫將會套用至面板的文字看起來像這樣：

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

在 [面板] 中相關聯的 CSS 類別，定義好用的背景色彩和也設定面板的固定的寬度：

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

實際的動畫是由 HTML 按鈕啟動：

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

然後，新增`AnimationExtender` 頁面上，以提供`ID`，則`TargetControlID`屬性和必要`runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

請注意，沒有任何`<Animations>`內的節點`AnimationExtender`控制項。 自訂的 JavaScript 程式碼用來提供要搭配控制項使用的動畫。

如同伺服器 API 的`AnimationExtender`，沒有任何簡單的方法，將尚未指派給擴充項的動畫。 不過擴充項會公開數種方法來讀取和寫入動畫向各種事件 (`OnClick`，`OnLoad`等等)。 以下是一些範例：

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

傳回值的格式`get_*()`函式和引數的格式`set_*()`函式是 JSON 字串，提供想要的 XML 標記的物件表示。 目前沒有任何方法，將物件傳送中，但您可指定動畫從讀取物件 (`get_OnXXXBehavior()`方法)。

以下是 JSON 字串 (不含分隔引號和格式化) 表示在按鈕中，所觸發的動畫，但是調整它，並在同一時間淡出動畫面板：

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

下列 JavaScript 程式碼會將指派到這個 JSON descripting`OnClick`動畫目前的擴充項，並執行它：

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]


[![動畫會立即執行，而不需要按下滑鼠 （和以非常少的標記）](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)

動畫會立即執行，沒有滑鼠點選 （和以非常少的標記） ([按一下以檢視完整大小的影像](changing-an-animation-using-client-side-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](executing-animations-using-client-side-code-vb.md)
> [下一頁](animating-an-updatepanel-control-vb.md)
