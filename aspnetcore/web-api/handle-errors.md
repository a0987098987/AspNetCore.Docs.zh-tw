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
# <a name="handle-errors-in-aspnet-core-web-apis"></a>處理 ASP.NET Core web Api 中的錯誤

本文說明如何使用 ASP.NET Core web Api 處理和自訂錯誤處理。

[查看或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/handle-errors/samples)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="developer-exception-page"></a>開發人員例外狀況頁面

[[開發人員例外](xref:fundamentals/error-handling)狀況] 頁面是一個實用的工具，可取得伺服器錯誤的詳細堆疊追蹤。

如果用戶端不接受 HTML 格式的輸出，則開發人員例外狀況頁面會顯示純文字回應。 例如：

```
> curl https://localhost:5001/weatherforecast
System.ArgumentException: count
   at errorhandling.Controllers.WeatherForecastController.Get(Int32 x) in D:\work\Samples\samples\aspnetcore\mvc\errorhandling\Controllers\WeatherForecastController.cs:line 35
   at lambda_method(Closure , Object , Object[] )
   at Microsoft.Extensions.Internal.ObjectMethodExecutor.Execute(Object target, Object[] parameters)
...
```

> [!WARNING]
> **僅有當應用程式是在開發環境中執行時**，才啟用開發人員例外狀況頁面。 當應用程式在生產環境中執行時，您不會想要公開共用例外狀況的詳細資訊。 如需設定環境的詳細資訊，請參閱<xref:fundamentals/environments>。

## <a name="exception-handler"></a>例外處理常式

在非開發環境中，[例外狀況處理中介軟體](xref:fundamentals/error-handling)可以用來產生錯誤承載：

1. 在`Startup.Configure`中<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> ，叫用以使用中介軟體：

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_UseExceptionHandler&highlight=9)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_UseExceptionHandler&highlight=9)]

    ::: moniker-end

1. 設定控制器動作以回應`/error`路由：

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Controllers/ErrorController.cs?name=snippet_ErrorController)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Controllers/ErrorController.cs?name=snippet_ErrorController)]

    ::: moniker-end

上述`Error`動作會將符合[RFC7807](https://tools.ietf.org/html/rfc7807)規範的承載傳送至用戶端。

例外狀況處理中介軟體也可以在本機開發環境中提供更詳細的內容協商輸出。 使用下列步驟，在開發與生產環境之間產生一致的裝載格式：

1. 在`Startup.Configure`中，註冊環境特定的例外狀況處理中介軟體實例：

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

    在上述程式碼中，中介軟體會向註冊：

    * 在開發環境`/error-local-development`中的路由。
    * 在非開發`/error`環境中的路由。
    
1. 將屬性路由套用至控制器動作：

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Controllers/ErrorController.cs?name=snippet_ErrorControllerEnvironmentSpecific)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Controllers/ErrorController.cs?name=snippet_ErrorControllerEnvironmentSpecific)]

    ::: moniker-end

## <a name="use-exceptions-to-modify-the-response"></a>使用例外狀況來修改回應

您可以從控制器外部修改回應的內容。 在 ASP.NET 4.x Web API 中，其中一種方法是使用<xref:System.Web.Http.HttpResponseException>型別。 ASP.NET Core 不包含對等的類型。 您可以`HttpResponseException`使用下列步驟來新增的支援：

1. 建立名為`HttpResponseException`的知名例外狀況類型：

    [!code-csharp[](handle-errors/samples/3.x/Exceptions/HttpResponseException.cs?name=snippet_HttpResponseException)]

1. 建立名為`HttpResponseExceptionFilter`的動作篩選準則：

    [!code-csharp[](handle-errors/samples/3.x/Filters/HttpResponseExceptionFilter.cs?name=snippet_HttpResponseExceptionFilter)]

1. 在`Startup.ConfigureServices`中，將動作篩選準則新增至篩選集合：

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.2"
    
    [!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.1"

    [!code-csharp[](handle-errors/samples/2.x/2.1/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

## <a name="validation-failure-error-response"></a>驗證失敗錯誤回應

針對 Web API 控制器，當模型驗證失敗<xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>時，MVC 會以回應類型回應。 MVC 會使用的結果<xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>來針對驗證失敗來建立錯誤回應。 下列範例會使用 factory，將預設回應類型變更為<xref:Microsoft.AspNetCore.Mvc.SerializableError>中`Startup.ConfigureServices`的：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=4-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=5-14)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](handle-errors/samples/2.x/2.1/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=6-15)]

::: moniker-end

## <a name="client-error-response"></a>用戶端錯誤回應

*錯誤結果*會定義為 HTTP 狀態碼為400或更高的結果。 針對 Web API 控制器，MVC 會使用<xref:Microsoft.AspNetCore.Mvc.ProblemDetails>將錯誤結果轉換成結果。

::: moniker range=">= aspnetcore-3.0"

您可以透過下列其中一種方式來設定錯誤回應：

1. [執行 ProblemDetailsFactory](#implement-problemdetailsfactory)
1. [使用 ApiBehaviorOptions. ClientErrorMapping](#use-apibehavioroptionsclienterrormapping)

### <a name="implement-problemdetailsfactory"></a>執行 ProblemDetailsFactory

MVC 會`Microsoft.AspNetCore.Mvc.ProblemDetailsFactory`使用來產生和<xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>的<xref:Microsoft.AspNetCore.Mvc.ProblemDetails>所有實例。 這包括用戶端錯誤回應、驗證失敗錯誤回應， `Microsoft.AspNetCore.Mvc.ControllerBase.Problem`以及和<xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> helper 方法。

若要自訂問題詳細資料回應，請在中`ProblemDetailsFactory` `Startup.ConfigureServices`註冊的自訂執行：

```csharp
public void ConfigureServices(IServiceCollection serviceCollection)
{
    services.AddControllers();
    services.AddTransient<ProblemDetailsFactory, CustomProblemDetailsFactory>();
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

您可以如[使用 ApiBehaviorOptions. ClientErrorMapping](#use-apibehavioroptionsclienterrormapping)一節中所述，設定錯誤回應。

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="use-apibehavioroptionsclienterrormapping"></a>使用 ApiBehaviorOptions. ClientErrorMapping

使用 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> 屬性可設定 `ProblemDetails` 回應的內容。 例如，中`Startup.ConfigureServices`的下列程式碼會更新`type` 404 回應的屬性：

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8-9)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=9-10)]

::: moniker-end
