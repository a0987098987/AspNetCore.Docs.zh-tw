---
title: ASP.NET Core 中的應用程式啟動
author: rick-anderson
description: 了解 ASP.NET Core 中的 Startup 類別如何設定服務和應用程式的要求管線。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/startup
ms.openlocfilehash: 39fba5ccc99ec0ecf32df5681cfc025c52bc5469
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82776431"
---
# <a name="app-startup-in-aspnet-core"></a><span data-ttu-id="57e7d-103">ASP.NET Core 中的應用程式啟動</span><span class="sxs-lookup"><span data-stu-id="57e7d-103">App startup in ASP.NET Core</span></span>

<span data-ttu-id="57e7d-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Tom Dykstra](https://github.com/tdykstra) 以及 [Steve Smith](https://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="57e7d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and [Steve Smith](https://ardalis.com)</span></span>

<span data-ttu-id="57e7d-105">`Startup` 類別可設定服務和應用程式的要求管線。</span><span class="sxs-lookup"><span data-stu-id="57e7d-105">The `Startup` class configures services and the app's request pipeline.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="the-startup-class"></a><span data-ttu-id="57e7d-106">Startup 類別</span><span class="sxs-lookup"><span data-stu-id="57e7d-106">The Startup class</span></span>

<span data-ttu-id="57e7d-107">ASP.NET Core 應用程式使用 `Startup` 類別，其依慣例命名為 `Startup`。</span><span class="sxs-lookup"><span data-stu-id="57e7d-107">ASP.NET Core apps use a `Startup` class, which is named `Startup` by convention.</span></span> <span data-ttu-id="57e7d-108">`Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="57e7d-108">The `Startup` class:</span></span>

* <span data-ttu-id="57e7d-109">選擇性地包含 <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> 方法來設定應用程式的服務\*\*。</span><span class="sxs-lookup"><span data-stu-id="57e7d-109">Optionally includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method to configure the app's *services*.</span></span> <span data-ttu-id="57e7d-110">服務的定義是可提供應用程式功能的可重複使用元件。</span><span class="sxs-lookup"><span data-stu-id="57e7d-110">A service is a reusable component that provides app functionality.</span></span> <span data-ttu-id="57e7d-111">服務會*registered*在中`ConfigureServices`註冊，並透過相依性[插入（DI）](xref:fundamentals/dependency-injection)或<xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>在整個應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="57e7d-111">Services are *registered* in `ConfigureServices` and consumed across the app via [dependency injection (DI)](xref:fundamentals/dependency-injection) or <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>
* <span data-ttu-id="57e7d-112">包含 <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> 方法來建立應用程式的要求處理管線。</span><span class="sxs-lookup"><span data-stu-id="57e7d-112">Includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method to create the app's request processing pipeline.</span></span>

<span data-ttu-id="57e7d-113">應用程式啟動時，ASP.NET Core 執行階段會呼叫 `ConfigureServices` 與 `Configure`：</span><span class="sxs-lookup"><span data-stu-id="57e7d-113">`ConfigureServices` and `Configure` are called by the ASP.NET Core runtime when the app starts:</span></span>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

<span data-ttu-id="57e7d-114">上述範例適用于[ Razor頁面](xref:razor-pages/index);MVC 版本很類似。</span><span class="sxs-lookup"><span data-stu-id="57e7d-114">The preceding sample is for [Razor Pages](xref:razor-pages/index); the MVC version is similar.</span></span>


<span data-ttu-id="57e7d-115">一旦建置應用程式[主機](xref:fundamentals/index#host)，就會指定 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="57e7d-115">The `Startup` class is specified when the app's [host](xref:fundamentals/index#host) is built.</span></span> <span data-ttu-id="57e7d-116">通常`Startup`會藉由在主機建立器上呼叫[WebHostBuilderExtensions\<. UseStartup TStartup>](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*)方法來指定類別：</span><span class="sxs-lookup"><span data-stu-id="57e7d-116">The `Startup` class is typically specified by calling the [WebHostBuilderExtensions.UseStartup\<TStartup>](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) method on the host builder:</span></span>

[!code-csharp[](startup/3.0_samples/Program3.cs?name=snippet_Program&highlight=12)]

<span data-ttu-id="57e7d-117">主機會提供可供 `Startup` 類別建構函式使用的服務。</span><span class="sxs-lookup"><span data-stu-id="57e7d-117">The host provides services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="57e7d-118">應用程式會透過 `ConfigureServices` 新增其他服務。</span><span class="sxs-lookup"><span data-stu-id="57e7d-118">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="57e7d-119">主機和應用程式服務都可以在 `Configure` 和整個應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="57e7d-119">Both the host and app services are available in `Configure` and throughout the app.</span></span>

<span data-ttu-id="57e7d-120">使用[泛型主機](xref:fundamentals/host/generic-host)（<xref:Microsoft.Extensions.Hosting.IHostBuilder>）時，只有下列服務`Startup`類型可以插入至此函式：</span><span class="sxs-lookup"><span data-stu-id="57e7d-120">Only the following service types can be injected into the `Startup` constructor when using the [Generic Host](xref:fundamentals/host/generic-host) (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span></span>

* <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment>
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartUp2.cs?name=snippet)]

<span data-ttu-id="57e7d-121">在呼叫 `Configure` 方法之前，大部分的服務都無法使用。</span><span class="sxs-lookup"><span data-stu-id="57e7d-121">Most services are not available until the `Configure` method is called.</span></span>

### <a name="multiple-startup"></a><span data-ttu-id="57e7d-122">多次啟動</span><span class="sxs-lookup"><span data-stu-id="57e7d-122">Multiple Startup</span></span>

<span data-ttu-id="57e7d-123">應用程式針對不同的環境定義個別的 `Startup` 類別 (例如 `StartupDevelopment`) 時，會在執行階段選取適當的 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="57e7d-123">When the app defines separate `Startup` classes for different environments (for example, `StartupDevelopment`), the appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="57e7d-124">將優先使用其名稱尾碼符合目前環境的類別。</span><span class="sxs-lookup"><span data-stu-id="57e7d-124">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="57e7d-125">如果應用程式是在開發環境中執行，且同時包含 `Startup` 類別和 `StartupDevelopment` 類別，則會使用 `StartupDevelopment` 類別。</span><span class="sxs-lookup"><span data-stu-id="57e7d-125">If the app is run in the Development environment and includes both a `Startup` class and a `StartupDevelopment` class, the `StartupDevelopment` class is used.</span></span> <span data-ttu-id="57e7d-126">如需詳細資訊，請參閱[使用多重環境](xref:fundamentals/environments#environment-based-startup-class-and-methods)。</span><span class="sxs-lookup"><span data-stu-id="57e7d-126">For more information, see [Use multiple environments](xref:fundamentals/environments#environment-based-startup-class-and-methods).</span></span>

<span data-ttu-id="57e7d-127">如需主機的詳細資訊，請參閱[主機](xref:fundamentals/index#host)。</span><span class="sxs-lookup"><span data-stu-id="57e7d-127">See [The host](xref:fundamentals/index#host) for more information on the host.</span></span> <span data-ttu-id="57e7d-128">如需在啟動期間處理錯誤的資訊，請參閱[啟動例外狀況處理](xref:fundamentals/error-handling#startup-exception-handling)。</span><span class="sxs-lookup"><span data-stu-id="57e7d-128">For information on handling errors during startup, see [Startup exception handling](xref:fundamentals/error-handling#startup-exception-handling).</span></span>

## <a name="the-configureservices-method"></a><span data-ttu-id="57e7d-129">ConfigureServices 方法</span><span class="sxs-lookup"><span data-stu-id="57e7d-129">The ConfigureServices method</span></span>

<span data-ttu-id="57e7d-130"><xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> 方法為：</span><span class="sxs-lookup"><span data-stu-id="57e7d-130">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method is:</span></span>

* <span data-ttu-id="57e7d-131">選擇性。</span><span class="sxs-lookup"><span data-stu-id="57e7d-131">Optional.</span></span>
* <span data-ttu-id="57e7d-132">由主機在 `Configure` 方法之前呼叫，來設定應用程式的服務。</span><span class="sxs-lookup"><span data-stu-id="57e7d-132">Called by the host before the `Configure` method to configure the app's services.</span></span>
* <span data-ttu-id="57e7d-133">[組態選項](xref:fundamentals/configuration/index)依慣例設定的位置。</span><span class="sxs-lookup"><span data-stu-id="57e7d-133">Where [configuration options](xref:fundamentals/configuration/index) are set by convention.</span></span>

<span data-ttu-id="57e7d-134">主機可能會在呼叫 `Startup` 方法之前，設定一些服務。</span><span class="sxs-lookup"><span data-stu-id="57e7d-134">The host may configure some services before `Startup` methods are called.</span></span> <span data-ttu-id="57e7d-135">如需詳細資訊，請參閱[主機](xref:fundamentals/index#host)。</span><span class="sxs-lookup"><span data-stu-id="57e7d-135">For more information, see [The host](xref:fundamentals/index#host).</span></span>

<span data-ttu-id="57e7d-136">對於需要大量安裝的功能，可從 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> 上取得 `Add{Service}` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="57e7d-136">For features that require substantial setup, there are `Add{Service}` extension methods on <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>.</span></span> <span data-ttu-id="57e7d-137">例如 **Add**DbContext、**Add**DefaultIdentity、**Add**EntityFrameworkStores 和 **Add**RazorPages：</span><span class="sxs-lookup"><span data-stu-id="57e7d-137">For example, **Add**DbContext, **Add**DefaultIdentity, **Add**EntityFrameworkStores, and **Add**RazorPages:</span></span>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartupIdentity.cs?name=snippet)]

<span data-ttu-id="57e7d-138">將服務新增至服務容器，使其可在應用程式和 `Configure` 方法內使用。</span><span class="sxs-lookup"><span data-stu-id="57e7d-138">Adding services to the service container makes them available within the app and in the `Configure` method.</span></span> <span data-ttu-id="57e7d-139">服務可透過[相依性插入](xref:fundamentals/dependency-injection)或從 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*> 獲得解析。</span><span class="sxs-lookup"><span data-stu-id="57e7d-139">The services are resolved via [dependency injection](xref:fundamentals/dependency-injection) or from <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>

## <a name="the-configure-method"></a><span data-ttu-id="57e7d-140">Configure 方法</span><span class="sxs-lookup"><span data-stu-id="57e7d-140">The Configure method</span></span>

<span data-ttu-id="57e7d-141"><xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> 方法可用來指定應用程式對 HTTP 要求的回應方式。</span><span class="sxs-lookup"><span data-stu-id="57e7d-141">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method is used to specify how the app responds to HTTP requests.</span></span> <span data-ttu-id="57e7d-142">藉由將[中介軟體](xref:fundamentals/middleware/index)元件新增至 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 執行個體，即可設定要求管線。</span><span class="sxs-lookup"><span data-stu-id="57e7d-142">The request pipeline is configured by adding [middleware](xref:fundamentals/middleware/index) components to an <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> instance.</span></span> <span data-ttu-id="57e7d-143">`IApplicationBuilder` 可用於 `Configure` 方法，但它未註冊在服務容器中。</span><span class="sxs-lookup"><span data-stu-id="57e7d-143">`IApplicationBuilder` is available to the `Configure` method, but it isn't registered in the service container.</span></span> <span data-ttu-id="57e7d-144">裝載會建立 `IApplicationBuilder`，並將它直接傳遞到 `Configure`。</span><span class="sxs-lookup"><span data-stu-id="57e7d-144">Hosting creates an `IApplicationBuilder` and passes it directly to `Configure`.</span></span>

<span data-ttu-id="57e7d-145">[ASP.NET Core 範本](/dotnet/core/tools/dotnet-new)會以下列支援設定管線：</span><span class="sxs-lookup"><span data-stu-id="57e7d-145">The [ASP.NET Core templates](/dotnet/core/tools/dotnet-new) configure the pipeline with support for:</span></span>

* [<span data-ttu-id="57e7d-146">開發人員例外狀況頁面</span><span class="sxs-lookup"><span data-stu-id="57e7d-146">Developer Exception Page</span></span>](xref:fundamentals/error-handling#developer-exception-page)
* [<span data-ttu-id="57e7d-147">例外處理常式</span><span class="sxs-lookup"><span data-stu-id="57e7d-147">Exception handler</span></span>](xref:fundamentals/error-handling#exception-handler-page)
* [<span data-ttu-id="57e7d-148">HTTP 嚴格的傳輸安全性 (HSTS)</span><span class="sxs-lookup"><span data-stu-id="57e7d-148">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts)
* [<span data-ttu-id="57e7d-149">HTTPS 重新導向</span><span class="sxs-lookup"><span data-stu-id="57e7d-149">HTTPS redirection</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="57e7d-150">靜態檔案</span><span class="sxs-lookup"><span data-stu-id="57e7d-150">Static files</span></span>](xref:fundamentals/static-files)
* <span data-ttu-id="57e7d-151">ASP.NET Core [MVC](xref:mvc/overview)和[ Razor頁面](xref:razor-pages/index)</span><span class="sxs-lookup"><span data-stu-id="57e7d-151">ASP.NET Core [MVC](xref:mvc/overview) and [Razor Pages](xref:razor-pages/index)</span></span>


[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

<span data-ttu-id="57e7d-152">上述範例適用于[ Razor頁面](xref:razor-pages/index);MVC 版本很類似。</span><span class="sxs-lookup"><span data-stu-id="57e7d-152">The preceding sample is for [Razor Pages](xref:razor-pages/index); the MVC version is similar.</span></span>

<span data-ttu-id="57e7d-153">各 `Use` 擴充方法會將一或多個中介軟體元件新增到要求管線。</span><span class="sxs-lookup"><span data-stu-id="57e7d-153">Each `Use` extension method adds one or more middleware components to the request pipeline.</span></span> <span data-ttu-id="57e7d-154">例如，<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> 會設定[中介軟體](xref:fundamentals/middleware/index)來提供[靜態檔案](xref:fundamentals/static-files)。</span><span class="sxs-lookup"><span data-stu-id="57e7d-154">For instance, <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> configures [middleware](xref:fundamentals/middleware/index) to serve [static files](xref:fundamentals/static-files).</span></span>

<span data-ttu-id="57e7d-155">要求管線中的每個中介軟體元件負責叫用管線中的下一個元件，或於適當時，對鏈結執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="57e7d-155">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the chain, if appropriate.</span></span>

<span data-ttu-id="57e7d-156">`IWebHostEnvironment`、`ILoggerFactory` 或任何在 `ConfigureServices` 中定義的其他服務，也可以在 `Configure` 方法簽章中指定。</span><span class="sxs-lookup"><span data-stu-id="57e7d-156">Additional services, such as `IWebHostEnvironment`, `ILoggerFactory`, or anything defined in `ConfigureServices`, can be specified in the `Configure` method signature.</span></span> <span data-ttu-id="57e7d-157">這些服務在可用時即會插入。</span><span class="sxs-lookup"><span data-stu-id="57e7d-157">These services are injected if they're available.</span></span>

<span data-ttu-id="57e7d-158">如需 `IApplicationBuilder` 的使用方式及中介軟體處理順序的詳細資訊，請參閱 <xref:fundamentals/middleware/index>。</span><span class="sxs-lookup"><span data-stu-id="57e7d-158">For more information on how to use `IApplicationBuilder` and the order of middleware processing, see <xref:fundamentals/middleware/index>.</span></span>

<a name="convenience-methods"></a>

## <a name="configure-services-without-startup"></a><span data-ttu-id="57e7d-159">在不啟動的情況下設定服務</span><span class="sxs-lookup"><span data-stu-id="57e7d-159">Configure services without Startup</span></span>

<span data-ttu-id="57e7d-160">若要設定服務及要求處理管線，且不使用 `Startup` 類別，請在主機建立器上呼叫 `ConfigureServices` 及 `Configure` 便利方法。</span><span class="sxs-lookup"><span data-stu-id="57e7d-160">To configure services and the request processing pipeline without using a `Startup` class, call `ConfigureServices` and `Configure` convenience methods on the host builder.</span></span> <span data-ttu-id="57e7d-161">多次呼叫 `ConfigureServices` 會彼此附加。</span><span class="sxs-lookup"><span data-stu-id="57e7d-161">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="57e7d-162">若多個 `Configure` 方法呼叫存在，則會使用最後的 `Configure` 呼叫。</span><span class="sxs-lookup"><span data-stu-id="57e7d-162">If multiple `Configure` method calls exist, the last `Configure` call is used.</span></span>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program1.cs?name=snippet)]

## <a name="extend-startup-with-startup-filters"></a><span data-ttu-id="57e7d-163">使用啟動篩選條件來擴充啟動</span><span class="sxs-lookup"><span data-stu-id="57e7d-163">Extend Startup with startup filters</span></span>

<span data-ttu-id="57e7d-164">使用<xref:Microsoft.AspNetCore.Hosting.IStartupFilter>：</span><span class="sxs-lookup"><span data-stu-id="57e7d-164">Use <xref:Microsoft.AspNetCore.Hosting.IStartupFilter>:</span></span>

* <span data-ttu-id="57e7d-165">在應用程式[設定](#the-configure-method)中介軟體管線的開頭或結尾設定中介軟體，而不需要明確呼叫`Use{Middleware}`。</span><span class="sxs-lookup"><span data-stu-id="57e7d-165">To configure middleware at the beginning or end of an app's [Configure](#the-configure-method) middleware pipeline without an explicit call to `Use{Middleware}`.</span></span> <span data-ttu-id="57e7d-166">`IStartupFilter`ASP.NET Core 用來將預設值新增至管線的開頭，而不需要讓應用程式作者明確註冊預設中介軟體。</span><span class="sxs-lookup"><span data-stu-id="57e7d-166">`IStartupFilter` is used by ASP.NET Core to add defaults to the beginning of the pipeline without having to make the app author explicitly register the default middleware.</span></span> <span data-ttu-id="57e7d-167">`IStartupFilter`允許代表應用程式作者`Use{Middleware}`進行不同的元件呼叫。</span><span class="sxs-lookup"><span data-stu-id="57e7d-167">`IStartupFilter` allows a different component call `Use{Middleware}` on behalf of the app author.</span></span>
* <span data-ttu-id="57e7d-168">建立`Configure`方法的管線。</span><span class="sxs-lookup"><span data-stu-id="57e7d-168">To create a pipeline of `Configure` methods.</span></span> <span data-ttu-id="57e7d-169">[IStartupFilter.Configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) 可以將中介軟體設為在程式庫新增中介軟體之前或之後執行。</span><span class="sxs-lookup"><span data-stu-id="57e7d-169">[IStartupFilter.Configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) can set a middleware to run before or after middleware added by libraries.</span></span>

<span data-ttu-id="57e7d-170">`IStartupFilter` 會實作 <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>，其會接收並傳回 `Action<IApplicationBuilder>`。</span><span class="sxs-lookup"><span data-stu-id="57e7d-170">`IStartupFilter` implements <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, which receives and returns an `Action<IApplicationBuilder>`.</span></span> <span data-ttu-id="57e7d-171"><xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 會定義類別，以設定應用程式的要求管線。</span><span class="sxs-lookup"><span data-stu-id="57e7d-171">An <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> defines a class to configure an app's request pipeline.</span></span> <span data-ttu-id="57e7d-172">如需詳細資訊，請參閱[使用 IApplicationBuilder 建立中介軟體管線](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)。</span><span class="sxs-lookup"><span data-stu-id="57e7d-172">For more information, see [Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).</span></span>

<span data-ttu-id="57e7d-173">每個 `IStartupFilter` 都可在要求管線中新增一或多個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="57e7d-173">Each `IStartupFilter` can add one or more middlewares in the request pipeline.</span></span> <span data-ttu-id="57e7d-174">篩選條件將依照它們新增至服務容器的順序叫用。</span><span class="sxs-lookup"><span data-stu-id="57e7d-174">The filters are invoked in the order they were added to the service container.</span></span> <span data-ttu-id="57e7d-175">篩選條件可能會在控制權傳給下一個篩選條件之前或之後新增中介軟體，因此它們會附加至應用程式管線的開頭或結尾。</span><span class="sxs-lookup"><span data-stu-id="57e7d-175">Filters may add middleware before or after passing control to the next filter, thus they append to the beginning or end of the app pipeline.</span></span>

<span data-ttu-id="57e7d-176">以下範例示範如何使用 `IStartupFilter` 註冊中介軟體。</span><span class="sxs-lookup"><span data-stu-id="57e7d-176">The following example demonstrates how to register a middleware with `IStartupFilter`.</span></span> <span data-ttu-id="57e7d-177">此 `RequestSetOptionsMiddleware` 中介軟體會從查詢字串參數設定選項值：</span><span class="sxs-lookup"><span data-stu-id="57e7d-177">The `RequestSetOptionsMiddleware` middleware sets an options value from a query string parameter:</span></span>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsMiddleware.cs?name=snippet1)]

<span data-ttu-id="57e7d-178">`RequestSetOptionsMiddleware` 設定在 `RequestSetOptionsStartupFilter` 類別中：</span><span class="sxs-lookup"><span data-stu-id="57e7d-178">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

<span data-ttu-id="57e7d-179">`IStartupFilter` 註冊在 <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> 的服務容器中。</span><span class="sxs-lookup"><span data-stu-id="57e7d-179">The `IStartupFilter` is registered in the service container in <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>.</span></span>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program.cs?name=snippet&highlight=19-20)]

<span data-ttu-id="57e7d-180">提供 `option` 的查詢字串參數時，中介軟體會在 ASP.NET Core 中介軟體轉譯回應之前，先處理值指派。</span><span class="sxs-lookup"><span data-stu-id="57e7d-180">When a query string parameter for `option` is provided, the middleware processes the value assignment before the ASP.NET Core middleware renders the response.</span></span>

<span data-ttu-id="57e7d-181">中介軟體執行順序是依照 `IStartupFilter` 註冊順序來設定：</span><span class="sxs-lookup"><span data-stu-id="57e7d-181">Middleware execution order is set by the order of `IStartupFilter` registrations:</span></span>

* <span data-ttu-id="57e7d-182">多個 `IStartupFilter` 實作可能會與相同的物件互動。</span><span class="sxs-lookup"><span data-stu-id="57e7d-182">Multiple `IStartupFilter` implementations may interact with the same objects.</span></span> <span data-ttu-id="57e7d-183">如果順序很重要，請排列其 `IStartupFilter` 服務註冊順序，以符合其中介軟體執行應該依照的順序。</span><span class="sxs-lookup"><span data-stu-id="57e7d-183">If ordering is important, order their `IStartupFilter` service registrations to match the order that their middlewares should run.</span></span>
* <span data-ttu-id="57e7d-184">程式庫可能使用一或多個 `IStartupFilter` 實作 (其在使用 `IStartupFilter` 註冊的其他應用程式中介軟體之前或之後執行) 來新增中介軟體。</span><span class="sxs-lookup"><span data-stu-id="57e7d-184">Libraries may add middleware with one or more `IStartupFilter` implementations that run before or after other app middleware registered with `IStartupFilter`.</span></span> <span data-ttu-id="57e7d-185">在由程式庫的 `IStartupFilter` 所新增中介軟體之前叫用 `IStartupFilter` 中介軟體：</span><span class="sxs-lookup"><span data-stu-id="57e7d-185">To invoke an `IStartupFilter` middleware before a middleware added by a library's `IStartupFilter`:</span></span>

  * <span data-ttu-id="57e7d-186">先放置服務註冊，再將程式庫新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="57e7d-186">Position the service registration before the library is added to the service container.</span></span>
  * <span data-ttu-id="57e7d-187">若要在之後叫用，請在新增程式庫之後放置服務註冊。</span><span class="sxs-lookup"><span data-stu-id="57e7d-187">To invoke afterward, position the service registration after the library is added.</span></span>

## <a name="add-configuration-at-startup-from-an-external-assembly"></a><span data-ttu-id="57e7d-188">在啟動時從外部組件新增組態</span><span class="sxs-lookup"><span data-stu-id="57e7d-188">Add configuration at startup from an external assembly</span></span>

<span data-ttu-id="57e7d-189"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作允許在啟動時從應用程式 `Startup` 類別外部的外部組件，針對應用程式新增增強功能。</span><span class="sxs-lookup"><span data-stu-id="57e7d-189">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="57e7d-190">如需詳細資訊，請參閱<xref:fundamentals/configuration/platform-specific-configuration>。</span><span class="sxs-lookup"><span data-stu-id="57e7d-190">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="57e7d-191">其他資源</span><span class="sxs-lookup"><span data-stu-id="57e7d-191">Additional resources</span></span>

* [<span data-ttu-id="57e7d-192">主機</span><span class="sxs-lookup"><span data-stu-id="57e7d-192">The host</span></span>](xref:fundamentals/index#host)
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="the-startup-class"></a><span data-ttu-id="57e7d-193">Startup 類別</span><span class="sxs-lookup"><span data-stu-id="57e7d-193">The Startup class</span></span>

<span data-ttu-id="57e7d-194">ASP.NET Core 應用程式使用 `Startup` 類別，其依慣例命名為 `Startup`。</span><span class="sxs-lookup"><span data-stu-id="57e7d-194">ASP.NET Core apps use a `Startup` class, which is named `Startup` by convention.</span></span> <span data-ttu-id="57e7d-195">`Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="57e7d-195">The `Startup` class:</span></span>

* <span data-ttu-id="57e7d-196">選擇性地包含 <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> 方法來設定應用程式的服務\*\*。</span><span class="sxs-lookup"><span data-stu-id="57e7d-196">Optionally includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method to configure the app's *services*.</span></span> <span data-ttu-id="57e7d-197">服務的定義是可提供應用程式功能的可重複使用元件。</span><span class="sxs-lookup"><span data-stu-id="57e7d-197">A service is a reusable component that provides app functionality.</span></span> <span data-ttu-id="57e7d-198">服務會*registered*在中`ConfigureServices`註冊，並透過相依性[插入（DI）](xref:fundamentals/dependency-injection)或<xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>在整個應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="57e7d-198">Services are *registered* in `ConfigureServices` and consumed across the app via [dependency injection (DI)](xref:fundamentals/dependency-injection) or <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>
* <span data-ttu-id="57e7d-199">包含 <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> 方法來建立應用程式的要求處理管線。</span><span class="sxs-lookup"><span data-stu-id="57e7d-199">Includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method to create the app's request processing pipeline.</span></span>

<span data-ttu-id="57e7d-200">應用程式啟動時，ASP.NET Core 執行階段會呼叫 `ConfigureServices` 與 `Configure`：</span><span class="sxs-lookup"><span data-stu-id="57e7d-200">`ConfigureServices` and `Configure` are called by the ASP.NET Core runtime when the app starts:</span></span>

[!code-csharp[](startup/sample_snapshot/Startup1.cs)]

<span data-ttu-id="57e7d-201">一旦建置應用程式[主機](xref:fundamentals/index#host)，就會指定 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="57e7d-201">The `Startup` class is specified when the app's [host](xref:fundamentals/index#host) is built.</span></span> <span data-ttu-id="57e7d-202">通常`Startup`會藉由在主機建立器上呼叫[WebHostBuilderExtensions\<. UseStartup TStartup>](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*)方法來指定類別：</span><span class="sxs-lookup"><span data-stu-id="57e7d-202">The `Startup` class is typically specified by calling the [WebHostBuilderExtensions.UseStartup\<TStartup>](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) method on the host builder:</span></span>

[!code-csharp[](startup/sample_snapshot/Program3.cs?name=snippet_Program&highlight=12)]

<span data-ttu-id="57e7d-203">主機會提供可供 `Startup` 類別建構函式使用的服務。</span><span class="sxs-lookup"><span data-stu-id="57e7d-203">The host provides services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="57e7d-204">應用程式會透過 `ConfigureServices` 新增其他服務。</span><span class="sxs-lookup"><span data-stu-id="57e7d-204">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="57e7d-205">然後，主機和應用程式服務都可以在 `Configure` 和整個應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="57e7d-205">Both the host and app services are then available in `Configure` and throughout the app.</span></span>

<span data-ttu-id="57e7d-206">將[相依性插入](xref:fundamentals/dependency-injection)至 `Startup` 類別的常見用法是插入：</span><span class="sxs-lookup"><span data-stu-id="57e7d-206">A common use of [dependency injection](xref:fundamentals/dependency-injection) into the `Startup` class is to inject:</span></span>

* <span data-ttu-id="57e7d-207"><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> 來根據環境設定服務。</span><span class="sxs-lookup"><span data-stu-id="57e7d-207"><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> to configure services by environment.</span></span>
* <span data-ttu-id="57e7d-208"><xref:Microsoft.Extensions.Configuration.IConfiguration> 來讀取組態。</span><span class="sxs-lookup"><span data-stu-id="57e7d-208"><xref:Microsoft.Extensions.Configuration.IConfiguration> to read configuration.</span></span>
* <span data-ttu-id="57e7d-209"><xref:Microsoft.Extensions.Logging.ILoggerFactory> 來在 `Startup.ConfigureServices` 中建立記錄器。</span><span class="sxs-lookup"><span data-stu-id="57e7d-209"><xref:Microsoft.Extensions.Logging.ILoggerFactory> to create a logger in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](startup/sample_snapshot/Startup2.cs?highlight=7-8)]

<span data-ttu-id="57e7d-210">在呼叫 `Configure` 方法之前，大部分的服務都無法使用。</span><span class="sxs-lookup"><span data-stu-id="57e7d-210">Most services are not available until the `Configure` method is called.</span></span>

### <a name="multiple-startup"></a><span data-ttu-id="57e7d-211">多次啟動</span><span class="sxs-lookup"><span data-stu-id="57e7d-211">Multiple Startup</span></span>

<span data-ttu-id="57e7d-212">應用程式針對不同的環境定義個別的 `Startup` 類別 (例如 `StartupDevelopment`) 時，會在執行階段選取適當的 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="57e7d-212">When the app defines separate `Startup` classes for different environments (for example, `StartupDevelopment`), the appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="57e7d-213">將優先使用其名稱尾碼符合目前環境的類別。</span><span class="sxs-lookup"><span data-stu-id="57e7d-213">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="57e7d-214">如果應用程式是在開發環境中執行，且同時包含 `Startup` 類別和 `StartupDevelopment` 類別，則會使用 `StartupDevelopment` 類別。</span><span class="sxs-lookup"><span data-stu-id="57e7d-214">If the app is run in the Development environment and includes both a `Startup` class and a `StartupDevelopment` class, the `StartupDevelopment` class is used.</span></span> <span data-ttu-id="57e7d-215">如需詳細資訊，請參閱[使用多重環境](xref:fundamentals/environments#environment-based-startup-class-and-methods)。</span><span class="sxs-lookup"><span data-stu-id="57e7d-215">For more information, see [Use multiple environments](xref:fundamentals/environments#environment-based-startup-class-and-methods).</span></span>

<span data-ttu-id="57e7d-216">如需主機的詳細資訊，請參閱[主機](xref:fundamentals/index#host)。</span><span class="sxs-lookup"><span data-stu-id="57e7d-216">See [The host](xref:fundamentals/index#host) for more information on the host.</span></span> <span data-ttu-id="57e7d-217">如需在啟動期間處理錯誤的資訊，請參閱[啟動例外狀況處理](xref:fundamentals/error-handling#startup-exception-handling)。</span><span class="sxs-lookup"><span data-stu-id="57e7d-217">For information on handling errors during startup, see [Startup exception handling](xref:fundamentals/error-handling#startup-exception-handling).</span></span>

## <a name="the-configureservices-method"></a><span data-ttu-id="57e7d-218">ConfigureServices 方法</span><span class="sxs-lookup"><span data-stu-id="57e7d-218">The ConfigureServices method</span></span>

<span data-ttu-id="57e7d-219"><xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> 方法為：</span><span class="sxs-lookup"><span data-stu-id="57e7d-219">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method is:</span></span>

* <span data-ttu-id="57e7d-220">選擇性。</span><span class="sxs-lookup"><span data-stu-id="57e7d-220">Optional.</span></span>
* <span data-ttu-id="57e7d-221">由主機在 `Configure` 方法之前呼叫，來設定應用程式的服務。</span><span class="sxs-lookup"><span data-stu-id="57e7d-221">Called by the host before the `Configure` method to configure the app's services.</span></span>
* <span data-ttu-id="57e7d-222">[組態選項](xref:fundamentals/configuration/index)依慣例設定的位置。</span><span class="sxs-lookup"><span data-stu-id="57e7d-222">Where [configuration options](xref:fundamentals/configuration/index) are set by convention.</span></span>

<span data-ttu-id="57e7d-223">主機可能會在呼叫 `Startup` 方法之前，設定一些服務。</span><span class="sxs-lookup"><span data-stu-id="57e7d-223">The host may configure some services before `Startup` methods are called.</span></span> <span data-ttu-id="57e7d-224">如需詳細資訊，請參閱[主機](xref:fundamentals/index#host)。</span><span class="sxs-lookup"><span data-stu-id="57e7d-224">For more information, see [The host](xref:fundamentals/index#host).</span></span>

<span data-ttu-id="57e7d-225">對於需要大量安裝的功能，可從 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> 上取得 `Add{Service}` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="57e7d-225">For features that require substantial setup, there are `Add{Service}` extension methods on <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>.</span></span> <span data-ttu-id="57e7d-226">例如 **Add**DbContext、**Add**DefaultIdentity、**Add**EntityFrameworkStores 和 **Add**RazorPages：</span><span class="sxs-lookup"><span data-stu-id="57e7d-226">For example, **Add**DbContext, **Add**DefaultIdentity, **Add**EntityFrameworkStores, and **Add**RazorPages:</span></span>

[!code-csharp[](startup/sample_snapshot/Startup3.cs)]

<span data-ttu-id="57e7d-227">將服務新增至服務容器，使其可在應用程式和 `Configure` 方法內使用。</span><span class="sxs-lookup"><span data-stu-id="57e7d-227">Adding services to the service container makes them available within the app and in the `Configure` method.</span></span> <span data-ttu-id="57e7d-228">服務可透過[相依性插入](xref:fundamentals/dependency-injection)或從 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*> 獲得解析。</span><span class="sxs-lookup"><span data-stu-id="57e7d-228">The services are resolved via [dependency injection](xref:fundamentals/dependency-injection) or from <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>

<span data-ttu-id="57e7d-229">請參閱 [SetCompatibilityVersion](xref:mvc/compatibility-version) 以取得 `SetCompatibilityVersion` 的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="57e7d-229">See [SetCompatibilityVersion](xref:mvc/compatibility-version) for more information on `SetCompatibilityVersion`.</span></span>

## <a name="the-configure-method"></a><span data-ttu-id="57e7d-230">Configure 方法</span><span class="sxs-lookup"><span data-stu-id="57e7d-230">The Configure method</span></span>

<span data-ttu-id="57e7d-231"><xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> 方法可用來指定應用程式對 HTTP 要求的回應方式。</span><span class="sxs-lookup"><span data-stu-id="57e7d-231">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method is used to specify how the app responds to HTTP requests.</span></span> <span data-ttu-id="57e7d-232">藉由將[中介軟體](xref:fundamentals/middleware/index)元件新增至 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 執行個體，即可設定要求管線。</span><span class="sxs-lookup"><span data-stu-id="57e7d-232">The request pipeline is configured by adding [middleware](xref:fundamentals/middleware/index) components to an <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> instance.</span></span> <span data-ttu-id="57e7d-233">`IApplicationBuilder` 可用於 `Configure` 方法，但它未註冊在服務容器中。</span><span class="sxs-lookup"><span data-stu-id="57e7d-233">`IApplicationBuilder` is available to the `Configure` method, but it isn't registered in the service container.</span></span> <span data-ttu-id="57e7d-234">裝載會建立 `IApplicationBuilder`，並將它直接傳遞到 `Configure`。</span><span class="sxs-lookup"><span data-stu-id="57e7d-234">Hosting creates an `IApplicationBuilder` and passes it directly to `Configure`.</span></span>

<span data-ttu-id="57e7d-235">[ASP.NET Core 範本](/dotnet/core/tools/dotnet-new)會以下列支援設定管線：</span><span class="sxs-lookup"><span data-stu-id="57e7d-235">The [ASP.NET Core templates](/dotnet/core/tools/dotnet-new) configure the pipeline with support for:</span></span>

* [<span data-ttu-id="57e7d-236">開發人員例外狀況頁面</span><span class="sxs-lookup"><span data-stu-id="57e7d-236">Developer Exception Page</span></span>](xref:fundamentals/error-handling#developer-exception-page)
* [<span data-ttu-id="57e7d-237">例外處理常式</span><span class="sxs-lookup"><span data-stu-id="57e7d-237">Exception handler</span></span>](xref:fundamentals/error-handling#exception-handler-page)
* [<span data-ttu-id="57e7d-238">HTTP 嚴格的傳輸安全性 (HSTS)</span><span class="sxs-lookup"><span data-stu-id="57e7d-238">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts)
* [<span data-ttu-id="57e7d-239">HTTPS 重新導向</span><span class="sxs-lookup"><span data-stu-id="57e7d-239">HTTPS redirection</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="57e7d-240">靜態檔案</span><span class="sxs-lookup"><span data-stu-id="57e7d-240">Static files</span></span>](xref:fundamentals/static-files)
* <span data-ttu-id="57e7d-241">ASP.NET Core [MVC](xref:mvc/overview)和[ Razor頁面](xref:razor-pages/index)</span><span class="sxs-lookup"><span data-stu-id="57e7d-241">ASP.NET Core [MVC](xref:mvc/overview) and [Razor Pages](xref:razor-pages/index)</span></span>
* [<span data-ttu-id="57e7d-242">一般資料保護規定 (GDPR)</span><span class="sxs-lookup"><span data-stu-id="57e7d-242">General Data Protection Regulation (GDPR)</span></span>](xref:security/gdpr)

[!code-csharp[](startup/sample_snapshot/Startup4.cs)]

<span data-ttu-id="57e7d-243">各 `Use` 擴充方法會將一或多個中介軟體元件新增到要求管線。</span><span class="sxs-lookup"><span data-stu-id="57e7d-243">Each `Use` extension method adds one or more middleware components to the request pipeline.</span></span> <span data-ttu-id="57e7d-244">例如，<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> 會設定[中介軟體](xref:fundamentals/middleware/index)來提供[靜態檔案](xref:fundamentals/static-files)。</span><span class="sxs-lookup"><span data-stu-id="57e7d-244">For instance, <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> configures [middleware](xref:fundamentals/middleware/index) to serve [static files](xref:fundamentals/static-files).</span></span>

<span data-ttu-id="57e7d-245">要求管線中的每個中介軟體元件負責叫用管線中的下一個元件，或於適當時，對鏈結執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="57e7d-245">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the chain, if appropriate.</span></span>

<span data-ttu-id="57e7d-246">`IHostingEnvironment` 和 `ILoggerFactory` 等其他服務，或任何在 `ConfigureServices` 中定義的項目，也可以在 `Configure` 方法簽章中指定。</span><span class="sxs-lookup"><span data-stu-id="57e7d-246">Additional services, such as `IHostingEnvironment` and `ILoggerFactory`, or anything defined in `ConfigureServices`, can be specified in the `Configure` method signature.</span></span> <span data-ttu-id="57e7d-247">這些服務在可用時即會插入。</span><span class="sxs-lookup"><span data-stu-id="57e7d-247">These services are injected if they're available.</span></span>

<span data-ttu-id="57e7d-248">如需 `IApplicationBuilder` 的使用方式及中介軟體處理順序的詳細資訊，請參閱 <xref:fundamentals/middleware/index>。</span><span class="sxs-lookup"><span data-stu-id="57e7d-248">For more information on how to use `IApplicationBuilder` and the order of middleware processing, see <xref:fundamentals/middleware/index>.</span></span>

<a name="convenience-methods"></a>

## <a name="configure-services-without-startup"></a><span data-ttu-id="57e7d-249">在不啟動的情況下設定服務</span><span class="sxs-lookup"><span data-stu-id="57e7d-249">Configure services without Startup</span></span>

<span data-ttu-id="57e7d-250">若要設定服務及要求處理管線，且不使用 `Startup` 類別，請在主機建立器上呼叫 `ConfigureServices` 及 `Configure` 便利方法。</span><span class="sxs-lookup"><span data-stu-id="57e7d-250">To configure services and the request processing pipeline without using a `Startup` class, call `ConfigureServices` and `Configure` convenience methods on the host builder.</span></span> <span data-ttu-id="57e7d-251">多次呼叫 `ConfigureServices` 會彼此附加。</span><span class="sxs-lookup"><span data-stu-id="57e7d-251">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="57e7d-252">若多個 `Configure` 方法呼叫存在，則會使用最後的 `Configure` 呼叫。</span><span class="sxs-lookup"><span data-stu-id="57e7d-252">If multiple `Configure` method calls exist, the last `Configure` call is used.</span></span>

[!code-csharp[](startup/sample_snapshot/Program1.cs?highlight=16,20)]

## <a name="extend-startup-with-startup-filters"></a><span data-ttu-id="57e7d-253">使用啟動篩選條件來擴充啟動</span><span class="sxs-lookup"><span data-stu-id="57e7d-253">Extend Startup with startup filters</span></span>

<span data-ttu-id="57e7d-254">使用<xref:Microsoft.AspNetCore.Hosting.IStartupFilter>：</span><span class="sxs-lookup"><span data-stu-id="57e7d-254">Use <xref:Microsoft.AspNetCore.Hosting.IStartupFilter>:</span></span>

* <span data-ttu-id="57e7d-255">在應用程式[設定](#the-configure-method)中介軟體管線的開頭或結尾設定中介軟體，而不需要明確呼叫`Use{Middleware}`。</span><span class="sxs-lookup"><span data-stu-id="57e7d-255">To configure middleware at the beginning or end of an app's [Configure](#the-configure-method) middleware pipeline without an explicit call to `Use{Middleware}`.</span></span> <span data-ttu-id="57e7d-256">`IStartupFilter`ASP.NET Core 用來將預設值新增至管線的開頭，而不需要讓應用程式作者明確註冊預設中介軟體。</span><span class="sxs-lookup"><span data-stu-id="57e7d-256">`IStartupFilter` is used by ASP.NET Core to add defaults to the beginning of the pipeline without having to make the app author explicitly register the default middleware.</span></span> <span data-ttu-id="57e7d-257">`IStartupFilter`允許代表應用程式作者`Use{Middleware}`進行不同的元件呼叫。</span><span class="sxs-lookup"><span data-stu-id="57e7d-257">`IStartupFilter` allows a different component call `Use{Middleware}` on behalf of the app author.</span></span>
* <span data-ttu-id="57e7d-258">建立`Configure`方法的管線。</span><span class="sxs-lookup"><span data-stu-id="57e7d-258">To create a pipeline of `Configure` methods.</span></span> <span data-ttu-id="57e7d-259">[IStartupFilter.Configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) 可以將中介軟體設為在程式庫新增中介軟體之前或之後執行。</span><span class="sxs-lookup"><span data-stu-id="57e7d-259">[IStartupFilter.Configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) can set a middleware to run before or after middleware added by libraries.</span></span>

<span data-ttu-id="57e7d-260">`IStartupFilter` 會實作 <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>，其會接收並傳回 `Action<IApplicationBuilder>`。</span><span class="sxs-lookup"><span data-stu-id="57e7d-260">`IStartupFilter` implements <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, which receives and returns an `Action<IApplicationBuilder>`.</span></span> <span data-ttu-id="57e7d-261"><xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 會定義類別，以設定應用程式的要求管線。</span><span class="sxs-lookup"><span data-stu-id="57e7d-261">An <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> defines a class to configure an app's request pipeline.</span></span> <span data-ttu-id="57e7d-262">如需詳細資訊，請參閱[使用 IApplicationBuilder 建立中介軟體管線](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)。</span><span class="sxs-lookup"><span data-stu-id="57e7d-262">For more information, see [Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).</span></span>

<span data-ttu-id="57e7d-263">每個 `IStartupFilter` 都可在要求管線中新增一或多個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="57e7d-263">Each `IStartupFilter` can add one or more middlewares in the request pipeline.</span></span> <span data-ttu-id="57e7d-264">篩選條件將依照它們新增至服務容器的順序叫用。</span><span class="sxs-lookup"><span data-stu-id="57e7d-264">The filters are invoked in the order they were added to the service container.</span></span> <span data-ttu-id="57e7d-265">篩選條件可能會在控制權傳給下一個篩選條件之前或之後新增中介軟體，因此它們會附加至應用程式管線的開頭或結尾。</span><span class="sxs-lookup"><span data-stu-id="57e7d-265">Filters may add middleware before or after passing control to the next filter, thus they append to the beginning or end of the app pipeline.</span></span>

<span data-ttu-id="57e7d-266">以下範例示範如何使用 `IStartupFilter` 註冊中介軟體。</span><span class="sxs-lookup"><span data-stu-id="57e7d-266">The following example demonstrates how to register a middleware with `IStartupFilter`.</span></span> <span data-ttu-id="57e7d-267">此 `RequestSetOptionsMiddleware` 中介軟體會從查詢字串參數設定選項值：</span><span class="sxs-lookup"><span data-stu-id="57e7d-267">The `RequestSetOptionsMiddleware` middleware sets an options value from a query string parameter:</span></span>

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsMiddleware.cs?name=snippet1&highlight=21)]

<span data-ttu-id="57e7d-268">`RequestSetOptionsMiddleware` 設定在 `RequestSetOptionsStartupFilter` 類別中：</span><span class="sxs-lookup"><span data-stu-id="57e7d-268">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

<span data-ttu-id="57e7d-269">`IStartupFilter` 註冊在 <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> 的服務容器中。</span><span class="sxs-lookup"><span data-stu-id="57e7d-269">The `IStartupFilter` is registered in the service container in <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>.</span></span>

[!code-csharp[](startup/sample_snapshot/Program2.cs?name=snippet1&highlight=4-5)]

<span data-ttu-id="57e7d-270">提供 `option` 的查詢字串參數時，中介軟體會在 ASP.NET Core 中介軟體轉譯回應之前，先處理值指派。</span><span class="sxs-lookup"><span data-stu-id="57e7d-270">When a query string parameter for `option` is provided, the middleware processes the value assignment before the ASP.NET Core middleware renders the response.</span></span>

<span data-ttu-id="57e7d-271">中介軟體執行順序是依照 `IStartupFilter` 註冊順序來設定：</span><span class="sxs-lookup"><span data-stu-id="57e7d-271">Middleware execution order is set by the order of `IStartupFilter` registrations:</span></span>

* <span data-ttu-id="57e7d-272">多個 `IStartupFilter` 實作可能會與相同的物件互動。</span><span class="sxs-lookup"><span data-stu-id="57e7d-272">Multiple `IStartupFilter` implementations may interact with the same objects.</span></span> <span data-ttu-id="57e7d-273">如果順序很重要，請排列其 `IStartupFilter` 服務註冊順序，以符合其中介軟體執行應該依照的順序。</span><span class="sxs-lookup"><span data-stu-id="57e7d-273">If ordering is important, order their `IStartupFilter` service registrations to match the order that their middlewares should run.</span></span>
* <span data-ttu-id="57e7d-274">程式庫可能使用一或多個 `IStartupFilter` 實作 (其在使用 `IStartupFilter` 註冊的其他應用程式中介軟體之前或之後執行) 來新增中介軟體。</span><span class="sxs-lookup"><span data-stu-id="57e7d-274">Libraries may add middleware with one or more `IStartupFilter` implementations that run before or after other app middleware registered with `IStartupFilter`.</span></span> <span data-ttu-id="57e7d-275">在由程式庫的 `IStartupFilter` 所新增中介軟體之前叫用 `IStartupFilter` 中介軟體：</span><span class="sxs-lookup"><span data-stu-id="57e7d-275">To invoke an `IStartupFilter` middleware before a middleware added by a library's `IStartupFilter`:</span></span>

  * <span data-ttu-id="57e7d-276">先放置服務註冊，再將程式庫新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="57e7d-276">Position the service registration before the library is added to the service container.</span></span>
  * <span data-ttu-id="57e7d-277">若要在之後叫用，請在新增程式庫之後放置服務註冊。</span><span class="sxs-lookup"><span data-stu-id="57e7d-277">To invoke afterward, position the service registration after the library is added.</span></span>

## <a name="add-configuration-at-startup-from-an-external-assembly"></a><span data-ttu-id="57e7d-278">在啟動時從外部組件新增組態</span><span class="sxs-lookup"><span data-stu-id="57e7d-278">Add configuration at startup from an external assembly</span></span>

<span data-ttu-id="57e7d-279"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作允許在啟動時從應用程式 `Startup` 類別外部的外部組件，針對應用程式新增增強功能。</span><span class="sxs-lookup"><span data-stu-id="57e7d-279">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="57e7d-280">如需詳細資訊，請參閱<xref:fundamentals/configuration/platform-specific-configuration>。</span><span class="sxs-lookup"><span data-stu-id="57e7d-280">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="57e7d-281">其他資源</span><span class="sxs-lookup"><span data-stu-id="57e7d-281">Additional resources</span></span>

* [<span data-ttu-id="57e7d-282">主機</span><span class="sxs-lookup"><span data-stu-id="57e7d-282">The host</span></span>](xref:fundamentals/index#host)
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>

::: moniker-end