---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
title: 啟動 從伺服器程式碼 (C#) 強制回應的快顯視窗 |Microsoft 文件
author: wenz
description: AJAX Control Toolkit ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應的快顯。 但是某些情況下會需要該 t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2f67d8ef-73ca-447d-a0cc-6e3168431e6a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 04bb056ee29065a472a70d480568b897a789ae59
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872893"
---
<a name="launching-a-modal-popup-window-from-server-code-c"></a><span data-ttu-id="98afa-104">啟動強制回應的快顯視窗，從伺服器程式碼 (C#)</span><span class="sxs-lookup"><span data-stu-id="98afa-104">Launching a Modal Popup Window from Server Code (C#)</span></span>
====================
<span data-ttu-id="98afa-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="98afa-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="98afa-106">[下載程式碼](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="98afa-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)</span></span>

> <span data-ttu-id="98afa-107">AJAX Control Toolkit ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應的快顯。</span><span class="sxs-lookup"><span data-stu-id="98afa-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="98afa-108">不過，某些情況下需要開啟強制回應的快顯視窗會觸發伺服器端上。</span><span class="sxs-lookup"><span data-stu-id="98afa-108">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>


## <a name="overview"></a><span data-ttu-id="98afa-109">總覽</span><span class="sxs-lookup"><span data-stu-id="98afa-109">Overview</span></span>

<span data-ttu-id="98afa-110">AJAX Control Toolkit ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應的快顯。</span><span class="sxs-lookup"><span data-stu-id="98afa-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="98afa-111">不過，某些情況下需要開啟強制回應的快顯視窗會觸發伺服器端上。</span><span class="sxs-lookup"><span data-stu-id="98afa-111">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="steps"></a><span data-ttu-id="98afa-112">步驟</span><span class="sxs-lookup"><span data-stu-id="98afa-112">Steps</span></span>

<span data-ttu-id="98afa-113">首先，ASP.NET 按鈕 web 控制項才能示範 ModalPopup 控制的運作方式。</span><span class="sxs-lookup"><span data-stu-id="98afa-113">First of all, an ASP.NET Button web control is required to demonstrate how the ModalPopup control works.</span></span> <span data-ttu-id="98afa-114">將這類按鈕內加入&lt;表單&gt;新頁面上的元素：</span><span class="sxs-lookup"><span data-stu-id="98afa-114">Add such a button within the &lt;form&gt; element on a new page:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample1.aspx)]

<span data-ttu-id="98afa-115">然後，您必須標記您想要建立快顯功能表。</span><span class="sxs-lookup"><span data-stu-id="98afa-115">Then, you need the markup for the popup you want to create.</span></span> <span data-ttu-id="98afa-116">它定義為`<asp:Panel>`控制項，並請確定它包含按鈕控制項。</span><span class="sxs-lookup"><span data-stu-id="98afa-116">Define it as an `<asp:Panel>` control and make sure that it includes a Button control.</span></span> <span data-ttu-id="98afa-117">ModalPopup 控制項提供的功能，使這類按鈕來關閉快顯視窗。否則便沒有簡單的方法，讓它會消失。</span><span class="sxs-lookup"><span data-stu-id="98afa-117">The ModalPopup control offers the functionality to make such a button close the popup; otherwise there is no easy way to let it vanish.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample2.aspx)]

<span data-ttu-id="98afa-118">接下來將 ASP.NET AJAX toolkit ModalPopup 控制項加入頁面。</span><span class="sxs-lookup"><span data-stu-id="98afa-118">Next add the ModalPopup control from the ASP.NET AJAX Toolkit to the page.</span></span> <span data-ttu-id="98afa-119">設定屬性載入控制項的按鈕、 [] 按鈕便會消失，以及實際的快顯視窗的識別碼。</span><span class="sxs-lookup"><span data-stu-id="98afa-119">Set properties for the button which loads the control, the button which makes it disappear, and the ID of the actual popup.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample3.aspx)]

<span data-ttu-id="98afa-120">如同所有 ASP.NET AJAX; 為基礎的 web 網頁指令碼管理員，才能載入必要的 JavaScript 程式庫，針對不同的目標瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="98afa-120">As with all web pages based on ASP.NET AJAX; the Script Manager is required to load the necessary JavaScript libraries for the different target browsers:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample4.aspx)]

<span data-ttu-id="98afa-121">在瀏覽器中執行範例。</span><span class="sxs-lookup"><span data-stu-id="98afa-121">Run the example in the browser.</span></span> <span data-ttu-id="98afa-122">當您按一下按鈕，強制回應的快顯視窗出現。</span><span class="sxs-lookup"><span data-stu-id="98afa-122">When you click on the button, the modal popup appears.</span></span> <span data-ttu-id="98afa-123">為了達到使用伺服器端程式碼相同的效果，新的按鈕，則需要：</span><span class="sxs-lookup"><span data-stu-id="98afa-123">In order to achieve the same effect using server-side code, a new button is required:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample5.aspx)]

<span data-ttu-id="98afa-124">如您所見，按一下按鈕會產生回傳，並且執行`ServerButton_Click()`在伺服器上的方法。</span><span class="sxs-lookup"><span data-stu-id="98afa-124">As you can see, a click on the button generates a postback and executes the `ServerButton_Click()` method on the server.</span></span> <span data-ttu-id="98afa-125">這種方法呼叫的 JavaScript 函式`launchModal()`執行很精確，JavaScript 函式會執行一次載入頁面：</span><span class="sxs-lookup"><span data-stu-id="98afa-125">In this method, a JavaScript function called `launchModal()` is executed to be exact, the JavaScript function will be executed once the page has been loaded:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample6.aspx)]

<span data-ttu-id="98afa-126">作業的`launchModal()`是顯示 ModalPopup。</span><span class="sxs-lookup"><span data-stu-id="98afa-126">The job of `launchModal()` is to display the ModalPopup.</span></span> <span data-ttu-id="98afa-127">`launchModal()`完整的 HTML 頁面已載入後，會執行函式。</span><span class="sxs-lookup"><span data-stu-id="98afa-127">The `launchModal()` function is executed once the complete HTML page has been loaded.</span></span> <span data-ttu-id="98afa-128">在該時間點，不過，ASP.NET AJAX framework 尚未完全載入。</span><span class="sxs-lookup"><span data-stu-id="98afa-128">At that moment, however, the ASP.NET AJAX framework has not been fully loaded yet.</span></span> <span data-ttu-id="98afa-129">因此，`launchModal()`函式只會將 ModalPopup 控制項必須在稍後顯示的變數：</span><span class="sxs-lookup"><span data-stu-id="98afa-129">Therefore, the `launchModal()` function just sets a variable that the ModalPopup control must be shown later on:</span></span>

[!code-html[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample7.html)]

<span data-ttu-id="98afa-130">`pageLoad()` JavaScript 函式是執行 ASP.NET AJAX 已完全載入後的特殊函式。</span><span class="sxs-lookup"><span data-stu-id="98afa-130">The `pageLoad()` JavaScript function is a special function that gets executed once ASP.NET AJAX has been fully loaded.</span></span> <span data-ttu-id="98afa-131">因此我們將程式碼加入這個函數來顯示 ModalPopup 控制項，但是只有`launchModal()`之前已呼叫：</span><span class="sxs-lookup"><span data-stu-id="98afa-131">Therefore we add code to this function to show the ModalPopup control, but only if `launchModal()` has been called before:</span></span>

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample8.js)]

<span data-ttu-id="98afa-132">`$find()`函式尋找具名的項目頁面上，並且預期會做為參數的伺服器端識別碼。</span><span class="sxs-lookup"><span data-stu-id="98afa-132">The `$find()` function is looking for a named element on the page and expects the server-side ID as a parameter.</span></span> <span data-ttu-id="98afa-133">因此，`$find("mpe")`傳回 ModalPopup 控制項的用戶端表示法，則其`show()`方法可讓顯示的快顯視窗。</span><span class="sxs-lookup"><span data-stu-id="98afa-133">Therefore, `$find("mpe")` returns the client representation of the ModalPopup control; its `show()` method lets the popup appear.</span></span>


<span data-ttu-id="98afa-134">[![強制回應的快顯視窗出現時按下的按鈕](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="98afa-134">[![The modal popup appears when either of the buttons is clicked](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="98afa-135">強制回應的快顯視窗出現時的按鈕按下 ([按一下以檢視完整大小的影像](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="98afa-135">The modal popup appears when either of the buttons is clicked ([Click to view full-size image](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="98afa-136">下一步</span><span class="sxs-lookup"><span data-stu-id="98afa-136">Next</span></span>](using-modalpopup-with-a-repeater-control-cs.md)
