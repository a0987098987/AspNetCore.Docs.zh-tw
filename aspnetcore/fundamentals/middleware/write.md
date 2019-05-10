---
title: 撰寫自訂的 ASP.NET Core 中介軟體
author: rick-anderson
description: 了解如何撰寫自訂的 ASP.NET Core 中介軟體。
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2019
uid: fundamentals/middleware/write
ms.openlocfilehash: 2c5577394a10370d92c8a83f9d806b63f3245c8b
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64889163"
---
# <a name="write-custom-aspnet-core-middleware"></a>撰寫自訂的 ASP.NET Core 中介軟體

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫

中介軟體為組成應用程式管線的軟體，用以處理要求與回應。 ASP.NET Core 提供一組豐富的內建中介軟體元件，但在某些情況下，您可能想要撰寫自訂的中介軟體。

## <a name="middleware-class"></a>中介軟體類別

中介軟體通常封裝在類別中，並以擴充方法公開。 請考慮下列中介軟體，其會為來自查詢字串的目前要求設定文化特性：

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

上述範例程式碼用於示範中介軟體元件的建立。 如需 ASP.NET Core 的內建當地語系化支援，請參閱 <xref:fundamentals/localization>。

您可以藉由傳入文化特性來測試中介軟體。 例如，`http://localhost:7997/?culture=no`。

下列程式碼會將中介軟體委派移至類別：

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

中介軟體 `Task` 方法的名稱必須是 `Invoke`。 在 ASP.NET Core 2.0 或更新版本中，此名稱可以是 `Invoke` 或 `InvokeAsync`。

::: moniker-end

## <a name="middleware-extension-method"></a>中介軟體擴充方法

下列擴充方法透過 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 公開中介軟體：

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

下列程式碼會從 `Startup.Configure` 呼叫中介軟體：

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

中介軟體應於其建構函式中公開其相依性，以遵循[明確的相依性原則](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)。 中介軟體會在每次「應用程式存留期」就建構一次。 若您需要在要求內與中介軟體共用服務，請參閱[依要求的相依性](#per-request-dependencies)一節。

中介軟體元件可透過建構函式參數，解析其來自[相依性插入 (DI)](xref:fundamentals/dependency-injection) 的相依性。 [UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) 也可直接接受其它參數。

## <a name="per-request-dependencies"></a>依要求的相依性

因為中介軟體建構於應用程式啟動時，而非依要求建構，所以在每個要求期間，中介軟體建構函式使用的「已限定範圍」存留期服務不會與其它插入相依性的類型共用。 如果您必須在中介軟體和其他類型間共用「已限定範圍」的服務，請將這些服務新增至 `Invoke` 方法的簽章。 `Invoke` 方法可以接受 DI 所填入的其他參數：

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // IMyScopedService is injected into Invoke
    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/middleware/index>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
