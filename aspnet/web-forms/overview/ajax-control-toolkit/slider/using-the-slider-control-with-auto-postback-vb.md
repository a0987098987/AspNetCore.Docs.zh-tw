---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: 使用滑桿控制項具有自動回傳 (VB) |Microsoft 文件
author: wenz
description: 滑桿控制項在 AJAX Control Toolkit 提供圖形化的滑桿，可以使用滑鼠來控制。 它可能是進行滑桿張貼...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: edb8fa13716c3c0beb7cf86dd3843caaec939483
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="using-the-slider-control-with-auto-postback-vb"></a><span data-ttu-id="0ad1a-104">使用滑桿控制項具有自動回傳 (VB)</span><span class="sxs-lookup"><span data-stu-id="0ad1a-104">Using the Slider Control With Auto-Postback (VB)</span></span>
====================
<span data-ttu-id="0ad1a-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0ad1a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0ad1a-106">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="0ad1a-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span></span>

> <span data-ttu-id="0ad1a-107">滑桿控制項在 AJAX Control Toolkit 提供圖形化的滑桿，可以使用滑鼠來控制。</span><span class="sxs-lookup"><span data-stu-id="0ad1a-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="0ad1a-108">它可能是進行滑桿 autopostback 一次其值變更。</span><span class="sxs-lookup"><span data-stu-id="0ad1a-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="0ad1a-109">總覽</span><span class="sxs-lookup"><span data-stu-id="0ad1a-109">Overview</span></span>

<span data-ttu-id="0ad1a-110">滑桿控制項在 AJAX Control Toolkit 提供圖形化的滑桿，可以使用滑鼠來控制。</span><span class="sxs-lookup"><span data-stu-id="0ad1a-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="0ad1a-111">它可能是進行滑桿 autopostback 一次其值變更。</span><span class="sxs-lookup"><span data-stu-id="0ad1a-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="0ad1a-112">步驟</span><span class="sxs-lookup"><span data-stu-id="0ad1a-112">Steps</span></span>

<span data-ttu-id="0ad1a-113">為了讓滑桿會自動回傳發生變更時，這兩個文字方塊需要屬性`AutoPostBack="true"`： 文字方塊在即將成為本身，滑桿和保存的滑桿位置的文字方塊。</span><span class="sxs-lookup"><span data-stu-id="0ad1a-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="0ad1a-114">以下是必要的標記：</span><span class="sxs-lookup"><span data-stu-id="0ad1a-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

<span data-ttu-id="0ad1a-115">`SliderExtender`控制項從 ASP.NET AJAX Control Toolkit 將滑桿功能指派給兩個文字方塊：</span><span class="sxs-lookup"><span data-stu-id="0ad1a-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

<span data-ttu-id="0ad1a-116">額外的標籤項目稍後將用於通知回傳使用者：</span><span class="sxs-lookup"><span data-stu-id="0ad1a-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

<span data-ttu-id="0ad1a-117">最後， `ScriptManager` ASP.NET ajax 控制項載入必要的 JavaScript 才能將控制項工具組：</span><span class="sxs-lookup"><span data-stu-id="0ad1a-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

<span data-ttu-id="0ad1a-118">現在滑桿會回傳。在伺服器端，此事件可能會攔截到和採用：</span><span class="sxs-lookup"><span data-stu-id="0ad1a-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]


<span data-ttu-id="0ad1a-119">[![移動滑桿觸發回傳](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0ad1a-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span></span>

<span data-ttu-id="0ad1a-120">移動滑桿觸發回傳 ([按一下以檢視完整大小的影像](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0ad1a-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span></span>


<span data-ttu-id="0ad1a-121">[![這項變更的日期之後，以標籤](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="0ad1a-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span></span>

<span data-ttu-id="0ad1a-122">這項變更的日期之後，以標籤 ([按一下以檢視完整大小的影像](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="0ad1a-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0ad1a-123">[上一頁](databinding-the-slider-control-cs.md)
> [下一頁](databinding-the-slider-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="0ad1a-123">[Previous](databinding-the-slider-control-cs.md)
[Next](databinding-the-slider-control-vb.md)</span></span>
