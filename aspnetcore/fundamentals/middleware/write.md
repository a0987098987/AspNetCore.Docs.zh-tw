---
title: 撰寫自訂的 ASP.NET Core 中介軟體
author: rick-anderson
description: 了解如何撰寫自訂的 ASP.NET Core 中介軟體。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 5/18/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/middleware/write
ms.openlocfilehash: aeebc7d5a7cbfaaab6d48b8da76d35c47c2c552b
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85407846"
---
# <a name="write-custom-aspnet-core-middleware"></a><span data-ttu-id="2ea2b-103">撰寫自訂的 ASP.NET Core 中介軟體</span><span class="sxs-lookup"><span data-stu-id="2ea2b-103">Write custom ASP.NET Core middleware</span></span>

<span data-ttu-id="2ea2b-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫</span><span class="sxs-lookup"><span data-stu-id="2ea2b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="2ea2b-105">中介軟體為組成應用程式管線的軟體，用以處理要求與回應。</span><span class="sxs-lookup"><span data-stu-id="2ea2b-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="2ea2b-106">ASP.NET Core 提供一組豐富的內建中介軟體元件，但在某些情況下，您可能想要撰寫自訂的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="2ea2b-106">ASP.NET Core provides a rich set of built-in middleware components, but in some scenarios you might want to write a custom middleware.</span></span>

> [!NOTE]
> <span data-ttu-id="2ea2b-107">本主題說明如何撰寫以*慣例為基礎的*中介軟體。</span><span class="sxs-lookup"><span data-stu-id="2ea2b-107">This topic describes how to write *convention-based* middleware.</span></span> <span data-ttu-id="2ea2b-108">如需使用強式類型和每個要求啟用的方法，請參閱 <xref:fundamentals/middleware/extensibility> 。</span><span class="sxs-lookup"><span data-stu-id="2ea2b-108">For an approach that uses strong typing and per-request activation, see <xref:fundamentals/middleware/extensibility>.</span></span>

## <a name="middleware-class"></a><span data-ttu-id="2ea2b-109">中介軟體類別</span><span class="sxs-lookup"><span data-stu-id="2ea2b-109">Middleware class</span></span>

<span data-ttu-id="2ea2b-110">中介軟體通常封裝在類別中，並以擴充方法公開。</span><span class="sxs-lookup"><span data-stu-id="2ea2b-110">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="2ea2b-111">請考慮下列中介軟體，其會為來自查詢字串的目前要求設定文化特性：</span><span class="sxs-lookup"><span data-stu-id="2ea2b-111">Consider the following middleware, which sets the culture for the current request from a query string:</span></span>

[!code-csharp[](write/snapshot/StartupCulture.cs)]

<span data-ttu-id="2ea2b-112">上述範例程式碼用於示範中介軟體元件的建立。</span><span class="sxs-lookup"><span data-stu-id="2ea2b-112">The preceding sample code is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="2ea2b-113">如需 ASP.NET Core 的內建當地語系化支援，請參閱 <xref:fundamentals/localization>。</span><span class="sxs-lookup"><span data-stu-id="2ea2b-113">For ASP.NET Core's built-in localization support, see <xref:fundamentals/localization>.</span></span>

<span data-ttu-id="2ea2b-114">藉由傳入文化特性來測試中介軟體。</span><span class="sxs-lookup"><span data-stu-id="2ea2b-114">Test the middleware by passing in the culture.</span></span> <span data-ttu-id="2ea2b-115">例如，要求 `https://localhost:5001/?culture=no`。</span><span class="sxs-lookup"><span data-stu-id="2ea2b-115">For example, request `https://localhost:5001/?culture=no`.</span></span>

<span data-ttu-id="2ea2b-116">下列程式碼會將中介軟體委派移至類別：</span><span class="sxs-lookup"><span data-stu-id="2ea2b-116">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](write/snapshot/RequestCultureMiddleware.cs)]

<span data-ttu-id="2ea2b-117">中介軟體類別必須包含：</span><span class="sxs-lookup"><span data-stu-id="2ea2b-117">The middleware class must include:</span></span>

* <span data-ttu-id="2ea2b-118">具有 <xref:Microsoft.AspNetCore.Http.RequestDelegate> 類型參數的公用建構函式。</span><span class="sxs-lookup"><span data-stu-id="2ea2b-118">A public constructor with a parameter of type <xref:Microsoft.AspNetCore.Http.RequestDelegate>.</span></span>
* <span data-ttu-id="2ea2b-119">名為 `Invoke` 或 `InvokeAsync` 的公用方法。</span><span class="sxs-lookup"><span data-stu-id="2ea2b-119">A public method named `Invoke` or `InvokeAsync`.</span></span> <span data-ttu-id="2ea2b-120">此方法必須：</span><span class="sxs-lookup"><span data-stu-id="2ea2b-120">This method must:</span></span>
  * <span data-ttu-id="2ea2b-121">傳回 `Task`。</span><span class="sxs-lookup"><span data-stu-id="2ea2b-121">Return a `Task`.</span></span>
  * <span data-ttu-id="2ea2b-122">接受 <xref:Microsoft.AspNetCore.Http.HttpContext> 類型的第一個參數。</span><span class="sxs-lookup"><span data-stu-id="2ea2b-122">Accept a first parameter of type <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>
  
<span data-ttu-id="2ea2b-123">建構函式和 `Invoke`/`InvokeAsync` 的其他參數會由[相依性插入 (DI)](xref:fundamentals/dependency-injection) 所填入。</span><span class="sxs-lookup"><span data-stu-id="2ea2b-123">Additional parameters for the constructor and `Invoke`/`InvokeAsync` are populated by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware-dependencies"></a><span data-ttu-id="2ea2b-124">中介軟體相依性</span><span class="sxs-lookup"><span data-stu-id="2ea2b-124">Middleware dependencies</span></span>

<span data-ttu-id="2ea2b-125">中介軟體應於其建構函式中公開其相依性，以遵循[明確的相依性原則](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)。</span><span class="sxs-lookup"><span data-stu-id="2ea2b-125">Middleware should follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="2ea2b-126">中介軟體會在每次「應用程式存留期」\*\* 就建構一次。</span><span class="sxs-lookup"><span data-stu-id="2ea2b-126">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="2ea2b-127">若您需要在要求內與中介軟體共用服務，請參閱[依要求的中介軟體相依性](#per-request-middleware-dependencies)一節。</span><span class="sxs-lookup"><span data-stu-id="2ea2b-127">See the [Per-request middleware dependencies](#per-request-middleware-dependencies) section if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="2ea2b-128">中介軟體元件可透過建構函式參數，解析其來自[相依性插入 (DI)](xref:fundamentals/dependency-injection) 的相依性。</span><span class="sxs-lookup"><span data-stu-id="2ea2b-128">Middleware components can resolve their dependencies from [dependency injection (DI)](xref:fundamentals/dependency-injection) through constructor parameters.</span></span> <span data-ttu-id="2ea2b-129">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) 也可直接接受其它參數。</span><span class="sxs-lookup"><span data-stu-id="2ea2b-129">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) can also accept additional parameters directly.</span></span>

## <a name="per-request-middleware-dependencies"></a><span data-ttu-id="2ea2b-130">依要求的中介軟體相依性</span><span class="sxs-lookup"><span data-stu-id="2ea2b-130">Per-request middleware dependencies</span></span>

<span data-ttu-id="2ea2b-131">因為中介軟體建構於應用程式啟動時，而非依要求建構，所以在每個要求期間，中介軟體建構函式使用的「已限定範圍」\*\* 存留期服務不會與其它插入相依性的類型共用。</span><span class="sxs-lookup"><span data-stu-id="2ea2b-131">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors aren't shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="2ea2b-132">如果您必須在中介軟體和其他類型間共用「已限定範圍」\*\* 的服務，請將這些服務新增至 `Invoke` 方法的簽章。</span><span class="sxs-lookup"><span data-stu-id="2ea2b-132">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="2ea2b-133">`Invoke` 方法可以接受 DI 所填入的其他參數：</span><span class="sxs-lookup"><span data-stu-id="2ea2b-133">The `Invoke` method can accept additional parameters that are populated by DI:</span></span>

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

## <a name="middleware-extension-method"></a><span data-ttu-id="2ea2b-134">中介軟體擴充方法</span><span class="sxs-lookup"><span data-stu-id="2ea2b-134">Middleware extension method</span></span>

<span data-ttu-id="2ea2b-135">下列擴充方法透過 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 公開中介軟體：</span><span class="sxs-lookup"><span data-stu-id="2ea2b-135">The following extension method exposes the middleware through <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span></span>

[!code-csharp[](write/snapshot/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="2ea2b-136">下列程式碼會從 `Startup.Configure` 呼叫中介軟體：</span><span class="sxs-lookup"><span data-stu-id="2ea2b-136">The following code calls the middleware from `Startup.Configure`:</span></span>

[!code-csharp[](write/snapshot/Startup.cs?highlight=5)]

## <a name="additional-resources"></a><span data-ttu-id="2ea2b-137">其他資源</span><span class="sxs-lookup"><span data-stu-id="2ea2b-137">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* <xref:test/middleware>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
