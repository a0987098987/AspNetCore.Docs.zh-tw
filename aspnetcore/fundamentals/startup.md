---
title: ASP.NET Core 中的應用程式啟動
author: tdykstra
description: 了解 ASP.NET Core 中的 Startup 類別如何設定服務和應用程式的要求管線。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/17/2019
uid: fundamentals/startup
ms.openlocfilehash: 362186be6feeeefeca3c56688ee6420de5fb9659
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468620"
---
# <a name="app-startup-in-aspnet-core"></a><span data-ttu-id="ed908-103">ASP.NET Core 中的應用程式啟動</span><span class="sxs-lookup"><span data-stu-id="ed908-103">App startup in ASP.NET Core</span></span>

<span data-ttu-id="ed908-104">作者：[Tom Dykstra](https://github.com/tdykstra)、[Luke Latham](https://github.com/guardrex) 及 [Steve Smith](https://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="ed908-104">By [Tom Dykstra](https://github.com/tdykstra), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com)</span></span>

<span data-ttu-id="ed908-105">`Startup` 類別可設定服務和應用程式的要求管線。</span><span class="sxs-lookup"><span data-stu-id="ed908-105">The `Startup` class configures services and the app's request pipeline.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="ed908-106">Startup 類別</span><span class="sxs-lookup"><span data-stu-id="ed908-106">The Startup class</span></span>

<span data-ttu-id="ed908-107">ASP.NET Core 應用程式使用 `Startup` 類別，其依慣例命名為 `Startup`。</span><span class="sxs-lookup"><span data-stu-id="ed908-107">ASP.NET Core apps use a `Startup` class, which is named `Startup` by convention.</span></span> <span data-ttu-id="ed908-108">`Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="ed908-108">The `Startup` class:</span></span>

* <span data-ttu-id="ed908-109">選擇性地包含 <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> 方法來設定應用程式的服務。</span><span class="sxs-lookup"><span data-stu-id="ed908-109">Optionally includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method to configure the app's *services*.</span></span> <span data-ttu-id="ed908-110">服務的定義是可提供應用程式功能的可重複使用元件。</span><span class="sxs-lookup"><span data-stu-id="ed908-110">A service is a reusable component that provides app functionality.</span></span> <span data-ttu-id="ed908-111">服務會在 `ConfigureServices`中經過設定&mdash;也會受描述為「已註冊」&mdash;且會透過 [ 相依性插入 (DI)](xref:fundamentals/dependency-injection) 或 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*> 在應用程式間受取用。</span><span class="sxs-lookup"><span data-stu-id="ed908-111">Services are configured&mdash;also described as *registered*&mdash;in `ConfigureServices` and consumed across the app via [dependency injection (DI)](xref:fundamentals/dependency-injection) or <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>
* <span data-ttu-id="ed908-112">包含 <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> 方法來建立應用程式的要求處理管線。</span><span class="sxs-lookup"><span data-stu-id="ed908-112">Includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method to create the app's request processing pipeline.</span></span>

`ConfigureServices` <span data-ttu-id="ed908-113">和 `Configure` 會在應用程式啟動時由執行階段呼叫：</span><span class="sxs-lookup"><span data-stu-id="ed908-113">and `Configure` are called by the runtime when the app starts:</span></span>

[!code-csharp[](startup/sample_snapshot/Startup1.cs?highlight=4,10)]

<span data-ttu-id="ed908-114">一旦建置應用程式[主機](xref:fundamentals/index#host)，就會對應用程式指定 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="ed908-114">The `Startup` class is specified to the app when the app's [host](xref:fundamentals/index#host) is built.</span></span> <span data-ttu-id="ed908-115">當在 `Program` 類別中的主機建立器上呼叫 `Build` 時，就會建置該應用程式的主機。</span><span class="sxs-lookup"><span data-stu-id="ed908-115">The app's host is built when `Build` is called on the host builder in the `Program` class.</span></span> <span data-ttu-id="ed908-116">通常會在主機建立器上透過呼叫 [WebHostBuilderExtensions.UseStartup\<TStartup>](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*)方法來指定 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="ed908-116">The `Startup` class is usually specified by calling the [WebHostBuilderExtensions.UseStartup\<TStartup>](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) method on the host builder:</span></span>

[!code-csharp[](startup/sample_snapshot/Program3.cs?name=snippet_Program&highlight=10)]

<span data-ttu-id="ed908-117">主機會提供可供 `Startup` 類別建構函式使用的服務。</span><span class="sxs-lookup"><span data-stu-id="ed908-117">The host provides services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="ed908-118">應用程式會透過 `ConfigureServices` 新增其他服務。</span><span class="sxs-lookup"><span data-stu-id="ed908-118">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="ed908-119">然後，主機和應用程式服務都可以在 `Configure` 和整個應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="ed908-119">Both the host and app services are then available in `Configure` and throughout the app.</span></span>

<span data-ttu-id="ed908-120">將[相依性插入](xref:fundamentals/dependency-injection)至 `Startup` 類別的常見用法是插入：</span><span class="sxs-lookup"><span data-stu-id="ed908-120">A common use of [dependency injection](xref:fundamentals/dependency-injection) into the `Startup` class is to inject:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> <span data-ttu-id="ed908-121">來根據環境設定服務。</span><span class="sxs-lookup"><span data-stu-id="ed908-121">to configure services by environment.</span></span>
* <xref:Microsoft.Extensions.Configuration.IConfiguration> <span data-ttu-id="ed908-122">來讀取設定。</span><span class="sxs-lookup"><span data-stu-id="ed908-122">to read configuration.</span></span>
* <xref:Microsoft.Extensions.Logging.ILoggerFactory> <span data-ttu-id="ed908-123">來在 `Startup.ConfigureServices` 中建立記錄器。</span><span class="sxs-lookup"><span data-stu-id="ed908-123">to create a logger in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](startup/sample_snapshot/Startup2.cs?highlight=7-8)]

<span data-ttu-id="ed908-124">插入 `IHostingEnvironment` 的替代方式是使用基於慣例的方法。</span><span class="sxs-lookup"><span data-stu-id="ed908-124">An alternative to injecting `IHostingEnvironment` is to use a conventions-based approach.</span></span> <span data-ttu-id="ed908-125">應用程式針對不同的環境定義個別的 `Startup` 類別 (例如 `StartupDevelopment`) 時，會在執行階段選取適當的 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="ed908-125">When the app defines separate `Startup` classes for different environments (for example, `StartupDevelopment`), the appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="ed908-126">將優先使用其名稱尾碼符合目前環境的類別。</span><span class="sxs-lookup"><span data-stu-id="ed908-126">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="ed908-127">如果應用程式是在開發環境中執行，且同時包含 `Startup` 類別和 `StartupDevelopment` 類別，則會使用 `StartupDevelopment` 類別。</span><span class="sxs-lookup"><span data-stu-id="ed908-127">If the app is run in the Development environment and includes both a `Startup` class and a `StartupDevelopment` class, the `StartupDevelopment` class is used.</span></span> <span data-ttu-id="ed908-128">如需詳細資訊，請參閱[使用多重環境](xref:fundamentals/environments#environment-based-startup-class-and-methods)。</span><span class="sxs-lookup"><span data-stu-id="ed908-128">For more information, see [Use multiple environments](xref:fundamentals/environments#environment-based-startup-class-and-methods).</span></span>

<span data-ttu-id="ed908-129">若要深入了解主機，請參閱[主機](xref:fundamentals/index#host)。</span><span class="sxs-lookup"><span data-stu-id="ed908-129">To learn more about the host, see [The host](xref:fundamentals/index#host).</span></span> <span data-ttu-id="ed908-130">如需在啟動期間處理錯誤的資訊，請參閱[啟動例外狀況處理](xref:fundamentals/error-handling#startup-exception-handling)。</span><span class="sxs-lookup"><span data-stu-id="ed908-130">For information on handling errors during startup, see [Startup exception handling](xref:fundamentals/error-handling#startup-exception-handling).</span></span>

## <a name="the-configureservices-method"></a><span data-ttu-id="ed908-131">ConfigureServices 方法</span><span class="sxs-lookup"><span data-stu-id="ed908-131">The ConfigureServices method</span></span>

<span data-ttu-id="ed908-132"><xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> 方法為：</span><span class="sxs-lookup"><span data-stu-id="ed908-132">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method is:</span></span>

* <span data-ttu-id="ed908-133">選擇性。</span><span class="sxs-lookup"><span data-stu-id="ed908-133">Optional.</span></span>
* <span data-ttu-id="ed908-134">由主機在 `Configure` 方法之前呼叫，來設定應用程式的服務。</span><span class="sxs-lookup"><span data-stu-id="ed908-134">Called by the host before the `Configure` method to configure the app's services.</span></span>
* <span data-ttu-id="ed908-135">[組態選項](xref:fundamentals/configuration/index)依慣例設定的位置。</span><span class="sxs-lookup"><span data-stu-id="ed908-135">Where [configuration options](xref:fundamentals/configuration/index) are set by convention.</span></span>

<span data-ttu-id="ed908-136">典型的模式為呼叫所有 `Add{Service}` 方法，然後呼叫所有 `services.Configure{Service}` 方法。</span><span class="sxs-lookup"><span data-stu-id="ed908-136">The typical pattern is to call all the `Add{Service}` methods and then call all of the `services.Configure{Service}` methods.</span></span> <span data-ttu-id="ed908-137">例如，請參閱[設定身分識別服務](xref:security/authentication/identity#pw)。</span><span class="sxs-lookup"><span data-stu-id="ed908-137">For example, see [Configure Identity services](xref:security/authentication/identity#pw).</span></span>

<span data-ttu-id="ed908-138">主機可能會在呼叫 `Startup` 方法之前，設定一些服務。</span><span class="sxs-lookup"><span data-stu-id="ed908-138">The host may configure some services before `Startup` methods are called.</span></span> <span data-ttu-id="ed908-139">如需詳細資訊，請參閱[主機](xref:fundamentals/index#host)。</span><span class="sxs-lookup"><span data-stu-id="ed908-139">For more information, see [The host](xref:fundamentals/index#host).</span></span>

<span data-ttu-id="ed908-140">對於需要大量安裝的功能，可從 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> 上取得 `Add{Service}` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="ed908-140">For features that require substantial setup, there are `Add{Service}` extension methods on <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>.</span></span> <span data-ttu-id="ed908-141">適用於 Entity Framework、身分識別和 MVC 的典型 ASP.NET Core 應用程式註冊服務：</span><span class="sxs-lookup"><span data-stu-id="ed908-141">A typical ASP.NET Core app registers services for Entity Framework, Identity, and MVC:</span></span>

[!code-csharp[](startup/sample_snapshot/Startup3.cs)]

<span data-ttu-id="ed908-142">將服務新增至服務容器，使其可在應用程式和 `Configure` 方法內使用。</span><span class="sxs-lookup"><span data-stu-id="ed908-142">Adding services to the service container makes them available within the app and in the `Configure` method.</span></span> <span data-ttu-id="ed908-143">服務可透過[相依性插入](xref:fundamentals/dependency-injection)或從 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*> 獲得解析。</span><span class="sxs-lookup"><span data-stu-id="ed908-143">The services are resolved via [dependency injection](xref:fundamentals/dependency-injection) or from <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>

<span data-ttu-id="ed908-144">請參閱 [SetCompatibilityVersion](xref:mvc/compatibility-version) 以取得 `SetCompatibilityVersion` 的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="ed908-144">See [SetCompatibilityVersion](xref:mvc/compatibility-version) for more information on `SetCompatibilityVersion`.</span></span>

## <a name="the-configure-method"></a><span data-ttu-id="ed908-145">Configure 方法</span><span class="sxs-lookup"><span data-stu-id="ed908-145">The Configure method</span></span>

<span data-ttu-id="ed908-146"><xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> 方法可用來指定應用程式對 HTTP 要求的回應方式。</span><span class="sxs-lookup"><span data-stu-id="ed908-146">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method is used to specify how the app responds to HTTP requests.</span></span> <span data-ttu-id="ed908-147">藉由將[中介軟體](xref:fundamentals/middleware/index)元件新增至 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 執行個體，即可設定要求管線。</span><span class="sxs-lookup"><span data-stu-id="ed908-147">The request pipeline is configured by adding [middleware](xref:fundamentals/middleware/index) components to an <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> instance.</span></span> `IApplicationBuilder` <span data-ttu-id="ed908-148">可用於 `Configure` 方法，但它未註冊在服務容器中。</span><span class="sxs-lookup"><span data-stu-id="ed908-148">is available to the `Configure` method, but it isn't registered in the service container.</span></span> <span data-ttu-id="ed908-149">裝載會建立 `IApplicationBuilder`，並將它直接傳遞到 `Configure`。</span><span class="sxs-lookup"><span data-stu-id="ed908-149">Hosting creates an `IApplicationBuilder` and passes it directly to `Configure`.</span></span>

<span data-ttu-id="ed908-150">[ASP.NET Core 範本](/dotnet/core/tools/dotnet-new)會以下列支援設定管線：</span><span class="sxs-lookup"><span data-stu-id="ed908-150">The [ASP.NET Core templates](/dotnet/core/tools/dotnet-new) configure the pipeline with support for:</span></span>

* [<span data-ttu-id="ed908-151">開發人員例外狀況頁面</span><span class="sxs-lookup"><span data-stu-id="ed908-151">Developer Exception Page</span></span>](xref:fundamentals/error-handling#developer-exception-page)
* [<span data-ttu-id="ed908-152">例外處理常式</span><span class="sxs-lookup"><span data-stu-id="ed908-152">Exception handler</span></span>](xref:fundamentals/error-handling#exception-handler-page)
* [<span data-ttu-id="ed908-153">HTTP 嚴格的傳輸安全性 (HSTS)</span><span class="sxs-lookup"><span data-stu-id="ed908-153">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts)
* [<span data-ttu-id="ed908-154">HTTPS 重新導向</span><span class="sxs-lookup"><span data-stu-id="ed908-154">HTTPS redirection</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="ed908-155">靜態檔案</span><span class="sxs-lookup"><span data-stu-id="ed908-155">Static files</span></span>](xref:fundamentals/static-files)
* [<span data-ttu-id="ed908-156">一般資料保護規定 (GDPR)</span><span class="sxs-lookup"><span data-stu-id="ed908-156">General Data Protection Regulation (GDPR)</span></span>](xref:security/gdpr)
* <span data-ttu-id="ed908-157">ASP.NET Core [MVC](xref:mvc/overview) 及 [Razor Pages](xref:razor-pages/index)</span><span class="sxs-lookup"><span data-stu-id="ed908-157">ASP.NET Core [MVC](xref:mvc/overview) and [Razor Pages](xref:razor-pages/index)</span></span>

[!code-csharp[](startup/sample_snapshot/Startup4.cs)]

<span data-ttu-id="ed908-158">各 `Use` 擴充方法會將一或多個中介軟體元件新增到要求管線。</span><span class="sxs-lookup"><span data-stu-id="ed908-158">Each `Use` extension method adds one or more middleware components to the request pipeline.</span></span> <span data-ttu-id="ed908-159">比方說，`UseMvc` 擴充方法會將[路由中介軟體](xref:fundamentals/routing)新增到要求管線，並將 [MVC](xref:mvc/overview) 設定為預設處理常式。</span><span class="sxs-lookup"><span data-stu-id="ed908-159">For instance, the `UseMvc` extension method adds [Routing Middleware](xref:fundamentals/routing) to the request pipeline and configures [MVC](xref:mvc/overview) as the default handler.</span></span>

<span data-ttu-id="ed908-160">要求管線中的每個中介軟體元件負責叫用管線中的下一個元件，或於適當時，對鏈結執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="ed908-160">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the chain, if appropriate.</span></span> <span data-ttu-id="ed908-161">如果沿著中介軟體鏈結不會進行最少運算，每個中介軟體在將要求傳送至用戶端之前，會有第二次機會來處理該要求。</span><span class="sxs-lookup"><span data-stu-id="ed908-161">If short-circuiting doesn't occur along the middleware chain, each middleware has a second chance to process the request before it's sent to the client.</span></span>

<span data-ttu-id="ed908-162">`IHostingEnvironment` 和 `ILoggerFactory` 等其他服務也可以在 `Configure` 方法簽章中指定。</span><span class="sxs-lookup"><span data-stu-id="ed908-162">Additional services, such as `IHostingEnvironment` and `ILoggerFactory`, may also be specified in the `Configure` method signature.</span></span> <span data-ttu-id="ed908-163">如果已指定，其他服務在可用時即會插入。</span><span class="sxs-lookup"><span data-stu-id="ed908-163">When specified, additional services are injected if they're available.</span></span>

<span data-ttu-id="ed908-164">如需 `IApplicationBuilder` 的使用方式及中介軟體處理順序的詳細資訊，請參閱 <xref:fundamentals/middleware/index>。</span><span class="sxs-lookup"><span data-stu-id="ed908-164">For more information on how to use `IApplicationBuilder` and the order of middleware processing, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="convenience-methods"></a><span data-ttu-id="ed908-165">便利的方法</span><span class="sxs-lookup"><span data-stu-id="ed908-165">Convenience methods</span></span>

<span data-ttu-id="ed908-166">若要設定服務及要求處理管線，且不使用 `Startup` 類別，請在主機建立器上呼叫 `ConfigureServices` 及 `Configure` 便利方法。</span><span class="sxs-lookup"><span data-stu-id="ed908-166">To configure services and the request processing pipeline without using a `Startup` class, call `ConfigureServices` and `Configure` convenience methods on the host builder.</span></span> <span data-ttu-id="ed908-167">多次呼叫 `ConfigureServices` 會彼此附加。</span><span class="sxs-lookup"><span data-stu-id="ed908-167">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="ed908-168">若多個 `Configure` 方法呼叫存在，則會使用最後的 `Configure` 呼叫。</span><span class="sxs-lookup"><span data-stu-id="ed908-168">If multiple `Configure` method calls exist, the last `Configure` call is used.</span></span>

[!code-csharp[](startup/sample_snapshot/Program1.cs?highlight=16,20)]

## <a name="extend-startup-with-startup-filters"></a><span data-ttu-id="ed908-169">使用啟動篩選條件來擴充啟動</span><span class="sxs-lookup"><span data-stu-id="ed908-169">Extend Startup with startup filters</span></span>

<span data-ttu-id="ed908-170">使用 <xref:Microsoft.AspNetCore.Hosting.IStartupFilter> 在應用程式的 [Configure](#the-configure-method) 中介軟體管線的開頭或結尾設定中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ed908-170">Use <xref:Microsoft.AspNetCore.Hosting.IStartupFilter> to configure middleware at the beginning or end of an app's [Configure](#the-configure-method) middleware pipeline.</span></span> `IStartupFilter` <span data-ttu-id="ed908-171">有助於確保某個中介軟體會在應用程式要求處理管線的開頭或結尾，於程式庫所新增的中介軟體之前或之後執行。</span><span class="sxs-lookup"><span data-stu-id="ed908-171">is useful to ensure that a middleware runs before or after middleware added by libraries at the start or end of the app's request processing pipeline.</span></span>

`IStartupFilter` <span data-ttu-id="ed908-172">會實作 <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> 這個單一方法，且該方法會接收並傳回 `Action<IApplicationBuilder>`。</span><span class="sxs-lookup"><span data-stu-id="ed908-172">implements a single method, <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, which receives and returns an `Action<IApplicationBuilder>`.</span></span> <span data-ttu-id="ed908-173"><xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 會定義類別，以設定應用程式的要求管線。</span><span class="sxs-lookup"><span data-stu-id="ed908-173">An <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> defines a class to configure an app's request pipeline.</span></span> <span data-ttu-id="ed908-174">如需詳細資訊，請參閱[使用 IApplicationBuilder 建立中介軟體管線](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)。</span><span class="sxs-lookup"><span data-stu-id="ed908-174">For more information, see [Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).</span></span>

<span data-ttu-id="ed908-175">每個 `IStartupFilter` 會在要求管線中實作一或多個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ed908-175">Each `IStartupFilter` implements one or more middlewares in the request pipeline.</span></span> <span data-ttu-id="ed908-176">篩選條件將依照它們新增至服務容器的順序叫用。</span><span class="sxs-lookup"><span data-stu-id="ed908-176">The filters are invoked in the order they were added to the service container.</span></span> <span data-ttu-id="ed908-177">篩選條件可能會在控制權傳給下一個篩選條件之前或之後新增中介軟體，因此它們會附加至應用程式管線的開頭或結尾。</span><span class="sxs-lookup"><span data-stu-id="ed908-177">Filters may add middleware before or after passing control to the next filter, thus they append to the beginning or end of the app pipeline.</span></span>

<span data-ttu-id="ed908-178">以下範例示範如何使用 `IStartupFilter` 註冊中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ed908-178">The following example demonstrates how to register a middleware with `IStartupFilter`.</span></span>

<span data-ttu-id="ed908-179">此 `RequestSetOptionsMiddleware` 中介軟體會從查詢字串參數設定選項值：</span><span class="sxs-lookup"><span data-stu-id="ed908-179">The `RequestSetOptionsMiddleware` middleware sets an options value from a query string parameter:</span></span>

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsMiddleware.cs?name=snippet1&highlight=21)]

<span data-ttu-id="ed908-180">`RequestSetOptionsMiddleware` 設定在 `RequestSetOptionsStartupFilter` 類別中：</span><span class="sxs-lookup"><span data-stu-id="ed908-180">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

<span data-ttu-id="ed908-181">在 `Startup` 類別之外的引數 `Startup` 及 <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> 中的服務容器註冊 `IStartupFilter`：</span><span class="sxs-lookup"><span data-stu-id="ed908-181">The `IStartupFilter` is registered in the service container in <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> and augments `Startup` from outside of the `Startup` class:</span></span>

[!code-csharp[](startup/sample_snapshot/Program2.cs?name=snippet1&highlight=4-5)]

<span data-ttu-id="ed908-182">提供 `option` 的查詢字串參數時，中介軟體會在 MVC 中介軟體呈現回應之前，先處理值指派：</span><span class="sxs-lookup"><span data-stu-id="ed908-182">When a query string parameter for `option` is provided, the middleware processes the value assignment before the MVC middleware renders the response:</span></span>

![顯示呈現的 Index 頁面的瀏覽器視窗。](startup/_static/index.png)

<span data-ttu-id="ed908-185">中介軟體執行順序是依照 `IStartupFilter` 註冊順序來設定：</span><span class="sxs-lookup"><span data-stu-id="ed908-185">Middleware execution order is set by the order of `IStartupFilter` registrations:</span></span>

* <span data-ttu-id="ed908-186">多個 `IStartupFilter` 實作可能會與相同的物件互動。</span><span class="sxs-lookup"><span data-stu-id="ed908-186">Multiple `IStartupFilter` implementations may interact with the same objects.</span></span> <span data-ttu-id="ed908-187">如果順序很重要，請排列其 `IStartupFilter` 服務註冊順序，以符合其中介軟體執行應該依照的順序。</span><span class="sxs-lookup"><span data-stu-id="ed908-187">If ordering is important, order their `IStartupFilter` service registrations to match the order that their middlewares should run.</span></span>
* <span data-ttu-id="ed908-188">程式庫可能使用一或多個 `IStartupFilter` 實作 (其在使用 `IStartupFilter` 註冊的其他應用程式中介軟體之前或之後執行) 來新增中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ed908-188">Libraries may add middleware with one or more `IStartupFilter` implementations that run before or after other app middleware registered with `IStartupFilter`.</span></span> <span data-ttu-id="ed908-189">若要在程式庫的 `IStartupFilter` 所新增的中介軟體之前叫用 `IStartupFilter` 中介軟體，請在程式庫新增至服務容器之前放置服務註冊。</span><span class="sxs-lookup"><span data-stu-id="ed908-189">To invoke an `IStartupFilter` middleware before a middleware added by a library's `IStartupFilter`, position the service registration before the library is added to the service container.</span></span> <span data-ttu-id="ed908-190">若要在之後叫用它，請在新增程式庫之後放置服務註冊。</span><span class="sxs-lookup"><span data-stu-id="ed908-190">To invoke it afterward, position the service registration after the library is added.</span></span>

## <a name="add-configuration-at-startup-from-an-external-assembly"></a><span data-ttu-id="ed908-191">在啟動時從外部組件新增組態</span><span class="sxs-lookup"><span data-stu-id="ed908-191">Add configuration at startup from an external assembly</span></span>

<span data-ttu-id="ed908-192"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作允許在啟動時從應用程式 `Startup` 類別外部的外部組件，針對應用程式新增增強功能。</span><span class="sxs-lookup"><span data-stu-id="ed908-192">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="ed908-193">如需詳細資訊，請參閱<xref:fundamentals/configuration/platform-specific-configuration>。</span><span class="sxs-lookup"><span data-stu-id="ed908-193">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ed908-194">其他資源</span><span class="sxs-lookup"><span data-stu-id="ed908-194">Additional resources</span></span>

* [<span data-ttu-id="ed908-195">主機</span><span class="sxs-lookup"><span data-stu-id="ed908-195">The host</span></span>](xref:fundamentals/index#host)
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
