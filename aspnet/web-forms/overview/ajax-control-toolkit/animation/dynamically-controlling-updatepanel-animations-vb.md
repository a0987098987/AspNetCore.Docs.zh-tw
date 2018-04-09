---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: 以動態方式控制 UpdatePanel 動畫 (VB) |Microsoft 文件
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 內容...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: ff2853b4457a83a7367b4d1072d21929c40a3ed2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="dynamically-controlling-updatepanel-animations-vb"></a><span data-ttu-id="a2ce4-104">以動態方式控制 UpdatePanel 動畫 (VB)</span><span class="sxs-lookup"><span data-stu-id="a2ce4-104">Dynamically Controlling UpdatePanel Animations (VB)</span></span>
====================
<span data-ttu-id="a2ce4-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a2ce4-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a2ce4-106">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="a2ce4-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span></span>

> <span data-ttu-id="a2ce4-107">動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="a2ce4-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a2ce4-108">UpdatePanel 的內容，特殊的擴充項存在，高度依賴動畫 framework: UpdatePanelAnimation。</span><span class="sxs-lookup"><span data-stu-id="a2ce4-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="a2ce4-109">它也可以一起 UpdatePanel 觸發程序。</span><span class="sxs-lookup"><span data-stu-id="a2ce4-109">It can also work together with UpdatePanel triggers.</span></span>


## <a name="overview"></a><span data-ttu-id="a2ce4-110">總覽</span><span class="sxs-lookup"><span data-stu-id="a2ce4-110">Overview</span></span>

<span data-ttu-id="a2ce4-111">動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="a2ce4-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a2ce4-112">內容的`UpdatePanel`，特殊的擴充項存在，高度依賴動畫 framework: `UpdatePanelAnimation`。</span><span class="sxs-lookup"><span data-stu-id="a2ce4-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="a2ce4-113">它也可搭配`UpdatePanel`觸發程序。</span><span class="sxs-lookup"><span data-stu-id="a2ce4-113">It can also work together with `UpdatePanel` triggers.</span></span>

## <a name="steps"></a><span data-ttu-id="a2ce4-114">步驟</span><span class="sxs-lookup"><span data-stu-id="a2ce4-114">Steps</span></span>

<span data-ttu-id="a2ce4-115">第一個步驟是包含如往常般`ScriptManager`頁面中，而 ASP.NET AJAX library 載入 Control Toolkit 可用：</span><span class="sxs-lookup"><span data-stu-id="a2ce4-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

<span data-ttu-id="a2ce4-116">在此案例中動畫會套用到目前時間的顯示。</span><span class="sxs-lookup"><span data-stu-id="a2ce4-116">The animation in this scenario will be applied to a display of the current time.</span></span> <span data-ttu-id="a2ce4-117">這項資訊可以寫入標籤使用`Page_Load()`方法，或使用下列的內嵌程式碼 （為了簡單起見）：</span><span class="sxs-lookup"><span data-stu-id="a2ce4-117">This information can be written into a label using the `Page_Load()` method, or (for the sake of simplicity) the following inline code is used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

<span data-ttu-id="a2ce4-118">此外，也會建立的按鈕來觸發更新的時間：</span><span class="sxs-lookup"><span data-stu-id="a2ce4-118">Also, a button to trigger updating the time is created:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

<span data-ttu-id="a2ce4-119">此程式碼再將放入`<ContentTemplate>`區段`UpdatePanel`項目。</span><span class="sxs-lookup"><span data-stu-id="a2ce4-119">This code is then put into the `<ContentTemplate>` section of an `UpdatePanel` element.</span></span> <span data-ttu-id="a2ce4-120">面板的`UpdateMode`屬性必須設為`"Conditional"`，因為只有觸發程序可能會更新面板的內容。</span><span class="sxs-lookup"><span data-stu-id="a2ce4-120">The panel's `UpdateMode` attribute must be set to `"Conditional"`, since only triggers may update the panel's contents.</span></span> <span data-ttu-id="a2ce4-121">在`<Triggers>`區段`UpdatePanel`，非同步回傳觸發程序建立並繫結至`Click`按鈕的事件。</span><span class="sxs-lookup"><span data-stu-id="a2ce4-121">In the `<Triggers>` section of the `UpdatePanel`, an asynchronous postback trigger is created and tied to the `Click` event of the button.</span></span> <span data-ttu-id="a2ce4-122">因此，如果使用者按一下按鈕，`UpdatePanel`會重新整理。</span><span class="sxs-lookup"><span data-stu-id="a2ce4-122">Thus, if the user clicks on the button, the `UpdatePanel` is refreshed.</span></span> <span data-ttu-id="a2ce4-123">以下是標記`UpdatePanel`控制項：</span><span class="sxs-lookup"><span data-stu-id="a2ce4-123">Here is the markup for the `UpdatePanel` control:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

<span data-ttu-id="a2ce4-124">最後，`UpdatePanelAnimationExtender`必須設定： 設定`TargetControlID`面板的 id 屬性，定義內 extender 動畫。</span><span class="sxs-lookup"><span data-stu-id="a2ce4-124">Finally, the `UpdatePanelAnimationExtender` must be configured: Set the `TargetControlID` attribute to the ID of the panel, and define an animation within the extender.</span></span> <span data-ttu-id="a2ce4-125">淡入使意義而言，建立好 visual 強調更新的時間。</span><span class="sxs-lookup"><span data-stu-id="a2ce4-125">Fading in makes sense, which creates a nice visual emphasis on the updated time.</span></span> <span data-ttu-id="a2ce4-126">然後，您擴充項的標記可能看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="a2ce4-126">Your extender markup may then look like this:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

<span data-ttu-id="a2ce4-127">在瀏覽器中執行的檔案。</span><span class="sxs-lookup"><span data-stu-id="a2ce4-127">Run the file in the browser.</span></span> <span data-ttu-id="a2ce4-128">每當您按一下按鈕時，目前的時間會顯示在面板中，一律為一秒期間淡入。</span><span class="sxs-lookup"><span data-stu-id="a2ce4-128">Whenever you click on the button, the current time is shown in the panel, always fading in for the duration of one second.</span></span>


<span data-ttu-id="a2ce4-129">[![目前時間淡入](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a2ce4-129">[![The current time is fading in](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span></span>

<span data-ttu-id="a2ce4-130">目前時間淡入 ([按一下以檢視完整大小的影像](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a2ce4-130">The current time is fading in ([Click to view full-size image](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a2ce4-131">上一步</span><span class="sxs-lookup"><span data-stu-id="a2ce4-131">Previous</span></span>](animating-an-updatepanel-control-vb.md)
