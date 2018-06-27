---
title: ASP.NET Core Web API 中的控制器動作傳回型別
author: scottaddie
description: 了解在 ASP.NET Core Web API 中使用各種控制器動作方法傳回型別。
ms.author: scaddie
ms.custom: mvc
ms.date: 03/21/2018
uid: web-api/action-return-types
ms.openlocfilehash: 422db97da222fb5e742e1d8e6ae410edc90dbc18
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273552"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a><span data-ttu-id="97f61-103">ASP.NET Core Web API 中的控制器動作傳回型別</span><span class="sxs-lookup"><span data-stu-id="97f61-103">Controller action return types in ASP.NET Core Web API</span></span>

<span data-ttu-id="97f61-104">作者：[Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="97f61-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="97f61-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="97f61-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="97f61-106">ASP.NET Core 提供下列 Web API 控制器動作傳回型別選項：</span><span class="sxs-lookup"><span data-stu-id="97f61-106">ASP.NET Core offers the following options for Web API controller action return types:</span></span>

::: moniker range="<= aspnetcore-2.0"
* [<span data-ttu-id="97f61-107">特定類型</span><span class="sxs-lookup"><span data-stu-id="97f61-107">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="97f61-108">IActionResult</span><span class="sxs-lookup"><span data-stu-id="97f61-108">IActionResult</span></span>](#iactionresult-type)
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
* [<span data-ttu-id="97f61-109">特定類型</span><span class="sxs-lookup"><span data-stu-id="97f61-109">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="97f61-110">IActionResult</span><span class="sxs-lookup"><span data-stu-id="97f61-110">IActionResult</span></span>](#iactionresult-type)
* [<span data-ttu-id="97f61-111">ActionResult\<T></span><span class="sxs-lookup"><span data-stu-id="97f61-111">ActionResult\<T></span></span>](#actionresultt-type)
::: moniker-end

<span data-ttu-id="97f61-112">本文件說明使用每個傳回型別的最適時機。</span><span class="sxs-lookup"><span data-stu-id="97f61-112">This document explains when it's most appropriate to use each return type.</span></span>

## <a name="specific-type"></a><span data-ttu-id="97f61-113">特定類型</span><span class="sxs-lookup"><span data-stu-id="97f61-113">Specific type</span></span>

<span data-ttu-id="97f61-114">最簡單的動作傳回基本或複雜的資料類型 (例如，`string` 或自訂物件類型)。</span><span class="sxs-lookup"><span data-stu-id="97f61-114">The simplest action returns a primitive or complex data type (for example, `string` or a custom object type).</span></span> <span data-ttu-id="97f61-115">請考慮下列動作，它會傳回自訂 `Product` 物件的集合：</span><span class="sxs-lookup"><span data-stu-id="97f61-115">Consider the following action, which returns a collection of custom `Product` objects:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

<span data-ttu-id="97f61-116">無已知條件，無法確保在動作執行期間能滿足傳回特定類型。</span><span class="sxs-lookup"><span data-stu-id="97f61-116">Without known conditions to safeguard against during action execution, returning a specific type could suffice.</span></span> <span data-ttu-id="97f61-117">上述動作不接受任何參數，因此不需要驗證參數條件約束。</span><span class="sxs-lookup"><span data-stu-id="97f61-117">The preceding action accepts no parameters, so parameter constraints validation isn't needed.</span></span>

<span data-ttu-id="97f61-118">當動作需要計入已知情況時，會引入多個傳回路徑。</span><span class="sxs-lookup"><span data-stu-id="97f61-118">When known conditions need to be accounted for in an action, multiple return paths are introduced.</span></span> <span data-ttu-id="97f61-119">在此情況下，通常會混合 [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) 傳回型別和基本或複雜的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="97f61-119">In such a case, it's common to mix an [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return type with the primitive or complex return type.</span></span> <span data-ttu-id="97f61-120">[IActionResult](#iactionresult-type) 或 [ActionResult\<T>](#actionresultt-type) 是容納此動作類型的必要項目。</span><span class="sxs-lookup"><span data-stu-id="97f61-120">Either [IActionResult](#iactionresult-type) or [ActionResult\<T>](#actionresultt-type) are necessary to accommodate this type of action.</span></span>

## <a name="iactionresult-type"></a><span data-ttu-id="97f61-121">IActionResult 類型</span><span class="sxs-lookup"><span data-stu-id="97f61-121">IActionResult type</span></span>

<span data-ttu-id="97f61-122">當動作中可能有多個 [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) 傳回型別時，適合使用 [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) 傳回型別。</span><span class="sxs-lookup"><span data-stu-id="97f61-122">The [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) return type is appropriate when multiple [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return types are possible in an action.</span></span> <span data-ttu-id="97f61-123">`ActionResult` 類型代表各種 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="97f61-123">The `ActionResult` types represent various HTTP status codes.</span></span> <span data-ttu-id="97f61-124">某些落在這個類別的常見傳回型別是 [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400)、[NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404) 和 [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200)。</span><span class="sxs-lookup"><span data-stu-id="97f61-124">Some common return types falling into this category are [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400), [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404), and [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).</span></span>

<span data-ttu-id="97f61-125">因為動作中有多個傳回型別和路徑，所以有必要隨意使用 [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) 屬性。</span><span class="sxs-lookup"><span data-stu-id="97f61-125">Because there are multiple return types and paths in the action, liberal use of the [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) attribute is necessary.</span></span> <span data-ttu-id="97f61-126">此屬性會產生由 [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger) 等工具所產生之 API 說明頁面的更具體描述回應詳細資料。</span><span class="sxs-lookup"><span data-stu-id="97f61-126">This attribute produces more descriptive response details for API help pages generated by tools like [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="97f61-127">`[ProducesResponseType]` 表示已知類型和 HTTP 狀態碼要由動作傳回。</span><span class="sxs-lookup"><span data-stu-id="97f61-127">`[ProducesResponseType]` indicates the known types and HTTP status codes to be returned by the action.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="97f61-128">同步動作</span><span class="sxs-lookup"><span data-stu-id="97f61-128">Synchronous action</span></span>

<span data-ttu-id="97f61-129">請考慮下列有兩個可能傳回型別的同步動作：</span><span class="sxs-lookup"><span data-stu-id="97f61-129">Consider the following synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="97f61-130">在上述動作中，當基礎資料存放區中無 `id` 所代表的產品時，會傳回 404 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="97f61-130">In the preceding action, a 404 status code is returned when the product represented by `id` doesn't exist in the underlying data store.</span></span> <span data-ttu-id="97f61-131">[NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) helper 方法會當成 `return new NotFoundResult();` 的捷徑叫用。</span><span class="sxs-lookup"><span data-stu-id="97f61-131">The [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) helper method is invoked as a shortcut to `return new NotFoundResult();`.</span></span> <span data-ttu-id="97f61-132">如果產品不存在，就會傳回代表承載的 `Product` 物件，狀態碼為 200。</span><span class="sxs-lookup"><span data-stu-id="97f61-132">If the product does exist, a `Product` object representing the payload is returned with a 200 status code.</span></span> <span data-ttu-id="97f61-133">[Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) 協助程式方法會當成 `return new OkObjectResult(product);` 的縮寫形式叫用。</span><span class="sxs-lookup"><span data-stu-id="97f61-133">The [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) helper method is invoked as the shorthand form of `return new OkObjectResult(product);`.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="97f61-134">非同步動作</span><span class="sxs-lookup"><span data-stu-id="97f61-134">Asynchronous action</span></span>

<span data-ttu-id="97f61-135">請考慮下列有兩個可能傳回型別的同步動作：</span><span class="sxs-lookup"><span data-stu-id="97f61-135">Consider the following asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="97f61-136">在上述動作中，當模型驗證失敗時並叫用 [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) 協助程式方法時，會傳回 400 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="97f61-136">In the preceding action, a 400 status code is returned when model validation fails and the [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) helper method is invoked.</span></span> <span data-ttu-id="97f61-137">例如，下列模型指出，要求必須提供 `Name` 屬性和值。</span><span class="sxs-lookup"><span data-stu-id="97f61-137">For example, the following model indicates that requests must provide the `Name` property and a value.</span></span> <span data-ttu-id="97f61-138">因此，無法在要求中提供適當的 `Name` 會導致模型驗證失敗。</span><span class="sxs-lookup"><span data-stu-id="97f61-138">Therefore, failure to provide a proper `Name` in the request causes model validation to fail.</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6)]

<span data-ttu-id="97f61-139">上述動作的其他已知傳回碼為 201，由 [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction) 協助程式方法產生。</span><span class="sxs-lookup"><span data-stu-id="97f61-139">The preceding action's other known return code is a 201, which is generated by the [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction) helper method.</span></span> <span data-ttu-id="97f61-140">在此路徑中，會傳回 `Product` 物件。</span><span class="sxs-lookup"><span data-stu-id="97f61-140">In this path, the `Product` object is returned.</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="actionresultt-type"></a><span data-ttu-id="97f61-141">ActionResult\<T> 類型</span><span class="sxs-lookup"><span data-stu-id="97f61-141">ActionResult\<T> type</span></span>

<span data-ttu-id="97f61-142">ASP.NET Core 2.1 為 Web API 控制器動作引入了 [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) 傳回型別。</span><span class="sxs-lookup"><span data-stu-id="97f61-142">ASP.NET Core 2.1 introduces the [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) return type for Web API controller actions.</span></span> <span data-ttu-id="97f61-143">它可讓您傳回衍生自 [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) 的類型或傳回[特定類型](#specific-type)。</span><span class="sxs-lookup"><span data-stu-id="97f61-143">It enables you to return a type deriving from [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) or return a [specific type](#specific-type).</span></span> <span data-ttu-id="97f61-144">`ActionResult<T>` 透過 [IActionResult 類型](#iactionresult-type)提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="97f61-144">`ActionResult<T>` offers the following benefits over the [IActionResult type](#iactionresult-type):</span></span>

* <span data-ttu-id="97f61-145">[[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) 屬性的 `Type` 屬性可被排除。</span><span class="sxs-lookup"><span data-stu-id="97f61-145">The [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) attribute's `Type` property can be excluded.</span></span>
* <span data-ttu-id="97f61-146">[隱含轉型運算子](/dotnet/csharp/language-reference/keywords/implicit)支援 `T` 和 `ActionResult` 轉換成 `ActionResult<T>`。</span><span class="sxs-lookup"><span data-stu-id="97f61-146">[Implicit cast operators](/dotnet/csharp/language-reference/keywords/implicit) support the conversion of both `T` and `ActionResult` to `ActionResult<T>`.</span></span> <span data-ttu-id="97f61-147">`T` 轉換成 [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult)，這表示 `return new ObjectResult(T);` 已簡化成 `return T;`。</span><span class="sxs-lookup"><span data-stu-id="97f61-147">`T` converts to [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), which means `return new ObjectResult(T);` is simplified to `return T;`.</span></span>

<span data-ttu-id="97f61-148">大部分的動作都有特定的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="97f61-148">Most actions have a specific return type.</span></span> <span data-ttu-id="97f61-149">如果在動作執行期間發生非預期的狀況，就不會傳回特定的類型。</span><span class="sxs-lookup"><span data-stu-id="97f61-149">Unexpected conditions can occur during action execution, in which case the specific type isn't returned.</span></span> <span data-ttu-id="97f61-150">例如，動作的輸入參數可能無法驗證模型。</span><span class="sxs-lookup"><span data-stu-id="97f61-150">For example, an action's input parameter may fail model validation.</span></span> <span data-ttu-id="97f61-151">在此情況下，通常會傳回適當的 `ActionResult` 類型而不是特定的類型。</span><span class="sxs-lookup"><span data-stu-id="97f61-151">In such a case, it's common to return the appropriate `ActionResult` type instead of the specific type.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="97f61-152">同步動作</span><span class="sxs-lookup"><span data-stu-id="97f61-152">Synchronous action</span></span>

<span data-ttu-id="97f61-153">請考慮有兩個可能傳回型別的同步動作：</span><span class="sxs-lookup"><span data-stu-id="97f61-153">Consider a synchronous action in which there are two possible return types:</span></span>

<span data-ttu-id="97f61-154">[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]</span><span class="sxs-lookup"><span data-stu-id="97f61-154">[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]</span></span>

<span data-ttu-id="97f61-155">在上述程式碼中，當資料庫中無產品時，會傳回 404 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="97f61-155">In the preceding code, a 404 status code is returned when the product doesn't exist in the database.</span></span> <span data-ttu-id="97f61-156">如果產品不存在，就會傳回對應的 `Product` 物件。</span><span class="sxs-lookup"><span data-stu-id="97f61-156">If the product does exist, the corresponding `Product` object is returned.</span></span> <span data-ttu-id="97f61-157">ASP.NET Core 2.1 前的 `return product;` 行可能是 `return Ok(product);`。</span><span class="sxs-lookup"><span data-stu-id="97f61-157">Before ASP.NET Core 2.1, the `return product;` line would have been `return Ok(product);`.</span></span>

> [!TIP]
> <span data-ttu-id="97f61-158">自 ASP.NET Core 2.1 開始，使用 `[ApiController]` 屬性裝飾控制器類別時，會啟用動作參數繫結來源推斷。</span><span class="sxs-lookup"><span data-stu-id="97f61-158">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="97f61-159">使用要求路由資料自動繫結在路由範本中比對名稱的參數名稱。</span><span class="sxs-lookup"><span data-stu-id="97f61-159">A parameter name matching a name in the route template is automatically bound using the request route data.</span></span> <span data-ttu-id="97f61-160">因此，不會以 [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) 屬性明確標註上述動作的 `id` 參數。</span><span class="sxs-lookup"><span data-stu-id="97f61-160">Consequently, the preceding action's `id` parameter isn't explicitly annotated with the [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) attribute.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="97f61-161">非同步動作</span><span class="sxs-lookup"><span data-stu-id="97f61-161">Asynchronous action</span></span>

<span data-ttu-id="97f61-162">請考慮有兩個可能傳回型別的非同步動作：</span><span class="sxs-lookup"><span data-stu-id="97f61-162">Consider an asynchronous action in which there are two possible return types:</span></span>

<span data-ttu-id="97f61-163">[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]</span><span class="sxs-lookup"><span data-stu-id="97f61-163">[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]</span></span>

<span data-ttu-id="97f61-164">如果模型驗證失敗，即叫用 [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest#Microsoft_AspNetCore_Mvc_ControllerBase_BadRequest_Microsoft_AspNetCore_Mvc_ModelBinding_ModelStateDictionary_) 以傳回 400 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="97f61-164">If model validation fails, the [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest#Microsoft_AspNetCore_Mvc_ControllerBase_BadRequest_Microsoft_AspNetCore_Mvc_ModelBinding_ModelStateDictionary_) method is invoked to return a 400 status code.</span></span> <span data-ttu-id="97f61-165">包含特定驗證錯誤的 [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) 屬性會傳送給它。</span><span class="sxs-lookup"><span data-stu-id="97f61-165">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property containing the specific validation errors is passed to it.</span></span> <span data-ttu-id="97f61-166">如果模型驗證成功，則會在資料庫中建立產品。</span><span class="sxs-lookup"><span data-stu-id="97f61-166">If model validation succeeds, the product is created in the database.</span></span> <span data-ttu-id="97f61-167">傳回 201 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="97f61-167">A 201 status code is returned.</span></span>

> [!TIP]
> <span data-ttu-id="97f61-168">自 ASP.NET Core 2.1 開始，使用 `[ApiController]` 屬性裝飾控制器類別時，會啟用動作參數繫結來源推斷。</span><span class="sxs-lookup"><span data-stu-id="97f61-168">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="97f61-169">複雜類型參數會使用要求本文自動繫結。</span><span class="sxs-lookup"><span data-stu-id="97f61-169">Complex type parameters are automatically bound using the request body.</span></span> <span data-ttu-id="97f61-170">因此，不會以 [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) 屬性明確標註上述動作的 `product` 參數。</span><span class="sxs-lookup"><span data-stu-id="97f61-170">Consequently, the preceding action's `product` parameter isn't explicitly annotated with the [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute.</span></span>
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="97f61-171">其他資源</span><span class="sxs-lookup"><span data-stu-id="97f61-171">Additional resources</span></span>

* [<span data-ttu-id="97f61-172">控制器動作</span><span class="sxs-lookup"><span data-stu-id="97f61-172">Controller actions</span></span>](xref:mvc/controllers/actions)
* [<span data-ttu-id="97f61-173">模型驗證</span><span class="sxs-lookup"><span data-stu-id="97f61-173">Model validation</span></span>](xref:mvc/models/validation)
* [<span data-ttu-id="97f61-174">使用 Swagger 的 Web API 說明頁面</span><span class="sxs-lookup"><span data-stu-id="97f61-174">Web API help pages using Swagger</span></span>](xref:tutorials/web-api-help-pages-using-swagger)
