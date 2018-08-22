---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: 建立互斥的核取方塊 (VB) |Microsoft Docs
author: wenz
description: 只有其中一組選項可能會被選取時，通常用選項按鈕。 不過是一項缺點，： 一旦選取一個選項按鈕群組中的，...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: fd338b622779b64dd59f9cf6f3e2365ef5cb3ffb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830161"
---
<a name="creating-mutually-exclusive-checkboxes-vb"></a><span data-ttu-id="d066f-104">建立互斥的核取方塊 (VB)</span><span class="sxs-lookup"><span data-stu-id="d066f-104">Creating Mutually Exclusive Checkboxes (VB)</span></span>
====================
<span data-ttu-id="d066f-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d066f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d066f-106">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="d066f-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span></span>

> <span data-ttu-id="d066f-107">只有其中一組選項可能會被選取時，通常用選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="d066f-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="d066f-108">不過是一項缺點，： 一旦選取一個選項按鈕群組中的，不能夠取消選取所有選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="d066f-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="d066f-109">核取方塊可以隨時取消勾選，不過不會互斥。</span><span class="sxs-lookup"><span data-stu-id="d066f-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="d066f-110">本教學課程提供最佳的這兩種方法： 互斥的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="d066f-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>


## <a name="overview"></a><span data-ttu-id="d066f-111">總覽</span><span class="sxs-lookup"><span data-stu-id="d066f-111">Overview</span></span>

<span data-ttu-id="d066f-112">只有其中一組選項可能會被選取時，通常用選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="d066f-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="d066f-113">不過是一項缺點，： 一旦選取一個選項按鈕群組中的，不能夠取消選取所有選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="d066f-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="d066f-114">核取方塊可以隨時取消勾選，不過不會互斥。</span><span class="sxs-lookup"><span data-stu-id="d066f-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="d066f-115">本教學課程提供最佳的這兩種方法： 互斥的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="d066f-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="d066f-116">步驟</span><span class="sxs-lookup"><span data-stu-id="d066f-116">Steps</span></span>

<span data-ttu-id="d066f-117">ASP.NET AJAX Control Toolkit 包含 MutuallyExclusiveCheckBox 擴充項。</span><span class="sxs-lookup"><span data-stu-id="d066f-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="d066f-118">這可讓程式設計人員指派至群組名稱的任何核取方塊 (`Key`屬性)。</span><span class="sxs-lookup"><span data-stu-id="d066f-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="d066f-119">從相同群組內的所有核取方塊，只有一個可選取一次。</span><span class="sxs-lookup"><span data-stu-id="d066f-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="d066f-120">讓我們開始將新的 ASP.NET 網頁上的兩個核取方塊。</span><span class="sxs-lookup"><span data-stu-id="d066f-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="d066f-121">可以有多個，但其中兩個足以示範原則：</span><span class="sxs-lookup"><span data-stu-id="d066f-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

<span data-ttu-id="d066f-122">針對這兩個核取方塊，則必須將 MutuallyExclusiveCheckBoxExtender 控制項放在頁面上。</span><span class="sxs-lookup"><span data-stu-id="d066f-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="d066f-123">這兩個索引鍵屬性必須有相同的值，就如同 HTML 選項按鈕項目的屬性必須是代表其所屬的群組相同的值。</span><span class="sxs-lookup"><span data-stu-id="d066f-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="d066f-124">擴充項的 TargetControlID 屬性指向的核取方塊的識別碼。</span><span class="sxs-lookup"><span data-stu-id="d066f-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

<span data-ttu-id="d066f-125">最後，包括 ASP.NET AJAX`ScriptManager`所需的 ASP.NET AJAX Control Toolkit 中的所有項目：</span><span class="sxs-lookup"><span data-stu-id="d066f-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

<span data-ttu-id="d066f-126">儲存並執行的頁面： 您可以檢查，並取消核取這兩個核取方塊，但是沒有這兩個核取方塊可檢查。</span><span class="sxs-lookup"><span data-stu-id="d066f-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>


<span data-ttu-id="d066f-127">[![只有一個核取方塊可以檢查一次](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d066f-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span></span>

<span data-ttu-id="d066f-128">只有一個核取方塊可以檢查一次 ([按一下以檢視完整大小的影像](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d066f-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d066f-129">上一步</span><span class="sxs-lookup"><span data-stu-id="d066f-129">Previous</span></span>](creating-mutually-exclusive-checkboxes-cs.md)
