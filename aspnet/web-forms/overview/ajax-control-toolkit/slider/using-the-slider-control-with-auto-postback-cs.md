---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: 使用滑桿控制項具有自動回傳 (C#) |Microsoft 文件
author: wenz
description: 滑桿控制項在 AJAX Control Toolkit 提供圖形化的滑桿，可以使用滑鼠來控制。 它可能是進行滑桿張貼...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: e347d20c5c2ee48e6ed801e95459af6f0bcd2667
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879240"
---
<a name="using-the-slider-control-with-auto-postback-c"></a>使用滑桿控制項具有自動回傳 (C#)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)

> 滑桿控制項在 AJAX Control Toolkit 提供圖形化的滑桿，可以使用滑鼠來控制。 它可能是進行滑桿 autopostback 一次其值變更。


## <a name="overview"></a>總覽

滑桿控制項在 AJAX Control Toolkit 提供圖形化的滑桿，可以使用滑鼠來控制。 它可能是進行滑桿 autopostback 一次其值變更。

## <a name="steps"></a>步驟

為了讓滑桿會自動回傳發生變更時，這兩個文字方塊需要屬性`AutoPostBack="true"`： 文字方塊在即將成為本身，滑桿和保存的滑桿位置的文字方塊。 以下是必要的標記：

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

`SliderExtender`控制項從 ASP.NET AJAX Control Toolkit 將滑桿功能指派給兩個文字方塊：

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

額外的標籤項目稍後將用於通知回傳使用者：

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

最後， `ScriptManager` ASP.NET ajax 控制項載入必要的 JavaScript 才能將控制項工具組：

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

現在滑桿會回傳。在伺服器端，此事件可能會攔截到和採用：

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


[![移動滑桿觸發回傳](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)

移動滑桿觸發回傳 ([按一下以檢視完整大小的影像](using-the-slider-control-with-auto-postback-cs/_static/image3.png))


[![這項變更的日期之後，以標籤](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)

這項變更的日期之後，以標籤 ([按一下以檢視完整大小的影像](using-the-slider-control-with-auto-postback-cs/_static/image6.png))

> [!div class="step-by-step"]
> [下一步](databinding-the-slider-control-cs.md)
