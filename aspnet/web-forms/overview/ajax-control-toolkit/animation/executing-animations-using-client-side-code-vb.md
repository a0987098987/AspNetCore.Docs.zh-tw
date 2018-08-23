---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: 執行使用用戶端程式碼 (VB) 的動畫 |Microsoft Docs
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 動畫執行...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 08cba7fa04249da4f0c7baa8e730ac75489e0efc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833814"
---
<a name="executing-animations-using-client-side-code-vb"></a><span data-ttu-id="a93d4-104">執行動畫使用用戶端程式碼 (VB)</span><span class="sxs-lookup"><span data-stu-id="a93d4-104">Executing Animations Using Client-Side Code (VB)</span></span>
====================
<span data-ttu-id="a93d4-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a93d4-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a93d4-106">[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="a93d4-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span></span>

> <span data-ttu-id="a93d4-107">動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="a93d4-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a93d4-108">動畫執行也可能使用自訂用戶端 JavaScript 程式碼會觸發。</span><span class="sxs-lookup"><span data-stu-id="a93d4-108">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="a93d4-109">總覽</span><span class="sxs-lookup"><span data-stu-id="a93d4-109">Overview</span></span>

<span data-ttu-id="a93d4-110">動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="a93d4-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a93d4-111">動畫執行也可能使用自訂用戶端 JavaScript 程式碼會觸發。</span><span class="sxs-lookup"><span data-stu-id="a93d4-111">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="a93d4-112">步驟</span><span class="sxs-lookup"><span data-stu-id="a93d4-112">Steps</span></span>

<span data-ttu-id="a93d4-113">首先，包括`ScriptManager`單元頁面; 然後，ASP.NET AJAX 程式庫載入，因此能夠使用控制項工具組：</span><span class="sxs-lookup"><span data-stu-id="a93d4-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="a93d4-114">動畫將會套用至面板的文字看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="a93d4-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="a93d4-115">在 [面板] 中相關聯的 CSS 類別，定義好用的背景色彩和也設定面板的固定的寬度：</span><span class="sxs-lookup"><span data-stu-id="a93d4-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="a93d4-116">然後，新增`AnimationExtender` 頁面上，以提供`ID`，則`TargetControlID`屬性和必要`runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="a93d4-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="a93d4-117">內`<Animations>`節點，請使用`<OnClick>`執行動畫一次使用者按一下面板上。</span><span class="sxs-lookup"><span data-stu-id="a93d4-117">Within the `<Animations>` node, use `<OnClick>` to run the animations once the user clicks on the panel.</span></span> <span data-ttu-id="a93d4-118">新增兩個動畫 parallelly 執行：</span><span class="sxs-lookup"><span data-stu-id="a93d4-118">Add two animations to be executed parallelly:</span></span>

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

<span data-ttu-id="a93d4-119">為了示範，這個動畫 （和任何其他使用 Control Toolkit 建立的動畫） 執行之後執行網頁時，使用 JavaScript 程式碼。</span><span class="sxs-lookup"><span data-stu-id="a93d4-119">For the sake of demonstration, this animation (and any other animation created using the Control Toolkit) is executed using JavaScript code, once the page runs.</span></span> <span data-ttu-id="a93d4-120">首先我們必須存取`AnimationExtender`控制項。</span><span class="sxs-lookup"><span data-stu-id="a93d4-120">First of all we need access to the `AnimationExtender` control.</span></span> <span data-ttu-id="a93d4-121">ASP.NET AJAX 程式庫提供`$find()`函式，這項工作：</span><span class="sxs-lookup"><span data-stu-id="a93d4-121">The ASP.NET AJAX library provides the `$find()` function for this task:</span></span>

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

<span data-ttu-id="a93d4-122">`AnimationExtender`控制項會公開豐富的 API，包括具有名稱相同的 XML 標記中所使用的事件處理常式的方法： `OnClick()`， `OnLoad()`，依此類推。</span><span class="sxs-lookup"><span data-stu-id="a93d4-122">The `AnimationExtender` control exposes a rich API, including methods with names identical to the event handlers used in the XML markup: `OnClick()`, `OnLoad()`, and so on.</span></span> <span data-ttu-id="a93d4-123">比方說，呼叫`OnClick()`方法會執行內的動畫`<OnClick>`項目`AnimationExtender`控制項：</span><span class="sxs-lookup"><span data-stu-id="a93d4-123">For instance, a call of the `OnClick()` method executes the animation within the `<OnClick>` element of the `AnimationExtender` control:</span></span>

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

<span data-ttu-id="a93d4-124">以下是 頁面完全載入後，模擬面板上的按一下 完成用戶端 JavaScript 程式碼，請注意，`pageLoad()`函式名稱可由呼叫 ASP.NET AJAX 一次頁面和所有包含的 JavaScript 程式庫已載入。</span><span class="sxs-lookup"><span data-stu-id="a93d4-124">Here is the complete client-side JavaScript code that emulates the click on the panel once the page has been fully loaded note that the `pageLoad()` function name is used which is called by ASP.NET AJAX once the page and all included JavaScript libraries have been loaded.</span></span>

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]


<span data-ttu-id="a93d4-125">[![動畫會立即執行，而不需要按下滑鼠](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a93d4-125">[![The animation runs immediately, without a mouse click](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="a93d4-126">動畫會立即執行，沒有滑鼠點選 ([按一下以檢視完整大小的影像](executing-animations-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a93d4-126">The animation runs immediately, without a mouse click ([Click to view full-size image](executing-animations-using-client-side-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a93d4-127">[上一頁](modifying-animations-from-the-server-side-vb.md)
> [下一頁](changing-an-animation-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a93d4-127">[Previous](modifying-animations-from-the-server-side-vb.md)
[Next](changing-an-animation-using-client-side-code-vb.md)</span></span>
