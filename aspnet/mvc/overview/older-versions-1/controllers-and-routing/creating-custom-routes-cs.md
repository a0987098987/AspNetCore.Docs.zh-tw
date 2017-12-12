---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: "建立自訂路由 (C#) |Microsoft 文件"
author: microsoft
description: "了解如何將自訂的路由新增至 ASP.NET MVC 應用程式。 在本教學課程中，您會學習如何修改預設的路由表，在 Global.asax 檔案中。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: d1542103298f2fa09dc71706284afb18d8381403
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-custom-routes-c"></a><span data-ttu-id="404b8-104">建立自訂路由 (C#)</span><span class="sxs-lookup"><span data-stu-id="404b8-104">Creating Custom Routes (C#)</span></span>
====================
<span data-ttu-id="404b8-105">由[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="404b8-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="404b8-106">了解如何將自訂的路由新增至 ASP.NET MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="404b8-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="404b8-107">在本教學課程中，您會學習如何修改預設的路由表，在 Global.asax 檔案中。</span><span class="sxs-lookup"><span data-stu-id="404b8-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>


<span data-ttu-id="404b8-108">在本教學課程中，您會學習如何將自訂的路由加入至 ASP.NET MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="404b8-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="404b8-109">您了解如何修改預設的路由表，以自訂路由的 Global.asax 檔中。</span><span class="sxs-lookup"><span data-stu-id="404b8-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="404b8-110">許多簡單的 ASP.NET MVC 應用程式，預設路由表，將會正常運作。</span><span class="sxs-lookup"><span data-stu-id="404b8-110">For many simple ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="404b8-111">不過，您可能會發現您專用的路由需求。</span><span class="sxs-lookup"><span data-stu-id="404b8-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="404b8-112">在此情況下，您可以建立自訂的路由。</span><span class="sxs-lookup"><span data-stu-id="404b8-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="404b8-113">例如，假設您要建置的部落格應用程式。</span><span class="sxs-lookup"><span data-stu-id="404b8-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="404b8-114">您可能想要處理傳入的要求看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="404b8-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="404b8-115">/ 封存/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="404b8-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="404b8-116">當使用者輸入這個要求時，您想要的日期傳回對應的部落格項目 2009 年 12 月 25 日。</span><span class="sxs-lookup"><span data-stu-id="404b8-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="404b8-117">若要處理這種類型的要求，您需要建立自訂的路由。</span><span class="sxs-lookup"><span data-stu-id="404b8-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="404b8-118">Global.asax 檔案中列出的 1 會包含新的自訂路由，名為部落格，看起來像 /Archive/ 哪些控點要求*輸入日期*。</span><span class="sxs-lookup"><span data-stu-id="404b8-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="404b8-119">**列出 1-Global.asax （與自訂路由）**</span><span class="sxs-lookup"><span data-stu-id="404b8-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

<span data-ttu-id="404b8-120">您將新增至路由表的路由順序很重要。</span><span class="sxs-lookup"><span data-stu-id="404b8-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="404b8-121">現有的預設路由之前已加入我們新的自訂部落格路由。</span><span class="sxs-lookup"><span data-stu-id="404b8-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="404b8-122">如果您依相反順序排列，然後預設路由一律會呼叫而不是自訂的路由。</span><span class="sxs-lookup"><span data-stu-id="404b8-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="404b8-123">自訂的部落格路由會符合開頭/封存/任何要求。</span><span class="sxs-lookup"><span data-stu-id="404b8-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="404b8-124">因此，它符合所有下列 Url:</span><span class="sxs-lookup"><span data-stu-id="404b8-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="404b8-125">/ 封存/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="404b8-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="404b8-126">/ 封存/10-6-2004</span><span class="sxs-lookup"><span data-stu-id="404b8-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="404b8-127">/ 封存/apple</span><span class="sxs-lookup"><span data-stu-id="404b8-127">/Archive/apple</span></span>

<span data-ttu-id="404b8-128">自訂路由將內送的要求對應至名為 Archive 的控制站，並叫用 Entry() 動作。</span><span class="sxs-lookup"><span data-stu-id="404b8-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="404b8-129">Entry() 方法呼叫時，輸入日期做為參數命名為 entryDate 傳遞。</span><span class="sxs-lookup"><span data-stu-id="404b8-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="404b8-130">您可以使用列表 2 中的控制站的部落格的自訂路由。</span><span class="sxs-lookup"><span data-stu-id="404b8-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="404b8-131">**列出 2-ArchiveController.cs**</span><span class="sxs-lookup"><span data-stu-id="404b8-131">**Listing 2 - ArchiveController.cs**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

<span data-ttu-id="404b8-132">請注意清單 2 Entry() 方法接受 DateTime 類型的參數。</span><span class="sxs-lookup"><span data-stu-id="404b8-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="404b8-133">MVC 架構是聰明，可以從 URL 中將日期轉換成日期時間值時，自動。</span><span class="sxs-lookup"><span data-stu-id="404b8-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="404b8-134">如果從 URL 的項目日期參數無法轉換成 DateTime，就會引發錯誤 （請參閱圖 1）。</span><span class="sxs-lookup"><span data-stu-id="404b8-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="404b8-135">**圖 1： 將參數轉換的錯誤**</span><span class="sxs-lookup"><span data-stu-id="404b8-135">**Figure 1 - Error from converting parameter**</span></span>


<span data-ttu-id="404b8-136">[![新增專案 對話方塊](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="404b8-136">[![The New Project dialog box](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span></span>

<span data-ttu-id="404b8-137">**圖 01**： 將參數轉換的錯誤 ([按一下以檢視完整大小的影像](creating-custom-routes-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="404b8-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-cs/_static/image2.png))</span></span>


## <a name="summary"></a><span data-ttu-id="404b8-138">總結</span><span class="sxs-lookup"><span data-stu-id="404b8-138">Summary</span></span>

<span data-ttu-id="404b8-139">本教學課程的目標是為了示範如何建立自訂的路由。</span><span class="sxs-lookup"><span data-stu-id="404b8-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="404b8-140">您已學習如何將自訂的路由加入至 Global.asax 檔案代表部落格文章中的路由表。</span><span class="sxs-lookup"><span data-stu-id="404b8-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="404b8-141">我們將討論如何將要求的部落格項目對應至名為 ArchiveController 控制器和名為 Entry() 控制器動作。</span><span class="sxs-lookup"><span data-stu-id="404b8-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="404b8-142">[上一頁](aspnet-mvc-controllers-overview-cs.md)
[下一頁](creating-a-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="404b8-142">[Previous](aspnet-mvc-controllers-overview-cs.md)
[Next](creating-a-route-constraint-cs.md)</span></span>
