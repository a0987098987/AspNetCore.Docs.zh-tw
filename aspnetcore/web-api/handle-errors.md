---
title: 處理 ASP.NET Core web Api 中的錯誤
author: pranavkm
description: 瞭解 ASP.NET Core web Api 的錯誤處理。
monikerRange: '>= aspnetcore-2.1'
ms.author: prkrishn
ms.custom: mvc
ms.date: 09/27/2019
uid: web-api/handle-errors
ms.openlocfilehash: dc21d4b2cf096b8d38b0a24d739e6874186004e7
ms.sourcegitcommit: 5d25a7f22c50ca6fdd0f8ecd8e525822e1b35b7a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/28/2019
ms.locfileid: "71551735"
---
# <a name="handle-errors-in-aspnet-core-web-apis"></a><span data-ttu-id="cda5e-103">處理 ASP.NET Core web Api 中的錯誤</span><span class="sxs-lookup"><span data-stu-id="cda5e-103">Handle errors in ASP.NET Core web APIs</span></span>

<span data-ttu-id="cda5e-104">本文說明如何使用 ASP.NET Core web Api 處理和自訂錯誤處理。</span><span class="sxs-lookup"><span data-stu-id="cda5e-104">This article describes how to handle and customize error handling with ASP.NET Core web APIs.</span></span>

<span data-ttu-id="cda5e-105">[查看或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/handle-errors/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="cda5e-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/handle-errors/samples) ([How to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="cda5e-106">開發人員例外狀況頁面</span><span class="sxs-lookup"><span data-stu-id="cda5e-106">Developer Exception Page</span></span>

<span data-ttu-id="cda5e-107">[[開發人員例外](xref:fundamentals/error-handling)狀況] 頁面是一個實用的工具，可取得伺服器錯誤的詳細堆疊追蹤。</span><span class="sxs-lookup"><span data-stu-id="cda5e-107">The [Developer Exception Page](xref:fundamentals/error-handling) is a useful tool to get detailed stack traces for server errors.</span></span> <span data-ttu-id="cda5e-108">它會使用 <xref:Microsoft.AspNetCore.Diagnostics.DeveloperExceptionPageMiddleware>，從 HTTP 管線捕獲同步和非同步例外狀況，並產生錯誤回應。</span><span class="sxs-lookup"><span data-stu-id="cda5e-108">It uses <xref:Microsoft.AspNetCore.Diagnostics.DeveloperExceptionPageMiddleware> to capture synchronous and asynchronous exceptions from the HTTP pipeline and to generate error responses.</span></span> <span data-ttu-id="cda5e-109">為了說明，請考慮下列控制器動作：</span><span class="sxs-lookup"><span data-stu-id="cda5e-109">To illustrate, consider the following controller action:</span></span>

[!code-csharp[](handle-errors/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_GetByCity)]

<span data-ttu-id="cda5e-110">執行下列 `curl` 命令來測試前一個動作：</span><span class="sxs-lookup"><span data-stu-id="cda5e-110">Run the following `curl` command to test the preceding action:</span></span>

```bash
curl -i https://localhost:5001/weatherforecast/chicago
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="cda5e-111">在 ASP.NET Core 3.0 和更新版本中，如果用戶端不要求 HTML 格式的輸出，則開發人員例外狀況頁面會顯示純文字回應。</span><span class="sxs-lookup"><span data-stu-id="cda5e-111">In ASP.NET Core 3.0 and later, the Developer Exception Page displays a plain-text response if the client doesn't request HTML-formatted output.</span></span> <span data-ttu-id="cda5e-112">即會出現下列輸出：</span><span class="sxs-lookup"><span data-stu-id="cda5e-112">The following output appears:</span></span>

```console
HTTP/1.1 500 Internal Server Error
Transfer-Encoding: chunked
Content-Type: text/plain
Server: Microsoft-IIS/10.0
X-Powered-By: ASP.NET
Date: Fri, 27 Sep 2019 16:13:16 GMT

System.ArgumentException: We don't offer a weather forecast for chicago. (Parameter 'city')
   at WebApiSample.Controllers.WeatherForecastController.Get(String city) in C:\working_folder\aspnet\AspNetCore.Docs\aspnetcore\web-api\handle-errors\samples\3.x\Controllers\WeatherForecastController.cs:line 34
   at lambda_method(Closure , Object , Object[] )
   at Microsoft.Extensions.Internal.ObjectMethodExecutor.Execute(Object target, Object[] parameters)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ActionMethodExecutor.SyncObjectResultExecutor.Execute(IActionResultTypeMapper mapper, ObjectMethodExecutor executor, Object controller, Object[] arguments)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.<InvokeActionMethodAsync>g__Logged|12_1(ControllerActionInvoker invoker)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.<InvokeNextActionFilterAsync>g__Awaited|10_0(ControllerActionInvoker invoker, Task lastTask, State next, Scope scope, Object state, Boolean isCompleted)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.Rethrow(ActionExecutedContextSealed context)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.Next(State& next, Scope& scope, Object& state, Boolean& isCompleted)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.InvokeInnerFilterAsync()
--- End of stack trace from previous location where exception was thrown ---
   at Microsoft.AspNetCore.Mvc.Infrastructure.ResourceInvoker.<InvokeFilterPipelineAsync>g__Awaited|19_0(ResourceInvoker invoker, Task lastTask, State next, Scope scope, Object state, Boolean isCompleted)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ResourceInvoker.<InvokeAsync>g__Logged|17_1(ResourceInvoker invoker)
   at Microsoft.AspNetCore.Routing.EndpointMiddleware.<Invoke>g__AwaitRequestTask|6_0(Endpoint endpoint, Task requestTask, ILogger logger)
   at Microsoft.AspNetCore.Authorization.AuthorizationMiddleware.Invoke(HttpContext context)
   at Microsoft.AspNetCore.Diagnostics.DeveloperExceptionPageMiddleware.Invoke(HttpContext context)

HEADERS
=======
Accept: */*
Host: localhost:44312
User-Agent: curl/7.55.1
```

<span data-ttu-id="cda5e-113">若要改為顯示 HTML 格式的回應，請將 `Accept` HTTP 要求標頭設為 `text/html` 媒體類型。</span><span class="sxs-lookup"><span data-stu-id="cda5e-113">To display an HTML-formatted response instead, set the `Accept` HTTP request header to the `text/html` media type.</span></span> <span data-ttu-id="cda5e-114">例如:</span><span class="sxs-lookup"><span data-stu-id="cda5e-114">For example:</span></span>

```bash
curl -i -H "Accept: text/html" https://localhost:5001/weatherforecast/chicago
```

<span data-ttu-id="cda5e-115">請考慮下列來自 HTTP 回應的摘錄：</span><span class="sxs-lookup"><span data-stu-id="cda5e-115">Consider the following excerpt from the HTTP response:</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="cda5e-116">在 ASP.NET Core 2.2 和更早版本中，開發人員例外狀況頁面會顯示 HTML 格式的回應。</span><span class="sxs-lookup"><span data-stu-id="cda5e-116">In ASP.NET Core 2.2 and earlier, the Developer Exception Page displays an HTML-formatted response.</span></span> <span data-ttu-id="cda5e-117">例如，請考慮下列來自 HTTP 回應的摘錄：</span><span class="sxs-lookup"><span data-stu-id="cda5e-117">For example, consider the following excerpt from the HTTP response:</span></span>

::: moniker-end

```console
HTTP/1.1 500 Internal Server Error
Transfer-Encoding: chunked
Content-Type: text/html; charset=utf-8
Server: Microsoft-IIS/10.0
X-Powered-By: ASP.NET
Date: Fri, 27 Sep 2019 16:55:37 GMT

<!DOCTYPE html>
<html lang="en" xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <meta charset="utf-8" />
        <title>Internal Server Error</title>
        <style>
            body {
    font-family: 'Segoe UI', Tahoma, Arial, Helvetica, sans-serif;
    font-size: .813em;
    color: #222;
    background-color: #fff;
}
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="cda5e-118">透過 Postman 之類的工具進行測試時，HTML 格式的回應會變得很有用。</span><span class="sxs-lookup"><span data-stu-id="cda5e-118">The HTML-formatted response becomes useful when testing via tools like Postman.</span></span> <span data-ttu-id="cda5e-119">下列螢幕擷取畫面顯示 Postman 中的純文字和 HTML 格式的回應：</span><span class="sxs-lookup"><span data-stu-id="cda5e-119">The following screen capture shows both the plain-text and the HTML-formatted responses in Postman:</span></span>

![Postman 中的開發人員例外狀況頁面測試](handle-errors/_static/developer-exception-page-postman.gif)

::: moniker-end

> [!WARNING]
> <span data-ttu-id="cda5e-121">**僅有當應用程式是在開發環境中執行時**，才啟用開發人員例外狀況頁面。</span><span class="sxs-lookup"><span data-stu-id="cda5e-121">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="cda5e-122">當應用程式在生產環境中執行時，您不會想要公開共用例外狀況的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="cda5e-122">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="cda5e-123">如需設定環境的詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="cda5e-123">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

## <a name="exception-handler"></a><span data-ttu-id="cda5e-124">例外處理常式</span><span class="sxs-lookup"><span data-stu-id="cda5e-124">Exception handler</span></span>

<span data-ttu-id="cda5e-125">在非開發環境中，[例外狀況處理中介軟體](xref:fundamentals/error-handling)可以用來產生錯誤承載：</span><span class="sxs-lookup"><span data-stu-id="cda5e-125">In non-development environments, [Exception Handling Middleware](xref:fundamentals/error-handling) can be used to produce an error payload:</span></span>

1. <span data-ttu-id="cda5e-126">在`Startup.Configure`中<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> ，叫用以使用中介軟體：</span><span class="sxs-lookup"><span data-stu-id="cda5e-126">In `Startup.Configure`, invoke <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> to use the middleware:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_UseExceptionHandler&highlight=9)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_UseExceptionHandler&highlight=9)]

    ::: moniker-end

1. <span data-ttu-id="cda5e-127">設定控制器動作以回應`/error`路由：</span><span class="sxs-lookup"><span data-stu-id="cda5e-127">Configure a controller action to respond to the `/error` route:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Controllers/ErrorController.cs?name=snippet_ErrorController)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Controllers/ErrorController.cs?name=snippet_ErrorController)]

    ::: moniker-end

<span data-ttu-id="cda5e-128">上述`Error`動作會將符合[RFC7807](https://tools.ietf.org/html/rfc7807)規範的承載傳送至用戶端。</span><span class="sxs-lookup"><span data-stu-id="cda5e-128">The preceding `Error` action sends an [RFC7807](https://tools.ietf.org/html/rfc7807)-compliant payload to the client.</span></span>

<span data-ttu-id="cda5e-129">例外狀況處理中介軟體也可以在本機開發環境中提供更詳細的內容協商輸出。</span><span class="sxs-lookup"><span data-stu-id="cda5e-129">Exception Handling Middleware can also provide more detailed content-negotiated output in the local development environment.</span></span> <span data-ttu-id="cda5e-130">使用下列步驟，在開發與生產環境之間產生一致的裝載格式：</span><span class="sxs-lookup"><span data-stu-id="cda5e-130">Use the following steps to produce a consistent payload format across development and production environments:</span></span>

1. <span data-ttu-id="cda5e-131">在`Startup.Configure`中，註冊環境特定的例外狀況處理中介軟體實例：</span><span class="sxs-lookup"><span data-stu-id="cda5e-131">In `Startup.Configure`, register environment-specific Exception Handling Middleware instances:</span></span>

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

    <span data-ttu-id="cda5e-132">在上述程式碼中，中介軟體會向註冊：</span><span class="sxs-lookup"><span data-stu-id="cda5e-132">In the preceding code, the middleware is registered with:</span></span>

    * <span data-ttu-id="cda5e-133">在開發環境`/error-local-development`中的路由。</span><span class="sxs-lookup"><span data-stu-id="cda5e-133">A route of `/error-local-development` in the Development environment.</span></span>
    * <span data-ttu-id="cda5e-134">在非開發`/error`環境中的路由。</span><span class="sxs-lookup"><span data-stu-id="cda5e-134">A route of `/error` in environments that aren't Development.</span></span>
    
1. <span data-ttu-id="cda5e-135">將屬性路由套用至控制器動作：</span><span class="sxs-lookup"><span data-stu-id="cda5e-135">Apply attribute routing to controller actions:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Controllers/ErrorController.cs?name=snippet_ErrorControllerEnvironmentSpecific)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Controllers/ErrorController.cs?name=snippet_ErrorControllerEnvironmentSpecific)]

    ::: moniker-end

## <a name="use-exceptions-to-modify-the-response"></a><span data-ttu-id="cda5e-136">使用例外狀況來修改回應</span><span class="sxs-lookup"><span data-stu-id="cda5e-136">Use exceptions to modify the response</span></span>

<span data-ttu-id="cda5e-137">您可以從控制器外部修改回應的內容。</span><span class="sxs-lookup"><span data-stu-id="cda5e-137">The contents of the response can be modified from outside of the controller.</span></span> <span data-ttu-id="cda5e-138">在 ASP.NET 4.x Web API 中，其中一種方法是使用<xref:System.Web.Http.HttpResponseException>型別。</span><span class="sxs-lookup"><span data-stu-id="cda5e-138">In ASP.NET 4.x Web API, one way to do this was using the <xref:System.Web.Http.HttpResponseException> type.</span></span> <span data-ttu-id="cda5e-139">ASP.NET Core 不包含對等的類型。</span><span class="sxs-lookup"><span data-stu-id="cda5e-139">ASP.NET Core doesn't include an equivalent type.</span></span> <span data-ttu-id="cda5e-140">您可以`HttpResponseException`使用下列步驟來新增的支援：</span><span class="sxs-lookup"><span data-stu-id="cda5e-140">Support for `HttpResponseException` can be added with the following steps:</span></span>

1. <span data-ttu-id="cda5e-141">建立名為`HttpResponseException`的知名例外狀況類型：</span><span class="sxs-lookup"><span data-stu-id="cda5e-141">Create a well-known exception type named `HttpResponseException`:</span></span>

    [!code-csharp[](handle-errors/samples/3.x/Exceptions/HttpResponseException.cs?name=snippet_HttpResponseException)]

1. <span data-ttu-id="cda5e-142">建立名為`HttpResponseExceptionFilter`的動作篩選準則：</span><span class="sxs-lookup"><span data-stu-id="cda5e-142">Create an action filter named `HttpResponseExceptionFilter`:</span></span>

    [!code-csharp[](handle-errors/samples/3.x/Filters/HttpResponseExceptionFilter.cs?name=snippet_HttpResponseExceptionFilter)]

1. <span data-ttu-id="cda5e-143">在`Startup.ConfigureServices`中，將動作篩選準則新增至篩選集合：</span><span class="sxs-lookup"><span data-stu-id="cda5e-143">In `Startup.ConfigureServices`, add the action filter to the filters collection:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.2"
    
    [!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.1"

    [!code-csharp[](handle-errors/samples/2.x/2.1/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

## <a name="validation-failure-error-response"></a><span data-ttu-id="cda5e-144">驗證失敗錯誤回應</span><span class="sxs-lookup"><span data-stu-id="cda5e-144">Validation failure error response</span></span>

<span data-ttu-id="cda5e-145">針對 Web API 控制器，當模型驗證失敗<xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>時，MVC 會以回應類型回應。</span><span class="sxs-lookup"><span data-stu-id="cda5e-145">For web API controllers, MVC responds with a <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> response type when model validation fails.</span></span> <span data-ttu-id="cda5e-146">MVC 會使用的結果<xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>來針對驗證失敗來建立錯誤回應。</span><span class="sxs-lookup"><span data-stu-id="cda5e-146">MVC uses the results of <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to construct the error response for a validation failure.</span></span> <span data-ttu-id="cda5e-147">下列範例會使用 factory，將預設回應類型變更為<xref:Microsoft.AspNetCore.Mvc.SerializableError>中`Startup.ConfigureServices`的：</span><span class="sxs-lookup"><span data-stu-id="cda5e-147">The following example uses the factory to change the default response type to <xref:Microsoft.AspNetCore.Mvc.SerializableError> in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=4-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=5-14)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](handle-errors/samples/2.x/2.1/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=6-15)]

::: moniker-end

## <a name="client-error-response"></a><span data-ttu-id="cda5e-148">用戶端錯誤回應</span><span class="sxs-lookup"><span data-stu-id="cda5e-148">Client error response</span></span>

<span data-ttu-id="cda5e-149">*錯誤結果*會定義為 HTTP 狀態碼為400或更高的結果。</span><span class="sxs-lookup"><span data-stu-id="cda5e-149">An *error result* is defined as a result with an HTTP status code of 400 or higher.</span></span> <span data-ttu-id="cda5e-150">針對 Web API 控制器，MVC 會使用<xref:Microsoft.AspNetCore.Mvc.ProblemDetails>將錯誤結果轉換成結果。</span><span class="sxs-lookup"><span data-stu-id="cda5e-150">For web API controllers, MVC transforms an error result to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="cda5e-151">您可以透過下列其中一種方式來設定錯誤回應：</span><span class="sxs-lookup"><span data-stu-id="cda5e-151">The error response can be configured in one of the following ways:</span></span>

1. [<span data-ttu-id="cda5e-152">執行 ProblemDetailsFactory</span><span class="sxs-lookup"><span data-stu-id="cda5e-152">Implement ProblemDetailsFactory</span></span>](#implement-problemdetailsfactory)
1. [<span data-ttu-id="cda5e-153">使用 ApiBehaviorOptions. ClientErrorMapping</span><span class="sxs-lookup"><span data-stu-id="cda5e-153">Use ApiBehaviorOptions.ClientErrorMapping</span></span>](#use-apibehavioroptionsclienterrormapping)

### <a name="implement-problemdetailsfactory"></a><span data-ttu-id="cda5e-154">執行 ProblemDetailsFactory</span><span class="sxs-lookup"><span data-stu-id="cda5e-154">Implement ProblemDetailsFactory</span></span>

<span data-ttu-id="cda5e-155">MVC 會`Microsoft.AspNetCore.Mvc.ProblemDetailsFactory`使用來產生和<xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>的<xref:Microsoft.AspNetCore.Mvc.ProblemDetails>所有實例。</span><span class="sxs-lookup"><span data-stu-id="cda5e-155">MVC uses `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory` to produce all instances of <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> and <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="cda5e-156">這包括用戶端錯誤回應、驗證失敗錯誤回應， `Microsoft.AspNetCore.Mvc.ControllerBase.Problem`以及和<xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> helper 方法。</span><span class="sxs-lookup"><span data-stu-id="cda5e-156">This includes client error responses, validation failure error responses, and the `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> helper methods.</span></span>

<span data-ttu-id="cda5e-157">若要自訂問題詳細資料回應，請在中`ProblemDetailsFactory` `Startup.ConfigureServices`註冊的自訂執行：</span><span class="sxs-lookup"><span data-stu-id="cda5e-157">To customize the problem details response, register a custom implementation of `ProblemDetailsFactory` in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection serviceCollection)
{
    services.AddControllers();
    services.AddTransient<ProblemDetailsFactory, CustomProblemDetailsFactory>();
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="cda5e-158">您可以如[使用 ApiBehaviorOptions. ClientErrorMapping](#use-apibehavioroptionsclienterrormapping)一節中所述，設定錯誤回應。</span><span class="sxs-lookup"><span data-stu-id="cda5e-158">The error response can be configured as outlined in the [Use ApiBehaviorOptions.ClientErrorMapping](#use-apibehavioroptionsclienterrormapping) section.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="use-apibehavioroptionsclienterrormapping"></a><span data-ttu-id="cda5e-159">使用 ApiBehaviorOptions. ClientErrorMapping</span><span class="sxs-lookup"><span data-stu-id="cda5e-159">Use ApiBehaviorOptions.ClientErrorMapping</span></span>

<span data-ttu-id="cda5e-160">使用 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> 屬性可設定 `ProblemDetails` 回應的內容。</span><span class="sxs-lookup"><span data-stu-id="cda5e-160">Use the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="cda5e-161">例如，中`Startup.ConfigureServices`的下列程式碼會更新`type` 404 回應的屬性：</span><span class="sxs-lookup"><span data-stu-id="cda5e-161">For example, the following code in `Startup.ConfigureServices` updates the `type` property for 404 responses:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8-9)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=9-10)]

::: moniker-end
