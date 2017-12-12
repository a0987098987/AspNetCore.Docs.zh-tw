---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: "將動畫加入至控制項 (VB) |Microsoft 文件"
author: wenz
description: "動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 本教學課程示範如何..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: c2d6971ade89405245c8d23cafb6fd8bb9468639
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="adding-animation-to-a-control-vb"></a><span data-ttu-id="7a28f-104">將動畫加入至控制項 (VB)</span><span class="sxs-lookup"><span data-stu-id="7a28f-104">Adding Animation to a Control (VB)</span></span>
====================
<span data-ttu-id="7a28f-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7a28f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7a28f-106">[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="7a28f-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span></span>

> <span data-ttu-id="7a28f-107">動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="7a28f-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7a28f-108">本教學課程會示範如何設定這類動畫。</span><span class="sxs-lookup"><span data-stu-id="7a28f-108">This tutorial shows how to set up such an animation.</span></span>


## <a name="overview"></a><span data-ttu-id="7a28f-109">概觀</span><span class="sxs-lookup"><span data-stu-id="7a28f-109">Overview</span></span>

<span data-ttu-id="7a28f-110">動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="7a28f-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7a28f-111">本教學課程會示範如何設定這類動畫。</span><span class="sxs-lookup"><span data-stu-id="7a28f-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="7a28f-112">步驟</span><span class="sxs-lookup"><span data-stu-id="7a28f-112">Steps</span></span>

<span data-ttu-id="7a28f-113">第一個步驟是包含如往常般`ScriptManager`頁面中，而 ASP.NET AJAX library 載入 Control Toolkit 可用：</span><span class="sxs-lookup"><span data-stu-id="7a28f-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

<span data-ttu-id="7a28f-114">在此案例中動畫會套用到面板中的文字看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="7a28f-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

<span data-ttu-id="7a28f-115">面板的相關聯的 CSS 類別定義的背景色彩和寬度：</span><span class="sxs-lookup"><span data-stu-id="7a28f-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

<span data-ttu-id="7a28f-116">接下來，我們需要`AnimationExtender`。</span><span class="sxs-lookup"><span data-stu-id="7a28f-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="7a28f-117">提供後`ID`和一般`runat="server"`、`TargetControlID`屬性必須設為控制項，以動畫方式顯示在此案例中的面板：</span><span class="sxs-lookup"><span data-stu-id="7a28f-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

<span data-ttu-id="7a28f-118">整個動畫套用瞬間以宣告方式，使用 XML 語法，不幸的是 Visual Studio intellisense 目前不完全支援。</span><span class="sxs-lookup"><span data-stu-id="7a28f-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="7a28f-119">根節點是`<Animations>;`在此節點中，數個事件會允許用來決定當動畫 take(s) 位置：</span><span class="sxs-lookup"><span data-stu-id="7a28f-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="7a28f-120">`OnClick`（按一下滑鼠）</span><span class="sxs-lookup"><span data-stu-id="7a28f-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="7a28f-121">`OnHoverOut`（當滑鼠離開控制項）</span><span class="sxs-lookup"><span data-stu-id="7a28f-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="7a28f-122">`OnHoverOver`(當滑鼠停留在控制項上，停止`OnHoverOut`動畫)</span><span class="sxs-lookup"><span data-stu-id="7a28f-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="7a28f-123">`OnLoad`（當已經載入頁面）</span><span class="sxs-lookup"><span data-stu-id="7a28f-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="7a28f-124">`OnMouseOut`（當滑鼠離開控制項）</span><span class="sxs-lookup"><span data-stu-id="7a28f-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="7a28f-125">`OnMouseOver`(當滑鼠停留在控制項上時，不停止`OnMouseOut`動畫)</span><span class="sxs-lookup"><span data-stu-id="7a28f-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="7a28f-126">架構隨附一組自己的 XML 元素所代表的每個動畫。</span><span class="sxs-lookup"><span data-stu-id="7a28f-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="7a28f-127">以下是選取項目：</span><span class="sxs-lookup"><span data-stu-id="7a28f-127">Here is a selection:</span></span>

- <span data-ttu-id="7a28f-128">`<Color>`（變更一種色彩）</span><span class="sxs-lookup"><span data-stu-id="7a28f-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="7a28f-129">`<FadeIn>`（淡入）</span><span class="sxs-lookup"><span data-stu-id="7a28f-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="7a28f-130">`<FadeOut>`（淡出）</span><span class="sxs-lookup"><span data-stu-id="7a28f-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="7a28f-131">`<Property>`（變更控制項的屬性）</span><span class="sxs-lookup"><span data-stu-id="7a28f-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="7a28f-132">`<Pulse>`(pulsating)</span><span class="sxs-lookup"><span data-stu-id="7a28f-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="7a28f-133">`<Resize>`（變更大小）</span><span class="sxs-lookup"><span data-stu-id="7a28f-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="7a28f-134">`<Scale>`（按比例變更大小）</span><span class="sxs-lookup"><span data-stu-id="7a28f-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="7a28f-135">在此範例中，[面板] 中應該淡出。動畫應採用 1.5 秒 (`Duration`屬性)，顯示每秒 24 畫面格數 （動畫步驟） (`Fps` attributs)。</span><span class="sxs-lookup"><span data-stu-id="7a28f-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attributs).</span></span> <span data-ttu-id="7a28f-136">以下是完整標記`AnimationExtender`控制項：</span><span class="sxs-lookup"><span data-stu-id="7a28f-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

<span data-ttu-id="7a28f-137">當您執行此指令碼時，面板會顯示和淡出一個半秒。</span><span class="sxs-lookup"><span data-stu-id="7a28f-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>


<span data-ttu-id="7a28f-138">[![面板淡出](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7a28f-138">[![The panel is fading out](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="7a28f-139">面板淡出 ([按一下以檢視完整大小的影像](adding-animation-to-a-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7a28f-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="7a28f-140">[上一頁](dynamically-controlling-updatepanel-animations-cs.md)
[下一頁](executing-several-animations-at-the-same-time-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7a28f-140">[Previous](dynamically-controlling-updatepanel-animations-cs.md)
[Next](executing-several-animations-at-the-same-time-vb.md)</span></span>
