---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: 以動態方式控制 UpdatePanel 動畫 (VB) |Microsoft 文件
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 內容...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: ff2853b4457a83a7367b4d1072d21929c40a3ed2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="dynamically-controlling-updatepanel-animations-vb"></a>以動態方式控制 UpdatePanel 動畫 (VB)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)

> 動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 UpdatePanel 的內容，特殊的擴充項存在，高度依賴動畫 framework: UpdatePanelAnimation。 它也可以一起 UpdatePanel 觸發程序。


## <a name="overview"></a>總覽

動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 內容的`UpdatePanel`，特殊的擴充項存在，高度依賴動畫 framework: `UpdatePanelAnimation`。 它也可搭配`UpdatePanel`觸發程序。

## <a name="steps"></a>步驟

第一個步驟是包含如往常般`ScriptManager`頁面中，而 ASP.NET AJAX library 載入 Control Toolkit 可用：


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

在此案例中動畫會套用到目前時間的顯示。 這項資訊可以寫入標籤使用`Page_Load()`方法，或使用下列的內嵌程式碼 （為了簡單起見）：


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

此外，也會建立的按鈕來觸發更新的時間：


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

此程式碼再將放入`<ContentTemplate>`區段`UpdatePanel`項目。 面板的`UpdateMode`屬性必須設為`"Conditional"`，因為只有觸發程序可能會更新面板的內容。 在`<Triggers>`區段`UpdatePanel`，非同步回傳觸發程序建立並繫結至`Click`按鈕的事件。 因此，如果使用者按一下按鈕，`UpdatePanel`會重新整理。 以下是標記`UpdatePanel`控制項：


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

最後，`UpdatePanelAnimationExtender`必須設定： 設定`TargetControlID`面板的 id 屬性，定義內 extender 動畫。 淡入使意義而言，建立好 visual 強調更新的時間。 然後，您擴充項的標記可能看起來像這樣：


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

在瀏覽器中執行的檔案。 每當您按一下按鈕時，目前的時間會顯示在面板中，一律為一秒期間淡入。


[![目前時間淡入](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)

目前時間淡入 ([按一下以檢視完整大小的影像](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一步](animating-an-updatepanel-control-vb.md)
