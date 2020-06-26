---
title: ASP.NET Core 的 Factory 中介軟體啟用
author: rick-anderson
description: 了解如何在 ASP.NET Core 中搭配使用 Factory 啟用實作與強型別中介軟體。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/22/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 893dce119328acaa4ffd3087b94328017f5915ca
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85399903"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a>ASP.NET Core 的 Factory 中介軟體啟用

::: moniker range=">= aspnetcore-3.0"

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> 是[中介軟體](xref:fundamentals/middleware/index)啟用的擴充點。

<xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> 擴充方法會檢查中介軟體的已註冊類型是否實作 <xref:Microsoft.AspNetCore.Http.IMiddleware>。 如果是，系統會使用容器中已註冊的 <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> 執行個體來解析 <xref:Microsoft.AspNetCore.Http.IMiddleware> 實作，而不是使用以慣例為基礎的中介軟體啟用邏輯。 中介軟體會註冊為應用程式服務容器中的[範圍服務或暫時性服務](xref:fundamentals/dependency-injection#service-lifetimes)。

優點：

* 依據用戶端要求啟用 (插入範圍服務)
* 強型別的中介軟體

<xref:Microsoft.AspNetCore.Http.IMiddleware> 是依據用戶端要求 (連線) 來啟用，因此範圍服務可以插入中介軟體的建構函式中。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="imiddleware"></a>IMiddleware

<xref:Microsoft.AspNetCore.Http.IMiddleware> 可定義應用程式要求管線的中介軟體。 [InvokeAsync(HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) 方法會處理要求，並傳回 <xref:System.Threading.Tasks.Task> 以代表中介軟體的執行。

依據慣例啟用的中介軟體：

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

由 <xref:Microsoft.AspNetCore.Http.MiddlewareFactory> 啟用的中介軟體：

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

您可為中介軟體建立延伸模組：

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

您無法使用 <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> 將物件傳遞給由 Factory 啟用的中介軟體：

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

由 Factory 所啟用中介軟體會新增至 `Startup.ConfigureServices` 中的內建容器：

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet1&highlight=6)]

這兩個中介軟體都會在要求處理管線的 `Startup.Configure` 中註冊：

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet2&highlight=12-13)]

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> 提供建立中介軟體的方法。 中介軟體 Factory 實作會在容器中註冊為範圍服務。

預設的 <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> 實作是 <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>，其位於 [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) 套件中。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> 是[中介軟體](xref:fundamentals/middleware/index)啟用的擴充點。

<xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> 擴充方法會檢查中介軟體的已註冊類型是否實作 <xref:Microsoft.AspNetCore.Http.IMiddleware>。 如果是，系統會使用容器中已註冊的 <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> 執行個體來解析 <xref:Microsoft.AspNetCore.Http.IMiddleware> 實作，而不是使用以慣例為基礎的中介軟體啟用邏輯。 中介軟體會註冊為應用程式服務容器中的[範圍服務或暫時性服務](xref:fundamentals/dependency-injection#service-lifetimes)。

優點：

* 依據用戶端要求啟用 (插入範圍服務)
* 強型別的中介軟體

<xref:Microsoft.AspNetCore.Http.IMiddleware> 是依據用戶端要求 (連線) 來啟用，因此範圍服務可以插入中介軟體的建構函式中。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="imiddleware"></a>IMiddleware

<xref:Microsoft.AspNetCore.Http.IMiddleware> 可定義應用程式要求管線的中介軟體。 [InvokeAsync(HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) 方法會處理要求，並傳回 <xref:System.Threading.Tasks.Task> 以代表中介軟體的執行。

依據慣例啟用的中介軟體：

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

由 <xref:Microsoft.AspNetCore.Http.MiddlewareFactory> 啟用的中介軟體：

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

您可為中介軟體建立延伸模組：

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

您無法使用 <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> 將物件傳遞給由 Factory 啟用的中介軟體：

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

由 Factory 所啟用中介軟體會新增至 `Startup.ConfigureServices` 中的內建容器：

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet1&highlight=6)]

這兩個中介軟體都會在要求處理管線的 `Startup.Configure` 中註冊：

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet2&highlight=13-14)]

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> 提供建立中介軟體的方法。 中介軟體 Factory 實作會在容器中註冊為範圍服務。

預設的 <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> 實作是 <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>，其位於 [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) 套件中。

::: moniker-end

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/middleware/index>
* <xref:fundamentals/middleware/extensibility-third-party-container>
