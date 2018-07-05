---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: 定位 ModalPopup (VB) |Microsoft Docs
author: wenz
description: AJAX Control Toolkit 之 ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應快顯。 控制項不提供的不過...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 1578a1bd6d5e3b595eba526552b8723daefb2ba1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387828"
---
<a name="positioning-a-modalpopup-vb"></a><span data-ttu-id="a8a37-104">定位 ModalPopup (VB)</span><span class="sxs-lookup"><span data-stu-id="a8a37-104">Positioning a ModalPopup (VB)</span></span>
====================
<span data-ttu-id="a8a37-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a8a37-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a8a37-106">[下載程式碼](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="a8a37-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span></span>

> <span data-ttu-id="a8a37-107">AJAX Control Toolkit 之 ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應快顯。</span><span class="sxs-lookup"><span data-stu-id="a8a37-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="a8a37-108">但是控制項不提供內建的功能，可以定位快顯視窗。</span><span class="sxs-lookup"><span data-stu-id="a8a37-108">However the control does not offer a built-in functionality to position the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="a8a37-109">總覽</span><span class="sxs-lookup"><span data-stu-id="a8a37-109">Overview</span></span>

<span data-ttu-id="a8a37-110">AJAX Control Toolkit 之 ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應快顯。</span><span class="sxs-lookup"><span data-stu-id="a8a37-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="a8a37-111">但是控制項不提供內建的功能，可以定位快顯視窗。</span><span class="sxs-lookup"><span data-stu-id="a8a37-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="a8a37-112">步驟</span><span class="sxs-lookup"><span data-stu-id="a8a37-112">Steps</span></span>

<span data-ttu-id="a8a37-113">若要啟動的 ASP.NET AJAX Control Toolkit 中，功能`ScriptManager`。</span><span class="sxs-lookup"><span data-stu-id="a8a37-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="a8a37-114">控制項必須放置在任何位置上 (但在`<form>`項目):</span><span class="sxs-lookup"><span data-stu-id="a8a37-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="a8a37-115">接下來，加入做為強制回應快顯面板。</span><span class="sxs-lookup"><span data-stu-id="a8a37-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="a8a37-116">按鈕來關閉快顯視窗：</span><span class="sxs-lookup"><span data-stu-id="a8a37-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="a8a37-117">每當在快顯視窗會顯示，它應該會位於網頁的特定位置。</span><span class="sxs-lookup"><span data-stu-id="a8a37-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="a8a37-118">這項工作，會建立用戶端的 JavaScript 函式。</span><span class="sxs-lookup"><span data-stu-id="a8a37-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="a8a37-119">它會先嘗試存取面板。</span><span class="sxs-lookup"><span data-stu-id="a8a37-119">It first tries to access the panel.</span></span> <span data-ttu-id="a8a37-120">如果成功，面板的位置會設定使用 CSS 和 JavaScript （在快顯視窗的位置會的變更）。</span><span class="sxs-lookup"><span data-stu-id="a8a37-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="a8a37-121">不過，`ModalPopupExtender`控制項也會嘗試快顯視窗的位置。</span><span class="sxs-lookup"><span data-stu-id="a8a37-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="a8a37-122">因此，JavaScript 程式碼重複將的快顯視窗中，而每個十分之一秒。</span><span class="sxs-lookup"><span data-stu-id="a8a37-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

<span data-ttu-id="a8a37-123">如您所見的傳回值`setTimeout()`JavaScript 方法儲存在全域變數。</span><span class="sxs-lookup"><span data-stu-id="a8a37-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="a8a37-124">若要停止重複的定位快顯視窗，視情況下，這可讓使用`clearTimeout()`方法：</span><span class="sxs-lookup"><span data-stu-id="a8a37-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

<span data-ttu-id="a8a37-125">現在剩下要執行的是讓瀏覽器呼叫在情況允許時，這些函式。</span><span class="sxs-lookup"><span data-stu-id="a8a37-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="a8a37-126">`movePanel()`觸發程序 [面板] 中，按一下此按鈕時，就必須呼叫 JavaScript 函式：</span><span class="sxs-lookup"><span data-stu-id="a8a37-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

<span data-ttu-id="a8a37-127">而`stopMoving()`函式派上用場時快顯視窗已關閉，這可能會觸發在`ModalPopupExtender`控制項：</span><span class="sxs-lookup"><span data-stu-id="a8a37-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]


<span data-ttu-id="a8a37-128">[![強制回應快顯視窗會出現在指定的位置](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a8a37-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="a8a37-129">強制回應快顯視窗會出現在指定的位置 ([按一下以檢視完整大小的影像](positioning-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a8a37-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a8a37-130">上一步</span><span class="sxs-lookup"><span data-stu-id="a8a37-130">Previous</span></span>](handling-postbacks-from-a-modalpopup-vb.md)
