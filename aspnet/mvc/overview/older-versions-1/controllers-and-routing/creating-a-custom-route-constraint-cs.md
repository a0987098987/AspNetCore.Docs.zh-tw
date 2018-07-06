---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: 建立自訂的路由條件約束 (C#) |Microsoft Docs
author: StephenWalther
description: Stephen Walther 會示範如何建立自訂路由條件約束。 我們會實作簡單的自訂條件約束可以防止路由比對 w...
ms.author: aspnetcontent
ms.date: 02/16/2009
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: e21e7e027cf66f390fc37ec08a07ae007e8242c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831809"
---
<a name="creating-a-custom-route-constraint-c"></a><span data-ttu-id="70e8c-104">建立自訂的路由條件約束 (C#)</span><span class="sxs-lookup"><span data-stu-id="70e8c-104">Creating a Custom Route Constraint (C#)</span></span>
====================
<span data-ttu-id="70e8c-105">藉由[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="70e8c-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="70e8c-106">Stephen Walther 會示範如何建立自訂路由條件約束。</span><span class="sxs-lookup"><span data-stu-id="70e8c-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="70e8c-107">我們會實作簡單的自訂條件約束，可防止瀏覽器要求從遠端電腦時要比對路由。</span><span class="sxs-lookup"><span data-stu-id="70e8c-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>


<span data-ttu-id="70e8c-108">本教學課程的目的在於示範如何建立自訂路由條件約束。</span><span class="sxs-lookup"><span data-stu-id="70e8c-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="70e8c-109">自訂路由條件約束可讓您防止某些自訂的條件會比對，除非符合路由。</span><span class="sxs-lookup"><span data-stu-id="70e8c-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="70e8c-110">在本教學課程中，我們會建立 Localhost 的路由條件約束。</span><span class="sxs-lookup"><span data-stu-id="70e8c-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="70e8c-111">Localhost 路由條件約束只會比對從本機電腦所提出的要求。</span><span class="sxs-lookup"><span data-stu-id="70e8c-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="70e8c-112">從遠端在網際網路上的要求不符。</span><span class="sxs-lookup"><span data-stu-id="70e8c-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="70e8c-113">您可以實作 IRouteConstraint 介面，以實作自訂路由條件約束。</span><span class="sxs-lookup"><span data-stu-id="70e8c-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="70e8c-114">這是非常簡單的介面描述單一方法：</span><span class="sxs-lookup"><span data-stu-id="70e8c-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="70e8c-115">方法會傳回布林值。</span><span class="sxs-lookup"><span data-stu-id="70e8c-115">The method returns a Boolean value.</span></span> <span data-ttu-id="70e8c-116">如果傳回 false，則含有條件約束相關聯的路由不會符合瀏覽器要求。</span><span class="sxs-lookup"><span data-stu-id="70e8c-116">If you return false, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="70e8c-117">列表 1 中包含的 Localhost 條件約束。</span><span class="sxs-lookup"><span data-stu-id="70e8c-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="70e8c-118">**列表 1-LocalhostConstraint.cs**</span><span class="sxs-lookup"><span data-stu-id="70e8c-118">**Listing 1 - LocalhostConstraint.cs**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="70e8c-119">列表 1 中的條件約束會利用 HttpRequest 類別所公開的 IsLocal 屬性。</span><span class="sxs-lookup"><span data-stu-id="70e8c-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="70e8c-120">當要求的 IP 位址是任一 127.0.0.1 或要求的 IP 是伺服器的 IP 位址相同，則這個屬性會傳回 true。</span><span class="sxs-lookup"><span data-stu-id="70e8c-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="70e8c-121">您使用自訂的條件約束內 Global.asax 檔案中定義的路由。</span><span class="sxs-lookup"><span data-stu-id="70e8c-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="70e8c-122">列表 2 中的 Global.asax 檔會使用 Localhost 條件約束，防止任何人要求系統管理員 頁面，除非它們從本機伺服器提出要求。</span><span class="sxs-lookup"><span data-stu-id="70e8c-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="70e8c-123">例如，從遠端伺服器時，將會失敗 /Admin/DeleteAll 的要求。</span><span class="sxs-lookup"><span data-stu-id="70e8c-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="70e8c-124">**列表 2-Global.asax**</span><span class="sxs-lookup"><span data-stu-id="70e8c-124">**Listing 2 - Global.asax**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="70e8c-125">管理路由的定義所用的 Localhost 條件約束。</span><span class="sxs-lookup"><span data-stu-id="70e8c-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="70e8c-126">此路由不會比對遠端瀏覽器要求。</span><span class="sxs-lookup"><span data-stu-id="70e8c-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="70e8c-127">不過，要注意的是，其他 Global.asax 中所定義的路由可能會符合相同的要求。</span><span class="sxs-lookup"><span data-stu-id="70e8c-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="70e8c-128">請務必了解條件約束可防止特定路由相符的要求，而且並非所有的路由定義於 Global.asax 檔案。</span><span class="sxs-lookup"><span data-stu-id="70e8c-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="70e8c-129">請注意，預設路由具有已標記為註解列表 2 中的 Global.asax 檔中。</span><span class="sxs-lookup"><span data-stu-id="70e8c-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="70e8c-130">如果您包含預設路由時，預設路由會比對系統管理員控制站的要求。</span><span class="sxs-lookup"><span data-stu-id="70e8c-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="70e8c-131">在此情況下，遠端使用者可能仍然叫用動作的系統管理控制站即使其要求不符合管理路由。</span><span class="sxs-lookup"><span data-stu-id="70e8c-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="70e8c-132">[上一頁](creating-a-route-constraint-cs.md)
> [下一頁](asp-net-mvc-controller-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="70e8c-132">[Previous](creating-a-route-constraint-cs.md)
[Next](asp-net-mvc-controller-overview-vb.md)</span></span>
