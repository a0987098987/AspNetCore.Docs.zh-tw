---
title: 使用 ASP.NET Core 建置 Web API
author: scottaddie
description: 了解可用來在 ASP.NET Core 中建置 Web API 的功能，以及每個功能的使用時機。
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: web-api/index
ms.openlocfilehash: ab672667d1ca349d80c4ca80f8d1f32f4871c7e6
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274962"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="63b26-103">使用 ASP.NET Core 建置 Web API</span><span class="sxs-lookup"><span data-stu-id="63b26-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="63b26-104">作者：[Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="63b26-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="63b26-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="63b26-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="63b26-106">本文件說明如何在 ASP.NET Core 中建置 Web API，以及每項功能的最佳使用時機。</span><span class="sxs-lookup"><span data-stu-id="63b26-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="63b26-107">來自 ControllerBase 的衍生類別</span><span class="sxs-lookup"><span data-stu-id="63b26-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="63b26-108">繼承自用來作為 Web API 的控制器 [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) 類別。</span><span class="sxs-lookup"><span data-stu-id="63b26-108">Inherit from the [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="63b26-109">例如: </span><span class="sxs-lookup"><span data-stu-id="63b26-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="63b26-110">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="63b26-110">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="63b26-111">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="63b26-111">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]</span></span>
::: moniker-end

<span data-ttu-id="63b26-112">`ControllerBase` 類別可用來存取許多屬性和方法。</span><span class="sxs-lookup"><span data-stu-id="63b26-112">The `ControllerBase` class provides access to numerous properties and methods.</span></span> <span data-ttu-id="63b26-113">在上述範例中，這類方法包括 [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) 和 [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction)。</span><span class="sxs-lookup"><span data-stu-id="63b26-113">In the preceding example, some such methods include [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) and [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span></span> <span data-ttu-id="63b26-114">您可以在動作方法內叫用這些方法，以分別傳回 HTTP 400 和 201 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="63b26-114">These methods are invoked within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="63b26-115">您可以存取 [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) 屬性 (亦由 `ControllerBase` 提供)，以執行要求模型驗證。</span><span class="sxs-lookup"><span data-stu-id="63b26-115">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property, also provided by `ControllerBase`, is accessed to perform request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="63b26-116">以 ApiControllerAttribute 標註類別</span><span class="sxs-lookup"><span data-stu-id="63b26-116">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="63b26-117">ASP.NET Core 2.1 新增了 [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) 屬性代表 Web API 控制器類別。</span><span class="sxs-lookup"><span data-stu-id="63b26-117">ASP.NET Core 2.1 introduces the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="63b26-118">例如: </span><span class="sxs-lookup"><span data-stu-id="63b26-118">For example:</span></span>

<span data-ttu-id="63b26-119">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="63b26-119">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]</span></span>

<span data-ttu-id="63b26-120">這個屬性通常會搭配使用 `ControllerBase` 以便存取有效的方法和屬性。</span><span class="sxs-lookup"><span data-stu-id="63b26-120">This attribute is commonly coupled with `ControllerBase` to gain access to useful methods and properties.</span></span> <span data-ttu-id="63b26-121">`ControllerBase` 可讓您存取 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) 和 [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file) 等方法。</span><span class="sxs-lookup"><span data-stu-id="63b26-121">`ControllerBase` provides access to methods such as [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) and [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span></span>

<span data-ttu-id="63b26-122">另一個方法是建立標註了 `[ApiController]` 屬性的自訂基底控制器類別：</span><span class="sxs-lookup"><span data-stu-id="63b26-122">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

<span data-ttu-id="63b26-123">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]</span><span class="sxs-lookup"><span data-stu-id="63b26-123">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]</span></span>

<span data-ttu-id="63b26-124">下列章節說明該屬性新增的便利功能。</span><span class="sxs-lookup"><span data-stu-id="63b26-124">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="63b26-125">HTTP 400 自動回應</span><span class="sxs-lookup"><span data-stu-id="63b26-125">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="63b26-126">驗證錯誤會自動觸發 HTTP 400 回應。</span><span class="sxs-lookup"><span data-stu-id="63b26-126">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="63b26-127">您的動作中將不再需要下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="63b26-127">The following code becomes unnecessary in your actions:</span></span>

<span data-ttu-id="63b26-128">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]</span><span class="sxs-lookup"><span data-stu-id="63b26-128">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]</span></span>

<span data-ttu-id="63b26-129">您可以使用 *Startup.ConfigureServices* 中的下列程式碼來停用此預設行為：</span><span class="sxs-lookup"><span data-stu-id="63b26-129">This default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

<span data-ttu-id="63b26-130">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]</span><span class="sxs-lookup"><span data-stu-id="63b26-130">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]</span></span>

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="63b26-131">繫結來源參數推斷</span><span class="sxs-lookup"><span data-stu-id="63b26-131">Binding source parameter inference</span></span>

<span data-ttu-id="63b26-132">繫結來源屬性可定義動作參數值的所在位置。</span><span class="sxs-lookup"><span data-stu-id="63b26-132">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="63b26-133">系統提供下列繫結來源屬性：</span><span class="sxs-lookup"><span data-stu-id="63b26-133">The following binding source attributes exist:</span></span>

|<span data-ttu-id="63b26-134">屬性</span><span class="sxs-lookup"><span data-stu-id="63b26-134">Attribute</span></span>|<span data-ttu-id="63b26-135">繫結來源</span><span class="sxs-lookup"><span data-stu-id="63b26-135">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="63b26-136">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span><span class="sxs-lookup"><span data-stu-id="63b26-136">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span></span>     | <span data-ttu-id="63b26-137">要求本文</span><span class="sxs-lookup"><span data-stu-id="63b26-137">Request body</span></span> |
|<span data-ttu-id="63b26-138">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span><span class="sxs-lookup"><span data-stu-id="63b26-138">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span></span>     | <span data-ttu-id="63b26-139">要求本文中的表單資料</span><span class="sxs-lookup"><span data-stu-id="63b26-139">Form data in the request body</span></span> |
|<span data-ttu-id="63b26-140">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span><span class="sxs-lookup"><span data-stu-id="63b26-140">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span></span> | <span data-ttu-id="63b26-141">要求標頭</span><span class="sxs-lookup"><span data-stu-id="63b26-141">Request header</span></span> |
|<span data-ttu-id="63b26-142">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span><span class="sxs-lookup"><span data-stu-id="63b26-142">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span></span>   | <span data-ttu-id="63b26-143">要求查詢字串參數</span><span class="sxs-lookup"><span data-stu-id="63b26-143">Request query string parameter</span></span> |
|<span data-ttu-id="63b26-144">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span><span class="sxs-lookup"><span data-stu-id="63b26-144">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span></span>   | <span data-ttu-id="63b26-145">來自目前要求的路由資料</span><span class="sxs-lookup"><span data-stu-id="63b26-145">Route data from the current request</span></span> |
|<span data-ttu-id="63b26-146">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="63b26-146">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="63b26-147">作為動作參數插入的要求服務</span><span class="sxs-lookup"><span data-stu-id="63b26-147">The request service injected as an action parameter</span></span> |

> [!NOTE]
> <span data-ttu-id="63b26-148">當值可能包含 `%2f` (也就是 `/`) 時**請勿**使用 `[FromRoute]`，因為 `%2f` 不會是未逸出的 `/`。</span><span class="sxs-lookup"><span data-stu-id="63b26-148">Do **not** use `[FromRoute]` when values might contain `%2f` (that is `/`) because `%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="63b26-149">如果值可能包含 `%2f`，請使用 `[FromQuery]`。</span><span class="sxs-lookup"><span data-stu-id="63b26-149">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="63b26-150">不含 `[ApiController]` 屬性時，即會明確定義繫結來源屬性。</span><span class="sxs-lookup"><span data-stu-id="63b26-150">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="63b26-151">在下列範例中，`[FromQuery]` 屬性表示 `discontinuedOnly` 參數值是在要求 URL 查詢字串中提供：</span><span class="sxs-lookup"><span data-stu-id="63b26-151">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

<span data-ttu-id="63b26-152">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="63b26-152">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]</span></span>

<span data-ttu-id="63b26-153">系統會依據動作參數的預設資料來源，套用推斷規則。</span><span class="sxs-lookup"><span data-stu-id="63b26-153">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="63b26-154">然後，這些規則會設定繫結來源，否則就可能要由您手動將其套用至動作參數。</span><span class="sxs-lookup"><span data-stu-id="63b26-154">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="63b26-155">繫結來源屬性的行為如下所示：</span><span class="sxs-lookup"><span data-stu-id="63b26-155">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="63b26-156">系統會依據複雜類型參數推斷 **[FromBody]**。</span><span class="sxs-lookup"><span data-stu-id="63b26-156">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="63b26-157">如果是任何具有 [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) 和 [CancellationToken](/dotnet/api/system.threading.cancellationtoken) 等特殊意義的複雜、內建類型，則為此規則的例外。</span><span class="sxs-lookup"><span data-stu-id="63b26-157">An exception to this rule is any complex, built-in type with a special meaning, such as [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) and [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span></span> <span data-ttu-id="63b26-158">繫結來源推斷程式碼會忽略這些特殊的類型。</span><span class="sxs-lookup"><span data-stu-id="63b26-158">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="63b26-159">當動作已明確指定多個參數 (透過 `[FromBody]`) 或推斷為從要求主體繫結時，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="63b26-159">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="63b26-160">例如，下列動作簽章會造成例外狀況：</span><span class="sxs-lookup"><span data-stu-id="63b26-160">For example, the following action signatures cause an exception:</span></span>

<span data-ttu-id="63b26-161">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]</span><span class="sxs-lookup"><span data-stu-id="63b26-161">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]</span></span>

* <span data-ttu-id="63b26-162">系統會依據 [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) 和 [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection) 動作參數類型，推斷 **[FromForm]**，</span><span class="sxs-lookup"><span data-stu-id="63b26-162">**[FromForm]** is inferred for action parameters of type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span></span> <span data-ttu-id="63b26-163">而不會依據任何簡單或使用者定義的類型進行推斷。</span><span class="sxs-lookup"><span data-stu-id="63b26-163">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="63b26-164">系統會依據符合路由範本參數的任何動作參數名稱，推斷 **[FromRoute]**。</span><span class="sxs-lookup"><span data-stu-id="63b26-164">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="63b26-165">如果有多個路由符合動作參數，則會將任何路由值視為 `[FromRoute]`。</span><span class="sxs-lookup"><span data-stu-id="63b26-165">When multiple routes match an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="63b26-166">系統會依據任何其他動作參數，推斷 **[FromQuery]**。</span><span class="sxs-lookup"><span data-stu-id="63b26-166">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="63b26-167">您可以使用 *Startup.ConfigureServices* 中的下列程式碼來停用預設推斷原則：</span><span class="sxs-lookup"><span data-stu-id="63b26-167">The default inference rules are disabled with the following code in *Startup.ConfigureServices*:</span></span>

<span data-ttu-id="63b26-168">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]</span><span class="sxs-lookup"><span data-stu-id="63b26-168">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]</span></span>

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="63b26-169">多部分/表單資料要求推斷</span><span class="sxs-lookup"><span data-stu-id="63b26-169">Multipart/form-data request inference</span></span>

<span data-ttu-id="63b26-170">當動作參數標註了 [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) 屬性時，會推斷 `multipart/form-data` 要求內容類型。</span><span class="sxs-lookup"><span data-stu-id="63b26-170">When an action parameter is annotated with the [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="63b26-171">您可以使用 *Startup.ConfigureServices* 中的下列程式碼來停用預設行為：</span><span class="sxs-lookup"><span data-stu-id="63b26-171">The default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

<span data-ttu-id="63b26-172">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="63b26-172">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]</span></span>

### <a name="attribute-routing-requirement"></a><span data-ttu-id="63b26-173">屬性路由需求</span><span class="sxs-lookup"><span data-stu-id="63b26-173">Attribute routing requirement</span></span>

<span data-ttu-id="63b26-174">屬性路由已變成必要項目。</span><span class="sxs-lookup"><span data-stu-id="63b26-174">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="63b26-175">例如: </span><span class="sxs-lookup"><span data-stu-id="63b26-175">For example:</span></span>

<span data-ttu-id="63b26-176">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="63b26-176">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]</span></span>

<span data-ttu-id="63b26-177">您無法透過 [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) 中定義的[慣例路由](xref:mvc/controllers/routing#conventional-routing)或 *Startup.Configure* 中的 [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 來存取動作。</span><span class="sxs-lookup"><span data-stu-id="63b26-177">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) or by [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*.</span></span>
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="63b26-178">其他資源</span><span class="sxs-lookup"><span data-stu-id="63b26-178">Additional resources</span></span>

* [<span data-ttu-id="63b26-179">控制器動作的傳回類型</span><span class="sxs-lookup"><span data-stu-id="63b26-179">Controller action return types</span></span>](xref:web-api/action-return-types)
* [<span data-ttu-id="63b26-180">自訂格式器</span><span class="sxs-lookup"><span data-stu-id="63b26-180">Custom formatters</span></span>](xref:web-api/advanced/custom-formatters)
* [<span data-ttu-id="63b26-181">格式化回應資料</span><span class="sxs-lookup"><span data-stu-id="63b26-181">Format response data</span></span>](xref:web-api/advanced/formatting)
* [<span data-ttu-id="63b26-182">使用 Swagger 的說明頁面</span><span class="sxs-lookup"><span data-stu-id="63b26-182">Help pages using Swagger</span></span>](xref:tutorials/web-api-help-pages-using-swagger)
* [<span data-ttu-id="63b26-183">傳送至控制器動作</span><span class="sxs-lookup"><span data-stu-id="63b26-183">Routing to controller actions</span></span>](xref:mvc/controllers/routing)
