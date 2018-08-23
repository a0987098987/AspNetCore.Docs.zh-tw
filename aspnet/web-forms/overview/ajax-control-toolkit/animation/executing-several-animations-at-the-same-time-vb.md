---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
title: 在相同的時間 (VB) 中執行數個動畫 |Microsoft Docs
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 它可讓執行 severa...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2469f7ea-1489-42fb-a8e1-414c90141ce9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
msc.type: authoredcontent
ms.openlocfilehash: 20baa40c34dd8c8907b940764987441bc7a91da9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834188"
---
<a name="executing-several-animations-at-the-same-time-vb"></a><span data-ttu-id="d5692-104">執行數個動畫在相同的時間 (VB)</span><span class="sxs-lookup"><span data-stu-id="d5692-104">Executing Several Animations at The Same Time (VB)</span></span>
====================
<span data-ttu-id="d5692-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d5692-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d5692-106">[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="d5692-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)</span></span>

> <span data-ttu-id="d5692-107">動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="d5692-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="d5692-108">它可讓以平行方式執行數個動畫。</span><span class="sxs-lookup"><span data-stu-id="d5692-108">It allows to run several animations in a parallel fashion.</span></span>


## <a name="overview"></a><span data-ttu-id="d5692-109">總覽</span><span class="sxs-lookup"><span data-stu-id="d5692-109">Overview</span></span>

<span data-ttu-id="d5692-110">動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="d5692-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="d5692-111">它可讓以平行方式執行數個動畫。</span><span class="sxs-lookup"><span data-stu-id="d5692-111">It allows to run several animations in a parallel fashion.</span></span>

## <a name="steps"></a><span data-ttu-id="d5692-112">步驟</span><span class="sxs-lookup"><span data-stu-id="d5692-112">Steps</span></span>

<span data-ttu-id="d5692-113">首先，包括`ScriptManager`單元頁面; 然後，ASP.NET AJAX 程式庫載入，因此能夠使用控制項工具組：</span><span class="sxs-lookup"><span data-stu-id="d5692-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample1.aspx)]

<span data-ttu-id="d5692-114">動畫將會套用至面板的文字看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="d5692-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample2.aspx)]

<span data-ttu-id="d5692-115">在 [面板] 中相關聯的 CSS 類別，定義好用的背景色彩和也設定面板的固定的寬度：</span><span class="sxs-lookup"><span data-stu-id="d5692-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-at-the-same-time-vb/samples/sample3.css)]

<span data-ttu-id="d5692-116">然後，新增`AnimationExtender` 頁面上，以提供`ID`，則`TargetControlID`屬性和必要`runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="d5692-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample4.aspx)]

<span data-ttu-id="d5692-117">內`<Animations>`節點，請使用`<OnLoad>`頁面完全載入後，執行動畫。</span><span class="sxs-lookup"><span data-stu-id="d5692-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="d5692-118">一般而言，`<OnLoad>`只接受一個動畫。</span><span class="sxs-lookup"><span data-stu-id="d5692-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="d5692-119">動畫架構可讓您加入到另一種使用數個動畫`<Parallel>`項目。</span><span class="sxs-lookup"><span data-stu-id="d5692-119">The Animation framework allows you to join several animations into one using the `<Parallel>` element.</span></span> <span data-ttu-id="d5692-120">中的所有動畫`<Parallel>`會執行一次。</span><span class="sxs-lookup"><span data-stu-id="d5692-120">All animations within `<Parallel>` are executed at the same time.</span></span>

<span data-ttu-id="d5692-121">以下是可能的標記如`AnimationExtender`控制項、 淡出，以及調整面板大小在相同的時間：</span><span class="sxs-lookup"><span data-stu-id="d5692-121">Here is the a possible markup for the `AnimationExtender` control, fading out and resizing the panel at the same time:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample5.aspx)]

<span data-ttu-id="d5692-122">和確實： 當您執行此指令碼，面板會顯示，然後調整大小 (大於 threefolding 其寬度和 halfing 高度) 和淡出，在相同的時間。</span><span class="sxs-lookup"><span data-stu-id="d5692-122">And indeed: when you run this script, the panel is displayed, then resizes (more than threefolding its width and halfing its height) and fades out at the same time.</span></span>


<span data-ttu-id="d5692-123">[![面板淡出，調整大小 （包括其內容，因為瀏覽器的轉譯引擎）](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d5692-123">[![The panel is fading out and resizing (including its content, thanks to the browser's rendering engine)](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)</span></span>

<span data-ttu-id="d5692-124">面板淡出，調整大小 （包括其內容，因為瀏覽器的轉譯引擎） ([按一下以檢視完整大小的影像](executing-several-animations-at-the-same-time-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d5692-124">The panel is fading out and resizing (including its content, thanks to the browser's rendering engine) ([Click to view full-size image](executing-several-animations-at-the-same-time-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d5692-125">[上一頁](adding-animation-to-a-control-vb.md)
> [下一頁](executing-several-animations-after-each-other-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d5692-125">[Previous](adding-animation-to-a-control-vb.md)
[Next](executing-several-animations-after-each-other-vb.md)</span></span>
