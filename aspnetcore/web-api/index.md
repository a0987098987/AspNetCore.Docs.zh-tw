---
title: 使用 ASP.NET Core 建置 Web API
author: scottaddie
description: 了解可用來在 ASP.NET Core 中建置 Web API 的功能，以及每個功能的使用時機。
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: web-api/index
ms.openlocfilehash: ab672667d1ca349d80c4ca80f8d1f32f4871c7e6
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/04/2018
ms.locfileid: "36274962"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="e25fd-103">使用 ASP.NET Core 建置 Web API</span><span class="sxs-lookup"><span data-stu-id="e25fd-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="e25fd-104">作者：[Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="e25fd-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="e25fd-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e25fd-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e25fd-106">本文件說明如何在 ASP.NET Core 中建置 Web API，以及每項功能的最佳使用時機。</span><span class="sxs-lookup"><span data-stu-id="e25fd-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="e25fd-107">來自 ControllerBase 的衍生類別</span><span class="sxs-lookup"><span data-stu-id="e25fd-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="e25fd-108">繼承自用來作為 Web API 的控制器 [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) 類別。</span><span class="sxs-lookup"><span data-stu-id="e25fd-108">Inherit from the [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="e25fd-109">例如: </span><span class="sxs-lookup"><span data-stu-id="e25fd-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end

<span data-ttu-id="e25fd-110">`ControllerBase` 類別可用來存取許多屬性和方法。</span><span class="sxs-lookup"><span data-stu-id="e25fd-110">The `ControllerBase` class provides access to numerous properties and methods.</span></span> <span data-ttu-id="e25fd-111">在上述範例中，這類方法包括 [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) 和 [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction)。</span><span class="sxs-lookup"><span data-stu-id="e25fd-111">In the preceding example, some such methods include [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) and [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span></span> <span data-ttu-id="e25fd-112">您可以在動作方法內叫用這些方法，以分別傳回 HTTP 400 和 201 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="e25fd-112">These methods are invoked within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="e25fd-113">您可以存取 [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) 屬性 (亦由 `ControllerBase` 提供)，以執行要求模型驗證。</span><span class="sxs-lookup"><span data-stu-id="e25fd-113">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property, also provided by `ControllerBase`, is accessed to perform request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="e25fd-114">以 ApiControllerAttribute 標註類別</span><span class="sxs-lookup"><span data-stu-id="e25fd-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="e25fd-115">ASP.NET Core 2.1 新增了 [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) 屬性代表 Web API 控制器類別。</span><span class="sxs-lookup"><span data-stu-id="e25fd-115">ASP.NET Core 2.1 introduces the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="e25fd-116">例如: </span><span class="sxs-lookup"><span data-stu-id="e25fd-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="e25fd-117">這個屬性通常會搭配使用 `ControllerBase` 以便存取有效的方法和屬性。</span><span class="sxs-lookup"><span data-stu-id="e25fd-117">This attribute is commonly coupled with `ControllerBase` to gain access to useful methods and properties.</span></span> <span data-ttu-id="e25fd-118">`ControllerBase` 可讓您存取 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) 和 [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file) 等方法。</span><span class="sxs-lookup"><span data-stu-id="e25fd-118">`ControllerBase` provides access to methods such as [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) and [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span></span>

<span data-ttu-id="e25fd-119">另一個方法是建立標註了 `[ApiController]` 屬性的自訂基底控制器類別：</span><span class="sxs-lookup"><span data-stu-id="e25fd-119">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="e25fd-120">下列章節說明該屬性新增的便利功能。</span><span class="sxs-lookup"><span data-stu-id="e25fd-120">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="e25fd-121">HTTP 400 自動回應</span><span class="sxs-lookup"><span data-stu-id="e25fd-121">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="e25fd-122">驗證錯誤會自動觸發 HTTP 400 回應。</span><span class="sxs-lookup"><span data-stu-id="e25fd-122">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="e25fd-123">您的動作中將不再需要下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="e25fd-123">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

<span data-ttu-id="e25fd-124">您可以使用 *Startup.ConfigureServices* 中的下列程式碼來停用此預設行為：</span><span class="sxs-lookup"><span data-stu-id="e25fd-124">This default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="e25fd-125">繫結來源參數推斷</span><span class="sxs-lookup"><span data-stu-id="e25fd-125">Binding source parameter inference</span></span>

<span data-ttu-id="e25fd-126">繫結來源屬性可定義動作參數值的所在位置。</span><span class="sxs-lookup"><span data-stu-id="e25fd-126">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="e25fd-127">系統提供下列繫結來源屬性：</span><span class="sxs-lookup"><span data-stu-id="e25fd-127">The following binding source attributes exist:</span></span>

|<span data-ttu-id="e25fd-128">屬性</span><span class="sxs-lookup"><span data-stu-id="e25fd-128">Attribute</span></span>|<span data-ttu-id="e25fd-129">繫結來源</span><span class="sxs-lookup"><span data-stu-id="e25fd-129">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="e25fd-130">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span><span class="sxs-lookup"><span data-stu-id="e25fd-130">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span></span>     | <span data-ttu-id="e25fd-131">要求本文</span><span class="sxs-lookup"><span data-stu-id="e25fd-131">Request body</span></span> |
|<span data-ttu-id="e25fd-132">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span><span class="sxs-lookup"><span data-stu-id="e25fd-132">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span></span>     | <span data-ttu-id="e25fd-133">要求本文中的表單資料</span><span class="sxs-lookup"><span data-stu-id="e25fd-133">Form data in the request body</span></span> |
|<span data-ttu-id="e25fd-134">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span><span class="sxs-lookup"><span data-stu-id="e25fd-134">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span></span> | <span data-ttu-id="e25fd-135">要求標頭</span><span class="sxs-lookup"><span data-stu-id="e25fd-135">Request header</span></span> |
|<span data-ttu-id="e25fd-136">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span><span class="sxs-lookup"><span data-stu-id="e25fd-136">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span></span>   | <span data-ttu-id="e25fd-137">要求查詢字串參數</span><span class="sxs-lookup"><span data-stu-id="e25fd-137">Request query string parameter</span></span> |
|<span data-ttu-id="e25fd-138">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span><span class="sxs-lookup"><span data-stu-id="e25fd-138">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span></span>   | <span data-ttu-id="e25fd-139">來自目前要求的路由資料</span><span class="sxs-lookup"><span data-stu-id="e25fd-139">Route data from the current request</span></span> |
|<span data-ttu-id="e25fd-140">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="e25fd-140">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="e25fd-141">作為動作參數插入的要求服務</span><span class="sxs-lookup"><span data-stu-id="e25fd-141">The request service injected as an action parameter</span></span> |

> [!NOTE]
> <span data-ttu-id="e25fd-142">當值可能包含 `%2f` (也就是 `/`) 時**請勿**使用 `[FromRoute]`，因為 `%2f` 不會是未逸出的 `/`。</span><span class="sxs-lookup"><span data-stu-id="e25fd-142">Do **not** use `[FromRoute]` when values might contain `%2f` (that is `/`) because `%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="e25fd-143">如果值可能包含 `%2f`，請使用 `[FromQuery]`。</span><span class="sxs-lookup"><span data-stu-id="e25fd-143">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="e25fd-144">不含 `[ApiController]` 屬性時，即會明確定義繫結來源屬性。</span><span class="sxs-lookup"><span data-stu-id="e25fd-144">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="e25fd-145">在下列範例中，`[FromQuery]` 屬性表示 `discontinuedOnly` 參數值是在要求 URL 查詢字串中提供：</span><span class="sxs-lookup"><span data-stu-id="e25fd-145">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]

<span data-ttu-id="e25fd-146">系統會依據動作參數的預設資料來源，套用推斷規則。</span><span class="sxs-lookup"><span data-stu-id="e25fd-146">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="e25fd-147">然後，這些規則會設定繫結來源，否則就可能要由您手動將其套用至動作參數。</span><span class="sxs-lookup"><span data-stu-id="e25fd-147">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="e25fd-148">繫結來源屬性的行為如下所示：</span><span class="sxs-lookup"><span data-stu-id="e25fd-148">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="e25fd-149">系統會依據複雜類型參數推斷 **[FromBody]**。</span><span class="sxs-lookup"><span data-stu-id="e25fd-149">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="e25fd-150">如果是任何具有 [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) 和 [CancellationToken](/dotnet/api/system.threading.cancellationtoken) 等特殊意義的複雜、內建類型，則為此規則的例外。</span><span class="sxs-lookup"><span data-stu-id="e25fd-150">An exception to this rule is any complex, built-in type with a special meaning, such as [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) and [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span></span> <span data-ttu-id="e25fd-151">繫結來源推斷程式碼會忽略這些特殊的類型。</span><span class="sxs-lookup"><span data-stu-id="e25fd-151">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="e25fd-152">當動作已明確指定多個參數 (透過 `[FromBody]`) 或推斷為從要求主體繫結時，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="e25fd-152">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="e25fd-153">例如，下列動作簽章會造成例外狀況：</span><span class="sxs-lookup"><span data-stu-id="e25fd-153">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="e25fd-154">系統會依據 [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) 和 [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection) 動作參數類型，推斷 **[FromForm]**，</span><span class="sxs-lookup"><span data-stu-id="e25fd-154">**[FromForm]** is inferred for action parameters of type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span></span> <span data-ttu-id="e25fd-155">而不會依據任何簡單或使用者定義的類型進行推斷。</span><span class="sxs-lookup"><span data-stu-id="e25fd-155">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="e25fd-156">系統會依據符合路由範本參數的任何動作參數名稱，推斷 **[FromRoute]**。</span><span class="sxs-lookup"><span data-stu-id="e25fd-156">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="e25fd-157">如果有多個路由符合動作參數，則會將任何路由值視為 `[FromRoute]`。</span><span class="sxs-lookup"><span data-stu-id="e25fd-157">When multiple routes match an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="e25fd-158">系統會依據任何其他動作參數，推斷 **[FromQuery]**。</span><span class="sxs-lookup"><span data-stu-id="e25fd-158">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="e25fd-159">您可以使用 *Startup.ConfigureServices* 中的下列程式碼來停用預設推斷原則：</span><span class="sxs-lookup"><span data-stu-id="e25fd-159">The default inference rules are disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="e25fd-160">多部分/表單資料要求推斷</span><span class="sxs-lookup"><span data-stu-id="e25fd-160">Multipart/form-data request inference</span></span>

<span data-ttu-id="e25fd-161">當動作參數標註了 [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) 屬性時，會推斷 `multipart/form-data` 要求內容類型。</span><span class="sxs-lookup"><span data-stu-id="e25fd-161">When an action parameter is annotated with the [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="e25fd-162">您可以使用 *Startup.ConfigureServices* 中的下列程式碼來停用預設行為：</span><span class="sxs-lookup"><span data-stu-id="e25fd-162">The default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="e25fd-163">屬性路由需求</span><span class="sxs-lookup"><span data-stu-id="e25fd-163">Attribute routing requirement</span></span>

<span data-ttu-id="e25fd-164">屬性路由已變成必要項目。</span><span class="sxs-lookup"><span data-stu-id="e25fd-164">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="e25fd-165">例如: </span><span class="sxs-lookup"><span data-stu-id="e25fd-165">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="e25fd-166">您無法透過 [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) 中定義的[慣例路由](xref:mvc/controllers/routing#conventional-routing)或 *Startup.Configure* 中的 [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 來存取動作。</span><span class="sxs-lookup"><span data-stu-id="e25fd-166">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) or by [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*.</span></span>
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="e25fd-167">其他資源</span><span class="sxs-lookup"><span data-stu-id="e25fd-167">Additional resources</span></span>

* [<span data-ttu-id="e25fd-168">控制器動作的傳回類型</span><span class="sxs-lookup"><span data-stu-id="e25fd-168">Controller action return types</span></span>](xref:web-api/action-return-types)
* [<span data-ttu-id="e25fd-169">自訂格式器</span><span class="sxs-lookup"><span data-stu-id="e25fd-169">Custom formatters</span></span>](xref:web-api/advanced/custom-formatters)
* [<span data-ttu-id="e25fd-170">格式化回應資料</span><span class="sxs-lookup"><span data-stu-id="e25fd-170">Format response data</span></span>](xref:web-api/advanced/formatting)
* [<span data-ttu-id="e25fd-171">使用 Swagger 的說明頁面</span><span class="sxs-lookup"><span data-stu-id="e25fd-171">Help pages using Swagger</span></span>](xref:tutorials/web-api-help-pages-using-swagger)
* [<span data-ttu-id="e25fd-172">傳送至控制器動作</span><span class="sxs-lookup"><span data-stu-id="e25fd-172">Routing to controller actions</span></span>](xref:mvc/controllers/routing)
