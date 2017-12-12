---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: "以動態方式加入 Accordion 窗格 (C#) |Microsoft 文件"
author: wenz
description: "AJAX Control Toolkit Accordion 控制項提供多個窗格，並可讓使用者一次顯示其中一個。 面板通常會宣告 w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: 1c6af79186ca21082647beec500c47974a47c35a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-adding-an-accordion-pane-c"></a><span data-ttu-id="02bc4-104">以動態方式加入 Accordion 窗格 (C#)</span><span class="sxs-lookup"><span data-stu-id="02bc4-104">Dynamically Adding An Accordion Pane (C#)</span></span>
====================
<span data-ttu-id="02bc4-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="02bc4-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="02bc4-106">[下載程式碼](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="02bc4-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)</span></span>

> <span data-ttu-id="02bc4-107">AJAX Control Toolkit Accordion 控制項提供多個窗格，並可讓使用者一次顯示其中一個。</span><span class="sxs-lookup"><span data-stu-id="02bc4-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="02bc4-108">面板通常會宣告本身，在頁面內，但伺服器端程式碼可用來達到相同的結果。</span><span class="sxs-lookup"><span data-stu-id="02bc4-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>


## <a name="overview"></a><span data-ttu-id="02bc4-109">概觀</span><span class="sxs-lookup"><span data-stu-id="02bc4-109">Overview</span></span>

<span data-ttu-id="02bc4-110">AJAX Control Toolkit Accordion 控制項提供多個窗格，並可讓使用者一次顯示其中一個。</span><span class="sxs-lookup"><span data-stu-id="02bc4-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="02bc4-111">面板通常會宣告本身，在頁面內，但伺服器端程式碼可用來達到相同的結果。</span><span class="sxs-lookup"><span data-stu-id="02bc4-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="02bc4-112">步驟</span><span class="sxs-lookup"><span data-stu-id="02bc4-112">Steps</span></span>

<span data-ttu-id="02bc4-113">Accordion 控制項會公開伺服器端程式碼的所有重要屬性。</span><span class="sxs-lookup"><span data-stu-id="02bc4-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="02bc4-114">在其他方面，`Panes`屬性授與存取權窗格 Accordion 所組成的集合。</span><span class="sxs-lookup"><span data-stu-id="02bc4-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="02bc4-115">沒有類型的每個窗格`AccordionPane`。</span><span class="sxs-lookup"><span data-stu-id="02bc4-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="02bc4-116">因此，它是 trivial 建立這類的窗格：</span><span class="sxs-lookup"><span data-stu-id="02bc4-116">It is therefore trivial to create such a pane:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

<span data-ttu-id="02bc4-117">`HeaderContainer`屬性`AccordionPane`提供 ASP.NET 控制項的窗格; 標頭區段內存取`ContentContainer`屬性`AccordionPane`沒有相同的窗格的 [內容] 區段。</span><span class="sxs-lookup"><span data-stu-id="02bc4-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="02bc4-118">這可讓 ASP.NET 程式碼，將內容加入至 [] 窗格：</span><span class="sxs-lookup"><span data-stu-id="02bc4-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

<span data-ttu-id="02bc4-119">最後，窗格必須新增至`Panes`accordion 設定的集合：</span><span class="sxs-lookup"><span data-stu-id="02bc4-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

<span data-ttu-id="02bc4-120">以下是完整的伺服器端程式碼加入 accordion 設定控制項的兩個窗格：</span><span class="sxs-lookup"><span data-stu-id="02bc4-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

<span data-ttu-id="02bc4-121">只有遺漏的項目是 Accordion 本身，這取決於是否存在的 ASP.NET`ScriptManager`控制項：</span><span class="sxs-lookup"><span data-stu-id="02bc4-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

<span data-ttu-id="02bc4-122">若要完成此範例，這兩個 accordion 設定控制項中所參考的 CSS 類別會提供瀏覽器的樣式資訊：</span><span class="sxs-lookup"><span data-stu-id="02bc4-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]


<span data-ttu-id="02bc4-123">[![依伺服器端程式碼以動態方式加入 accordion 設定中的資料](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="02bc4-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)</span></span>

<span data-ttu-id="02bc4-124">依伺服器端程式碼以動態方式加入 accordion 設定中的資料 ([按一下以檢視完整大小的影像](dynamically-adding-an-accordion-pane-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="02bc4-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="02bc4-125">[上一頁](databinding-to-an-accordion-cs.md)
[下一頁](databinding-to-an-accordion-vb.md)</span><span class="sxs-lookup"><span data-stu-id="02bc4-125">[Previous](databinding-to-an-accordion-cs.md)
[Next](databinding-to-an-accordion-vb.md)</span></span>
