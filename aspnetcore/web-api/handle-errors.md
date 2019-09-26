---
title: 處理 ASP.NET Core web Api 中的錯誤
author: pranavkm
description: 瞭解 ASP.NET Core web Api 的錯誤處理。
monikerRange: '>= aspnetcore-2.1'
ms.author: prkrishn
ms.custom: mvc
ms.date: 09/25/2019
uid: web-api/handle-errors
ms.openlocfilehash: 9c5dd2f89e7351f386d1f0633c831952dc58e568
ms.sourcegitcommit: 994da92edb0abf856b1655c18880028b15a28897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/25/2019
ms.locfileid: "71278725"
---
# <a name="handle-errors-in-aspnet-core-web-apis"></a><span data-ttu-id="1df8c-103">處理 ASP.NET Core web Api 中的錯誤</span><span class="sxs-lookup"><span data-stu-id="1df8c-103">Handle errors in ASP.NET Core web APIs</span></span>

<span data-ttu-id="1df8c-104">本文說明如何使用 ASP.NET Core web Api 處理和自訂錯誤處理。</span><span class="sxs-lookup"><span data-stu-id="1df8c-104">This article describes how to handle and customize error handling with ASP.NET Core web APIs.</span></span>

<span data-ttu-id="1df8c-105">[查看或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/handle-errors/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="1df8c-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/handle-errors/samples) ([How to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="1df8c-106">開發人員例外狀況頁面</span><span class="sxs-lookup"><span data-stu-id="1df8c-106">Developer Exception Page</span></span>

<span data-ttu-id="1df8c-107">[[開發人員例外](xref:fundamentals/error-handling)狀況] 頁面是一個實用的工具，可取得伺服器錯誤的詳細堆疊追蹤。</span><span class="sxs-lookup"><span data-stu-id="1df8c-107">The [Developer Exception Page](xref:fundamentals/error-handling) is a useful tool to get detailed stack traces for server errors.</span></span>

<span data-ttu-id="1df8c-108">如果用戶端不接受 HTML 格式的輸出，則開發人員例外狀況頁面會顯示純文字回應。</span><span class="sxs-lookup"><span data-stu-id="1df8c-108">The Developer Exception Page displays a plain-text response if the client doesn't accept HTML-formatted output.</span></span> <span data-ttu-id="1df8c-109">例如：</span><span class="sxs-lookup"><span data-stu-id="1df8c-109">For example:</span></span>

```
> curl https://localhost:5001/weatherforecast
System.ArgumentException: count
   at errorhandling.Controllers.WeatherForecastController.Get(Int32 x) in D:\work\Samples\samples\aspnetcore\mvc\errorhandling\Controllers\WeatherForecastController.cs:line 35
   at lambda_method(Closure , Object , Object[] )
   at Microsoft.Extensions.Internal.ObjectMethodExecutor.Execute(Object target, Object[] parameters)
...
```

> [!WARNING]
> <span data-ttu-id="1df8c-110">**僅有當應用程式是在開發環境中執行時**，才啟用開發人員例外狀況頁面。</span><span class="sxs-lookup"><span data-stu-id="1df8c-110">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="1df8c-111">當應用程式在生產環境中執行時，您不會想要公開共用例外狀況的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="1df8c-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="1df8c-112">如需設定環境的詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="1df8c-112">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

## <a name="exception-handler"></a><span data-ttu-id="1df8c-113">例外處理常式</span><span class="sxs-lookup"><span data-stu-id="1df8c-113">Exception handler</span></span>

<span data-ttu-id="1df8c-114">在非開發環境中，[例外狀況處理中介軟體](xref:fundamentals/error-handling)可以用來產生錯誤承載：</span><span class="sxs-lookup"><span data-stu-id="1df8c-114">In non-development environments, [Exception Handling Middleware](xref:fundamentals/error-handling) can be used to produce an error payload:</span></span>

1. <span data-ttu-id="1df8c-115">在`Startup.Configure`中<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> ，叫用以使用中介軟體：</span><span class="sxs-lookup"><span data-stu-id="1df8c-115">In `Startup.Configure`, invoke <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> to use the middleware:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_UseExceptionHandler&highlight=9)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_UseExceptionHandler&highlight=9)]

    ::: moniker-end

1. <span data-ttu-id="1df8c-116">設定控制器動作以回應`/error`路由：</span><span class="sxs-lookup"><span data-stu-id="1df8c-116">Configure a controller action to respond to the `/error` route:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Controllers/ErrorController.cs?name=snippet_ErrorController)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Controllers/ErrorController.cs?name=snippet_ErrorController)]

    ::: moniker-end

<span data-ttu-id="1df8c-117">上述`Error`動作會將符合[RFC7807](https://tools.ietf.org/html/rfc7807)規範的承載傳送至用戶端。</span><span class="sxs-lookup"><span data-stu-id="1df8c-117">The preceding `Error` action sends an [RFC7807](https://tools.ietf.org/html/rfc7807)-compliant payload to the client.</span></span>

<span data-ttu-id="1df8c-118">例外狀況處理中介軟體也可以在本機開發環境中提供更詳細的內容協商輸出。</span><span class="sxs-lookup"><span data-stu-id="1df8c-118">Exception Handling Middleware can also provide more detailed content-negotiated output in the local development environment.</span></span> <span data-ttu-id="1df8c-119">使用下列步驟，在開發與生產環境之間產生一致的裝載格式：</span><span class="sxs-lookup"><span data-stu-id="1df8c-119">Use the following steps to produce a consistent payload format across development and production environments:</span></span>

1. <span data-ttu-id="1df8c-120">在`Startup.Configure`中，註冊環境特定的例外狀況處理中介軟體實例：</span><span class="sxs-lookup"><span data-stu-id="1df8c-120">In `Startup.Configure`, register environment-specific Exception Handling Middleware instances:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    ```csharp
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseExceptionHandler("/error-local-development");
        }
        else
        {
            app.UseExceptionHandler("/error");
        }
    }
    ```

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseExceptionHandler("/error-local-development");
        }
        else
        {
            app.UseExceptionHandler("/error");
        }
    }
    ```

    ::: moniker-end

    <span data-ttu-id="1df8c-121">在上述程式碼中，中介軟體會向註冊：</span><span class="sxs-lookup"><span data-stu-id="1df8c-121">In the preceding code, the middleware is registered with:</span></span>

    * <span data-ttu-id="1df8c-122">在開發環境`/error-local-development`中的路由。</span><span class="sxs-lookup"><span data-stu-id="1df8c-122">A route of `/error-local-development` in the Development environment.</span></span>
    * <span data-ttu-id="1df8c-123">在非開發`/error`環境中的路由。</span><span class="sxs-lookup"><span data-stu-id="1df8c-123">A route of `/error` in environments that aren't Development.</span></span>
    
1. <span data-ttu-id="1df8c-124">將屬性路由套用至控制器動作：</span><span class="sxs-lookup"><span data-stu-id="1df8c-124">Apply attribute routing to controller actions:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Controllers/ErrorController.cs?name=snippet_ErrorControllerEnvironmentSpecific)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Controllers/ErrorController.cs?name=snippet_ErrorControllerEnvironmentSpecific)]

    ::: moniker-end

## <a name="use-exceptions-to-modify-the-response"></a><span data-ttu-id="1df8c-125">使用例外狀況來修改回應</span><span class="sxs-lookup"><span data-stu-id="1df8c-125">Use exceptions to modify the response</span></span>

<span data-ttu-id="1df8c-126">您可以從控制器外部修改回應的內容。</span><span class="sxs-lookup"><span data-stu-id="1df8c-126">The contents of the response can be modified from outside of the controller.</span></span> <span data-ttu-id="1df8c-127">在 ASP.NET 4.x Web API 中，其中一種方法是使用<xref:System.Web.Http.HttpResponseException>型別。</span><span class="sxs-lookup"><span data-stu-id="1df8c-127">In ASP.NET 4.x Web API, one way to do this was using the <xref:System.Web.Http.HttpResponseException> type.</span></span> <span data-ttu-id="1df8c-128">ASP.NET Core 不包含對等的類型。</span><span class="sxs-lookup"><span data-stu-id="1df8c-128">ASP.NET Core doesn't include an equivalent type.</span></span> <span data-ttu-id="1df8c-129">您可以`HttpResponseException`使用下列步驟來新增的支援：</span><span class="sxs-lookup"><span data-stu-id="1df8c-129">Support for `HttpResponseException` can be added with the following steps:</span></span>

1. <span data-ttu-id="1df8c-130">建立名為`HttpResponseException`的知名例外狀況類型：</span><span class="sxs-lookup"><span data-stu-id="1df8c-130">Create a well-known exception type named `HttpResponseException`:</span></span>

    [!code-csharp[](handle-errors/samples/3.x/Exceptions/HttpResponseException.cs?name=snippet_HttpResponseException)]

1. <span data-ttu-id="1df8c-131">建立名為`HttpResponseExceptionFilter`的動作篩選準則：</span><span class="sxs-lookup"><span data-stu-id="1df8c-131">Create an action filter named `HttpResponseExceptionFilter`:</span></span>

    [!code-csharp[](handle-errors/samples/3.x/Filters/HttpResponseExceptionFilter.cs?name=snippet_HttpResponseExceptionFilter)]

1. <span data-ttu-id="1df8c-132">在`Startup.ConfigureServices`中，將動作篩選準則新增至篩選集合：</span><span class="sxs-lookup"><span data-stu-id="1df8c-132">In `Startup.ConfigureServices`, add the action filter to the filters collection:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.2"
    
    [!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.1"

    [!code-csharp[](handle-errors/samples/2.x/2.1/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

## <a name="validation-failure-error-response"></a><span data-ttu-id="1df8c-133">驗證失敗錯誤回應</span><span class="sxs-lookup"><span data-stu-id="1df8c-133">Validation failure error response</span></span>

<span data-ttu-id="1df8c-134">針對 Web API 控制器，當模型驗證失敗<xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>時，MVC 會以回應類型回應。</span><span class="sxs-lookup"><span data-stu-id="1df8c-134">For web API controllers, MVC responds with a <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> response type when model validation fails.</span></span> <span data-ttu-id="1df8c-135">MVC 會使用的結果<xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>來針對驗證失敗來建立錯誤回應。</span><span class="sxs-lookup"><span data-stu-id="1df8c-135">MVC uses the results of <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to construct the error response for a validation failure.</span></span> <span data-ttu-id="1df8c-136">下列範例會使用 factory，將預設回應類型變更為<xref:Microsoft.AspNetCore.Mvc.SerializableError>中`Startup.ConfigureServices`的：</span><span class="sxs-lookup"><span data-stu-id="1df8c-136">The following example uses the factory to change the default response type to <xref:Microsoft.AspNetCore.Mvc.SerializableError> in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=4-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=5-14)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](handle-errors/samples/2.x/2.1/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=6-15)]

::: moniker-end

## <a name="client-error-response"></a><span data-ttu-id="1df8c-137">用戶端錯誤回應</span><span class="sxs-lookup"><span data-stu-id="1df8c-137">Client error response</span></span>

<span data-ttu-id="1df8c-138">*錯誤結果*會定義為 HTTP 狀態碼為400或更高的結果。</span><span class="sxs-lookup"><span data-stu-id="1df8c-138">An *error result* is defined as a result with an HTTP status code of 400 or higher.</span></span> <span data-ttu-id="1df8c-139">針對 Web API 控制器，MVC 會使用<xref:Microsoft.AspNetCore.Mvc.ProblemDetails>將錯誤結果轉換成結果。</span><span class="sxs-lookup"><span data-stu-id="1df8c-139">For web API controllers, MVC transforms an error result to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1df8c-140">您可以透過下列其中一種方式來設定錯誤回應：</span><span class="sxs-lookup"><span data-stu-id="1df8c-140">The error response can be configured in one of the following ways:</span></span>

1. [<span data-ttu-id="1df8c-141">執行 ProblemDetailsFactory</span><span class="sxs-lookup"><span data-stu-id="1df8c-141">Implement ProblemDetailsFactory</span></span>](#implement-problemdetailsfactory)
1. [<span data-ttu-id="1df8c-142">使用 ApiBehaviorOptions. ClientErrorMapping</span><span class="sxs-lookup"><span data-stu-id="1df8c-142">Use ApiBehaviorOptions.ClientErrorMapping</span></span>](#use-apibehavioroptionsclienterrormapping)

### <a name="implement-problemdetailsfactory"></a><span data-ttu-id="1df8c-143">執行 ProblemDetailsFactory</span><span class="sxs-lookup"><span data-stu-id="1df8c-143">Implement ProblemDetailsFactory</span></span>

<span data-ttu-id="1df8c-144">MVC 會`Microsoft.AspNetCore.Mvc.ProblemDetailsFactory`使用來產生和<xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>的<xref:Microsoft.AspNetCore.Mvc.ProblemDetails>所有實例。</span><span class="sxs-lookup"><span data-stu-id="1df8c-144">MVC uses `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory` to produce all instances of <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> and <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="1df8c-145">這包括用戶端錯誤回應、驗證失敗錯誤回應， `Microsoft.AspNetCore.Mvc.ControllerBase.Problem`以及和<xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> helper 方法。</span><span class="sxs-lookup"><span data-stu-id="1df8c-145">This includes client error responses, validation failure error responses, and the `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> helper methods.</span></span>

<span data-ttu-id="1df8c-146">若要自訂問題詳細資料回應，請在中`ProblemDetailsFactory` `Startup.ConfigureServices`註冊的自訂執行：</span><span class="sxs-lookup"><span data-stu-id="1df8c-146">To customize the problem details response, register a custom implementation of `ProblemDetailsFactory` in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection serviceCollection)
{
    services.AddControllers();
    services.AddTransient<ProblemDetailsFactory, CustomProblemDetailsFactory>();
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="1df8c-147">您可以如[使用 ApiBehaviorOptions. ClientErrorMapping](#use-apibehavioroptionsclienterrormapping)一節中所述，設定錯誤回應。</span><span class="sxs-lookup"><span data-stu-id="1df8c-147">The error response can be configured as outlined in the [Use ApiBehaviorOptions.ClientErrorMapping](#use-apibehavioroptionsclienterrormapping) section.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="use-apibehavioroptionsclienterrormapping"></a><span data-ttu-id="1df8c-148">使用 ApiBehaviorOptions. ClientErrorMapping</span><span class="sxs-lookup"><span data-stu-id="1df8c-148">Use ApiBehaviorOptions.ClientErrorMapping</span></span>

<span data-ttu-id="1df8c-149">使用 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> 屬性可設定 `ProblemDetails` 回應的內容。</span><span class="sxs-lookup"><span data-stu-id="1df8c-149">Use the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="1df8c-150">例如，中`Startup.ConfigureServices`的下列程式碼會更新`type` 404 回應的屬性：</span><span class="sxs-lookup"><span data-stu-id="1df8c-150">For example, the following code in `Startup.ConfigureServices` updates the `type` property for 404 responses:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8-9)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=9-10)]

::: moniker-end
