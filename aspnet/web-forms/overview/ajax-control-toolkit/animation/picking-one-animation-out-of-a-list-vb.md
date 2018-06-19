---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: 挑選清單 (VB) 超出一個動畫 |Microsoft 文件
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 架構也允許...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: f2bd1b3cc72595da7e8901786ea8415d7c1c524a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872061"
---
<a name="picking-one-animation-out-of-a-list-vb"></a><span data-ttu-id="4ade5-104">挑選清單 (VB) 超出一個動畫</span><span class="sxs-lookup"><span data-stu-id="4ade5-104">Picking One Animation Out Of a List (VB)</span></span>
====================
<span data-ttu-id="4ade5-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4ade5-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4ade5-106">[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="4ade5-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)</span></span>

> <span data-ttu-id="4ade5-107">動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="4ade5-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4ade5-108">此架構也可讓程式設計人員挑選一個動畫超出清單的動畫，根據一些 JavaScript 程式碼評估。</span><span class="sxs-lookup"><span data-stu-id="4ade5-108">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="4ade5-109">總覽</span><span class="sxs-lookup"><span data-stu-id="4ade5-109">Overview</span></span>

<span data-ttu-id="4ade5-110">動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="4ade5-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4ade5-111">此架構也可讓程式設計人員挑選一個動畫超出清單的動畫，根據一些 JavaScript 程式碼評估。</span><span class="sxs-lookup"><span data-stu-id="4ade5-111">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="4ade5-112">步驟</span><span class="sxs-lookup"><span data-stu-id="4ade5-112">Steps</span></span>

<span data-ttu-id="4ade5-113">首先，包括`ScriptManager`在頁面; 然後，ASP.NET AJAX 程式庫載入，因此您能夠使用控制項的工具組：</span><span class="sxs-lookup"><span data-stu-id="4ade5-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

<span data-ttu-id="4ade5-114">動畫會套用到面板中的文字看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="4ade5-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

<span data-ttu-id="4ade5-115">在 [面板] 中相關聯的 CSS 類別，定義好的背景色彩，也將面板固定的寬度設定：</span><span class="sxs-lookup"><span data-stu-id="4ade5-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

<span data-ttu-id="4ade5-116">接著，新增`AnimationExtender`到頁面上，提供`ID`、`TargetControlID`屬性和必要 `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="4ade5-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

<span data-ttu-id="4ade5-117">內`<Animations>`節點，請使用`<OnLoad>`頁面已完全載入後，執行動畫。</span><span class="sxs-lookup"><span data-stu-id="4ade5-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="4ade5-118">其中一個規則的動畫，而不是`<Case>`元素派上用場。</span><span class="sxs-lookup"><span data-stu-id="4ade5-118">Instead of one of the regular animations, the `<Case>` element comes into play.</span></span> <span data-ttu-id="4ade5-119">會評估其 SelectScript 屬性的值;傳回值必須是數字。</span><span class="sxs-lookup"><span data-stu-id="4ade5-119">The value of its SelectScript attribute is evaluated; the return value must be numerical.</span></span> <span data-ttu-id="4ade5-120">根據這個數字，其中一個內 subanimations&lt;案例&gt;執行。</span><span class="sxs-lookup"><span data-stu-id="4ade5-120">Depending on this number, one of the subanimations within &lt;Case&gt; is executed.</span></span> <span data-ttu-id="4ade5-121">比方說，如果 SelectScript 判斷值為 2，Control Toolkit 執行內的第三個動畫&lt;案例&gt;（從 0 開始）。</span><span class="sxs-lookup"><span data-stu-id="4ade5-121">For instance, if SelectScript evaluates to 2, the Control Toolkit runs the third animation within &lt;Case&gt; (counting starts at 0).</span></span>

<span data-ttu-id="4ade5-122">下列標記會定義三個 subanimations： 調整大小的寬度、 高度調整大小和淡出。JavaScript 程式碼 (`Math.floor(3 * Math.random())`) 然後挑選一個介於 0 和 2，使其中的三個動畫執行：</span><span class="sxs-lookup"><span data-stu-id="4ade5-122">The following markup defines three subanimations: Resizing the width, resizing the height, and fading out. The JavaScript code (`Math.floor(3 * Math.random())`) then picks a number between 0 and 2, so that one of the three animations is run:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]


<span data-ttu-id="4ade5-123">[![其中一個可能的三個動畫： 面板變寬](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4ade5-123">[![One of the possible three animations: The panel gets wider](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)</span></span>

<span data-ttu-id="4ade5-124">其中一個可能的三個動畫： 面板變寬 ([按一下以檢視完整大小的影像](picking-one-animation-out-of-a-list-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4ade5-124">One of the possible three animations: The panel gets wider ([Click to view full-size image](picking-one-animation-out-of-a-list-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4ade5-125">[上一頁](animation-depending-on-a-condition-vb.md)
> [下一頁](animating-in-response-to-user-interaction-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4ade5-125">[Previous](animation-depending-on-a-condition-vb.md)
[Next](animating-in-response-to-user-interaction-vb.md)</span></span>
