---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: 停用動作期間動畫 (C#) |Microsoft Docs
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 它也支援動作...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: 396411332735503d1781e171bead8ab6aaed6efa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828191"
---
<a name="disabling-actions-during-animation-c"></a><span data-ttu-id="b6960-104">停用動作期間動畫 (C#)</span><span class="sxs-lookup"><span data-stu-id="b6960-104">Disabling Actions during Animation (C#)</span></span>
====================
<span data-ttu-id="b6960-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b6960-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b6960-106">[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="b6960-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span></span>

> <span data-ttu-id="b6960-107">動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="b6960-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="b6960-108">它也支援動作，例如滑鼠點按。</span><span class="sxs-lookup"><span data-stu-id="b6960-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="b6960-109">不過當按下滑鼠，開始播放動畫，最好在動畫期間停用滑鼠點按。</span><span class="sxs-lookup"><span data-stu-id="b6960-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>


## <a name="overview"></a><span data-ttu-id="b6960-110">總覽</span><span class="sxs-lookup"><span data-stu-id="b6960-110">Overview</span></span>

<span data-ttu-id="b6960-111">動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="b6960-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="b6960-112">它也支援動作，例如滑鼠點按。</span><span class="sxs-lookup"><span data-stu-id="b6960-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="b6960-113">不過當按下滑鼠，開始播放動畫，最好在動畫期間停用滑鼠點按。</span><span class="sxs-lookup"><span data-stu-id="b6960-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="b6960-114">步驟</span><span class="sxs-lookup"><span data-stu-id="b6960-114">Steps</span></span>

<span data-ttu-id="b6960-115">首先，包括`ScriptManager`單元頁面; 然後，ASP.NET AJAX 程式庫載入，因此能夠使用控制項工具組：</span><span class="sxs-lookup"><span data-stu-id="b6960-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

<span data-ttu-id="b6960-116">動畫會套用至 HTML 按鈕如下：</span><span class="sxs-lookup"><span data-stu-id="b6960-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

<span data-ttu-id="b6960-117">請注意，HTML 控制項使用而不是 Web 控制項，因為我們不想要的按鈕，以建立回傳;它只應該會啟動我們的用戶端動畫。</span><span class="sxs-lookup"><span data-stu-id="b6960-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="b6960-118">然後，新增`AnimationExtender` 頁面上，以提供`ID`，則`TargetControlID`屬性和必要`runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="b6960-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

<span data-ttu-id="b6960-119">內`<Animations>`節點，`<OnClick>`是正確的項目，來處理滑鼠點選。</span><span class="sxs-lookup"><span data-stu-id="b6960-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="b6960-120">不過，動畫播放期間，也可以按一下按鈕。</span><span class="sxs-lookup"><span data-stu-id="b6960-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="b6960-121">`<EnableAction>`可以處理項目。</span><span class="sxs-lookup"><span data-stu-id="b6960-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="b6960-122">設定`Enabled="false"`動畫的過程中停用按鈕。</span><span class="sxs-lookup"><span data-stu-id="b6960-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="b6960-123">因為我們使用數個個別的動畫 （停用按鈕和實際的動畫）`<Parallel>`元素必須結合在一起成一個單一的動畫。</span><span class="sxs-lookup"><span data-stu-id="b6960-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="b6960-124">以下是完成標記`AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="b6960-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

<span data-ttu-id="b6960-125">它也會重新啟用按鈕動畫，並在清單結尾處使用下列的 XML 項目之後：</span><span class="sxs-lookup"><span data-stu-id="b6960-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

<span data-ttu-id="b6960-126">不過在特定案例這會是毫無用處自按鈕淡出，並在動畫結束時看不到。</span><span class="sxs-lookup"><span data-stu-id="b6960-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>


<span data-ttu-id="b6960-127">[![動畫執行時，會停用按鈕](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b6960-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span></span>

<span data-ttu-id="b6960-128">按鈕已停用，只要執行動畫 ([按一下以檢視完整大小的影像](disabling-actions-during-animation-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b6960-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b6960-129">[上一頁](animating-in-response-to-user-interaction-cs.md)
> [下一頁](triggering-an-animation-in-another-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="b6960-129">[Previous](animating-in-response-to-user-interaction-cs.md)
[Next](triggering-an-animation-in-another-control-cs.md)</span></span>
