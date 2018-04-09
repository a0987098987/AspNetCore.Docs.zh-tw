---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: 變更使用用戶端程式碼 (VB) 動畫 |Microsoft 文件
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 動畫也可以...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 7f9b72576cc3a9e91827cfb40983821704621060
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="changing-an-animation-using-client-side-code-vb"></a><span data-ttu-id="df9db-104">變更動畫使用用戶端程式碼 (VB)</span><span class="sxs-lookup"><span data-stu-id="df9db-104">Changing an Animation Using Client-Side Code (VB)</span></span>
====================
<span data-ttu-id="df9db-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="df9db-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="df9db-106">[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="df9db-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span></span>

> <span data-ttu-id="df9db-107">動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="df9db-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="df9db-108">動畫也可以使用自訂用戶端 JavaScript 程式碼會變更。</span><span class="sxs-lookup"><span data-stu-id="df9db-108">The animation can also be changed using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="df9db-109">總覽</span><span class="sxs-lookup"><span data-stu-id="df9db-109">Overview</span></span>

<span data-ttu-id="df9db-110">動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="df9db-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="df9db-111">動畫也可以使用自訂用戶端 JavaScript 程式碼會變更。</span><span class="sxs-lookup"><span data-stu-id="df9db-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="df9db-112">步驟</span><span class="sxs-lookup"><span data-stu-id="df9db-112">Steps</span></span>

<span data-ttu-id="df9db-113">首先，包括`ScriptManager`在頁面; 然後，ASP.NET AJAX 程式庫載入，因此您能夠使用控制項的工具組：</span><span class="sxs-lookup"><span data-stu-id="df9db-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="df9db-114">動畫會套用到面板中的文字看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="df9db-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="df9db-115">在 [面板] 中相關聯的 CSS 類別，定義好的背景色彩，也將面板固定的寬度設定：</span><span class="sxs-lookup"><span data-stu-id="df9db-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="df9db-116">實際的動畫是由 HTML 按鈕啟動：</span><span class="sxs-lookup"><span data-stu-id="df9db-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="df9db-117">接著，新增`AnimationExtender`到頁面上，提供`ID`、`TargetControlID`屬性和必要`runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="df9db-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

<span data-ttu-id="df9db-118">請注意，沒有任何`<Animations>`節點內`AnimationExtender`控制項。</span><span class="sxs-lookup"><span data-stu-id="df9db-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="df9db-119">自訂 JavaScript 程式碼用來提供以用於控制項的動畫。</span><span class="sxs-lookup"><span data-stu-id="df9db-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="df9db-120">使用伺服器 API 的`AnimationExtender`，沒有簡單的方法可以尚未指派給 extender 的動畫。</span><span class="sxs-lookup"><span data-stu-id="df9db-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="df9db-121">但是擴充項會公開數種方法來讀取和寫入動畫向各種事件 (`OnClick`，`OnLoad`等等)。</span><span class="sxs-lookup"><span data-stu-id="df9db-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="df9db-122">以下是一些範例：</span><span class="sxs-lookup"><span data-stu-id="df9db-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="df9db-123">傳回值的格式`get_*()`函式和引數格式`set_*()`函式是 JSON 字串而提供的 XML 標記會是以物件表示。</span><span class="sxs-lookup"><span data-stu-id="df9db-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="df9db-124">目前沒有任何方法，將物件傳送中，但是可以讀取指定的動畫物件 (`get_OnXXXBehavior()`方法)。</span><span class="sxs-lookup"><span data-stu-id="df9db-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="df9db-125">以下是 JSON 字串 (不含引號分隔且妥善格式化) 代表按鈕，所觸發的動畫，但是調整它，並在同一時間淡出動畫面板：</span><span class="sxs-lookup"><span data-stu-id="df9db-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

<span data-ttu-id="df9db-126">下列 JavaScript 程式碼會將指派到這個 JSON descripting`OnClick`目前的擴充項的動畫並執行它：</span><span class="sxs-lookup"><span data-stu-id="df9db-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]


<span data-ttu-id="df9db-127">[![動畫會立即執行，不按滑鼠 （和以非常少的標記）](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="df9db-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="df9db-128">動畫會立即執行，而按一下滑鼠 （和以非常少的標記） ([按一下以檢視完整大小的影像](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="df9db-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="df9db-129">[上一頁](executing-animations-using-client-side-code-vb.md)
> [下一頁](animating-an-updatepanel-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="df9db-129">[Previous](executing-animations-using-client-side-code-vb.md)
[Next](animating-an-updatepanel-control-vb.md)</span></span>
