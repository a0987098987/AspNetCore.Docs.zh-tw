---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
title: 建立評等控制項 (VB) |Microsoft Docs
author: wenz
description: 許多網站中，從電子商務社群網站，以提供使用者速率文件或項目。 這通常需要一些編碼工作，但我們確實有...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6d0d70f4-725e-4258-8ae8-24a6ba1ddbf7
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
msc.type: authoredcontent
ms.openlocfilehash: b229d0155fbab0437c644b41424164e4c655f5e5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825547"
---
<a name="creating-a-rating-control-vb"></a><span data-ttu-id="6d17c-104">建立評等控制項 (VB)</span><span class="sxs-lookup"><span data-stu-id="6d17c-104">Creating a Rating Control (VB)</span></span>
====================
<span data-ttu-id="6d17c-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6d17c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6d17c-106">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="6d17c-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)</span></span>

> <span data-ttu-id="6d17c-107">許多網站中，從電子商務社群網站，以提供使用者速率文件或項目。</span><span class="sxs-lookup"><span data-stu-id="6d17c-107">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="6d17c-108">這通常需要一些編碼工作，但我們確實有 Control Toolkit，以供我們運用。</span><span class="sxs-lookup"><span data-stu-id="6d17c-108">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>


## <a name="overview"></a><span data-ttu-id="6d17c-109">總覽</span><span class="sxs-lookup"><span data-stu-id="6d17c-109">Overview</span></span>

<span data-ttu-id="6d17c-110">許多網站中，從電子商務社群網站，以提供使用者速率文件或項目。</span><span class="sxs-lookup"><span data-stu-id="6d17c-110">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="6d17c-111">這通常需要一些編碼工作，但我們確實有 Control Toolkit，以供我們運用。</span><span class="sxs-lookup"><span data-stu-id="6d17c-111">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="steps"></a><span data-ttu-id="6d17c-112">步驟</span><span class="sxs-lookup"><span data-stu-id="6d17c-112">Steps</span></span>

<span data-ttu-id="6d17c-113">首先，您必須 （至少） 兩種類型的映像： 一個用於區域分布評等項目，和一個空的評等項目。</span><span class="sxs-lookup"><span data-stu-id="6d17c-113">First of all, you need (at least) two kinds of images: one for a filled out rating item, and one for an empty rating item.</span></span> <span data-ttu-id="6d17c-114">評等項目通常是以星號或笑臉。</span><span class="sxs-lookup"><span data-stu-id="6d17c-114">A rating item is usually a star or a smiley.</span></span> <span data-ttu-id="6d17c-115">此案例中，您尋找三個檔案、 smiley.png 和 empty.png 和笑臉 done.png 之來源的程式碼下載本教學課程的一部分。</span><span class="sxs-lookup"><span data-stu-id="6d17c-115">For this scenario, you find three files, smiley.png and empty.png and smiley-done.png as part of the source code downloads for this tutorial.</span></span>

<span data-ttu-id="6d17c-116">然後，建立新的 ASP.NET 檔案，並開始新增`ScriptManager`控制項：</span><span class="sxs-lookup"><span data-stu-id="6d17c-116">Then, create a new ASP.NET file and start with adding a `ScriptManager` control to it:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample1.aspx)]

<span data-ttu-id="6d17c-117">然後，新增`Rating`從 ASP.NET AJAX Control Toolkit 控制項。</span><span class="sxs-lookup"><span data-stu-id="6d17c-117">Then, add the `Rating` control from the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="6d17c-118">設定此範例中，需要下列屬性：</span><span class="sxs-lookup"><span data-stu-id="6d17c-118">The following attributes need to be set for this example:</span></span>

- <span data-ttu-id="6d17c-119">`CurrentRating` 若要使用初始的評等</span><span class="sxs-lookup"><span data-stu-id="6d17c-119">`CurrentRating` the initial rating to be used</span></span>
- <span data-ttu-id="6d17c-120">`MaxRating` 最高分級</span><span class="sxs-lookup"><span data-stu-id="6d17c-120">`MaxRating` the maximum rating</span></span>
- <span data-ttu-id="6d17c-121">`EmptyStarCssClass` 若要使用的評等項目 （star 認證） 時的 CSS 類別是空的</span><span class="sxs-lookup"><span data-stu-id="6d17c-121">`EmptyStarCssClass` the CSS class to use when a rating item ( star ) is empty</span></span>
- <span data-ttu-id="6d17c-122">`FilledStarCssClass` 若要使用的評等項目 （star 認證） 時的 CSS 類別填妥</span><span class="sxs-lookup"><span data-stu-id="6d17c-122">`FilledStarCssClass` the CSS class to use when a rating item ( star ) is filled out</span></span>
- <span data-ttu-id="6d17c-123">`StarCssClass` 要用於顯示狀態的 CSS 類別</span><span class="sxs-lookup"><span data-stu-id="6d17c-123">`StarCssClass` the CSS class to use for a visible stat</span></span>
- <span data-ttu-id="6d17c-124">`WaitingStarCssClass` 要使用星級評等會傳送至伺服器時的 CSS 類別</span><span class="sxs-lookup"><span data-stu-id="6d17c-124">`WaitingStarCssClass` the CSS class to use while a star rating is sent back to the server</span></span>

<span data-ttu-id="6d17c-125">以下是建立具有五個的評等控制項的標記項目 (smileys) 的無填妥一開始：</span><span class="sxs-lookup"><span data-stu-id="6d17c-125">And here is the markup which creates a rating control with five items (smileys) of which none is filled out initially:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample2.aspx)]

<span data-ttu-id="6d17c-126">三個參考的 CSS 類別現在需要顯示適當的映像檔案中，這是容易使用 CSS:</span><span class="sxs-lookup"><span data-stu-id="6d17c-126">The three referenced CSS classes now need to show the appropriate image files, which is easy to do using CSS:</span></span>

[!code-css[Main](creating-a-rating-control-vb/samples/sample3.css)]

<span data-ttu-id="6d17c-127">請確定您提供的三個影像的高度與寬度，否則顯示看起來可能有點 messed 註冊。</span><span class="sxs-lookup"><span data-stu-id="6d17c-127">Make sure that you provide the width and height of the three images, otherwise the display may look a bit messed up.</span></span>

<span data-ttu-id="6d17c-128">最後，評等的結果應該會顯示給使用者 （或至少會儲存在資料庫中）。</span><span class="sxs-lookup"><span data-stu-id="6d17c-128">Finally, the result of the rating should be displayed to the user (or, at least saved in a database).</span></span> <span data-ttu-id="6d17c-129">因此，加入的標籤文字訊息和提交按鈕回傳到伺服器的評等形式的輸出：</span><span class="sxs-lookup"><span data-stu-id="6d17c-129">So add a label for the output of a text message and a submit button to post back the rating form to the server:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample4.aspx)]

<span data-ttu-id="6d17c-130">在伺服器端程式碼中，存取 評等控制項，透過其`ID`，然後存取其`CurrentRating`屬性，也就是所選的評等項目，在本例中的值介於 0 到 5 之間的數字。</span><span class="sxs-lookup"><span data-stu-id="6d17c-130">In the server-side code, access the Rating control via its `ID` and then access its `CurrentRating` property which is the number of the selected rating items, in our example a value between 0 and 5.</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample5.aspx)]

<span data-ttu-id="6d17c-131">儲存頁面，並將其載入至您的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="6d17c-131">Save the page and load it into your browser.</span></span> <span data-ttu-id="6d17c-132">當您暫留 （一開始是空的） 的評等項目時，就會發生 JavaScript 的效果： 評等變更。</span><span class="sxs-lookup"><span data-stu-id="6d17c-132">When you hover over the (initially empty) rating items, a JavaScript effect occurs: The rating changes.</span></span> <span data-ttu-id="6d17c-133">當您按一下組星號時，會保留目前的評等。</span><span class="sxs-lookup"><span data-stu-id="6d17c-133">When you click on the set of stars, the current rating is retained.</span></span> <span data-ttu-id="6d17c-134">最後，當您提交表單時，伺服器端程式碼會輸出選取的評等。</span><span class="sxs-lookup"><span data-stu-id="6d17c-134">Finally, when you submit the form, the server-side code outputs the selected rating.</span></span>


<span data-ttu-id="6d17c-135">[![使用最少的程式碼建立的分級系統](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6d17c-135">[![Creating a rating system with minimal code](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="6d17c-136">使用最少的程式碼建立的分級系統 ([按一下以檢視完整大小的影像](creating-a-rating-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6d17c-136">Creating a rating system with minimal code ([Click to view full-size image](creating-a-rating-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="6d17c-137">上一步</span><span class="sxs-lookup"><span data-stu-id="6d17c-137">Previous</span></span>](creating-a-rating-control-cs.md)
