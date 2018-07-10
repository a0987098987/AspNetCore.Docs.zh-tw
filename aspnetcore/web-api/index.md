---
title: 使用 ASP.NET Core 建置 Web API
author: scottaddie
description: 了解可用來在 ASP.NET Core 中建置 Web API 的功能，以及每個功能的使用時機。
ms.author: scaddie
ms.custom: mvc
ms.date: 07/06/2018
uid: web-api/index
ms.openlocfilehash: 84e4a51a8a8ab031752ef054cba834bd202a4927
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894211"
---
# <a name="build-web-apis-with-aspnet-core"></a>使用 ASP.NET Core 建置 Web API

作者：[Scott Addie](https://github.com/scottaddie)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

本文件說明如何在 ASP.NET Core 中建置 Web API，以及每項功能的最佳使用時機。

## <a name="derive-class-from-controllerbase"></a>來自 ControllerBase 的衍生類別

繼承自用來作為 Web API 的控制器 [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) 類別。 例如: 

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

`ControllerBase` 類別可用來存取數個屬性和方法。 在上述程式碼中，這些範例包括 [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) 和 [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction)。 您可以在動作方法內呼叫這些方法，以分別傳回 HTTP 400 和 201 狀態碼。 您可以存取 [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) 屬性 (亦由 `ControllerBase` 提供)，以處理要求模型驗證。

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a>以 ApiControllerAttribute 標註類別

ASP.NET Core 2.1 新增了 [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) 屬性代表 Web API 控制器類別。 例如: 

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

透過 [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion) 設定的相容性 2.1 版或更新版本，是使用這個屬性的必要項。 例如，*Startup.ConfigureServices* 中的醒目提示程式碼會設定 2.1 相容性旗標：

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

`[ApiController]` 屬性通常會與 `ControllerBase` 搭配使用，為控制站啟用 REST 特定行為。 `ControllerBase` 可讓您存取 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) 和 [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file) 等方法。

另一個方法是建立標註了 `[ApiController]` 屬性的自訂基底控制器類別：

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

下列章節說明該屬性新增的便利功能。

### <a name="automatic-http-400-responses"></a>HTTP 400 自動回應

驗證錯誤會自動觸發 HTTP 400 回應。 您的動作中將不再需要下列程式碼：

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

當 [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) 屬性設定為 `true` 時，會停用預設行為。 請在 *Startup.ConfigureServices* 的 `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` 之後新增下列程式碼：

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a>繫結來源參數推斷

繫結來源屬性可定義動作參數值的所在位置。 系統提供下列繫結來源屬性：

|屬性|繫結來源 |
|---------|---------|
|**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**     | 要求本文 |
|**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**     | 要求本文中的表單資料 |
|**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)** | 要求標頭 |
|**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**   | 要求查詢字串參數 |
|**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**   | 來自目前要求的路由資料 |
|**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)** | 作為動作參數插入的要求服務 |

> [!WARNING]
> 當值可能包含 `%2f` (也就是 `/`) 時，請勿使用 `[FromRoute]`。 `%2f` 不會是未逸出的 `/`。 如果值可能包含 `%2f`，請使用 `[FromQuery]`。

不含 `[ApiController]` 屬性時，即會明確定義繫結來源屬性。 在下列範例中，`[FromQuery]` 屬性表示 `discontinuedOnly` 參數值是在要求 URL 查詢字串中提供：

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]

系統會依據動作參數的預設資料來源，套用推斷規則。 然後，這些規則會設定繫結來源，否則就可能要由您手動將其套用至動作參數。 繫結來源屬性的行為如下所示：

* 系統會依據複雜類型參數推斷 **[FromBody]**。 如果是任何具有 [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) 和 [CancellationToken](/dotnet/api/system.threading.cancellationtoken) 等特殊意義的複雜、內建類型，則為此規則的例外。 繫結來源推斷程式碼會忽略這些特殊的類型。 當動作已明確指定多個參數 (透過 `[FromBody]`) 或推斷為從要求主體繫結時，會擲回例外狀況。 例如，下列動作簽章會造成例外狀況：

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* 系統會依據 [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) 和 [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection) 動作參數類型，推斷 **[FromForm]**， 而不會依據任何簡單或使用者定義的類型進行推斷。
* 系統會依據符合路由範本參數的任何動作參數名稱，推斷 **[FromRoute]**。 如果有多個路由符合動作參數，則會將任何路由值視為 `[FromRoute]`。
* 系統會依據任何其他動作參數，推斷 **[FromQuery]**。

當 [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) 屬性設定為 `true` 時，會停用預設推斷規則。 請在 *Startup.ConfigureServices* 的 `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` 之後新增下列程式碼：

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a>多部分/表單資料要求推斷

當動作參數標註了 [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) 屬性時，會推斷 `multipart/form-data` 要求內容類型。

當 [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) 屬性設定為 `true` 時，會停用預設行為。 請在 *Startup.ConfigureServices* 的 `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` 之後新增下列程式碼：

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a>屬性路由需求

屬性路由已變成必要項目。 例如: 

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

您無法透過 [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) 中定義的[慣例路由](xref:mvc/controllers/routing#conventional-routing)或 *Startup.Configure* 中的 [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 來存取動作。

::: moniker-end

## <a name="additional-resources"></a>其他資源

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
