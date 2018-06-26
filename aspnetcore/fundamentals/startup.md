---
title: ASP.NET Core 中的應用程式啟動
author: ardalis
description: 探索 Startup 類別如何在 ASP.NET Core 中設定服務和應用程式的要求管線。
ms.author: tdykstra
ms.custom: mvc
ms.date: 4/13/2018
uid: fundamentals/startup
ms.openlocfilehash: f0b907e4322809dfe2bcd287bb064f35f5ebe150
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/25/2018
ms.locfileid: "36314116"
---
# <a name="application-startup-in-aspnet-core"></a><span data-ttu-id="31c52-103">ASP.NET Core 中的應用程式啟動</span><span class="sxs-lookup"><span data-stu-id="31c52-103">Application startup in ASP.NET Core</span></span>

<span data-ttu-id="31c52-104">作者：[Steve Smith](https://ardalis.com)、[Tom Dykstra](https://github.com/tdykstra) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="31c52-104">By [Steve Smith](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="31c52-105">`Startup` 類別可設定服務和應用程式的要求管線。</span><span class="sxs-lookup"><span data-stu-id="31c52-105">The `Startup` class configures services and the app's request pipeline.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="31c52-106">Startup 類別</span><span class="sxs-lookup"><span data-stu-id="31c52-106">The Startup class</span></span>

<span data-ttu-id="31c52-107">ASP.NET Core 應用程式使用 `Startup` 類別，其依慣例命名為 `Startup`。</span><span class="sxs-lookup"><span data-stu-id="31c52-107">ASP.NET Core apps use a `Startup` class, which is named `Startup` by convention.</span></span> <span data-ttu-id="31c52-108">`Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="31c52-108">The `Startup` class:</span></span>

* <span data-ttu-id="31c52-109">可以選擇性地包含 [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) 方法來設定應用程式的服務。</span><span class="sxs-lookup"><span data-stu-id="31c52-109">Can optionally include a [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) method to configure the app's services.</span></span>
* <span data-ttu-id="31c52-110">必須包含 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) 方法來建立應用程式的要求處理管線。</span><span class="sxs-lookup"><span data-stu-id="31c52-110">Must include a [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) method to create the app's request processing pipeline.</span></span>

<span data-ttu-id="31c52-111">應用程式啟動時，執行階段就會呼叫 `ConfigureServices` 和 `Configure`：</span><span class="sxs-lookup"><span data-stu-id="31c52-111">`ConfigureServices` and `Configure` are called by the runtime when the app starts:</span></span>

[!code-csharp[](startup/snapshot_sample/Startup1.cs)]

<span data-ttu-id="31c52-112">使用 [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) 方法來指定 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="31c52-112">Specify the `Startup` class with the [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) method:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

<span data-ttu-id="31c52-113">`Startup` 類別建構函式接受主機所定義的相依性。</span><span class="sxs-lookup"><span data-stu-id="31c52-113">The `Startup` class constructor accepts dependencies defined by the host.</span></span> <span data-ttu-id="31c52-114">將[相依性插入](xref:fundamentals/dependency-injection)至 `Startup` 類別的常見用法是插入：</span><span class="sxs-lookup"><span data-stu-id="31c52-114">A common use of [dependency injection](xref:fundamentals/dependency-injection) into the `Startup` class is to inject:</span></span>

* <span data-ttu-id="31c52-115">[IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment)，用來依環境設定服務。</span><span class="sxs-lookup"><span data-stu-id="31c52-115">[IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) to configure services by environment.</span></span>
* <span data-ttu-id="31c52-116">[IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration)，用來在啟動期間設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="31c52-116">[IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) to configure the app during startup.</span></span>

[!code-csharp[](startup/snapshot_sample/Startup2.cs)]

<span data-ttu-id="31c52-117">插入 `IHostingEnvironment` 的替代方式是使用基於慣例的方法。</span><span class="sxs-lookup"><span data-stu-id="31c52-117">An alternative to injecting `IHostingEnvironment` is to use a conventions-based approach.</span></span> <span data-ttu-id="31c52-118">應用程式可以針對不同的環境定義個別的 `Startup` 類別 (例如，`StartupDevelopment`)，並在執行階段選取適當的啟動類別。</span><span class="sxs-lookup"><span data-stu-id="31c52-118">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`), and the appropriate startup class is selected at runtime.</span></span> <span data-ttu-id="31c52-119">將優先使用其名稱尾碼符合目前環境的類別。</span><span class="sxs-lookup"><span data-stu-id="31c52-119">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="31c52-120">如果應用程式是在開發環境中執行，且同時包含 `Startup` 類別和 `StartupDevelopment` 類別，則會使用 `StartupDevelopment` 類別。</span><span class="sxs-lookup"><span data-stu-id="31c52-120">If the app is run in the Development environment and includes both a `Startup` class and a `StartupDevelopment` class, the `StartupDevelopment` class is used.</span></span> <span data-ttu-id="31c52-121">如需詳細資訊，請參閱[使用多重環境](xref:fundamentals/environments#environment-based-startup-class-and-methods)。</span><span class="sxs-lookup"><span data-stu-id="31c52-121">For more information, see [Use multiple environments](xref:fundamentals/environments#environment-based-startup-class-and-methods).</span></span>

<span data-ttu-id="31c52-122">若要深入了解 `WebHostBuilder`，請參閱[裝載](xref:fundamentals/host/index)主題。</span><span class="sxs-lookup"><span data-stu-id="31c52-122">To learn more about `WebHostBuilder`, see the [Hosting](xref:fundamentals/host/index) topic.</span></span> <span data-ttu-id="31c52-123">如需在啟動期間處理錯誤的資訊，請參閱[啟動例外狀況處理](xref:fundamentals/error-handling#startup-exception-handling)。</span><span class="sxs-lookup"><span data-stu-id="31c52-123">For information on handling errors during startup, see [Startup exception handling](xref:fundamentals/error-handling#startup-exception-handling).</span></span>

## <a name="the-configureservices-method"></a><span data-ttu-id="31c52-124">ConfigureServices 方法</span><span class="sxs-lookup"><span data-stu-id="31c52-124">The ConfigureServices method</span></span>

<span data-ttu-id="31c52-125">[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) 方法是：</span><span class="sxs-lookup"><span data-stu-id="31c52-125">The [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) method is:</span></span>

* <span data-ttu-id="31c52-126">Optional</span><span class="sxs-lookup"><span data-stu-id="31c52-126">Optional</span></span>
* <span data-ttu-id="31c52-127">由 Web 主機在 `Configure` 方法之前呼叫，用來設定應用程式的服務。</span><span class="sxs-lookup"><span data-stu-id="31c52-127">Called by the web host before the `Configure` method to configure the app's services.</span></span>
* <span data-ttu-id="31c52-128">[組態選項](xref:fundamentals/configuration/index)依慣例設定的位置。</span><span class="sxs-lookup"><span data-stu-id="31c52-128">Where [configuration options](xref:fundamentals/configuration/index) are set by convention.</span></span>

<span data-ttu-id="31c52-129">將服務新增至服務容器，使其可在應用程式和 `Configure` 方法內使用。</span><span class="sxs-lookup"><span data-stu-id="31c52-129">Adding services to the service container makes them available within the app and in the `Configure` method.</span></span> <span data-ttu-id="31c52-130">這些服務會透過[相依性插入](xref:fundamentals/dependency-injection)或從 [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) 加以解析。</span><span class="sxs-lookup"><span data-stu-id="31c52-130">The services are resolved via [dependency injection](xref:fundamentals/dependency-injection) or from [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).</span></span>

<span data-ttu-id="31c52-131">Web 主機可能會在呼叫 `Startup` 方法之前設定一些服務。</span><span class="sxs-lookup"><span data-stu-id="31c52-131">The web host may configure some services before `Startup` methods are called.</span></span> <span data-ttu-id="31c52-132">詳細資料可於[在 ASP.NET Core 中代管](xref:fundamentals/host/index)主題中取得。</span><span class="sxs-lookup"><span data-stu-id="31c52-132">Details are available in the [Host in ASP.NET Core](xref:fundamentals/host/index) topic.</span></span>

<span data-ttu-id="31c52-133">對於需要大量安裝的功能，[IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection) 上有 `Add[Service]` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="31c52-133">For features that require substantial setup, there are `Add[Service]` extension methods on [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection).</span></span> <span data-ttu-id="31c52-134">適用於 Entity Framework、身分識別和 MVC 的典型 Web 應用程式註冊服務：</span><span class="sxs-lookup"><span data-stu-id="31c52-134">A typical web app registers services for Entity Framework, Identity, and MVC:</span></span>

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

::: moniker range=">= aspnetcore-2.1"

<a name="setcompatibilityversion"></a>

### <a name="setcompatibilityversion-for-aspnet-core-mvc"></a><span data-ttu-id="31c52-135">適用於 ASP.NET Core MVC 的 SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="31c52-135">SetCompatibilityVersion for ASP.NET Core MVC</span></span> 

<span data-ttu-id="31c52-136">`SetCompatibilityVersion` 方法可讓應用程式選擇加入或退出 ASP.NET MVC Core 2.1 以上版本所引入的可能重大行為變更。</span><span class="sxs-lookup"><span data-stu-id="31c52-136">The `SetCompatibilityVersion` method allows an app to opt-in or opt-out of potentially breaking behavior changes introduced in ASP.NET MVC Core 2.1+.</span></span> <span data-ttu-id="31c52-137">這些可能的重大行為變更通常在於 MVC 子系統的運作方式，以及執行階段呼叫**您的程式碼**的方式。</span><span class="sxs-lookup"><span data-stu-id="31c52-137">These potentially breaking behavior changes are generally in how the MVC subsystem behaves and how **your code** is called by the runtime.</span></span> <span data-ttu-id="31c52-138">透過選擇加入，您可以取得最新的行為和 ASP.NET Core 的長期行為。</span><span class="sxs-lookup"><span data-stu-id="31c52-138">By opting in, you get the latest behavior, and the long-term behavior of ASP.NET Core.</span></span>

<span data-ttu-id="31c52-139">下列程式碼會將相容性模式設定為 ASP.NET Core 2.1：</span><span class="sxs-lookup"><span data-stu-id="31c52-139">The following code sets the compatibility mode to ASP.NET Core 2.1:</span></span>

[!code-csharp[Main](startup/sampleCompatibility/Startup.cs?name=snippet1)]

<span data-ttu-id="31c52-140">建議您使用最新版本 (`CompatibilityVersion.Version_2_1`) 測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="31c52-140">We recommend you test your application using the latest version (`CompatibilityVersion.Version_2_1`).</span></span> <span data-ttu-id="31c52-141">預計大部分的應用程式都不會使用最新版本進行重大行為變更。</span><span class="sxs-lookup"><span data-stu-id="31c52-141">We anticipate that most applications will not have breaking behavior changes using the latest version.</span></span> 

<span data-ttu-id="31c52-142">呼叫 `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` 的應用程式會受到保護，防止執行 ASP.NET Core 2.1 MVC 和更新版本的 2.x 版所引入的可能重大行為變更。</span><span class="sxs-lookup"><span data-stu-id="31c52-142">Applications that call `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` are protected from potentially breaking behavior changes introduced in the ASP.NET Core 2.1 MVC and later 2.x versions.</span></span> <span data-ttu-id="31c52-143">這項保護：</span><span class="sxs-lookup"><span data-stu-id="31c52-143">This protection:</span></span>

* <span data-ttu-id="31c52-144">不適用於所有 2.1 和更新版本的變更，它的目標是 MVC 子系統中的可能重大 ASP.NET Core 執行階段行為變更。</span><span class="sxs-lookup"><span data-stu-id="31c52-144">Does not apply to all 2.1 and later changes, it's targeted to potentially breaking ASP.NET Core runtime behavior changes in the MVC subsystem.</span></span>
* <span data-ttu-id="31c52-145">不會擴充到下一個主要版本。</span><span class="sxs-lookup"><span data-stu-id="31c52-145">Does not extend to the next major version.</span></span>

<span data-ttu-id="31c52-146">適用於 ASP.NET Core 2.1 和更新版本的 2.x 應用程式且**不會**呼叫 `SetCompatibilityVersion` 的預設相容性為 2.0 相容性。</span><span class="sxs-lookup"><span data-stu-id="31c52-146">The default compatibility for ASP.NET Core 2.1 and later 2.x applications that do **not** call `SetCompatibilityVersion` is 2.0 compatibility.</span></span> <span data-ttu-id="31c52-147">也就是說，不呼叫 `SetCompatibilityVersion` 等同於呼叫 `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`。</span><span class="sxs-lookup"><span data-stu-id="31c52-147">That is, not calling `SetCompatibilityVersion` is the same as calling `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.</span></span>

<span data-ttu-id="31c52-148">下列程式碼會將相容性模式設定為 ASP.NET Core 2.1，但下列行為除外：</span><span class="sxs-lookup"><span data-stu-id="31c52-148">The following code sets the compatibility mode to ASP.NET Core 2.1, except for the following behaviors:</span></span>

* [<span data-ttu-id="31c52-149">AllowCombiningAuthorizeFilters</span><span class="sxs-lookup"><span data-stu-id="31c52-149">AllowCombiningAuthorizeFilters</span></span>](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)
* [<span data-ttu-id="31c52-150">InputFormatterExceptionPolicy</span><span class="sxs-lookup"><span data-stu-id="31c52-150">InputFormatterExceptionPolicy</span></span>](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)

[!code-csharp[Main](startup/sampleCompatibility/Startup2.cs?name=snippet1)]

<span data-ttu-id="31c52-151">對於出現重大行為變更的應用程式，使用適當的相容性參數：</span><span class="sxs-lookup"><span data-stu-id="31c52-151">For apps that encounter breaking behavior changes, using the appropriate compatibility switches:</span></span>

* <span data-ttu-id="31c52-152">可讓您使用最新版本，並選擇退出特定的重大行為變更。</span><span class="sxs-lookup"><span data-stu-id="31c52-152">Allows you to use the latest release and opt out of specific breaking behavior changes.</span></span>
* <span data-ttu-id="31c52-153">讓您有時間來更新您的應用程式，使其適用於最新的變更。</span><span class="sxs-lookup"><span data-stu-id="31c52-153">Gives you time to update your app so it works with the latest changes.</span></span>

<span data-ttu-id="31c52-154">[MvcOptions](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) 類別來源註解充分說明變更的項目，以及所做變更對於大部分使用者來說都得到改善的原因。</span><span class="sxs-lookup"><span data-stu-id="31c52-154">The [MvcOptions](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) class source comments have a good explanation of what changed and why the changes are an improvement for most users.</span></span>

<span data-ttu-id="31c52-155">在未來某個日期將會推出 [ASP.NET Core 3.0 版](https://github.com/aspnet/Home/wiki/Roadmap)。</span><span class="sxs-lookup"><span data-stu-id="31c52-155">At some future date, there will be an [ASP.NET Core 3.0 version](https://github.com/aspnet/Home/wiki/Roadmap).</span></span> <span data-ttu-id="31c52-156">3.0 版將移除相容性參數支援的舊行為。</span><span class="sxs-lookup"><span data-stu-id="31c52-156">Old behaviors supported by compatibility switches will be removed in the 3.0 version.</span></span> <span data-ttu-id="31c52-157">我們覺得這些是有利於幾乎所有使用者的正向變更。</span><span class="sxs-lookup"><span data-stu-id="31c52-157">We feel these are positive changes benefitting nearly all users.</span></span> <span data-ttu-id="31c52-158">現在引進這些變更，大部分的應用程式皆可獲益，而其他人則有時間來更新其應用程式。</span><span class="sxs-lookup"><span data-stu-id="31c52-158">By introducing these changes now, most apps can benefit now, and the others will have time to update their applications.</span></span>

::: moniker-end

## <a name="services-available-in-startup"></a><span data-ttu-id="31c52-159">Startup 中可用的服務</span><span class="sxs-lookup"><span data-stu-id="31c52-159">Services available in Startup</span></span>

<span data-ttu-id="31c52-160">Web 主機提供一些可用於 `Startup` 類別建構函式的服務。</span><span class="sxs-lookup"><span data-stu-id="31c52-160">The web host provides some services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="31c52-161">應用程式會透過 `ConfigureServices` 新增其他服務。</span><span class="sxs-lookup"><span data-stu-id="31c52-161">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="31c52-162">然後，主機和應用程式服務都可以在 `Configure` 和整個應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="31c52-162">Both the host and app services are then available in `Configure` and throughout the application.</span></span>

## <a name="the-configure-method"></a><span data-ttu-id="31c52-163">Configure 方法</span><span class="sxs-lookup"><span data-stu-id="31c52-163">The Configure method</span></span>

<span data-ttu-id="31c52-164">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) 方法用來指定應用程式如何回應 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="31c52-164">The [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) method is used to specify how the app responds to HTTP requests.</span></span> <span data-ttu-id="31c52-165">藉由將[中介軟體](xref:fundamentals/middleware/index)元件新增至 [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) 執行個體，即可設定要求管線。</span><span class="sxs-lookup"><span data-stu-id="31c52-165">The request pipeline is configured by adding [middleware](xref:fundamentals/middleware/index) components to an [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) instance.</span></span> <span data-ttu-id="31c52-166">`IApplicationBuilder` 可用於 `Configure` 方法，但它未註冊在服務容器中。</span><span class="sxs-lookup"><span data-stu-id="31c52-166">`IApplicationBuilder` is available to the `Configure` method, but it isn't registered in the service container.</span></span> <span data-ttu-id="31c52-167">裝載會建立 `IApplicationBuilder`，並將它直接傳遞至 `Configure` ([參考來源](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192))。</span><span class="sxs-lookup"><span data-stu-id="31c52-167">Hosting creates an `IApplicationBuilder` and passes it directly to `Configure` ([reference source](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).</span></span>

<span data-ttu-id="31c52-168">[ASP.NET Core 範本](/dotnet/core/tools/dotnet-new)將管線設定為支援開發人員例外狀況頁面、[BrowserLink](http://vswebessentials.com/features/browserlink)、錯誤頁面、靜態檔案，以及 ASP.NET MVC：</span><span class="sxs-lookup"><span data-stu-id="31c52-168">The [ASP.NET Core templates](/dotnet/core/tools/dotnet-new) configure the pipeline with support for a developer exception page, [BrowserLink](http://vswebessentials.com/features/browserlink), error pages, static files, and ASP.NET MVC:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

<span data-ttu-id="31c52-169">每個 `Use` 擴充方法會將中介軟體元件新增至要求管線。</span><span class="sxs-lookup"><span data-stu-id="31c52-169">Each `Use` extension method adds a middleware component to the request pipeline.</span></span> <span data-ttu-id="31c52-170">比方說，`UseMvc` 擴充方法會將[路由中介軟體](xref:fundamentals/routing)新增至要求管線，並將 [MVC](xref:mvc/overview) 設定為預設處理常式。</span><span class="sxs-lookup"><span data-stu-id="31c52-170">For instance, the `UseMvc` extension method adds the [Routing Middleware](xref:fundamentals/routing) to the request pipeline and configures [MVC](xref:mvc/overview) as the default handler.</span></span>

<span data-ttu-id="31c52-171">要求管線中的每個中介軟體元件負責叫用管線中的下一個元件，或於適當時，對鏈結執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="31c52-171">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the chain, if appropriate.</span></span> <span data-ttu-id="31c52-172">如果沿著中介軟體鏈結不會進行最少運算，每個中介軟體在將要求傳送至用戶端之前，會有第二次機會來處理該要求。</span><span class="sxs-lookup"><span data-stu-id="31c52-172">If short-circuiting doesn't occur along the middleware chain, each middleware has a second chance to process the request before it's sent to the client.</span></span>

<span data-ttu-id="31c52-173">`IHostingEnvironment` 和 `ILoggerFactory` 等其他服務也可以指定在方法簽章中。</span><span class="sxs-lookup"><span data-stu-id="31c52-173">Additional services, such as `IHostingEnvironment` and `ILoggerFactory`, may also be specified in the method signature.</span></span> <span data-ttu-id="31c52-174">如果已指定，其他服務在可用時即會插入。</span><span class="sxs-lookup"><span data-stu-id="31c52-174">When specified, additional services are injected if they're available.</span></span>

<span data-ttu-id="31c52-175">如需 `IApplicationBuilder` 使用方式和中介軟體處理順序的詳細資訊，請參閱[中介軟體](xref:fundamentals/middleware/index)。</span><span class="sxs-lookup"><span data-stu-id="31c52-175">For more information on how to use `IApplicationBuilder` and the order of middleware processing, see [Middleware](xref:fundamentals/middleware/index).</span></span>

## <a name="convenience-methods"></a><span data-ttu-id="31c52-176">便利的方法</span><span class="sxs-lookup"><span data-stu-id="31c52-176">Convenience methods</span></span>

<span data-ttu-id="31c52-177">您可以使用 [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) 和 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) 這些便利的方法來取代指定 `Startup` 類別。</span><span class="sxs-lookup"><span data-stu-id="31c52-177">[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) and [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) convenience methods can be used instead of specifying a `Startup` class.</span></span> <span data-ttu-id="31c52-178">多次呼叫 `ConfigureServices` 會彼此附加。</span><span class="sxs-lookup"><span data-stu-id="31c52-178">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="31c52-179">多個呼叫 `Configure` 則使用最後一個方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="31c52-179">Multiple calls to `Configure` use the last method call.</span></span>

[!code-csharp[](startup/snapshot_sample/Program.cs?highlight=18,22)]

## <a name="startup-filters"></a><span data-ttu-id="31c52-180">啟動篩選條件</span><span class="sxs-lookup"><span data-stu-id="31c52-180">Startup filters</span></span>

<span data-ttu-id="31c52-181">使用 [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) 在應用程式的 [Configure](#the-configure-method) 中介軟體管線的開頭或結尾設定中介軟體。</span><span class="sxs-lookup"><span data-stu-id="31c52-181">Use [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) to configure middleware at the beginning or end of an app's [Configure](#the-configure-method) middleware pipeline.</span></span> <span data-ttu-id="31c52-182">`IStartupFilter` 有助於確保某個中介軟體會在應用程式要求處理管線的開頭或結尾，於程式庫所新增的中介軟體之前或之後執行。</span><span class="sxs-lookup"><span data-stu-id="31c52-182">`IStartupFilter` is useful to ensure that a middleware runs before or after middleware added by libraries at the start or end of the app's request processing pipeline.</span></span>

<span data-ttu-id="31c52-183">`IStartupFilter` 會實作 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure) 這個單一方法，以接收並傳回 `Action<IApplicationBuilder>`。</span><span class="sxs-lookup"><span data-stu-id="31c52-183">`IStartupFilter` implements a single method, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), which receives and returns an `Action<IApplicationBuilder>`.</span></span> <span data-ttu-id="31c52-184">[IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) 會定義類別來設定應用程式的要求管線。</span><span class="sxs-lookup"><span data-stu-id="31c52-184">An [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) defines a class to configure an app's request pipeline.</span></span> <span data-ttu-id="31c52-185">如需詳細資訊，請參閱[使用 IApplicationBuilder 建立中介軟體管線](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder)。</span><span class="sxs-lookup"><span data-stu-id="31c52-185">For more information, see [Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder).</span></span>

<span data-ttu-id="31c52-186">每個 `IStartupFilter` 會在要求管線中實作一或多個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="31c52-186">Each `IStartupFilter` implements one or more middlewares in the request pipeline.</span></span> <span data-ttu-id="31c52-187">篩選條件將依照它們新增至服務容器的順序叫用。</span><span class="sxs-lookup"><span data-stu-id="31c52-187">The filters are invoked in the order they were added to the service container.</span></span> <span data-ttu-id="31c52-188">篩選條件可能會在控制權傳給下一個篩選條件之前或之後新增中介軟體，因此它們會附加至應用程式管線的開頭或結尾。</span><span class="sxs-lookup"><span data-stu-id="31c52-188">Filters may add middleware before or after passing control to the next filter, thus they append to the beginning or end of the app pipeline.</span></span>

<span data-ttu-id="31c52-189">[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([如何下載](xref:tutorials/index#how-to-download-a-sample)) 示範如何使用 `IStartupFilter` 註冊中介軟體。</span><span class="sxs-lookup"><span data-stu-id="31c52-189">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) demonstrates how to register a middleware with `IStartupFilter`.</span></span> <span data-ttu-id="31c52-190">範例應用程式包含一個中介軟體，用來從查詢字串參數設定選項值：</span><span class="sxs-lookup"><span data-stu-id="31c52-190">The sample app includes a middleware that sets an options value from a query string parameter:</span></span>

[!code-csharp[](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

<span data-ttu-id="31c52-191">`RequestSetOptionsMiddleware` 設定在 `RequestSetOptionsStartupFilter` 類別中：</span><span class="sxs-lookup"><span data-stu-id="31c52-191">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

<span data-ttu-id="31c52-192">`IStartupFilter` 註冊在 `ConfigureServices` 的服務容器中：</span><span class="sxs-lookup"><span data-stu-id="31c52-192">The `IStartupFilter` is registered in the service container in `ConfigureServices`:</span></span>

[!code-csharp[](startup/sample/Startup.cs?name=snippet1&highlight=3)]

<span data-ttu-id="31c52-193">提供 `option` 的查詢字串參數時，中介軟體會在 MVC 中介軟體呈現回應之前，先處理值指派：</span><span class="sxs-lookup"><span data-stu-id="31c52-193">When a query string parameter for `option` is provided, the middleware processes the value assignment before the MVC middleware renders the response:</span></span>

![顯示呈現的 Index 頁面的瀏覽器視窗。](startup/_static/index.png)

<span data-ttu-id="31c52-196">中介軟體執行順序是依照 `IStartupFilter` 註冊順序來設定：</span><span class="sxs-lookup"><span data-stu-id="31c52-196">Middleware execution order is set by the order of `IStartupFilter` registrations:</span></span>

* <span data-ttu-id="31c52-197">多個 `IStartupFilter` 實作可能會與相同的物件互動。</span><span class="sxs-lookup"><span data-stu-id="31c52-197">Multiple `IStartupFilter` implementations may interact with the same objects.</span></span> <span data-ttu-id="31c52-198">如果順序很重要，請排列其 `IStartupFilter` 服務註冊順序，以符合其中介軟體執行應該依照的順序。</span><span class="sxs-lookup"><span data-stu-id="31c52-198">If ordering is important, order their `IStartupFilter` service registrations to match the order that their middlewares should run.</span></span>
* <span data-ttu-id="31c52-199">程式庫可能使用一或多個 `IStartupFilter` 實作 (其在使用 `IStartupFilter` 註冊的其他應用程式中介軟體之前或之後執行) 來新增中介軟體。</span><span class="sxs-lookup"><span data-stu-id="31c52-199">Libraries may add middleware with one or more `IStartupFilter` implementations that run before or after other app middleware registered with `IStartupFilter`.</span></span> <span data-ttu-id="31c52-200">若要在程式庫的 `IStartupFilter` 所新增的中介軟體之前叫用 `IStartupFilter` 中介軟體，請在程式庫新增至服務容器之前放置服務註冊。</span><span class="sxs-lookup"><span data-stu-id="31c52-200">To invoke an `IStartupFilter` middleware before a middleware added by a library's `IStartupFilter`, position the service registration before the library is added to the service container.</span></span> <span data-ttu-id="31c52-201">若要在之後叫用它，請在新增程式庫之後放置服務註冊。</span><span class="sxs-lookup"><span data-stu-id="31c52-201">To invoke it afterward, position the service registration after the library is added.</span></span>

## <a name="adding-configuration-at-startup-from-an-external-assembly"></a><span data-ttu-id="31c52-202">在啟動時從外部組件新增組態</span><span class="sxs-lookup"><span data-stu-id="31c52-202">Adding configuration at startup from an external assembly</span></span>

<span data-ttu-id="31c52-203">實作 [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) 即可在啟動時，從應用程式非 `Startup` 類別的外部組件，對應用程式新增強功能。</span><span class="sxs-lookup"><span data-stu-id="31c52-203">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="31c52-204">如需詳細資訊，請參閱[從外部組件增強應用程式](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="31c52-204">For more information, see [Enhance an app from an external assembly](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="31c52-205">其他資源</span><span class="sxs-lookup"><span data-stu-id="31c52-205">Additional resources</span></span>

* [<span data-ttu-id="31c52-206">裝載</span><span class="sxs-lookup"><span data-stu-id="31c52-206">Hosting</span></span>](xref:fundamentals/host/index)
* [<span data-ttu-id="31c52-207">使用多重環境</span><span class="sxs-lookup"><span data-stu-id="31c52-207">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="31c52-208">中介軟體</span><span class="sxs-lookup"><span data-stu-id="31c52-208">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="31c52-209">記錄</span><span class="sxs-lookup"><span data-stu-id="31c52-209">Logging</span></span>](xref:fundamentals/logging/index)
* [<span data-ttu-id="31c52-210">組態</span><span class="sxs-lookup"><span data-stu-id="31c52-210">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="31c52-211">StartupLoader 類別：FindStartupType 方法 (參考來源)</span><span class="sxs-lookup"><span data-stu-id="31c52-211">StartupLoader class: FindStartupType method (reference source)</span></span>](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)
