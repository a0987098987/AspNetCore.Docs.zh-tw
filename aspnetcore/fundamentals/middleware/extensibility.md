---
title: ASP.NET Core 的 Factory 中介軟體啟用
author: guardrex
description: 了解如何在 ASP.NET Core 中搭配使用 Factory 啟用實作與強型別中介軟體。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/14/2018
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 346f5e7b28a9cd17a03a864772ed8b2e4be9455b
ms.sourcegitcommit: 2c158fcfd325cad97ead608a816e525fe3dcf757
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2018
ms.locfileid: "41751473"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a><span data-ttu-id="e43e6-103">ASP.NET Core 的 Factory 中介軟體啟用</span><span class="sxs-lookup"><span data-stu-id="e43e6-103">Factory-based middleware activation in ASP.NET Core</span></span>

<span data-ttu-id="e43e6-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e43e6-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e43e6-105">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) 是[中介軟體](xref:fundamentals/middleware/index)啟用的擴充點。</span><span class="sxs-lookup"><span data-stu-id="e43e6-105">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) is an extensibility point for [middleware](xref:fundamentals/middleware/index) activation.</span></span>

<span data-ttu-id="e43e6-106">`UseMiddleware` 擴充方法會檢查中介軟體的已註冊類型是否實作 `IMiddleware`。</span><span class="sxs-lookup"><span data-stu-id="e43e6-106">`UseMiddleware` extension methods check if a middleware's registered type implements `IMiddleware`.</span></span> <span data-ttu-id="e43e6-107">如果是，系統會使用容器中已註冊的 `IMiddlewareFactory` 執行個體來解析 `IMiddleware` 實作，而不是使用以慣例為基礎的中介軟體啟用邏輯。</span><span class="sxs-lookup"><span data-stu-id="e43e6-107">If it does, the `IMiddlewareFactory` instance registered in the container is used to resolve the `IMiddleware` implementation instead of using the convention-based middleware activation logic.</span></span> <span data-ttu-id="e43e6-108">中介軟體會註冊為應用程式服務容器中的範圍服務或暫時性服務。</span><span class="sxs-lookup"><span data-stu-id="e43e6-108">The middleware is registered as a scoped or transient service in the app's service container.</span></span>

<span data-ttu-id="e43e6-109">優點：</span><span class="sxs-lookup"><span data-stu-id="e43e6-109">Benefits:</span></span>

* <span data-ttu-id="e43e6-110">依據要求啟用 (插入範圍服務)</span><span class="sxs-lookup"><span data-stu-id="e43e6-110">Activation per request (injection of scoped services)</span></span>
* <span data-ttu-id="e43e6-111">強型別的中介軟體</span><span class="sxs-lookup"><span data-stu-id="e43e6-111">Strong typing of middleware</span></span>

<span data-ttu-id="e43e6-112">`IMiddleware` 是依據要求啟用，因此範圍服務可以插入中介軟體的建構函式中。</span><span class="sxs-lookup"><span data-stu-id="e43e6-112">`IMiddleware` is activated per request, so scoped services can be injected into the middleware's constructor.</span></span>

<span data-ttu-id="e43e6-113">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e43e6-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e43e6-114">範例應用程式會示範如何依據下列條件來啟用中介軟體：</span><span class="sxs-lookup"><span data-stu-id="e43e6-114">The sample app demonstrates middleware activated by:</span></span>

* <span data-ttu-id="e43e6-115">慣例。</span><span class="sxs-lookup"><span data-stu-id="e43e6-115">Convention.</span></span> <span data-ttu-id="e43e6-116">如需傳統中介軟體啟用的詳細資訊，請參閱[中介軟體](xref:fundamentals/middleware/index)主題。</span><span class="sxs-lookup"><span data-stu-id="e43e6-116">For more information on conventional middleware activation, see the [Middleware](xref:fundamentals/middleware/index) topic.</span></span>
* <span data-ttu-id="e43e6-117">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) 實作。</span><span class="sxs-lookup"><span data-stu-id="e43e6-117">An [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) implementation.</span></span> <span data-ttu-id="e43e6-118">預設的 [MiddlewareFactory 類別](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory)會啟動中介軟體。</span><span class="sxs-lookup"><span data-stu-id="e43e6-118">The default [MiddlewareFactory class](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) activates the middleware.</span></span>

<span data-ttu-id="e43e6-119">中介軟體實作運作方式都相同，並會記錄查詢字串參數 (`key`) 所提供的值。</span><span class="sxs-lookup"><span data-stu-id="e43e6-119">The middleware implementations function identically and record the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="e43e6-120">中介軟體會使用插入的資料庫內容 (範圍服務)，以記錄記憶體中資料庫的查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="e43e6-120">The middlewares use an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

## <a name="imiddleware"></a><span data-ttu-id="e43e6-121">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="e43e6-121">IMiddleware</span></span>

<span data-ttu-id="e43e6-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) 可定義應用程式要求管線的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="e43e6-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) defines middleware for the app's request pipeline.</span></span> <span data-ttu-id="e43e6-123">[InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) 方法會處理要求，並傳回 `Task` 以代表中介軟體的執行。</span><span class="sxs-lookup"><span data-stu-id="e43e6-123">The [InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) method handles requests and returns a `Task` that represents the execution of the middleware.</span></span>

<span data-ttu-id="e43e6-124">依據慣例啟用的中介軟體：</span><span class="sxs-lookup"><span data-stu-id="e43e6-124">Middleware activated by convention:</span></span>

[!code-csharp[](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<span data-ttu-id="e43e6-125">由 `MiddlewareFactory` 啟用的中介軟體：</span><span class="sxs-lookup"><span data-stu-id="e43e6-125">Middleware activated by `MiddlewareFactory`:</span></span>

[!code-csharp[](extensibility/sample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="e43e6-126">您可為中介軟體建立延伸模組：</span><span class="sxs-lookup"><span data-stu-id="e43e6-126">Extensions are created for the middlewares:</span></span>

[!code-csharp[](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="e43e6-127">您無法使用 `UseMiddleware` 將物件傳遞給由 Factory 啟用的中介軟體：</span><span class="sxs-lookup"><span data-stu-id="e43e6-127">It isn't possible to pass objects to the factory-activated middleware with `UseMiddleware`:</span></span>

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

<span data-ttu-id="e43e6-128">由 Factory 啟用的中介軟體會新增至 *Startup.cs* 的內建容器中：</span><span class="sxs-lookup"><span data-stu-id="e43e6-128">The factory-activated middleware is added to the built-in container in *Startup.cs*:</span></span>

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet1&highlight=12)]

<span data-ttu-id="e43e6-129">這兩個中介軟體都會在要求處理管線的 `Configure` 中註冊：</span><span class="sxs-lookup"><span data-stu-id="e43e6-129">Both middlewares are registered in the request processing pipeline in `Configure`:</span></span>

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet2&highlight=14-15)]

## <a name="imiddlewarefactory"></a><span data-ttu-id="e43e6-130">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="e43e6-130">IMiddlewareFactory</span></span>

<span data-ttu-id="e43e6-131">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) 可提供建立中介軟體的方法。</span><span class="sxs-lookup"><span data-stu-id="e43e6-131">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) provides methods to create middleware.</span></span> <span data-ttu-id="e43e6-132">中介軟體 Factory 實作會在容器中註冊為範圍服務。</span><span class="sxs-lookup"><span data-stu-id="e43e6-132">The middleware factory implementation is registered in the container as a scoped service.</span></span>

<span data-ttu-id="e43e6-133">預設的 `IMiddlewareFactory` 實作是 [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory)，其位於 [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) 套件中。</span><span class="sxs-lookup"><span data-stu-id="e43e6-133">The default `IMiddlewareFactory` implementation, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), is found in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e43e6-134">其他資源</span><span class="sxs-lookup"><span data-stu-id="e43e6-134">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* <xref:fundamentals/middleware/extensibility-third-party-container>
