---
title: 使用 ASP.NET Core 建置 Web API
author: scottaddie
description: 了解可用來在 ASP.NET Core 中建置 Web API 的功能，以及每個功能的使用時機。
ms.author: scaddie
ms.custom: mvc
ms.date: 01/11/2019
uid: web-api/index
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="caf66-103">使用 ASP.NET Core 建置 Web API</span><span class="sxs-lookup"><span data-stu-id="caf66-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="caf66-104">作者：[Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="caf66-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="caf66-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="caf66-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="caf66-106">本文件說明如何在 ASP.NET Core 中建置 Web API，以及每項功能的最佳使用時機。</span><span class="sxs-lookup"><span data-stu-id="caf66-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="caf66-107">來自 ControllerBase 的衍生類別</span><span class="sxs-lookup"><span data-stu-id="caf66-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="caf66-108">繼承自用作為 Web API 的控制器中之 <xref:Microsoft.AspNetCore.Mvc.ControllerBase> 類別。</span><span class="sxs-lookup"><span data-stu-id="caf66-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="caf66-109">例如：</span><span class="sxs-lookup"><span data-stu-id="caf66-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="caf66-110">`ControllerBase` 類別可用來存取數個屬性和方法。</span><span class="sxs-lookup"><span data-stu-id="caf66-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="caf66-111">在上述程式碼中，範例包含 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> 與 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>。</span><span class="sxs-lookup"><span data-stu-id="caf66-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="caf66-112">您可以在動作方法內呼叫這些方法，以分別傳回 HTTP 400 和 201 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="caf66-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="caf66-113">可以存取 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> 屬性 (亦由 `ControllerBase` 提供)，以處理要求模型驗證。</span><span class="sxs-lookup"><span data-stu-id="caf66-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotation-with-apicontroller-attribute"></a><span data-ttu-id="caf66-114">使用 ApiController 屬性的註釋</span><span class="sxs-lookup"><span data-stu-id="caf66-114">Annotation with ApiController attribute</span></span>

<span data-ttu-id="caf66-115">ASP.NET Core 2.1 新增了 [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) 屬性代表 Web API 控制器類別。</span><span class="sxs-lookup"><span data-stu-id="caf66-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="caf66-116">例如：</span><span class="sxs-lookup"><span data-stu-id="caf66-116">For example:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="caf66-117">透過 <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> 所設定的 2.1 或更新相容性版本必須在控制器層級使用此屬性。</span><span class="sxs-lookup"><span data-stu-id="caf66-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute at the controller level.</span></span> <span data-ttu-id="caf66-118">舉例來說，`Startup.ConfigureServices` 中的醒目提示程式碼會設定 2.1 相容性旗標：</span><span class="sxs-lookup"><span data-stu-id="caf66-118">For example, the highlighted code in `Startup.ConfigureServices` sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="caf66-119">如需詳細資訊，請參閱<xref:mvc/compatibility-version>。</span><span class="sxs-lookup"><span data-stu-id="caf66-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="caf66-120">在 ASP.NET Core 2.2 或更新版本中，`[ApiController]` 屬性可以套用至組件。</span><span class="sxs-lookup"><span data-stu-id="caf66-120">In ASP.NET Core 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="caf66-121">以這種方式標註會將 Web API 行為套用至組件中的所有控制器。</span><span class="sxs-lookup"><span data-stu-id="caf66-121">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="caf66-122">請注意，沒有任何方法可以退出個別控制器。</span><span class="sxs-lookup"><span data-stu-id="caf66-122">Beware that there's no way to opt out for individual controllers.</span></span> <span data-ttu-id="caf66-123">建議您，組件層級屬性應套用至 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="caf66-123">As a recommendation, assembly-level attributes should be applied to the `Startup` class:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ApiControllerAttributeOnAssembly&highlight=1)]

<span data-ttu-id="caf66-124">透過 <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> 所設定的 2.2 或更新相容性版本必須在組件層級使用此屬性。</span><span class="sxs-lookup"><span data-stu-id="caf66-124">A compatibility version of 2.2 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute at the assembly level.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="caf66-125">`[ApiController]` 屬性通常會與 `ControllerBase` 搭配使用，為控制站啟用 REST 特定行為。</span><span class="sxs-lookup"><span data-stu-id="caf66-125">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="caf66-126">`ControllerBase` 可讓您存取 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> 與 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*> 等方法。</span><span class="sxs-lookup"><span data-stu-id="caf66-126">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="caf66-127">另一個方法是建立標註了 `[ApiController]` 屬性的自訂基底控制器類別：</span><span class="sxs-lookup"><span data-stu-id="caf66-127">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="caf66-128">下列章節說明該屬性新增的便利功能。</span><span class="sxs-lookup"><span data-stu-id="caf66-128">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="caf66-129">HTTP 400 自動回應</span><span class="sxs-lookup"><span data-stu-id="caf66-129">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="caf66-130">模型驗證錯誤會自動觸發 HTTP 400 回應。</span><span class="sxs-lookup"><span data-stu-id="caf66-130">Model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="caf66-131">因此，您的動作將不再需要下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="caf66-131">Consequently, the following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="caf66-132">使用 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> 可自訂所產生回應的輸出。</span><span class="sxs-lookup"><span data-stu-id="caf66-132">Use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to customize the output of the resulting response.</span></span>

<span data-ttu-id="caf66-133">當您的動作可以從模型驗證錯誤復原時，停用預設行為將會有所幫助。</span><span class="sxs-lookup"><span data-stu-id="caf66-133">Disabling the default behavior is useful when your action can recover from a model validation error.</span></span> <span data-ttu-id="caf66-134">當 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> 屬性設定為 `true` 時，會停用預設行為。</span><span class="sxs-lookup"><span data-stu-id="caf66-134">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="caf66-135">在 `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);` 後，於 `Startup.ConfigureServices` 中新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="caf66-135">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=7)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="caf66-136">使用 2.2 或更新版本的相容性旗標，HTTP 400 回應的預設回應類型為 <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>。</span><span class="sxs-lookup"><span data-stu-id="caf66-136">With a compatibility flag of 2.2 or later, the default response type for HTTP 400 responses is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="caf66-137">`ValidationProblemDetails` 類型符合 [RFC 7807 規格](https://tools.ietf.org/html/rfc7807)。</span><span class="sxs-lookup"><span data-stu-id="caf66-137">The `ValidationProblemDetails` type complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span> <span data-ttu-id="caf66-138">將 `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` 屬性設定為 `true`，以改為傳回 <xref:Microsoft.AspNetCore.Mvc.SerializableError> 的 ASP.NET Core 2.1 錯誤格式。</span><span class="sxs-lookup"><span data-stu-id="caf66-138">Set the `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` property to `true` to instead return the ASP.NET Core 2.1 error format of <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span></span> <span data-ttu-id="caf66-139">將下列程式碼加入 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="caf66-139">Add the following code in `Startup.ConfigureServices`:</span></span>

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

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="caf66-140">繫結來源參數推斷</span><span class="sxs-lookup"><span data-stu-id="caf66-140">Binding source parameter inference</span></span>

<span data-ttu-id="caf66-141">繫結來源屬性可定義動作參數值的所在位置。</span><span class="sxs-lookup"><span data-stu-id="caf66-141">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="caf66-142">系統提供下列繫結來源屬性：</span><span class="sxs-lookup"><span data-stu-id="caf66-142">The following binding source attributes exist:</span></span>

|<span data-ttu-id="caf66-143">屬性</span><span class="sxs-lookup"><span data-stu-id="caf66-143">Attribute</span></span>|<span data-ttu-id="caf66-144">繫結來源</span><span class="sxs-lookup"><span data-stu-id="caf66-144">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="caf66-145">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="caf66-145">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="caf66-146">要求本文</span><span class="sxs-lookup"><span data-stu-id="caf66-146">Request body</span></span> |
|<span data-ttu-id="caf66-147">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="caf66-147">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="caf66-148">要求本文中的表單資料</span><span class="sxs-lookup"><span data-stu-id="caf66-148">Form data in the request body</span></span> |
|<span data-ttu-id="caf66-149">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="caf66-149">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="caf66-150">要求標頭</span><span class="sxs-lookup"><span data-stu-id="caf66-150">Request header</span></span> |
|<span data-ttu-id="caf66-151">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="caf66-151">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="caf66-152">要求查詢字串參數</span><span class="sxs-lookup"><span data-stu-id="caf66-152">Request query string parameter</span></span> |
|<span data-ttu-id="caf66-153">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="caf66-153">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="caf66-154">來自目前要求的路由資料</span><span class="sxs-lookup"><span data-stu-id="caf66-154">Route data from the current request</span></span> |
|<span data-ttu-id="caf66-155">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="caf66-155">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="caf66-156">作為動作參數插入的要求服務</span><span class="sxs-lookup"><span data-stu-id="caf66-156">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="caf66-157">當值可能包含 `%2f` (也就是 `/`) 時，請勿使用 `[FromRoute]`。</span><span class="sxs-lookup"><span data-stu-id="caf66-157">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="caf66-158">`%2f` 不會是未逸出的 `/`。</span><span class="sxs-lookup"><span data-stu-id="caf66-158">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="caf66-159">如果值可能包含 `%2f`，請使用 `[FromQuery]`。</span><span class="sxs-lookup"><span data-stu-id="caf66-159">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="caf66-160">不含 `[ApiController]` 屬性時，即會明確定義繫結來源屬性。</span><span class="sxs-lookup"><span data-stu-id="caf66-160">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="caf66-161">若沒有 `[ApiController]` 或其他繫結來源屬性 (例如 `[FromQuery]`)，ASP.NET Core 執行階段會嘗試使用複雜物件模型繫結器。</span><span class="sxs-lookup"><span data-stu-id="caf66-161">Without `[ApiController]` or other binding source attributes like `[FromQuery]`, the ASP.NET Core runtime attempts to use the complex object model binder.</span></span> <span data-ttu-id="caf66-162">複雜物件模型繫結器會從值提供者 (具有已定義的順序) 提取資料。</span><span class="sxs-lookup"><span data-stu-id="caf66-162">The complex object model binder pulls data from value providers (which have a defined order).</span></span> <span data-ttu-id="caf66-163">例如，「主體模型繫結器」一律是選擇加入的。</span><span class="sxs-lookup"><span data-stu-id="caf66-163">For instance, the 'body model binder' is always opt in.</span></span>

<span data-ttu-id="caf66-164">在下列範例中，`[FromQuery]` 屬性表示 `discontinuedOnly` 參數值是在要求 URL 查詢字串中提供：</span><span class="sxs-lookup"><span data-stu-id="caf66-164">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="caf66-165">系統會依據動作參數的預設資料來源，套用推斷規則。</span><span class="sxs-lookup"><span data-stu-id="caf66-165">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="caf66-166">然後，這些規則會設定繫結來源，否則就可能要由您手動將其套用至動作參數。</span><span class="sxs-lookup"><span data-stu-id="caf66-166">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="caf66-167">繫結來源屬性的行為如下所示：</span><span class="sxs-lookup"><span data-stu-id="caf66-167">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="caf66-168">系統會依據複雜類型參數推斷 **[FromBody]**。</span><span class="sxs-lookup"><span data-stu-id="caf66-168">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="caf66-169">如果是任何具有像是 <xref:Microsoft.AspNetCore.Http.IFormCollection> 與 <xref:System.Threading.CancellationToken> 等特殊意義的複雜內建類型，則為此規則的例外。</span><span class="sxs-lookup"><span data-stu-id="caf66-169">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="caf66-170">繫結來源推斷程式碼會忽略這些特殊的類型。</span><span class="sxs-lookup"><span data-stu-id="caf66-170">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="caf66-171">並不會為像是 `string` 或 `int` 等簡單型別，推斷 `[FromBody]`。</span><span class="sxs-lookup"><span data-stu-id="caf66-171">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="caf66-172">因此，當需要使用該功能時，應為簡單型別使用 `[FromBody]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="caf66-172">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span> <span data-ttu-id="caf66-173">當動作已明確指定多個參數 (透過 `[FromBody]`) 或推斷為從要求主體繫結時，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="caf66-173">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="caf66-174">例如，下列動作簽章會造成例外狀況：</span><span class="sxs-lookup"><span data-stu-id="caf66-174">For example, the following action signatures cause an exception:</span></span>

    [!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

    > [!NOTE]
    > <span data-ttu-id="caf66-175">在 ASP.NET Core 2.1 中，集合類型參數 (例如清單與陣列) 不正確地推斷為 [[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)。</span><span class="sxs-lookup"><span data-stu-id="caf66-175">In ASP.NET Core 2.1, collection type parameters such as lists and arrays are incorrectly inferred as [[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute).</span></span> <span data-ttu-id="caf66-176">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) 應該用於這些參數 (若它們將從要求本文繫結)。</span><span class="sxs-lookup"><span data-stu-id="caf66-176">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) should be used for these parameters if they are to be bound from the request body.</span></span> <span data-ttu-id="caf66-177">此行為在 ASP.NET Core 2.2 或更新版本中已修復，其中集合類型參數預設推斷為從本文繫結。</span><span class="sxs-lookup"><span data-stu-id="caf66-177">This behavior is fixed in ASP.NET Core 2.2 or later, where collection type parameters are inferred to be bound from the body by default.</span></span>

* <span data-ttu-id="caf66-178">為類型 <xref:Microsoft.AspNetCore.Http.IFormFile> 與 <xref:Microsoft.AspNetCore.Http.IFormFileCollection> 的動作參數，推斷 **[FromForm]**。</span><span class="sxs-lookup"><span data-stu-id="caf66-178">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="caf66-179">而不會依據任何簡單或使用者定義的類型進行推斷。</span><span class="sxs-lookup"><span data-stu-id="caf66-179">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="caf66-180">系統會依據符合路由範本參數的任何動作參數名稱，推斷 **[FromRoute]**。</span><span class="sxs-lookup"><span data-stu-id="caf66-180">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="caf66-181">如果有多個路由符合動作參數，則會將任何路由值視為 `[FromRoute]`。</span><span class="sxs-lookup"><span data-stu-id="caf66-181">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="caf66-182">系統會依據任何其他動作參數，推斷 **[FromQuery]**。</span><span class="sxs-lookup"><span data-stu-id="caf66-182">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="caf66-183">當 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> 屬性設定為 `true` 時，會停用預設推斷規則。</span><span class="sxs-lookup"><span data-stu-id="caf66-183">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="caf66-184">在 `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);` 後，於 `Startup.ConfigureServices` 中新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="caf66-184">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=6)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="caf66-185">多部分/表單資料要求推斷</span><span class="sxs-lookup"><span data-stu-id="caf66-185">Multipart/form-data request inference</span></span>

<span data-ttu-id="caf66-186">當動作參數標註了 [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) 屬性時，會推斷 `multipart/form-data` 要求內容類型。</span><span class="sxs-lookup"><span data-stu-id="caf66-186">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="caf66-187">當 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> 屬性設定為 `true` 時，會停用預設行為。</span><span class="sxs-lookup"><span data-stu-id="caf66-187">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="caf66-188">將下列程式碼加入 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="caf66-188">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="caf66-189">在 `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` 後，於 `Startup.ConfigureServices` 中新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="caf66-189">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="attribute-routing-requirement"></a><span data-ttu-id="caf66-190">屬性路由需求</span><span class="sxs-lookup"><span data-stu-id="caf66-190">Attribute routing requirement</span></span>

<span data-ttu-id="caf66-191">屬性路由已變成必要項目。</span><span class="sxs-lookup"><span data-stu-id="caf66-191">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="caf66-192">例如：</span><span class="sxs-lookup"><span data-stu-id="caf66-192">For example:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="caf66-193">無法透過 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> 中所定義的[慣例路由](xref:mvc/controllers/routing#conventional-routing)或 `Startup.Configure` 中的 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*>來存取動作。</span><span class="sxs-lookup"><span data-stu-id="caf66-193">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="problem-details-responses-for-error-status-codes"></a><span data-ttu-id="caf66-194">錯誤狀態碼的問題詳細資料回應</span><span class="sxs-lookup"><span data-stu-id="caf66-194">Problem details responses for error status codes</span></span>

<span data-ttu-id="caf66-195">在 ASP.NET Core 2.2 或更新版本中，MVC 會將錯誤結果 (具有狀態碼 400 以上 (含) 的結果) 為具有 <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> 的結果。</span><span class="sxs-lookup"><span data-stu-id="caf66-195">In ASP.NET Core 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="caf66-196">`ProblemDetails` 是：</span><span class="sxs-lookup"><span data-stu-id="caf66-196">`ProblemDetails` is:</span></span>

* <span data-ttu-id="caf66-197">以 [RFC 7807 規格](https://tools.ietf.org/html/rfc7807)為基礎的類型。</span><span class="sxs-lookup"><span data-stu-id="caf66-197">A type based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>
* <span data-ttu-id="caf66-198">標準化格式，用以在 HTTP 回應中指定電腦可讀取的錯誤詳細資料。</span><span class="sxs-lookup"><span data-stu-id="caf66-198">A standardized format for specifying machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="caf66-199">請考慮下列控制器動作中的程式碼：</span><span class="sxs-lookup"><span data-stu-id="caf66-199">Consider the following code in a controller action:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Controllers/ProductsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="caf66-200">`NotFound` 的 HTTP 回應具有 404 狀態碼，並附有 `ProblemDetails` 本文。</span><span class="sxs-lookup"><span data-stu-id="caf66-200">The HTTP response for `NotFound` has a 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="caf66-201">例如：</span><span class="sxs-lookup"><span data-stu-id="caf66-201">For example:</span></span>

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

<span data-ttu-id="caf66-202">問題詳細資料功能需要 2.2 或更新版本的相容性旗標。</span><span class="sxs-lookup"><span data-stu-id="caf66-202">The problem details feature requires a compatibility flag of 2.2 or later.</span></span> <span data-ttu-id="caf66-203">當 `SuppressMapClientErrors` 屬性設定為 `true` 時，會停用預設行為。</span><span class="sxs-lookup"><span data-stu-id="caf66-203">The default behavior is disabled when the `SuppressMapClientErrors` property is set to `true`.</span></span> <span data-ttu-id="caf66-204">將下列程式碼加入 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="caf66-204">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8)]

<span data-ttu-id="caf66-205">使用 `ClientErrorMapping` 屬性可設定 `ProblemDetails` 回應的內容。</span><span class="sxs-lookup"><span data-stu-id="caf66-205">Use the `ClientErrorMapping` property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="caf66-206">舉例來說，下列程式碼會更新 404 回應的 `type` 屬性：</span><span class="sxs-lookup"><span data-stu-id="caf66-206">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="caf66-207">其他資源</span><span class="sxs-lookup"><span data-stu-id="caf66-207">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
