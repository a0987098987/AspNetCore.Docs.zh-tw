---
title: ASP.NET Core 的 Factory 中介軟體啟用
author: guardrex
description: 了解如何在 ASP.NET Core 中搭配使用 Factory 啟用實作與強型別中介軟體。
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 01/29/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 76ba257abfb11e0c2950b974f837c6ae5818a6a1
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a>ASP.NET Core 的 Factory 中介軟體啟用

作者：[Luke Latham](https://github.com/guardrex)

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) 是[中介軟體](xref:fundamentals/middleware/index)啟用的擴充點。

`UseMiddleware` 擴充方法會檢查中介軟體的已註冊類型是否實作 `IMiddleware`。 如果是，系統會使用容器中已註冊的 `IMiddlewareFactory` 執行個體來解析 `IMiddleware` 實作，而不是使用以慣例為基礎的中介軟體啟用邏輯。 中介軟體會註冊為應用程式服務容器中的範圍服務或暫時性服務。

優點：

* 依據要求啟用 (插入範圍服務)
* 強型別的中介軟體

`IMiddleware` 是依據要求啟用，因此範圍服務可以插入中介軟體的建構函式中。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

範例應用程式會示範如何依據下列條件來啟用中介軟體：

* 慣例。 如需傳統中介軟體啟用的詳細資訊，請參閱[中介軟體](xref:fundamentals/middleware/index)主題。
* [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) 實作。 預設的 [MiddlewareFactory 類別](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory)會啟動中介軟體。

中介軟體實作運作方式都相同，並會記錄查詢字串參數 (`key`) 所提供的值。 中介軟體會使用插入的資料庫內容 (範圍服務)，以記錄記憶體中資料庫的查詢字串值。

## <a name="imiddleware"></a>IMiddleware

[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) 可定義應用程式要求管線的中介軟體。 [InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) 方法會處理要求，並傳回 `Task` 以代表中介軟體的執行。

依據慣例啟用的中介軟體：

[!code-csharp[](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

由 `MiddlewareFactory` 啟用的中介軟體：

[!code-csharp[](extensibility/sample/Middleware/IMiddlewareMiddleware.cs?name=snippet1)]

您可為中介軟體建立延伸模組：

[!code-csharp[](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

您無法使用 `UseMiddleware` 將物件傳遞給由 Factory 啟用的中介軟體：

```csharp
public static IApplicationBuilder UseIMiddlewareMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<IMiddlewareMiddleware>(option);
}
```

由 Factory 啟用的中介軟體會新增至 *Startup.cs* 的內建容器中：

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet1&highlight=6)]

這兩個中介軟體都會在要求處理管線的 `Configure` 中註冊：

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet2&highlight=12-13)]

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) 可提供建立中介軟體的方法。 中介軟體 Factory 實作會在容器中註冊為範圍服務。

預設的 `IMiddlewareFactory` 實作是 [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory)，其位於 [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) 套件中 ([參考來源](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs))。

## <a name="additional-resources"></a>其他資源

* [中介軟體](xref:fundamentals/middleware/index)
* [以協力廠商容器啟用中介軟體](xref:fundamentals/middleware/extensibility-third-party-container)
