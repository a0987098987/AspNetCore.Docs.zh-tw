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
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/startup
ms.openlocfilehash: 0d1bf0dd23b7f3b99d6e9383ffc72291c1b138ec
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85400709"
---
# <a name="app-startup-in-aspnet-core"></a><span data-ttu-id="a8ea6-103">ASP.NET Core 中的應用程式啟動</span><span class="sxs-lookup"><span data-stu-id="a8ea6-103">App startup in ASP.NET Core</span></span>

<span data-ttu-id="a8ea6-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Tom Dykstra](https://github.com/tdykstra) 以及 [Steve Smith](https://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="a8ea6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and [Steve Smith](https://ardalis.com)</span></span>

<span data-ttu-id="a8ea6-105">`Startup` 類別可設定服務和應用程式的要求管線。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-105">The `Startup` class configures services and the app's request pipeline.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="the-startup-class"></a><span data-ttu-id="a8ea6-106">Startup 類別</span><span class="sxs-lookup"><span data-stu-id="a8ea6-106">The Startup class</span></span>

<span data-ttu-id="a8ea6-107">ASP.NET Core 應用程式使用 `Startup` 類別，其依慣例命名為 `Startup`。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-107">ASP.NET Core apps use a `Startup` class, which is named `Startup` by convention.</span></span> <span data-ttu-id="a8ea6-108">`Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="a8ea6-108">The `Startup` class:</span></span>

* <span data-ttu-id="a8ea6-109">選擇性地包含 <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> 方法來設定應用程式的服務\*\*。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-109">Optionally includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method to configure the app's *services*.</span></span> <span data-ttu-id="a8ea6-110">服務的定義是可提供應用程式功能的可重複使用元件。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-110">A service is a reusable component that provides app functionality.</span></span> <span data-ttu-id="a8ea6-111">服務會在中*註冊*，並透過相依性 `ConfigureServices` [插入（DI）](xref:fundamentals/dependency-injection)或在整個應用程式中使用 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*> 。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-111">Services are *registered* in `ConfigureServices` and consumed across the app via [dependency injection (DI)](xref:fundamentals/dependency-injection) or <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>
* <span data-ttu-id="a8ea6-112">包含 <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> 方法來建立應用程式的要求處理管線。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-112">Includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method to create the app's request processing pipeline.</span></span>

<span data-ttu-id="a8ea6-113">應用程式啟動時，ASP.NET Core 執行階段會呼叫 `ConfigureServices` 與 `Configure`：</span><span class="sxs-lookup"><span data-stu-id="a8ea6-113">`ConfigureServices` and `Configure` are called by the ASP.NET Core runtime when the app starts:</span></span>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

<span data-ttu-id="a8ea6-114">上述範例適用于[ Razor 頁面](xref:razor-pages/index); MVC 版本類似。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-114">The preceding sample is for [Razor Pages](xref:razor-pages/index); the MVC version is similar.</span></span>


<span data-ttu-id="a8ea6-115">一旦建置應用程式[主機](xref:fundamentals/index#host)，就會指定 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-115">The `Startup` class is specified when the app's [host](xref:fundamentals/index#host) is built.</span></span> <span data-ttu-id="a8ea6-116">`Startup`通常會藉由呼叫主機產生器上的[WebHostBuilderExtensions \<TStartup> UseStartup](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*)方法來指定類別：</span><span class="sxs-lookup"><span data-stu-id="a8ea6-116">The `Startup` class is typically specified by calling the [WebHostBuilderExtensions.UseStartup\<TStartup>](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) method on the host builder:</span></span>

[!code-csharp[](startup/3.0_samples/Program3.cs?name=snippet_Program&highlight=12)]

<span data-ttu-id="a8ea6-117">主機會提供可供 `Startup` 類別建構函式使用的服務。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-117">The host provides services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="a8ea6-118">應用程式會透過 `ConfigureServices` 新增其他服務。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-118">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="a8ea6-119">主機和應用程式服務都可以在 `Configure` 和整個應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-119">Both the host and app services are available in `Configure` and throughout the app.</span></span>

<span data-ttu-id="a8ea6-120">`Startup`使用[泛型主機](xref:fundamentals/host/generic-host)（）時，只有下列服務類型可以插入至此函式 <xref:Microsoft.Extensions.Hosting.IHostBuilder> ：</span><span class="sxs-lookup"><span data-stu-id="a8ea6-120">Only the following service types can be injected into the `Startup` constructor when using the [Generic Host](xref:fundamentals/host/generic-host) (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span></span>

* <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment>
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartUp2.cs?name=snippet)]

<span data-ttu-id="a8ea6-121">在呼叫 `Configure` 方法之前，大部分的服務都無法使用。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-121">Most services are not available until the `Configure` method is called.</span></span>

### <a name="multiple-startup"></a><span data-ttu-id="a8ea6-122">多次啟動</span><span class="sxs-lookup"><span data-stu-id="a8ea6-122">Multiple Startup</span></span>

<span data-ttu-id="a8ea6-123">應用程式針對不同的環境定義個別的 `Startup` 類別 (例如 `StartupDevelopment`) 時，會在執行階段選取適當的 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-123">When the app defines separate `Startup` classes for different environments (for example, `StartupDevelopment`), the appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="a8ea6-124">將優先使用其名稱尾碼符合目前環境的類別。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-124">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="a8ea6-125">如果應用程式是在開發環境中執行，且同時包含 `Startup` 類別和 `StartupDevelopment` 類別，則會使用 `StartupDevelopment` 類別。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-125">If the app is run in the Development environment and includes both a `Startup` class and a `StartupDevelopment` class, the `StartupDevelopment` class is used.</span></span> <span data-ttu-id="a8ea6-126">如需詳細資訊，請參閱[使用多重環境](xref:fundamentals/environments#environment-based-startup-class-and-methods)。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-126">For more information, see [Use multiple environments](xref:fundamentals/environments#environment-based-startup-class-and-methods).</span></span>

<span data-ttu-id="a8ea6-127">如需主機的詳細資訊，請參閱[主機](xref:fundamentals/index#host)。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-127">See [The host](xref:fundamentals/index#host) for more information on the host.</span></span> <span data-ttu-id="a8ea6-128">如需在啟動期間處理錯誤的資訊，請參閱[啟動例外狀況處理](xref:fundamentals/error-handling#startup-exception-handling)。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-128">For information on handling errors during startup, see [Startup exception handling](xref:fundamentals/error-handling#startup-exception-handling).</span></span>

## <a name="the-configureservices-method"></a><span data-ttu-id="a8ea6-129">ConfigureServices 方法</span><span class="sxs-lookup"><span data-stu-id="a8ea6-129">The ConfigureServices method</span></span>

<span data-ttu-id="a8ea6-130"><xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> 方法為：</span><span class="sxs-lookup"><span data-stu-id="a8ea6-130">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method is:</span></span>

* <span data-ttu-id="a8ea6-131">選擇性。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-131">Optional.</span></span>
* <span data-ttu-id="a8ea6-132">由主機在 `Configure` 方法之前呼叫，來設定應用程式的服務。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-132">Called by the host before the `Configure` method to configure the app's services.</span></span>
* <span data-ttu-id="a8ea6-133">[組態選項](xref:fundamentals/configuration/index)依慣例設定的位置。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-133">Where [configuration options](xref:fundamentals/configuration/index) are set by convention.</span></span>

<span data-ttu-id="a8ea6-134">主機可能會在呼叫 `Startup` 方法之前，設定一些服務。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-134">The host may configure some services before `Startup` methods are called.</span></span> <span data-ttu-id="a8ea6-135">如需詳細資訊，請參閱[主機](xref:fundamentals/index#host)。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-135">For more information, see [The host](xref:fundamentals/index#host).</span></span>

<span data-ttu-id="a8ea6-136">對於需要大量安裝的功能，可從 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> 上取得 `Add{Service}` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-136">For features that require substantial setup, there are `Add{Service}` extension methods on <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>.</span></span> <span data-ttu-id="a8ea6-137">例如 **Add**DbContext、**Add**DefaultIdentity、**Add**EntityFrameworkStores 和 **Add**RazorPages：</span><span class="sxs-lookup"><span data-stu-id="a8ea6-137">For example, **Add**DbContext, **Add**DefaultIdentity, **Add**EntityFrameworkStores, and **Add**RazorPages:</span></span>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartupIdentity.cs?name=snippet)]

<span data-ttu-id="a8ea6-138">將服務新增至服務容器，使其可在應用程式和 `Configure` 方法內使用。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-138">Adding services to the service container makes them available within the app and in the `Configure` method.</span></span> <span data-ttu-id="a8ea6-139">服務可透過[相依性插入](xref:fundamentals/dependency-injection)或從 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*> 獲得解析。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-139">The services are resolved via [dependency injection](xref:fundamentals/dependency-injection) or from <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>

## <a name="the-configure-method"></a><span data-ttu-id="a8ea6-140">Configure 方法</span><span class="sxs-lookup"><span data-stu-id="a8ea6-140">The Configure method</span></span>

<span data-ttu-id="a8ea6-141"><xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> 方法可用來指定應用程式對 HTTP 要求的回應方式。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-141">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method is used to specify how the app responds to HTTP requests.</span></span> <span data-ttu-id="a8ea6-142">藉由將[中介軟體](xref:fundamentals/middleware/index)元件新增至 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 執行個體，即可設定要求管線。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-142">The request pipeline is configured by adding [middleware](xref:fundamentals/middleware/index) components to an <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> instance.</span></span> <span data-ttu-id="a8ea6-143">`IApplicationBuilder` 可用於 `Configure` 方法，但它未註冊在服務容器中。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-143">`IApplicationBuilder` is available to the `Configure` method, but it isn't registered in the service container.</span></span> <span data-ttu-id="a8ea6-144">裝載會建立 `IApplicationBuilder`，並將它直接傳遞到 `Configure`。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-144">Hosting creates an `IApplicationBuilder` and passes it directly to `Configure`.</span></span>

<span data-ttu-id="a8ea6-145">[ASP.NET Core 範本](/dotnet/core/tools/dotnet-new)會以下列支援設定管線：</span><span class="sxs-lookup"><span data-stu-id="a8ea6-145">The [ASP.NET Core templates](/dotnet/core/tools/dotnet-new) configure the pipeline with support for:</span></span>

* [<span data-ttu-id="a8ea6-146">開發人員例外狀況頁面</span><span class="sxs-lookup"><span data-stu-id="a8ea6-146">Developer Exception Page</span></span>](xref:fundamentals/error-handling#developer-exception-page)
* [<span data-ttu-id="a8ea6-147">例外處理常式</span><span class="sxs-lookup"><span data-stu-id="a8ea6-147">Exception handler</span></span>](xref:fundamentals/error-handling#exception-handler-page)
* [<span data-ttu-id="a8ea6-148">HTTP 嚴格的傳輸安全性 (HSTS)</span><span class="sxs-lookup"><span data-stu-id="a8ea6-148">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts)
* [<span data-ttu-id="a8ea6-149">HTTPS 重新導向</span><span class="sxs-lookup"><span data-stu-id="a8ea6-149">HTTPS redirection</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="a8ea6-150">靜態檔案</span><span class="sxs-lookup"><span data-stu-id="a8ea6-150">Static files</span></span>](xref:fundamentals/static-files)
* <span data-ttu-id="a8ea6-151">ASP.NET Core [MVC](xref:mvc/overview)和[ Razor 頁面](xref:razor-pages/index)</span><span class="sxs-lookup"><span data-stu-id="a8ea6-151">ASP.NET Core [MVC](xref:mvc/overview) and [Razor Pages](xref:razor-pages/index)</span></span>


[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

<span data-ttu-id="a8ea6-152">上述範例適用于[ Razor 頁面](xref:razor-pages/index); MVC 版本類似。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-152">The preceding sample is for [Razor Pages](xref:razor-pages/index); the MVC version is similar.</span></span>

<span data-ttu-id="a8ea6-153">各 `Use` 擴充方法會將一或多個中介軟體元件新增到要求管線。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-153">Each `Use` extension method adds one or more middleware components to the request pipeline.</span></span> <span data-ttu-id="a8ea6-154">例如，<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> 會設定[中介軟體](xref:fundamentals/middleware/index)來提供[靜態檔案](xref:fundamentals/static-files)。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-154">For instance, <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> configures [middleware](xref:fundamentals/middleware/index) to serve [static files](xref:fundamentals/static-files).</span></span>

<span data-ttu-id="a8ea6-155">要求管線中的每個中介軟體元件負責叫用管線中的下一個元件，或於適當時，對鏈結執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-155">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the chain, if appropriate.</span></span>

<span data-ttu-id="a8ea6-156">`IWebHostEnvironment`、`ILoggerFactory` 或任何在 `ConfigureServices` 中定義的其他服務，也可以在 `Configure` 方法簽章中指定。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-156">Additional services, such as `IWebHostEnvironment`, `ILoggerFactory`, or anything defined in `ConfigureServices`, can be specified in the `Configure` method signature.</span></span> <span data-ttu-id="a8ea6-157">這些服務在可用時即會插入。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-157">These services are injected if they're available.</span></span>

<span data-ttu-id="a8ea6-158">如需 `IApplicationBuilder` 的使用方式及中介軟體處理順序的詳細資訊，請參閱 <xref:fundamentals/middleware/index>。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-158">For more information on how to use `IApplicationBuilder` and the order of middleware processing, see <xref:fundamentals/middleware/index>.</span></span>

<a name="convenience-methods"></a>

## <a name="configure-services-without-startup"></a><span data-ttu-id="a8ea6-159">在不啟動的情況下設定服務</span><span class="sxs-lookup"><span data-stu-id="a8ea6-159">Configure services without Startup</span></span>

<span data-ttu-id="a8ea6-160">若要設定服務及要求處理管線，且不使用 `Startup` 類別，請在主機建立器上呼叫 `ConfigureServices` 及 `Configure` 便利方法。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-160">To configure services and the request processing pipeline without using a `Startup` class, call `ConfigureServices` and `Configure` convenience methods on the host builder.</span></span> <span data-ttu-id="a8ea6-161">多次呼叫 `ConfigureServices` 會彼此附加。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-161">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="a8ea6-162">若多個 `Configure` 方法呼叫存在，則會使用最後的 `Configure` 呼叫。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-162">If multiple `Configure` method calls exist, the last `Configure` call is used.</span></span>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program1.cs?name=snippet)]

## <a name="extend-startup-with-startup-filters"></a><span data-ttu-id="a8ea6-163">使用啟動篩選條件來擴充啟動</span><span class="sxs-lookup"><span data-stu-id="a8ea6-163">Extend Startup with startup filters</span></span>

<span data-ttu-id="a8ea6-164">使用 <xref:Microsoft.AspNetCore.Hosting.IStartupFilter> ：</span><span class="sxs-lookup"><span data-stu-id="a8ea6-164">Use <xref:Microsoft.AspNetCore.Hosting.IStartupFilter>:</span></span>

* <span data-ttu-id="a8ea6-165">在應用程式[設定](#the-configure-method)中介軟體管線的開頭或結尾設定中介軟體，而不需要明確呼叫 `Use{Middleware}` 。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-165">To configure middleware at the beginning or end of an app's [Configure](#the-configure-method) middleware pipeline without an explicit call to `Use{Middleware}`.</span></span> <span data-ttu-id="a8ea6-166">`IStartupFilter`ASP.NET Core 用來將預設值新增至管線的開頭，而不需要讓應用程式作者明確註冊預設中介軟體。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-166">`IStartupFilter` is used by ASP.NET Core to add defaults to the beginning of the pipeline without having to make the app author explicitly register the default middleware.</span></span> <span data-ttu-id="a8ea6-167">`IStartupFilter`允許代表應用程式作者進行不同的元件呼叫 `Use{Middleware}` 。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-167">`IStartupFilter` allows a different component call `Use{Middleware}` on behalf of the app author.</span></span>
* <span data-ttu-id="a8ea6-168">建立方法的管線 `Configure` 。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-168">To create a pipeline of `Configure` methods.</span></span> <span data-ttu-id="a8ea6-169">[IStartupFilter.Configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) 可以將中介軟體設為在程式庫新增中介軟體之前或之後執行。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-169">[IStartupFilter.Configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) can set a middleware to run before or after middleware added by libraries.</span></span>

<span data-ttu-id="a8ea6-170">`IStartupFilter` 會實作 <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>，其會接收並傳回 `Action<IApplicationBuilder>`。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-170">`IStartupFilter` implements <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, which receives and returns an `Action<IApplicationBuilder>`.</span></span> <span data-ttu-id="a8ea6-171"><xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 會定義類別，以設定應用程式的要求管線。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-171">An <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> defines a class to configure an app's request pipeline.</span></span> <span data-ttu-id="a8ea6-172">如需詳細資訊，請參閱[使用 IApplicationBuilder 建立中介軟體管線](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-172">For more information, see [Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).</span></span>

<span data-ttu-id="a8ea6-173">每個 `IStartupFilter` 都可在要求管線中新增一或多個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-173">Each `IStartupFilter` can add one or more middlewares in the request pipeline.</span></span> <span data-ttu-id="a8ea6-174">篩選條件將依照它們新增至服務容器的順序叫用。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-174">The filters are invoked in the order they were added to the service container.</span></span> <span data-ttu-id="a8ea6-175">篩選條件可能會在控制權傳給下一個篩選條件之前或之後新增中介軟體，因此它們會附加至應用程式管線的開頭或結尾。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-175">Filters may add middleware before or after passing control to the next filter, thus they append to the beginning or end of the app pipeline.</span></span>

<span data-ttu-id="a8ea6-176">以下範例示範如何使用 `IStartupFilter` 註冊中介軟體。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-176">The following example demonstrates how to register a middleware with `IStartupFilter`.</span></span> <span data-ttu-id="a8ea6-177">此 `RequestSetOptionsMiddleware` 中介軟體會從查詢字串參數設定選項值：</span><span class="sxs-lookup"><span data-stu-id="a8ea6-177">The `RequestSetOptionsMiddleware` middleware sets an options value from a query string parameter:</span></span>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsMiddleware.cs?name=snippet1)]

<span data-ttu-id="a8ea6-178">`RequestSetOptionsMiddleware` 設定在 `RequestSetOptionsStartupFilter` 類別中：</span><span class="sxs-lookup"><span data-stu-id="a8ea6-178">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

<span data-ttu-id="a8ea6-179">`IStartupFilter` 註冊在 <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> 的服務容器中。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-179">The `IStartupFilter` is registered in the service container in <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>.</span></span>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program.cs?name=snippet&highlight=19-20)]

<span data-ttu-id="a8ea6-180">提供 `option` 的查詢字串參數時，中介軟體會在 ASP.NET Core 中介軟體轉譯回應之前，先處理值指派。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-180">When a query string parameter for `option` is provided, the middleware processes the value assignment before the ASP.NET Core middleware renders the response.</span></span>

<span data-ttu-id="a8ea6-181">中介軟體執行順序是依照 `IStartupFilter` 註冊順序來設定：</span><span class="sxs-lookup"><span data-stu-id="a8ea6-181">Middleware execution order is set by the order of `IStartupFilter` registrations:</span></span>

* <span data-ttu-id="a8ea6-182">多個 `IStartupFilter` 實作可能會與相同的物件互動。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-182">Multiple `IStartupFilter` implementations may interact with the same objects.</span></span> <span data-ttu-id="a8ea6-183">如果順序很重要，請排列其 `IStartupFilter` 服務註冊順序，以符合其中介軟體執行應該依照的順序。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-183">If ordering is important, order their `IStartupFilter` service registrations to match the order that their middlewares should run.</span></span>
* <span data-ttu-id="a8ea6-184">程式庫可能使用一或多個 `IStartupFilter` 實作 (其在使用 `IStartupFilter` 註冊的其他應用程式中介軟體之前或之後執行) 來新增中介軟體。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-184">Libraries may add middleware with one or more `IStartupFilter` implementations that run before or after other app middleware registered with `IStartupFilter`.</span></span> <span data-ttu-id="a8ea6-185">在由程式庫的 `IStartupFilter` 所新增中介軟體之前叫用 `IStartupFilter` 中介軟體：</span><span class="sxs-lookup"><span data-stu-id="a8ea6-185">To invoke an `IStartupFilter` middleware before a middleware added by a library's `IStartupFilter`:</span></span>

  * <span data-ttu-id="a8ea6-186">先放置服務註冊，再將程式庫新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-186">Position the service registration before the library is added to the service container.</span></span>
  * <span data-ttu-id="a8ea6-187">若要在之後叫用，請在新增程式庫之後放置服務註冊。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-187">To invoke afterward, position the service registration after the library is added.</span></span>

## <a name="add-configuration-at-startup-from-an-external-assembly"></a><span data-ttu-id="a8ea6-188">在啟動時從外部組件新增組態</span><span class="sxs-lookup"><span data-stu-id="a8ea6-188">Add configuration at startup from an external assembly</span></span>

<span data-ttu-id="a8ea6-189"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作允許在啟動時從應用程式 `Startup` 類別外部的外部組件，針對應用程式新增增強功能。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-189">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="a8ea6-190">如需詳細資訊，請參閱 <xref:fundamentals/configuration/platform-specific-configuration> 。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-190">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a8ea6-191">其他資源</span><span class="sxs-lookup"><span data-stu-id="a8ea6-191">Additional resources</span></span>

* [<span data-ttu-id="a8ea6-192">主機</span><span class="sxs-lookup"><span data-stu-id="a8ea6-192">The host</span></span>](xref:fundamentals/index#host)
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="the-startup-class"></a><span data-ttu-id="a8ea6-193">Startup 類別</span><span class="sxs-lookup"><span data-stu-id="a8ea6-193">The Startup class</span></span>

<span data-ttu-id="a8ea6-194">ASP.NET Core 應用程式使用 `Startup` 類別，其依慣例命名為 `Startup`。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-194">ASP.NET Core apps use a `Startup` class, which is named `Startup` by convention.</span></span> <span data-ttu-id="a8ea6-195">`Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="a8ea6-195">The `Startup` class:</span></span>

* <span data-ttu-id="a8ea6-196">選擇性地包含 <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> 方法來設定應用程式的服務\*\*。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-196">Optionally includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method to configure the app's *services*.</span></span> <span data-ttu-id="a8ea6-197">服務的定義是可提供應用程式功能的可重複使用元件。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-197">A service is a reusable component that provides app functionality.</span></span> <span data-ttu-id="a8ea6-198">服務會在中*註冊*，並透過相依性 `ConfigureServices` [插入（DI）](xref:fundamentals/dependency-injection)或在整個應用程式中使用 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*> 。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-198">Services are *registered* in `ConfigureServices` and consumed across the app via [dependency injection (DI)](xref:fundamentals/dependency-injection) or <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>
* <span data-ttu-id="a8ea6-199">包含 <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> 方法來建立應用程式的要求處理管線。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-199">Includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method to create the app's request processing pipeline.</span></span>

<span data-ttu-id="a8ea6-200">應用程式啟動時，ASP.NET Core 執行階段會呼叫 `ConfigureServices` 與 `Configure`：</span><span class="sxs-lookup"><span data-stu-id="a8ea6-200">`ConfigureServices` and `Configure` are called by the ASP.NET Core runtime when the app starts:</span></span>

[!code-csharp[](startup/sample_snapshot/Startup1.cs)]

<span data-ttu-id="a8ea6-201">一旦建置應用程式[主機](xref:fundamentals/index#host)，就會指定 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-201">The `Startup` class is specified when the app's [host](xref:fundamentals/index#host) is built.</span></span> <span data-ttu-id="a8ea6-202">`Startup`通常會藉由呼叫主機產生器上的[WebHostBuilderExtensions \<TStartup> UseStartup](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*)方法來指定類別：</span><span class="sxs-lookup"><span data-stu-id="a8ea6-202">The `Startup` class is typically specified by calling the [WebHostBuilderExtensions.UseStartup\<TStartup>](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) method on the host builder:</span></span>

[!code-csharp[](startup/sample_snapshot/Program3.cs?name=snippet_Program&highlight=12)]

<span data-ttu-id="a8ea6-203">主機會提供可供 `Startup` 類別建構函式使用的服務。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-203">The host provides services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="a8ea6-204">應用程式會透過 `ConfigureServices` 新增其他服務。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-204">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="a8ea6-205">然後，主機和應用程式服務都可以在 `Configure` 和整個應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-205">Both the host and app services are then available in `Configure` and throughout the app.</span></span>

<span data-ttu-id="a8ea6-206">將[相依性插入](xref:fundamentals/dependency-injection)至 `Startup` 類別的常見用法是插入：</span><span class="sxs-lookup"><span data-stu-id="a8ea6-206">A common use of [dependency injection](xref:fundamentals/dependency-injection) into the `Startup` class is to inject:</span></span>

* <span data-ttu-id="a8ea6-207"><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> 來根據環境設定服務。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-207"><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> to configure services by environment.</span></span>
* <span data-ttu-id="a8ea6-208"><xref:Microsoft.Extensions.Configuration.IConfiguration> 來讀取組態。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-208"><xref:Microsoft.Extensions.Configuration.IConfiguration> to read configuration.</span></span>
* <span data-ttu-id="a8ea6-209"><xref:Microsoft.Extensions.Logging.ILoggerFactory> 來在 `Startup.ConfigureServices` 中建立記錄器。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-209"><xref:Microsoft.Extensions.Logging.ILoggerFactory> to create a logger in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](startup/sample_snapshot/Startup2.cs?highlight=7-8)]

<span data-ttu-id="a8ea6-210">在呼叫 `Configure` 方法之前，大部分的服務都無法使用。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-210">Most services are not available until the `Configure` method is called.</span></span>

### <a name="multiple-startup"></a><span data-ttu-id="a8ea6-211">多次啟動</span><span class="sxs-lookup"><span data-stu-id="a8ea6-211">Multiple Startup</span></span>

<span data-ttu-id="a8ea6-212">應用程式針對不同的環境定義個別的 `Startup` 類別 (例如 `StartupDevelopment`) 時，會在執行階段選取適當的 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-212">When the app defines separate `Startup` classes for different environments (for example, `StartupDevelopment`), the appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="a8ea6-213">將優先使用其名稱尾碼符合目前環境的類別。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-213">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="a8ea6-214">如果應用程式是在開發環境中執行，且同時包含 `Startup` 類別和 `StartupDevelopment` 類別，則會使用 `StartupDevelopment` 類別。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-214">If the app is run in the Development environment and includes both a `Startup` class and a `StartupDevelopment` class, the `StartupDevelopment` class is used.</span></span> <span data-ttu-id="a8ea6-215">如需詳細資訊，請參閱[使用多重環境](xref:fundamentals/environments#environment-based-startup-class-and-methods)。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-215">For more information, see [Use multiple environments](xref:fundamentals/environments#environment-based-startup-class-and-methods).</span></span>

<span data-ttu-id="a8ea6-216">如需主機的詳細資訊，請參閱[主機](xref:fundamentals/index#host)。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-216">See [The host](xref:fundamentals/index#host) for more information on the host.</span></span> <span data-ttu-id="a8ea6-217">如需在啟動期間處理錯誤的資訊，請參閱[啟動例外狀況處理](xref:fundamentals/error-handling#startup-exception-handling)。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-217">For information on handling errors during startup, see [Startup exception handling](xref:fundamentals/error-handling#startup-exception-handling).</span></span>

## <a name="the-configureservices-method"></a><span data-ttu-id="a8ea6-218">ConfigureServices 方法</span><span class="sxs-lookup"><span data-stu-id="a8ea6-218">The ConfigureServices method</span></span>

<span data-ttu-id="a8ea6-219"><xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> 方法為：</span><span class="sxs-lookup"><span data-stu-id="a8ea6-219">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method is:</span></span>

* <span data-ttu-id="a8ea6-220">選擇性。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-220">Optional.</span></span>
* <span data-ttu-id="a8ea6-221">由主機在 `Configure` 方法之前呼叫，來設定應用程式的服務。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-221">Called by the host before the `Configure` method to configure the app's services.</span></span>
* <span data-ttu-id="a8ea6-222">[組態選項](xref:fundamentals/configuration/index)依慣例設定的位置。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-222">Where [configuration options](xref:fundamentals/configuration/index) are set by convention.</span></span>

<span data-ttu-id="a8ea6-223">主機可能會在呼叫 `Startup` 方法之前，設定一些服務。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-223">The host may configure some services before `Startup` methods are called.</span></span> <span data-ttu-id="a8ea6-224">如需詳細資訊，請參閱[主機](xref:fundamentals/index#host)。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-224">For more information, see [The host](xref:fundamentals/index#host).</span></span>

<span data-ttu-id="a8ea6-225">對於需要大量安裝的功能，可從 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> 上取得 `Add{Service}` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-225">For features that require substantial setup, there are `Add{Service}` extension methods on <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>.</span></span> <span data-ttu-id="a8ea6-226">例如 **Add**DbContext、**Add**DefaultIdentity、**Add**EntityFrameworkStores 和 **Add**RazorPages：</span><span class="sxs-lookup"><span data-stu-id="a8ea6-226">For example, **Add**DbContext, **Add**DefaultIdentity, **Add**EntityFrameworkStores, and **Add**RazorPages:</span></span>

[!code-csharp[](startup/sample_snapshot/Startup3.cs)]

<span data-ttu-id="a8ea6-227">將服務新增至服務容器，使其可在應用程式和 `Configure` 方法內使用。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-227">Adding services to the service container makes them available within the app and in the `Configure` method.</span></span> <span data-ttu-id="a8ea6-228">服務可透過[相依性插入](xref:fundamentals/dependency-injection)或從 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*> 獲得解析。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-228">The services are resolved via [dependency injection](xref:fundamentals/dependency-injection) or from <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>

<span data-ttu-id="a8ea6-229">請參閱 [SetCompatibilityVersion](xref:mvc/compatibility-version) 以取得 `SetCompatibilityVersion` 的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-229">See [SetCompatibilityVersion](xref:mvc/compatibility-version) for more information on `SetCompatibilityVersion`.</span></span>

## <a name="the-configure-method"></a><span data-ttu-id="a8ea6-230">Configure 方法</span><span class="sxs-lookup"><span data-stu-id="a8ea6-230">The Configure method</span></span>

<span data-ttu-id="a8ea6-231"><xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> 方法可用來指定應用程式對 HTTP 要求的回應方式。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-231">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method is used to specify how the app responds to HTTP requests.</span></span> <span data-ttu-id="a8ea6-232">藉由將[中介軟體](xref:fundamentals/middleware/index)元件新增至 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 執行個體，即可設定要求管線。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-232">The request pipeline is configured by adding [middleware](xref:fundamentals/middleware/index) components to an <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> instance.</span></span> <span data-ttu-id="a8ea6-233">`IApplicationBuilder` 可用於 `Configure` 方法，但它未註冊在服務容器中。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-233">`IApplicationBuilder` is available to the `Configure` method, but it isn't registered in the service container.</span></span> <span data-ttu-id="a8ea6-234">裝載會建立 `IApplicationBuilder`，並將它直接傳遞到 `Configure`。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-234">Hosting creates an `IApplicationBuilder` and passes it directly to `Configure`.</span></span>

<span data-ttu-id="a8ea6-235">[ASP.NET Core 範本](/dotnet/core/tools/dotnet-new)會以下列支援設定管線：</span><span class="sxs-lookup"><span data-stu-id="a8ea6-235">The [ASP.NET Core templates](/dotnet/core/tools/dotnet-new) configure the pipeline with support for:</span></span>

* [<span data-ttu-id="a8ea6-236">開發人員例外狀況頁面</span><span class="sxs-lookup"><span data-stu-id="a8ea6-236">Developer Exception Page</span></span>](xref:fundamentals/error-handling#developer-exception-page)
* [<span data-ttu-id="a8ea6-237">例外處理常式</span><span class="sxs-lookup"><span data-stu-id="a8ea6-237">Exception handler</span></span>](xref:fundamentals/error-handling#exception-handler-page)
* [<span data-ttu-id="a8ea6-238">HTTP 嚴格的傳輸安全性 (HSTS)</span><span class="sxs-lookup"><span data-stu-id="a8ea6-238">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts)
* [<span data-ttu-id="a8ea6-239">HTTPS 重新導向</span><span class="sxs-lookup"><span data-stu-id="a8ea6-239">HTTPS redirection</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="a8ea6-240">靜態檔案</span><span class="sxs-lookup"><span data-stu-id="a8ea6-240">Static files</span></span>](xref:fundamentals/static-files)
* <span data-ttu-id="a8ea6-241">ASP.NET Core [MVC](xref:mvc/overview)和[ Razor 頁面](xref:razor-pages/index)</span><span class="sxs-lookup"><span data-stu-id="a8ea6-241">ASP.NET Core [MVC](xref:mvc/overview) and [Razor Pages](xref:razor-pages/index)</span></span>
* [<span data-ttu-id="a8ea6-242">一般資料保護規定 (GDPR)</span><span class="sxs-lookup"><span data-stu-id="a8ea6-242">General Data Protection Regulation (GDPR)</span></span>](xref:security/gdpr)

[!code-csharp[](startup/sample_snapshot/Startup4.cs)]

<span data-ttu-id="a8ea6-243">各 `Use` 擴充方法會將一或多個中介軟體元件新增到要求管線。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-243">Each `Use` extension method adds one or more middleware components to the request pipeline.</span></span> <span data-ttu-id="a8ea6-244">例如，<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> 會設定[中介軟體](xref:fundamentals/middleware/index)來提供[靜態檔案](xref:fundamentals/static-files)。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-244">For instance, <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> configures [middleware](xref:fundamentals/middleware/index) to serve [static files](xref:fundamentals/static-files).</span></span>

<span data-ttu-id="a8ea6-245">要求管線中的每個中介軟體元件負責叫用管線中的下一個元件，或於適當時，對鏈結執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-245">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the chain, if appropriate.</span></span>

<span data-ttu-id="a8ea6-246">`IHostingEnvironment` 和 `ILoggerFactory` 等其他服務，或任何在 `ConfigureServices` 中定義的項目，也可以在 `Configure` 方法簽章中指定。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-246">Additional services, such as `IHostingEnvironment` and `ILoggerFactory`, or anything defined in `ConfigureServices`, can be specified in the `Configure` method signature.</span></span> <span data-ttu-id="a8ea6-247">這些服務在可用時即會插入。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-247">These services are injected if they're available.</span></span>

<span data-ttu-id="a8ea6-248">如需 `IApplicationBuilder` 的使用方式及中介軟體處理順序的詳細資訊，請參閱 <xref:fundamentals/middleware/index>。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-248">For more information on how to use `IApplicationBuilder` and the order of middleware processing, see <xref:fundamentals/middleware/index>.</span></span>

<a name="convenience-methods"></a>

## <a name="configure-services-without-startup"></a><span data-ttu-id="a8ea6-249">在不啟動的情況下設定服務</span><span class="sxs-lookup"><span data-stu-id="a8ea6-249">Configure services without Startup</span></span>

<span data-ttu-id="a8ea6-250">若要設定服務及要求處理管線，且不使用 `Startup` 類別，請在主機建立器上呼叫 `ConfigureServices` 及 `Configure` 便利方法。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-250">To configure services and the request processing pipeline without using a `Startup` class, call `ConfigureServices` and `Configure` convenience methods on the host builder.</span></span> <span data-ttu-id="a8ea6-251">多次呼叫 `ConfigureServices` 會彼此附加。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-251">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="a8ea6-252">若多個 `Configure` 方法呼叫存在，則會使用最後的 `Configure` 呼叫。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-252">If multiple `Configure` method calls exist, the last `Configure` call is used.</span></span>

[!code-csharp[](startup/sample_snapshot/Program1.cs?highlight=16,20)]

## <a name="extend-startup-with-startup-filters"></a><span data-ttu-id="a8ea6-253">使用啟動篩選條件來擴充啟動</span><span class="sxs-lookup"><span data-stu-id="a8ea6-253">Extend Startup with startup filters</span></span>

<span data-ttu-id="a8ea6-254">使用 <xref:Microsoft.AspNetCore.Hosting.IStartupFilter> ：</span><span class="sxs-lookup"><span data-stu-id="a8ea6-254">Use <xref:Microsoft.AspNetCore.Hosting.IStartupFilter>:</span></span>

* <span data-ttu-id="a8ea6-255">在應用程式[設定](#the-configure-method)中介軟體管線的開頭或結尾設定中介軟體，而不需要明確呼叫 `Use{Middleware}` 。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-255">To configure middleware at the beginning or end of an app's [Configure](#the-configure-method) middleware pipeline without an explicit call to `Use{Middleware}`.</span></span> <span data-ttu-id="a8ea6-256">`IStartupFilter`ASP.NET Core 用來將預設值新增至管線的開頭，而不需要讓應用程式作者明確註冊預設中介軟體。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-256">`IStartupFilter` is used by ASP.NET Core to add defaults to the beginning of the pipeline without having to make the app author explicitly register the default middleware.</span></span> <span data-ttu-id="a8ea6-257">`IStartupFilter`允許代表應用程式作者進行不同的元件呼叫 `Use{Middleware}` 。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-257">`IStartupFilter` allows a different component call `Use{Middleware}` on behalf of the app author.</span></span>
* <span data-ttu-id="a8ea6-258">建立方法的管線 `Configure` 。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-258">To create a pipeline of `Configure` methods.</span></span> <span data-ttu-id="a8ea6-259">[IStartupFilter.Configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) 可以將中介軟體設為在程式庫新增中介軟體之前或之後執行。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-259">[IStartupFilter.Configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) can set a middleware to run before or after middleware added by libraries.</span></span>

<span data-ttu-id="a8ea6-260">`IStartupFilter` 會實作 <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>，其會接收並傳回 `Action<IApplicationBuilder>`。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-260">`IStartupFilter` implements <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, which receives and returns an `Action<IApplicationBuilder>`.</span></span> <span data-ttu-id="a8ea6-261"><xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 會定義類別，以設定應用程式的要求管線。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-261">An <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> defines a class to configure an app's request pipeline.</span></span> <span data-ttu-id="a8ea6-262">如需詳細資訊，請參閱[使用 IApplicationBuilder 建立中介軟體管線](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-262">For more information, see [Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).</span></span>

<span data-ttu-id="a8ea6-263">每個 `IStartupFilter` 都可在要求管線中新增一或多個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-263">Each `IStartupFilter` can add one or more middlewares in the request pipeline.</span></span> <span data-ttu-id="a8ea6-264">篩選條件將依照它們新增至服務容器的順序叫用。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-264">The filters are invoked in the order they were added to the service container.</span></span> <span data-ttu-id="a8ea6-265">篩選條件可能會在控制權傳給下一個篩選條件之前或之後新增中介軟體，因此它們會附加至應用程式管線的開頭或結尾。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-265">Filters may add middleware before or after passing control to the next filter, thus they append to the beginning or end of the app pipeline.</span></span>

<span data-ttu-id="a8ea6-266">以下範例示範如何使用 `IStartupFilter` 註冊中介軟體。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-266">The following example demonstrates how to register a middleware with `IStartupFilter`.</span></span> <span data-ttu-id="a8ea6-267">此 `RequestSetOptionsMiddleware` 中介軟體會從查詢字串參數設定選項值：</span><span class="sxs-lookup"><span data-stu-id="a8ea6-267">The `RequestSetOptionsMiddleware` middleware sets an options value from a query string parameter:</span></span>

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsMiddleware.cs?name=snippet1&highlight=21)]

<span data-ttu-id="a8ea6-268">`RequestSetOptionsMiddleware` 設定在 `RequestSetOptionsStartupFilter` 類別中：</span><span class="sxs-lookup"><span data-stu-id="a8ea6-268">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

<span data-ttu-id="a8ea6-269">`IStartupFilter` 註冊在 <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> 的服務容器中。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-269">The `IStartupFilter` is registered in the service container in <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>.</span></span>

[!code-csharp[](startup/sample_snapshot/Program2.cs?name=snippet1&highlight=4-5)]

<span data-ttu-id="a8ea6-270">提供 `option` 的查詢字串參數時，中介軟體會在 ASP.NET Core 中介軟體轉譯回應之前，先處理值指派。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-270">When a query string parameter for `option` is provided, the middleware processes the value assignment before the ASP.NET Core middleware renders the response.</span></span>

<span data-ttu-id="a8ea6-271">中介軟體執行順序是依照 `IStartupFilter` 註冊順序來設定：</span><span class="sxs-lookup"><span data-stu-id="a8ea6-271">Middleware execution order is set by the order of `IStartupFilter` registrations:</span></span>

* <span data-ttu-id="a8ea6-272">多個 `IStartupFilter` 實作可能會與相同的物件互動。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-272">Multiple `IStartupFilter` implementations may interact with the same objects.</span></span> <span data-ttu-id="a8ea6-273">如果順序很重要，請排列其 `IStartupFilter` 服務註冊順序，以符合其中介軟體執行應該依照的順序。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-273">If ordering is important, order their `IStartupFilter` service registrations to match the order that their middlewares should run.</span></span>
* <span data-ttu-id="a8ea6-274">程式庫可能使用一或多個 `IStartupFilter` 實作 (其在使用 `IStartupFilter` 註冊的其他應用程式中介軟體之前或之後執行) 來新增中介軟體。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-274">Libraries may add middleware with one or more `IStartupFilter` implementations that run before or after other app middleware registered with `IStartupFilter`.</span></span> <span data-ttu-id="a8ea6-275">在由程式庫的 `IStartupFilter` 所新增中介軟體之前叫用 `IStartupFilter` 中介軟體：</span><span class="sxs-lookup"><span data-stu-id="a8ea6-275">To invoke an `IStartupFilter` middleware before a middleware added by a library's `IStartupFilter`:</span></span>

  * <span data-ttu-id="a8ea6-276">先放置服務註冊，再將程式庫新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-276">Position the service registration before the library is added to the service container.</span></span>
  * <span data-ttu-id="a8ea6-277">若要在之後叫用，請在新增程式庫之後放置服務註冊。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-277">To invoke afterward, position the service registration after the library is added.</span></span>

## <a name="add-configuration-at-startup-from-an-external-assembly"></a><span data-ttu-id="a8ea6-278">在啟動時從外部組件新增組態</span><span class="sxs-lookup"><span data-stu-id="a8ea6-278">Add configuration at startup from an external assembly</span></span>

<span data-ttu-id="a8ea6-279"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作允許在啟動時從應用程式 `Startup` 類別外部的外部組件，針對應用程式新增增強功能。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-279">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="a8ea6-280">如需詳細資訊，請參閱 <xref:fundamentals/configuration/platform-specific-configuration> 。</span><span class="sxs-lookup"><span data-stu-id="a8ea6-280">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a8ea6-281">其他資源</span><span class="sxs-lookup"><span data-stu-id="a8ea6-281">Additional resources</span></span>

* [<span data-ttu-id="a8ea6-282">主機</span><span class="sxs-lookup"><span data-stu-id="a8ea6-282">The host</span></span>](xref:fundamentals/index#host)
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>

::: moniker-end