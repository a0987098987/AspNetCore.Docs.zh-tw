---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: 建立動作 (C#) |Microsoft Docs
author: microsoft
description: 了解如何將新的動作新增至 ASP.NET MVC 控制器。 深入了解將動作方法的需求。
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: bc85e1439bfa0a89d60271dda53829e921b26f55
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812221"
---
<a name="creating-an-action-c"></a><span data-ttu-id="b8c3d-104">建立動作 (C#)</span><span class="sxs-lookup"><span data-stu-id="b8c3d-104">Creating an Action (C#)</span></span>
====================
<span data-ttu-id="b8c3d-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="b8c3d-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="b8c3d-106">了解如何將新的動作新增至 ASP.NET MVC 控制器。</span><span class="sxs-lookup"><span data-stu-id="b8c3d-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="b8c3d-107">深入了解將動作方法的需求。</span><span class="sxs-lookup"><span data-stu-id="b8c3d-107">Learn about the requirements for a method to be an action.</span></span>


<span data-ttu-id="b8c3d-108">本教學課程的目標是要說明如何建立新的控制器動作。</span><span class="sxs-lookup"><span data-stu-id="b8c3d-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="b8c3d-109">您了解動作方法的需求。</span><span class="sxs-lookup"><span data-stu-id="b8c3d-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="b8c3d-110">您也了解如何防止公開做為動作方法。</span><span class="sxs-lookup"><span data-stu-id="b8c3d-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="b8c3d-111">將動作新增至控制器</span><span class="sxs-lookup"><span data-stu-id="b8c3d-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="b8c3d-112">您新增新的動作，您可以控制站的新方法新增至控制器。</span><span class="sxs-lookup"><span data-stu-id="b8c3d-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="b8c3d-113">例如，列表 1 中的控制器包含名為 index （） 的動作，以及名為 SayHello() 的動作。</span><span class="sxs-lookup"><span data-stu-id="b8c3d-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="b8c3d-114">這兩種方法都會公開為動作。</span><span class="sxs-lookup"><span data-stu-id="b8c3d-114">Both methods are exposed as actions.</span></span>

<span data-ttu-id="b8c3d-115">**列表 1-Controllers\HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="b8c3d-115">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

<span data-ttu-id="b8c3d-116">若要公開給 universe 做為動作，方法必須符合特定需求：</span><span class="sxs-lookup"><span data-stu-id="b8c3d-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="b8c3d-117">這個方法必須是公用的。</span><span class="sxs-lookup"><span data-stu-id="b8c3d-117">The method must be public.</span></span>
- <span data-ttu-id="b8c3d-118">此方法不能是靜態方法。</span><span class="sxs-lookup"><span data-stu-id="b8c3d-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="b8c3d-119">方法無法擴充方法。</span><span class="sxs-lookup"><span data-stu-id="b8c3d-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="b8c3d-120">方法不可為建構函式、 getter 或 setter。</span><span class="sxs-lookup"><span data-stu-id="b8c3d-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="b8c3d-121">此方法不能有開放式的泛型型別。</span><span class="sxs-lookup"><span data-stu-id="b8c3d-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="b8c3d-122">此方法不是控制器的基底類別的方法。</span><span class="sxs-lookup"><span data-stu-id="b8c3d-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="b8c3d-123">此方法不能包含**ref**或是**出**參數。</span><span class="sxs-lookup"><span data-stu-id="b8c3d-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="b8c3d-124">請注意，控制器動作的傳回型別不受任何限制。</span><span class="sxs-lookup"><span data-stu-id="b8c3d-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="b8c3d-125">控制器動作可以傳回的字串、 日期時間，或 void Random 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="b8c3d-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="b8c3d-126">ASP.NET MVC 架構會轉換成字串的動作結果不是任何傳回型別，並呈現至瀏覽器的字串。</span><span class="sxs-lookup"><span data-stu-id="b8c3d-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="b8c3d-127">當您新增不違反這些需求，來控制站的任何方法時，則會將方法公開為控制器動作。</span><span class="sxs-lookup"><span data-stu-id="b8c3d-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="b8c3d-128">只能小心為妙。</span><span class="sxs-lookup"><span data-stu-id="b8c3d-128">Be careful here.</span></span> <span data-ttu-id="b8c3d-129">可以叫用控制器動作，任何人連線到網際網路。</span><span class="sxs-lookup"><span data-stu-id="b8c3d-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="b8c3d-130">，例如，建立 DeleteMyWebsite() 控制器動作。</span><span class="sxs-lookup"><span data-stu-id="b8c3d-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="b8c3d-131">阻止叫用的公用方法</span><span class="sxs-lookup"><span data-stu-id="b8c3d-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="b8c3d-132">如果您需要在控制器類別中建立的公用方法，而且您不想要公開為控制器動作方法您可以使用 [NonAction] 屬性來叫用防止方法。</span><span class="sxs-lookup"><span data-stu-id="b8c3d-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the [NonAction] attribute.</span></span> <span data-ttu-id="b8c3d-133">例如，列表 2 中的控制站會包含名為 [NonAction] 屬性裝飾的 CompanySecrets() 的公用方法。</span><span class="sxs-lookup"><span data-stu-id="b8c3d-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the [NonAction] attribute.</span></span>

<span data-ttu-id="b8c3d-134">**列表 2-Controllers\WorkController.cs**</span><span class="sxs-lookup"><span data-stu-id="b8c3d-134">**Listing 2 - Controllers\WorkController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

<span data-ttu-id="b8c3d-135">如果您嘗試叫用 CompanySecrets() 控制器動作，在您的瀏覽器的網址列中輸入 /Work/CompanySecrets 您會在 圖 1 中收到錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="b8c3d-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>


<span data-ttu-id="b8c3d-136">[![叫用 NonAction 方法](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b8c3d-136">[![Invoking a NonAction method](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span></span>

<span data-ttu-id="b8c3d-137">**圖 01**： 叫用 NonAction 方法 ([按一下以檢視完整大小的影像](creating-an-action-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="b8c3d-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-cs/_static/image2.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b8c3d-138">[上一頁](creating-a-controller-cs.md)
> [下一頁](asp-net-mvc-routing-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b8c3d-138">[Previous](creating-a-controller-cs.md)
[Next](asp-net-mvc-routing-overview-vb.md)</span></span>
