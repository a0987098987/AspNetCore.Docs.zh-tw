---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
title: 建立路由條件約束 (C#) |Microsoft Docs
author: StephenWalther
description: 在本教學課程中，Stephen Walther 會示範如何控制瀏覽器使用規則運算式中建立路由條件約束所要求的相符項目路由。
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 0bfd06b1-12d3-4fbb-9779-a82e5eb7fe7d
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 51d76248a59968e1d6befd5d5404f7bf6c2878e4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823682"
---
<a name="creating-a-route-constraint-c"></a><span data-ttu-id="f1f8d-103">建立路由條件約束 (C#)</span><span class="sxs-lookup"><span data-stu-id="f1f8d-103">Creating a Route Constraint (C#)</span></span>
====================
<span data-ttu-id="f1f8d-104">藉由[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="f1f8d-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="f1f8d-105">在本教學課程中，Stephen Walther 會示範如何控制瀏覽器使用規則運算式中建立路由條件約束所要求的相符項目路由。</span><span class="sxs-lookup"><span data-stu-id="f1f8d-105">In this tutorial, Stephen Walther demonstrates how you can control how browser requests match routes by creating route constraints with regular expressions.</span></span>


<span data-ttu-id="f1f8d-106">您可以使用路由條件約束來限制瀏覽器會要求符合特定的路由。</span><span class="sxs-lookup"><span data-stu-id="f1f8d-106">You use route constraints to restrict the browser requests that match a particular route.</span></span> <span data-ttu-id="f1f8d-107">您可以使用規則運算式來指定路由條件約束。</span><span class="sxs-lookup"><span data-stu-id="f1f8d-107">You can use a regular expression to specify a route constraint.</span></span>

<span data-ttu-id="f1f8d-108">例如，假設您已定義的路由清單 1 在 Global.asax 檔案中。</span><span class="sxs-lookup"><span data-stu-id="f1f8d-108">For example, imagine that you have defined the route in Listing 1 in your Global.asax file.</span></span>

<span data-ttu-id="f1f8d-109">**列表 1-Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="f1f8d-109">**Listing 1 - Global.asax.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="f1f8d-110">列表 1 包含名為 Product 的路由。</span><span class="sxs-lookup"><span data-stu-id="f1f8d-110">Listing 1 contains a route named Product.</span></span> <span data-ttu-id="f1f8d-111">您可以使用產品路由以將瀏覽器要求對應至 列表 2 中所包含的 ProductController。</span><span class="sxs-lookup"><span data-stu-id="f1f8d-111">You can use the Product route to map browser requests to the ProductController contained in Listing 2.</span></span>

<span data-ttu-id="f1f8d-112">**列表 2-Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="f1f8d-112">**Listing 2 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="f1f8d-113">請注意，Product 控制器所公開的 Details() 動作會接受名為 productId 的單一參數。</span><span class="sxs-lookup"><span data-stu-id="f1f8d-113">Notice that the Details() action exposed by the Product controller accepts a single parameter named productId.</span></span> <span data-ttu-id="f1f8d-114">這個參數是一個整數參數。</span><span class="sxs-lookup"><span data-stu-id="f1f8d-114">This parameter is an integer parameter.</span></span>

<span data-ttu-id="f1f8d-115">在 列表 1 中定義的路由會符合任何下列 url:</span><span class="sxs-lookup"><span data-stu-id="f1f8d-115">The route defined in Listing 1 will match any of the following URLs:</span></span>

- <span data-ttu-id="f1f8d-116">/ 產品/23</span><span class="sxs-lookup"><span data-stu-id="f1f8d-116">/Product/23</span></span>
- <span data-ttu-id="f1f8d-117">/ 產品/7</span><span class="sxs-lookup"><span data-stu-id="f1f8d-117">/Product/7</span></span>

<span data-ttu-id="f1f8d-118">不幸的是，路由也會比對下列 Url:</span><span class="sxs-lookup"><span data-stu-id="f1f8d-118">Unfortunately, the route will also match the following URLs:</span></span>

- <span data-ttu-id="f1f8d-119">/ 產品/blah</span><span class="sxs-lookup"><span data-stu-id="f1f8d-119">/Product/blah</span></span>
- <span data-ttu-id="f1f8d-120">/ 產品/apple</span><span class="sxs-lookup"><span data-stu-id="f1f8d-120">/Product/apple</span></span>

<span data-ttu-id="f1f8d-121">由於 Details() 動作預期整數參數，提出要求，其中包含整數值以外的項目會造成錯誤。</span><span class="sxs-lookup"><span data-stu-id="f1f8d-121">Because the Details() action expects an integer parameter, making a request that contains something other than an integer value will cause an error.</span></span> <span data-ttu-id="f1f8d-122">比方說，如果您的瀏覽器中輸入 URL /Product/apple 然後就會出現錯誤頁面 [圖 1] 中。</span><span class="sxs-lookup"><span data-stu-id="f1f8d-122">For example, if you type the URL /Product/apple into your browser then you will get the error page in Figure 1.</span></span>


<span data-ttu-id="f1f8d-123">[![[新增專案] 對話方塊](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f1f8d-123">[![The New Project dialog box](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)</span></span>

<span data-ttu-id="f1f8d-124">**圖 01**： 看到 explode 頁面 ([按一下以檢視完整大小的影像](creating-a-route-constraint-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="f1f8d-124">**Figure 01**: Seeing a page explode ([Click to view full-size image](creating-a-route-constraint-cs/_static/image2.png))</span></span>


<span data-ttu-id="f1f8d-125">您真的要做什麼是只比對包含適當的整數 productId 的 Url。</span><span class="sxs-lookup"><span data-stu-id="f1f8d-125">What you really want to do is only match URLs that contain a proper integer productId.</span></span> <span data-ttu-id="f1f8d-126">可以使用條件約束定義的路由時，來限制路由相符的 Url。</span><span class="sxs-lookup"><span data-stu-id="f1f8d-126">You can use a constraint when defining a route to restrict the URLs that match the route.</span></span> <span data-ttu-id="f1f8d-127">修改的產品路由列表 3 中包含的規則運算式條件約束，只會比對整數。</span><span class="sxs-lookup"><span data-stu-id="f1f8d-127">The modified Product route in Listing 3 contains a regular expression constraint that only matches integers.</span></span>

<span data-ttu-id="f1f8d-128">**列表 3-Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="f1f8d-128">**Listing 3 - Global.asax.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="f1f8d-129">規則運算式 \d+ 比對一個或多個整數。</span><span class="sxs-lookup"><span data-stu-id="f1f8d-129">The regular expression \d+ matches one or more integers.</span></span> <span data-ttu-id="f1f8d-130">此條件約束會造成產品路由，以符合下列 Url:</span><span class="sxs-lookup"><span data-stu-id="f1f8d-130">This constraint causes the Product route to match the following URLs:</span></span>

- <span data-ttu-id="f1f8d-131">/ 產品/3</span><span class="sxs-lookup"><span data-stu-id="f1f8d-131">/Product/3</span></span>
- <span data-ttu-id="f1f8d-132">/ 產品/8999</span><span class="sxs-lookup"><span data-stu-id="f1f8d-132">/Product/8999</span></span>

<span data-ttu-id="f1f8d-133">但下列 Url:</span><span class="sxs-lookup"><span data-stu-id="f1f8d-133">But not the following URLs:</span></span>

- <span data-ttu-id="f1f8d-134">/ 產品/apple</span><span class="sxs-lookup"><span data-stu-id="f1f8d-134">/Product/apple</span></span>
- <span data-ttu-id="f1f8d-135">/ 產品</span><span class="sxs-lookup"><span data-stu-id="f1f8d-135">/Product</span></span>

- <span data-ttu-id="f1f8d-136">這些瀏覽器要求交由另一個路由或，如果不有任何相符的路由*找不到資源*便會傳回錯誤。</span><span class="sxs-lookup"><span data-stu-id="f1f8d-136">These browser requests will be handled by another route or, if there are no matching routes, a *The resource could not be found* error will be returned.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f1f8d-137">[上一頁](creating-custom-routes-cs.md)
> [下一頁](creating-a-custom-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="f1f8d-137">[Previous](creating-custom-routes-cs.md)
[Next](creating-a-custom-route-constraint-cs.md)</span></span>
