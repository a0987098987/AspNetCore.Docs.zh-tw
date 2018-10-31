---
title: 使用 ASP.NET Core 建置 Web API
author: scottaddie
description: 了解可用來在 ASP.NET Core 中建置 Web API 的功能，以及每個功能的使用時機。
ms.author: scaddie
ms.custom: mvc
ms.date: 10/30/2018
uid: web-api/index
ms.openlocfilehash: b3e26bee5e4dc8937e810bc5db300a486437f568
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244758"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="6830e-103">使用 ASP.NET Core 建置 Web API</span><span class="sxs-lookup"><span data-stu-id="6830e-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="6830e-104">作者：[Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="6830e-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="6830e-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6830e-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6830e-106">本文件說明如何在 ASP.NET Core 中建置 Web API，以及每項功能的最佳使用時機。</span><span class="sxs-lookup"><span data-stu-id="6830e-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="6830e-107">來自 ControllerBase 的衍生類別</span><span class="sxs-lookup"><span data-stu-id="6830e-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="6830e-108">繼承自用作為 Web API 的控制器中之 <xref:Microsoft.AspNetCore.Mvc.ControllerBase> 類別。</span><span class="sxs-lookup"><span data-stu-id="6830e-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="6830e-109">例如: </span><span class="sxs-lookup"><span data-stu-id="6830e-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="6830e-110">`ControllerBase` 類別可用來存取數個屬性和方法。</span><span class="sxs-lookup"><span data-stu-id="6830e-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="6830e-111">在上述程式碼中，範例包含 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> 與 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>。</span><span class="sxs-lookup"><span data-stu-id="6830e-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="6830e-112">您可以在動作方法內呼叫這些方法，以分別傳回 HTTP 400 和 201 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="6830e-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="6830e-113">可以存取 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> 屬性 (亦由 `ControllerBase` 提供)，以處理要求模型驗證。</span><span class="sxs-lookup"><span data-stu-id="6830e-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotation-with-apicontrollerattribute"></a><span data-ttu-id="6830e-114">使用 ApiControllerAttribute 標註</span><span class="sxs-lookup"><span data-stu-id="6830e-114">Annotation with ApiControllerAttribute</span></span>

<span data-ttu-id="6830e-115">ASP.NET Core 2.1 新增了 [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) 屬性代表 Web API 控制器類別。</span><span class="sxs-lookup"><span data-stu-id="6830e-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="6830e-116">例如: </span><span class="sxs-lookup"><span data-stu-id="6830e-116">For example:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="6830e-117">透過 <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> 所設定的 2.1 或更新相容性版本必須在控制器層級使用此屬性。</span><span class="sxs-lookup"><span data-stu-id="6830e-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute at the controller level.</span></span> <span data-ttu-id="6830e-118">舉例來說，`Startup.ConfigureServices` 中的醒目提示程式碼會設定 2.1 相容性旗標：</span><span class="sxs-lookup"><span data-stu-id="6830e-118">For example, the highlighted code in `Startup.ConfigureServices` sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="6830e-119">如需詳細資訊，請參閱<xref:mvc/compatibility-version>。</span><span class="sxs-lookup"><span data-stu-id="6830e-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6830e-120">在 ASP.NET Core 2.2 或更新版本中，`[ApiController]` 屬性可以套用至組件。</span><span class="sxs-lookup"><span data-stu-id="6830e-120">In ASP.NET Core 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="6830e-121">以這種方式標註會將 Web API 行為套用至組件中的所有控制器。</span><span class="sxs-lookup"><span data-stu-id="6830e-121">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="6830e-122">請注意，沒有任何方法可以退出個別控制器。</span><span class="sxs-lookup"><span data-stu-id="6830e-122">Beware that there's no way to opt out for individual controllers.</span></span> <span data-ttu-id="6830e-123">建議您，組件層級屬性應套用至 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="6830e-123">As a recommendation, assembly-level attributes should be applied to the `Startup` class:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ApiControllerAttributeOnAssembly&highlight=1)]

<span data-ttu-id="6830e-124">透過 <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> 所設定的 2.2 或更新相容性版本必須在組件層級使用此屬性。</span><span class="sxs-lookup"><span data-stu-id="6830e-124">A compatibility version of 2.2 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute at the assembly level.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6830e-125">`[ApiController]` 屬性通常會與 `ControllerBase` 搭配使用，為控制站啟用 REST 特定行為。</span><span class="sxs-lookup"><span data-stu-id="6830e-125">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="6830e-126">`ControllerBase` 可讓您存取 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> 與 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*> 等方法。</span><span class="sxs-lookup"><span data-stu-id="6830e-126">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="6830e-127">另一個方法是建立標註了 `[ApiController]` 屬性的自訂基底控制器類別：</span><span class="sxs-lookup"><span data-stu-id="6830e-127">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="6830e-128">下列章節說明該屬性新增的便利功能。</span><span class="sxs-lookup"><span data-stu-id="6830e-128">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="6830e-129">HTTP 400 自動回應</span><span class="sxs-lookup"><span data-stu-id="6830e-129">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="6830e-130">驗證錯誤會自動觸發 HTTP 400 回應。</span><span class="sxs-lookup"><span data-stu-id="6830e-130">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="6830e-131">您的動作中將不再需要下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="6830e-131">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="6830e-132">使用 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> 可自訂所產生回應的輸出。</span><span class="sxs-lookup"><span data-stu-id="6830e-132">Use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to customize the output of the resulting response.</span></span>

<span data-ttu-id="6830e-133">當 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> 屬性設定為 `true` 時，會停用預設行為。</span><span class="sxs-lookup"><span data-stu-id="6830e-133">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="6830e-134">在 `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);` 後，於 `Startup.ConfigureServices` 中新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="6830e-134">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=7)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6830e-135">使用 2.2 或更新版本的相容性旗標，HTTP 400 回應的預設回應類型為 <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>。</span><span class="sxs-lookup"><span data-stu-id="6830e-135">With a compatibility flag of 2.2 or later, the default response type for HTTP 400 responses is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="6830e-136">`ValidationProblemDetails` 類型符合 [RFC 7807 規格](https://tools.ietf.org/html/rfc7807)。</span><span class="sxs-lookup"><span data-stu-id="6830e-136">The `ValidationProblemDetails` type complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span> <span data-ttu-id="6830e-137">將 `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` 屬性設定為 `true`，以改為傳回 <xref:Microsoft.AspNetCore.Mvc.SerializableError> 的 ASP.NET Core 2.1 錯誤格式。</span><span class="sxs-lookup"><span data-stu-id="6830e-137">Set the `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` property to `true` to instead return the ASP.NET Core 2.1 error format of <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span></span> <span data-ttu-id="6830e-138">將下列程式碼加入 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="6830e-138">Add the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2)
    .ConfigureApiBehaviorOptions(options =>
    {
        options
          .SuppressUseValidationProblemDetailsForInvalidModelStateResponses = true;
    });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="6830e-139">繫結來源參數推斷</span><span class="sxs-lookup"><span data-stu-id="6830e-139">Binding source parameter inference</span></span>

<span data-ttu-id="6830e-140">繫結來源屬性可定義動作參數值的所在位置。</span><span class="sxs-lookup"><span data-stu-id="6830e-140">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="6830e-141">系統提供下列繫結來源屬性：</span><span class="sxs-lookup"><span data-stu-id="6830e-141">The following binding source attributes exist:</span></span>

|<span data-ttu-id="6830e-142">屬性</span><span class="sxs-lookup"><span data-stu-id="6830e-142">Attribute</span></span>|<span data-ttu-id="6830e-143">繫結來源</span><span class="sxs-lookup"><span data-stu-id="6830e-143">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="6830e-144">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="6830e-144">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="6830e-145">要求本文</span><span class="sxs-lookup"><span data-stu-id="6830e-145">Request body</span></span> |
|<span data-ttu-id="6830e-146">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="6830e-146">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="6830e-147">要求本文中的表單資料</span><span class="sxs-lookup"><span data-stu-id="6830e-147">Form data in the request body</span></span> |
|<span data-ttu-id="6830e-148">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="6830e-148">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="6830e-149">要求標頭</span><span class="sxs-lookup"><span data-stu-id="6830e-149">Request header</span></span> |
|<span data-ttu-id="6830e-150">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="6830e-150">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="6830e-151">要求查詢字串參數</span><span class="sxs-lookup"><span data-stu-id="6830e-151">Request query string parameter</span></span> |
|<span data-ttu-id="6830e-152">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="6830e-152">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="6830e-153">來自目前要求的路由資料</span><span class="sxs-lookup"><span data-stu-id="6830e-153">Route data from the current request</span></span> |
|<span data-ttu-id="6830e-154">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="6830e-154">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="6830e-155">作為動作參數插入的要求服務</span><span class="sxs-lookup"><span data-stu-id="6830e-155">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="6830e-156">當值可能包含 `%2f` (也就是 `/`) 時，請勿使用 `[FromRoute]`。</span><span class="sxs-lookup"><span data-stu-id="6830e-156">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="6830e-157">`%2f` 不會是未逸出的 `/`。</span><span class="sxs-lookup"><span data-stu-id="6830e-157">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="6830e-158">如果值可能包含 `%2f`，請使用 `[FromQuery]`。</span><span class="sxs-lookup"><span data-stu-id="6830e-158">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="6830e-159">不含 `[ApiController]` 屬性時，即會明確定義繫結來源屬性。</span><span class="sxs-lookup"><span data-stu-id="6830e-159">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="6830e-160">在下列範例中，`[FromQuery]` 屬性表示 `discontinuedOnly` 參數值是在要求 URL 查詢字串中提供：</span><span class="sxs-lookup"><span data-stu-id="6830e-160">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="6830e-161">系統會依據動作參數的預設資料來源，套用推斷規則。</span><span class="sxs-lookup"><span data-stu-id="6830e-161">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="6830e-162">然後，這些規則會設定繫結來源，否則就可能要由您手動將其套用至動作參數。</span><span class="sxs-lookup"><span data-stu-id="6830e-162">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="6830e-163">繫結來源屬性的行為如下所示：</span><span class="sxs-lookup"><span data-stu-id="6830e-163">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="6830e-164">系統會依據複雜類型參數推斷 **[FromBody]**。</span><span class="sxs-lookup"><span data-stu-id="6830e-164">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="6830e-165">如果是任何具有像是 <xref:Microsoft.AspNetCore.Http.IFormCollection> 與 <xref:System.Threading.CancellationToken> 等特殊意義的複雜內建類型，則為此規則的例外。</span><span class="sxs-lookup"><span data-stu-id="6830e-165">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="6830e-166">繫結來源推斷程式碼會忽略這些特殊的類型。</span><span class="sxs-lookup"><span data-stu-id="6830e-166">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="6830e-167">並不會為像是 `string` 或 `int` 等簡單型別，推斷 `[FromBody]`。</span><span class="sxs-lookup"><span data-stu-id="6830e-167">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="6830e-168">因此，當需要使用該功能時，應為簡單型別使用 `[FromBody]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="6830e-168">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span> <span data-ttu-id="6830e-169">當動作已明確指定多個參數 (透過 `[FromBody]`) 或推斷為從要求主體繫結時，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="6830e-169">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="6830e-170">例如，下列動作簽章會造成例外狀況：</span><span class="sxs-lookup"><span data-stu-id="6830e-170">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="6830e-171">為類型 <xref:Microsoft.AspNetCore.Http.IFormFile> 與 <xref:Microsoft.AspNetCore.Http.IFormFileCollection> 的動作參數，推斷 **[FromForm]**。</span><span class="sxs-lookup"><span data-stu-id="6830e-171">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="6830e-172">而不會依據任何簡單或使用者定義的類型進行推斷。</span><span class="sxs-lookup"><span data-stu-id="6830e-172">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="6830e-173">系統會依據符合路由範本參數的任何動作參數名稱，推斷 **[FromRoute]**。</span><span class="sxs-lookup"><span data-stu-id="6830e-173">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="6830e-174">如果有多個路由符合動作參數，則會將任何路由值視為 `[FromRoute]`。</span><span class="sxs-lookup"><span data-stu-id="6830e-174">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="6830e-175">系統會依據任何其他動作參數，推斷 **[FromQuery]**。</span><span class="sxs-lookup"><span data-stu-id="6830e-175">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="6830e-176">當 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> 屬性設定為 `true` 時，會停用預設推斷規則。</span><span class="sxs-lookup"><span data-stu-id="6830e-176">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="6830e-177">在 `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);` 後，於 `Startup.ConfigureServices` 中新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="6830e-177">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=6)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="6830e-178">多部分/表單資料要求推斷</span><span class="sxs-lookup"><span data-stu-id="6830e-178">Multipart/form-data request inference</span></span>

<span data-ttu-id="6830e-179">當動作參數標註了 [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) 屬性時，會推斷 `multipart/form-data` 要求內容類型。</span><span class="sxs-lookup"><span data-stu-id="6830e-179">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="6830e-180">當 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> 屬性設定為 `true` 時，會停用預設行為。</span><span class="sxs-lookup"><span data-stu-id="6830e-180">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6830e-181">將下列程式碼加入 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="6830e-181">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="6830e-182">在 `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` 後，於 `Startup.ConfigureServices` 中新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="6830e-182">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="attribute-routing-requirement"></a><span data-ttu-id="6830e-183">屬性路由需求</span><span class="sxs-lookup"><span data-stu-id="6830e-183">Attribute routing requirement</span></span>

<span data-ttu-id="6830e-184">屬性路由已變成必要項目。</span><span class="sxs-lookup"><span data-stu-id="6830e-184">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="6830e-185">例如: </span><span class="sxs-lookup"><span data-stu-id="6830e-185">For example:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="6830e-186">無法透過 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> 中所定義的[慣例路由](xref:mvc/controllers/routing#conventional-routing)或 `Startup.Configure` 中的 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*>來存取動作。</span><span class="sxs-lookup"><span data-stu-id="6830e-186">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="problem-details-responses-for-error-status-codes"></a><span data-ttu-id="6830e-187">錯誤狀態碼的問題詳細資料回應</span><span class="sxs-lookup"><span data-stu-id="6830e-187">Problem details responses for error status codes</span></span>

<span data-ttu-id="6830e-188">在 ASP.NET Core 2.2 或更新版本中，MVC 會將錯誤結果 (具有狀態碼 400 以上 (含) 的結果) 為具有 <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> 的結果。</span><span class="sxs-lookup"><span data-stu-id="6830e-188">In ASP.NET Core 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="6830e-189">`ProblemDetails` 是：</span><span class="sxs-lookup"><span data-stu-id="6830e-189">`ProblemDetails` is:</span></span>

* <span data-ttu-id="6830e-190">以 [RFC 7807 規格](https://tools.ietf.org/html/rfc7807)為基礎的類型。</span><span class="sxs-lookup"><span data-stu-id="6830e-190">A type based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>
* <span data-ttu-id="6830e-191">標準化格式，用以在 HTTP 回應中指定電腦可讀取的錯誤詳細資料。</span><span class="sxs-lookup"><span data-stu-id="6830e-191">A standardized format for specifying machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="6830e-192">請考慮下列控制器動作中的程式碼：</span><span class="sxs-lookup"><span data-stu-id="6830e-192">Consider the following code in a controller action:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Controllers/ProductsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="6830e-193">`NotFound` 的 HTTP 回應具有 404 狀態碼，並附有 `ProblemDetails` 本文。</span><span class="sxs-lookup"><span data-stu-id="6830e-193">The HTTP response for `NotFound` has a 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="6830e-194">例如: </span><span class="sxs-lookup"><span data-stu-id="6830e-194">For example:</span></span>

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

<span data-ttu-id="6830e-195">問題詳細資料功能需要 2.2 或更新版本的相容性旗標。</span><span class="sxs-lookup"><span data-stu-id="6830e-195">The problem details feature requires a compatibility flag of 2.2 or later.</span></span> <span data-ttu-id="6830e-196">當 `SuppressMapClientErrors` 屬性設定為 `true` 時，會停用預設行為。</span><span class="sxs-lookup"><span data-stu-id="6830e-196">The default behavior is disabled when the `SuppressMapClientErrors` property is set to `true`.</span></span> <span data-ttu-id="6830e-197">將下列程式碼加入 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="6830e-197">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8)]

<span data-ttu-id="6830e-198">使用 `ClientErrorMapping` 屬性可設定 `ProblemDetails` 回應的內容。</span><span class="sxs-lookup"><span data-stu-id="6830e-198">Use the `ClientErrorMapping` property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="6830e-199">舉例來說，下列程式碼會更新 404 回應的 `type` 屬性：</span><span class="sxs-lookup"><span data-stu-id="6830e-199">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="6830e-200">其他資源</span><span class="sxs-lookup"><span data-stu-id="6830e-200">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
