---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: 執行數個動畫之後彼此 (C#) |Microsoft Docs
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 它可讓執行 severa...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: 2317a029d4b12ba66d2d06e5012bb0bf892086a3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807599"
---
<a name="executing-several-animations-after-each-other-c"></a><span data-ttu-id="3a049-104">執行數個動畫之後彼此 (C#)</span><span class="sxs-lookup"><span data-stu-id="3a049-104">Executing Several Animations after Each Other (C#)</span></span>
====================
<span data-ttu-id="3a049-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3a049-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3a049-106">[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="3a049-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span></span>

> <span data-ttu-id="3a049-107">動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="3a049-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="3a049-108">它可讓執行數個動畫一個接著一個。</span><span class="sxs-lookup"><span data-stu-id="3a049-108">It allows to run several animations one after the other.</span></span>


<span data-ttu-id="3a049-109">動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="3a049-109">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="3a049-110">它可讓執行數個動畫一個接著一個。</span><span class="sxs-lookup"><span data-stu-id="3a049-110">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="3a049-111">步驟</span><span class="sxs-lookup"><span data-stu-id="3a049-111">Steps</span></span>

<span data-ttu-id="3a049-112">首先，包括`ScriptManager`單元頁面; 然後，ASP.NET AJAX 程式庫載入，因此能夠使用控制項工具組：</span><span class="sxs-lookup"><span data-stu-id="3a049-112">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

<span data-ttu-id="3a049-113">動畫將會套用至面板的文字看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="3a049-113">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

<span data-ttu-id="3a049-114">在 [面板] 中相關聯的 CSS 類別，定義好用的背景色彩和也設定面板的固定的寬度：</span><span class="sxs-lookup"><span data-stu-id="3a049-114">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

<span data-ttu-id="3a049-115">然後，新增`AnimationExtender` 頁面上，以提供`ID`，則`TargetControlID`屬性和必要 `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="3a049-115">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

<span data-ttu-id="3a049-116">內`<Animations>`節點，請使用`<OnLoad>`頁面完全載入後，執行動畫。</span><span class="sxs-lookup"><span data-stu-id="3a049-116">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="3a049-117">一般而言，`<OnLoad>`只接受一個動畫。</span><span class="sxs-lookup"><span data-stu-id="3a049-117">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="3a049-118">動畫架構可讓您加入到另一種使用數個動畫`<Sequence>`項目。</span><span class="sxs-lookup"><span data-stu-id="3a049-118">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="3a049-119">中的所有動畫`<Sequence>`會執行的一個接著一個。</span><span class="sxs-lookup"><span data-stu-id="3a049-119">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="3a049-120">以下是可能的標記如`AnimationExtender`控制項，第一次進行更多面板，並再降低其高度：</span><span class="sxs-lookup"><span data-stu-id="3a049-120">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

<span data-ttu-id="3a049-121">當您執行此指令碼，面板寬度，然後較小的第一次取得。</span><span class="sxs-lookup"><span data-stu-id="3a049-121">When you run this script, the panel first gets wider and then smaller.</span></span>


<span data-ttu-id="3a049-122">[![寬度增加的第一次](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3a049-122">[![First the width is increased](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)</span></span>

<span data-ttu-id="3a049-123">寬度增加的第一次 ([按一下以檢視完整大小的影像](executing-several-animations-after-each-other-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3a049-123">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image3.png))</span></span>


<span data-ttu-id="3a049-124">[![然後就會減少高度](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="3a049-124">[![Then the height is decreased](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)</span></span>

<span data-ttu-id="3a049-125">然後就會減少高度 ([按一下以檢視完整大小的影像](executing-several-animations-after-each-other-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="3a049-125">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3a049-126">[上一頁](executing-several-animations-at-the-same-time-cs.md)
> [下一頁](animation-depending-on-a-condition-cs.md)</span><span class="sxs-lookup"><span data-stu-id="3a049-126">[Previous](executing-several-animations-at-the-same-time-cs.md)
[Next](animation-depending-on-a-condition-cs.md)</span></span>
