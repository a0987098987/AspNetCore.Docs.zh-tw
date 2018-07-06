---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
title: 資料繫結滑桿控制項 (VB) |Microsoft Docs
author: wenz
description: 在 AJAX Control Toolkit 中的滑桿控制項提供的圖形化的滑桿，可以使用滑鼠來控制。 它也可以繫結目前 positio...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 4f3ba53f-d166-422d-b29c-403348057836
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
msc.type: authoredcontent
ms.openlocfilehash: c55ba530bbe7eee7c02e5dfb2e80dddafabc9325
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836424"
---
<a name="databinding-the-slider-control-vb"></a><span data-ttu-id="cba60-104">資料繫結滑桿控制項 (VB)</span><span class="sxs-lookup"><span data-stu-id="cba60-104">Databinding the Slider Control (VB)</span></span>
====================
<span data-ttu-id="cba60-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="cba60-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="cba60-106">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="cba60-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)</span></span>

> <span data-ttu-id="cba60-107">在 AJAX Control Toolkit 中的滑桿控制項提供的圖形化的滑桿，可以使用滑鼠來控制。</span><span class="sxs-lookup"><span data-stu-id="cba60-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="cba60-108">可以將滑桿的目前位置到另一個的 ASP.NET 控制項繫結。</span><span class="sxs-lookup"><span data-stu-id="cba60-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>


## <a name="overview"></a><span data-ttu-id="cba60-109">總覽</span><span class="sxs-lookup"><span data-stu-id="cba60-109">Overview</span></span>

<span data-ttu-id="cba60-110">在 AJAX Control Toolkit 中的滑桿控制項提供的圖形化的滑桿，可以使用滑鼠來控制。</span><span class="sxs-lookup"><span data-stu-id="cba60-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="cba60-111">可以將滑桿的目前位置到另一個的 ASP.NET 控制項繫結。</span><span class="sxs-lookup"><span data-stu-id="cba60-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="cba60-112">步驟</span><span class="sxs-lookup"><span data-stu-id="cba60-112">Steps</span></span>

<span data-ttu-id="cba60-113">若要啟動的 ASP.NET AJAX Control Toolkit 中，功能`ScriptManager`控制項必須放置在任何位置上 (但在`<form>`項目):</span><span class="sxs-lookup"><span data-stu-id="cba60-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample1.aspx)]

<span data-ttu-id="cba60-114">接下來，新增兩個`TextBox`至網頁的控制項。</span><span class="sxs-lookup"><span data-stu-id="cba60-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="cba60-115">其中一個會轉換成圖形化的滑桿，按住不放，另一個將滑桿的位置。</span><span class="sxs-lookup"><span data-stu-id="cba60-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample2.aspx)]

<span data-ttu-id="cba60-116">下一個步驟已經是最後一個步驟。</span><span class="sxs-lookup"><span data-stu-id="cba60-116">The next step is already the final step.</span></span> <span data-ttu-id="cba60-117">`SliderExtender`從 ASP.NET AJAX Control Toolkit 控制項讓滑桿移出第一個文字方塊和滑桿的位置變更時，自動更新第二個文字方塊。</span><span class="sxs-lookup"><span data-stu-id="cba60-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="cba60-118">為了使工作時，`SliderExtender`的`TargetControlID`屬性必須設為識別碼的第一個文字方塊;`BoundControlID`屬性必須設為第二個文字方塊的識別碼。</span><span class="sxs-lookup"><span data-stu-id="cba60-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample3.aspx)]

<span data-ttu-id="cba60-119">您可以看到瀏覽器中，資料繫結可以雙向運作： 在文字方塊中輸入新值更新滑桿的位置。</span><span class="sxs-lookup"><span data-stu-id="cba60-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="cba60-120">如果您進行第二個唯讀的文字方塊中，您可能加入弱式保護的文字欄位，以便讓使用者以手動方式更新裡面的值更難。</span><span class="sxs-lookup"><span data-stu-id="cba60-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>


<span data-ttu-id="cba60-121">[![滑桿和文字方塊都保持同步](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="cba60-121">[![Slider and text box are in sync](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="cba60-122">滑桿和文字方塊都是同步 ([按一下以檢視完整大小的影像](databinding-the-slider-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="cba60-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="cba60-123">上一步</span><span class="sxs-lookup"><span data-stu-id="cba60-123">Previous</span></span>](using-the-slider-control-with-auto-postback-vb.md)
