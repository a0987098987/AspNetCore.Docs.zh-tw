---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: 以動態方式控制 UpdatePanel 動畫 (VB) |Microsoft Docs
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 內容...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: 2a4c01ffdee20f2c7970d999b34bf1374088d4c5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824739"
---
<a name="dynamically-controlling-updatepanel-animations-vb"></a>以動態方式控制 UpdatePanel 動畫 (VB)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)

> 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 UpdatePanel 的內容，特殊的擴充項存在，依賴動畫架構： UpdatePanelAnimation。 它也可與 UpdatePanel 觸發程序。


## <a name="overview"></a>總覽

動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 內容`UpdatePanel`，特殊的擴充性已存在，而且依賴動畫架構： `UpdatePanelAnimation`。 它也可搭配`UpdatePanel`觸發程序。

## <a name="steps"></a>步驟

如往常般的第一個步驟是加入`ScriptManager`頁面中，讓 ASP.NET AJAX 程式庫已載入，並控制工具組可以用於：


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

在此案例中的動畫會套用至目前時間的顯示。 這項資訊寫入至標籤使用`Page_Load()`方法，或使用下列的內嵌程式碼 （為了簡單起見）：


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

此外，也會建立按鈕來觸發更新的時間：


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

此程式碼則進入`<ContentTemplate>`一節`UpdatePanel`項目。 面板`UpdateMode`屬性必須設為`"Conditional"`，因為只有觸發程序可能會更新面板內容。 在 `<Triggers>`一節`UpdatePanel`，非同步回傳觸發程序建立並繫結至`Click`按鈕的事件。 因此，如果使用者按一下按鈕，`UpdatePanel`會重新整理。 以下是標記`UpdatePanel`控制項：


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

最後，`UpdatePanelAnimationExtender`必須設定： 設定`TargetControlID`面板的 id 屬性，並定義動畫擴充項中的。 在進行中的漸淡方面來說，這會建立好 visual 強調更新的時間。 然後，您擴充項的標記可能看起來像這樣：


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

在瀏覽器中執行此檔案。 每當您按一下按鈕時，目前的時間會顯示在面板中，一律為一秒期間淡入。


[![目前的時間淡入](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)

目前的時間淡入 ([按一下以檢視完整大小的影像](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一步](animating-an-updatepanel-control-vb.md)
