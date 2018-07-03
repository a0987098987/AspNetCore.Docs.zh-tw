---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: 驗證與服務層 (C#) |Microsoft Docs
author: StephenWalther
description: 了解如何移動您的驗證邏輯出您對控制器動作並進入個別的服務層。 在本教學課程中，Stephen walther 將說明如何您...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 8f533fca3357d060f072e2e28d05071d636cd9e6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362300"
---
<a name="validating-with-a-service-layer-c"></a><span data-ttu-id="350cb-104">驗證與服務層 (C#)</span><span class="sxs-lookup"><span data-stu-id="350cb-104">Validating with a Service Layer (C#)</span></span>
====================
<span data-ttu-id="350cb-105">藉由[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="350cb-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="350cb-106">了解如何移動您的驗證邏輯出您對控制器動作並進入個別的服務層。</span><span class="sxs-lookup"><span data-stu-id="350cb-106">Learn how to move your validation logic out of your controller actions and into a separate service layer.</span></span> <span data-ttu-id="350cb-107">在本教學課程中，Stephen Walther 會說明如何藉由隔離您的服務層，從您的控制器層級維護 sharp 關注點分離。</span><span class="sxs-lookup"><span data-stu-id="350cb-107">In this tutorial, Stephen Walther explains how you can maintain a sharp separation of concerns by isolating your service layer from your controller layer.</span></span>


<span data-ttu-id="350cb-108">本教學課程的目標是說明 ASP.NET MVC 應用程式中執行驗證的一種方法。</span><span class="sxs-lookup"><span data-stu-id="350cb-108">The goal of this tutorial is to describe one method of performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="350cb-109">在本教學課程中，您將了解如何移動您的驗證邏輯出您的控制器並進入個別的服務層。</span><span class="sxs-lookup"><span data-stu-id="350cb-109">In this tutorial, you learn how to move your validation logic out of your controllers and into a separate service layer.</span></span>

## <a name="separating-concerns"></a><span data-ttu-id="350cb-110">區隔的考量</span><span class="sxs-lookup"><span data-stu-id="350cb-110">Separating Concerns</span></span>

<span data-ttu-id="350cb-111">當您建置 ASP.NET MVC 應用程式時，不應該將您的資料庫邏輯放在您對控制器動作。</span><span class="sxs-lookup"><span data-stu-id="350cb-111">When you build an ASP.NET MVC application, you should not place your database logic inside your controller actions.</span></span> <span data-ttu-id="350cb-112">混用您的資料庫和控制器邏輯，可讓您的應用程式更難維護一段時間。</span><span class="sxs-lookup"><span data-stu-id="350cb-112">Mixing your database and controller logic makes your application more difficult to maintain over time.</span></span> <span data-ttu-id="350cb-113">建議您將所有資料庫邏輯放在個別的存放庫圖層。</span><span class="sxs-lookup"><span data-stu-id="350cb-113">The recommendation is that you place all of your database logic in a separate repository layer.</span></span>

<span data-ttu-id="350cb-114">例如，列表 1 包含名為 ProductRepository 簡單存放庫。</span><span class="sxs-lookup"><span data-stu-id="350cb-114">For example, Listing 1 contains a simple repository named the ProductRepository.</span></span> <span data-ttu-id="350cb-115">產品存放庫包含所有應用程式的資料存取程式碼。</span><span class="sxs-lookup"><span data-stu-id="350cb-115">The product repository contains all of the data access code for the application.</span></span> <span data-ttu-id="350cb-116">清單也包含產品存放庫會實作 IProductRepository 介面。</span><span class="sxs-lookup"><span data-stu-id="350cb-116">The listing also includes the IProductRepository interface that the product repository implements.</span></span>

<span data-ttu-id="350cb-117">**列表 1-Models\ProductRepository.cs**</span><span class="sxs-lookup"><span data-stu-id="350cb-117">**Listing 1 -- Models\ProductRepository.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

<span data-ttu-id="350cb-118">列表 2 中的控制站會使用其 index （）] 和 [create （） 動作中的儲存機制層。</span><span class="sxs-lookup"><span data-stu-id="350cb-118">The controller in Listing 2 uses the repository layer in both its Index() and Create() actions.</span></span> <span data-ttu-id="350cb-119">請注意，此控制器不包含任何資料庫的邏輯。</span><span class="sxs-lookup"><span data-stu-id="350cb-119">Notice that this controller does not contain any database logic.</span></span> <span data-ttu-id="350cb-120">建立儲存機制層，可讓您維持清楚分離關注點。</span><span class="sxs-lookup"><span data-stu-id="350cb-120">Creating a repository layer enables you to maintain a clean separation of concerns.</span></span> <span data-ttu-id="350cb-121">控制器負責應用程式的流程控制邏輯，並將存放庫會負責資料存取邏輯。</span><span class="sxs-lookup"><span data-stu-id="350cb-121">Controllers are responsible for application flow control logic and the repository is responsible for data access logic.</span></span>

<span data-ttu-id="350cb-122">**列表 2-Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="350cb-122">**Listing 2 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a><span data-ttu-id="350cb-123">建立服務層</span><span class="sxs-lookup"><span data-stu-id="350cb-123">Creating a Service Layer</span></span>

<span data-ttu-id="350cb-124">因此，應用程式流程控制邏輯位於控制器，而所屬的存放庫中的資料存取邏輯。</span><span class="sxs-lookup"><span data-stu-id="350cb-124">So, application flow control logic belongs in a controller and data access logic belongs in a repository.</span></span> <span data-ttu-id="350cb-125">在此情況下，您在其中放置您的驗證邏輯？</span><span class="sxs-lookup"><span data-stu-id="350cb-125">In that case, where do you put your validation logic?</span></span> <span data-ttu-id="350cb-126">其中一個選項是將您的驗證邏輯，在*服務層*。</span><span class="sxs-lookup"><span data-stu-id="350cb-126">One option is to place your validation logic in a *service layer*.</span></span>

<span data-ttu-id="350cb-127">服務層是在 ASP.NET MVC 應用程式，以促成控制站和儲存機制層之間的通訊額外層級。</span><span class="sxs-lookup"><span data-stu-id="350cb-127">A service layer is an additional layer in an ASP.NET MVC application that mediates communication between a controller and repository layer.</span></span> <span data-ttu-id="350cb-128">服務層包含商務邏輯。</span><span class="sxs-lookup"><span data-stu-id="350cb-128">The service layer contains business logic.</span></span> <span data-ttu-id="350cb-129">特別是，它會包含驗證邏輯。</span><span class="sxs-lookup"><span data-stu-id="350cb-129">In particular, it contains validation logic.</span></span>

<span data-ttu-id="350cb-130">例如，產品的服務層，在 列表 3 中有 CreateProduct() 方法。</span><span class="sxs-lookup"><span data-stu-id="350cb-130">For example, the product service layer in Listing 3 has a CreateProduct() method.</span></span> <span data-ttu-id="350cb-131">CreateProduct() 方法會呼叫 ValidateProduct() 来驗證的方法將新產品，然後將產品傳遞至產品存放庫。</span><span class="sxs-lookup"><span data-stu-id="350cb-131">The CreateProduct() method calls the ValidateProduct() method to validate a new product before passing the product to the product repository.</span></span>

<span data-ttu-id="350cb-132">**列表 3-Models\ProductService.cs**</span><span class="sxs-lookup"><span data-stu-id="350cb-132">**Listing 3 - Models\ProductService.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

<span data-ttu-id="350cb-133">若要使用的服務層，而不是儲存機制層的列表 4 中已更新 Product 控制器。</span><span class="sxs-lookup"><span data-stu-id="350cb-133">The Product controller has been updated in Listing 4 to use the service layer instead of the repository layer.</span></span> <span data-ttu-id="350cb-134">在控制器層與服務層。</span><span class="sxs-lookup"><span data-stu-id="350cb-134">The controller layer talks to the service layer.</span></span> <span data-ttu-id="350cb-135">服務層與儲存機制層。</span><span class="sxs-lookup"><span data-stu-id="350cb-135">The service layer talks to the repository layer.</span></span> <span data-ttu-id="350cb-136">每一層都有不同的責任。</span><span class="sxs-lookup"><span data-stu-id="350cb-136">Each layer has a separate responsibility.</span></span>

<span data-ttu-id="350cb-137">**列表 4-Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="350cb-137">**Listing 4 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

<span data-ttu-id="350cb-138">請注意，產品中會建立服務產品控制器建構函式。</span><span class="sxs-lookup"><span data-stu-id="350cb-138">Notice that the product service is created in the product controller constructor.</span></span> <span data-ttu-id="350cb-139">建立產品服務時，位於模型狀態字典會傳遞至服務。</span><span class="sxs-lookup"><span data-stu-id="350cb-139">When the product service is created, the model state dictionary is passed to the service.</span></span> <span data-ttu-id="350cb-140">產品服務會用來通過驗證的設定，錯誤訊息傳回至控制器的模型狀態。</span><span class="sxs-lookup"><span data-stu-id="350cb-140">The product service uses model state to pass validation error messages back to the controller.</span></span>

## <a name="decoupling-the-service-layer"></a><span data-ttu-id="350cb-141">減少服務層</span><span class="sxs-lookup"><span data-stu-id="350cb-141">Decoupling the Service Layer</span></span>

<span data-ttu-id="350cb-142">我們無法隔離控制站和服務層，在一個方面。</span><span class="sxs-lookup"><span data-stu-id="350cb-142">We have failed to isolate the controller and service layers in one respect.</span></span> <span data-ttu-id="350cb-143">在控制站和服務層透過模型狀態進行通訊。</span><span class="sxs-lookup"><span data-stu-id="350cb-143">The controller and service layers communicate through model state.</span></span> <span data-ttu-id="350cb-144">換句話說，服務層會將相依性對 ASP.NET MVC 架構的特定功能。</span><span class="sxs-lookup"><span data-stu-id="350cb-144">In other words, the service layer has a dependency on a particular feature of the ASP.NET MVC framework.</span></span>

<span data-ttu-id="350cb-145">我們想要隔離服務層，從我們盡量的控制器層級。</span><span class="sxs-lookup"><span data-stu-id="350cb-145">We want to isolate the service layer from our controller layer as much as possible.</span></span> <span data-ttu-id="350cb-146">理論上，我們應該能夠使用任何類型的應用程式和不只在 ASP.NET MVC 應用程式的服務層。</span><span class="sxs-lookup"><span data-stu-id="350cb-146">In theory, we should be able to use the service layer with any type of application and not only an ASP.NET MVC application.</span></span> <span data-ttu-id="350cb-147">比方說，在未來，我們可能會想要建置 WPF 應用程式的前端。</span><span class="sxs-lookup"><span data-stu-id="350cb-147">For example, in the future, we might want to build a WPF front-end for our application.</span></span> <span data-ttu-id="350cb-148">我們應該找出方法來移除對 ASP.NET MVC 的相依性模型狀態，從我們的服務層。</span><span class="sxs-lookup"><span data-stu-id="350cb-148">We should find a way to remove the dependency on ASP.NET MVC model state from our service layer.</span></span>

<span data-ttu-id="350cb-149">表 5 中，使其不再使用的模型狀態已更新的服務層。</span><span class="sxs-lookup"><span data-stu-id="350cb-149">In Listing 5, the service layer has been updated so that it no longer uses model state.</span></span> <span data-ttu-id="350cb-150">相反地，它會使用任何可實作 IValidationDictionary 介面的類別。</span><span class="sxs-lookup"><span data-stu-id="350cb-150">Instead, it uses any class that implements the IValidationDictionary interface.</span></span>

<span data-ttu-id="350cb-151">**列表 5-Models\ProductService.cs （低耦合）**</span><span class="sxs-lookup"><span data-stu-id="350cb-151">**Listing 5 - Models\ProductService.cs (decoupled)**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

<span data-ttu-id="350cb-152">IValidationDictionary 介面被定義在列表 6。</span><span class="sxs-lookup"><span data-stu-id="350cb-152">The IValidationDictionary interface is defined in Listing 6.</span></span> <span data-ttu-id="350cb-153">這個簡單的介面具有單一方法和單一屬性。</span><span class="sxs-lookup"><span data-stu-id="350cb-153">This simple interface has a single method and a single property.</span></span>

<span data-ttu-id="350cb-154">**列表 6-Models\IValidationDictionary.cs**</span><span class="sxs-lookup"><span data-stu-id="350cb-154">**Listing 6 - Models\IValidationDictionary.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

<span data-ttu-id="350cb-155">列表 7，名為 ModelStateWrapper 類別中的類別會實作 IValidationDictionary 介面。</span><span class="sxs-lookup"><span data-stu-id="350cb-155">The class in Listing 7, named the ModelStateWrapper class, implements the IValidationDictionary interface.</span></span> <span data-ttu-id="350cb-156">您可以將模型狀態字典傳遞至建構函式具現化 ModelStateWrapper 類別。</span><span class="sxs-lookup"><span data-stu-id="350cb-156">You can instantiate the ModelStateWrapper class by passing a model state dictionary to the constructor.</span></span>

<span data-ttu-id="350cb-157">**列表 7-Models\ModelStateWrapper.cs**</span><span class="sxs-lookup"><span data-stu-id="350cb-157">**Listing 7 - Models\ModelStateWrapper.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

<span data-ttu-id="350cb-158">最後，在 列表 8 更新的控制器會使用 ModelStateWrapper 其建構函式中建立的服務層時。</span><span class="sxs-lookup"><span data-stu-id="350cb-158">Finally, the updated controller in Listing 8 uses the ModelStateWrapper when creating the service layer in its constructor.</span></span>

<span data-ttu-id="350cb-159">**列表 8-Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="350cb-159">**Listing 8 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

<span data-ttu-id="350cb-160">使用 IValidationDictionary 介面和 ModelStateWrapper 類別可讓我們完全區隔我們的服務層，從我們的控制器層級。</span><span class="sxs-lookup"><span data-stu-id="350cb-160">Using the IValidationDictionary interface and the ModelStateWrapper class enables us to completely isolate our service layer from our controller layer.</span></span> <span data-ttu-id="350cb-161">服務層不再相依於模型狀態。</span><span class="sxs-lookup"><span data-stu-id="350cb-161">The service layer is no longer dependent on model state.</span></span> <span data-ttu-id="350cb-162">您可以傳遞任何實作服務層的 IValidationDictionary 介面的類別。</span><span class="sxs-lookup"><span data-stu-id="350cb-162">You can pass any class that implements the IValidationDictionary interface to the service layer.</span></span> <span data-ttu-id="350cb-163">比方說，WPF 應用程式可能會實作簡單的集合類別的 IValidationDictionary 介面。</span><span class="sxs-lookup"><span data-stu-id="350cb-163">For example, a WPF application might implement the IValidationDictionary interface with a simple collection class.</span></span>

## <a name="summary"></a><span data-ttu-id="350cb-164">總結</span><span class="sxs-lookup"><span data-stu-id="350cb-164">Summary</span></span>

<span data-ttu-id="350cb-165">本教學課程的目標是要討論的 ASP.NET MVC 應用程式中執行驗證的其中一種方法。</span><span class="sxs-lookup"><span data-stu-id="350cb-165">The goal of this tutorial was to discuss one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="350cb-166">在本教學課程中，您已了解如何移動所有的驗證邏輯出您的控制器並進入個別的服務層。</span><span class="sxs-lookup"><span data-stu-id="350cb-166">In this tutorial, you learned how to move all of your validation logic out of your controllers and into a separate service layer.</span></span> <span data-ttu-id="350cb-167">您也學到如何建立 ModelStateWrapper 類別，以隔離您的服務層，從您的控制器層級。</span><span class="sxs-lookup"><span data-stu-id="350cb-167">You also learned how to isolate your service layer from your controller layer by creating a ModelStateWrapper class.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="350cb-168">[上一頁](validating-with-the-idataerrorinfo-interface-cs.md)
> [下一頁](validation-with-the-data-annotation-validators-cs.md)</span><span class="sxs-lookup"><span data-stu-id="350cb-168">[Previous](validating-with-the-idataerrorinfo-interface-cs.md)
[Next](validation-with-the-data-annotation-validators-cs.md)</span></span>
