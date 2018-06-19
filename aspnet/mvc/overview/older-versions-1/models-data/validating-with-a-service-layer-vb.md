---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
title: 驗證與服務層 (VB) |Microsoft 文件
author: StephenWalther
description: 了解如何將驗證邏輯，從控制器動作移至個別的服務層移。 在此教學課程中，說明作者： Stephen Walther 如何您...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 344bb38e-4965-4c47-bda1-f6d29ae5b83a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: bb1191b663f863bf881def620efab4f2f03edc56
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868239"
---
<a name="validating-with-a-service-layer-vb"></a><span data-ttu-id="d8f8b-104">驗證與服務層 (VB)</span><span class="sxs-lookup"><span data-stu-id="d8f8b-104">Validating with a Service Layer (VB)</span></span>
====================
<span data-ttu-id="d8f8b-105">由[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="d8f8b-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="d8f8b-106">了解如何將驗證邏輯，從控制器動作移至個別的服務層移。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-106">Learn how to move your validation logic out of your controller actions and into a separate service layer.</span></span> <span data-ttu-id="d8f8b-107">在本教學課程中，作者： Stephen Walther 會說明如何維護尖的重要性分離，藉由隔離您的服務層，從控制器層。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-107">In this tutorial, Stephen Walther explains how you can maintain a sharp separation of concerns by isolating your service layer from your controller layer.</span></span>


<span data-ttu-id="d8f8b-108">本教學課程的目標是描述的方法之一在 ASP.NET MVC 應用程式中執行驗證。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-108">The goal of this tutorial is to describe one method of performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="d8f8b-109">在本教學課程中，您可以了解如何將驗證邏輯，從您的控制站移至個別的服務層移。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-109">In this tutorial, you learn how to move your validation logic out of your controllers and into a separate service layer.</span></span>

## <a name="separating-concerns"></a><span data-ttu-id="d8f8b-110">區隔的考量</span><span class="sxs-lookup"><span data-stu-id="d8f8b-110">Separating Concerns</span></span>

<span data-ttu-id="d8f8b-111">當您建置 ASP.NET MVC 應用程式時，您不應該將您的資料庫邏輯內部控制器動作。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-111">When you build an ASP.NET MVC application, you should not place your database logic inside your controller actions.</span></span> <span data-ttu-id="d8f8b-112">混用您的資料庫和控制器邏輯，可讓您的應用程式更難維護一段時間。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-112">Mixing your database and controller logic makes your application more difficult to maintain over time.</span></span> <span data-ttu-id="d8f8b-113">建議您將所有的資料庫邏輯放在不同的儲存機制的圖層。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-113">The recommendation is that you place all of your database logic in a separate repository layer.</span></span>

<span data-ttu-id="d8f8b-114">例如，列出 1 包含名為 ProductRepository 簡單的儲存機制。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-114">For example, Listing 1 contains a simple repository named the ProductRepository.</span></span> <span data-ttu-id="d8f8b-115">產品儲存機制包含所有應用程式的資料存取程式碼。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-115">The product repository contains all of the data access code for the application.</span></span> <span data-ttu-id="d8f8b-116">清單也包含產品儲存機制實作 IProductRepository 介面。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-116">The listing also includes the IProductRepository interface that the product repository implements.</span></span>

<span data-ttu-id="d8f8b-117">**Listing 1 - Models\ProductRepository.vb**</span><span class="sxs-lookup"><span data-stu-id="d8f8b-117">**Listing 1 - Models\ProductRepository.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample1.vb)]

<span data-ttu-id="d8f8b-118">列表 2 中的控制站會使用儲存機制層在 [動作] 其 index 和 create （）。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-118">The controller in Listing 2 uses the repository layer in both its Index() and Create() actions.</span></span> <span data-ttu-id="d8f8b-119">請注意，此控制站不包含任何資料庫的邏輯。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-119">Notice that this controller does not contain any database logic.</span></span> <span data-ttu-id="d8f8b-120">建立儲存機制層可讓您維護清楚的重要性分離。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-120">Creating a repository layer enables you to maintain a clean separation of concerns.</span></span> <span data-ttu-id="d8f8b-121">控制站會負責使用應用程式的流程控制邏輯和儲存機制會負責資料存取邏輯。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-121">Controllers are responsible for application flow control logic and the repository is responsible for data access logic.</span></span>

<span data-ttu-id="d8f8b-122">**Listing 2 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="d8f8b-122">**Listing 2 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample2.vb)]

## <a name="creating-a-service-layer"></a><span data-ttu-id="d8f8b-123">建立服務層</span><span class="sxs-lookup"><span data-stu-id="d8f8b-123">Creating a Service Layer</span></span>

<span data-ttu-id="d8f8b-124">因此，應用程式流程控制邏輯應該位在控制器中，資料存取邏輯應該位在儲存機制。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-124">So, application flow control logic belongs in a controller and data access logic belongs in a repository.</span></span> <span data-ttu-id="d8f8b-125">在此情況下，請勿放置您的驗證邏輯？</span><span class="sxs-lookup"><span data-stu-id="d8f8b-125">In that case, where do you put your validation logic?</span></span> <span data-ttu-id="d8f8b-126">其中一個選項是將您的驗證邏輯中*服務層*。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-126">One option is to place your validation logic in a *service layer*.</span></span>

<span data-ttu-id="d8f8b-127">服務層是 ASP.NET MVC 應用程式會調解控制站和儲存機制層之間的通訊中的其他層級。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-127">A service layer is an additional layer in an ASP.NET MVC application that mediates communication between a controller and repository layer.</span></span> <span data-ttu-id="d8f8b-128">服務層包含商務邏輯。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-128">The service layer contains business logic.</span></span> <span data-ttu-id="d8f8b-129">特別是，其中包含驗證邏輯。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-129">In particular, it contains validation logic.</span></span>

<span data-ttu-id="d8f8b-130">例如，產品的服務層中列出的 3 具有 CreateProduct() 方法。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-130">For example, the product service layer in Listing 3 has a CreateProduct() method.</span></span> <span data-ttu-id="d8f8b-131">CreateProduct() 方法呼叫 ValidateProduct() 方法來驗證新產品，然後將產品傳遞至產品儲存機制。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-131">The CreateProduct() method calls the ValidateProduct() method to validate a new product before passing the product to the product repository.</span></span>

<span data-ttu-id="d8f8b-132">**Listing 3 - Models\ProductService.vb**</span><span class="sxs-lookup"><span data-stu-id="d8f8b-132">**Listing 3 - Models\ProductService.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample3.vb)]

<span data-ttu-id="d8f8b-133">已更新 Product 控制器中列出的 4，以使用服務層，而不是儲存機制層。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-133">The Product controller has been updated in Listing 4 to use the service layer instead of the repository layer.</span></span> <span data-ttu-id="d8f8b-134">控制器層級交談的服務層。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-134">The controller layer talks to the service layer.</span></span> <span data-ttu-id="d8f8b-135">儲存機制層交談服務層。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-135">The service layer talks to the repository layer.</span></span> <span data-ttu-id="d8f8b-136">每個圖層都有個別的責任。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-136">Each layer has a separate responsibility.</span></span>

<span data-ttu-id="d8f8b-137">**Listing 4 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="d8f8b-137">**Listing 4 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample4.vb)]

<span data-ttu-id="d8f8b-138">請注意，產品服務建立於產品控制器建構函式。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-138">Notice that the product service is created in the product controller constructor.</span></span> <span data-ttu-id="d8f8b-139">建立產品服務時，位於模型狀態字典會傳遞至服務。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-139">When the product service is created, the model state dictionary is passed to the service.</span></span> <span data-ttu-id="d8f8b-140">產品服務會將驗證錯誤訊息傳回至控制器使用模型狀態。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-140">The product service uses model state to pass validation error messages back to the controller.</span></span>

## <a name="decoupling-the-service-layer"></a><span data-ttu-id="d8f8b-141">減少服務層</span><span class="sxs-lookup"><span data-stu-id="d8f8b-141">Decoupling the Service Layer</span></span>

<span data-ttu-id="d8f8b-142">我們無法找出在控制器與一點的服務層級。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-142">We have failed to isolate the controller and service layers in one respect.</span></span> <span data-ttu-id="d8f8b-143">在控制器電腦與服務層透過模型狀態進行通訊。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-143">The controller and service layers communicate through model state.</span></span> <span data-ttu-id="d8f8b-144">換句話說，服務層會有的相依性的 ASP.NET MVC 架構的特定功能。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-144">In other words, the service layer has a dependency on a particular feature of the ASP.NET MVC framework.</span></span>

<span data-ttu-id="d8f8b-145">我們想要找出盡可能我們控制器層從服務層。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-145">We want to isolate the service layer from our controller layer as much as possible.</span></span> <span data-ttu-id="d8f8b-146">理論上來說，我們應該能夠使用任何類型的應用程式並不只在 ASP.NET MVC 應用程式的服務層。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-146">In theory, we should be able to use the service layer with any type of application and not only an ASP.NET MVC application.</span></span> <span data-ttu-id="d8f8b-147">比方說，在未來，我們可能會想要建置 WPF 應用程式前端。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-147">For example, in the future, we might want to build a WPF front-end for our application.</span></span> <span data-ttu-id="d8f8b-148">我們應設法將移除的相依性 ASP.NET MVC 模型狀態從我們的服務層。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-148">We should find a way to remove the dependency on ASP.NET MVC model state from our service layer.</span></span>

<span data-ttu-id="d8f8b-149">在列出 5，使其不再使用的模型狀態已更新服務層。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-149">In Listing 5, the service layer has been updated so that it no longer uses model state.</span></span> <span data-ttu-id="d8f8b-150">相反地，它會使用任何實作 IValidationDictionary 介面的類別。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-150">Instead, it uses any class that implements the IValidationDictionary interface.</span></span>

<span data-ttu-id="d8f8b-151">**Listing 5 - Models\ProductService.vb (decoupled)**</span><span class="sxs-lookup"><span data-stu-id="d8f8b-151">**Listing 5 - Models\ProductService.vb (decoupled)**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample5.vb)]

<span data-ttu-id="d8f8b-152">IValidationDictionary 介面被定義在程式碼範例 6。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-152">The IValidationDictionary interface is defined in Listing 6.</span></span> <span data-ttu-id="d8f8b-153">這個簡單的介面具有單一方法和單一屬性。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-153">This simple interface has a single method and a single property.</span></span>

<span data-ttu-id="d8f8b-154">**列出 6-Models\IValidationDictionary.cs**</span><span class="sxs-lookup"><span data-stu-id="d8f8b-154">**Listing 6 - Models\IValidationDictionary.cs**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample6.vb)]

<span data-ttu-id="d8f8b-155">列出第 7 中名為 ModelStateWrapper 類別，類別會實作 IValidationDictionary 介面。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-155">The class in Listing 7, named the ModelStateWrapper class, implements the IValidationDictionary interface.</span></span> <span data-ttu-id="d8f8b-156">您可以將模型狀態字典傳遞至建構函式具現化 ModelStateWrapper 類別。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-156">You can instantiate the ModelStateWrapper class by passing a model state dictionary to the constructor.</span></span>

<span data-ttu-id="d8f8b-157">**Listing 7 - Models\ModelStateWrapper.vb**</span><span class="sxs-lookup"><span data-stu-id="d8f8b-157">**Listing 7 - Models\ModelStateWrapper.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample7.vb)]

<span data-ttu-id="d8f8b-158">最後，在列出 8 更新的控制器會使用 ModelStateWrapper 在其建構函式中建立服務層時。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-158">Finally, the updated controller in Listing 8 uses the ModelStateWrapper when creating the service layer in its constructor.</span></span>

<span data-ttu-id="d8f8b-159">**Listing 8 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="d8f8b-159">**Listing 8 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample8.vb)]

<span data-ttu-id="d8f8b-160">使用 IValidationDictionary 介面，以及 ModelStateWrapper 類別可讓我們將我們的服務層，與我們的控制器層完全隔離。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-160">Using the IValidationDictionary interface and the ModelStateWrapper class enables us to completely isolate our service layer from our controller layer.</span></span> <span data-ttu-id="d8f8b-161">服務層不再相依於模型狀態。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-161">The service layer is no longer dependent on model state.</span></span> <span data-ttu-id="d8f8b-162">您可以傳遞至服務層的 IValidationDictionary 介面會實作任何類別。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-162">You can pass any class that implements the IValidationDictionary interface to the service layer.</span></span> <span data-ttu-id="d8f8b-163">例如，WPF 應用程式可能會實作 IValidationDictionary 介面的簡單集合類別。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-163">For example, a WPF application might implement the IValidationDictionary interface with a simple collection class.</span></span>

## <a name="summary"></a><span data-ttu-id="d8f8b-164">總結</span><span class="sxs-lookup"><span data-stu-id="d8f8b-164">Summary</span></span>

<span data-ttu-id="d8f8b-165">本教學課程的目標是為了討論在 ASP.NET MVC 應用程式中執行驗證的其中一個方法。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-165">The goal of this tutorial was to discuss one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="d8f8b-166">在本教學課程中，您學會如何將所有驗證邏輯從您的控制站移至個別的服務層的移動。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-166">In this tutorial, you learned how to move all of your validation logic out of your controllers and into a separate service layer.</span></span> <span data-ttu-id="d8f8b-167">您也學到如何建立 ModelStateWrapper 類別隔離您的服務層，從控制器層。</span><span class="sxs-lookup"><span data-stu-id="d8f8b-167">You also learned how to isolate your service layer from your controller layer by creating a ModelStateWrapper class.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d8f8b-168">[上一頁](validating-with-the-idataerrorinfo-interface-vb.md)
> [下一頁](validation-with-the-data-annotation-validators-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d8f8b-168">[Previous](validating-with-the-idataerrorinfo-interface-vb.md)
[Next](validation-with-the-data-annotation-validators-vb.md)</span></span>
