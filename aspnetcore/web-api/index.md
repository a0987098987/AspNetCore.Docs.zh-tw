---
title: 使用 ASP.NET Core 建置 Web API
author: scottaddie
description: 了解可用來在 ASP.NET Core 中建置 Web API 的功能，以及每個功能的使用時機。
ms.author: scaddie
ms.custom: mvc
ms.date: 01/11/2019
uid: web-api/index
ms.openlocfilehash: a826bdecdd3a25eb23597123166695c169ba4229
ms.sourcegitcommit: ec71fd5a988f927ae301813aae5ff764feb3bb6a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/12/2019
ms.locfileid: "54249434"
---
# <a name="build-web-apis-with-aspnet-core"></a>使用 ASP.NET Core 建置 Web API

作者：[Scott Addie](https://github.com/scottaddie)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

本文件說明如何在 ASP.NET Core 中建置 Web API，以及每項功能的最佳使用時機。

## <a name="derive-class-from-controllerbase"></a>來自 ControllerBase 的衍生類別

繼承自用作為 Web API 的控制器中之 <xref:Microsoft.AspNetCore.Mvc.ControllerBase> 類別。 例如：

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

`ControllerBase` 類別可用來存取數個屬性和方法。 在上述程式碼中，範例包含 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> 與 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>。 您可以在動作方法內呼叫這些方法，以分別傳回 HTTP 400 和 201 狀態碼。 可以存取 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> 屬性 (亦由 `ControllerBase` 提供)，以處理要求模型驗證。

::: moniker range=">= aspnetcore-2.1"

## <a name="annotation-with-apicontroller-attribute"></a>使用 ApiController 屬性的註釋

ASP.NET Core 2.1 新增了 [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) 屬性代表 Web API 控制器類別。 例如：

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

透過 <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> 所設定的 2.1 或更新相容性版本必須在控制器層級使用此屬性。 舉例來說，`Startup.ConfigureServices` 中的醒目提示程式碼會設定 2.1 相容性旗標：

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

如需詳細資訊，請參閱<xref:mvc/compatibility-version>。

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

在 ASP.NET Core 2.2 或更新版本中，`[ApiController]` 屬性可以套用至組件。 以這種方式標註會將 Web API 行為套用至組件中的所有控制器。 請注意，沒有任何方法可以退出個別控制器。 建議您，組件層級屬性應套用至 `Startup` 類別：

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ApiControllerAttributeOnAssembly&highlight=1)]

透過 <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> 所設定的 2.2 或更新相容性版本必須在組件層級使用此屬性。

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

`[ApiController]` 屬性通常會與 `ControllerBase` 搭配使用，為控制站啟用 REST 特定行為。 `ControllerBase` 可讓您存取 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> 與 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*> 等方法。

另一個方法是建立標註了 `[ApiController]` 屬性的自訂基底控制器類別：

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

下列章節說明該屬性新增的便利功能。

### <a name="automatic-http-400-responses"></a>HTTP 400 自動回應

模型驗證錯誤會自動觸發 HTTP 400 回應。 因此，您的動作將不再需要下列程式碼：

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

使用 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> 可自訂所產生回應的輸出。

當您的動作可以從模型驗證錯誤復原時，停用預設行為將會有所幫助。 當 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> 屬性設定為 `true` 時，會停用預設行為。 在 `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);` 後，於 `Startup.ConfigureServices` 中新增下列程式碼：

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=7)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

使用 2.2 或更新版本的相容性旗標，HTTP 400 回應的預設回應類型為 <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>。 `ValidationProblemDetails` 類型符合 [RFC 7807 規格](https://tools.ietf.org/html/rfc7807)。 將 `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` 屬性設定為 `true`，以改為傳回 <xref:Microsoft.AspNetCore.Mvc.SerializableError> 的 ASP.NET Core 2.1 錯誤格式。 將下列程式碼加入 `Startup.ConfigureServices`：

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

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

系統會依據動作參數的預設資料來源，套用推斷規則。 然後，這些規則會設定繫結來源，否則就可能要由您手動將其套用至動作參數。 繫結來源屬性的行為如下所示：

* 系統會依據複雜類型參數推斷 **[FromBody]**。 如果是任何具有像是 <xref:Microsoft.AspNetCore.Http.IFormCollection> 與 <xref:System.Threading.CancellationToken> 等特殊意義的複雜內建類型，則為此規則的例外。 繫結來源推斷程式碼會忽略這些特殊的類型。 並不會為像是 `string` 或 `int` 等簡單型別，推斷 `[FromBody]`。 因此，當需要使用該功能時，應為簡單型別使用 `[FromBody]` 屬性。 當動作已明確指定多個參數 (透過 `[FromBody]`) 或推斷為從要求主體繫結時，會擲回例外狀況。 例如，下列動作簽章會造成例外狀況：

    [!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

    > [!NOTE]
    > 在 ASP.NET Core 2.1 中，集合類型參數 (例如清單與陣列) 不正確地推斷為 [[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)。 [[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) 應該用於這些參數 (若它們將從要求本文繫結)。 此行為在 ASP.NET Core 2.2 或更新版本中已修復，其中集合類型參數預設推斷為從本文繫結。

* 為類型 <xref:Microsoft.AspNetCore.Http.IFormFile> 與 <xref:Microsoft.AspNetCore.Http.IFormFileCollection> 的動作參數，推斷 **[FromForm]**。 而不會依據任何簡單或使用者定義的類型進行推斷。
* 系統會依據符合路由範本參數的任何動作參數名稱，推斷 **[FromRoute]**。 如果有多個路由符合動作參數，則會將任何路由值視為 `[FromRoute]`。
* 系統會依據任何其他動作參數，推斷 **[FromQuery]**。

當 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> 屬性設定為 `true` 時，會停用預設推斷規則。 在 `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);` 後，於 `Startup.ConfigureServices` 中新增下列程式碼：

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=6)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="multipartform-data-request-inference"></a>多部分/表單資料要求推斷

當動作參數標註了 [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) 屬性時，會推斷 `multipart/form-data` 要求內容類型。

當 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> 屬性設定為 `true` 時，會停用預設行為。

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

將下列程式碼加入 `Startup.ConfigureServices`：

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

在 `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` 後，於 `Startup.ConfigureServices` 中新增下列程式碼：

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="attribute-routing-requirement"></a>屬性路由需求

屬性路由已變成必要項目。 例如：

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

無法透過 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> 中所定義的[慣例路由](xref:mvc/controllers/routing#conventional-routing)或 `Startup.Configure` 中的 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*>來存取動作。

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="problem-details-responses-for-error-status-codes"></a>錯誤狀態碼的問題詳細資料回應

在 ASP.NET Core 2.2 或更新版本中，MVC 會將錯誤結果 (具有狀態碼 400 以上 (含) 的結果) 為具有 <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> 的結果。 `ProblemDetails` 是：

* 以 [RFC 7807 規格](https://tools.ietf.org/html/rfc7807)為基礎的類型。
* 標準化格式，用以在 HTTP 回應中指定電腦可讀取的錯誤詳細資料。

請考慮下列控制器動作中的程式碼：

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Controllers/ProductsController.cs?name=snippet_ProblemDetailsStatusCode)]

`NotFound` 的 HTTP 回應具有 404 狀態碼，並附有 `ProblemDetails` 本文。 例如：

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

問題詳細資料功能需要 2.2 或更新版本的相容性旗標。 當 `SuppressMapClientErrors` 屬性設定為 `true` 時，會停用預設行為。 將下列程式碼加入 `Startup.ConfigureServices`：

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8)]

使用 `ClientErrorMapping` 屬性可設定 `ProblemDetails` 回應的內容。 舉例來說，下列程式碼會更新 404 回應的 `type` 屬性：

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

::: moniker-end

## <a name="additional-resources"></a>其他資源

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
