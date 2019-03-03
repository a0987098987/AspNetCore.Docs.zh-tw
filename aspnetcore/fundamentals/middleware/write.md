---
title: 撰寫自訂的 ASP.NET Core 中介軟體
author: rick-anderson
description: 了解如何撰寫自訂的 ASP.NET Core 中介軟體。
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2019
uid: fundamentals/middleware/write
ms.openlocfilehash: 2c5577394a10370d92c8a83f9d806b63f3245c8b
ms.sourcegitcommit: b3894b65e313570e97a2ab78b8addd22f427cac8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/23/2019
ms.locfileid: "56744898"
---
# <a name="write-custom-aspnet-core-middleware"></a><span data-ttu-id="48090-103">撰寫自訂的 ASP.NET Core 中介軟體</span><span class="sxs-lookup"><span data-stu-id="48090-103">Write custom ASP.NET Core middleware</span></span>

<span data-ttu-id="48090-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫</span><span class="sxs-lookup"><span data-stu-id="48090-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="48090-105">中介軟體為組成應用程式管線的軟體，用以處理要求與回應。</span><span class="sxs-lookup"><span data-stu-id="48090-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="48090-106">ASP.NET Core 提供一組豐富的內建中介軟體元件，但在某些情況下，您可能想要撰寫自訂的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="48090-106">ASP.NET Core provides a rich set of built-in middleware components, but in some scenarios you might want to write a custom middleware.</span></span>

## <a name="middleware-class"></a><span data-ttu-id="48090-107">中介軟體類別</span><span class="sxs-lookup"><span data-stu-id="48090-107">Middleware class</span></span>

<span data-ttu-id="48090-108">中介軟體通常封裝在類別中，並以擴充方法公開。</span><span class="sxs-lookup"><span data-stu-id="48090-108">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="48090-109">請考慮下列中介軟體，其會為來自查詢字串的目前要求設定文化特性：</span><span class="sxs-lookup"><span data-stu-id="48090-109">Consider the following middleware, which sets the culture for the current request from a query string:</span></span>

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="48090-110">上述範例程式碼用於示範中介軟體元件的建立。</span><span class="sxs-lookup"><span data-stu-id="48090-110">The preceding sample code is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="48090-111">如需 ASP.NET Core 的內建當地語系化支援，請參閱 <xref:fundamentals/localization>。</span><span class="sxs-lookup"><span data-stu-id="48090-111">For ASP.NET Core's built-in localization support, see <xref:fundamentals/localization>.</span></span>

<span data-ttu-id="48090-112">您可以藉由傳入文化特性來測試中介軟體。</span><span class="sxs-lookup"><span data-stu-id="48090-112">You can test the middleware by passing in the culture.</span></span> <span data-ttu-id="48090-113">例如，`http://localhost:7997/?culture=no`。</span><span class="sxs-lookup"><span data-stu-id="48090-113">For example, `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="48090-114">下列程式碼會將中介軟體委派移至類別：</span><span class="sxs-lookup"><span data-stu-id="48090-114">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="48090-115">中介軟體 `Task` 方法的名稱必須是 `Invoke`。</span><span class="sxs-lookup"><span data-stu-id="48090-115">The middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="48090-116">在 ASP.NET Core 2.0 或更新版本中，此名稱可以是 `Invoke` 或 `InvokeAsync`。</span><span class="sxs-lookup"><span data-stu-id="48090-116">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

::: moniker-end

## <a name="middleware-extension-method"></a><span data-ttu-id="48090-117">中介軟體擴充方法</span><span class="sxs-lookup"><span data-stu-id="48090-117">Middleware extension method</span></span>

<span data-ttu-id="48090-118">下列擴充方法透過 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 公開中介軟體：</span><span class="sxs-lookup"><span data-stu-id="48090-118">The following extension method exposes the middleware through <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="48090-119">下列程式碼會從 `Startup.Configure` 呼叫中介軟體：</span><span class="sxs-lookup"><span data-stu-id="48090-119">The following code calls the middleware from `Startup.Configure`:</span></span>

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="48090-120">中介軟體應於其建構函式中公開其相依性，以遵循[明確的相依性原則](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)。</span><span class="sxs-lookup"><span data-stu-id="48090-120">Middleware should follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="48090-121">中介軟體會在每次「應用程式存留期」就建構一次。</span><span class="sxs-lookup"><span data-stu-id="48090-121">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="48090-122">若您需要在要求內與中介軟體共用服務，請參閱[依要求的相依性](#per-request-dependencies)一節。</span><span class="sxs-lookup"><span data-stu-id="48090-122">See the [Per-request dependencies](#per-request-dependencies) section if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="48090-123">中介軟體元件可透過建構函式參數，解析其來自[相依性插入 (DI)](xref:fundamentals/dependency-injection) 的相依性。</span><span class="sxs-lookup"><span data-stu-id="48090-123">Middleware components can resolve their dependencies from [dependency injection (DI)](xref:fundamentals/dependency-injection) through constructor parameters.</span></span> <span data-ttu-id="48090-124">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) 也可直接接受其它參數。</span><span class="sxs-lookup"><span data-stu-id="48090-124">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) can also accept additional parameters directly.</span></span>

## <a name="per-request-dependencies"></a><span data-ttu-id="48090-125">依要求的相依性</span><span class="sxs-lookup"><span data-stu-id="48090-125">Per-request dependencies</span></span>

<span data-ttu-id="48090-126">因為中介軟體建構於應用程式啟動時，而非依要求建構，所以在每個要求期間，中介軟體建構函式使用的「已限定範圍」存留期服務不會與其它插入相依性的類型共用。</span><span class="sxs-lookup"><span data-stu-id="48090-126">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors aren't shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="48090-127">如果您必須在中介軟體和其他類型間共用「已限定範圍」的服務，請將這些服務新增至 `Invoke` 方法的簽章。</span><span class="sxs-lookup"><span data-stu-id="48090-127">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="48090-128">`Invoke` 方法可以接受 DI 所填入的其他參數：</span><span class="sxs-lookup"><span data-stu-id="48090-128">The `Invoke` method can accept additional parameters that are populated by DI:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="48090-129">其他資源</span><span class="sxs-lookup"><span data-stu-id="48090-129">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
