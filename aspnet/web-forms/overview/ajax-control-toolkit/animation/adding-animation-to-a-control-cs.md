---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: 將動畫加入至控制項 (C#) |Microsoft Docs
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 本教學課程示範如何...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: aa977883af931bb74b791104cf4ee3212079e43a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835023"
---
<a name="adding-animation-to-a-control-c"></a><span data-ttu-id="a727c-104">將動畫加入至控制項 (C#)</span><span class="sxs-lookup"><span data-stu-id="a727c-104">Adding Animation to a Control (C#)</span></span>
====================
<span data-ttu-id="a727c-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a727c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a727c-106">[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="a727c-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span></span>

> <span data-ttu-id="a727c-107">動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="a727c-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a727c-108">本教學課程會示範如何設定這類動畫。</span><span class="sxs-lookup"><span data-stu-id="a727c-108">This tutorial shows how to set up such an animation.</span></span>


## <a name="overview"></a><span data-ttu-id="a727c-109">總覽</span><span class="sxs-lookup"><span data-stu-id="a727c-109">Overview</span></span>

<span data-ttu-id="a727c-110">動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="a727c-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a727c-111">本教學課程會示範如何設定這類動畫。</span><span class="sxs-lookup"><span data-stu-id="a727c-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="a727c-112">步驟</span><span class="sxs-lookup"><span data-stu-id="a727c-112">Steps</span></span>

<span data-ttu-id="a727c-113">如往常般的第一個步驟是加入`ScriptManager`頁面中，讓 ASP.NET AJAX 程式庫已載入，並控制工具組可以用於：</span><span class="sxs-lookup"><span data-stu-id="a727c-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="a727c-114">在此案例中的動畫將會套用至面板的文字看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="a727c-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="a727c-115">面板相關聯的 CSS 類別定義的背景色彩和寬度：</span><span class="sxs-lookup"><span data-stu-id="a727c-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

<span data-ttu-id="a727c-116">接下來，我們需要`AnimationExtender`。</span><span class="sxs-lookup"><span data-stu-id="a727c-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="a727c-117">提供之後`ID`和一般`runat="server"`，則`TargetControlID`屬性必須設定為控制項，以在我們的案例中，[面板] 中建立動畫：</span><span class="sxs-lookup"><span data-stu-id="a727c-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="a727c-118">整個套用該動畫以宣告方式，使用 XML 語法，不幸的是目前不完全受到 Visual Studio's IntelliSense。</span><span class="sxs-lookup"><span data-stu-id="a727c-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="a727c-119">根節點是`<Animations>;`此節點中，內有多個事件允許用來決定當 animation(s) take(s) 位置：</span><span class="sxs-lookup"><span data-stu-id="a727c-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="a727c-120">`OnClick` （按一下滑鼠）</span><span class="sxs-lookup"><span data-stu-id="a727c-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="a727c-121">`OnHoverOut` （當滑鼠離開控制項）</span><span class="sxs-lookup"><span data-stu-id="a727c-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="a727c-122">`OnHoverOver` (當滑鼠停留在控制項時，停止`OnHoverOut`動畫)</span><span class="sxs-lookup"><span data-stu-id="a727c-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="a727c-123">`OnLoad` （當頁面已載入）</span><span class="sxs-lookup"><span data-stu-id="a727c-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="a727c-124">`OnMouseOut` （當滑鼠離開控制項）</span><span class="sxs-lookup"><span data-stu-id="a727c-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="a727c-125">`OnMouseOver` (當滑鼠停留在控制項時，不會 」 停止`OnMouseOut`動畫)</span><span class="sxs-lookup"><span data-stu-id="a727c-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="a727c-126">此架構隨附一組動畫，由它自己的 XML 項目表示每一個。</span><span class="sxs-lookup"><span data-stu-id="a727c-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="a727c-127">以下是選取項目：</span><span class="sxs-lookup"><span data-stu-id="a727c-127">Here is a selection:</span></span>

- <span data-ttu-id="a727c-128">`<Color>` （變更色彩）</span><span class="sxs-lookup"><span data-stu-id="a727c-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="a727c-129">`<FadeIn>` （淡入）</span><span class="sxs-lookup"><span data-stu-id="a727c-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="a727c-130">`<FadeOut>` （淡出）</span><span class="sxs-lookup"><span data-stu-id="a727c-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="a727c-131">`<Property>` （變更控制項的屬性）</span><span class="sxs-lookup"><span data-stu-id="a727c-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="a727c-132">`<Pulse>` (pulsating)</span><span class="sxs-lookup"><span data-stu-id="a727c-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="a727c-133">`<Resize>` （變更大小）</span><span class="sxs-lookup"><span data-stu-id="a727c-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="a727c-134">`<Scale>` （按比例變更大小）</span><span class="sxs-lookup"><span data-stu-id="a727c-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="a727c-135">在此範例中，[面板] 中應該會淡出。動畫應採用 1.5 秒 (`Duration`屬性)，顯示每秒 24 畫面格數 （動畫步驟） (`Fps` attributs)。</span><span class="sxs-lookup"><span data-stu-id="a727c-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attributs).</span></span> <span data-ttu-id="a727c-136">以下是完成標記`AnimationExtender`控制項：</span><span class="sxs-lookup"><span data-stu-id="a727c-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="a727c-137">當您執行此指令碼時，面板會顯示，而且在一個半秒內淡出。</span><span class="sxs-lookup"><span data-stu-id="a727c-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>


<span data-ttu-id="a727c-138">[![面板淡出](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a727c-138">[![The panel is fading out](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="a727c-139">面板淡出 ([按一下以檢視完整大小的影像](adding-animation-to-a-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a727c-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a727c-140">下一步</span><span class="sxs-lookup"><span data-stu-id="a727c-140">Next</span></span>](executing-several-animations-at-the-same-time-cs.md)
