---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: 以動態方式加入 Accordion 窗格 (VB) |Microsoft 文件
author: wenz
description: AJAX Control Toolkit Accordion 控制項提供多個窗格，並可讓使用者一次顯示其中一個。 面板通常會宣告 w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: 68c60ba6d4be5eb6709f7558d6be4165f8232a4f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="dynamically-adding-an-accordion-pane-vb"></a><span data-ttu-id="59d66-104">以動態方式加入 Accordion 窗格 (VB)</span><span class="sxs-lookup"><span data-stu-id="59d66-104">Dynamically Adding An Accordion Pane (VB)</span></span>
====================
<span data-ttu-id="59d66-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="59d66-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="59d66-106">[下載程式碼](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="59d66-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span></span>

> <span data-ttu-id="59d66-107">AJAX Control Toolkit Accordion 控制項提供多個窗格，並可讓使用者一次顯示其中一個。</span><span class="sxs-lookup"><span data-stu-id="59d66-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="59d66-108">面板通常會宣告本身，在頁面內，但伺服器端程式碼可用來達到相同的結果。</span><span class="sxs-lookup"><span data-stu-id="59d66-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>


## <a name="overview"></a><span data-ttu-id="59d66-109">總覽</span><span class="sxs-lookup"><span data-stu-id="59d66-109">Overview</span></span>

<span data-ttu-id="59d66-110">AJAX Control Toolkit Accordion 控制項提供多個窗格，並可讓使用者一次顯示其中一個。</span><span class="sxs-lookup"><span data-stu-id="59d66-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="59d66-111">面板通常會宣告本身，在頁面內，但伺服器端程式碼可用來達到相同的結果。</span><span class="sxs-lookup"><span data-stu-id="59d66-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="59d66-112">步驟</span><span class="sxs-lookup"><span data-stu-id="59d66-112">Steps</span></span>

<span data-ttu-id="59d66-113">Accordion 控制項會公開伺服器端程式碼的所有重要屬性。</span><span class="sxs-lookup"><span data-stu-id="59d66-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="59d66-114">在其他方面，`Panes`屬性授與存取權窗格 Accordion 所組成的集合。</span><span class="sxs-lookup"><span data-stu-id="59d66-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="59d66-115">沒有類型的每個窗格`AccordionPane`。</span><span class="sxs-lookup"><span data-stu-id="59d66-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="59d66-116">因此，它是 trivial 建立這類的窗格：</span><span class="sxs-lookup"><span data-stu-id="59d66-116">It is therefore trivial to create such a pane:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

<span data-ttu-id="59d66-117">`HeaderContainer`屬性`AccordionPane`提供 ASP.NET 控制項的窗格; 標頭區段內存取`ContentContainer`屬性`AccordionPane`沒有相同的窗格的 [內容] 區段。</span><span class="sxs-lookup"><span data-stu-id="59d66-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="59d66-118">這可讓 ASP.NET 程式碼，將內容加入至 [] 窗格：</span><span class="sxs-lookup"><span data-stu-id="59d66-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

<span data-ttu-id="59d66-119">最後，窗格必須新增至`Panes`accordion 設定的集合：</span><span class="sxs-lookup"><span data-stu-id="59d66-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

<span data-ttu-id="59d66-120">以下是完整的伺服器端程式碼加入 accordion 設定控制項的兩個窗格：</span><span class="sxs-lookup"><span data-stu-id="59d66-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

<span data-ttu-id="59d66-121">只有遺漏的項目是 Accordion 本身，這取決於是否存在的 ASP.NET`ScriptManager`控制項：</span><span class="sxs-lookup"><span data-stu-id="59d66-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

<span data-ttu-id="59d66-122">若要完成此範例，這兩個 accordion 設定控制項中所參考的 CSS 類別會提供瀏覽器的樣式資訊：</span><span class="sxs-lookup"><span data-stu-id="59d66-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


<span data-ttu-id="59d66-123">[![依伺服器端程式碼以動態方式加入 accordion 設定中的資料](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="59d66-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span></span>

<span data-ttu-id="59d66-124">依伺服器端程式碼以動態方式加入 accordion 設定中的資料 ([按一下以檢視完整大小的影像](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="59d66-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="59d66-125">上一步</span><span class="sxs-lookup"><span data-stu-id="59d66-125">Previous</span></span>](databinding-to-an-accordion-vb.md)
