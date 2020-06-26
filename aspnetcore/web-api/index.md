---
title: 使用 ASP.NET Core 建立 Web API
author: scottaddie
description: 了解使用 ASP.NET Core 建立 Web API 的基本概念。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/02/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: web-api/index
ms.openlocfilehash: 7c9762d23ff612155846357bfadeaaad492c7299
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85404726"
---
# <a name="create-web-apis-with-aspnet-core"></a><span data-ttu-id="bd4b3-103">使用 ASP.NET Core 建立 Web API</span><span class="sxs-lookup"><span data-stu-id="bd4b3-103">Create web APIs with ASP.NET Core</span></span>

<span data-ttu-id="bd4b3-104">作者：[Scott Addie](https://github.com/scottaddie) 與 [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="bd4b3-104">By [Scott Addie](https://github.com/scottaddie) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="bd4b3-105">ASP.NET Core 支援使用 C# 建立 RESTful 服務，也稱為 Web API。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-105">ASP.NET Core supports creating RESTful services, also known as web APIs, using C#.</span></span> <span data-ttu-id="bd4b3-106">若要處理要求，Web API 會使用控制器。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-106">To handle requests, a web API uses controllers.</span></span> <span data-ttu-id="bd4b3-107">Web API 中的「控制器」\*\* 都衍生自類別 `ControllerBase`。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-107">*Controllers* in a web API are classes that derive from `ControllerBase`.</span></span> <span data-ttu-id="bd4b3-108">本文說明如何使用控制器來處理 Web API 要求。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-108">This article shows how to use controllers for handling web API requests.</span></span>

<span data-ttu-id="bd4b3-109">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples)。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-109">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span></span> <span data-ttu-id="bd4b3-110">([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-110">([How to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="controllerbase-class"></a><span data-ttu-id="bd4b3-111">ControllerBase 類別</span><span class="sxs-lookup"><span data-stu-id="bd4b3-111">ControllerBase class</span></span>

<span data-ttu-id="bd4b3-112">Web API 由一個或多個衍生自的控制器類別所組成 <xref:Microsoft.AspNetCore.Mvc.ControllerBase> 。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-112">A web API consists of one or more controller classes that derive from <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="bd4b3-113">Web API 專案範本提供入門控制器：</span><span class="sxs-lookup"><span data-stu-id="bd4b3-113">The web API project template provides a starter controller:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

<span data-ttu-id="bd4b3-114">請不要從 <xref:Microsoft.AspNetCore.Mvc.Controller> 類別衍生以建立 Web API 控制器。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-114">Don't create a web API controller by deriving from the <xref:Microsoft.AspNetCore.Mvc.Controller> class.</span></span> <span data-ttu-id="bd4b3-115">`Controller` 衍生自 `ControllerBase` 並會新增檢視支援，以供處理網頁，而不是 Web API 要求。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-115">`Controller` derives from `ControllerBase` and adds support for views, so it's for handling web pages, not web API requests.</span></span> <span data-ttu-id="bd4b3-116">此規則有例外狀況：如果您打算針對 views 和 web Api 使用相同的控制器，請從衍生 `Controller` 。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-116">There's an exception to this rule: if you plan to use the same controller for both views and web APIs, derive it from `Controller`.</span></span>

<span data-ttu-id="bd4b3-117">`ControllerBase` 類別提供許多處理 HTTP 要求的實用屬性和方法。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-117">The `ControllerBase` class provides many properties and methods that are useful for handling HTTP requests.</span></span> <span data-ttu-id="bd4b3-118">例如，`ControllerBase.CreatedAtAction` 會傳回 201 狀態碼：</span><span class="sxs-lookup"><span data-stu-id="bd4b3-118">For example, `ControllerBase.CreatedAtAction` returns a 201 status code:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_400And201&highlight=10)]

<span data-ttu-id="bd4b3-119">以下是 `ControllerBase` 提供的一些其他方法範例。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-119">Here are some more examples of methods that `ControllerBase` provides.</span></span>

|<span data-ttu-id="bd4b3-120">方法</span><span class="sxs-lookup"><span data-stu-id="bd4b3-120">Method</span></span>   |<span data-ttu-id="bd4b3-121">注意</span><span class="sxs-lookup"><span data-stu-id="bd4b3-121">Notes</span></span>    |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest%2A>| <span data-ttu-id="bd4b3-122">傳回 400 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-122">Returns 400 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound%2A>|<span data-ttu-id="bd4b3-123">傳回 404 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-123">Returns 404 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile%2A>|<span data-ttu-id="bd4b3-124">傳回檔案。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-124">Returns a file.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync%2A>|<span data-ttu-id="bd4b3-125">叫用[模型繫結](xref:mvc/models/model-binding)。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-125">Invokes [model binding](xref:mvc/models/model-binding).</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel%2A>|<span data-ttu-id="bd4b3-126">叫用[模型驗證](xref:mvc/models/validation)。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-126">Invokes [model validation](xref:mvc/models/validation).</span></span>|

<span data-ttu-id="bd4b3-127">如需所有可用方法和屬性的清單，請參閱 <xref:Microsoft.AspNetCore.Mvc.ControllerBase>。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-127">For a list of all available methods and properties, see <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span>

## <a name="attributes"></a><span data-ttu-id="bd4b3-128">屬性</span><span class="sxs-lookup"><span data-stu-id="bd4b3-128">Attributes</span></span>

<span data-ttu-id="bd4b3-129"><xref:Microsoft.AspNetCore.Mvc> 命名空間提供的屬性，可用來設定 Web API 控制器和動作方法的行為。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-129">The <xref:Microsoft.AspNetCore.Mvc> namespace provides attributes that can be used to configure the behavior of web API controllers and action methods.</span></span> <span data-ttu-id="bd4b3-130">下列範例會使用屬性來指定支援的 HTTP 動作動詞，以及任何可能傳回的已知 HTTP 狀態碼：</span><span class="sxs-lookup"><span data-stu-id="bd4b3-130">The following example uses attributes to specify the supported HTTP action verb and any known HTTP status codes that could be returned:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

<span data-ttu-id="bd4b3-131">以下是一些其他可用的屬性範例。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-131">Here are some more examples of attributes that are available.</span></span>

|<span data-ttu-id="bd4b3-132">屬性</span><span class="sxs-lookup"><span data-stu-id="bd4b3-132">Attribute</span></span>|<span data-ttu-id="bd4b3-133">注意</span><span class="sxs-lookup"><span data-stu-id="bd4b3-133">Notes</span></span>|
|---------|-----|
|[`[Route]`](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)      |<span data-ttu-id="bd4b3-134">指定控制器或動作的 URL 模式。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-134">Specifies URL pattern for a controller or action.</span></span>|
|[`[Bind]`](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)        |<span data-ttu-id="bd4b3-135">指定模型繫結要包含的前置詞和屬性。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-135">Specifies prefix and properties to include for model binding.</span></span>|
|[`[HttpGet]`](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)  |<span data-ttu-id="bd4b3-136">識別支援 HTTP GET 動作動詞的動作。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-136">Identifies an action that supports the HTTP GET action verb.</span></span>|
|[`[Consumes]`](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)|<span data-ttu-id="bd4b3-137">指定動作所接受的資料類型。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-137">Specifies data types that an action accepts.</span></span>|
|[`[Produces]`](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)|<span data-ttu-id="bd4b3-138">指定動作所傳回的資料類型。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-138">Specifies data types that an action returns.</span></span>|

<span data-ttu-id="bd4b3-139">如需包含可用屬性的清單，請參閱 <xref:Microsoft.AspNetCore.Mvc> 命名空間。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-139">For a list that includes the available attributes, see the <xref:Microsoft.AspNetCore.Mvc> namespace.</span></span>

## <a name="apicontroller-attribute"></a><span data-ttu-id="bd4b3-140">ApiController 屬性</span><span class="sxs-lookup"><span data-stu-id="bd4b3-140">ApiController attribute</span></span>

<span data-ttu-id="bd4b3-141">[`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute)屬性可以套用至控制器類別，以啟用下列固定的 API 特定行為：</span><span class="sxs-lookup"><span data-stu-id="bd4b3-141">The [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute can be applied to a controller class to enable the following opinionated, API-specific behaviors:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="bd4b3-142">屬性路由需求</span><span class="sxs-lookup"><span data-stu-id="bd4b3-142">Attribute routing requirement</span></span>](#attribute-routing-requirement)
* [<span data-ttu-id="bd4b3-143">HTTP 400 自動回應</span><span class="sxs-lookup"><span data-stu-id="bd4b3-143">Automatic HTTP 400 responses</span></span>](#automatic-http-400-responses)
* [<span data-ttu-id="bd4b3-144">繫結來源參數推斷</span><span class="sxs-lookup"><span data-stu-id="bd4b3-144">Binding source parameter inference</span></span>](#binding-source-parameter-inference)
* [<span data-ttu-id="bd4b3-145">多部分/表單資料要求推斷</span><span class="sxs-lookup"><span data-stu-id="bd4b3-145">Multipart/form-data request inference</span></span>](#multipartform-data-request-inference)
* [<span data-ttu-id="bd4b3-146">錯誤狀態碼的問題詳細資料</span><span class="sxs-lookup"><span data-stu-id="bd4b3-146">Problem details for error status codes</span></span>](#problem-details-for-error-status-codes)

<span data-ttu-id="bd4b3-147">*錯誤狀態碼功能的問題詳細資料*需要2.2 或更新版本的[相容性版本](xref:mvc/compatibility-version)。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-147">The *Problem details for error status codes* feature requires a [compatibility version](xref:mvc/compatibility-version) of 2.2 or later.</span></span> <span data-ttu-id="bd4b3-148">其他功能需要2.1 或更新版本的相容性版本。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-148">The other features require a compatibility version of 2.1 or later.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

* [<span data-ttu-id="bd4b3-149">屬性路由需求</span><span class="sxs-lookup"><span data-stu-id="bd4b3-149">Attribute routing requirement</span></span>](#attribute-routing-requirement)
* [<span data-ttu-id="bd4b3-150">HTTP 400 自動回應</span><span class="sxs-lookup"><span data-stu-id="bd4b3-150">Automatic HTTP 400 responses</span></span>](#automatic-http-400-responses)
* [<span data-ttu-id="bd4b3-151">繫結來源參數推斷</span><span class="sxs-lookup"><span data-stu-id="bd4b3-151">Binding source parameter inference</span></span>](#binding-source-parameter-inference)
* [<span data-ttu-id="bd4b3-152">多部分/表單資料要求推斷</span><span class="sxs-lookup"><span data-stu-id="bd4b3-152">Multipart/form-data request inference</span></span>](#multipartform-data-request-inference)

<span data-ttu-id="bd4b3-153">這些功能都需要[相容性版本](xref:mvc/compatibility-version) 2.1 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-153">These features require a [compatibility version](xref:mvc/compatibility-version) of 2.1 or later.</span></span>

::: moniker-end

### <a name="attribute-on-specific-controllers"></a><span data-ttu-id="bd4b3-154">特定控制器上的屬性</span><span class="sxs-lookup"><span data-stu-id="bd4b3-154">Attribute on specific controllers</span></span>

<span data-ttu-id="bd4b3-155">`[ApiController]` 屬性可以套用至特定的控制器，如專案範本中的下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="bd4b3-155">The `[ApiController]` attribute can be applied to specific controllers, as in the following example from the project template:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2])]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=2)]

::: moniker-end

### <a name="attribute-on-multiple-controllers"></a><span data-ttu-id="bd4b3-156">多個控制器上的屬性</span><span class="sxs-lookup"><span data-stu-id="bd4b3-156">Attribute on multiple controllers</span></span>

<span data-ttu-id="bd4b3-157">在多個控制站上使用同一屬性的方法之一，就是建立以 `[ApiController]` 屬性標註的自訂基底控制器類別。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-157">One approach to using the attribute on more than one controller is to create a custom base controller class annotated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="bd4b3-158">下列範例顯示自訂基類，以及從它衍生的控制器：</span><span class="sxs-lookup"><span data-stu-id="bd4b3-158">The following example shows a custom base class and a controller that derives from it:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="attribute-on-an-assembly"></a><span data-ttu-id="bd4b3-159">元件上的屬性</span><span class="sxs-lookup"><span data-stu-id="bd4b3-159">Attribute on an assembly</span></span>

<span data-ttu-id="bd4b3-160">如果[相容性版本](xref:mvc/compatibility-version)設定為 2.2 或更新版本，`[ApiController]` 屬性就可以套用至組件。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-160">If [compatibility version](xref:mvc/compatibility-version) is set to 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="bd4b3-161">以這種方式標註會將 Web API 行為套用至組件中的所有控制器。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-161">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="bd4b3-162">沒有任何方法可以退出個別控制器。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-162">There's no way to opt out for individual controllers.</span></span> <span data-ttu-id="bd4b3-163">將元件層級屬性套用至類別周圍的命名空間宣告 `Startup` ：</span><span class="sxs-lookup"><span data-stu-id="bd4b3-163">Apply the assembly-level attribute to the namespace declaration surrounding the `Startup` class:</span></span>

```csharp
[assembly: ApiController]
namespace WebApiSample
{
    public class Startup
    {
        ...
    }
}
```

::: moniker-end

## <a name="attribute-routing-requirement"></a><span data-ttu-id="bd4b3-164">屬性路由需求</span><span class="sxs-lookup"><span data-stu-id="bd4b3-164">Attribute routing requirement</span></span>

<span data-ttu-id="bd4b3-165">`[ApiController]` 屬性會讓屬性路由需求。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-165">The `[ApiController]` attribute makes attribute routing a requirement.</span></span> <span data-ttu-id="bd4b3-166">例如：</span><span class="sxs-lookup"><span data-stu-id="bd4b3-166">For example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="bd4b3-167">在中，您可以透過、或所定義的慣例[路由](xref:mvc/controllers/routing#conventional-routing)來存取動作 `UseEndpoints` <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A> <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-167">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by `UseEndpoints`, <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A>, or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> in `Startup.Configure`.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="bd4b3-168">無法透過 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A> 中所定義的[慣例路由](xref:mvc/controllers/routing#conventional-routing)或 `Startup.Configure` 中的 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> 來存取動作。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-168">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A> or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> in `Startup.Configure`.</span></span>

::: moniker-end

## <a name="automatic-http-400-responses"></a><span data-ttu-id="bd4b3-169">HTTP 400 自動回應</span><span class="sxs-lookup"><span data-stu-id="bd4b3-169">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="bd4b3-170">`[ApiController]` 屬性會讓模型驗證錯誤自動觸發 HTTP 400 回應。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-170">The `[ApiController]` attribute makes model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="bd4b3-171">因此，動作方法不再需要下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="bd4b3-171">Consequently, the following code is unnecessary in an action method:</span></span>

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

<span data-ttu-id="bd4b3-172">ASP.NET Core MVC 會使用 <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ModelStateInvalidFilter> 動作篩選準則來執行上述檢查。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-172">ASP.NET Core MVC uses the <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ModelStateInvalidFilter> action filter to do the preceding check.</span></span>

### <a name="default-badrequest-response"></a><span data-ttu-id="bd4b3-173">預設 BadRequest 回應</span><span class="sxs-lookup"><span data-stu-id="bd4b3-173">Default BadRequest response</span></span>

<span data-ttu-id="bd4b3-174">當相容性版本為2.1 時，HTTP 400 回應的預設回應類型為 <xref:Microsoft.AspNetCore.Mvc.SerializableError> 。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-174">With a compatibility version of 2.1, the default response type for an HTTP 400 response is <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span></span> <span data-ttu-id="bd4b3-175">下列要求主體是序列化類型的範例：</span><span class="sxs-lookup"><span data-stu-id="bd4b3-175">The following request body is an example of the serialized type:</span></span>

```json
{
  "": [
    "A non-empty request body is required."
  ]
}
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="bd4b3-176">當相容性版本為2.2 或更新版本時，HTTP 400 回應的預設回應類型為 <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> 。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-176">With a compatibility version of 2.2 or later, the default response type for an HTTP 400 response is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="bd4b3-177">下列要求主體是序列化類型的範例：</span><span class="sxs-lookup"><span data-stu-id="bd4b3-177">The following request body is an example of the serialized type:</span></span>

```json
{
  "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",
  "title": "One or more validation errors occurred.",
  "status": 400,
  "traceId": "|7fb5e16a-4c8f23bbfc974667.",
  "errors": {
    "": [
      "A non-empty request body is required."
    ]
  }
}
```

<span data-ttu-id="bd4b3-178">`ValidationProblemDetails`類型：</span><span class="sxs-lookup"><span data-stu-id="bd4b3-178">The `ValidationProblemDetails` type:</span></span>

* <span data-ttu-id="bd4b3-179">提供電腦可讀取的格式，以指定 Web API 回應中的錯誤。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-179">Provides a machine-readable format for specifying errors in web API responses.</span></span>
* <span data-ttu-id="bd4b3-180">符合[RFC 7807 規格](https://tools.ietf.org/html/rfc7807)。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-180">Complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>

::: moniker-end

### <a name="log-automatic-400-responses"></a><span data-ttu-id="bd4b3-181">記錄自動 400 回應</span><span class="sxs-lookup"><span data-stu-id="bd4b3-181">Log automatic 400 responses</span></span>

<span data-ttu-id="bd4b3-182">請參閱[如何在模型驗證錯誤上記錄自動 400 回應 (aspnet/AspNetCore.Docs #12157)](https://github.com/dotnet/AspNetCore.Docs/issues/12157) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-182">See [How to log automatic 400 responses on model validation errors (aspnet/AspNetCore.Docs #12157)](https://github.com/dotnet/AspNetCore.Docs/issues/12157).</span></span>

### <a name="disable-automatic-400-response"></a><span data-ttu-id="bd4b3-183">停用自動400回應</span><span class="sxs-lookup"><span data-stu-id="bd4b3-183">Disable automatic 400 response</span></span>

<span data-ttu-id="bd4b3-184">若要停用自動 400 行為，請將 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> 屬性設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-184">To disable the automatic 400 behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property to `true`.</span></span> <span data-ttu-id="bd4b3-185">在 `Startup.ConfigureServices` 中新增下列醒目提示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="bd4b3-185">Add the following highlighted code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,6)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](index/samples/2.x/2.1/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=1,5)]

::: moniker-end

## <a name="binding-source-parameter-inference"></a><span data-ttu-id="bd4b3-186">繫結來源參數推斷</span><span class="sxs-lookup"><span data-stu-id="bd4b3-186">Binding source parameter inference</span></span>

<span data-ttu-id="bd4b3-187">繫結來源屬性可定義動作參數值的所在位置。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-187">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="bd4b3-188">系統提供下列繫結來源屬性：</span><span class="sxs-lookup"><span data-stu-id="bd4b3-188">The following binding source attributes exist:</span></span>

|<span data-ttu-id="bd4b3-189">屬性</span><span class="sxs-lookup"><span data-stu-id="bd4b3-189">Attribute</span></span>|<span data-ttu-id="bd4b3-190">繫結來源</span><span class="sxs-lookup"><span data-stu-id="bd4b3-190">Binding source</span></span> |
|---------|---------|
|[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)     | <span data-ttu-id="bd4b3-191">要求本文</span><span class="sxs-lookup"><span data-stu-id="bd4b3-191">Request body</span></span> |
|[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)     | <span data-ttu-id="bd4b3-192">要求本文中的表單資料</span><span class="sxs-lookup"><span data-stu-id="bd4b3-192">Form data in the request body</span></span> |
|[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) | <span data-ttu-id="bd4b3-193">要求標頭</span><span class="sxs-lookup"><span data-stu-id="bd4b3-193">Request header</span></span> |
|[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)   | <span data-ttu-id="bd4b3-194">要求查詢字串參數</span><span class="sxs-lookup"><span data-stu-id="bd4b3-194">Request query string parameter</span></span> |
|[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)   | <span data-ttu-id="bd4b3-195">來自目前要求的路由資料</span><span class="sxs-lookup"><span data-stu-id="bd4b3-195">Route data from the current request</span></span> |
|[`[FromServices]`](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) | <span data-ttu-id="bd4b3-196">作為動作參數插入的要求服務</span><span class="sxs-lookup"><span data-stu-id="bd4b3-196">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="bd4b3-197">當值可能包含 `%2f` (也就是 `/`) 時，請勿使用 `[FromRoute]`。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-197">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="bd4b3-198">`%2f` 不會是未逸出的 `/`。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-198">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="bd4b3-199">如果值可能包含 `%2f`，請使用 `[FromQuery]`。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-199">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="bd4b3-200">若沒有 `[ApiController]` 屬性或繫結來源屬性 (例如 `[FromQuery]`)，ASP.NET Core 執行階段會嘗試使用複雜物件模型繫結器。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-200">Without the `[ApiController]` attribute or binding source attributes like `[FromQuery]`, the ASP.NET Core runtime attempts to use the complex object model binder.</span></span> <span data-ttu-id="bd4b3-201">複雜物件模型繫結器會以定義的順序從值提供者提取資料。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-201">The complex object model binder pulls data from value providers in a defined order.</span></span>

<span data-ttu-id="bd4b3-202">在下列範例中，`[FromQuery]` 屬性表示 `discontinuedOnly` 參數值是在要求 URL 查詢字串中提供：</span><span class="sxs-lookup"><span data-stu-id="bd4b3-202">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="bd4b3-203">`[ApiController]` 屬性會依據動作參數的預設資料來源套用推斷規則。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-203">The `[ApiController]` attribute applies inference rules for the default data sources of action parameters.</span></span> <span data-ttu-id="bd4b3-204">這些規則透過將屬性套用至動作參數，讓您不必以手動方式識別的繫結來源。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-204">These rules save you from having to identify binding sources manually by applying attributes to the action parameters.</span></span> <span data-ttu-id="bd4b3-205">繫結來源推斷規則的行為如下所示：</span><span class="sxs-lookup"><span data-stu-id="bd4b3-205">The binding source inference rules behave as follows:</span></span>

* <span data-ttu-id="bd4b3-206">系統會依據複雜類型參數推斷 `[FromBody]`。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-206">`[FromBody]` is inferred for complex type parameters.</span></span> <span data-ttu-id="bd4b3-207">如果是任何具有像是 <xref:Microsoft.AspNetCore.Http.IFormCollection> 與 <xref:System.Threading.CancellationToken> 等特殊意義的複雜內建類型，則為 `[FromBody]` 推斷規則的例外。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-207">An exception to the `[FromBody]` inference rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="bd4b3-208">繫結來源推斷程式碼會忽略這些特殊的類型。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-208">The binding source inference code ignores those special types.</span></span>
* <span data-ttu-id="bd4b3-209">`[FromForm]` 針對類型 <xref:Microsoft.AspNetCore.Http.IFormFile> 與 <xref:Microsoft.AspNetCore.Http.IFormFileCollection> 的動作參數推斷的。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-209">`[FromForm]` is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="bd4b3-210">而不會依據任何簡單或使用者定義的類型進行推斷。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-210">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="bd4b3-211">系統會依據符合路由範本參數的任何動作參數名稱推斷 `[FromRoute]`。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-211">`[FromRoute]` is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="bd4b3-212">如果有多個路由符合動作參數，則會將任何路由值視為 `[FromRoute]`。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-212">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="bd4b3-213">系統會依據任何其他動作參數推斷 `[FromQuery]`。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-213">`[FromQuery]` is inferred for any other action parameters.</span></span>

### <a name="frombody-inference-notes"></a><span data-ttu-id="bd4b3-214">FromBody 推斷備註</span><span class="sxs-lookup"><span data-stu-id="bd4b3-214">FromBody inference notes</span></span>

<span data-ttu-id="bd4b3-215">並不會為像是 `string` 或 `int` 等簡單型別，推斷 `[FromBody]`。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-215">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="bd4b3-216">因此，當需要使用該功能時，應為簡單型別使用 `[FromBody]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-216">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span>

<span data-ttu-id="bd4b3-217">當動作有多個參數從要求主體繫結時，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-217">When an action has more than one parameter bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="bd4b3-218">例如，下列所有動作方法簽章都會造成例外狀況：</span><span class="sxs-lookup"><span data-stu-id="bd4b3-218">For example, all of the following action method signatures cause an exception:</span></span>

* <span data-ttu-id="bd4b3-219">因兩個參數都是複雜類型，而推斷 `[FromBody]`。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-219">`[FromBody]` inferred on both because they're complex types.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* <span data-ttu-id="bd4b3-220">因為其為複雜類型，所以一個有 `[FromBody]` 屬性，而推斷另一個。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-220">`[FromBody]` attribute on one, inferred on the other because it's a complex type.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* <span data-ttu-id="bd4b3-221">兩者均有 `[FromBody]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-221">`[FromBody]` attribute on both.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

::: moniker range="= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="bd4b3-222">在 ASP.NET Core 2.1 中，集合類型參數 (例如清單與陣列) 不正確地推斷為 `[FromQuery]`。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-222">In ASP.NET Core 2.1, collection type parameters such as lists and arrays are incorrectly inferred as `[FromQuery]`.</span></span> <span data-ttu-id="bd4b3-223">`[FromBody]` 屬性應該用於這些參數 (若它們將從要求本文繫結)。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-223">The `[FromBody]` attribute should be used for these parameters if they are to be bound from the request body.</span></span> <span data-ttu-id="bd4b3-224">此行為在 ASP.NET Core 2.2 或更新版本中已修正，其中集合類型參數預設推斷為從本文繫結。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-224">This behavior is corrected in ASP.NET Core 2.2 or later, where collection type parameters are inferred to be bound from the body by default.</span></span>

::: moniker-end

### <a name="disable-inference-rules"></a><span data-ttu-id="bd4b3-225">停用推斷規則</span><span class="sxs-lookup"><span data-stu-id="bd4b3-225">Disable inference rules</span></span>

<span data-ttu-id="bd4b3-226">若要停用繫結來源推斷，請將 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> 設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-226">To disable binding source inference, set <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> to `true`.</span></span> <span data-ttu-id="bd4b3-227">將下列程式碼加入 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="bd4b3-227">Add the following code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,5)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](index/samples/2.x/2.1/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=1,4)]

::: moniker-end

## <a name="multipartform-data-request-inference"></a><span data-ttu-id="bd4b3-228">多部分/表單資料要求推斷</span><span class="sxs-lookup"><span data-stu-id="bd4b3-228">Multipart/form-data request inference</span></span>

<span data-ttu-id="bd4b3-229">`[ApiController]`當動作參數以屬性標注時，屬性會套用推斷規則 [`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) 。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-229">The `[ApiController]` attribute applies an inference rule when an action parameter is annotated with the [`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute.</span></span> <span data-ttu-id="bd4b3-230">`multipart/form-data`會推斷要求內容類型。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-230">The `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="bd4b3-231">若要停用預設行為，請將 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> 屬性設定為 `true` 中的 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="bd4b3-231">To disable the default behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property to `true` in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,4)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,5)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](index/samples/2.x/2.1/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=1,3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="problem-details-for-error-status-codes"></a><span data-ttu-id="bd4b3-232">錯誤狀態碼的問題詳細資料</span><span class="sxs-lookup"><span data-stu-id="bd4b3-232">Problem details for error status codes</span></span>

<span data-ttu-id="bd4b3-233">當相容性版本為 2.2 或更新版本時，MVC 會將錯誤結果 (具有狀態碼 400 以上的結果) 為具有 <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> 的結果。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-233">When the compatibility version is 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="bd4b3-234">以 [RFC 7807 規格](https://tools.ietf.org/html/rfc7807)為基礎的 `ProblemDetails` 類型，在 HTTP 回應中提供電腦可讀取的錯誤詳細資料。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-234">The `ProblemDetails` type is based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807) for providing machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="bd4b3-235">請考慮下列控制器動作中的程式碼：</span><span class="sxs-lookup"><span data-stu-id="bd4b3-235">Consider the following code in a controller action:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="bd4b3-236">`NotFound`方法會產生具有主體的 HTTP 404 狀態碼 `ProblemDetails` 。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-236">The `NotFound` method produces an HTTP 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="bd4b3-237">例如：</span><span class="sxs-lookup"><span data-stu-id="bd4b3-237">For example:</span></span>

```json
{
  type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
  title: "Not Found",
  status: 404,
  traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="disable-problemdetails-response"></a><span data-ttu-id="bd4b3-238">停用 ProblemDetails 回應</span><span class="sxs-lookup"><span data-stu-id="bd4b3-238">Disable ProblemDetails response</span></span>

<span data-ttu-id="bd4b3-239">`ProblemDetails`當屬性設定為時，會停用的自動建立錯誤狀態碼 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors%2A> `true` 。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-239">The automatic creation of a `ProblemDetails` for error status codes is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors%2A> property is set to `true`.</span></span> <span data-ttu-id="bd4b3-240">將下列程式碼加入 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="bd4b3-240">Add the following code in `Startup.ConfigureServices`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,7)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

::: moniker-end

<a name="consumes"></a>

## <a name="define-supported-request-content-types-with-the-consumes-attribute"></a><span data-ttu-id="bd4b3-241">使用 [使用] 屬性定義支援的要求內容類型</span><span class="sxs-lookup"><span data-stu-id="bd4b3-241">Define supported request content types with the [Consumes] attribute</span></span>

<span data-ttu-id="bd4b3-242">根據預設，動作支援所有可用的要求內容類型。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-242">By default, an action supports all available request content types.</span></span> <span data-ttu-id="bd4b3-243">例如，如果將應用程式設定為同時支援 JSON 和 XML[輸入](xref:mvc/models/model-binding#input-formatters)格式器，則動作支援多種內容類型，包括 `application/json` 和 `application/xml` 。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-243">For example, if an app is configured to support both JSON and XML [input formatters](xref:mvc/models/model-binding#input-formatters), an action supports multiple content types, including `application/json` and `application/xml`.</span></span>

<span data-ttu-id="bd4b3-244">[[使用]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)屬性允許動作限制支援的要求內容類型。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-244">The [[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>) attribute allows an action to limit the supported request content types.</span></span> <span data-ttu-id="bd4b3-245">將 `[Consumes]` 屬性套用至動作或控制器，並指定一或多個內容類型：</span><span class="sxs-lookup"><span data-stu-id="bd4b3-245">Apply the `[Consumes]` attribute to an action or controller, specifying one or more content types:</span></span>

```csharp
[HttpPost]
[Consumes("application/xml")]
public IActionResult CreateProduct(Product product)
```

<span data-ttu-id="bd4b3-246">在上述程式碼中， `CreateProduct` 動作會指定內容類型 `application/xml` 。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-246">In the preceding code, the `CreateProduct` action specifies the content type `application/xml`.</span></span> <span data-ttu-id="bd4b3-247">路由至此動作的要求必須指定 `Content-Type` 標頭 `application/xml` 。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-247">Requests routed to this action must specify a `Content-Type` header of `application/xml`.</span></span> <span data-ttu-id="bd4b3-248">如果要求未指定 `Content-Type` 標頭， `application/xml` 則會產生[415 不支援的媒體類型](https://developer.mozilla.org/docs/Web/HTTP/Status/415)回應。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-248">Requests that don't specify a `Content-Type` header of `application/xml` result in a [415 Unsupported Media Type](https://developer.mozilla.org/docs/Web/HTTP/Status/415) response.</span></span>

<span data-ttu-id="bd4b3-249">`[Consumes]`屬性也允許動作根據傳入要求的內容類型（藉由套用類型條件約束）來影響其選取。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-249">The `[Consumes]` attribute also allows an action to influence its selection based on an incoming request's content type by applying a type constraint.</span></span> <span data-ttu-id="bd4b3-250">請考慮下列範例：</span><span class="sxs-lookup"><span data-stu-id="bd4b3-250">Consider the following example:</span></span>

[!code-csharp[](index/samples/3.x/Controllers/ConsumesController.cs?name=snippet_Class)]

<span data-ttu-id="bd4b3-251">在上述程式碼中， `ConsumesController` 是設定來處理傳送至 URL 的要求 `https://localhost:5001/api/Consumes` 。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-251">In the preceding code, `ConsumesController` is configured to handle requests sent to the `https://localhost:5001/api/Consumes` URL.</span></span> <span data-ttu-id="bd4b3-252">這兩個控制器的動作， `PostJson` 和都會 `PostForm` 處理具有相同 URL 的 POST 要求。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-252">Both of the controller's actions, `PostJson` and `PostForm`, handle POST requests with the same URL.</span></span> <span data-ttu-id="bd4b3-253">如果沒有套用 `[Consumes]` 類型條件約束的屬性，就會擲回不明確的 match 例外狀況。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-253">Without the `[Consumes]` attribute applying a type constraint, an ambiguous match exception is thrown.</span></span>

<span data-ttu-id="bd4b3-254">`[Consumes]`屬性會套用至這兩個動作。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-254">The `[Consumes]` attribute is applied to both actions.</span></span> <span data-ttu-id="bd4b3-255">`PostJson`動作會處理標頭所傳送 `Content-Type` 的要求 `application/json` 。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-255">The `PostJson` action handles requests sent with a `Content-Type` header of `application/json`.</span></span> <span data-ttu-id="bd4b3-256">`PostForm`動作會處理標頭所傳送 `Content-Type` 的要求 `application/x-www-form-urlencoded` 。</span><span class="sxs-lookup"><span data-stu-id="bd4b3-256">The `PostForm` action handles requests sent with a `Content-Type` header of `application/x-www-form-urlencoded`.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="bd4b3-257">其他資源</span><span class="sxs-lookup"><span data-stu-id="bd4b3-257">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/handle-errors>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
