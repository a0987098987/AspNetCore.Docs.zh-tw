---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: 處理沒有 UpdatePanel (VB) 的快顯視窗控制項回傳 |Microsoft Docs
author: wenz
description: PopupControl 擴充項在 AJAX Control Toolkit 提供簡單的方式，來啟動任何其他控制項時，觸發快顯視窗。 當發生回傳時 su 中...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 8530d62b931782af662cd6445db78d666b32e329
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808190"
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a><span data-ttu-id="aa69d-104">處理沒有 UpdatePanel (VB) 的快顯視窗控制項回傳</span><span class="sxs-lookup"><span data-stu-id="aa69d-104">Handling Postbacks from A Popup Control Without an UpdatePanel (VB)</span></span>
====================
<span data-ttu-id="aa69d-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="aa69d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="aa69d-106">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="aa69d-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span></span>

> <span data-ttu-id="aa69d-107">PopupControl 擴充項在 AJAX Control Toolkit 提供簡單的方式，來啟動任何其他控制項時，觸發快顯視窗。</span><span class="sxs-lookup"><span data-stu-id="aa69d-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="aa69d-108">回傳，就會發生這類面板中，並在頁面上有數個面板時很難判斷按下的面板。</span><span class="sxs-lookup"><span data-stu-id="aa69d-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>


## <a name="overview"></a><span data-ttu-id="aa69d-109">總覽</span><span class="sxs-lookup"><span data-stu-id="aa69d-109">Overview</span></span>

<span data-ttu-id="aa69d-110">PopupControl 擴充項在 AJAX Control Toolkit 提供簡單的方式，來啟動任何其他控制項時，觸發快顯視窗。</span><span class="sxs-lookup"><span data-stu-id="aa69d-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="aa69d-111">回傳，就會發生這類面板中，並在頁面上有數個面板時很難判斷按下的面板。</span><span class="sxs-lookup"><span data-stu-id="aa69d-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="aa69d-112">步驟</span><span class="sxs-lookup"><span data-stu-id="aa69d-112">Steps</span></span>

<span data-ttu-id="aa69d-113">使用時`PopupControl`回傳，但無須`UpdatePanel`在頁面上，Control Toolkit 並未提供一個方式來判斷哪一個用戶端項目已觸發的快顯視窗接著造成回傳。</span><span class="sxs-lookup"><span data-stu-id="aa69d-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="aa69d-114">不過小秘訣提供此案例的因應措施。</span><span class="sxs-lookup"><span data-stu-id="aa69d-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="aa69d-115">首先，以下是基本安裝： 這兩個觸發程序相同的快顯行事曆的兩個文字方塊。</span><span class="sxs-lookup"><span data-stu-id="aa69d-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="aa69d-116">兩個`PopupControlExtenders`結合文字方塊和快顯視窗。</span><span class="sxs-lookup"><span data-stu-id="aa69d-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="aa69d-117">基本構想是將隱藏的表單欄位中加入&lt; `form` &gt;保留啟動快顯文字方塊中的項目：</span><span class="sxs-lookup"><span data-stu-id="aa69d-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="aa69d-118">當網頁載入時，JavaScript 程式碼會將事件處理常式加入至這兩個文字方塊： 每當使用者按一下文字方塊中，其名稱會寫入其中的隱藏的欄位：</span><span class="sxs-lookup"><span data-stu-id="aa69d-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

<span data-ttu-id="aa69d-119">在伺服器端程式碼中，您必須讀取隱藏欄位的值。</span><span class="sxs-lookup"><span data-stu-id="aa69d-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="aa69d-120">隱藏的表單欄位是 trivial 操作，因為驗證隱藏的值的允許清單方法需要。</span><span class="sxs-lookup"><span data-stu-id="aa69d-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="aa69d-121">一旦已識別正確的文字方塊中，從行事曆的日期會寫入到其中。</span><span class="sxs-lookup"><span data-stu-id="aa69d-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]


<span data-ttu-id="aa69d-122">[![當使用者按一下文字方塊，即出現日曆](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="aa69d-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="aa69d-123">當使用者按一下文字方塊，即出現日曆 ([按一下以檢視完整大小的影像](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="aa69d-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span></span>


<span data-ttu-id="aa69d-124">[![按一下某個日期將它放在文字方塊中](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="aa69d-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="aa69d-125">按一下某個日期將它放在文字方塊中 ([按一下以檢視完整大小的影像](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="aa69d-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="aa69d-126">上一步</span><span class="sxs-lookup"><span data-stu-id="aa69d-126">Previous</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
