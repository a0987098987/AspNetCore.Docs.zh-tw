---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
title: 資料繫結滑桿控制項 (VB) |Microsoft Docs
author: wenz
description: 在 AJAX Control Toolkit 中的滑桿控制項提供的圖形化的滑桿，可以使用滑鼠來控制。 它也可以繫結目前 positio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4f3ba53f-d166-422d-b29c-403348057836
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
msc.type: authoredcontent
ms.openlocfilehash: aeaca2ebf61f49a5c081a3a1df188aa1541192d9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376303"
---
<a name="databinding-the-slider-control-vb"></a>資料繫結滑桿控制項 (VB)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)

> 在 AJAX Control Toolkit 中的滑桿控制項提供的圖形化的滑桿，可以使用滑鼠來控制。 可以將滑桿的目前位置到另一個的 ASP.NET 控制項繫結。


## <a name="overview"></a>總覽

在 AJAX Control Toolkit 中的滑桿控制項提供的圖形化的滑桿，可以使用滑鼠來控制。 可以將滑桿的目前位置到另一個的 ASP.NET 控制項繫結。

## <a name="steps"></a>步驟

若要啟動的 ASP.NET AJAX Control Toolkit 中，功能`ScriptManager`控制項必須放置在任何位置上 (但在`<form>`項目):

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample1.aspx)]

接下來，新增兩個`TextBox`至網頁的控制項。 其中一個會轉換成圖形化的滑桿，按住不放，另一個將滑桿的位置。

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample2.aspx)]

下一個步驟已經是最後一個步驟。 `SliderExtender`從 ASP.NET AJAX Control Toolkit 控制項讓滑桿移出第一個文字方塊和滑桿的位置變更時，自動更新第二個文字方塊。 為了使工作時，`SliderExtender`的`TargetControlID`屬性必須設為識別碼的第一個文字方塊;`BoundControlID`屬性必須設為第二個文字方塊的識別碼。

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample3.aspx)]

您可以看到瀏覽器中，資料繫結可以雙向運作： 在文字方塊中輸入新值更新滑桿的位置。 如果您進行第二個唯讀的文字方塊中，您可能加入弱式保護的文字欄位，以便讓使用者以手動方式更新裡面的值更難。


[![滑桿和文字方塊都保持同步](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)

滑桿和文字方塊都是同步 ([按一下以檢視完整大小的影像](databinding-the-slider-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一步](using-the-slider-control-with-auto-postback-vb.md)
