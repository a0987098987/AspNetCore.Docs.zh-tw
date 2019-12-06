---
title: 使用 ASP.NET Core 建立 Web API
author: scottaddie
description: 了解使用 ASP.NET Core 建立 Web API 的基本概念。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 11/22/2019
uid: web-api/index
ms.openlocfilehash: 5ef8b4d012f4ed90339ffea191612e4dc365d958
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880534"
---
# <a name="create-web-apis-with-aspnet-core"></a><span data-ttu-id="6792c-103">使用 ASP.NET Core 建立 Web API</span><span class="sxs-lookup"><span data-stu-id="6792c-103">Create web APIs with ASP.NET Core</span></span>

<span data-ttu-id="6792c-104">作者：[Scott Addie](https://github.com/scottaddie) 與 [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="6792c-104">By [Scott Addie](https://github.com/scottaddie) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="6792c-105">ASP.NET Core 支援使用 C# 建立 RESTful 服務，也稱為 Web API。</span><span class="sxs-lookup"><span data-stu-id="6792c-105">ASP.NET Core supports creating RESTful services, also known as web APIs, using C#.</span></span> <span data-ttu-id="6792c-106">若要處理要求，Web API 會使用控制器。</span><span class="sxs-lookup"><span data-stu-id="6792c-106">To handle requests, a web API uses controllers.</span></span> <span data-ttu-id="6792c-107">Web API 中的「控制器」都衍生自類別 `ControllerBase`。</span><span class="sxs-lookup"><span data-stu-id="6792c-107">*Controllers* in a web API are classes that derive from `ControllerBase`.</span></span> <span data-ttu-id="6792c-108">本文說明如何使用控制器來處理 Web API 要求。</span><span class="sxs-lookup"><span data-stu-id="6792c-108">This article shows how to use controllers for handling web API requests.</span></span>

<span data-ttu-id="6792c-109">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples)。</span><span class="sxs-lookup"><span data-stu-id="6792c-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span></span> <span data-ttu-id="6792c-110">([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="6792c-110">([How to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="controllerbase-class"></a><span data-ttu-id="6792c-111">ControllerBase 類別</span><span class="sxs-lookup"><span data-stu-id="6792c-111">ControllerBase class</span></span>

<span data-ttu-id="6792c-112">Web API 由一或多個衍生自 <xref:Microsoft.AspNetCore.Mvc.ControllerBase>的控制器類別所組成。</span><span class="sxs-lookup"><span data-stu-id="6792c-112">A web API consists of one or more controller classes that derive from <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="6792c-113">Web API 專案範本提供入門控制器：</span><span class="sxs-lookup"><span data-stu-id="6792c-113">The web API project template provides a starter controller:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

<span data-ttu-id="6792c-114">請不要從 <xref:Microsoft.AspNetCore.Mvc.Controller> 類別衍生以建立 Web API 控制器。</span><span class="sxs-lookup"><span data-stu-id="6792c-114">Don't create a web API controller by deriving from the <xref:Microsoft.AspNetCore.Mvc.Controller> class.</span></span> <span data-ttu-id="6792c-115">`Controller` 衍生自 `ControllerBase` 並會新增檢視支援，以供處理網頁，而不是 Web API 要求。</span><span class="sxs-lookup"><span data-stu-id="6792c-115">`Controller` derives from `ControllerBase` and adds support for views, so it's for handling web pages, not web API requests.</span></span> <span data-ttu-id="6792c-116">此規則有例外狀況：如果您想要將相同的控制器用於 views 和 web Api，請從 `Controller`衍生。</span><span class="sxs-lookup"><span data-stu-id="6792c-116">There's an exception to this rule: if you plan to use the same controller for both views and web APIs, derive it from `Controller`.</span></span>

<span data-ttu-id="6792c-117">`ControllerBase` 類別提供許多處理 HTTP 要求的實用屬性和方法。</span><span class="sxs-lookup"><span data-stu-id="6792c-117">The `ControllerBase` class provides many properties and methods that are useful for handling HTTP requests.</span></span> <span data-ttu-id="6792c-118">例如，`ControllerBase.CreatedAtAction` 會傳回 201 狀態碼：</span><span class="sxs-lookup"><span data-stu-id="6792c-118">For example, `ControllerBase.CreatedAtAction` returns a 201 status code:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_400And201&highlight=10)]

<span data-ttu-id="6792c-119">以下是 `ControllerBase` 提供的一些其他方法範例。</span><span class="sxs-lookup"><span data-stu-id="6792c-119">Here are some more examples of methods that `ControllerBase` provides.</span></span>

|<span data-ttu-id="6792c-120">方法</span><span class="sxs-lookup"><span data-stu-id="6792c-120">Method</span></span>   |<span data-ttu-id="6792c-121">備註</span><span class="sxs-lookup"><span data-stu-id="6792c-121">Notes</span></span>    |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest%2A>| <span data-ttu-id="6792c-122">傳回 400 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="6792c-122">Returns 400 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound%2A>|<span data-ttu-id="6792c-123">傳回 404 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="6792c-123">Returns 404 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile%2A>|<span data-ttu-id="6792c-124">傳回檔案。</span><span class="sxs-lookup"><span data-stu-id="6792c-124">Returns a file.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync%2A>|<span data-ttu-id="6792c-125">叫用[模型繫結](xref:mvc/models/model-binding)。</span><span class="sxs-lookup"><span data-stu-id="6792c-125">Invokes [model binding](xref:mvc/models/model-binding).</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel%2A>|<span data-ttu-id="6792c-126">叫用[模型驗證](xref:mvc/models/validation)。</span><span class="sxs-lookup"><span data-stu-id="6792c-126">Invokes [model validation](xref:mvc/models/validation).</span></span>|

<span data-ttu-id="6792c-127">如需所有可用方法和屬性的清單，請參閱 <xref:Microsoft.AspNetCore.Mvc.ControllerBase>。</span><span class="sxs-lookup"><span data-stu-id="6792c-127">For a list of all available methods and properties, see <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span>

## <a name="attributes"></a><span data-ttu-id="6792c-128">屬性</span><span class="sxs-lookup"><span data-stu-id="6792c-128">Attributes</span></span>

<span data-ttu-id="6792c-129"><xref:Microsoft.AspNetCore.Mvc> 命名空間提供的屬性，可用來設定 Web API 控制器和動作方法的行為。</span><span class="sxs-lookup"><span data-stu-id="6792c-129">The <xref:Microsoft.AspNetCore.Mvc> namespace provides attributes that can be used to configure the behavior of web API controllers and action methods.</span></span> <span data-ttu-id="6792c-130">下列範例會使用屬性來指定支援的 HTTP 動作動詞，以及任何可能傳回的已知 HTTP 狀態碼：</span><span class="sxs-lookup"><span data-stu-id="6792c-130">The following example uses attributes to specify the supported HTTP action verb and any known HTTP status codes that could be returned:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

<span data-ttu-id="6792c-131">以下是一些其他可用的屬性範例。</span><span class="sxs-lookup"><span data-stu-id="6792c-131">Here are some more examples of attributes that are available.</span></span>

|<span data-ttu-id="6792c-132">屬性</span><span class="sxs-lookup"><span data-stu-id="6792c-132">Attribute</span></span>|<span data-ttu-id="6792c-133">備註</span><span class="sxs-lookup"><span data-stu-id="6792c-133">Notes</span></span>|
|---------|-----|
|[`[Route]`](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)      |<span data-ttu-id="6792c-134">指定控制器或動作的 URL 模式。</span><span class="sxs-lookup"><span data-stu-id="6792c-134">Specifies URL pattern for a controller or action.</span></span>|
|[`[Bind]`](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)        |<span data-ttu-id="6792c-135">指定模型繫結要包含的前置詞和屬性。</span><span class="sxs-lookup"><span data-stu-id="6792c-135">Specifies prefix and properties to include for model binding.</span></span>|
|[`[HttpGet]`](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)  |<span data-ttu-id="6792c-136">識別支援 HTTP GET 動作動詞的動作。</span><span class="sxs-lookup"><span data-stu-id="6792c-136">Identifies an action that supports the HTTP GET action verb.</span></span>|
|[`[Consumes]`](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)|<span data-ttu-id="6792c-137">指定動作所接受的資料類型。</span><span class="sxs-lookup"><span data-stu-id="6792c-137">Specifies data types that an action accepts.</span></span>|
|[`[Produces]`](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)|<span data-ttu-id="6792c-138">指定動作所傳回的資料類型。</span><span class="sxs-lookup"><span data-stu-id="6792c-138">Specifies data types that an action returns.</span></span>|

<span data-ttu-id="6792c-139">如需包含可用屬性的清單，請參閱 <xref:Microsoft.AspNetCore.Mvc> 命名空間。</span><span class="sxs-lookup"><span data-stu-id="6792c-139">For a list that includes the available attributes, see the <xref:Microsoft.AspNetCore.Mvc> namespace.</span></span>

## <a name="apicontroller-attribute"></a><span data-ttu-id="6792c-140">ApiController 屬性</span><span class="sxs-lookup"><span data-stu-id="6792c-140">ApiController attribute</span></span>

<span data-ttu-id="6792c-141">[`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute)屬性可以套用至控制器類別，以啟用下列固定的 API 特定行為：</span><span class="sxs-lookup"><span data-stu-id="6792c-141">The [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute can be applied to a controller class to enable the following opinionated, API-specific behaviors:</span></span>

* [<span data-ttu-id="6792c-142">屬性路由需求</span><span class="sxs-lookup"><span data-stu-id="6792c-142">Attribute routing requirement</span></span>](#attribute-routing-requirement)
* [<span data-ttu-id="6792c-143">HTTP 400 自動回應</span><span class="sxs-lookup"><span data-stu-id="6792c-143">Automatic HTTP 400 responses</span></span>](#automatic-http-400-responses)
* [<span data-ttu-id="6792c-144">繫結來源參數推斷</span><span class="sxs-lookup"><span data-stu-id="6792c-144">Binding source parameter inference</span></span>](#binding-source-parameter-inference)
* [<span data-ttu-id="6792c-145">多部分/表單資料要求推斷</span><span class="sxs-lookup"><span data-stu-id="6792c-145">Multipart/form-data request inference</span></span>](#multipartform-data-request-inference)
* [<span data-ttu-id="6792c-146">錯誤狀態碼的問題詳細資料</span><span class="sxs-lookup"><span data-stu-id="6792c-146">Problem details for error status codes</span></span>](#problem-details-for-error-status-codes)

<span data-ttu-id="6792c-147">這些功能都需要[相容性版本](xref:mvc/compatibility-version) 2.1 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="6792c-147">These features require a [compatibility version](xref:mvc/compatibility-version) of 2.1 or later.</span></span>

### <a name="attribute-on-specific-controllers"></a><span data-ttu-id="6792c-148">特定控制器上的屬性</span><span class="sxs-lookup"><span data-stu-id="6792c-148">Attribute on specific controllers</span></span>

<span data-ttu-id="6792c-149">`[ApiController]` 屬性可以套用至特定的控制器，如專案範本中的下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="6792c-149">The `[ApiController]` attribute can be applied to specific controllers, as in the following example from the project template:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2])]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=2)]

::: moniker-end

### <a name="attribute-on-multiple-controllers"></a><span data-ttu-id="6792c-150">多個控制器上的屬性</span><span class="sxs-lookup"><span data-stu-id="6792c-150">Attribute on multiple controllers</span></span>

<span data-ttu-id="6792c-151">在多個控制站上使用同一屬性的方法之一，就是建立以 `[ApiController]` 屬性標註的自訂基底控制器類別。</span><span class="sxs-lookup"><span data-stu-id="6792c-151">One approach to using the attribute on more than one controller is to create a custom base controller class annotated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="6792c-152">下列範例顯示自訂基類，以及從它衍生的控制器：</span><span class="sxs-lookup"><span data-stu-id="6792c-152">The following example shows a custom base class and a controller that derives from it:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="attribute-on-an-assembly"></a><span data-ttu-id="6792c-153">元件上的屬性</span><span class="sxs-lookup"><span data-stu-id="6792c-153">Attribute on an assembly</span></span>

<span data-ttu-id="6792c-154">如果[相容性版本](xref:mvc/compatibility-version)設定為 2.2 或更新版本，`[ApiController]` 屬性就可以套用至組件。</span><span class="sxs-lookup"><span data-stu-id="6792c-154">If [compatibility version](xref:mvc/compatibility-version) is set to 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="6792c-155">以這種方式標註會將 Web API 行為套用至組件中的所有控制器。</span><span class="sxs-lookup"><span data-stu-id="6792c-155">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="6792c-156">沒有任何方法可以退出個別控制器。</span><span class="sxs-lookup"><span data-stu-id="6792c-156">There's no way to opt out for individual controllers.</span></span> <span data-ttu-id="6792c-157">將元件層級屬性套用至圍繞 `Startup` 類別的命名空間宣告：</span><span class="sxs-lookup"><span data-stu-id="6792c-157">Apply the assembly-level attribute to the namespace declaration surrounding the `Startup` class:</span></span>

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

## <a name="attribute-routing-requirement"></a><span data-ttu-id="6792c-158">屬性路由需求</span><span class="sxs-lookup"><span data-stu-id="6792c-158">Attribute routing requirement</span></span>

<span data-ttu-id="6792c-159">`[ApiController]` 屬性會讓屬性路由需求。</span><span class="sxs-lookup"><span data-stu-id="6792c-159">The `[ApiController]` attribute makes attribute routing a requirement.</span></span> <span data-ttu-id="6792c-160">例如：</span><span class="sxs-lookup"><span data-stu-id="6792c-160">For example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="6792c-161">在中<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A> <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A>`UseEndpoints`，您`Startup.Configure`可以透過、或所定義的[慣例路由](xref:mvc/controllers/routing#conventional-routing)來存取動作。</span><span class="sxs-lookup"><span data-stu-id="6792c-161">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by `UseEndpoints`, <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A>, or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> in `Startup.Configure`.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="6792c-162">無法透過 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A> 中所定義的[慣例路由](xref:mvc/controllers/routing#conventional-routing)或 `Startup.Configure` 中的 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> 來存取動作。</span><span class="sxs-lookup"><span data-stu-id="6792c-162">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A> or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> in `Startup.Configure`.</span></span>

::: moniker-end

## <a name="automatic-http-400-responses"></a><span data-ttu-id="6792c-163">HTTP 400 自動回應</span><span class="sxs-lookup"><span data-stu-id="6792c-163">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="6792c-164">`[ApiController]` 屬性會讓模型驗證錯誤自動觸發 HTTP 400 回應。</span><span class="sxs-lookup"><span data-stu-id="6792c-164">The `[ApiController]` attribute makes model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="6792c-165">因此，動作方法不再需要下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="6792c-165">Consequently, the following code is unnecessary in an action method:</span></span>

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

<span data-ttu-id="6792c-166">ASP.NET Core MVC 會使用 <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ModelStateInvalidFilter> 動作篩選準則來執行上述檢查。</span><span class="sxs-lookup"><span data-stu-id="6792c-166">ASP.NET Core MVC uses the <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ModelStateInvalidFilter> action filter to do the preceding check.</span></span>

### <a name="default-badrequest-response"></a><span data-ttu-id="6792c-167">預設 BadRequest 回應</span><span class="sxs-lookup"><span data-stu-id="6792c-167">Default BadRequest response</span></span>

<span data-ttu-id="6792c-168">當相容性版本為2.1 時，HTTP 400 回應的預設回應類型為 <xref:Microsoft.AspNetCore.Mvc.SerializableError>。</span><span class="sxs-lookup"><span data-stu-id="6792c-168">With a compatibility version of 2.1, the default response type for an HTTP 400 response is <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span></span> <span data-ttu-id="6792c-169">下列要求主體是序列化類型的範例：</span><span class="sxs-lookup"><span data-stu-id="6792c-169">The following request body is an example of the serialized type:</span></span>

```json
{
  "": [
    "A non-empty request body is required."
  ]
}
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6792c-170">使用2.2 版或更新版本的相容性版本時，會 <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>HTTP 400 回應的預設回應類型。</span><span class="sxs-lookup"><span data-stu-id="6792c-170">With a compatibility version of 2.2 or later, the default response type for an HTTP 400 response is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="6792c-171">下列要求主體是序列化類型的範例：</span><span class="sxs-lookup"><span data-stu-id="6792c-171">The following request body is an example of the serialized type:</span></span>

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

<span data-ttu-id="6792c-172">`ValidationProblemDetails` 類型：</span><span class="sxs-lookup"><span data-stu-id="6792c-172">The `ValidationProblemDetails` type:</span></span>

* <span data-ttu-id="6792c-173">提供電腦可讀取的格式，以指定 Web API 回應中的錯誤。</span><span class="sxs-lookup"><span data-stu-id="6792c-173">Provides a machine-readable format for specifying errors in web API responses.</span></span>
* <span data-ttu-id="6792c-174">符合[RFC 7807 規格](https://tools.ietf.org/html/rfc7807)。</span><span class="sxs-lookup"><span data-stu-id="6792c-174">Complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>

::: moniker-end

### <a name="log-automatic-400-responses"></a><span data-ttu-id="6792c-175">記錄自動 400 回應</span><span class="sxs-lookup"><span data-stu-id="6792c-175">Log automatic 400 responses</span></span>

<span data-ttu-id="6792c-176">請參閱[如何在模型驗證錯誤上記錄自動 400 回應 (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="6792c-176">See [How to log automatic 400 responses on model validation errors (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span></span>

### <a name="disable-automatic-400-response"></a><span data-ttu-id="6792c-177">停用自動400回應</span><span class="sxs-lookup"><span data-stu-id="6792c-177">Disable automatic 400 response</span></span>

<span data-ttu-id="6792c-178">若要停用自動 400 行為，請將 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> 屬性設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="6792c-178">To disable the automatic 400 behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property to `true`.</span></span> <span data-ttu-id="6792c-179">在 `Startup.ConfigureServices` 中新增下列醒目提示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="6792c-179">Add the following highlighted code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,6)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](index/samples/2.x/2.1/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=1,5)]

::: moniker-end

## <a name="binding-source-parameter-inference"></a><span data-ttu-id="6792c-180">繫結來源參數推斷</span><span class="sxs-lookup"><span data-stu-id="6792c-180">Binding source parameter inference</span></span>

<span data-ttu-id="6792c-181">繫結來源屬性可定義動作參數值的所在位置。</span><span class="sxs-lookup"><span data-stu-id="6792c-181">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="6792c-182">系統提供下列繫結來源屬性：</span><span class="sxs-lookup"><span data-stu-id="6792c-182">The following binding source attributes exist:</span></span>

|<span data-ttu-id="6792c-183">屬性</span><span class="sxs-lookup"><span data-stu-id="6792c-183">Attribute</span></span>|<span data-ttu-id="6792c-184">繫結來源</span><span class="sxs-lookup"><span data-stu-id="6792c-184">Binding source</span></span> |
|---------|---------|
|[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)     | <span data-ttu-id="6792c-185">要求本文</span><span class="sxs-lookup"><span data-stu-id="6792c-185">Request body</span></span> |
|[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)     | <span data-ttu-id="6792c-186">要求本文中的表單資料</span><span class="sxs-lookup"><span data-stu-id="6792c-186">Form data in the request body</span></span> |
|[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) | <span data-ttu-id="6792c-187">要求標頭</span><span class="sxs-lookup"><span data-stu-id="6792c-187">Request header</span></span> |
|[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)   | <span data-ttu-id="6792c-188">要求查詢字串參數</span><span class="sxs-lookup"><span data-stu-id="6792c-188">Request query string parameter</span></span> |
|[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)   | <span data-ttu-id="6792c-189">來自目前要求的路由資料</span><span class="sxs-lookup"><span data-stu-id="6792c-189">Route data from the current request</span></span> |
|[`[FromServices]`](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) | <span data-ttu-id="6792c-190">作為動作參數插入的要求服務</span><span class="sxs-lookup"><span data-stu-id="6792c-190">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="6792c-191">當值可能包含 `%2f` (也就是 `/`) 時，請勿使用 `[FromRoute]`。</span><span class="sxs-lookup"><span data-stu-id="6792c-191">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="6792c-192">`%2f` 不會是未逸出的 `/`。</span><span class="sxs-lookup"><span data-stu-id="6792c-192">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="6792c-193">如果值可能包含 `%2f`，請使用 `[FromQuery]`。</span><span class="sxs-lookup"><span data-stu-id="6792c-193">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="6792c-194">若沒有 `[ApiController]` 屬性或繫結來源屬性 (例如 `[FromQuery]`)，ASP.NET Core 執行階段會嘗試使用複雜物件模型繫結器。</span><span class="sxs-lookup"><span data-stu-id="6792c-194">Without the `[ApiController]` attribute or binding source attributes like `[FromQuery]`, the ASP.NET Core runtime attempts to use the complex object model binder.</span></span> <span data-ttu-id="6792c-195">複雜物件模型繫結器會以定義的順序從值提供者提取資料。</span><span class="sxs-lookup"><span data-stu-id="6792c-195">The complex object model binder pulls data from value providers in a defined order.</span></span>

<span data-ttu-id="6792c-196">在下列範例中，`[FromQuery]` 屬性表示 `discontinuedOnly` 參數值是在要求 URL 查詢字串中提供：</span><span class="sxs-lookup"><span data-stu-id="6792c-196">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="6792c-197">`[ApiController]` 屬性會依據動作參數的預設資料來源套用推斷規則。</span><span class="sxs-lookup"><span data-stu-id="6792c-197">The `[ApiController]` attribute applies inference rules for the default data sources of action parameters.</span></span> <span data-ttu-id="6792c-198">這些規則透過將屬性套用至動作參數，讓您不必以手動方式識別的繫結來源。</span><span class="sxs-lookup"><span data-stu-id="6792c-198">These rules save you from having to identify binding sources manually by applying attributes to the action parameters.</span></span> <span data-ttu-id="6792c-199">繫結來源推斷規則的行為如下所示：</span><span class="sxs-lookup"><span data-stu-id="6792c-199">The binding source inference rules behave as follows:</span></span>

* <span data-ttu-id="6792c-200">系統會依據複雜類型參數推斷 `[FromBody]`。</span><span class="sxs-lookup"><span data-stu-id="6792c-200">`[FromBody]` is inferred for complex type parameters.</span></span> <span data-ttu-id="6792c-201">如果是任何具有像是 <xref:Microsoft.AspNetCore.Http.IFormCollection> 與 <xref:System.Threading.CancellationToken> 等特殊意義的複雜內建類型，則為 `[FromBody]` 推斷規則的例外。</span><span class="sxs-lookup"><span data-stu-id="6792c-201">An exception to the `[FromBody]` inference rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="6792c-202">繫結來源推斷程式碼會忽略這些特殊的類型。</span><span class="sxs-lookup"><span data-stu-id="6792c-202">The binding source inference code ignores those special types.</span></span>
* <span data-ttu-id="6792c-203">`[FromForm]` 針對類型 <xref:Microsoft.AspNetCore.Http.IFormFile> 與 <xref:Microsoft.AspNetCore.Http.IFormFileCollection> 的動作參數推斷的。</span><span class="sxs-lookup"><span data-stu-id="6792c-203">`[FromForm]` is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="6792c-204">而不會依據任何簡單或使用者定義的類型進行推斷。</span><span class="sxs-lookup"><span data-stu-id="6792c-204">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="6792c-205">系統會依據符合路由範本參數的任何動作參數名稱推斷 `[FromRoute]`。</span><span class="sxs-lookup"><span data-stu-id="6792c-205">`[FromRoute]` is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="6792c-206">如果有多個路由符合動作參數，則會將任何路由值視為 `[FromRoute]`。</span><span class="sxs-lookup"><span data-stu-id="6792c-206">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="6792c-207">系統會依據任何其他動作參數推斷 `[FromQuery]`。</span><span class="sxs-lookup"><span data-stu-id="6792c-207">`[FromQuery]` is inferred for any other action parameters.</span></span>

### <a name="frombody-inference-notes"></a><span data-ttu-id="6792c-208">FromBody 推斷備註</span><span class="sxs-lookup"><span data-stu-id="6792c-208">FromBody inference notes</span></span>

<span data-ttu-id="6792c-209">並不會為像是 `string` 或 `int` 等簡單型別，推斷 `[FromBody]`。</span><span class="sxs-lookup"><span data-stu-id="6792c-209">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="6792c-210">因此，當需要使用該功能時，應為簡單型別使用 `[FromBody]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="6792c-210">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span>

<span data-ttu-id="6792c-211">當動作有多個參數從要求主體繫結時，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="6792c-211">When an action has more than one parameter bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="6792c-212">例如，下列所有動作方法簽章都會造成例外狀況：</span><span class="sxs-lookup"><span data-stu-id="6792c-212">For example, all of the following action method signatures cause an exception:</span></span>

* <span data-ttu-id="6792c-213">因兩個參數都是複雜類型，而推斷 `[FromBody]`。</span><span class="sxs-lookup"><span data-stu-id="6792c-213">`[FromBody]` inferred on both because they're complex types.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* <span data-ttu-id="6792c-214">因為其為複雜類型，所以一個有 `[FromBody]` 屬性，而推斷另一個。</span><span class="sxs-lookup"><span data-stu-id="6792c-214">`[FromBody]` attribute on one, inferred on the other because it's a complex type.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* <span data-ttu-id="6792c-215">兩者均有 `[FromBody]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="6792c-215">`[FromBody]` attribute on both.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

::: moniker range="= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="6792c-216">在 ASP.NET Core 2.1 中，集合類型參數 (例如清單與陣列) 不正確地推斷為 `[FromQuery]`。</span><span class="sxs-lookup"><span data-stu-id="6792c-216">In ASP.NET Core 2.1, collection type parameters such as lists and arrays are incorrectly inferred as `[FromQuery]`.</span></span> <span data-ttu-id="6792c-217">`[FromBody]` 屬性應該用於這些參數 (若它們將從要求本文繫結)。</span><span class="sxs-lookup"><span data-stu-id="6792c-217">The `[FromBody]` attribute should be used for these parameters if they are to be bound from the request body.</span></span> <span data-ttu-id="6792c-218">此行為在 ASP.NET Core 2.2 或更新版本中已修正，其中集合類型參數預設推斷為從本文繫結。</span><span class="sxs-lookup"><span data-stu-id="6792c-218">This behavior is corrected in ASP.NET Core 2.2 or later, where collection type parameters are inferred to be bound from the body by default.</span></span>

::: moniker-end

### <a name="disable-inference-rules"></a><span data-ttu-id="6792c-219">停用推斷規則</span><span class="sxs-lookup"><span data-stu-id="6792c-219">Disable inference rules</span></span>

<span data-ttu-id="6792c-220">若要停用繫結來源推斷，請將 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> 設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="6792c-220">To disable binding source inference, set <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> to `true`.</span></span> <span data-ttu-id="6792c-221">將下列程式碼加入 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="6792c-221">Add the following code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,5)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](index/samples/2.x/2.1/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=1,4)]

::: moniker-end

## <a name="multipartform-data-request-inference"></a><span data-ttu-id="6792c-222">多部分/表單資料要求推斷</span><span class="sxs-lookup"><span data-stu-id="6792c-222">Multipart/form-data request inference</span></span>

<span data-ttu-id="6792c-223">當動作參數以[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)屬性標注時，`[ApiController]` 屬性會套用推斷規則。</span><span class="sxs-lookup"><span data-stu-id="6792c-223">The `[ApiController]` attribute applies an inference rule when an action parameter is annotated with the [`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute.</span></span> <span data-ttu-id="6792c-224">會推斷 `multipart/form-data` 要求內容類型。</span><span class="sxs-lookup"><span data-stu-id="6792c-224">The `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="6792c-225">若要停用預設行為，請將 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> 屬性設定為 `Startup.ConfigureServices`中的 `true`：</span><span class="sxs-lookup"><span data-stu-id="6792c-225">To disable the default behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property to `true` in `Startup.ConfigureServices`:</span></span>

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

## <a name="problem-details-for-error-status-codes"></a><span data-ttu-id="6792c-226">錯誤狀態碼的問題詳細資料</span><span class="sxs-lookup"><span data-stu-id="6792c-226">Problem details for error status codes</span></span>

<span data-ttu-id="6792c-227">當相容性版本為 2.2 或更新版本時，MVC 會將錯誤結果 (具有狀態碼 400 以上的結果) 為具有 <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> 的結果。</span><span class="sxs-lookup"><span data-stu-id="6792c-227">When the compatibility version is 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="6792c-228">以 [RFC 7807 規格](https://tools.ietf.org/html/rfc7807)為基礎的 `ProblemDetails` 類型，在 HTTP 回應中提供電腦可讀取的錯誤詳細資料。</span><span class="sxs-lookup"><span data-stu-id="6792c-228">The `ProblemDetails` type is based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807) for providing machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="6792c-229">請考慮下列控制器動作中的程式碼：</span><span class="sxs-lookup"><span data-stu-id="6792c-229">Consider the following code in a controller action:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="6792c-230">`NotFound` 方法會產生具有 `ProblemDetails` 主體的 HTTP 404 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="6792c-230">The `NotFound` method produces an HTTP 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="6792c-231">例如：</span><span class="sxs-lookup"><span data-stu-id="6792c-231">For example:</span></span>

```json
{
  type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
  title: "Not Found",
  status: 404,
  traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="disable-problemdetails-response"></a><span data-ttu-id="6792c-232">停用 ProblemDetails 回應</span><span class="sxs-lookup"><span data-stu-id="6792c-232">Disable ProblemDetails response</span></span>

<span data-ttu-id="6792c-233">當 [<xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors%2A>] 屬性設定為 [`true`] 時，會停用自動建立 `ProblemDetails` 實例。</span><span class="sxs-lookup"><span data-stu-id="6792c-233">The automatic creation of a `ProblemDetails` instance is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors%2A> property is set to `true`.</span></span> <span data-ttu-id="6792c-234">將下列程式碼加入 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="6792c-234">Add the following code in `Startup.ConfigureServices`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,7)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="6792c-235">其他資源</span><span class="sxs-lookup"><span data-stu-id="6792c-235">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/handle-errors>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
