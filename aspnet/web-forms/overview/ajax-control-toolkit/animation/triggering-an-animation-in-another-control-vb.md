---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: 觸發另一個控制項 (VB) 的動畫 |Microsoft Docs
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 一般而言，啟動...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: d9ef7d35e34bd4f2b1433f7fb02d0c48834b2357
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804888"
---
<a name="triggering-an-animation-in-another-control-vb"></a>觸發另一個控制項 (VB) 的動畫
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)

> 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 一般而言，啟動動畫時觸發相同的控制項與使用者互動。 不過也可以使用一個控制項，然後動畫互動另一個控制項。


## <a name="overview"></a>總覽

動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 一般而言，啟動動畫時觸發相同的控制項與使用者互動。 不過也可以使用一個控制項，然後動畫互動另一個控制項。

## <a name="steps"></a>步驟

首先，包括`ScriptManager`單元頁面; 然後，ASP.NET AJAX 程式庫載入，因此能夠使用控制項工具組：

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

動畫將會套用至面板的文字看起來像這樣：

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

在 [面板] 中相關聯的 CSS 類別，定義好用的背景色彩和也設定面板的固定的寬度：

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

若要開始以動畫顯示面板，可使用 HTML 按鈕。 請注意， `<input type="button" />` favoured 透過`<asp:Button />`因為我們不想回傳時使用者按下該按鈕。

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

然後，新增`AnimationExtender` 頁面上，以提供`ID`，則`TargetControlID`屬性和必要`runat="server"`。 請務必設定`TargetControlID`id 的按鈕 （項目觸發動畫），不為面板 （正在顯示動畫的元素） 的識別碼

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

內`<Animations>`節點，如往常般進行動畫。 為了讓它們變更 [面板] 中，不是按鈕，設定`AnimationTarget`內的每個動畫項目的屬性`AnimationExtender`。 值`AnimationTarget`當然是面板的 ID。 如此一來，動畫會發生的窗格中，不會與觸發按鈕。 以下是`AnimationExtender`此案例中的標記：

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

請注意特殊個別動畫顯示的順序。 首先，動畫執行後，便會停用按鈕。 因為沒有任何`AnimationTarget`屬性中`<EnableAction>`項目，這個動畫會套用至原始的控制項: [] 按鈕。 下面兩個動畫步驟都應該實行 parallelly (`<Parallel>`項目)。 兩者都有其`AnimationTarget`屬性設定為`"Panel1"`，因此以動畫顯示的窗格中，不是按鈕。


[![在按鈕上的按一下滑鼠開始面板動畫](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)

在按鈕上的按一下滑鼠開始面板動畫 ([按一下以檢視完整大小的影像](triggering-an-animation-in-another-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](disabling-actions-during-animation-vb.md)
> [下一頁](modifying-animations-from-the-server-side-vb.md)
