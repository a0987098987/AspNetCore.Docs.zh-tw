---
title: ASP.NET Core 的 Factory 中介軟體啟用
author: guardrex
description: 了解如何在 ASP.NET Core 中搭配使用 Factory 啟用實作與強型別中介軟體。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/22/2019
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 17018d2dd20ed7b26bd0aa1095fa720a73f77261
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/23/2019
ms.locfileid: "71186939"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a><span data-ttu-id="4c342-103">ASP.NET Core 的 Factory 中介軟體啟用</span><span class="sxs-lookup"><span data-stu-id="4c342-103">Factory-based middleware activation in ASP.NET Core</span></span>

<span data-ttu-id="4c342-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4c342-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4c342-105"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> 是[中介軟體](xref:fundamentals/middleware/index)啟用的擴充點。</span><span class="sxs-lookup"><span data-stu-id="4c342-105"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> is an extensibility point for [middleware](xref:fundamentals/middleware/index) activation.</span></span>

<span data-ttu-id="4c342-106"><xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> 擴充方法會檢查中介軟體的已註冊類型是否實作 <xref:Microsoft.AspNetCore.Http.IMiddleware>。</span><span class="sxs-lookup"><span data-stu-id="4c342-106"><xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> extension methods check if a middleware's registered type implements <xref:Microsoft.AspNetCore.Http.IMiddleware>.</span></span> <span data-ttu-id="4c342-107">如果是，系統會使用容器中已註冊的 <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> 執行個體來解析 <xref:Microsoft.AspNetCore.Http.IMiddleware> 實作，而不是使用以慣例為基礎的中介軟體啟用邏輯。</span><span class="sxs-lookup"><span data-stu-id="4c342-107">If it does, the <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> instance registered in the container is used to resolve the <xref:Microsoft.AspNetCore.Http.IMiddleware> implementation instead of using the convention-based middleware activation logic.</span></span> <span data-ttu-id="4c342-108">中介軟體會註冊為應用程式服務容器中的[範圍服務或暫時性服務](xref:fundamentals/dependency-injection#service-lifetimes)。</span><span class="sxs-lookup"><span data-stu-id="4c342-108">The middleware is registered as a [scoped or transient service](xref:fundamentals/dependency-injection#service-lifetimes) in the app's service container.</span></span>

<span data-ttu-id="4c342-109">優點：</span><span class="sxs-lookup"><span data-stu-id="4c342-109">Benefits:</span></span>

* <span data-ttu-id="4c342-110">依據用戶端要求啟用 (插入範圍服務)</span><span class="sxs-lookup"><span data-stu-id="4c342-110">Activation per client request (injection of scoped services)</span></span>
* <span data-ttu-id="4c342-111">強型別的中介軟體</span><span class="sxs-lookup"><span data-stu-id="4c342-111">Strong typing of middleware</span></span>

<span data-ttu-id="4c342-112"><xref:Microsoft.AspNetCore.Http.IMiddleware> 是依據用戶端要求 (連線) 來啟用，因此範圍服務可以插入中介軟體的建構函式中。</span><span class="sxs-lookup"><span data-stu-id="4c342-112"><xref:Microsoft.AspNetCore.Http.IMiddleware> is activated per client request (connection), so scoped services can be injected into the middleware's constructor.</span></span>

<span data-ttu-id="4c342-113">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4c342-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="imiddleware"></a><span data-ttu-id="4c342-114">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="4c342-114">IMiddleware</span></span>

<span data-ttu-id="4c342-115"><xref:Microsoft.AspNetCore.Http.IMiddleware> 可定義應用程式要求管線的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="4c342-115"><xref:Microsoft.AspNetCore.Http.IMiddleware> defines middleware for the app's request pipeline.</span></span> <span data-ttu-id="4c342-116">[InvokeAsync(HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) 方法會處理要求，並傳回 <xref:System.Threading.Tasks.Task> 以代表中介軟體的執行。</span><span class="sxs-lookup"><span data-stu-id="4c342-116">The [InvokeAsync(HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) method handles requests and returns a <xref:System.Threading.Tasks.Task> that represents the execution of the middleware.</span></span>

<span data-ttu-id="4c342-117">依據慣例啟用的中介軟體：</span><span class="sxs-lookup"><span data-stu-id="4c342-117">Middleware activated by convention:</span></span>

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<span data-ttu-id="4c342-118">由 <xref:Microsoft.AspNetCore.Http.MiddlewareFactory> 啟用的中介軟體：</span><span class="sxs-lookup"><span data-stu-id="4c342-118">Middleware activated by <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>:</span></span>

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="4c342-119">您可為中介軟體建立延伸模組：</span><span class="sxs-lookup"><span data-stu-id="4c342-119">Extensions are created for the middlewares:</span></span>

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="4c342-120">您無法使用 <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> 將物件傳遞給由 Factory 啟用的中介軟體：</span><span class="sxs-lookup"><span data-stu-id="4c342-120">It isn't possible to pass objects to the factory-activated middleware with <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>:</span></span>

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

<span data-ttu-id="4c342-121">由 Factory 所啟用中介軟體會新增至 `Startup.ConfigureServices` 中的內建容器：</span><span class="sxs-lookup"><span data-stu-id="4c342-121">The factory-activated middleware is added to the built-in container in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet1&highlight=6)]

<span data-ttu-id="4c342-122">這兩個中介軟體都會在要求處理管線的 `Startup.Configure` 中註冊：</span><span class="sxs-lookup"><span data-stu-id="4c342-122">Both middlewares are registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet2&highlight=12-13)]

## <a name="imiddlewarefactory"></a><span data-ttu-id="4c342-123">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="4c342-123">IMiddlewareFactory</span></span>

<span data-ttu-id="4c342-124"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> 提供建立中介軟體的方法。</span><span class="sxs-lookup"><span data-stu-id="4c342-124"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> provides methods to create middleware.</span></span> <span data-ttu-id="4c342-125">中介軟體 Factory 實作會在容器中註冊為範圍服務。</span><span class="sxs-lookup"><span data-stu-id="4c342-125">The middleware factory implementation is registered in the container as a scoped service.</span></span>

<span data-ttu-id="4c342-126">預設的 <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> 實作是 <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>，其位於 [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) 套件中。</span><span class="sxs-lookup"><span data-stu-id="4c342-126">The default <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> implementation, <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, is found in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="4c342-127"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> 是[中介軟體](xref:fundamentals/middleware/index)啟用的擴充點。</span><span class="sxs-lookup"><span data-stu-id="4c342-127"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> is an extensibility point for [middleware](xref:fundamentals/middleware/index) activation.</span></span>

<span data-ttu-id="4c342-128"><xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> 擴充方法會檢查中介軟體的已註冊類型是否實作 <xref:Microsoft.AspNetCore.Http.IMiddleware>。</span><span class="sxs-lookup"><span data-stu-id="4c342-128"><xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> extension methods check if a middleware's registered type implements <xref:Microsoft.AspNetCore.Http.IMiddleware>.</span></span> <span data-ttu-id="4c342-129">如果是，系統會使用容器中已註冊的 <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> 執行個體來解析 <xref:Microsoft.AspNetCore.Http.IMiddleware> 實作，而不是使用以慣例為基礎的中介軟體啟用邏輯。</span><span class="sxs-lookup"><span data-stu-id="4c342-129">If it does, the <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> instance registered in the container is used to resolve the <xref:Microsoft.AspNetCore.Http.IMiddleware> implementation instead of using the convention-based middleware activation logic.</span></span> <span data-ttu-id="4c342-130">中介軟體會註冊為應用程式服務容器中的[範圍服務或暫時性服務](xref:fundamentals/dependency-injection#service-lifetimes)。</span><span class="sxs-lookup"><span data-stu-id="4c342-130">The middleware is registered as a [scoped or transient service](xref:fundamentals/dependency-injection#service-lifetimes) in the app's service container.</span></span>

<span data-ttu-id="4c342-131">優點：</span><span class="sxs-lookup"><span data-stu-id="4c342-131">Benefits:</span></span>

* <span data-ttu-id="4c342-132">依據用戶端要求啟用 (插入範圍服務)</span><span class="sxs-lookup"><span data-stu-id="4c342-132">Activation per client request (injection of scoped services)</span></span>
* <span data-ttu-id="4c342-133">強型別的中介軟體</span><span class="sxs-lookup"><span data-stu-id="4c342-133">Strong typing of middleware</span></span>

<span data-ttu-id="4c342-134"><xref:Microsoft.AspNetCore.Http.IMiddleware> 是依據用戶端要求 (連線) 來啟用，因此範圍服務可以插入中介軟體的建構函式中。</span><span class="sxs-lookup"><span data-stu-id="4c342-134"><xref:Microsoft.AspNetCore.Http.IMiddleware> is activated per client request (connection), so scoped services can be injected into the middleware's constructor.</span></span>

<span data-ttu-id="4c342-135">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4c342-135">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="imiddleware"></a><span data-ttu-id="4c342-136">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="4c342-136">IMiddleware</span></span>

<span data-ttu-id="4c342-137"><xref:Microsoft.AspNetCore.Http.IMiddleware> 可定義應用程式要求管線的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="4c342-137"><xref:Microsoft.AspNetCore.Http.IMiddleware> defines middleware for the app's request pipeline.</span></span> <span data-ttu-id="4c342-138">[InvokeAsync(HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) 方法會處理要求，並傳回 <xref:System.Threading.Tasks.Task> 以代表中介軟體的執行。</span><span class="sxs-lookup"><span data-stu-id="4c342-138">The [InvokeAsync(HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) method handles requests and returns a <xref:System.Threading.Tasks.Task> that represents the execution of the middleware.</span></span>

<span data-ttu-id="4c342-139">依據慣例啟用的中介軟體：</span><span class="sxs-lookup"><span data-stu-id="4c342-139">Middleware activated by convention:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<span data-ttu-id="4c342-140">由 <xref:Microsoft.AspNetCore.Http.MiddlewareFactory> 啟用的中介軟體：</span><span class="sxs-lookup"><span data-stu-id="4c342-140">Middleware activated by <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="4c342-141">您可為中介軟體建立延伸模組：</span><span class="sxs-lookup"><span data-stu-id="4c342-141">Extensions are created for the middlewares:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="4c342-142">您無法使用 <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> 將物件傳遞給由 Factory 啟用的中介軟體：</span><span class="sxs-lookup"><span data-stu-id="4c342-142">It isn't possible to pass objects to the factory-activated middleware with <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>:</span></span>

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

<span data-ttu-id="4c342-143">由 Factory 所啟用中介軟體會新增至 `Startup.ConfigureServices` 中的內建容器：</span><span class="sxs-lookup"><span data-stu-id="4c342-143">The factory-activated middleware is added to the built-in container in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet1&highlight=6)]

<span data-ttu-id="4c342-144">這兩個中介軟體都會在要求處理管線的 `Startup.Configure` 中註冊：</span><span class="sxs-lookup"><span data-stu-id="4c342-144">Both middlewares are registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet2&highlight=13-14)]

## <a name="imiddlewarefactory"></a><span data-ttu-id="4c342-145">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="4c342-145">IMiddlewareFactory</span></span>

<span data-ttu-id="4c342-146"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> 提供建立中介軟體的方法。</span><span class="sxs-lookup"><span data-stu-id="4c342-146"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> provides methods to create middleware.</span></span> <span data-ttu-id="4c342-147">中介軟體 Factory 實作會在容器中註冊為範圍服務。</span><span class="sxs-lookup"><span data-stu-id="4c342-147">The middleware factory implementation is registered in the container as a scoped service.</span></span>

<span data-ttu-id="4c342-148">預設的 <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> 實作是 <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>，其位於 [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) 套件中。</span><span class="sxs-lookup"><span data-stu-id="4c342-148">The default <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> implementation, <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, is found in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="4c342-149">其他資源</span><span class="sxs-lookup"><span data-stu-id="4c342-149">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* <xref:fundamentals/middleware/extensibility-third-party-container>
