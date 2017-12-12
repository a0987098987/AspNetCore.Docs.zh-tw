---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: "觸發另一個控制項 (C#) 中的動畫 |Microsoft 文件"
author: wenz
description: "動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 一般而言，啟動..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 8d243eebc42b66f1e86b38a1b7531e527144ea7e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="triggering-an-animation-in-another-control-c"></a>觸發另一個控制項 (C#) 中的動畫
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)

> 動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 一般而言，啟動動畫會觸發相同的控制項與使用者互動。 不過也可以與一個控制項，然後動畫互動另一個控制項。


## <a name="overview"></a>概觀

動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 一般而言，啟動動畫會觸發相同的控制項與使用者互動。 不過也可以與一個控制項，然後動畫互動另一個控制項。

## <a name="steps"></a>步驟

首先，包括`ScriptManager`在頁面; 然後，ASP.NET AJAX 程式庫載入，因此您能夠使用控制項的工具組：

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

動畫會套用到面板中的文字看起來像這樣：

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

在 [面板] 中相關聯的 CSS 類別，定義好的背景色彩，也將面板固定的寬度設定：

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

若要啟動動畫 面板中，會使用 HTML 按鈕。 請注意， `<input type="button" />` favoured 透過`<asp:Button />`因為我們不希望回傳當使用者按一下該按鈕時。

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

接著，新增`AnimationExtender`到頁面上，提供`ID`、`TargetControlID`屬性和必要`runat="server"`。 請務必設定`TargetControlID`id 的按鈕 （觸發動畫元素），無法為識別碼的面板 （正在顯示動畫項目）

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

內`<Animations>`節點，如往常般進行動畫。 若要讓這些變更面板，請設定不是按鈕，`AnimationTarget`內每個動畫項目的屬性`AnimationExtender`。 值`AnimationTarget`當然是面板的識別碼。 這樣一來，動畫會面板中，不能搭配觸發按鈕發生。 以下是`AnimationExtender`此案例中的標記：

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

請注意特殊個別動畫顯示的順序。 首先，一旦執行動畫取得停用按鈕。 因為沒有任何`AnimationTarget`屬性中`<EnableAction>`項目，這個動畫套用至原始控制項: [] 按鈕。 下面兩個動畫步驟應該實行 parallelly (`<Parallel>`項目)。 兩者都有其`AnimationTarget`屬性設為`"Panel1"`，因此建立動畫 面板中，不是按鈕。


[![在按鈕上的按一下滑鼠開始面板動畫](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)

在按鈕上的按一下滑鼠開始面板動畫 ([按一下以檢視完整大小的影像](triggering-an-animation-in-another-control-cs/_static/image3.png))

>[!div class="step-by-step"]
[上一頁](disabling-actions-during-animation-cs.md)
[下一頁](modifying-animations-from-the-server-side-cs.md)
