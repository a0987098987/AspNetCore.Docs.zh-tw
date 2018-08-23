---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: 觸發另一個控制項 (C#) 中的動畫 |Microsoft Docs
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 一般而言，啟動...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ad6dfecf71a7577215e43222a8788e5c48d0c4c2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831167"
---
<a name="triggering-an-animation-in-another-control-c"></a><span data-ttu-id="74670-104">觸發另一個控制項 (C#) 中的動畫</span><span class="sxs-lookup"><span data-stu-id="74670-104">Triggering an Animation in another Control (C#)</span></span>
====================
<span data-ttu-id="74670-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="74670-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="74670-106">[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="74670-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span></span>

> <span data-ttu-id="74670-107">動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="74670-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="74670-108">一般而言，啟動動畫時觸發相同的控制項與使用者互動。</span><span class="sxs-lookup"><span data-stu-id="74670-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="74670-109">不過也可以使用一個控制項，然後動畫互動另一個控制項。</span><span class="sxs-lookup"><span data-stu-id="74670-109">It is however also possible to interact with one control and then animation another control.</span></span>


## <a name="overview"></a><span data-ttu-id="74670-110">總覽</span><span class="sxs-lookup"><span data-stu-id="74670-110">Overview</span></span>

<span data-ttu-id="74670-111">動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="74670-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="74670-112">一般而言，啟動動畫時觸發相同的控制項與使用者互動。</span><span class="sxs-lookup"><span data-stu-id="74670-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="74670-113">不過也可以使用一個控制項，然後動畫互動另一個控制項。</span><span class="sxs-lookup"><span data-stu-id="74670-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="74670-114">步驟</span><span class="sxs-lookup"><span data-stu-id="74670-114">Steps</span></span>

<span data-ttu-id="74670-115">首先，包括`ScriptManager`單元頁面; 然後，ASP.NET AJAX 程式庫載入，因此能夠使用控制項工具組：</span><span class="sxs-lookup"><span data-stu-id="74670-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

<span data-ttu-id="74670-116">動畫將會套用至面板的文字看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="74670-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

<span data-ttu-id="74670-117">在 [面板] 中相關聯的 CSS 類別，定義好用的背景色彩和也設定面板的固定的寬度：</span><span class="sxs-lookup"><span data-stu-id="74670-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

<span data-ttu-id="74670-118">若要開始以動畫顯示面板，可使用 HTML 按鈕。</span><span class="sxs-lookup"><span data-stu-id="74670-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="74670-119">請注意， `<input type="button" />` favoured 透過`<asp:Button />`因為我們不想回傳時使用者按下該按鈕。</span><span class="sxs-lookup"><span data-stu-id="74670-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

<span data-ttu-id="74670-120">然後，新增`AnimationExtender` 頁面上，以提供`ID`，則`TargetControlID`屬性和必要`runat="server"`。</span><span class="sxs-lookup"><span data-stu-id="74670-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="74670-121">請務必設定`TargetControlID`id 的按鈕 （項目觸發動畫），不為面板 （正在顯示動畫的元素） 的識別碼</span><span class="sxs-lookup"><span data-stu-id="74670-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

<span data-ttu-id="74670-122">內`<Animations>`節點，如往常般進行動畫。</span><span class="sxs-lookup"><span data-stu-id="74670-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="74670-123">為了讓它們變更 [面板] 中，不是按鈕，設定`AnimationTarget`內的每個動畫項目的屬性`AnimationExtender`。</span><span class="sxs-lookup"><span data-stu-id="74670-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="74670-124">值`AnimationTarget`當然是面板的 ID。</span><span class="sxs-lookup"><span data-stu-id="74670-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="74670-125">如此一來，動畫會發生的窗格中，不會與觸發按鈕。</span><span class="sxs-lookup"><span data-stu-id="74670-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="74670-126">以下是`AnimationExtender`此案例中的標記：</span><span class="sxs-lookup"><span data-stu-id="74670-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

<span data-ttu-id="74670-127">請注意特殊個別動畫顯示的順序。</span><span class="sxs-lookup"><span data-stu-id="74670-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="74670-128">首先，動畫執行後，便會停用按鈕。</span><span class="sxs-lookup"><span data-stu-id="74670-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="74670-129">因為沒有任何`AnimationTarget`屬性中`<EnableAction>`項目，這個動畫會套用至原始的控制項: [] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="74670-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="74670-130">下面兩個動畫步驟都應該實行 parallelly (`<Parallel>`項目)。</span><span class="sxs-lookup"><span data-stu-id="74670-130">The next two animation steps shall be carried out parallelly (`<Parallel>` element).</span></span> <span data-ttu-id="74670-131">兩者都有其`AnimationTarget`屬性設定為`"Panel1"`，因此以動畫顯示的窗格中，不是按鈕。</span><span class="sxs-lookup"><span data-stu-id="74670-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>


<span data-ttu-id="74670-132">[![在按鈕上的按一下滑鼠開始面板動畫](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="74670-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="74670-133">在按鈕上的按一下滑鼠開始面板動畫 ([按一下以檢視完整大小的影像](triggering-an-animation-in-another-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="74670-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="74670-134">[上一頁](disabling-actions-during-animation-cs.md)
> [下一頁](modifying-animations-from-the-server-side-cs.md)</span><span class="sxs-lookup"><span data-stu-id="74670-134">[Previous](disabling-actions-during-animation-cs.md)
[Next](modifying-animations-from-the-server-side-cs.md)</span></span>
