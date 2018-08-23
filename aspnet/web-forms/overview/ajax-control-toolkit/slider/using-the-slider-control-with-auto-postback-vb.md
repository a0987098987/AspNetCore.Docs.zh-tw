---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: 使用滑桿控制項具有自動回傳 (VB) |Microsoft Docs
author: wenz
description: 在 AJAX Control Toolkit 中的滑桿控制項提供的圖形化的滑桿，可以使用滑鼠來控制。 您可將滑桿張貼...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: 403e8fa6e54d84eb091769cbdb6ef1003ad4245c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835042"
---
<a name="using-the-slider-control-with-auto-postback-vb"></a><span data-ttu-id="c2b6b-104">使用滑桿控制項具有自動回傳 (VB)</span><span class="sxs-lookup"><span data-stu-id="c2b6b-104">Using the Slider Control With Auto-Postback (VB)</span></span>
====================
<span data-ttu-id="c2b6b-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c2b6b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c2b6b-106">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c2b6b-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span></span>

> <span data-ttu-id="c2b6b-107">在 AJAX Control Toolkit 中的滑桿控制項提供的圖形化的滑桿，可以使用滑鼠來控制。</span><span class="sxs-lookup"><span data-stu-id="c2b6b-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="c2b6b-108">可以變更滑桿 autopostback 一次其值。</span><span class="sxs-lookup"><span data-stu-id="c2b6b-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="c2b6b-109">總覽</span><span class="sxs-lookup"><span data-stu-id="c2b6b-109">Overview</span></span>

<span data-ttu-id="c2b6b-110">在 AJAX Control Toolkit 中的滑桿控制項提供的圖形化的滑桿，可以使用滑鼠來控制。</span><span class="sxs-lookup"><span data-stu-id="c2b6b-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="c2b6b-111">可以變更滑桿 autopostback 一次其值。</span><span class="sxs-lookup"><span data-stu-id="c2b6b-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="c2b6b-112">步驟</span><span class="sxs-lookup"><span data-stu-id="c2b6b-112">Steps</span></span>

<span data-ttu-id="c2b6b-113">若要讓發生變更時自動回傳的滑桿，這兩個文字方塊必須屬性`AutoPostBack="true"`： 文字方塊將成為本身，滑桿和保存滑桿的位置的文字方塊。</span><span class="sxs-lookup"><span data-stu-id="c2b6b-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="c2b6b-114">以下是該所需的標記：</span><span class="sxs-lookup"><span data-stu-id="c2b6b-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

<span data-ttu-id="c2b6b-115">`SliderExtender`從 ASP.NET AJAX Control Toolkit 控制項將滑桿功能指派給兩個文字方塊：</span><span class="sxs-lookup"><span data-stu-id="c2b6b-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

<span data-ttu-id="c2b6b-116">其他標籤項目，則稍後將用於通知使用者回傳：</span><span class="sxs-lookup"><span data-stu-id="c2b6b-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

<span data-ttu-id="c2b6b-117">最後， `ScriptManager` ASP.NET ajax 控制項載入必要的 JavaScript，才能控制工具組：</span><span class="sxs-lookup"><span data-stu-id="c2b6b-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

<span data-ttu-id="c2b6b-118">現在滑桿會回傳;伺服器端上可能會攔截及處理此事件：</span><span class="sxs-lookup"><span data-stu-id="c2b6b-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]


<span data-ttu-id="c2b6b-119">[![移動滑桿會觸發回傳](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c2b6b-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span></span>

<span data-ttu-id="c2b6b-120">移動滑桿會觸發回傳 ([按一下以檢視完整大小的影像](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c2b6b-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span></span>


<span data-ttu-id="c2b6b-121">[![之後，這項變更的日期是以標籤](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="c2b6b-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span></span>

<span data-ttu-id="c2b6b-122">之後，這項變更的日期以標籤 ([按一下以檢視完整大小的影像](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="c2b6b-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c2b6b-123">[上一頁](databinding-the-slider-control-cs.md)
> [下一頁](databinding-the-slider-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c2b6b-123">[Previous](databinding-the-slider-control-cs.md)
[Next](databinding-the-slider-control-vb.md)</span></span>
