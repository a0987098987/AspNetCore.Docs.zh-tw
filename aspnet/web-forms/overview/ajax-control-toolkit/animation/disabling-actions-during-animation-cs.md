---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: 停用動作期間動畫 (C#) |Microsoft Docs
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 它也支援動作...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: a82d46f47cf12b29284bf9211545f8984a586c03
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365577"
---
<a name="disabling-actions-during-animation-c"></a>停用動作期間動畫 (C#)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)

> 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 它也支援動作，例如滑鼠點按。 不過當按下滑鼠，開始播放動畫，最好在動畫期間停用滑鼠點按。


## <a name="overview"></a>總覽

動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 它也支援動作，例如滑鼠點按。 不過當按下滑鼠，開始播放動畫，最好在動畫期間停用滑鼠點按。

## <a name="steps"></a>步驟

首先，包括`ScriptManager`單元頁面; 然後，ASP.NET AJAX 程式庫載入，因此能夠使用控制項工具組：

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

動畫會套用至 HTML 按鈕如下：

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

請注意，HTML 控制項使用而不是 Web 控制項，因為我們不想要的按鈕，以建立回傳;它只應該會啟動我們的用戶端動畫。

然後，新增`AnimationExtender` 頁面上，以提供`ID`，則`TargetControlID`屬性和必要`runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

內`<Animations>`節點，`<OnClick>`是正確的項目，來處理滑鼠點選。 不過，動畫播放期間，也可以按一下按鈕。 `<EnableAction>`可以處理項目。 設定`Enabled="false"`動畫的過程中停用按鈕。 因為我們使用數個個別的動畫 （停用按鈕和實際的動畫）`<Parallel>`元素必須結合在一起成一個單一的動畫。 以下是完成標記`AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

它也會重新啟用按鈕動畫，並在清單結尾處使用下列的 XML 項目之後：

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

不過在特定案例這會是毫無用處自按鈕淡出，並在動畫結束時看不到。


[![動畫執行時，會停用按鈕](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)

按鈕已停用，只要執行動畫 ([按一下以檢視完整大小的影像](disabling-actions-during-animation-cs/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](animating-in-response-to-user-interaction-cs.md)
> [下一頁](triggering-an-animation-in-another-control-cs.md)
