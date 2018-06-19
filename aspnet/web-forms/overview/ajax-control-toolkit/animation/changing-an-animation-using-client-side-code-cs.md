---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: 變更使用用戶端程式碼 (C#) 動畫 |Microsoft 文件
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 動畫也可以...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 2f572efeb012d88ab15740bab7b0e4383572f3f7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870644"
---
<a name="changing-an-animation-using-client-side-code-c"></a>變更動畫使用用戶端程式碼 (C#)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)

> 動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 動畫也可以使用自訂用戶端 JavaScript 程式碼會變更。


## <a name="overview"></a>總覽

動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 動畫也可以使用自訂用戶端 JavaScript 程式碼會變更。

## <a name="steps"></a>步驟

首先，包括`ScriptManager`在頁面; 然後，ASP.NET AJAX 程式庫載入，因此您能夠使用控制項的工具組：

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

動畫會套用到面板中的文字看起來像這樣：

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

在 [面板] 中相關聯的 CSS 類別，定義好的背景色彩，也將面板固定的寬度設定：

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

實際的動畫是由 HTML 按鈕啟動：

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

接著，新增`AnimationExtender`到頁面上，提供`ID`、`TargetControlID`屬性和必要`runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

請注意，沒有任何`<Animations>`節點內`AnimationExtender`控制項。 自訂 JavaScript 程式碼用來提供以用於控制項的動畫。

使用伺服器 API 的`AnimationExtender`，沒有簡單的方法可以尚未指派給 extender 的動畫。 但是擴充項會公開數種方法來讀取和寫入動畫向各種事件 (`OnClick`，`OnLoad`等等)。 以下是一些範例：

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

傳回值的格式`get_*()`函式和引數格式`set_*()`函式是 JSON 字串而提供的 XML 標記會是以物件表示。 目前沒有任何方法，將物件傳送中，但是可以讀取指定的動畫物件 (`get_OnXXXBehavior()`方法)。

以下是 JSON 字串 (不含引號分隔且妥善格式化) 代表按鈕，所觸發的動畫，但是調整它，並在同一時間淡出動畫面板：

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

下列 JavaScript 程式碼會將指派到這個 JSON descripting`OnClick`目前的擴充項的動畫並執行它：

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


[![動畫會立即執行，不按滑鼠 （和以非常少的標記）](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)

動畫會立即執行，而按一下滑鼠 （和以非常少的標記） ([按一下以檢視完整大小的影像](changing-an-animation-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](executing-animations-using-client-side-code-cs.md)
> [下一頁](animating-an-updatepanel-control-cs.md)
