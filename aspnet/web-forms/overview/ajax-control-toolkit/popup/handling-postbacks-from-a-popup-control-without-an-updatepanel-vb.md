---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: 處理回傳從快顯視窗控制項沒有 UpdatePanel (VB) |Microsoft 文件
author: wenz
description: AJAX Control Toolkit PopupControl extender 提供簡單的方式來啟動任何其他控制項時，觸發快顯視窗。 當回傳 su 中...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: c92083d4a57067c7b646721f21af059fc1e1e7d9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871359"
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a><span data-ttu-id="f0393-104">處理回傳從快顯視窗控制項沒有 UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="f0393-104">Handling Postbacks from A Popup Control Without an UpdatePanel (VB)</span></span>
====================
<span data-ttu-id="f0393-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f0393-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f0393-106">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f0393-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span></span>

> <span data-ttu-id="f0393-107">AJAX Control Toolkit PopupControl extender 提供簡單的方式來啟動任何其他控制項時，觸發快顯視窗。</span><span class="sxs-lookup"><span data-stu-id="f0393-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="f0393-108">回傳發生這類面板中，在頁面上有數個面板時很難判斷已按下的面板。</span><span class="sxs-lookup"><span data-stu-id="f0393-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>


## <a name="overview"></a><span data-ttu-id="f0393-109">總覽</span><span class="sxs-lookup"><span data-stu-id="f0393-109">Overview</span></span>

<span data-ttu-id="f0393-110">AJAX Control Toolkit PopupControl extender 提供簡單的方式來啟動任何其他控制項時，觸發快顯視窗。</span><span class="sxs-lookup"><span data-stu-id="f0393-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="f0393-111">回傳發生這類面板中，在頁面上有數個面板時很難判斷已按下的面板。</span><span class="sxs-lookup"><span data-stu-id="f0393-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="f0393-112">步驟</span><span class="sxs-lookup"><span data-stu-id="f0393-112">Steps</span></span>

<span data-ttu-id="f0393-113">當使用`PopupControl`回傳，但不需要`UpdatePanel`在頁面上，Control Toolkit 不提供一個方式來判斷哪些用戶端項目已經觸發輪流導致回傳快顯視窗。</span><span class="sxs-lookup"><span data-stu-id="f0393-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="f0393-114">不過小墩提供此案例中因應措施。</span><span class="sxs-lookup"><span data-stu-id="f0393-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="f0393-115">首先，以下是基本安裝： 這兩個觸發程序相同的快顯行事曆的兩個文字方塊。</span><span class="sxs-lookup"><span data-stu-id="f0393-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="f0393-116">兩個`PopupControlExtenders`將文字方塊和快顯結合在一起。</span><span class="sxs-lookup"><span data-stu-id="f0393-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="f0393-117">若要加入隱藏的表單欄位中的基本概念是&lt; `form` &gt;保留文字方塊啟動快顯功能表項目：</span><span class="sxs-lookup"><span data-stu-id="f0393-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="f0393-118">當網頁載入時，JavaScript 程式碼會將事件處理常式加入至這兩個文字方塊： 每當使用者在文字方塊中，其名稱會寫入的隱藏的欄位：</span><span class="sxs-lookup"><span data-stu-id="f0393-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

<span data-ttu-id="f0393-119">在伺服器端程式碼中，就必須讀取隱藏欄位的值。</span><span class="sxs-lookup"><span data-stu-id="f0393-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="f0393-120">隱藏的表單欄位是 trivial 操作，因為驗證隱藏的值的允許清單方法需要。</span><span class="sxs-lookup"><span data-stu-id="f0393-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="f0393-121">一旦已識別正確的文字方塊，從行事曆日期會寫入到其中。</span><span class="sxs-lookup"><span data-stu-id="f0393-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]


<span data-ttu-id="f0393-122">[![當使用者按一下的文字方塊中，就會出現日曆](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f0393-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="f0393-123">當使用者按一下的文字方塊中，就會出現日曆 ([按一下以檢視完整大小的影像](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f0393-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span></span>


<span data-ttu-id="f0393-124">[![按一下日期將它放在文字方塊中](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="f0393-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="f0393-125">按一下日期將它放在文字方塊中 ([按一下以檢視完整大小的影像](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="f0393-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f0393-126">上一步</span><span class="sxs-lookup"><span data-stu-id="f0393-126">Previous</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
