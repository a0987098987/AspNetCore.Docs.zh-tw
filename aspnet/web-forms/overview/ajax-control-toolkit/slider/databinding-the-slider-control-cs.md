---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: "資料繫結滑桿控制項 (C#) |Microsoft 文件"
author: wenz
description: "滑桿控制項在 AJAX Control Toolkit 提供圖形化的滑桿，可以使用滑鼠來控制。 這是可以繫結目前 positio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 2aa770bce5969a4ab57893d5ceecc127cf7f7872
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="databinding-the-slider-control-c"></a><span data-ttu-id="4c744-104">資料繫結滑桿控制項 (C#)</span><span class="sxs-lookup"><span data-stu-id="4c744-104">Databinding the Slider Control (C#)</span></span>
====================
<span data-ttu-id="4c744-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4c744-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4c744-106">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="4c744-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span></span>

> <span data-ttu-id="4c744-107">滑桿控制項在 AJAX Control Toolkit 提供圖形化的滑桿，可以使用滑鼠來控制。</span><span class="sxs-lookup"><span data-stu-id="4c744-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="4c744-108">您可將繫結至其他 ASP.NET 控制項的滑桿的目前位置。</span><span class="sxs-lookup"><span data-stu-id="4c744-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>


## <a name="overview"></a><span data-ttu-id="4c744-109">概觀</span><span class="sxs-lookup"><span data-stu-id="4c744-109">Overview</span></span>

<span data-ttu-id="4c744-110">滑桿控制項在 AJAX Control Toolkit 提供圖形化的滑桿，可以使用滑鼠來控制。</span><span class="sxs-lookup"><span data-stu-id="4c744-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="4c744-111">您可將繫結至其他 ASP.NET 控制項的滑桿的目前位置。</span><span class="sxs-lookup"><span data-stu-id="4c744-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="4c744-112">步驟</span><span class="sxs-lookup"><span data-stu-id="4c744-112">Steps</span></span>

<span data-ttu-id="4c744-113">為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`控制項必須任意位置放置在頁面 (但內部`<form>`項目):</span><span class="sxs-lookup"><span data-stu-id="4c744-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

<span data-ttu-id="4c744-114">接下來，加入兩個`TextBox`至頁面的控制項。</span><span class="sxs-lookup"><span data-stu-id="4c744-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="4c744-115">其中一個會轉換成圖形化的滑桿，按住不放，另一個將滑桿的位置。</span><span class="sxs-lookup"><span data-stu-id="4c744-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

<span data-ttu-id="4c744-116">下一個步驟已是最後一個步驟。</span><span class="sxs-lookup"><span data-stu-id="4c744-116">The next step is already the final step.</span></span> <span data-ttu-id="4c744-117">`SliderExtender`控制項從 ASP.NET AJAX Control Toolkit 滑桿從第一個文字方塊並滑桿位置變更時，自動更新第二個文字方塊。</span><span class="sxs-lookup"><span data-stu-id="4c744-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="4c744-118">為了讓，才能執行，`SliderExtender`的`TargetControlID`屬性必須設為第一個文字方塊中; 識別碼`BoundControlID`屬性必須設為第二個文字方塊中的識別碼。</span><span class="sxs-lookup"><span data-stu-id="4c744-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

<span data-ttu-id="4c744-119">您可以看到在瀏覽器中，資料繫結的雙向運作方式： 在文字方塊中輸入新值更新滑桿的位置。</span><span class="sxs-lookup"><span data-stu-id="4c744-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="4c744-120">如果您進行第二個唯讀的文字方塊中，您可以加入弱式保護的文字欄位使其具備要手動更新中有值的使用者不容易。</span><span class="sxs-lookup"><span data-stu-id="4c744-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>


<span data-ttu-id="4c744-121">[![滑桿和文字方塊會同步](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4c744-121">[![Slider and text box are in sync](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="4c744-122">滑桿和文字方塊為同步 ([按一下以檢視完整大小的影像](databinding-the-slider-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4c744-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="4c744-123">[上一頁](using-the-slider-control-with-auto-postback-cs.md)
[下一頁](using-the-slider-control-with-auto-postback-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4c744-123">[Previous](using-the-slider-control-with-auto-postback-cs.md)
[Next](using-the-slider-control-with-auto-postback-vb.md)</span></span>
