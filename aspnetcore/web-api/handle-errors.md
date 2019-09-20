---
title: 處理 ASP.NET Core web Api 中的錯誤
author: pranavkm
description: 瞭解 ASP.NET Core web Api 的錯誤處理。
monikerRange: '>= aspnetcore-2.1'
ms.author: prkrishn
ms.custom: mvc
ms.date: 09/18/2019
uid: web-api/handle-errors
ms.openlocfilehash: bf8229d37ef15c42b5d7bb4a12737ac65d7b753c
ms.sourcegitcommit: a11f09c10ef3d4eeab7ae9ce993e7f30427741c1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150905"
---
# <a name="handle-errors-in-aspnet-core-web-apis"></a><span data-ttu-id="96583-103">處理 ASP.NET Core web Api 中的錯誤</span><span class="sxs-lookup"><span data-stu-id="96583-103">Handle errors in ASP.NET Core web APIs</span></span>

<span data-ttu-id="96583-104">本文說明如何使用 ASP.NET Core web Api 處理和自訂錯誤處理。</span><span class="sxs-lookup"><span data-stu-id="96583-104">This article describes how to handle and customize error handling with ASP.NET Core web APIs.</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="96583-105">開發人員例外狀況頁面</span><span class="sxs-lookup"><span data-stu-id="96583-105">Developer Exception Page</span></span>

<span data-ttu-id="96583-106">[[開發人員例外](xref:fundamentals/error-handling)狀況] 頁面是一個實用的工具，可取得伺服器錯誤的詳細堆疊追蹤。</span><span class="sxs-lookup"><span data-stu-id="96583-106">The [Developer Exception Page](xref:fundamentals/error-handling) is a useful tool to get detailed stack traces for server errors.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="96583-107">如果用戶端不接受 HTML 格式的輸出，則開發人員例外狀況頁面會顯示純文字回應：</span><span class="sxs-lookup"><span data-stu-id="96583-107">The Developer Exception Page displays a plain-text response if the client doesn't accept HTML-formatted output:</span></span>

```
> curl https://localhost:5001/weatherforecast
System.ArgumentException: count
   at errorhandling.Controllers.WeatherForecastController.Get(Int32 x) in D:\work\Samples\samples\aspnetcore\mvc\errorhandling\Controllers\WeatherForecastController.cs:line 35
   at lambda_method(Closure , Object , Object[] )
   at Microsoft.Extensions.Internal.ObjectMethodExecutor.Execute(Object target, Object[] parameters)
...
```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="96583-108">**僅有當應用程式是在開發環境中執行時**，才啟用開發人員例外狀況頁面。</span><span class="sxs-lookup"><span data-stu-id="96583-108">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="96583-109">當應用程式在生產環境中執行時，您不會想要公開共用例外狀況的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="96583-109">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="96583-110">如需設定環境的詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="96583-110">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

## <a name="exception-handler"></a><span data-ttu-id="96583-111">例外處理常式</span><span class="sxs-lookup"><span data-stu-id="96583-111">Exception handler</span></span>

<span data-ttu-id="96583-112">在非開發環境中，[例外狀況處理中介軟體](xref:fundamentals/error-handling)可以用來產生錯誤承載：</span><span class="sxs-lookup"><span data-stu-id="96583-112">In non-development environments, [Exception Handling Middleware](xref:fundamentals/error-handling) can be used to produce an error payload:</span></span>

1. <span data-ttu-id="96583-113">在`Startup.Configure`中<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> ，叫用以使用中介軟體：</span><span class="sxs-lookup"><span data-stu-id="96583-113">In `Startup.Configure`, invoke <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> to use the middleware:</span></span>

    ```csharp
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }
        else
        {
            app.UseExceptionHandler("/error");
        }
    }
    ```

1. <span data-ttu-id="96583-114">設定控制器動作以回應`/error`路由：</span><span class="sxs-lookup"><span data-stu-id="96583-114">Configure a controller action to respond to the `/error` route:</span></span>

    ```csharp
    [ApiController]
    public class ErrorController : ControllerBase
    {
        [Route("/error")]
        public IActionResult Error() => Problem();
    }
    ```

<span data-ttu-id="96583-115">上述`Error`動作會將符合[RFC7807](https://tools.ietf.org/html/rfc7807)規範的承載傳送至用戶端。</span><span class="sxs-lookup"><span data-stu-id="96583-115">The preceding `Error` action sends an [RFC7807](https://tools.ietf.org/html/rfc7807)-compliant payload to the client.</span></span>

<span data-ttu-id="96583-116">例外狀況處理中介軟體也可以在本機開發環境中提供更詳細的內容協商輸出。</span><span class="sxs-lookup"><span data-stu-id="96583-116">Exception Handling Middleware can also provide more detailed content-negotiated output in the local development environment.</span></span> <span data-ttu-id="96583-117">使用下列步驟，在開發與生產環境之間產生一致的裝載格式：</span><span class="sxs-lookup"><span data-stu-id="96583-117">Use the following steps to produce a consistent payload format across development and production environments:</span></span>

1. <span data-ttu-id="96583-118">在`Startup.Configure`中，註冊環境特定的例外狀況處理中介軟體實例：</span><span class="sxs-lookup"><span data-stu-id="96583-118">In `Startup.Configure`, register environment-specific Exception Handling Middleware instances:</span></span>

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

    <span data-ttu-id="96583-119">在上述程式碼中，中介軟體會向註冊：</span><span class="sxs-lookup"><span data-stu-id="96583-119">In the preceding code, the middleware is registered with:</span></span>

    * <span data-ttu-id="96583-120">在開發環境`/error-local-development`中的路由。</span><span class="sxs-lookup"><span data-stu-id="96583-120">A route of `/error-local-development` in the Development environment.</span></span>
    * <span data-ttu-id="96583-121">在非開發`/error`環境中的路由。</span><span class="sxs-lookup"><span data-stu-id="96583-121">A route of `/error` in environments that aren't Development.</span></span>
    
1. <span data-ttu-id="96583-122">將屬性路由套用至控制器動作：</span><span class="sxs-lookup"><span data-stu-id="96583-122">Apply attribute routing to controller actions:</span></span>

    ```csharp
    [ApiController]
    public class ErrorController : ControllerBase
    {
        [Route("/error-local-development")]
        public IActionResult ErrorLocalDevelopment(
            [FromServices] IWebHostEnvironment webHostEnvironment)
        {
            if (!webHostEnvironment.IsDevelopment())
            {
                throw new InvalidOperationException(
                    "This shouldn't be invoked in non-development environments.");
            }
    
            var context = HttpContext.Features.Get<IExceptionHandlerFeature>();

            return Problem(
                detail: context.Error.StackTrace,
                title: context.Error.Message);
        }
    
        [Route("/error")]
        public IActionResult Error() => Problem();
    }
    ```

## <a name="use-exceptions-to-modify-the-response"></a><span data-ttu-id="96583-123">使用例外狀況來修改回應</span><span class="sxs-lookup"><span data-stu-id="96583-123">Use exceptions to modify the response</span></span>

<span data-ttu-id="96583-124">您可以從控制器外部修改回應的內容。</span><span class="sxs-lookup"><span data-stu-id="96583-124">The contents of the response can be modified from outside of the controller.</span></span> <span data-ttu-id="96583-125">在 ASP.NET 4.x Web API 中，其中一種方法是使用<xref:System.Web.Http.HttpResponseException>型別。</span><span class="sxs-lookup"><span data-stu-id="96583-125">In ASP.NET 4.x Web API, one way to do this was using the <xref:System.Web.Http.HttpResponseException> type.</span></span> <span data-ttu-id="96583-126">ASP.NET Core 不包含對等的類型。</span><span class="sxs-lookup"><span data-stu-id="96583-126">ASP.NET Core doesn't include an equivalent type.</span></span> <span data-ttu-id="96583-127">您可以`HttpResponseException`使用下列步驟來新增的支援：</span><span class="sxs-lookup"><span data-stu-id="96583-127">Support for `HttpResponseException` can be added with the following steps:</span></span>

1. <span data-ttu-id="96583-128">建立名為`HttpResponseException`的知名例外狀況類型：</span><span class="sxs-lookup"><span data-stu-id="96583-128">Create a well-known exception type named `HttpResponseException`:</span></span>

    ```csharp
    public class HttpResponseException : Exception
    {
        public int Status { get; set; } = 500;
    
        public object Value { get; set; }
    }
    ```

1. <span data-ttu-id="96583-129">建立名為`HttpResponseExceptionFilter`的動作篩選準則：</span><span class="sxs-lookup"><span data-stu-id="96583-129">Create an action filter named `HttpResponseExceptionFilter`:</span></span>

    ```csharp
    public class HttpResponseExceptionFilter : IActionFilter, IOrderedFilter
    {
        public int Order { get; set; } = int.MaxValue - 10;
    
        public void OnActionExecuting(ActionExecutingContext context) {}
    
        public void OnActionExecuted(ActionExecutedContext context)
        {
            if (context.Exception is HttpResponseException exception)
            {
                context.Result = new ObjectResult(exception.Value)
                {
                    Status = exception.Status,
                };
                context.ExceptionHandled = true;
            }
        }
    }
    ```

1. <span data-ttu-id="96583-130">在`Startup.ConfigureServices`中，將動作篩選準則新增至篩選集合：</span><span class="sxs-lookup"><span data-stu-id="96583-130">In `Startup.ConfigureServices`, add the action filter to the filters collection:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddControllers(options => 
            options.Filters.Add(new HttpResponseExceptionFilter()));
    }
    ```

## <a name="validation-failure-error-response"></a><span data-ttu-id="96583-131">驗證失敗錯誤回應</span><span class="sxs-lookup"><span data-stu-id="96583-131">Validation failure error response</span></span>

<span data-ttu-id="96583-132">針對 Web API 控制器，當模型驗證失敗<xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>時，MVC 會以回應類型回應。</span><span class="sxs-lookup"><span data-stu-id="96583-132">For web API controllers, MVC responds with a <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> response type when model validation fails.</span></span> <span data-ttu-id="96583-133">MVC 會使用的結果<xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>來針對驗證失敗來建立錯誤回應。</span><span class="sxs-lookup"><span data-stu-id="96583-133">MVC uses the results of <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to construct the error response for a validation failure.</span></span> <span data-ttu-id="96583-134">下列範例會使用 factory，將預設回應類型變更為<xref:Microsoft.AspNetCore.Mvc.SerializableError>中`Startup.ConfigureServices`的：</span><span class="sxs-lookup"><span data-stu-id="96583-134">The following example uses the factory to change the default response type to <xref:Microsoft.AspNetCore.Mvc.SerializableError> in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=4-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=5-14)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);

services.Configure<ApiBehaviorOptions>(options =>
{
    options.InvalidModelStateResponseFactory = context =>
    {
        var result = new BadRequestObjectResult(context.ModelState);

        // TODO: add `using using System.Net.Mime;` to resolve MediaTypeNames
        result.ContentTypes.Add(MediaTypeNames.Application.Json);
        result.ContentTypes.Add(MediaTypeNames.Application.Xml);

        return result;
    };
});
```

::: moniker-end

## <a name="client-error-response"></a><span data-ttu-id="96583-135">用戶端錯誤回應</span><span class="sxs-lookup"><span data-stu-id="96583-135">Client error response</span></span>

<span data-ttu-id="96583-136">*錯誤結果*會定義為 HTTP 狀態碼為400或更高的結果。</span><span class="sxs-lookup"><span data-stu-id="96583-136">An *error result* is defined as a result with an HTTP status code of 400 or higher.</span></span> <span data-ttu-id="96583-137">針對 Web API 控制器，MVC 會使用<xref:Microsoft.AspNetCore.Mvc.ProblemDetails>將錯誤結果轉換成結果。</span><span class="sxs-lookup"><span data-stu-id="96583-137">For web API controllers, MVC transforms an error result to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="96583-138">您可以透過下列其中一種方式來設定錯誤回應：</span><span class="sxs-lookup"><span data-stu-id="96583-138">The error response can be configured in one of the following ways:</span></span>

1. [<span data-ttu-id="96583-139">執行 ProblemDetailsFactory</span><span class="sxs-lookup"><span data-stu-id="96583-139">Implement ProblemDetailsFactory</span></span>](#implement-problemdetailsfactory)
1. [<span data-ttu-id="96583-140">使用 ApiBehaviorOptions. ClientErrorMapping</span><span class="sxs-lookup"><span data-stu-id="96583-140">Use ApiBehaviorOptions.ClientErrorMapping</span></span>](#use-apibehavioroptionsclienterrormapping)

### <a name="implement-problemdetailsfactory"></a><span data-ttu-id="96583-141">執行 ProblemDetailsFactory</span><span class="sxs-lookup"><span data-stu-id="96583-141">Implement ProblemDetailsFactory</span></span>

<span data-ttu-id="96583-142">MVC 會`Microsoft.AspNetCore.Mvc.ProblemDetailsFactory`使用來產生和<xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>的<xref:Microsoft.AspNetCore.Mvc.ProblemDetails>所有實例。</span><span class="sxs-lookup"><span data-stu-id="96583-142">MVC uses `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory` to produce all instances of <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> and <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="96583-143">這包括用戶端錯誤回應、驗證失敗錯誤回應， `Microsoft.AspNetCore.Mvc.ControllerBase.Problem`以及和<xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> helper 方法。</span><span class="sxs-lookup"><span data-stu-id="96583-143">This includes client error responses, validation failure error responses, and the `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> helper methods.</span></span>

<span data-ttu-id="96583-144">若要自訂問題詳細資料回應，請在中`ProblemDetailsFactory` `Startup.ConfigureServices`註冊的自訂執行：</span><span class="sxs-lookup"><span data-stu-id="96583-144">To customize the problem details response, register a custom implementation of `ProblemDetailsFactory` in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection serviceCollection)
{
    services.AddControllers();
    services.AddTransient<ProblemDetailsFactory, CustomProblemDetailsFactory>();
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="96583-145">您可以如[使用 ApiBehaviorOptions. ClientErrorMapping](#use-apibehavioroptionsclienterrormapping)一節中所述，設定錯誤回應。</span><span class="sxs-lookup"><span data-stu-id="96583-145">The error response can be configured as outlined in the [Use ApiBehaviorOptions.ClientErrorMapping](#use-apibehavioroptionsclienterrormapping) section.</span></span>

::: moniker-end

### <a name="use-apibehavioroptionsclienterrormapping"></a><span data-ttu-id="96583-146">使用 ApiBehaviorOptions. ClientErrorMapping</span><span class="sxs-lookup"><span data-stu-id="96583-146">Use ApiBehaviorOptions.ClientErrorMapping</span></span>

<span data-ttu-id="96583-147">使用 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> 屬性可設定 `ProblemDetails` 回應的內容。</span><span class="sxs-lookup"><span data-stu-id="96583-147">Use the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="96583-148">舉例來說，下列程式碼會更新 404 回應的 `type` 屬性：</span><span class="sxs-lookup"><span data-stu-id="96583-148">For example, the following code updates the `type` property for 404 responses:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8-9)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=9-10)]

::: moniker-end
