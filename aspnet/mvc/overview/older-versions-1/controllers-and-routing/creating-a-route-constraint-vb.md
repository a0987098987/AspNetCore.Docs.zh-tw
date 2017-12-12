---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: "建立路由條件約束 (VB) |Microsoft 文件"
author: StephenWalther
description: "在本教學課程中，作者： Stephen Walther 會示範如何控制瀏覽器藉由建立路由條件約束使用規則運算式所要求的相符項目路由。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 67ff2666f4558abd4f8d9bddffd7aef8bb68d7bd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-route-constraint-vb"></a><span data-ttu-id="39d56-103">建立路由條件約束 (VB)</span><span class="sxs-lookup"><span data-stu-id="39d56-103">Creating a Route Constraint (VB)</span></span>
====================
<span data-ttu-id="39d56-104">由[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="39d56-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="39d56-105">在本教學課程中，作者： Stephen Walther 會示範如何控制瀏覽器藉由建立路由條件約束使用規則運算式所要求的相符項目路由。</span><span class="sxs-lookup"><span data-stu-id="39d56-105">In this tutorial, Stephen Walther demonstrates how you can control how browser requests match routes by creating route constraints with regular expressions.</span></span>


<span data-ttu-id="39d56-106">您可以使用路由條件約束來限制符合特定路由的瀏覽器要求。</span><span class="sxs-lookup"><span data-stu-id="39d56-106">You use route constraints to restrict the browser requests that match a particular route.</span></span> <span data-ttu-id="39d56-107">您可以使用規則運算式來指定路由條件約束。</span><span class="sxs-lookup"><span data-stu-id="39d56-107">You can use a regular expression to specify a route constraint.</span></span>

<span data-ttu-id="39d56-108">例如，假設您已定義路由清單 1 中 Global.asax 檔中。</span><span class="sxs-lookup"><span data-stu-id="39d56-108">For example, imagine that you have defined the route in Listing 1 in your Global.asax file.</span></span>

<span data-ttu-id="39d56-109">**列出 1-Global.asax.vb**</span><span class="sxs-lookup"><span data-stu-id="39d56-109">**Listing 1 - Global.asax.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="39d56-110">清單 1 包含名為 Product 的路由。</span><span class="sxs-lookup"><span data-stu-id="39d56-110">Listing 1 contains a route named Product.</span></span> <span data-ttu-id="39d56-111">若要將瀏覽器要求對應至包含在清單 2 ProductController 可用產品路由。</span><span class="sxs-lookup"><span data-stu-id="39d56-111">You can use the Product route to map browser requests to the ProductController contained in Listing 2.</span></span>

<span data-ttu-id="39d56-112">**列出 2-Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="39d56-112">**Listing 2 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="39d56-113">請注意，Product 控制器所公開的 Details() 動作接受名為 productId 的單一參數。</span><span class="sxs-lookup"><span data-stu-id="39d56-113">Notice that the Details() action exposed by the Product controller accepts a single parameter named productId.</span></span> <span data-ttu-id="39d56-114">這個參數是一個整數參數。</span><span class="sxs-lookup"><span data-stu-id="39d56-114">This parameter is an integer parameter.</span></span>

<span data-ttu-id="39d56-115">在程式碼範例 1 中定義的路由會符合任何下列 url:</span><span class="sxs-lookup"><span data-stu-id="39d56-115">The route defined in Listing 1 will match any of the following URLs:</span></span>

- <span data-ttu-id="39d56-116">/ 產品/23</span><span class="sxs-lookup"><span data-stu-id="39d56-116">/Product/23</span></span>
- <span data-ttu-id="39d56-117">/ 產品/7</span><span class="sxs-lookup"><span data-stu-id="39d56-117">/Product/7</span></span>

<span data-ttu-id="39d56-118">不幸的是，路由也會比對下列 Url:</span><span class="sxs-lookup"><span data-stu-id="39d56-118">Unfortunately, the route will also match the following URLs:</span></span>

- <span data-ttu-id="39d56-119">/ 產品/blah</span><span class="sxs-lookup"><span data-stu-id="39d56-119">/Product/blah</span></span>
- <span data-ttu-id="39d56-120">/ 產品/apple</span><span class="sxs-lookup"><span data-stu-id="39d56-120">/Product/apple</span></span>

<span data-ttu-id="39d56-121">Details() 動作必須要有一個整數參數，因為提出要求，其中包含整數值以外的項目會導致錯誤。</span><span class="sxs-lookup"><span data-stu-id="39d56-121">Because the Details() action expects an integer parameter, making a request that contains something other than an integer value will cause an error.</span></span> <span data-ttu-id="39d56-122">例如，如果您的瀏覽器中輸入 URL /Product/apple 然後就會出現錯誤頁面中圖 1。</span><span class="sxs-lookup"><span data-stu-id="39d56-122">For example, if you type the URL /Product/apple into your browser then you will get the error page in Figure 1.</span></span>


<span data-ttu-id="39d56-123">[![新增專案 對話方塊](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="39d56-123">[![The New Project dialog box](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)</span></span>

<span data-ttu-id="39d56-124">**圖 01**： 看到分裂頁面 ([按一下以檢視完整大小的影像](creating-a-route-constraint-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="39d56-124">**Figure 01**: Seeing a page explode ([Click to view full-size image](creating-a-route-constraint-vb/_static/image2.png))</span></span>


<span data-ttu-id="39d56-125">您真的想要做什麼是只會比對包含適當的整數 productId 的 Url。</span><span class="sxs-lookup"><span data-stu-id="39d56-125">What you really want to do is only match URLs that contain a proper integer productId.</span></span> <span data-ttu-id="39d56-126">可以使用條件約束定義的路由時，來限制路由相符的 Url。</span><span class="sxs-lookup"><span data-stu-id="39d56-126">You can use a constraint when defining a route to restrict the URLs that match the route.</span></span> <span data-ttu-id="39d56-127">修改的產品路由中列出的 3 包含只比對整數的規則運算式條件約束。</span><span class="sxs-lookup"><span data-stu-id="39d56-127">The modified Product route in Listing 3 contains a regular expression constraint that only matches integers.</span></span>

<span data-ttu-id="39d56-128">**列出 3-Global.asax.vb**</span><span class="sxs-lookup"><span data-stu-id="39d56-128">**Listing 3 - Global.asax.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="39d56-129">規則運算式 \d+ 比對一個或多個整數。</span><span class="sxs-lookup"><span data-stu-id="39d56-129">The regular expression \d+ matches one or more integers.</span></span> <span data-ttu-id="39d56-130">此條件約束會造成產品路由，以符合下列 Url:</span><span class="sxs-lookup"><span data-stu-id="39d56-130">This constraint causes the Product route to match the following URLs:</span></span>

- <span data-ttu-id="39d56-131">/ 產品/3</span><span class="sxs-lookup"><span data-stu-id="39d56-131">/Product/3</span></span>
- <span data-ttu-id="39d56-132">/ 產品/8999</span><span class="sxs-lookup"><span data-stu-id="39d56-132">/Product/8999</span></span>

<span data-ttu-id="39d56-133">但下列 Url:</span><span class="sxs-lookup"><span data-stu-id="39d56-133">But not the following URLs:</span></span>

- <span data-ttu-id="39d56-134">/ 產品/apple</span><span class="sxs-lookup"><span data-stu-id="39d56-134">/Product/apple</span></span>
- <span data-ttu-id="39d56-135">/ 產品</span><span class="sxs-lookup"><span data-stu-id="39d56-135">/Product</span></span>

<span data-ttu-id="39d56-136">這些瀏覽器要求將由另一個路由，或如果沒有相符的路由，*找不到資源*會傳回錯誤。</span><span class="sxs-lookup"><span data-stu-id="39d56-136">These browser requests will be handled by another route or, if there are no matching routes, a *The resource could not be found* error will be returned.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="39d56-137">[上一頁](creating-custom-routes-vb.md)
[下一頁](creating-a-custom-route-constraint-vb.md)</span><span class="sxs-lookup"><span data-stu-id="39d56-137">[Previous](creating-custom-routes-vb.md)
[Next](creating-a-custom-route-constraint-vb.md)</span></span>
