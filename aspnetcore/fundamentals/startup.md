---
title: "在 ASP.NET Core 應用程式啟動"
author: ardalis
description: "探索啟動類別中 ASP.NET Core 如何設定服務和應用程式的要求管線。"
ms.author: tdykstra
manager: wpickett
ms.custom: mvc
ms.date: 12/08/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/startup
ms.openlocfilehash: dd2eb3d3996bc0bf277c8d5e772c8568ef9f147e
ms.sourcegitcommit: f5a7f0198628f0d152257d90dba6c3a0747a355a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/19/2017
---
# <a name="application-startup-in-aspnet-core"></a><span data-ttu-id="d25e3-103">在 ASP.NET Core 應用程式啟動</span><span class="sxs-lookup"><span data-stu-id="d25e3-103">Application startup in ASP.NET Core</span></span>

<span data-ttu-id="d25e3-104">由[Steve Smith](https://ardalis.com)， [Tom Dykstra](https://github.com/tdykstra)，和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d25e3-104">By [Steve Smith](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d25e3-105">`Startup`類別來設定服務和應用程式的要求管線。</span><span class="sxs-lookup"><span data-stu-id="d25e3-105">The `Startup` class configures services and the app's request pipeline.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="d25e3-106">啟動類別</span><span class="sxs-lookup"><span data-stu-id="d25e3-106">The Startup class</span></span>

<span data-ttu-id="d25e3-107">ASP.NET Core 應用程式使用`Startup`類別，名為`Startup`依慣例。</span><span class="sxs-lookup"><span data-stu-id="d25e3-107">ASP.NET Core apps use a `Startup` class, which is named `Startup` by convention.</span></span> <span data-ttu-id="d25e3-108">`Startup`類別：</span><span class="sxs-lookup"><span data-stu-id="d25e3-108">The `Startup` class:</span></span>

* <span data-ttu-id="d25e3-109">可以選擇性地包含[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices)方法來設定應用程式的服務。</span><span class="sxs-lookup"><span data-stu-id="d25e3-109">Can optionally include a [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) method to configure the app's services.</span></span>
* <span data-ttu-id="d25e3-110">必須包含[設定](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure)方法來建立應用程式的要求處理管線。</span><span class="sxs-lookup"><span data-stu-id="d25e3-110">Must include a [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) method to create the app's request processing pipeline.</span></span>

<span data-ttu-id="d25e3-111">`ConfigureServices`和`Configure`應用程式啟動時，執行階段會呼叫：</span><span class="sxs-lookup"><span data-stu-id="d25e3-111">`ConfigureServices` and `Configure` are called by the runtime when the app starts:</span></span>

[!code-csharp[Main](startup/snapshot_sample/Startup1.cs)]

<span data-ttu-id="d25e3-112">指定`Startup`類別[WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt; ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)方法：</span><span class="sxs-lookup"><span data-stu-id="d25e3-112">Specify the `Startup` class with the [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) method:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

<span data-ttu-id="d25e3-113">`Startup`類別建構函式接受主機所定義的相依性。</span><span class="sxs-lookup"><span data-stu-id="d25e3-113">The `Startup` class constructor accepts dependencies defined by the host.</span></span> <span data-ttu-id="d25e3-114">常見用法[相依性插入](xref:fundamentals/dependency-injection)到`Startup`類別是要插入[IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment)由環境中設定服務：</span><span class="sxs-lookup"><span data-stu-id="d25e3-114">A common use of [dependency injection](xref:fundamentals/dependency-injection) into the `Startup` class is to inject [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) to configure services by environment:</span></span>

[!code-csharp[Main](startup/snapshot_sample/Startup2.cs)]

<span data-ttu-id="d25e3-115">插入的替代方式`IHostingStartup`是使用以慣例為基礎的方法。</span><span class="sxs-lookup"><span data-stu-id="d25e3-115">An alternative to injecting `IHostingStartup` is to use a conventions-based approach.</span></span> <span data-ttu-id="d25e3-116">應用程式可以定義個別`Startup`不同環境的類別 (例如， `StartupDevelopment`)，並在執行階段選取適當的啟動類別。</span><span class="sxs-lookup"><span data-stu-id="d25e3-116">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`), and the appropriate startup class is selected at runtime.</span></span> <span data-ttu-id="d25e3-117">類別的名稱尾碼符合目前的環境已設定優先權。</span><span class="sxs-lookup"><span data-stu-id="d25e3-117">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="d25e3-118">如果應用程式會在開發環境中執行，並同時包含`Startup`類別和`StartupDevelopment`類別`StartupDevelopment`類別使用。</span><span class="sxs-lookup"><span data-stu-id="d25e3-118">If the app is run in the Development environment and includes both a `Startup` class and a `StartupDevelopment` class, the `StartupDevelopment` class is used.</span></span> <span data-ttu-id="d25e3-119">如需詳細資訊，請參閱[使用多個環境](xref:fundamentals/environments#startup-conventions)。</span><span class="sxs-lookup"><span data-stu-id="d25e3-119">For more information see [Working with multiple environments](xref:fundamentals/environments#startup-conventions).</span></span>

<span data-ttu-id="d25e3-120">若要深入了解`WebHostBuilder`，請參閱[主控](xref:fundamentals/hosting)主題。</span><span class="sxs-lookup"><span data-stu-id="d25e3-120">To learn more about `WebHostBuilder`, see the [Hosting](xref:fundamentals/hosting) topic.</span></span> <span data-ttu-id="d25e3-121">如需在啟動期間處理的錯誤，請參閱[啟動例外狀況處理](xref:fundamentals/error-handling#startup-exception-handling)。</span><span class="sxs-lookup"><span data-stu-id="d25e3-121">For information on handling errors during startup, see [Startup exception handling](xref:fundamentals/error-handling#startup-exception-handling).</span></span>

## <a name="the-configureservices-method"></a><span data-ttu-id="d25e3-122">ConfigureServices 方法</span><span class="sxs-lookup"><span data-stu-id="d25e3-122">The ConfigureServices method</span></span>

<span data-ttu-id="d25e3-123">[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices)方法是：</span><span class="sxs-lookup"><span data-stu-id="d25e3-123">The [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) method is:</span></span>

* <span data-ttu-id="d25e3-124">選擇性。</span><span class="sxs-lookup"><span data-stu-id="d25e3-124">Optional.</span></span>
* <span data-ttu-id="d25e3-125">由 web 主機之前呼叫`Configure`方法來設定應用程式的服務。</span><span class="sxs-lookup"><span data-stu-id="d25e3-125">Called by the web host before the `Configure` method to configure the app's services.</span></span>
* <span data-ttu-id="d25e3-126">其中[組態選項](xref:fundamentals/configuration/index)慣例所設定。</span><span class="sxs-lookup"><span data-stu-id="d25e3-126">Where [configuration options](xref:fundamentals/configuration/index) are set by convention.</span></span>

<span data-ttu-id="d25e3-127">將服務加入至服務容器可讓它們在應用程式和`Configure`方法。</span><span class="sxs-lookup"><span data-stu-id="d25e3-127">Adding services to the service container makes them available within the app and in the `Configure` method.</span></span> <span data-ttu-id="d25e3-128">服務是透過解析[相依性插入](xref:fundamentals/dependency-injection)或從[IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices)。</span><span class="sxs-lookup"><span data-stu-id="d25e3-128">The services are resolved via [dependency injection](xref:fundamentals/dependency-injection) or from [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).</span></span>

<span data-ttu-id="d25e3-129">Web 主機可能會設定某些服務之前`Startup`呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="d25e3-129">The web host may configure some services before `Startup` methods are called.</span></span> <span data-ttu-id="d25e3-130">在詳細資料可用[主控](xref:fundamentals/hosting)主題。</span><span class="sxs-lookup"><span data-stu-id="d25e3-130">Details are available in the [Hosting](xref:fundamentals/hosting) topic.</span></span> 

<span data-ttu-id="d25e3-131">對於需要大量的安裝程式的功能，有`Add[Service]`的擴充方法[IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection)。</span><span class="sxs-lookup"><span data-stu-id="d25e3-131">For features that require substantial setup, there are `Add[Service]` extension methods on [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection).</span></span> <span data-ttu-id="d25e3-132">典型的 web 應用程式註冊 Entity Framework、 識別和 MVC 的服務：</span><span class="sxs-lookup"><span data-stu-id="d25e3-132">A typical web app registers services for Entity Framework, Identity, and MVC:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="services-available-in-startup"></a><span data-ttu-id="d25e3-133">用於啟動服務</span><span class="sxs-lookup"><span data-stu-id="d25e3-133">Services available in Startup</span></span>

<span data-ttu-id="d25e3-134">Web 主機提供某些服務，可用於`Startup`類別建構函式。</span><span class="sxs-lookup"><span data-stu-id="d25e3-134">The web host provides some services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="d25e3-135">應用程式將透過其他服務`ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="d25e3-135">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="d25e3-136">主機和應用程式中可用服務然後`Configure`和整個應用程式。</span><span class="sxs-lookup"><span data-stu-id="d25e3-136">Both the host and app services are then available in `Configure` and throughout the application.</span></span>

## <a name="the-configure-method"></a><span data-ttu-id="d25e3-137">Configure 方法</span><span class="sxs-lookup"><span data-stu-id="d25e3-137">The Configure method</span></span>

<span data-ttu-id="d25e3-138">[設定](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure)方法用來指定應用程式如何回應 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="d25e3-138">The [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) method is used to specify how the app responds to HTTP requests.</span></span> <span data-ttu-id="d25e3-139">透過加入設定要求管線[中介軟體](xref:fundamentals/middleware)元件[IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder)執行個體。</span><span class="sxs-lookup"><span data-stu-id="d25e3-139">The request pipeline is configured by adding [middleware](xref:fundamentals/middleware) components to an [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) instance.</span></span> <span data-ttu-id="d25e3-140">`IApplicationBuilder`若要使用`Configure`方法，但它未登錄在服務容器。</span><span class="sxs-lookup"><span data-stu-id="d25e3-140">`IApplicationBuilder` is available to the `Configure` method, but it isn't registered in the service container.</span></span> <span data-ttu-id="d25e3-141">建立裝載`IApplicationBuilder`，並將它直接`Configure`([參考來源](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192))。</span><span class="sxs-lookup"><span data-stu-id="d25e3-141">Hosting creates an `IApplicationBuilder` and passes it directly to `Configure` ([reference source](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).</span></span>

<span data-ttu-id="d25e3-142">[ASP.NET Core 範本](/dotnet/core/tools/dotnet-new)設定管線，開發人員例外狀況 頁面上，支援[BrowserLink](http://vswebessentials.com/features/browserlink)，錯誤頁面、 靜態檔案，以及 ASP.NET MVC:</span><span class="sxs-lookup"><span data-stu-id="d25e3-142">The [ASP.NET Core templates](/dotnet/core/tools/dotnet-new) configure the pipeline with support for a developer exception page, [BrowserLink](http://vswebessentials.com/features/browserlink), error pages, static files, and ASP.NET MVC:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

<span data-ttu-id="d25e3-143">每個`Use`擴充方法新增中介軟體元件，以要求管線。</span><span class="sxs-lookup"><span data-stu-id="d25e3-143">Each `Use` extension method adds a middleware component to the request pipeline.</span></span> <span data-ttu-id="d25e3-144">比方說，`UseMvc`擴充方法新增[路由的中介軟體](xref:fundamentals/routing)要求管線，並設定[MVC](xref:mvc/overview)為預設處理常式。</span><span class="sxs-lookup"><span data-stu-id="d25e3-144">For instance, the `UseMvc` extension method adds the [routing middleware](xref:fundamentals/routing) to the request pipeline and configures [MVC](xref:mvc/overview) as the default handler.</span></span>

<span data-ttu-id="d25e3-145">其他服務，例如`IHostingEnvironment`和`ILoggerFactory`，也可以指定方法簽章中。</span><span class="sxs-lookup"><span data-stu-id="d25e3-145">Additional services, such as `IHostingEnvironment` and `ILoggerFactory`, may also be specified in the method signature.</span></span> <span data-ttu-id="d25e3-146">指定時，如果已經有會插入的其他服務。</span><span class="sxs-lookup"><span data-stu-id="d25e3-146">When specified, additional services are injected if they're available.</span></span>

<span data-ttu-id="d25e3-147">如需有關如何使用`IApplicationBuilder`，請參閱[中介軟體](xref:fundamentals/middleware)。</span><span class="sxs-lookup"><span data-stu-id="d25e3-147">For more information on how to use `IApplicationBuilder`, see [Middleware](xref:fundamentals/middleware).</span></span>

## <a name="convenience-methods"></a><span data-ttu-id="d25e3-148">便利的方法</span><span class="sxs-lookup"><span data-stu-id="d25e3-148">Convenience methods</span></span>

<span data-ttu-id="d25e3-149">[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices)和[設定](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure)使用便利的方法來取代指定`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="d25e3-149">[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) and [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) convenience methods can be used instead of specifying a `Startup` class.</span></span> <span data-ttu-id="d25e3-150">多個呼叫`ConfigureServices`附加至另一個。</span><span class="sxs-lookup"><span data-stu-id="d25e3-150">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="d25e3-151">多個呼叫`Configure`使用最後一個方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="d25e3-151">Multiple calls to `Configure` use the last method call.</span></span>

[!code-csharp[Main](startup/snapshot_sample/Program.cs?highlight=16,20)]

## <a name="startup-filters"></a><span data-ttu-id="d25e3-152">啟動篩選</span><span class="sxs-lookup"><span data-stu-id="d25e3-152">Startup filters</span></span>

<span data-ttu-id="d25e3-153">使用[IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter)開頭或結尾的應用程式設定中介軟體[設定](#the-configure-method)中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="d25e3-153">Use [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) to configure middleware at the beginning or end of an app's [Configure](#the-configure-method) middleware pipeline.</span></span> <span data-ttu-id="d25e3-154">`IStartupFilter`適合用於確保中介軟體執行之前或之後新增的開頭或結尾的應用程式要求處理管線的程式庫的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="d25e3-154">`IStartupFilter` is useful to ensure that a middleware runs before or after middleware added by libraries at the start or end of the app's request processing pipeline.</span></span>

<span data-ttu-id="d25e3-155">`IStartupFilter`實作單一方法[設定](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure)，會接收並傳回`Action<IApplicationBuilder>`。</span><span class="sxs-lookup"><span data-stu-id="d25e3-155">`IStartupFilter` implements a single method, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), which receives and returns an `Action<IApplicationBuilder>`.</span></span> <span data-ttu-id="d25e3-156">[IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder)定義來設定應用程式的要求管線的類別。</span><span class="sxs-lookup"><span data-stu-id="d25e3-156">An [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) defines a class to configure an app's request pipeline.</span></span> <span data-ttu-id="d25e3-157">如需詳細資訊，請參閱[建立中介軟體管線與 IApplicationBuilder](xref:fundamentals/middleware#creating-a-middleware-pipeline-with-iapplicationbuilder)。</span><span class="sxs-lookup"><span data-stu-id="d25e3-157">For more information, see [Creating a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware#creating-a-middleware-pipeline-with-iapplicationbuilder).</span></span>

<span data-ttu-id="d25e3-158">每個`IStartupFilter`實作一或多個 middlewares 要求管線中。</span><span class="sxs-lookup"><span data-stu-id="d25e3-158">Each `IStartupFilter` implements one or more middlewares in the request pipeline.</span></span> <span data-ttu-id="d25e3-159">篩選條件會加入至服務容器的順序叫用。</span><span class="sxs-lookup"><span data-stu-id="d25e3-159">The filters are invoked in the order they were added to the service container.</span></span> <span data-ttu-id="d25e3-160">篩選可能增加中的介軟體之前或之後將控制權傳遞至下一個篩選，因此它們附加至的開頭或結尾的應用程式管線。</span><span class="sxs-lookup"><span data-stu-id="d25e3-160">Filters may add middleware before or after passing control to the next filter, thus they append to the beginning or end of the app pipeline.</span></span>

<span data-ttu-id="d25e3-161">[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/)([如何下載](xref:tutorials/index#how-to-download-a-sample)) 示範如何註冊與中的介軟體`IStartupFilter`。</span><span class="sxs-lookup"><span data-stu-id="d25e3-161">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) demonstrates how to register a middleware with `IStartupFilter`.</span></span> <span data-ttu-id="d25e3-162">範例應用程式包含中介軟體，設定選項值從查詢字串參數：</span><span class="sxs-lookup"><span data-stu-id="d25e3-162">The sample app includes a middleware that sets an options value from a query string parameter:</span></span>

[!code-csharp[Main](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

<span data-ttu-id="d25e3-163">`RequestSetOptionsMiddleware`中設定`RequestSetOptionsStartupFilter`類別：</span><span class="sxs-lookup"><span data-stu-id="d25e3-163">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[Main](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

<span data-ttu-id="d25e3-164">`IStartupFilter`註冊的服務容器中`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d25e3-164">The `IStartupFilter` is registered in the service container in `ConfigureServices`:</span></span>

[!code-csharp[Main](startup/sample/Startup.cs?name=snippet1&highlight=3)]

<span data-ttu-id="d25e3-165">當查詢字串參數為`option`提供中介軟體的 MVC 中介軟體呈現回應之前，先進行處理的值指派：</span><span class="sxs-lookup"><span data-stu-id="d25e3-165">When a query string parameter for `option` is provided, the middleware processes the value assignment before the MVC middleware renders the response:</span></span>

![顯示轉譯的索引頁的瀏覽器視窗。](startup/_static/index.png)

<span data-ttu-id="d25e3-168">中介軟體執行順序依以下順序設定`IStartupFilter`註冊：</span><span class="sxs-lookup"><span data-stu-id="d25e3-168">Middleware execution order is set by the order of `IStartupFilter` registrations:</span></span>

* <span data-ttu-id="d25e3-169">多個`IStartupFilter`實作可能會與相同的物件互動。</span><span class="sxs-lookup"><span data-stu-id="d25e3-169">Multiple `IStartupFilter` implementations may interact with the same objects.</span></span> <span data-ttu-id="d25e3-170">如果順序很重要，其`IStartupFilter`服務註冊，以符合其 middlewares 應該執行的順序。</span><span class="sxs-lookup"><span data-stu-id="d25e3-170">If ordering is important, order their `IStartupFilter` service registrations to match the order that their middlewares should run.</span></span>
* <span data-ttu-id="d25e3-171">程式庫可能新增中介軟體的一或多個`IStartupFilter`之前或之後註冊其他應用程式中介軟體執行的實作`IStartupFilter`。</span><span class="sxs-lookup"><span data-stu-id="d25e3-171">Libraries may add middleware with one or more `IStartupFilter` implementations that run before or after other app middleware registered with `IStartupFilter`.</span></span> <span data-ttu-id="d25e3-172">要叫用`IStartupFilter`之前新增的媒體櫃的中介軟體的中介軟體`IStartupFilter`，程式庫加入至服務容器之前位置的服務登錄。</span><span class="sxs-lookup"><span data-stu-id="d25e3-172">To invoke an `IStartupFilter` middleware before a middleware added by a library's `IStartupFilter`, position the service registration before the library is added to the service container.</span></span> <span data-ttu-id="d25e3-173">若要之後叫用它，加入程式庫之後的位置的服務登錄。</span><span class="sxs-lookup"><span data-stu-id="d25e3-173">To invoke it afterward, position the service registration after the library is added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d25e3-174">其他資源</span><span class="sxs-lookup"><span data-stu-id="d25e3-174">Additional resources</span></span>

* [<span data-ttu-id="d25e3-175">裝載</span><span class="sxs-lookup"><span data-stu-id="d25e3-175">Hosting</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="d25e3-176">使用多個環境</span><span class="sxs-lookup"><span data-stu-id="d25e3-176">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="d25e3-177">中介軟體</span><span class="sxs-lookup"><span data-stu-id="d25e3-177">Middleware</span></span>](xref:fundamentals/middleware)
* [<span data-ttu-id="d25e3-178">記錄</span><span class="sxs-lookup"><span data-stu-id="d25e3-178">Logging</span></span>](xref:fundamentals/logging/index)
* [<span data-ttu-id="d25e3-179">組態</span><span class="sxs-lookup"><span data-stu-id="d25e3-179">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="d25e3-180">StartupLoader 類別： FindStartupType 方法 （參考來源）</span><span class="sxs-lookup"><span data-stu-id="d25e3-180">StartupLoader class: FindStartupType method (reference source)</span></span>](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)
