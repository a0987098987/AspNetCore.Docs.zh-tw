---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: "動畫 (VB) 期間停用動作 |Microsoft 文件"
author: wenz
description: "動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 它也支援動作..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 0f68d27f60e9b224a7cc0d598962553afeb3f28f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="disabling-actions-during-animation-vb"></a><span data-ttu-id="8bb36-104">停用的動作，在動畫中的 (VB)</span><span class="sxs-lookup"><span data-stu-id="8bb36-104">Disabling Actions during Animation (VB)</span></span>
====================
<span data-ttu-id="8bb36-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8bb36-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8bb36-106">[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="8bb36-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)</span></span>

> <span data-ttu-id="8bb36-107">動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="8bb36-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="8bb36-108">它也支援的動作，例如滑鼠點按。</span><span class="sxs-lookup"><span data-stu-id="8bb36-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="8bb36-109">不過當滑鼠點按開始播放動畫，最好在動畫期間停用滑鼠點按動作。</span><span class="sxs-lookup"><span data-stu-id="8bb36-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>


## <a name="overview"></a><span data-ttu-id="8bb36-110">概觀</span><span class="sxs-lookup"><span data-stu-id="8bb36-110">Overview</span></span>

<span data-ttu-id="8bb36-111">動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="8bb36-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="8bb36-112">它也支援的動作，例如滑鼠點按。</span><span class="sxs-lookup"><span data-stu-id="8bb36-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="8bb36-113">不過當滑鼠點按開始播放動畫，最好在動畫期間停用滑鼠點按動作。</span><span class="sxs-lookup"><span data-stu-id="8bb36-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="8bb36-114">步驟</span><span class="sxs-lookup"><span data-stu-id="8bb36-114">Steps</span></span>

<span data-ttu-id="8bb36-115">首先，包括`ScriptManager`在頁面; 然後，ASP.NET AJAX 程式庫載入，因此您能夠使用控制項的工具組：</span><span class="sxs-lookup"><span data-stu-id="8bb36-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

<span data-ttu-id="8bb36-116">動畫會套用至 HTML 按鈕像這樣：</span><span class="sxs-lookup"><span data-stu-id="8bb36-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

<span data-ttu-id="8bb36-117">請注意 HTML 控制項用而不是 Web 控制項，因為我們不想要建立回傳; 按鈕它只應該啟動為我們的用戶端動畫。</span><span class="sxs-lookup"><span data-stu-id="8bb36-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="8bb36-118">接著，新增`AnimationExtender`到頁面上，提供`ID`、`TargetControlID`屬性和必要`runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="8bb36-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

<span data-ttu-id="8bb36-119">內`<Animations>` 節點，`<OnClick>`是正確的項目，來處理滑鼠點按。</span><span class="sxs-lookup"><span data-stu-id="8bb36-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="8bb36-120">不過，無法在動畫，以及按一下按鈕。</span><span class="sxs-lookup"><span data-stu-id="8bb36-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="8bb36-121">`<EnableAction>`項目可以採取的處理。</span><span class="sxs-lookup"><span data-stu-id="8bb36-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="8bb36-122">設定`Enabled="false"`一部分動畫停用按鈕。</span><span class="sxs-lookup"><span data-stu-id="8bb36-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="8bb36-123">因為我們使用數個個別的動畫 （停用按鈕與實際的動畫，）`<Parallel>`元素必須在黏附結合成一個單一的動畫。</span><span class="sxs-lookup"><span data-stu-id="8bb36-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="8bb36-124">以下是完整標記`AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="8bb36-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

<span data-ttu-id="8bb36-125">它也會重新啟用按鈕動畫，並使用下列 XML 項目清單的結尾之後：</span><span class="sxs-lookup"><span data-stu-id="8bb36-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

<span data-ttu-id="8bb36-126">不過在特定案例中這會是毫無用處自按鈕淡出，結尾的動畫看不到。</span><span class="sxs-lookup"><span data-stu-id="8bb36-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>


<span data-ttu-id="8bb36-127">[![動畫執行時，會停用按鈕](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8bb36-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)</span></span>

<span data-ttu-id="8bb36-128">動畫執行時，會停用按鈕 ([按一下以檢視完整大小的影像](disabling-actions-during-animation-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8bb36-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="8bb36-129">[上一頁](animating-in-response-to-user-interaction-vb.md)
[下一頁](triggering-an-animation-in-another-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="8bb36-129">[Previous](animating-in-response-to-user-interaction-vb.md)
[Next](triggering-an-animation-in-another-control-vb.md)</span></span>
