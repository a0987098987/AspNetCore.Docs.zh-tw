---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: 根據條件 (C#) 動畫 |Microsoft 文件
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 動畫的是否...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: b530239e76654bc68a8fa6ac900a20df1d5699b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="animation-depending-on-a-condition-c"></a><span data-ttu-id="bbb84-104">動畫根據條件 (C#)</span><span class="sxs-lookup"><span data-stu-id="bbb84-104">Animation Depending On a Condition (C#)</span></span>
====================
<span data-ttu-id="bbb84-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="bbb84-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="bbb84-106">[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="bbb84-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)</span></span>

> <span data-ttu-id="bbb84-107">動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="bbb84-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="bbb84-108">動畫執行時或不可以也會取決於一些 JavaScript 程式碼的表單中的條件。</span><span class="sxs-lookup"><span data-stu-id="bbb84-108">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="bbb84-109">總覽</span><span class="sxs-lookup"><span data-stu-id="bbb84-109">Overview</span></span>

<span data-ttu-id="bbb84-110">動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="bbb84-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="bbb84-111">動畫執行時或不可以也會取決於一些 JavaScript 程式碼的表單中的條件。</span><span class="sxs-lookup"><span data-stu-id="bbb84-111">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="bbb84-112">步驟</span><span class="sxs-lookup"><span data-stu-id="bbb84-112">Steps</span></span>

<span data-ttu-id="bbb84-113">首先，包括`ScriptManager`在頁面; 然後，ASP.NET AJAX 程式庫載入，因此您能夠使用控制項的工具組：</span><span class="sxs-lookup"><span data-stu-id="bbb84-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

<span data-ttu-id="bbb84-114">動畫會套用到面板中的文字看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="bbb84-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

<span data-ttu-id="bbb84-115">在 [面板] 中相關聯的 CSS 類別，定義好的背景色彩，也將面板固定的寬度設定：</span><span class="sxs-lookup"><span data-stu-id="bbb84-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

<span data-ttu-id="bbb84-116">接著，新增`AnimationExtender`到頁面上，提供`ID`、`TargetControlID`屬性和必要 `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="bbb84-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

<span data-ttu-id="bbb84-117">內`<Animations>`節點，請使用`<OnLoad>`頁面已完全載入後，執行動畫。</span><span class="sxs-lookup"><span data-stu-id="bbb84-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="bbb84-118">其中一個規則的動畫，而不是`<Condition>`元素派上用場。</span><span class="sxs-lookup"><span data-stu-id="bbb84-118">Instead of one of the regular animations, the `<Condition>` element comes into play.</span></span> <span data-ttu-id="bbb84-119">提供的值為 JavaScript 程式碼`ConditionScript`屬性會在執行階段執行。</span><span class="sxs-lookup"><span data-stu-id="bbb84-119">The JavaScript code provided as the value of the `ConditionScript` attribute is executed at runtime.</span></span> <span data-ttu-id="bbb84-120">如果評估為 true，動畫會執行，否則不。</span><span class="sxs-lookup"><span data-stu-id="bbb84-120">If it evaluates to true, the animation is executed, otherwise not.</span></span> <span data-ttu-id="bbb84-121">下列標記會提供兩個動畫，每個以 50%的隨機時的情況下執行。</span><span class="sxs-lookup"><span data-stu-id="bbb84-121">The following markup provides two animations, each of them being executed in 50% of cases upon random.</span></span> <span data-ttu-id="bbb84-122">因為只能有一個動畫內`<OnLoad>`，兩個`<Condition>`動畫聯結在一起使用`<Sequence>`項目：</span><span class="sxs-lookup"><span data-stu-id="bbb84-122">Since there may only be one animation within `<OnLoad>`, the two `<Condition>` animations are joined together using the `<Sequence>` element:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

<span data-ttu-id="bbb84-123">請注意，小於符號 (`<`) 中`ConditionScript`屬性必須是逸出 （）。</span><span class="sxs-lookup"><span data-stu-id="bbb84-123">Note that the less than sign (`<`) in the `ConditionScript` attribute must be escaped ().</span></span> <span data-ttu-id="bbb84-124">當您執行此指令碼，可能是沒有動畫執行時，或兩者的其中一個存在，或同時執行。</span><span class="sxs-lookup"><span data-stu-id="bbb84-124">When you run this script, either no animation runs, or one of the two does, or both do.</span></span>


<span data-ttu-id="bbb84-125">[![面板淡出沒有調整大小，因此第二個動畫執行時，第一個未](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bbb84-125">[![The panel is fading out without resizing, so the second animation runs, the first one didn't](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)</span></span>

<span data-ttu-id="bbb84-126">面板淡出沒有調整大小，因此未在第二個動畫執行，第一個 ([按一下以檢視完整大小的影像](animation-depending-on-a-condition-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="bbb84-126">The panel is fading out without resizing, so the second animation runs, the first one didn't ([Click to view full-size image](animation-depending-on-a-condition-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bbb84-127">[上一頁](executing-several-animations-after-each-other-cs.md)
> [下一頁](picking-one-animation-out-of-a-list-cs.md)</span><span class="sxs-lookup"><span data-stu-id="bbb84-127">[Previous](executing-several-animations-after-each-other-cs.md)
[Next](picking-one-animation-out-of-a-list-cs.md)</span></span>
