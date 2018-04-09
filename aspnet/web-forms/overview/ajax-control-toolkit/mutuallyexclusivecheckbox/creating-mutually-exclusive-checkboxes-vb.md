---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: 建立互斥的核取方塊 (VB) |Microsoft 文件
author: wenz
description: 只有其中一個的一組選項可能會被選取時，通常用選項按鈕。 雖然會有缺點： 一旦選取一個選項按鈕群組中的，...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: cdb93a080fb62cfdc7e3ff0604a1447e2bb99071
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="creating-mutually-exclusive-checkboxes-vb"></a><span data-ttu-id="bf2ce-104">建立互斥的核取方塊 (VB)</span><span class="sxs-lookup"><span data-stu-id="bf2ce-104">Creating Mutually Exclusive Checkboxes (VB)</span></span>
====================
<span data-ttu-id="bf2ce-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="bf2ce-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="bf2ce-106">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="bf2ce-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span></span>

> <span data-ttu-id="bf2ce-107">只有其中一個的一組選項可能會被選取時，通常用選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="bf2ce-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="bf2ce-108">雖然會有缺點： 一旦選取一個選項按鈕群組中的，不可能取消選取所有選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="bf2ce-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="bf2ce-109">核取方塊可以是未檢查在任何時間，但是不會互斥。</span><span class="sxs-lookup"><span data-stu-id="bf2ce-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="bf2ce-110">本教學課程提供了最佳的這兩種方法： 互斥的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="bf2ce-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>


## <a name="overview"></a><span data-ttu-id="bf2ce-111">總覽</span><span class="sxs-lookup"><span data-stu-id="bf2ce-111">Overview</span></span>

<span data-ttu-id="bf2ce-112">只有其中一個的一組選項可能會被選取時，通常用選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="bf2ce-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="bf2ce-113">雖然會有缺點： 一旦選取一個選項按鈕群組中的，不可能取消選取所有選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="bf2ce-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="bf2ce-114">核取方塊可以是未檢查在任何時間，但是不會互斥。</span><span class="sxs-lookup"><span data-stu-id="bf2ce-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="bf2ce-115">本教學課程提供了最佳的這兩種方法： 互斥的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="bf2ce-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="bf2ce-116">步驟</span><span class="sxs-lookup"><span data-stu-id="bf2ce-116">Steps</span></span>

<span data-ttu-id="bf2ce-117">ASP.NET AJAX Control Toolkit 包含 MutuallyExclusiveCheckBox 擴充項。</span><span class="sxs-lookup"><span data-stu-id="bf2ce-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="bf2ce-118">這可讓程式設計人員指派至群組名稱的任何核取方塊 (`Key`屬性)。</span><span class="sxs-lookup"><span data-stu-id="bf2ce-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="bf2ce-119">從同一個群組內的所有核取方塊，只有一個可選取一次。</span><span class="sxs-lookup"><span data-stu-id="bf2ce-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="bf2ce-120">讓我們開始新的 ASP.NET 網頁上將兩個核取方塊。</span><span class="sxs-lookup"><span data-stu-id="bf2ce-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="bf2ce-121">可以有多個，但其中兩個已足夠來示範原則：</span><span class="sxs-lookup"><span data-stu-id="bf2ce-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

<span data-ttu-id="bf2ce-122">這兩個核取方塊，進行 MutuallyExclusiveCheckBoxExtender 控制項必須放在頁面上。</span><span class="sxs-lookup"><span data-stu-id="bf2ce-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="bf2ce-123">這兩個索引鍵屬性需要有相同的值，如同 HTML 選項按鈕項目的屬性必須是代表其所屬的群組相同的值。</span><span class="sxs-lookup"><span data-stu-id="bf2ce-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="bf2ce-124">擴充項的 TargetControlID 屬性指向的核取方塊的識別碼。</span><span class="sxs-lookup"><span data-stu-id="bf2ce-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

<span data-ttu-id="bf2ce-125">最後，包含 ASP.NET AJAX`ScriptManager`所需的 ASP.NET AJAX Control Toolkit 的所有項目：</span><span class="sxs-lookup"><span data-stu-id="bf2ce-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

<span data-ttu-id="bf2ce-126">儲存並執行頁面： 您可以檢查並取消核取這兩個核取方塊，但是沒有這兩個核取方塊能夠檢查。</span><span class="sxs-lookup"><span data-stu-id="bf2ce-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>


<span data-ttu-id="bf2ce-127">[![只有一個核取方塊可檢查一次](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bf2ce-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span></span>

<span data-ttu-id="bf2ce-128">只有一個核取方塊可檢查一次 ([按一下以檢視完整大小的影像](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="bf2ce-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bf2ce-129">上一步</span><span class="sxs-lookup"><span data-stu-id="bf2ce-129">Previous</span></span>](creating-mutually-exclusive-checkboxes-cs.md)
