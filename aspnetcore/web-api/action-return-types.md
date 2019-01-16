---
title: ASP.NET Core Web API 中的控制器動作傳回型別
author: scottaddie
description: 了解在 ASP.NET Core Web API 中使用各種控制器動作方法傳回型別。
ms.author: scaddie
ms.custom: mvc
ms.date: 01/04/2019
uid: web-api/action-return-types
ms.openlocfilehash: 98d70e0379d353cff98a6d7a13f2dd00eb4da206
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098728"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a><span data-ttu-id="46b7b-103">ASP.NET Core Web API 中的控制器動作傳回型別</span><span class="sxs-lookup"><span data-stu-id="46b7b-103">Controller action return types in ASP.NET Core Web API</span></span>

<span data-ttu-id="46b7b-104">作者：[Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="46b7b-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="46b7b-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="46b7b-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="46b7b-106">ASP.NET Core 提供下列 Web API 控制器動作傳回型別選項：</span><span class="sxs-lookup"><span data-stu-id="46b7b-106">ASP.NET Core offers the following options for Web API controller action return types:</span></span>

::: moniker range=">= aspnetcore-2.1"

* [<span data-ttu-id="46b7b-107">特定類型</span><span class="sxs-lookup"><span data-stu-id="46b7b-107">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="46b7b-108">IActionResult</span><span class="sxs-lookup"><span data-stu-id="46b7b-108">IActionResult</span></span>](#iactionresult-type)
* [<span data-ttu-id="46b7b-109">ActionResult\<T></span><span class="sxs-lookup"><span data-stu-id="46b7b-109">ActionResult\<T></span></span>](#actionresultt-type)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* [<span data-ttu-id="46b7b-110">特定類型</span><span class="sxs-lookup"><span data-stu-id="46b7b-110">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="46b7b-111">IActionResult</span><span class="sxs-lookup"><span data-stu-id="46b7b-111">IActionResult</span></span>](#iactionresult-type)

::: moniker-end

<span data-ttu-id="46b7b-112">本文件說明使用每個傳回型別的最適時機。</span><span class="sxs-lookup"><span data-stu-id="46b7b-112">This document explains when it's most appropriate to use each return type.</span></span>

## <a name="specific-type"></a><span data-ttu-id="46b7b-113">特定類型</span><span class="sxs-lookup"><span data-stu-id="46b7b-113">Specific type</span></span>

<span data-ttu-id="46b7b-114">最簡單的動作傳回基本或複雜的資料類型 (例如，`string` 或自訂物件類型)。</span><span class="sxs-lookup"><span data-stu-id="46b7b-114">The simplest action returns a primitive or complex data type (for example, `string` or a custom object type).</span></span> <span data-ttu-id="46b7b-115">請考慮下列動作，它會傳回自訂 `Product` 物件的集合：</span><span class="sxs-lookup"><span data-stu-id="46b7b-115">Consider the following action, which returns a collection of custom `Product` objects:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

<span data-ttu-id="46b7b-116">無已知條件，無法確保在動作執行期間能滿足傳回特定類型。</span><span class="sxs-lookup"><span data-stu-id="46b7b-116">Without known conditions to safeguard against during action execution, returning a specific type could suffice.</span></span> <span data-ttu-id="46b7b-117">上述動作不接受任何參數，因此不需要驗證參數條件約束。</span><span class="sxs-lookup"><span data-stu-id="46b7b-117">The preceding action accepts no parameters, so parameter constraints validation isn't needed.</span></span>

<span data-ttu-id="46b7b-118">當動作需要計入已知情況時，會引入多個傳回路徑。</span><span class="sxs-lookup"><span data-stu-id="46b7b-118">When known conditions need to be accounted for in an action, multiple return paths are introduced.</span></span> <span data-ttu-id="46b7b-119">在此情況下，通常會混合 [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) 傳回型別和基本或複雜的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="46b7b-119">In such a case, it's common to mix an [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return type with the primitive or complex return type.</span></span> <span data-ttu-id="46b7b-120">[IActionResult](#iactionresult-type) 或 [ActionResult\<T>](#actionresultt-type) 是容納此動作類型的必要項目。</span><span class="sxs-lookup"><span data-stu-id="46b7b-120">Either [IActionResult](#iactionresult-type) or [ActionResult\<T>](#actionresultt-type) are necessary to accommodate this type of action.</span></span>

## <a name="iactionresult-type"></a><span data-ttu-id="46b7b-121">IActionResult 類型</span><span class="sxs-lookup"><span data-stu-id="46b7b-121">IActionResult type</span></span>

<span data-ttu-id="46b7b-122">當動作中可能有多個 [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) 傳回型別時，適合使用 [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) 傳回型別。</span><span class="sxs-lookup"><span data-stu-id="46b7b-122">The [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) return type is appropriate when multiple [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return types are possible in an action.</span></span> <span data-ttu-id="46b7b-123">`ActionResult` 類型代表各種 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="46b7b-123">The `ActionResult` types represent various HTTP status codes.</span></span> <span data-ttu-id="46b7b-124">某些落在這個類別的常見傳回型別是 [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400)、[NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404) 和 [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200)。</span><span class="sxs-lookup"><span data-stu-id="46b7b-124">Some common return types falling into this category are [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400), [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404), and [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).</span></span>

<span data-ttu-id="46b7b-125">因為動作中有多個傳回型別和路徑，所以有必要隨意使用 [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) 屬性。</span><span class="sxs-lookup"><span data-stu-id="46b7b-125">Because there are multiple return types and paths in the action, liberal use of the [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) attribute is necessary.</span></span> <span data-ttu-id="46b7b-126">此屬性會產生由 [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger) 等工具所產生之 API 說明頁面的更具體描述回應詳細資料。</span><span class="sxs-lookup"><span data-stu-id="46b7b-126">This attribute produces more descriptive response details for API help pages generated by tools like [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="46b7b-127">`[ProducesResponseType]` 表示已知類型和 HTTP 狀態碼要由動作傳回。</span><span class="sxs-lookup"><span data-stu-id="46b7b-127">`[ProducesResponseType]` indicates the known types and HTTP status codes to be returned by the action.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="46b7b-128">同步動作</span><span class="sxs-lookup"><span data-stu-id="46b7b-128">Synchronous action</span></span>

<span data-ttu-id="46b7b-129">請考慮下列有兩個可能傳回型別的同步動作：</span><span class="sxs-lookup"><span data-stu-id="46b7b-129">Consider the following synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="46b7b-130">在上述動作中，當基礎資料存放區中無 `id` 所代表的產品時，會傳回 404 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="46b7b-130">In the preceding action, a 404 status code is returned when the product represented by `id` doesn't exist in the underlying data store.</span></span> <span data-ttu-id="46b7b-131">[NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) helper 方法會當成 `return new NotFoundResult();` 的捷徑叫用。</span><span class="sxs-lookup"><span data-stu-id="46b7b-131">The [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) helper method is invoked as a shortcut to `return new NotFoundResult();`.</span></span> <span data-ttu-id="46b7b-132">如果產品不存在，就會傳回代表承載的 `Product` 物件，狀態碼為 200。</span><span class="sxs-lookup"><span data-stu-id="46b7b-132">If the product does exist, a `Product` object representing the payload is returned with a 200 status code.</span></span> <span data-ttu-id="46b7b-133">[Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) 協助程式方法會當成 `return new OkObjectResult(product);` 的縮寫形式叫用。</span><span class="sxs-lookup"><span data-stu-id="46b7b-133">The [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) helper method is invoked as the shorthand form of `return new OkObjectResult(product);`.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="46b7b-134">非同步動作</span><span class="sxs-lookup"><span data-stu-id="46b7b-134">Asynchronous action</span></span>

<span data-ttu-id="46b7b-135">請考慮下列有兩個可能傳回型別的同步動作：</span><span class="sxs-lookup"><span data-stu-id="46b7b-135">Consider the following asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="46b7b-136">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="46b7b-136">In the preceding code:</span></span>

* <span data-ttu-id="46b7b-137">當產品描述包含 "XYZ Widget" 時，ASP.NET Core 執行階段會傳回 400 狀態碼 ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*))。</span><span class="sxs-lookup"><span data-stu-id="46b7b-137">A 400 status code ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) is returned by the ASP.NET Core runtime when the product description contains "XYZ Widget".</span></span>
* <span data-ttu-id="46b7b-138">當建立產品時，[CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) 方法會產生 201 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="46b7b-138">A 201 status code is generated by the [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) method when a product is created.</span></span> <span data-ttu-id="46b7b-139">在此程式碼路徑中，會傳回 `Product` 物件。</span><span class="sxs-lookup"><span data-stu-id="46b7b-139">In this code path, the `Product` object is returned.</span></span>

<span data-ttu-id="46b7b-140">例如，下列模型指出要求必須包含 `Name` 和 `Description` 屬性。</span><span class="sxs-lookup"><span data-stu-id="46b7b-140">For example, the following model indicates that requests must include the `Name` and `Description` properties.</span></span> <span data-ttu-id="46b7b-141">因此，若無法在要求中提供 `Name` 和 `Description`，就會導致模型驗證失敗。</span><span class="sxs-lookup"><span data-stu-id="46b7b-141">Therefore, failure to provide `Name` and `Description` in the request causes model validation to fail.</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6,8-9)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="46b7b-142">如果套用 ASP.NET Core 2.1 或更新版本中的 [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) 屬性，則模型驗證錯誤會導致產生 400 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="46b7b-142">If the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute in ASP.NET Core 2.1 or later  is applied, model validation errors result in a 400 status code.</span></span> <span data-ttu-id="46b7b-143">如需詳細資訊，請參閱[自動 HTTP 400 回應](xref:web-api/index#automatic-http-400-responses)。</span><span class="sxs-lookup"><span data-stu-id="46b7b-143">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="actionresultt-type"></a><span data-ttu-id="46b7b-144">ActionResult\<T> 類型</span><span class="sxs-lookup"><span data-stu-id="46b7b-144">ActionResult\<T> type</span></span>

<span data-ttu-id="46b7b-145">ASP.NET Core 2.1 為 Web API 控制器動作引入了 [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) 傳回型別。</span><span class="sxs-lookup"><span data-stu-id="46b7b-145">ASP.NET Core 2.1 introduces the [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) return type for Web API controller actions.</span></span> <span data-ttu-id="46b7b-146">它可讓您傳回衍生自 [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) 的類型或傳回[特定類型](#specific-type)。</span><span class="sxs-lookup"><span data-stu-id="46b7b-146">It enables you to return a type deriving from [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) or return a [specific type](#specific-type).</span></span> <span data-ttu-id="46b7b-147">`ActionResult<T>` 透過 [IActionResult 類型](#iactionresult-type)提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="46b7b-147">`ActionResult<T>` offers the following benefits over the [IActionResult type](#iactionresult-type):</span></span>

* <span data-ttu-id="46b7b-148">[[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) 屬性的 `Type` 屬性可被排除。</span><span class="sxs-lookup"><span data-stu-id="46b7b-148">The [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) attribute's `Type` property can be excluded.</span></span> <span data-ttu-id="46b7b-149">例如，`[ProducesResponseType(200, Type = typeof(Product))]` 簡化為 `[ProducesResponseType(200)]`。</span><span class="sxs-lookup"><span data-stu-id="46b7b-149">For example, `[ProducesResponseType(200, Type = typeof(Product))]` is simplified to `[ProducesResponseType(200)]`.</span></span> <span data-ttu-id="46b7b-150">該動作的預期傳回型別會改為從 `ActionResult<T>` 中的 `T` 推斷。</span><span class="sxs-lookup"><span data-stu-id="46b7b-150">The action's expected return type is instead inferred from the `T` in `ActionResult<T>`.</span></span>
* <span data-ttu-id="46b7b-151">[隱含轉型運算子](/dotnet/csharp/language-reference/keywords/implicit)支援 `T` 和 `ActionResult` 轉換成 `ActionResult<T>`。</span><span class="sxs-lookup"><span data-stu-id="46b7b-151">[Implicit cast operators](/dotnet/csharp/language-reference/keywords/implicit) support the conversion of both `T` and `ActionResult` to `ActionResult<T>`.</span></span> <span data-ttu-id="46b7b-152">`T` 轉換成 [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult)，這表示 `return new ObjectResult(T);` 已簡化成 `return T;`。</span><span class="sxs-lookup"><span data-stu-id="46b7b-152">`T` converts to [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), which means `return new ObjectResult(T);` is simplified to `return T;`.</span></span>

<span data-ttu-id="46b7b-153">C# 不支援介面上的隱含轉換運算子。</span><span class="sxs-lookup"><span data-stu-id="46b7b-153">C# doesn't support implicit cast operators on interfaces.</span></span> <span data-ttu-id="46b7b-154">因此，必須將介面轉換為具象型別才能使用 `ActionResult<T>`。</span><span class="sxs-lookup"><span data-stu-id="46b7b-154">Consequently, conversion of the interface to a concrete type is necessary to use `ActionResult<T>`.</span></span> <span data-ttu-id="46b7b-155">例如，在下列範例中使用 `IEnumerable` 將無法運作：</span><span class="sxs-lookup"><span data-stu-id="46b7b-155">For example, use of `IEnumerable` in the following example doesn't work:</span></span>

```csharp
[HttpGet]
public ActionResult<IEnumerable<Product>> Get()
{
    return _repository.GetProducts();
}
```

<span data-ttu-id="46b7b-156">修正上述程式碼的其中一個選項是傳回 `_repository.GetProducts().ToList();`。</span><span class="sxs-lookup"><span data-stu-id="46b7b-156">One option to fix the preceding code is to return `_repository.GetProducts().ToList();`.</span></span>

<span data-ttu-id="46b7b-157">大部分的動作都有特定的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="46b7b-157">Most actions have a specific return type.</span></span> <span data-ttu-id="46b7b-158">如果在動作執行期間發生非預期的狀況，就不會傳回特定的類型。</span><span class="sxs-lookup"><span data-stu-id="46b7b-158">Unexpected conditions can occur during action execution, in which case the specific type isn't returned.</span></span> <span data-ttu-id="46b7b-159">例如，動作的輸入參數可能無法驗證模型。</span><span class="sxs-lookup"><span data-stu-id="46b7b-159">For example, an action's input parameter may fail model validation.</span></span> <span data-ttu-id="46b7b-160">在此情況下，通常會傳回適當的 `ActionResult` 類型而不是特定的類型。</span><span class="sxs-lookup"><span data-stu-id="46b7b-160">In such a case, it's common to return the appropriate `ActionResult` type instead of the specific type.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="46b7b-161">同步動作</span><span class="sxs-lookup"><span data-stu-id="46b7b-161">Synchronous action</span></span>

<span data-ttu-id="46b7b-162">請考慮有兩個可能傳回型別的同步動作：</span><span class="sxs-lookup"><span data-stu-id="46b7b-162">Consider a synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="46b7b-163">在上述程式碼中，當資料庫中無產品時，會傳回 404 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="46b7b-163">In the preceding code, a 404 status code is returned when the product doesn't exist in the database.</span></span> <span data-ttu-id="46b7b-164">如果產品不存在，就會傳回對應的 `Product` 物件。</span><span class="sxs-lookup"><span data-stu-id="46b7b-164">If the product does exist, the corresponding `Product` object is returned.</span></span> <span data-ttu-id="46b7b-165">ASP.NET Core 2.1 前的 `return product;` 行可能是 `return Ok(product);`。</span><span class="sxs-lookup"><span data-stu-id="46b7b-165">Before ASP.NET Core 2.1, the `return product;` line would have been `return Ok(product);`.</span></span>

> [!TIP]
> <span data-ttu-id="46b7b-166">自 ASP.NET Core 2.1 開始，使用 `[ApiController]` 屬性裝飾控制器類別時，會啟用動作參數繫結來源推斷。</span><span class="sxs-lookup"><span data-stu-id="46b7b-166">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="46b7b-167">使用要求路由資料自動繫結在路由範本中比對名稱的參數名稱。</span><span class="sxs-lookup"><span data-stu-id="46b7b-167">A parameter name matching a name in the route template is automatically bound using the request route data.</span></span> <span data-ttu-id="46b7b-168">因此，不會以 [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) 屬性明確標註上述動作的 `id` 參數。</span><span class="sxs-lookup"><span data-stu-id="46b7b-168">Consequently, the preceding action's `id` parameter isn't explicitly annotated with the [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) attribute.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="46b7b-169">非同步動作</span><span class="sxs-lookup"><span data-stu-id="46b7b-169">Asynchronous action</span></span>

<span data-ttu-id="46b7b-170">請考慮有兩個可能傳回型別的非同步動作：</span><span class="sxs-lookup"><span data-stu-id="46b7b-170">Consider an asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="46b7b-171">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="46b7b-171">In the preceding code:</span></span>

* <span data-ttu-id="46b7b-172">在下列情況下，ASP.NET Core 執行階段會傳回 400 狀態碼 ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*))：</span><span class="sxs-lookup"><span data-stu-id="46b7b-172">A 400 status code ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) is returned by the ASP.NET Core runtime when:</span></span>
  * <span data-ttu-id="46b7b-173">已套用 [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) 屬性，而模型驗證失敗。</span><span class="sxs-lookup"><span data-stu-id="46b7b-173">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute has been applied and model validation fails.</span></span>
  * <span data-ttu-id="46b7b-174">產品描述包含 "XYZ Widget"。</span><span class="sxs-lookup"><span data-stu-id="46b7b-174">The product description contains "XYZ Widget".</span></span>
* <span data-ttu-id="46b7b-175">當建立產品時，[CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) 方法會產生 201 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="46b7b-175">A 201 status code is generated by the [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) method when a product is created.</span></span> <span data-ttu-id="46b7b-176">在此程式碼路徑中，會傳回 `Product` 物件。</span><span class="sxs-lookup"><span data-stu-id="46b7b-176">In this code path, the `Product` object is returned.</span></span>

> [!TIP]
> <span data-ttu-id="46b7b-177">自 ASP.NET Core 2.1 開始，使用 `[ApiController]` 屬性裝飾控制器類別時，會啟用動作參數繫結來源推斷。</span><span class="sxs-lookup"><span data-stu-id="46b7b-177">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="46b7b-178">複雜類型參數會使用要求本文自動繫結。</span><span class="sxs-lookup"><span data-stu-id="46b7b-178">Complex type parameters are automatically bound using the request body.</span></span> <span data-ttu-id="46b7b-179">因此，不會以 [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) 屬性明確標註上述動作的 `product` 參數。</span><span class="sxs-lookup"><span data-stu-id="46b7b-179">Consequently, the preceding action's `product` parameter isn't explicitly annotated with the [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="46b7b-180">其他資源</span><span class="sxs-lookup"><span data-stu-id="46b7b-180">Additional resources</span></span>

* <xref:mvc/controllers/actions>
* <xref:mvc/models/validation>
* <xref:tutorials/web-api-help-pages-using-swagger>
