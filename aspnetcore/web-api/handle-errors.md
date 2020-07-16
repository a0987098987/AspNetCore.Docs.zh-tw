---
title: 處理 ASP.NET Core web Api 中的錯誤
author: pranavkm
description: 瞭解 ASP.NET Core web Api 的錯誤處理。
monikerRange: '>= aspnetcore-2.1'
ms.author: prkrishn
ms.custom: mvc
ms.date: 12/10/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: web-api/handle-errors
ms.openlocfilehash: 0abb5e78e1971925c8e741386c65bdf71a0f0072
ms.sourcegitcommit: 6fb27ea41a92f6d0e91dfd0eba905d2ac1a707f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/15/2020
ms.locfileid: "86407628"
---
# <a name="handle-errors-in-aspnet-core-web-apis"></a><span data-ttu-id="ee019-103">處理 ASP.NET Core web Api 中的錯誤</span><span class="sxs-lookup"><span data-stu-id="ee019-103">Handle errors in ASP.NET Core web APIs</span></span>

<span data-ttu-id="ee019-104">本文說明如何使用 ASP.NET Core web Api 處理和自訂錯誤處理。</span><span class="sxs-lookup"><span data-stu-id="ee019-104">This article describes how to handle and customize error handling with ASP.NET Core web APIs.</span></span>

<span data-ttu-id="ee019-105">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/handle-errors/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="ee019-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/handle-errors/samples) ([How to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="ee019-106">開發人員例外狀況頁面</span><span class="sxs-lookup"><span data-stu-id="ee019-106">Developer Exception Page</span></span>

<span data-ttu-id="ee019-107">[[開發人員例外](xref:fundamentals/error-handling)狀況] 頁面是一個實用的工具，可取得伺服器錯誤的詳細堆疊追蹤。</span><span class="sxs-lookup"><span data-stu-id="ee019-107">The [Developer Exception Page](xref:fundamentals/error-handling) is a useful tool to get detailed stack traces for server errors.</span></span> <span data-ttu-id="ee019-108">它會使用 <xref:Microsoft.AspNetCore.Diagnostics.DeveloperExceptionPageMiddleware> 從 HTTP 管線捕獲同步和非同步例外狀況，並產生錯誤回應。</span><span class="sxs-lookup"><span data-stu-id="ee019-108">It uses <xref:Microsoft.AspNetCore.Diagnostics.DeveloperExceptionPageMiddleware> to capture synchronous and asynchronous exceptions from the HTTP pipeline and to generate error responses.</span></span> <span data-ttu-id="ee019-109">為了說明，請考慮下列控制器動作：</span><span class="sxs-lookup"><span data-stu-id="ee019-109">To illustrate, consider the following controller action:</span></span>

[!code-csharp[](handle-errors/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_GetByCity)]

<span data-ttu-id="ee019-110">執行下列 `curl` 命令來測試前一個動作：</span><span class="sxs-lookup"><span data-stu-id="ee019-110">Run the following `curl` command to test the preceding action:</span></span>

```bash
curl -i https://localhost:5001/weatherforecast/chicago
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ee019-111">在 ASP.NET Core 3.0 和更新版本中，如果用戶端不要求 HTML 格式的輸出，則開發人員例外狀況頁面會顯示純文字回應。</span><span class="sxs-lookup"><span data-stu-id="ee019-111">In ASP.NET Core 3.0 and later, the Developer Exception Page displays a plain-text response if the client doesn't request HTML-formatted output.</span></span> <span data-ttu-id="ee019-112">下列輸出會出現：</span><span class="sxs-lookup"><span data-stu-id="ee019-112">The following output appears:</span></span>

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

<span data-ttu-id="ee019-113">若要改為顯示 HTML 格式的回應，請將 `Accept` HTTP 要求標頭設定為 `text/html` 媒體類型。</span><span class="sxs-lookup"><span data-stu-id="ee019-113">To display an HTML-formatted response instead, set the `Accept` HTTP request header to the `text/html` media type.</span></span> <span data-ttu-id="ee019-114">例如：</span><span class="sxs-lookup"><span data-stu-id="ee019-114">For example:</span></span>

```bash
curl -i -H "Accept: text/html" https://localhost:5001/weatherforecast/chicago
```

<span data-ttu-id="ee019-115">請考慮下列來自 HTTP 回應的摘錄：</span><span class="sxs-lookup"><span data-stu-id="ee019-115">Consider the following excerpt from the HTTP response:</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="ee019-116">在 ASP.NET Core 2.2 和更早版本中，開發人員例外狀況頁面會顯示 HTML 格式的回應。</span><span class="sxs-lookup"><span data-stu-id="ee019-116">In ASP.NET Core 2.2 and earlier, the Developer Exception Page displays an HTML-formatted response.</span></span> <span data-ttu-id="ee019-117">例如，請考慮下列來自 HTTP 回應的摘錄：</span><span class="sxs-lookup"><span data-stu-id="ee019-117">For example, consider the following excerpt from the HTTP response:</span></span>

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

<span data-ttu-id="ee019-118">透過 Postman 之類的工具進行測試時，HTML 格式的回應會變得很有用。</span><span class="sxs-lookup"><span data-stu-id="ee019-118">The HTML-formatted response becomes useful when testing via tools like Postman.</span></span> <span data-ttu-id="ee019-119">下列螢幕擷取畫面顯示 Postman 中的純文字和 HTML 格式的回應：</span><span class="sxs-lookup"><span data-stu-id="ee019-119">The following screen capture shows both the plain-text and the HTML-formatted responses in Postman:</span></span>

![Postman 中的開發人員例外狀況頁面測試](handle-errors/_static/developer-exception-page-postman.gif)

::: moniker-end

> [!WARNING]
> <span data-ttu-id="ee019-121">**只有當應用程式在開發環境中執行時，才**啟用 [開發人員例外狀況] 頁面。</span><span class="sxs-lookup"><span data-stu-id="ee019-121">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="ee019-122">當應用程式在生產環境中執行時，您不會想要公開共用例外狀況的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="ee019-122">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="ee019-123">如需設定環境的詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="ee019-123">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

## <a name="exception-handler"></a><span data-ttu-id="ee019-124">例外處理常式</span><span class="sxs-lookup"><span data-stu-id="ee019-124">Exception handler</span></span>

<span data-ttu-id="ee019-125">在非開發環境中，[例外狀況處理中介軟體](xref:fundamentals/error-handling)可以用來產生錯誤承載：</span><span class="sxs-lookup"><span data-stu-id="ee019-125">In non-development environments, [Exception Handling Middleware](xref:fundamentals/error-handling) can be used to produce an error payload:</span></span>

1. <span data-ttu-id="ee019-126">在中 `Startup.Configure` ，叫 <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler%2A> 用以使用中介軟體：</span><span class="sxs-lookup"><span data-stu-id="ee019-126">In `Startup.Configure`, invoke <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler%2A> to use the middleware:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_UseExceptionHandler&highlight=9)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_UseExceptionHandler&highlight=9)]

    ::: moniker-end

1. <span data-ttu-id="ee019-127">設定控制器動作以回應 `/error` 路由：</span><span class="sxs-lookup"><span data-stu-id="ee019-127">Configure a controller action to respond to the `/error` route:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Controllers/ErrorController.cs?name=snippet_ErrorController)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Controllers/ErrorController.cs?name=snippet_ErrorController)]

    ::: moniker-end

<span data-ttu-id="ee019-128">上述 `Error` 動作會將[RFC 7807](https://tools.ietf.org/html/rfc7807)相容的承載傳送至用戶端。</span><span class="sxs-lookup"><span data-stu-id="ee019-128">The preceding `Error` action sends an [RFC 7807](https://tools.ietf.org/html/rfc7807)-compliant payload to the client.</span></span>

<span data-ttu-id="ee019-129">例外狀況處理中介軟體也可以在本機開發環境中提供更詳細的內容協商輸出。</span><span class="sxs-lookup"><span data-stu-id="ee019-129">Exception Handling Middleware can also provide more detailed content-negotiated output in the local development environment.</span></span> <span data-ttu-id="ee019-130">使用下列步驟，在開發與生產環境之間產生一致的裝載格式：</span><span class="sxs-lookup"><span data-stu-id="ee019-130">Use the following steps to produce a consistent payload format across development and production environments:</span></span>

1. <span data-ttu-id="ee019-131">在中 `Startup.Configure` ，註冊環境特定的例外狀況處理中介軟體實例：</span><span class="sxs-lookup"><span data-stu-id="ee019-131">In `Startup.Configure`, register environment-specific Exception Handling Middleware instances:</span></span>

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

    <span data-ttu-id="ee019-132">在上述程式碼中，中介軟體會向註冊：</span><span class="sxs-lookup"><span data-stu-id="ee019-132">In the preceding code, the middleware is registered with:</span></span>

    * <span data-ttu-id="ee019-133">`/error-local-development`在開發環境中的路由。</span><span class="sxs-lookup"><span data-stu-id="ee019-133">A route of `/error-local-development` in the Development environment.</span></span>
    * <span data-ttu-id="ee019-134">`/error`在非開發環境中的路由。</span><span class="sxs-lookup"><span data-stu-id="ee019-134">A route of `/error` in environments that aren't Development.</span></span>
    
1. <span data-ttu-id="ee019-135">將屬性路由套用至控制器動作：</span><span class="sxs-lookup"><span data-stu-id="ee019-135">Apply attribute routing to controller actions:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Controllers/ErrorController.cs?name=snippet_ErrorControllerEnvironmentSpecific)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Controllers/ErrorController.cs?name=snippet_ErrorControllerEnvironmentSpecific)]

    ::: moniker-end

## <a name="use-exceptions-to-modify-the-response"></a><span data-ttu-id="ee019-136">使用例外狀況來修改回應</span><span class="sxs-lookup"><span data-stu-id="ee019-136">Use exceptions to modify the response</span></span>

<span data-ttu-id="ee019-137">您可以從控制器外部修改回應的內容。</span><span class="sxs-lookup"><span data-stu-id="ee019-137">The contents of the response can be modified from outside of the controller.</span></span> <span data-ttu-id="ee019-138">在 ASP.NET 4.x Web API 中，其中一種方法是使用 <xref:System.Web.Http.HttpResponseException> 型別。</span><span class="sxs-lookup"><span data-stu-id="ee019-138">In ASP.NET 4.x Web API, one way to do this was using the <xref:System.Web.Http.HttpResponseException> type.</span></span> <span data-ttu-id="ee019-139">ASP.NET Core 不包含對等的類型。</span><span class="sxs-lookup"><span data-stu-id="ee019-139">ASP.NET Core doesn't include an equivalent type.</span></span> <span data-ttu-id="ee019-140">您 `HttpResponseException` 可以使用下列步驟來新增的支援：</span><span class="sxs-lookup"><span data-stu-id="ee019-140">Support for `HttpResponseException` can be added with the following steps:</span></span>

1. <span data-ttu-id="ee019-141">建立名為的知名例外狀況類型 `HttpResponseException` ：</span><span class="sxs-lookup"><span data-stu-id="ee019-141">Create a well-known exception type named `HttpResponseException`:</span></span>

    [!code-csharp[](handle-errors/samples/3.x/Exceptions/HttpResponseException.cs?name=snippet_HttpResponseException)]

1. <span data-ttu-id="ee019-142">建立名為的動作篩選準則 `HttpResponseExceptionFilter` ：</span><span class="sxs-lookup"><span data-stu-id="ee019-142">Create an action filter named `HttpResponseExceptionFilter`:</span></span>

    [!code-csharp[](handle-errors/samples/3.x/Filters/HttpResponseExceptionFilter.cs?name=snippet_HttpResponseExceptionFilter)]

1. <span data-ttu-id="ee019-143">在中 `Startup.ConfigureServices` ，將動作篩選準則新增至篩選集合：</span><span class="sxs-lookup"><span data-stu-id="ee019-143">In `Startup.ConfigureServices`, add the action filter to the filters collection:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.2"
    
    [!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.1"

    [!code-csharp[](handle-errors/samples/2.x/2.1/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

## <a name="validation-failure-error-response"></a><span data-ttu-id="ee019-144">驗證失敗錯誤回應</span><span class="sxs-lookup"><span data-stu-id="ee019-144">Validation failure error response</span></span>

<span data-ttu-id="ee019-145">針對 Web API 控制器， <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> 當模型驗證失敗時，MVC 會以回應類型回應。</span><span class="sxs-lookup"><span data-stu-id="ee019-145">For web API controllers, MVC responds with a <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> response type when model validation fails.</span></span> <span data-ttu-id="ee019-146">MVC 會使用的結果 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> 來針對驗證失敗來建立錯誤回應。</span><span class="sxs-lookup"><span data-stu-id="ee019-146">MVC uses the results of <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to construct the error response for a validation failure.</span></span> <span data-ttu-id="ee019-147">下列範例會使用 factory，將預設回應類型變更為 <xref:Microsoft.AspNetCore.Mvc.SerializableError> 中的 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="ee019-147">The following example uses the factory to change the default response type to <xref:Microsoft.AspNetCore.Mvc.SerializableError> in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=4-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=5-14)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](handle-errors/samples/2.x/2.1/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=6-15)]

::: moniker-end

## <a name="client-error-response"></a><span data-ttu-id="ee019-148">用戶端錯誤回應</span><span class="sxs-lookup"><span data-stu-id="ee019-148">Client error response</span></span>

<span data-ttu-id="ee019-149">*錯誤結果*會定義為 HTTP 狀態碼為400或更高的結果。</span><span class="sxs-lookup"><span data-stu-id="ee019-149">An *error result* is defined as a result with an HTTP status code of 400 or higher.</span></span> <span data-ttu-id="ee019-150">針對 Web API 控制器，MVC 會使用將錯誤結果轉換成結果 <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> 。</span><span class="sxs-lookup"><span data-stu-id="ee019-150">For web API controllers, MVC transforms an error result to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span>

::: moniker range="= aspnetcore-2.1"

> [!IMPORTANT]
> <span data-ttu-id="ee019-151">ASP.NET Core 2.1 會產生幾乎符合 RFC 7807 標準的問題詳細資料回應。</span><span class="sxs-lookup"><span data-stu-id="ee019-151">ASP.NET Core 2.1 generates a problem details response that's nearly RFC 7807-compliant.</span></span> <span data-ttu-id="ee019-152">如果符合100% 的合規性，請將專案升級至 ASP.NET Core 2.2 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="ee019-152">If 100 percent compliance is important, upgrade the project to ASP.NET Core 2.2 or later.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ee019-153">您可以透過下列其中一種方式來設定錯誤回應：</span><span class="sxs-lookup"><span data-stu-id="ee019-153">The error response can be configured in one of the following ways:</span></span>

1. [<span data-ttu-id="ee019-154">執行 ProblemDetailsFactory</span><span class="sxs-lookup"><span data-stu-id="ee019-154">Implement ProblemDetailsFactory</span></span>](#implement-problemdetailsfactory)
1. [<span data-ttu-id="ee019-155">使用 ApiBehaviorOptions. ClientErrorMapping</span><span class="sxs-lookup"><span data-stu-id="ee019-155">Use ApiBehaviorOptions.ClientErrorMapping</span></span>](#use-apibehavioroptionsclienterrormapping)

### <a name="implement-problemdetailsfactory"></a><span data-ttu-id="ee019-156">實作 `ProblemDetailsFactory`</span><span class="sxs-lookup"><span data-stu-id="ee019-156">Implement `ProblemDetailsFactory`</span></span>

<span data-ttu-id="ee019-157">MVC 會使用 <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ProblemDetailsFactory?displayProperty=fullName> 來產生和的所有實例 <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> 。</span><span class="sxs-lookup"><span data-stu-id="ee019-157">MVC uses <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ProblemDetailsFactory?displayProperty=fullName> to produce all instances of <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> and <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="ee019-158">這包括用戶端錯誤回應、驗證失敗錯誤回應，以及 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Problem%2A?displayProperty=nameWithType> 和 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem%2A?displayProperty=nameWithType> helper 方法。</span><span class="sxs-lookup"><span data-stu-id="ee019-158">This includes client error responses, validation failure error responses, and the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Problem%2A?displayProperty=nameWithType> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem%2A?displayProperty=nameWithType> helper methods.</span></span>

<span data-ttu-id="ee019-159">若要自訂問題詳細資料回應，請在中註冊的自訂執行 <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ProblemDetailsFactory> `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="ee019-159">To customize the problem details response, register a custom implementation of <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ProblemDetailsFactory> in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection serviceCollection)
{
    services.AddControllers();
    services.AddTransient<ProblemDetailsFactory, CustomProblemDetailsFactory>();
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="ee019-160">您可以如[使用 ApiBehaviorOptions. ClientErrorMapping](#use-apibehavioroptionsclienterrormapping)一節中所述，設定錯誤回應。</span><span class="sxs-lookup"><span data-stu-id="ee019-160">The error response can be configured as outlined in the [Use ApiBehaviorOptions.ClientErrorMapping](#use-apibehavioroptionsclienterrormapping) section.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="use-apibehavioroptionsclienterrormapping"></a><span data-ttu-id="ee019-161">使用 ApiBehaviorOptions. ClientErrorMapping</span><span class="sxs-lookup"><span data-stu-id="ee019-161">Use ApiBehaviorOptions.ClientErrorMapping</span></span>

<span data-ttu-id="ee019-162">使用 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping%2A> 屬性可設定 `ProblemDetails` 回應的內容。</span><span class="sxs-lookup"><span data-stu-id="ee019-162">Use the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping%2A> property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="ee019-163">例如，中的下列程式碼會 `Startup.ConfigureServices` 更新 `type` 404 回應的屬性：</span><span class="sxs-lookup"><span data-stu-id="ee019-163">For example, the following code in `Startup.ConfigureServices` updates the `type` property for 404 responses:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8-9)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=9-10)]

::: moniker-end
