---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: "建立動作 (VB) |Microsoft 文件"
author: microsoft
description: "了解如何將新的動作加入至 ASP.NET MVC 控制器。 深入了解將動作方法的需求。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: 3d1d355599c17df597f9c08d9d7f129fffc1a2e4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-an-action-vb"></a><span data-ttu-id="84a5a-104">建立動作 (VB)</span><span class="sxs-lookup"><span data-stu-id="84a5a-104">Creating an Action (VB)</span></span>
====================
<span data-ttu-id="84a5a-105">由[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="84a5a-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="84a5a-106">了解如何將新的動作加入至 ASP.NET MVC 控制器。</span><span class="sxs-lookup"><span data-stu-id="84a5a-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="84a5a-107">深入了解將動作方法的需求。</span><span class="sxs-lookup"><span data-stu-id="84a5a-107">Learn about the requirements for a method to be an action.</span></span>


<span data-ttu-id="84a5a-108">本教學課程的目標是要說明如何建立新的控制器動作。</span><span class="sxs-lookup"><span data-stu-id="84a5a-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="84a5a-109">您了解需求的動作方法。</span><span class="sxs-lookup"><span data-stu-id="84a5a-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="84a5a-110">您也了解如何防止公開為動作方法。</span><span class="sxs-lookup"><span data-stu-id="84a5a-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="84a5a-111">新增動作至控制器</span><span class="sxs-lookup"><span data-stu-id="84a5a-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="84a5a-112">您加入新的動作，您可以控制站將新方法加入至控制器。</span><span class="sxs-lookup"><span data-stu-id="84a5a-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="84a5a-113">例如，列表 1 中的控制站包含名為 index （） 的動作，以及名為 SayHello() 的動作。</span><span class="sxs-lookup"><span data-stu-id="84a5a-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="84a5a-114">這兩種方法會公開為動作。</span><span class="sxs-lookup"><span data-stu-id="84a5a-114">Both methods are exposed as actions.</span></span>

<span data-ttu-id="84a5a-115">**列出 1-Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="84a5a-115">**Listing 1 - Controllers\HomeController.vb**</span></span>

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

<span data-ttu-id="84a5a-116">若要為動作 universe 來公開，方法必須符合特定需求：</span><span class="sxs-lookup"><span data-stu-id="84a5a-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="84a5a-117">這個方法必須是公用的。</span><span class="sxs-lookup"><span data-stu-id="84a5a-117">The method must be public.</span></span>
- <span data-ttu-id="84a5a-118">此方法不能是靜態方法。</span><span class="sxs-lookup"><span data-stu-id="84a5a-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="84a5a-119">方法不可以擴充方法。</span><span class="sxs-lookup"><span data-stu-id="84a5a-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="84a5a-120">方法不能是建構函式、 getter 或 setter。</span><span class="sxs-lookup"><span data-stu-id="84a5a-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="84a5a-121">此方法不能有開放式的泛型型別。</span><span class="sxs-lookup"><span data-stu-id="84a5a-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="84a5a-122">方法不是方法的控制器的基底類別。</span><span class="sxs-lookup"><span data-stu-id="84a5a-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="84a5a-123">方法不能包含**ref**或**出**參數。</span><span class="sxs-lookup"><span data-stu-id="84a5a-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="84a5a-124">請注意，控制器動作的傳回型別沒有限制。</span><span class="sxs-lookup"><span data-stu-id="84a5a-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="84a5a-125">控制器動作，可以傳回字串，是日期時間，或 void Random 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="84a5a-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="84a5a-126">ASP.NET MVC 架構會轉換成字串的動作結果不是任何傳回型別，並轉譯至瀏覽器的字串。</span><span class="sxs-lookup"><span data-stu-id="84a5a-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="84a5a-127">當您新增任何符合這些需求的控制站的方法時，做為控制器動作公開的方法。</span><span class="sxs-lookup"><span data-stu-id="84a5a-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="84a5a-128">這裡格外小心。</span><span class="sxs-lookup"><span data-stu-id="84a5a-128">Be careful here.</span></span> <span data-ttu-id="84a5a-129">連線到網際網路的任何人都可以叫用控制器動作。</span><span class="sxs-lookup"><span data-stu-id="84a5a-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="84a5a-130">請勿，例如，建立 DeleteMyWebsite() 控制器動作。</span><span class="sxs-lookup"><span data-stu-id="84a5a-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="84a5a-131">防止叫用的公用方法</span><span class="sxs-lookup"><span data-stu-id="84a5a-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="84a5a-132">如果您要在控制器類別中建立的公用方法，而且不想公開做為控制器動作方法則可防止方法叫用使用&lt;NonAction&gt;屬性。</span><span class="sxs-lookup"><span data-stu-id="84a5a-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the &lt;NonAction&gt; attribute.</span></span> <span data-ttu-id="84a5a-133">例如，列表 2 中的控制站包含名為 CompanySecrets() 裝飾的公用方法&lt;NonAction&gt;屬性。</span><span class="sxs-lookup"><span data-stu-id="84a5a-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the &lt;NonAction&gt; attribute.</span></span>

<span data-ttu-id="84a5a-134">**列出 2-Controllers\WorkController.vb**</span><span class="sxs-lookup"><span data-stu-id="84a5a-134">**Listing 2 - Controllers\WorkController.vb**</span></span>

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

<span data-ttu-id="84a5a-135">如果您嘗試叫用 CompanySecrets() 控制器動作，您的瀏覽器的網址列中輸入 /Work/CompanySecrets 您會在圖 1 中取得的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="84a5a-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>


<span data-ttu-id="84a5a-136">[![叫用 NonAction 方法](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="84a5a-136">[![Invoking a NonAction method](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)</span></span>

<span data-ttu-id="84a5a-137">**圖 01**： 叫用 NonAction 方法 ([按一下以檢視完整大小的影像](creating-an-action-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="84a5a-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-vb/_static/image2.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="84a5a-138">[上一頁](creating-a-controller-vb.md)
[下一頁](aspnet-mvc-controllers-overview-cs.md)</span><span class="sxs-lookup"><span data-stu-id="84a5a-138">[Previous](creating-a-controller-vb.md)
[Next](aspnet-mvc-controllers-overview-cs.md)</span></span>
