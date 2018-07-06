---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: 動畫 (VB) 期間停用動作 |Microsoft Docs
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 它也支援動作...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 18c1eb74c73876b3fc6f1e37f69c31642806d043
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830875"
---
<a name="disabling-actions-during-animation-vb"></a><span data-ttu-id="27825-104">停用動作期間動畫 (VB)</span><span class="sxs-lookup"><span data-stu-id="27825-104">Disabling Actions during Animation (VB)</span></span>
====================
<span data-ttu-id="27825-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="27825-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="27825-106">[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="27825-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)</span></span>

> <span data-ttu-id="27825-107">動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="27825-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="27825-108">它也支援動作，例如滑鼠點按。</span><span class="sxs-lookup"><span data-stu-id="27825-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="27825-109">不過當按下滑鼠，開始播放動畫，最好在動畫期間停用滑鼠點按。</span><span class="sxs-lookup"><span data-stu-id="27825-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>


## <a name="overview"></a><span data-ttu-id="27825-110">總覽</span><span class="sxs-lookup"><span data-stu-id="27825-110">Overview</span></span>

<span data-ttu-id="27825-111">動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="27825-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="27825-112">它也支援動作，例如滑鼠點按。</span><span class="sxs-lookup"><span data-stu-id="27825-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="27825-113">不過當按下滑鼠，開始播放動畫，最好在動畫期間停用滑鼠點按。</span><span class="sxs-lookup"><span data-stu-id="27825-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="27825-114">步驟</span><span class="sxs-lookup"><span data-stu-id="27825-114">Steps</span></span>

<span data-ttu-id="27825-115">首先，包括`ScriptManager`單元頁面; 然後，ASP.NET AJAX 程式庫載入，因此能夠使用控制項工具組：</span><span class="sxs-lookup"><span data-stu-id="27825-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

<span data-ttu-id="27825-116">動畫會套用至 HTML 按鈕如下：</span><span class="sxs-lookup"><span data-stu-id="27825-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

<span data-ttu-id="27825-117">請注意，HTML 控制項使用而不是 Web 控制項，因為我們不想要的按鈕，以建立回傳;它只應該會啟動我們的用戶端動畫。</span><span class="sxs-lookup"><span data-stu-id="27825-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="27825-118">然後，新增`AnimationExtender` 頁面上，以提供`ID`，則`TargetControlID`屬性和必要`runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="27825-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

<span data-ttu-id="27825-119">內`<Animations>`節點，`<OnClick>`是正確的項目，來處理滑鼠點選。</span><span class="sxs-lookup"><span data-stu-id="27825-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="27825-120">不過，動畫播放期間，也可以按一下按鈕。</span><span class="sxs-lookup"><span data-stu-id="27825-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="27825-121">`<EnableAction>`可以處理項目。</span><span class="sxs-lookup"><span data-stu-id="27825-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="27825-122">設定`Enabled="false"`動畫的過程中停用按鈕。</span><span class="sxs-lookup"><span data-stu-id="27825-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="27825-123">因為我們使用數個個別的動畫 （停用按鈕和實際的動畫）`<Parallel>`元素必須結合在一起成一個單一的動畫。</span><span class="sxs-lookup"><span data-stu-id="27825-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="27825-124">以下是完成標記`AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="27825-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

<span data-ttu-id="27825-125">它也會重新啟用按鈕動畫，並在清單結尾處使用下列的 XML 項目之後：</span><span class="sxs-lookup"><span data-stu-id="27825-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

<span data-ttu-id="27825-126">不過在特定案例這會是毫無用處自按鈕淡出，並在動畫結束時看不到。</span><span class="sxs-lookup"><span data-stu-id="27825-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>


<span data-ttu-id="27825-127">[![動畫執行時，會停用按鈕](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="27825-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)</span></span>

<span data-ttu-id="27825-128">按鈕已停用，只要執行動畫 ([按一下以檢視完整大小的影像](disabling-actions-during-animation-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="27825-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="27825-129">[上一頁](animating-in-response-to-user-interaction-vb.md)
> [下一頁](triggering-an-animation-in-another-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="27825-129">[Previous](animating-in-response-to-user-interaction-vb.md)
[Next](triggering-an-animation-in-another-control-vb.md)</span></span>
