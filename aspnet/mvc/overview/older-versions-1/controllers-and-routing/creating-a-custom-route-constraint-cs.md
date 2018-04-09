---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: 建立自訂路由條件約束 (C#) |Microsoft 文件
author: StephenWalther
description: 作者： Stephen Walther 示範如何建立自訂的路由條件約束。 我們會實作簡單的自訂條件約束可以防止路由相符 w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 4c120a102b117433b6774f2ea7800f1c4a609f8b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-custom-route-constraint-c"></a><span data-ttu-id="f9603-104">建立自訂路由條件約束 (C#)</span><span class="sxs-lookup"><span data-stu-id="f9603-104">Creating a Custom Route Constraint (C#)</span></span>
====================
<span data-ttu-id="f9603-105">由[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="f9603-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="f9603-106">作者： Stephen Walther 示範如何建立自訂的路由條件約束。</span><span class="sxs-lookup"><span data-stu-id="f9603-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="f9603-107">我們會實作簡單的自訂條件約束，避免從遠端電腦進行瀏覽器要求的比對路由。</span><span class="sxs-lookup"><span data-stu-id="f9603-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>


<span data-ttu-id="f9603-108">本教學課程的目標是為了示範如何建立自訂的路由條件約束。</span><span class="sxs-lookup"><span data-stu-id="f9603-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="f9603-109">自訂路由條件約束，可讓您防止路由所比對，除非某些自訂條件為 matched。</span><span class="sxs-lookup"><span data-stu-id="f9603-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="f9603-110">在本教學課程中，我們會建立 Localhost 的路由條件約束。</span><span class="sxs-lookup"><span data-stu-id="f9603-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="f9603-111">Localhost 路由條件約束只符合從本機電腦所提出的要求。</span><span class="sxs-lookup"><span data-stu-id="f9603-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="f9603-112">從遠端在網際網路上的要求不符。</span><span class="sxs-lookup"><span data-stu-id="f9603-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="f9603-113">您藉由實作 IRouteConstraint 介面實作的自訂路由條件約束。</span><span class="sxs-lookup"><span data-stu-id="f9603-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="f9603-114">這是非常簡單的介面描述單一方法：</span><span class="sxs-lookup"><span data-stu-id="f9603-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="f9603-115">方法會傳回布林值。</span><span class="sxs-lookup"><span data-stu-id="f9603-115">The method returns a Boolean value.</span></span> <span data-ttu-id="f9603-116">如果傳回 false，則路由相關聯的條件約束都不會符合瀏覽器要求。</span><span class="sxs-lookup"><span data-stu-id="f9603-116">If you return false, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="f9603-117">Localhost 條件約束被包含在程式碼範例 1。</span><span class="sxs-lookup"><span data-stu-id="f9603-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="f9603-118">**列出 1-LocalhostConstraint.cs**</span><span class="sxs-lookup"><span data-stu-id="f9603-118">**Listing 1 - LocalhostConstraint.cs**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="f9603-119">程式碼範例 1 中的條件約束會利用 HttpRequest 類別所公開的 IsLocal 屬性。</span><span class="sxs-lookup"><span data-stu-id="f9603-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="f9603-120">當要求的 IP 位址是任一個 127.0.0.1 或要求的 IP 時伺服器的 IP 位址相同，則這個屬性會傳回 true。</span><span class="sxs-lookup"><span data-stu-id="f9603-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="f9603-121">您使用自訂的條件約束內 Global.asax 檔中定義的路由。</span><span class="sxs-lookup"><span data-stu-id="f9603-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="f9603-122">Global.asax 檔案中列出的 2 會使用 Localhost 條件約束，以防止任何人要求管理頁面，除非它們從本機伺服器提出要求。</span><span class="sxs-lookup"><span data-stu-id="f9603-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="f9603-123">例如，從遠端伺服器時，將會失敗 /Admin/DeleteAll 的要求。</span><span class="sxs-lookup"><span data-stu-id="f9603-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="f9603-124">**列出 2-Global.asax**</span><span class="sxs-lookup"><span data-stu-id="f9603-124">**Listing 2 - Global.asax**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="f9603-125">管理路由的定義所用的 Localhost 條件約束。</span><span class="sxs-lookup"><span data-stu-id="f9603-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="f9603-126">遠端瀏覽器要求，將不會符合此路由。</span><span class="sxs-lookup"><span data-stu-id="f9603-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="f9603-127">不過，請注意，在 Global.asax 中定義其他路由可能會符合相同的要求。</span><span class="sxs-lookup"><span data-stu-id="f9603-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="f9603-128">請務必了解條件約束可防止特定路由比對要求而不是所有的路由 Global.asax 檔案中定義。</span><span class="sxs-lookup"><span data-stu-id="f9603-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="f9603-129">請注意，預設路由已從 Global.asax 檔案中列出 2 略過。</span><span class="sxs-lookup"><span data-stu-id="f9603-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="f9603-130">如果您包含預設路由，則預設路由會符合管理控制器的要求。</span><span class="sxs-lookup"><span data-stu-id="f9603-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="f9603-131">在此情況下，遠端使用者可能仍叫用的管理控制器的動作即使其要求不符合系統管理員路由。</span><span class="sxs-lookup"><span data-stu-id="f9603-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f9603-132">[上一頁](creating-a-route-constraint-cs.md)
> [下一頁](asp-net-mvc-controller-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f9603-132">[Previous](creating-a-route-constraint-cs.md)
[Next](asp-net-mvc-controller-overview-vb.md)</span></span>
