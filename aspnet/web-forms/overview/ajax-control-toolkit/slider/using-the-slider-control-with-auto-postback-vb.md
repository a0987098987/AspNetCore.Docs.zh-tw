---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: 使用滑桿控制項具有自動回傳 (VB) |Microsoft Docs
author: wenz
description: 在 AJAX Control Toolkit 中的滑桿控制項提供的圖形化的滑桿，可以使用滑鼠來控制。 您可將滑桿張貼...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ddc5b119a7f58b4d289f11e1789cf193870ae4e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374035"
---
<a name="using-the-slider-control-with-auto-postback-vb"></a>使用滑桿控制項具有自動回傳 (VB)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)

> 在 AJAX Control Toolkit 中的滑桿控制項提供的圖形化的滑桿，可以使用滑鼠來控制。 可以變更滑桿 autopostback 一次其值。


## <a name="overview"></a>總覽

在 AJAX Control Toolkit 中的滑桿控制項提供的圖形化的滑桿，可以使用滑鼠來控制。 可以變更滑桿 autopostback 一次其值。

## <a name="steps"></a>步驟

若要讓發生變更時自動回傳的滑桿，這兩個文字方塊必須屬性`AutoPostBack="true"`： 文字方塊將成為本身，滑桿和保存滑桿的位置的文字方塊。 以下是該所需的標記：

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

`SliderExtender`從 ASP.NET AJAX Control Toolkit 控制項將滑桿功能指派給兩個文字方塊：

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

其他標籤項目，則稍後將用於通知使用者回傳：

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

最後， `ScriptManager` ASP.NET ajax 控制項載入必要的 JavaScript，才能控制工具組：

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

現在滑桿會回傳;伺服器端上可能會攔截及處理此事件：

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]


[![移動滑桿會觸發回傳](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)

移動滑桿會觸發回傳 ([按一下以檢視完整大小的影像](using-the-slider-control-with-auto-postback-vb/_static/image3.png))


[![之後，這項變更的日期是以標籤](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)

之後，這項變更的日期以標籤 ([按一下以檢視完整大小的影像](using-the-slider-control-with-auto-postback-vb/_static/image6.png))

> [!div class="step-by-step"]
> [上一頁](databinding-the-slider-control-cs.md)
> [下一頁](databinding-the-slider-control-vb.md)
