---
title: 使用 ASP.NET Core 建置 Web API
author: scottaddie
description: 了解可用來在 ASP.NET Core 中建置 Web API 的功能，以及每個功能的使用時機。
ms.author: scaddie
ms.custom: mvc
ms.date: 08/15/2018
uid: web-api/index
ms.openlocfilehash: 763b95fb8ed3806bc67b7ad199153ea1027efa57
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090416"
---
# <a name="build-web-apis-with-aspnet-core"></a>使用 ASP.NET Core 建置 Web API

作者：[Scott Addie](https://github.com/scottaddie)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

本文件說明如何在 ASP.NET Core 中建置 Web API，以及每項功能的最佳使用時機。

## <a name="derive-class-from-controllerbase"></a>來自 ControllerBase 的衍生類別

繼承自用作為 Web API 的控制器中之 <xref:Microsoft.AspNetCore.Mvc.ControllerBase> 類別。 例如: 

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

`ControllerBase` 類別可用來存取數個屬性和方法。 在上述程式碼中，範例包含 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> 與 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>。 您可以在動作方法內呼叫這些方法，以分別傳回 HTTP 400 和 201 狀態碼。 可以存取 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> 屬性 (亦由 `ControllerBase` 提供)，以處理要求模型驗證。

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a>以 ApiControllerAttribute 標註類別

ASP.NET Core 2.1 新增了 [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) 屬性代表 Web API 控制器類別。 例如: 

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

使用此屬性時，需要透過 <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> 設定的相容性版本 2.1 或更新版本。 舉例來說，*Startup.ConfigureServices* 中的醒目提示程式碼會設定 2.2 相容性旗標：

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

如需詳細資訊，請參閱<xref:mvc/compatibility-version>。

`[ApiController]` 屬性通常會與 `ControllerBase` 搭配使用，為控制站啟用 REST 特定行為。 `ControllerBase` 可讓您存取 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> 與 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*> 等方法。

另一個方法是建立標註了 `[ApiController]` 屬性的自訂基底控制器類別：

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

下列章節說明該屬性新增的便利功能。

### <a name="problem-details-responses-for-error-status-codes"></a>錯誤狀態碼的問題詳細資料回應

ASP.NET Core 2.1 及更新版本包含 [ProblemDetails](xref:Microsoft.AspNetCore.Mvc.ProblemDetails)，是一種以 [RFC 7807 規格](https://tools.ietf.org/html/rfc7807)為基礎的類型。 `ProblemDetails` 類型提供了標準化格式，可用來在 HTTP 回應中傳遞錯誤的電腦可讀取詳細資料。

在 ASP.NET Core 2.2 及更新版本中，MVC 會將錯誤狀態碼結果 (狀態碼 400 及以上) 轉換成有 `ProblemDetails` 的結果。 請考慮下列程式碼：

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_ProblemDetails_StatusCode&highlight=4)]

`NotFound` 結果的 HTTP 回應具有 404 狀態碼，以及如下所示的 `ProblemDetails` 本文：

```js
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

問題詳細資料功能需要 2.2 或更新版本的相容性旗標。 當 [SuppressMapClientErrors](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors> --> 屬性設定為 `true` 時，會停用預設行為。 下列來自 `Startup.ConfigureServices` 的醒目提示程式碼會停用問題詳細資料：

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=8)]

使用 [ClientErrorMapping](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping> --> 屬性可設定 `ProblemDetails` 回應的內容。 舉例來說，下列程式碼會更新 404 回應的 `type` 屬性：

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=10)]

### <a name="automatic-http-400-responses"></a>HTTP 400 自動回應

驗證錯誤會自動觸發 HTTP 400 回應。 您的動作中將不再需要下列程式碼：

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

使用 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> 可自訂所產生回應的輸出。

當 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> 屬性設定為 `true` 時，會停用預設行為。 請在 *Startup.ConfigureServices* 的 `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` 之後新增下列程式碼：

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

使用 2.2 或更新版本的相容性旗標，為 400 回應傳回的預設回應類型是 <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>。 使用 [SuppressUseValidationProblemDetailsForInvalidModelStateResponses](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressUseValidationProblemDetailsForInvalidModelStateResponses> --> 屬性可使用 ASP.NET Core 2.1 錯誤格式。

### <a name="binding-source-parameter-inference"></a>繫結來源參數推斷

繫結來源屬性可定義動作參數值的所在位置。 系統提供下列繫結來源屬性：

|屬性|繫結來源 |
|---------|---------|
|**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**     | 要求本文 |
|**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**     | 要求本文中的表單資料 |
|**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)** | 要求標頭 |
|**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**   | 要求查詢字串參數 |
|**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**   | 來自目前要求的路由資料 |
|**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)** | 作為動作參數插入的要求服務 |

> [!WARNING]
> 當值可能包含 `%2f` (也就是 `/`) 時，請勿使用 `[FromRoute]`。 `%2f` 不會是未逸出的 `/`。 如果值可能包含 `%2f`，請使用 `[FromQuery]`。

不含 `[ApiController]` 屬性時，即會明確定義繫結來源屬性。 在下列範例中，`[FromQuery]` 屬性表示 `discontinuedOnly` 參數值是在要求 URL 查詢字串中提供：

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

系統會依據動作參數的預設資料來源，套用推斷規則。 然後，這些規則會設定繫結來源，否則就可能要由您手動將其套用至動作參數。 繫結來源屬性的行為如下所示：

* 系統會依據複雜類型參數推斷 **[FromBody]**。 如果是任何具有像是 <xref:Microsoft.AspNetCore.Http.IFormCollection> 與 <xref:System.Threading.CancellationToken> 等特殊意義的複雜內建類型，則為此規則的例外。 繫結來源推斷程式碼會忽略這些特殊的類型。 並不會為像是 `string` 或 `int` 等簡單型別，推斷 `[FromBody]`。 因此，需要使用該功能時，應為簡單型別使用 `[FromBody]` 屬性。 當動作已明確指定多個參數 (透過 `[FromBody]`) 或推斷為從要求主體繫結時，會擲回例外狀況。 例如，下列動作簽章會造成例外狀況：

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* 為類型 <xref:Microsoft.AspNetCore.Http.IFormFile> 與 <xref:Microsoft.AspNetCore.Http.IFormFileCollection> 的動作參數，推斷 **[FromForm]**。 而不會依據任何簡單或使用者定義的類型進行推斷。
* 系統會依據符合路由範本參數的任何動作參數名稱，推斷 **[FromRoute]**。 如果有多個路由符合動作參數，則會將任何路由值視為 `[FromRoute]`。
* 系統會依據任何其他動作參數，推斷 **[FromQuery]**。

當 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> 屬性設定為 `true` 時，會停用預設推斷規則。 請在 *Startup.ConfigureServices* 的 `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` 之後新增下列程式碼：

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a>多部分/表單資料要求推斷

當動作參數標註了 [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) 屬性時，會推斷 `multipart/form-data` 要求內容類型。

當 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> 屬性設定為 `true` 時，會停用預設行為。 請在 *Startup.ConfigureServices* 的 `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` 之後新增下列程式碼：

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a>屬性路由需求

屬性路由已變成必要項目。 例如: 

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

無法透過 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> 中所定義的[慣例路由](xref:mvc/controllers/routing#conventional-routing)或 *Startup.Configure* 中的 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*>，來存取動作。

::: moniker-end

## <a name="additional-resources"></a>其他資源

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>