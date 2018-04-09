---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: 執行數個動畫之後彼此 (C#) |Microsoft 文件
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 它可讓執行 severa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: 836f0bba890a03e74ae62c2df029b7525b34275c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="executing-several-animations-after-each-other-c"></a><span data-ttu-id="bd8c6-104">執行數個動畫之後彼此 (C#)</span><span class="sxs-lookup"><span data-stu-id="bd8c6-104">Executing Several Animations after Each Other (C#)</span></span>
====================
<span data-ttu-id="bd8c6-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="bd8c6-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="bd8c6-106">[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="bd8c6-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span></span>

> <span data-ttu-id="bd8c6-107">動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="bd8c6-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="bd8c6-108">它可讓執行數個動畫一個接著一個。</span><span class="sxs-lookup"><span data-stu-id="bd8c6-108">It allows to run several animations one after the other.</span></span>


<span data-ttu-id="bd8c6-109">動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="bd8c6-109">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="bd8c6-110">它可讓執行數個動畫一個接著一個。</span><span class="sxs-lookup"><span data-stu-id="bd8c6-110">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="bd8c6-111">步驟</span><span class="sxs-lookup"><span data-stu-id="bd8c6-111">Steps</span></span>

<span data-ttu-id="bd8c6-112">首先，包括`ScriptManager`在頁面; 然後，ASP.NET AJAX 程式庫載入，因此您能夠使用控制項的工具組：</span><span class="sxs-lookup"><span data-stu-id="bd8c6-112">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

<span data-ttu-id="bd8c6-113">動畫會套用到面板中的文字看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="bd8c6-113">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

<span data-ttu-id="bd8c6-114">在 [面板] 中相關聯的 CSS 類別，定義好的背景色彩，也將面板固定的寬度設定：</span><span class="sxs-lookup"><span data-stu-id="bd8c6-114">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

<span data-ttu-id="bd8c6-115">接著，新增`AnimationExtender`到頁面上，提供`ID`、`TargetControlID`屬性和必要 `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="bd8c6-115">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

<span data-ttu-id="bd8c6-116">內`<Animations>`節點，請使用`<OnLoad>`頁面已完全載入後，執行動畫。</span><span class="sxs-lookup"><span data-stu-id="bd8c6-116">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="bd8c6-117">一般而言，`<OnLoad>`只接受一個動畫。</span><span class="sxs-lookup"><span data-stu-id="bd8c6-117">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="bd8c6-118">動畫架構可讓您加入到另一種使用數個動畫`<Sequence>`項目。</span><span class="sxs-lookup"><span data-stu-id="bd8c6-118">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="bd8c6-119">中的所有動畫`<Sequence>`會執行的一個接著一個。</span><span class="sxs-lookup"><span data-stu-id="bd8c6-119">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="bd8c6-120">以下是可能的標記為`AnimationExtender`控制項第一次進行更多面板，並再降低其高度：</span><span class="sxs-lookup"><span data-stu-id="bd8c6-120">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

<span data-ttu-id="bd8c6-121">當您執行此指令碼，將 [面板] 中第一次取得較寬且然後較小。</span><span class="sxs-lookup"><span data-stu-id="bd8c6-121">When you run this script, the panel first gets wider and then smaller.</span></span>


<span data-ttu-id="bd8c6-122">[![寬度會增加第一次](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bd8c6-122">[![First the width is increased](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)</span></span>

<span data-ttu-id="bd8c6-123">寬度會增加第一次 ([按一下以檢視完整大小的影像](executing-several-animations-after-each-other-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="bd8c6-123">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image3.png))</span></span>


<span data-ttu-id="bd8c6-124">[![然後就會減少高度](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="bd8c6-124">[![Then the height is decreased](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)</span></span>

<span data-ttu-id="bd8c6-125">然後就會減少高度 ([按一下以檢視完整大小的影像](executing-several-animations-after-each-other-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="bd8c6-125">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bd8c6-126">[上一頁](executing-several-animations-at-the-same-time-cs.md)
> [下一頁](animation-depending-on-a-condition-cs.md)</span><span class="sxs-lookup"><span data-stu-id="bd8c6-126">[Previous](executing-several-animations-at-the-same-time-cs.md)
[Next](animation-depending-on-a-condition-cs.md)</span></span>
