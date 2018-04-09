---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: 動畫 (VB) 期間停用動作 |Microsoft 文件
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 它也支援動作...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 9e2a0517800e90788bb67c1d75482a3d9340674b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="disabling-actions-during-animation-vb"></a>停用的動作，在動畫中的 (VB)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)

> 動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 它也支援的動作，例如滑鼠點按。 不過當滑鼠點按開始播放動畫，最好在動畫期間停用滑鼠點按動作。


## <a name="overview"></a>總覽

動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 它也支援的動作，例如滑鼠點按。 不過當滑鼠點按開始播放動畫，最好在動畫期間停用滑鼠點按動作。

## <a name="steps"></a>步驟

首先，包括`ScriptManager`在頁面; 然後，ASP.NET AJAX 程式庫載入，因此您能夠使用控制項的工具組：

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

動畫會套用至 HTML 按鈕像這樣：

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

請注意 HTML 控制項用而不是 Web 控制項，因為我們不想要建立回傳; 按鈕它只應該啟動為我們的用戶端動畫。

接著，新增`AnimationExtender`到頁面上，提供`ID`、`TargetControlID`屬性和必要`runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

內`<Animations>` 節點，`<OnClick>`是正確的項目，來處理滑鼠點按。 不過，無法在動畫，以及按一下按鈕。 `<EnableAction>`項目可以採取的處理。 設定`Enabled="false"`一部分動畫停用按鈕。 因為我們使用數個個別的動畫 （停用按鈕與實際的動畫，）`<Parallel>`元素必須在黏附結合成一個單一的動畫。 以下是完整標記`AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

它也會重新啟用按鈕動畫，並使用下列 XML 項目清單的結尾之後：

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

不過在特定案例中這會是毫無用處自按鈕淡出，結尾的動畫看不到。


[![動畫執行時，會停用按鈕](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)

動畫執行時，會停用按鈕 ([按一下以檢視完整大小的影像](disabling-actions-during-animation-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](animating-in-response-to-user-interaction-vb.md)
> [下一頁](triggering-an-animation-in-another-control-vb.md)
