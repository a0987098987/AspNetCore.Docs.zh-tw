---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: 觸發另一個控制項 (VB) 的動畫 |Microsoft Docs
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 一般而言，啟動...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: b316b22bf355d7abb0909e43f0c2c38ea3e68f24
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367806"
---
<a name="triggering-an-animation-in-another-control-vb"></a><span data-ttu-id="4ec2f-104">觸發另一個控制項 (VB) 的動畫</span><span class="sxs-lookup"><span data-stu-id="4ec2f-104">Triggering an Animation in another Control (VB)</span></span>
====================
<span data-ttu-id="4ec2f-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4ec2f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4ec2f-106">[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="4ec2f-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span></span>

> <span data-ttu-id="4ec2f-107">動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="4ec2f-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4ec2f-108">一般而言，啟動動畫時觸發相同的控制項與使用者互動。</span><span class="sxs-lookup"><span data-stu-id="4ec2f-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="4ec2f-109">不過也可以使用一個控制項，然後動畫互動另一個控制項。</span><span class="sxs-lookup"><span data-stu-id="4ec2f-109">It is however also possible to interact with one control and then animation another control.</span></span>


## <a name="overview"></a><span data-ttu-id="4ec2f-110">總覽</span><span class="sxs-lookup"><span data-stu-id="4ec2f-110">Overview</span></span>

<span data-ttu-id="4ec2f-111">動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="4ec2f-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4ec2f-112">一般而言，啟動動畫時觸發相同的控制項與使用者互動。</span><span class="sxs-lookup"><span data-stu-id="4ec2f-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="4ec2f-113">不過也可以使用一個控制項，然後動畫互動另一個控制項。</span><span class="sxs-lookup"><span data-stu-id="4ec2f-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="4ec2f-114">步驟</span><span class="sxs-lookup"><span data-stu-id="4ec2f-114">Steps</span></span>

<span data-ttu-id="4ec2f-115">首先，包括`ScriptManager`單元頁面; 然後，ASP.NET AJAX 程式庫載入，因此能夠使用控制項工具組：</span><span class="sxs-lookup"><span data-stu-id="4ec2f-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

<span data-ttu-id="4ec2f-116">動畫將會套用至面板的文字看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="4ec2f-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

<span data-ttu-id="4ec2f-117">在 [面板] 中相關聯的 CSS 類別，定義好用的背景色彩和也設定面板的固定的寬度：</span><span class="sxs-lookup"><span data-stu-id="4ec2f-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

<span data-ttu-id="4ec2f-118">若要開始以動畫顯示面板，可使用 HTML 按鈕。</span><span class="sxs-lookup"><span data-stu-id="4ec2f-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="4ec2f-119">請注意， `<input type="button" />` favoured 透過`<asp:Button />`因為我們不想回傳時使用者按下該按鈕。</span><span class="sxs-lookup"><span data-stu-id="4ec2f-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

<span data-ttu-id="4ec2f-120">然後，新增`AnimationExtender` 頁面上，以提供`ID`，則`TargetControlID`屬性和必要`runat="server"`。</span><span class="sxs-lookup"><span data-stu-id="4ec2f-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="4ec2f-121">請務必設定`TargetControlID`id 的按鈕 （項目觸發動畫），不為面板 （正在顯示動畫的元素） 的識別碼</span><span class="sxs-lookup"><span data-stu-id="4ec2f-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

<span data-ttu-id="4ec2f-122">內`<Animations>`節點，如往常般進行動畫。</span><span class="sxs-lookup"><span data-stu-id="4ec2f-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="4ec2f-123">為了讓它們變更 [面板] 中，不是按鈕，設定`AnimationTarget`內的每個動畫項目的屬性`AnimationExtender`。</span><span class="sxs-lookup"><span data-stu-id="4ec2f-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="4ec2f-124">值`AnimationTarget`當然是面板的 ID。</span><span class="sxs-lookup"><span data-stu-id="4ec2f-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="4ec2f-125">如此一來，動畫會發生的窗格中，不會與觸發按鈕。</span><span class="sxs-lookup"><span data-stu-id="4ec2f-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="4ec2f-126">以下是`AnimationExtender`此案例中的標記：</span><span class="sxs-lookup"><span data-stu-id="4ec2f-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

<span data-ttu-id="4ec2f-127">請注意特殊個別動畫顯示的順序。</span><span class="sxs-lookup"><span data-stu-id="4ec2f-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="4ec2f-128">首先，動畫執行後，便會停用按鈕。</span><span class="sxs-lookup"><span data-stu-id="4ec2f-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="4ec2f-129">因為沒有任何`AnimationTarget`屬性中`<EnableAction>`項目，這個動畫會套用至原始的控制項: [] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="4ec2f-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="4ec2f-130">下面兩個動畫步驟都應該實行 parallelly (`<Parallel>`項目)。</span><span class="sxs-lookup"><span data-stu-id="4ec2f-130">The next two animation steps shall be carried out parallelly (`<Parallel>` element).</span></span> <span data-ttu-id="4ec2f-131">兩者都有其`AnimationTarget`屬性設定為`"Panel1"`，因此以動畫顯示的窗格中，不是按鈕。</span><span class="sxs-lookup"><span data-stu-id="4ec2f-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>


<span data-ttu-id="4ec2f-132">[![在按鈕上的按一下滑鼠開始面板動畫](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4ec2f-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="4ec2f-133">在按鈕上的按一下滑鼠開始面板動畫 ([按一下以檢視完整大小的影像](triggering-an-animation-in-another-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4ec2f-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4ec2f-134">[上一頁](disabling-actions-during-animation-vb.md)
> [下一頁](modifying-animations-from-the-server-side-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4ec2f-134">[Previous](disabling-actions-during-animation-vb.md)
[Next](modifying-animations-from-the-server-side-vb.md)</span></span>
