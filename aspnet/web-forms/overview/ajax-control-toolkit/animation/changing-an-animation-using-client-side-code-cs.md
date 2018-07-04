---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: 變更動畫使用用戶端程式碼 (C#) |Microsoft Docs
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。 動畫也可以...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 4bf480401e244661e2c316adcde3cbde647a6dc3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393070"
---
<a name="changing-an-animation-using-client-side-code-c"></a><span data-ttu-id="686c7-104">變更動畫使用用戶端程式碼 (C#)</span><span class="sxs-lookup"><span data-stu-id="686c7-104">Changing an Animation Using Client-Side Code (C#)</span></span>
====================
<span data-ttu-id="686c7-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="686c7-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="686c7-106">[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="686c7-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span></span>

> <span data-ttu-id="686c7-107">動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="686c7-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="686c7-108">動畫也可以使用自訂用戶端 JavaScript 程式碼會變更。</span><span class="sxs-lookup"><span data-stu-id="686c7-108">The animation can also be changed using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="686c7-109">總覽</span><span class="sxs-lookup"><span data-stu-id="686c7-109">Overview</span></span>

<span data-ttu-id="686c7-110">動畫控制項在 ASP.NET AJAX Control Toolkit 中不只是控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="686c7-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="686c7-111">動畫也可以使用自訂用戶端 JavaScript 程式碼會變更。</span><span class="sxs-lookup"><span data-stu-id="686c7-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="686c7-112">步驟</span><span class="sxs-lookup"><span data-stu-id="686c7-112">Steps</span></span>

<span data-ttu-id="686c7-113">首先，包括`ScriptManager`單元頁面; 然後，ASP.NET AJAX 程式庫載入，因此能夠使用控制項工具組：</span><span class="sxs-lookup"><span data-stu-id="686c7-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

<span data-ttu-id="686c7-114">動畫將會套用至面板的文字看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="686c7-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

<span data-ttu-id="686c7-115">在 [面板] 中相關聯的 CSS 類別，定義好用的背景色彩和也設定面板的固定的寬度：</span><span class="sxs-lookup"><span data-stu-id="686c7-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

<span data-ttu-id="686c7-116">實際的動畫是由 HTML 按鈕啟動：</span><span class="sxs-lookup"><span data-stu-id="686c7-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

<span data-ttu-id="686c7-117">然後，新增`AnimationExtender` 頁面上，以提供`ID`，則`TargetControlID`屬性和必要`runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="686c7-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

<span data-ttu-id="686c7-118">請注意，沒有任何`<Animations>`內的節點`AnimationExtender`控制項。</span><span class="sxs-lookup"><span data-stu-id="686c7-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="686c7-119">自訂的 JavaScript 程式碼用來提供要搭配控制項使用的動畫。</span><span class="sxs-lookup"><span data-stu-id="686c7-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="686c7-120">如同伺服器 API 的`AnimationExtender`，沒有任何簡單的方法，將尚未指派給擴充項的動畫。</span><span class="sxs-lookup"><span data-stu-id="686c7-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="686c7-121">不過擴充項會公開數種方法來讀取和寫入動畫向各種事件 (`OnClick`，`OnLoad`等等)。</span><span class="sxs-lookup"><span data-stu-id="686c7-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="686c7-122">以下是一些範例：</span><span class="sxs-lookup"><span data-stu-id="686c7-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="686c7-123">傳回值的格式`get_*()`函式和引數的格式`set_*()`函式是 JSON 字串，提供想要的 XML 標記的物件表示。</span><span class="sxs-lookup"><span data-stu-id="686c7-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="686c7-124">目前沒有任何方法，將物件傳送中，但您可指定動畫從讀取物件 (`get_OnXXXBehavior()`方法)。</span><span class="sxs-lookup"><span data-stu-id="686c7-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="686c7-125">以下是 JSON 字串 (不含分隔引號和格式化) 表示在按鈕中，所觸發的動畫，但是調整它，並在同一時間淡出動畫面板：</span><span class="sxs-lookup"><span data-stu-id="686c7-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

<span data-ttu-id="686c7-126">下列 JavaScript 程式碼會將指派到這個 JSON descripting`OnClick`動畫目前的擴充項，並執行它：</span><span class="sxs-lookup"><span data-stu-id="686c7-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


<span data-ttu-id="686c7-127">[![動畫會立即執行，而不需要按下滑鼠 （和以非常少的標記）](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="686c7-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="686c7-128">動畫會立即執行，沒有滑鼠點選 （和以非常少的標記） ([按一下以檢視完整大小的影像](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="686c7-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="686c7-129">[上一頁](executing-animations-using-client-side-code-cs.md)
> [下一頁](animating-an-updatepanel-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="686c7-129">[Previous](executing-animations-using-client-side-code-cs.md)
[Next](animating-an-updatepanel-control-cs.md)</span></span>
