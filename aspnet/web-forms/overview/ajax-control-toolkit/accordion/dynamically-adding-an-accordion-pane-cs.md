---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: 以動態方式新增 Accordion 窗格 (C#) |Microsoft Docs
author: wenz
description: 在 AJAX Control Toolkit Accordion 控制項提供多個窗格，並可讓使用者一次顯示其中一個。 面板通常宣告 w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: 555a8a5cdd701bba3d41c3ff3313fe119a350e53
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381528"
---
<a name="dynamically-adding-an-accordion-pane-c"></a><span data-ttu-id="8628f-104">以動態方式新增 Accordion 窗格 (C#)</span><span class="sxs-lookup"><span data-stu-id="8628f-104">Dynamically Adding An Accordion Pane (C#)</span></span>
====================
<span data-ttu-id="8628f-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8628f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8628f-106">[下載程式碼](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="8628f-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)</span></span>

> <span data-ttu-id="8628f-107">在 AJAX Control Toolkit Accordion 控制項提供多個窗格，並可讓使用者一次顯示其中一個。</span><span class="sxs-lookup"><span data-stu-id="8628f-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="8628f-108">面板通常會宣告本身，在頁面中，但伺服器端程式碼可用來達到相同的結果。</span><span class="sxs-lookup"><span data-stu-id="8628f-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>


## <a name="overview"></a><span data-ttu-id="8628f-109">總覽</span><span class="sxs-lookup"><span data-stu-id="8628f-109">Overview</span></span>

<span data-ttu-id="8628f-110">在 AJAX Control Toolkit Accordion 控制項提供多個窗格，並可讓使用者一次顯示其中一個。</span><span class="sxs-lookup"><span data-stu-id="8628f-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="8628f-111">面板通常會宣告本身，在頁面中，但伺服器端程式碼可用來達到相同的結果。</span><span class="sxs-lookup"><span data-stu-id="8628f-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="8628f-112">步驟</span><span class="sxs-lookup"><span data-stu-id="8628f-112">Steps</span></span>

<span data-ttu-id="8628f-113">Accordion 控制項公開 （expose) 所有重要的屬性，以伺服器端程式碼。</span><span class="sxs-lookup"><span data-stu-id="8628f-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="8628f-114">除此之外，`Panes`屬性授與存取權的組成 Accordion 窗格集合。</span><span class="sxs-lookup"><span data-stu-id="8628f-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="8628f-115">沒有類型的每個窗格`AccordionPane`。</span><span class="sxs-lookup"><span data-stu-id="8628f-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="8628f-116">因此，它為 trivial 建立這類的窗格：</span><span class="sxs-lookup"><span data-stu-id="8628f-116">It is therefore trivial to create such a pane:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

<span data-ttu-id="8628f-117">`HeaderContainer`的屬性`AccordionPane`提供的存取權 窗格中，標頭區段內的 ASP.NET 控制項`ContentContainer`屬性`AccordionPane`對窗格的 內容 區段執行相同作業。</span><span class="sxs-lookup"><span data-stu-id="8628f-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="8628f-118">這可讓 ASP.NET 程式碼，將內容新增至窗格：</span><span class="sxs-lookup"><span data-stu-id="8628f-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

<span data-ttu-id="8628f-119">最後，您必須將窗格新增至`Panes`Accordion 的集合：</span><span class="sxs-lookup"><span data-stu-id="8628f-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

<span data-ttu-id="8628f-120">以下是完整的伺服器端程式碼新增至 Accordion 控制項的兩個窗格：</span><span class="sxs-lookup"><span data-stu-id="8628f-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

<span data-ttu-id="8628f-121">唯一的遺漏項目是 Accordion 本身，這取決於是否存在 ASP.NET`ScriptManager`控制項：</span><span class="sxs-lookup"><span data-stu-id="8628f-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

<span data-ttu-id="8628f-122">若要完成此範例，兩個 Accordion 控制項中所參考的 CSS 類別會提供瀏覽器的樣式資訊：</span><span class="sxs-lookup"><span data-stu-id="8628f-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]


<span data-ttu-id="8628f-123">[![伺服器端程式碼已以動態方式新增 accordion 中的資料](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8628f-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)</span></span>

<span data-ttu-id="8628f-124">伺服器端程式碼已以動態方式新增 accordion 中的資料 ([按一下以檢視完整大小的影像](dynamically-adding-an-accordion-pane-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8628f-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8628f-125">[上一頁](databinding-to-an-accordion-cs.md)
> [下一頁](databinding-to-an-accordion-vb.md)</span><span class="sxs-lookup"><span data-stu-id="8628f-125">[Previous](databinding-to-an-accordion-cs.md)
[Next](databinding-to-an-accordion-vb.md)</span></span>
