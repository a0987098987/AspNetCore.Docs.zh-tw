---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: 資料繫結滑桿控制項 (C#) |Microsoft Docs
author: wenz
description: 在 AJAX Control Toolkit 中的滑桿控制項提供的圖形化的滑桿，可以使用滑鼠來控制。 它也可以繫結目前 positio...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 951a7484f0dbb14ee7f1e1d62c9666e5cc49e7c1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823633"
---
<a name="databinding-the-slider-control-c"></a><span data-ttu-id="e3764-104">資料繫結滑桿控制項 (C#)</span><span class="sxs-lookup"><span data-stu-id="e3764-104">Databinding the Slider Control (C#)</span></span>
====================
<span data-ttu-id="e3764-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e3764-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e3764-106">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="e3764-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span></span>

> <span data-ttu-id="e3764-107">在 AJAX Control Toolkit 中的滑桿控制項提供的圖形化的滑桿，可以使用滑鼠來控制。</span><span class="sxs-lookup"><span data-stu-id="e3764-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="e3764-108">可以將滑桿的目前位置到另一個的 ASP.NET 控制項繫結。</span><span class="sxs-lookup"><span data-stu-id="e3764-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>


## <a name="overview"></a><span data-ttu-id="e3764-109">總覽</span><span class="sxs-lookup"><span data-stu-id="e3764-109">Overview</span></span>

<span data-ttu-id="e3764-110">在 AJAX Control Toolkit 中的滑桿控制項提供的圖形化的滑桿，可以使用滑鼠來控制。</span><span class="sxs-lookup"><span data-stu-id="e3764-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="e3764-111">可以將滑桿的目前位置到另一個的 ASP.NET 控制項繫結。</span><span class="sxs-lookup"><span data-stu-id="e3764-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="e3764-112">步驟</span><span class="sxs-lookup"><span data-stu-id="e3764-112">Steps</span></span>

<span data-ttu-id="e3764-113">若要啟動的 ASP.NET AJAX Control Toolkit 中，功能`ScriptManager`控制項必須放置在任何位置上 (但在`<form>`項目):</span><span class="sxs-lookup"><span data-stu-id="e3764-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

<span data-ttu-id="e3764-114">接下來，新增兩個`TextBox`至網頁的控制項。</span><span class="sxs-lookup"><span data-stu-id="e3764-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="e3764-115">其中一個會轉換成圖形化的滑桿，按住不放，另一個將滑桿的位置。</span><span class="sxs-lookup"><span data-stu-id="e3764-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

<span data-ttu-id="e3764-116">下一個步驟已經是最後一個步驟。</span><span class="sxs-lookup"><span data-stu-id="e3764-116">The next step is already the final step.</span></span> <span data-ttu-id="e3764-117">`SliderExtender`從 ASP.NET AJAX Control Toolkit 控制項讓滑桿移出第一個文字方塊和滑桿的位置變更時，自動更新第二個文字方塊。</span><span class="sxs-lookup"><span data-stu-id="e3764-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="e3764-118">為了使工作時，`SliderExtender`的`TargetControlID`屬性必須設為識別碼的第一個文字方塊;`BoundControlID`屬性必須設為第二個文字方塊的識別碼。</span><span class="sxs-lookup"><span data-stu-id="e3764-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

<span data-ttu-id="e3764-119">您可以看到瀏覽器中，資料繫結可以雙向運作： 在文字方塊中輸入新值更新滑桿的位置。</span><span class="sxs-lookup"><span data-stu-id="e3764-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="e3764-120">如果您進行第二個唯讀的文字方塊中，您可能加入弱式保護的文字欄位，以便讓使用者以手動方式更新裡面的值更難。</span><span class="sxs-lookup"><span data-stu-id="e3764-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>


<span data-ttu-id="e3764-121">[![滑桿和文字方塊都保持同步](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e3764-121">[![Slider and text box are in sync](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="e3764-122">滑桿和文字方塊都是同步 ([按一下以檢視完整大小的影像](databinding-the-slider-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e3764-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e3764-123">[上一頁](using-the-slider-control-with-auto-postback-cs.md)
> [下一頁](using-the-slider-control-with-auto-postback-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e3764-123">[Previous](using-the-slider-control-with-auto-postback-cs.md)
[Next](using-the-slider-control-with-auto-postback-vb.md)</span></span>
