---
title: 使用 ASP.NET Core 建置 Web API
author: scottaddie
description: 了解可用來在 ASP.NET Core 中建置 Web API 的功能，以及每個功能的使用時機。
ms.author: scaddie
ms.custom: mvc
ms.date: 08/15/2018
uid: web-api/index
ms.openlocfilehash: d410f28ff7fda3bf33f73c06b3e626dfd4ee7dd8
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2018
ms.locfileid: "41822136"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="5cee3-103">使用 ASP.NET Core 建置 Web API</span><span class="sxs-lookup"><span data-stu-id="5cee3-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="5cee3-104">作者：[Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="5cee3-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="5cee3-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5cee3-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="5cee3-106">本文件說明如何在 ASP.NET Core 中建置 Web API，以及每項功能的最佳使用時機。</span><span class="sxs-lookup"><span data-stu-id="5cee3-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="5cee3-107">來自 ControllerBase 的衍生類別</span><span class="sxs-lookup"><span data-stu-id="5cee3-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="5cee3-108">繼承自用作為 Web API 的控制器中之 <xref:Microsoft.AspNetCore.Mvc.ControllerBase> 類別。</span><span class="sxs-lookup"><span data-stu-id="5cee3-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="5cee3-109">例如: </span><span class="sxs-lookup"><span data-stu-id="5cee3-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="5cee3-110">`ControllerBase` 類別可用來存取數個屬性和方法。</span><span class="sxs-lookup"><span data-stu-id="5cee3-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="5cee3-111">在上述程式碼中，範例包含 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> 與 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>。</span><span class="sxs-lookup"><span data-stu-id="5cee3-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="5cee3-112">您可以在動作方法內呼叫這些方法，以分別傳回 HTTP 400 和 201 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="5cee3-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="5cee3-113">可以存取 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> 屬性 (亦由 `ControllerBase` 提供)，以處理要求模型驗證。</span><span class="sxs-lookup"><span data-stu-id="5cee3-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="5cee3-114">以 ApiControllerAttribute 標註類別</span><span class="sxs-lookup"><span data-stu-id="5cee3-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="5cee3-115">ASP.NET Core 2.1 新增了 [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) 屬性代表 Web API 控制器類別。</span><span class="sxs-lookup"><span data-stu-id="5cee3-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="5cee3-116">例如: </span><span class="sxs-lookup"><span data-stu-id="5cee3-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="5cee3-117">使用此屬性時，需要透過 <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> 設定的相容性版本 2.1 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="5cee3-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute.</span></span> <span data-ttu-id="5cee3-118">例如，*Startup.ConfigureServices* 中的醒目提示程式碼會設定 2.1 相容性旗標：</span><span class="sxs-lookup"><span data-stu-id="5cee3-118">For example, the highlighted code in *Startup.ConfigureServices* sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="5cee3-119">如需詳細資訊，請參閱<xref:mvc/compatibility-version>。</span><span class="sxs-lookup"><span data-stu-id="5cee3-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

<span data-ttu-id="5cee3-120">`[ApiController]` 屬性通常會與 `ControllerBase` 搭配使用，為控制站啟用 REST 特定行為。</span><span class="sxs-lookup"><span data-stu-id="5cee3-120">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="5cee3-121">`ControllerBase` 可讓您存取 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> 與 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*> 等方法。</span><span class="sxs-lookup"><span data-stu-id="5cee3-121">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="5cee3-122">另一個方法是建立標註了 `[ApiController]` 屬性的自訂基底控制器類別：</span><span class="sxs-lookup"><span data-stu-id="5cee3-122">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="5cee3-123">下列章節說明該屬性新增的便利功能。</span><span class="sxs-lookup"><span data-stu-id="5cee3-123">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="5cee3-124">HTTP 400 自動回應</span><span class="sxs-lookup"><span data-stu-id="5cee3-124">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="5cee3-125">驗證錯誤會自動觸發 HTTP 400 回應。</span><span class="sxs-lookup"><span data-stu-id="5cee3-125">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="5cee3-126">您的動作中將不再需要下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="5cee3-126">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="5cee3-127">當 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> 屬性設定為 `true` 時，會停用預設行為。</span><span class="sxs-lookup"><span data-stu-id="5cee3-127">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="5cee3-128">請在 *Startup.ConfigureServices* 的 `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` 之後新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="5cee3-128">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="5cee3-129">繫結來源參數推斷</span><span class="sxs-lookup"><span data-stu-id="5cee3-129">Binding source parameter inference</span></span>

<span data-ttu-id="5cee3-130">繫結來源屬性可定義動作參數值的所在位置。</span><span class="sxs-lookup"><span data-stu-id="5cee3-130">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="5cee3-131">系統提供下列繫結來源屬性：</span><span class="sxs-lookup"><span data-stu-id="5cee3-131">The following binding source attributes exist:</span></span>

|<span data-ttu-id="5cee3-132">屬性</span><span class="sxs-lookup"><span data-stu-id="5cee3-132">Attribute</span></span>|<span data-ttu-id="5cee3-133">繫結來源</span><span class="sxs-lookup"><span data-stu-id="5cee3-133">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="5cee3-134">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="5cee3-134">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="5cee3-135">要求本文</span><span class="sxs-lookup"><span data-stu-id="5cee3-135">Request body</span></span> |
|<span data-ttu-id="5cee3-136">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="5cee3-136">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="5cee3-137">要求本文中的表單資料</span><span class="sxs-lookup"><span data-stu-id="5cee3-137">Form data in the request body</span></span> |
|<span data-ttu-id="5cee3-138">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="5cee3-138">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="5cee3-139">要求標頭</span><span class="sxs-lookup"><span data-stu-id="5cee3-139">Request header</span></span> |
|<span data-ttu-id="5cee3-140">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="5cee3-140">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="5cee3-141">要求查詢字串參數</span><span class="sxs-lookup"><span data-stu-id="5cee3-141">Request query string parameter</span></span> |
|<span data-ttu-id="5cee3-142">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="5cee3-142">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="5cee3-143">來自目前要求的路由資料</span><span class="sxs-lookup"><span data-stu-id="5cee3-143">Route data from the current request</span></span> |
|<span data-ttu-id="5cee3-144">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="5cee3-144">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="5cee3-145">作為動作參數插入的要求服務</span><span class="sxs-lookup"><span data-stu-id="5cee3-145">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="5cee3-146">當值可能包含 `%2f` (也就是 `/`) 時，請勿使用 `[FromRoute]`。</span><span class="sxs-lookup"><span data-stu-id="5cee3-146">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="5cee3-147">`%2f` 不會是未逸出的 `/`。</span><span class="sxs-lookup"><span data-stu-id="5cee3-147">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="5cee3-148">如果值可能包含 `%2f`，請使用 `[FromQuery]`。</span><span class="sxs-lookup"><span data-stu-id="5cee3-148">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="5cee3-149">不含 `[ApiController]` 屬性時，即會明確定義繫結來源屬性。</span><span class="sxs-lookup"><span data-stu-id="5cee3-149">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="5cee3-150">在下列範例中，`[FromQuery]` 屬性表示 `discontinuedOnly` 參數值是在要求 URL 查詢字串中提供：</span><span class="sxs-lookup"><span data-stu-id="5cee3-150">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="5cee3-151">系統會依據動作參數的預設資料來源，套用推斷規則。</span><span class="sxs-lookup"><span data-stu-id="5cee3-151">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="5cee3-152">然後，這些規則會設定繫結來源，否則就可能要由您手動將其套用至動作參數。</span><span class="sxs-lookup"><span data-stu-id="5cee3-152">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="5cee3-153">繫結來源屬性的行為如下所示：</span><span class="sxs-lookup"><span data-stu-id="5cee3-153">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="5cee3-154">系統會依據複雜類型參數推斷 **[FromBody]**。</span><span class="sxs-lookup"><span data-stu-id="5cee3-154">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="5cee3-155">如果是任何具有像是 <xref:Microsoft.AspNetCore.Http.IFormCollection> 與 <xref:System.Threading.CancellationToken> 等特殊意義的複雜內建類型，則為此規則的例外。</span><span class="sxs-lookup"><span data-stu-id="5cee3-155">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="5cee3-156">繫結來源推斷程式碼會忽略這些特殊的類型。</span><span class="sxs-lookup"><span data-stu-id="5cee3-156">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="5cee3-157">並不會為像是 `string` 或 `int` 等簡單型別，推斷 `[FromBody]`。</span><span class="sxs-lookup"><span data-stu-id="5cee3-157">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="5cee3-158">因此，需要使用該功能時，應為簡單型別使用 `[FromBody]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="5cee3-158">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is desired.</span></span> <span data-ttu-id="5cee3-159">當動作已明確指定多個參數 (透過 `[FromBody]`) 或推斷為從要求主體繫結時，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="5cee3-159">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="5cee3-160">例如，下列動作簽章會造成例外狀況：</span><span class="sxs-lookup"><span data-stu-id="5cee3-160">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="5cee3-161">為類型 <xref:Microsoft.AspNetCore.Http.IFormFile> 與 <xref:Microsoft.AspNetCore.Http.IFormFileCollection> 的動作參數，推斷 **[FromForm]**。</span><span class="sxs-lookup"><span data-stu-id="5cee3-161">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="5cee3-162">而不會依據任何簡單或使用者定義的類型進行推斷。</span><span class="sxs-lookup"><span data-stu-id="5cee3-162">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="5cee3-163">系統會依據符合路由範本參數的任何動作參數名稱，推斷 **[FromRoute]**。</span><span class="sxs-lookup"><span data-stu-id="5cee3-163">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="5cee3-164">如果有多個路由符合動作參數，則會將任何路由值視為 `[FromRoute]`。</span><span class="sxs-lookup"><span data-stu-id="5cee3-164">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="5cee3-165">系統會依據任何其他動作參數，推斷 **[FromQuery]**。</span><span class="sxs-lookup"><span data-stu-id="5cee3-165">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="5cee3-166">當 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> 屬性設定為 `true` 時，會停用預設推斷規則。</span><span class="sxs-lookup"><span data-stu-id="5cee3-166">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="5cee3-167">請在 *Startup.ConfigureServices* 的 `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` 之後新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="5cee3-167">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="5cee3-168">多部分/表單資料要求推斷</span><span class="sxs-lookup"><span data-stu-id="5cee3-168">Multipart/form-data request inference</span></span>

<span data-ttu-id="5cee3-169">當動作參數標註了 [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) 屬性時，會推斷 `multipart/form-data` 要求內容類型。</span><span class="sxs-lookup"><span data-stu-id="5cee3-169">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="5cee3-170">當 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> 屬性設定為 `true` 時，會停用預設行為。</span><span class="sxs-lookup"><span data-stu-id="5cee3-170">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span> <span data-ttu-id="5cee3-171">請在 *Startup.ConfigureServices* 的 `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` 之後新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="5cee3-171">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="5cee3-172">屬性路由需求</span><span class="sxs-lookup"><span data-stu-id="5cee3-172">Attribute routing requirement</span></span>

<span data-ttu-id="5cee3-173">屬性路由已變成必要項目。</span><span class="sxs-lookup"><span data-stu-id="5cee3-173">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="5cee3-174">例如: </span><span class="sxs-lookup"><span data-stu-id="5cee3-174">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="5cee3-175">無法透過 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> 中所定義的[慣例路由](xref:mvc/controllers/routing#conventional-routing)或 *Startup.Configure* 中的 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*>，來存取動作。</span><span class="sxs-lookup"><span data-stu-id="5cee3-175">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in *Startup.Configure*.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="5cee3-176">其他資源</span><span class="sxs-lookup"><span data-stu-id="5cee3-176">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
