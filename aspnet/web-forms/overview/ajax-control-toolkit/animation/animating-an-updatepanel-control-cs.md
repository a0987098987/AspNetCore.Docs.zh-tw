---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
title: 建立動畫 UpdatePanel 控制項 (C#) |Microsoft 文件
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 內容...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e57f8c7c-3940-4bc0-9468-3a0ca69158ea
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 5d8d5b9c3f15b39045b5e01b455bdddfc9443a24
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="animating-an-updatepanel-control-c"></a>建立動畫 UpdatePanel 控制項 (C#)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1CS.pdf)

> 動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 UpdatePanel 的內容，特殊的擴充項存在，高度依賴動畫 framework: UpdatePanelAnimation。 本教學課程會示範如何設定這類動畫的 UpdatePanel。


## <a name="overview"></a>總覽

動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 內容的`UpdatePanel`，特殊的擴充項存在，高度依賴動畫 framework: `UpdatePanelAnimation`。 本教學課程示範如何設定這類的動畫`UpdatePanel`。

## <a name="steps"></a>步驟

第一個步驟是包含如往常般`ScriptManager`頁面中，而 ASP.NET AJAX library 載入 Control Toolkit 可用：

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample1.aspx)]

在此案例中動畫會套用到 ASP.NET `Wizard` web 控制項位於`UpdatePanel`。 （任意） 的三個步驟會提供觸發回傳足夠選項：

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample2.aspx)]

所需的標記`UpdatePanelAnimationExtender`控制項是相當類似於使用的標記`AnimationExtender`。 在`TargetControlID`屬性我們提供`ID`的`UpdatePanel`以動畫方式顯示; 內`UpdatePanelAnimationExtender`控制項，`<Animations>`項目會保存 XML 標記的動畫。 但是沒有一項差異： 事件和事件處理常式的數量會限制於`AnimationExtender`。 如`UpdatePanels`，只有兩個其中存在：

- `<OnUpdated>` UpdatePanel 更新時
- `<OnUpdating>` UpdatePanel 開始更新

在此案例中，新的內容`UpdatePanel`（之後回傳） 應該會淡入。 這是必要的標記：

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample3.aspx)]

現在每次回傳發生 UpdatePanel 內時，新面板的內容會淡入順暢。


[![下一個精靈步驟淡入](animating-an-updatepanel-control-cs/_static/image2.png)](animating-an-updatepanel-control-cs/_static/image1.png)

下一個精靈步驟淡入 ([按一下以檢視完整大小的影像](animating-an-updatepanel-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](changing-an-animation-using-client-side-code-cs.md)
> [下一頁](dynamically-controlling-updatepanel-animations-cs.md)
