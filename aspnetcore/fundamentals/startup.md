---
title: "在 ASP.NET Core 應用程式啟動"
author: ardalis
description: "說明在 ASP.NET Core 啟動類別。"
keywords: "ASP.NET Core，啟動時，設定方法、 ConfigureServices 方法"
ms.author: tdykstra
manager: wpickett
ms.date: 02/29/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/startup
ms.openlocfilehash: 009df1416c822018d6e88912cc77e525c7349c34
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2017
---
# <a name="application-startup-in-aspnet-core"></a><span data-ttu-id="bf13f-104">在 ASP.NET Core 應用程式啟動</span><span class="sxs-lookup"><span data-stu-id="bf13f-104">Application Startup in ASP.NET Core</span></span>

<span data-ttu-id="bf13f-105">由[Steve Smith](https://ardalis.com/)和[Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="bf13f-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="bf13f-106">`Startup`類別來設定服務和應用程式的要求管線。</span><span class="sxs-lookup"><span data-stu-id="bf13f-106">The `Startup` class configures services and the application's request pipeline.</span></span> 

## <a name="the-startup-class"></a><span data-ttu-id="bf13f-107">啟動類別</span><span class="sxs-lookup"><span data-stu-id="bf13f-107">The Startup class</span></span>

<span data-ttu-id="bf13f-108">ASP.NET Core 應用程式需要`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="bf13f-108">ASP.NET Core apps require a `Startup` class.</span></span> <span data-ttu-id="bf13f-109">依照慣例，`Startup`類別的名稱為 「 啟動 」。</span><span class="sxs-lookup"><span data-stu-id="bf13f-109">By convention, the `Startup` class is named "Startup".</span></span> <span data-ttu-id="bf13f-110">指定啟動類別名稱在`Main`程式的[WebHostBuilderExtensions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions) [ `UseStartup<TStartup>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)方法。</span><span class="sxs-lookup"><span data-stu-id="bf13f-110">You specify the startup class name in the `Main` program's [WebHostBuilderExtensions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions) [`UseStartup<TStartup>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) method.</span></span> <span data-ttu-id="bf13f-111">請參閱[主控](xref:fundamentals/hosting)若要深入了解`WebHostBuilder`，執行之前`Startup`。</span><span class="sxs-lookup"><span data-stu-id="bf13f-111">See [Hosting](xref:fundamentals/hosting) to learn more about `WebHostBuilder`, which runs before `Startup`.</span></span>

<span data-ttu-id="bf13f-112">您可以定義個別`Startup`不同環境中，並在執行階段會選取一個適當的類別。</span><span class="sxs-lookup"><span data-stu-id="bf13f-112">You can define separate `Startup` classes for different environments, and the appropriate one will be selected at runtime.</span></span> <span data-ttu-id="bf13f-113">如果您指定`startupAssembly`中[WebHost 設定](https://docs.microsoft.com/aspnet/core/fundamentals/hosting?tabs=aspnetcore2x#configuring-a-host)或裝載的選項將會載入該啟動組件，並搜尋`Startup`或`Startup[Environment]`型別。</span><span class="sxs-lookup"><span data-stu-id="bf13f-113">If you specify `startupAssembly` in the [WebHost configuration](https://docs.microsoft.com/aspnet/core/fundamentals/hosting?tabs=aspnetcore2x#configuring-a-host) or options, hosting will load that startup assembly and search for a `Startup` or `Startup[Environment]` type.</span></span> <span data-ttu-id="bf13f-114">類別目前的環境來排列優先順序的名稱後置詞相符項目，因此如果在執行應用程式*開發*環境中，並同時包含`Startup`和`StartupDevelopment`類別`StartupDevelopment`類別將為使用。</span><span class="sxs-lookup"><span data-stu-id="bf13f-114">The class whose name suffix matches the current environment will be prioritized, so if the app is run in the *Development* environment, and includes both a `Startup` and a `StartupDevelopment` class, the `StartupDevelopment` class will be used.</span></span> <span data-ttu-id="bf13f-115">請參閱[FindStartupType](https://github.com/aspnet/Hosting/blob/rel/1.1.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs)中`StartupLoader`和[使用多個環境](environments.md#startup-conventions)。</span><span class="sxs-lookup"><span data-stu-id="bf13f-115">See [FindStartupType](https://github.com/aspnet/Hosting/blob/rel/1.1.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs) in `StartupLoader` and [Working with multiple environments](environments.md#startup-conventions).</span></span>

<span data-ttu-id="bf13f-116">或者，您可以定義固定`Startup`不論環境將使用藉由呼叫類別`UseStartup<TStartup>`。</span><span class="sxs-lookup"><span data-stu-id="bf13f-116">Alternatively, you can define a fixed `Startup` class that will be used regardless of the environment by calling `UseStartup<TStartup>`.</span></span> <span data-ttu-id="bf13f-117">這是建議的處理方式。</span><span class="sxs-lookup"><span data-stu-id="bf13f-117">This is the recommended approach.</span></span>

<span data-ttu-id="bf13f-118">`Startup`類別建構函式可以接受經由所提供的相依性[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="bf13f-118">The `Startup` class constructor can accept dependencies that are provided through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="bf13f-119">常見的方法是使用`IHostingEnvironment`設定[組態](xref:fundamentals/configuration)來源。</span><span class="sxs-lookup"><span data-stu-id="bf13f-119">A common approach is to use `IHostingEnvironment` to set up [configuration](xref:fundamentals/configuration) sources.</span></span>

<span data-ttu-id="bf13f-120">`Startup`類別必須包含`Configure`方法，而且可以選擇包含`ConfigureServices`方法，這兩種應用程式啟動時呼叫。</span><span class="sxs-lookup"><span data-stu-id="bf13f-120">The `Startup` class must include a `Configure` method and can optionally include a `ConfigureServices` method, both of which are called when the application starts.</span></span> <span data-ttu-id="bf13f-121">類別也可以包含[這些方法的環境特定版本](xref:fundamentals/environments#startup-conventions)。</span><span class="sxs-lookup"><span data-stu-id="bf13f-121">The class can also include [environment-specific versions of these methods](xref:fundamentals/environments#startup-conventions).</span></span> <span data-ttu-id="bf13f-122">`ConfigureServices`如果有的話，會呼叫之前`Configure`。</span><span class="sxs-lookup"><span data-stu-id="bf13f-122">`ConfigureServices`, if present, is called before `Configure`.</span></span>

<span data-ttu-id="bf13f-123">深入了解[例外狀況處理應用程式啟動期間](xref:fundamentals/error-handling#startup-exception-handling)。</span><span class="sxs-lookup"><span data-stu-id="bf13f-123">Learn about [handling exceptions during application startup](xref:fundamentals/error-handling#startup-exception-handling).</span></span>

## <a name="the-configureservices-method"></a><span data-ttu-id="bf13f-124">ConfigureServices 方法</span><span class="sxs-lookup"><span data-stu-id="bf13f-124">The ConfigureServices method</span></span>

<span data-ttu-id="bf13f-125">[ConfigureServices](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.startupbase#Microsoft_AspNetCore_Hosting_StartupBase_ConfigureServices_Microsoft_Extensions_DependencyInjection_IServiceCollection_)方法是選擇項，但如果使用，它就稱為之前`Configure`web 主機的方法。</span><span class="sxs-lookup"><span data-stu-id="bf13f-125">The [ConfigureServices](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.startupbase#Microsoft_AspNetCore_Hosting_StartupBase_ConfigureServices_Microsoft_Extensions_DependencyInjection_IServiceCollection_) method is optional; but if used, it's called before the `Configure` method by the web host.</span></span> <span data-ttu-id="bf13f-126">Web 主機可能會設定某些服務之前``Startup``呼叫的方法 (請參閱[裝載](xref:fundamentals/hosting))。</span><span class="sxs-lookup"><span data-stu-id="bf13f-126">The web host may configure some services before ``Startup`` methods are called (see [hosting](xref:fundamentals/hosting)).</span></span> <span data-ttu-id="bf13f-127">依照慣例，[組態選項](xref:fundamentals/configuration)中這個方法所設定。</span><span class="sxs-lookup"><span data-stu-id="bf13f-127">By convention, [Configuration options](xref:fundamentals/configuration) are set in this method.</span></span>

<span data-ttu-id="bf13f-128">對於需要大量的安裝程式的功能有`Add[Service]`的擴充方法[IServiceCollection](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.iservicecollection)。</span><span class="sxs-lookup"><span data-stu-id="bf13f-128">For features that require substantial setup there are `Add[Service]` extension methods on [IServiceCollection](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.iservicecollection).</span></span> <span data-ttu-id="bf13f-129">這個範例來自預設的網站範本會設定應用程式使用 Entity Framework、 識別和 MVC 的服務：</span><span class="sxs-lookup"><span data-stu-id="bf13f-129">This example from the default web site template configures the app to use services for Entity Framework, Identity, and MVC:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

<span data-ttu-id="bf13f-130">將服務加入至服務容器可透過應用程式中使用[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="bf13f-130">Adding services to the services container makes them available within your application via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="services-available-in-startup"></a><span data-ttu-id="bf13f-131">用於啟動服務</span><span class="sxs-lookup"><span data-stu-id="bf13f-131">Services Available in Startup</span></span>

<span data-ttu-id="bf13f-132">ASP.NET Core 相依性插入的應用程式啟動時提供服務。</span><span class="sxs-lookup"><span data-stu-id="bf13f-132">ASP.NET Core dependency injection provides services during an application's startup.</span></span> <span data-ttu-id="bf13f-133">您可以要求這些服務包括做為參數的適當的介面上您`Startup`類別的建構函式或其`Configure`方法。</span><span class="sxs-lookup"><span data-stu-id="bf13f-133">You can request these services by including the appropriate interface as a parameter on your `Startup` class's constructor or its `Configure` method.</span></span> <span data-ttu-id="bf13f-134">`ConfigureServices`方法只接受`IServiceCollection`參數 （但任何的已註冊的服務可以擷取從這個集合中，因此不需要額外的參數）。</span><span class="sxs-lookup"><span data-stu-id="bf13f-134">The `ConfigureServices` method only takes an `IServiceCollection` parameter (but any registered service can be retrieved from this collection, so additional parameters are not necessary).</span></span>

<span data-ttu-id="bf13f-135">下面是一些通常會要求的服務`Startup`方法：</span><span class="sxs-lookup"><span data-stu-id="bf13f-135">Below are some of the services typically requested by `Startup` methods:</span></span>

* <span data-ttu-id="bf13f-136">建構函式： `IHostingEnvironment`，`ILogger<Startup>`</span><span class="sxs-lookup"><span data-stu-id="bf13f-136">In the constructor:  `IHostingEnvironment`, `ILogger<Startup>`</span></span>
* <span data-ttu-id="bf13f-137">在`ConfigureServices`:`IServiceCollection`</span><span class="sxs-lookup"><span data-stu-id="bf13f-137">In `ConfigureServices`:  `IServiceCollection`</span></span>
* <span data-ttu-id="bf13f-138">在`Configure`: `IApplicationBuilder`， `IHostingEnvironment`，`ILoggerFactory`</span><span class="sxs-lookup"><span data-stu-id="bf13f-138">In `Configure`:  `IApplicationBuilder`, `IHostingEnvironment`, `ILoggerFactory`</span></span>

<span data-ttu-id="bf13f-139">新增的任何服務``WebHostBuilder````ConfigureServices``方法可要求的``Startup``類別建構函式或其``Configure``方法。</span><span class="sxs-lookup"><span data-stu-id="bf13f-139">Any services added by the ``WebHostBuilder`` ``ConfigureServices`` method may be requested by the ``Startup`` class constructor or its ``Configure`` method.</span></span> <span data-ttu-id="bf13f-140">使用`WebHostBuilder`提供任何服務需要期間`Startup`方法。</span><span class="sxs-lookup"><span data-stu-id="bf13f-140">Use `WebHostBuilder` to provide any services you need during `Startup` methods.</span></span>

## <a name="the-configure-method"></a><span data-ttu-id="bf13f-141">Configure 方法</span><span class="sxs-lookup"><span data-stu-id="bf13f-141">The Configure method</span></span>

<span data-ttu-id="bf13f-142">`Configure`方法用來指定 ASP.NET 應用程式將如何回應 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="bf13f-142">The `Configure` method is used to specify how the ASP.NET application will respond to HTTP requests.</span></span> <span data-ttu-id="bf13f-143">透過加入設定要求管線[中介軟體](middleware.md)元件`IApplicationBuilder`相依性插入所提供的執行個體。</span><span class="sxs-lookup"><span data-stu-id="bf13f-143">The request pipeline is configured by adding [middleware](middleware.md) components to an `IApplicationBuilder` instance that is provided by dependency injection.</span></span>

<span data-ttu-id="bf13f-144">在下列範例從預設的網站範本中，數個擴充方法可用來設定管線，可以支援[BrowserLink](http://vswebessentials.com/features/browserlink)，錯誤頁面、 靜態檔案，ASP.NET MVC 和身分識別。</span><span class="sxs-lookup"><span data-stu-id="bf13f-144">In the following example from the default web site template, several extension methods are used to configure the pipeline with support for [BrowserLink](http://vswebessentials.com/features/browserlink), error pages, static files, ASP.NET MVC, and Identity.</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

<span data-ttu-id="bf13f-145">每個`Use`擴充方法新增[中介軟體](xref:fundamentals/middleware)要求管線元件。</span><span class="sxs-lookup"><span data-stu-id="bf13f-145">Each `Use` extension method adds a [middleware](xref:fundamentals/middleware) component to the request pipeline.</span></span> <span data-ttu-id="bf13f-146">比方說，`UseMvc`擴充方法新增[路由](routing.md)要求管線中的介軟體，並設定[MVC](xref:mvc/overview)為預設處理常式。</span><span class="sxs-lookup"><span data-stu-id="bf13f-146">For instance, the `UseMvc` extension method adds the [routing](routing.md) middleware to the request pipeline and configures [MVC](xref:mvc/overview) as the default handler.</span></span>

<span data-ttu-id="bf13f-147">如需有關如何使用`IApplicationBuilder`，請參閱[中介軟體](xref:fundamentals/middleware)。</span><span class="sxs-lookup"><span data-stu-id="bf13f-147">For more information about how to use `IApplicationBuilder`, see [Middleware](xref:fundamentals/middleware).</span></span>

<span data-ttu-id="bf13f-148">其他服務，例如`IHostingEnvironment`和`ILoggerFactory`也可以在方法簽章中指定這些服務將無法在此情況下[插入](dependency-injection.md)在有提供。</span><span class="sxs-lookup"><span data-stu-id="bf13f-148">Additional services, like `IHostingEnvironment` and `ILoggerFactory` may also be specified in the method signature, in which case these services will be [injected](dependency-injection.md) if they are available.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="bf13f-149">其他資源</span><span class="sxs-lookup"><span data-stu-id="bf13f-149">Additional Resources</span></span>

* [<span data-ttu-id="bf13f-150">使用多個環境</span><span class="sxs-lookup"><span data-stu-id="bf13f-150">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="bf13f-151">中介軟體</span><span class="sxs-lookup"><span data-stu-id="bf13f-151">Middleware</span></span>](xref:fundamentals/middleware)
* [<span data-ttu-id="bf13f-152">記錄</span><span class="sxs-lookup"><span data-stu-id="bf13f-152">Logging</span></span>](xref:fundamentals/logging)
* [<span data-ttu-id="bf13f-153">組態</span><span class="sxs-lookup"><span data-stu-id="bf13f-153">Configuration</span></span>](xref:fundamentals/configuration)
