---
title: .NET 泛型主機
author: rick-anderson
description: 了解 .NET Core 的泛型主機，其負責啟動應用程式及管理存留期。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 4/17/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/host/generic-host
ms.openlocfilehash: 5a2d39af6c921323ae9113fd4aca27dcdedd44a5
ms.sourcegitcommit: 895e952aec11c91d703fbdd3640a979307b8cc67
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/01/2020
ms.locfileid: "85793459"
---
# <a name="net-generic-host"></a><span data-ttu-id="8c25b-103">.NET 泛型主機</span><span class="sxs-lookup"><span data-stu-id="8c25b-103">.NET Generic Host</span></span>

::: moniker range=">= aspnetcore-3.0 <= aspnetcore-3.1"

<span data-ttu-id="8c25b-104">ASP.NET Core 範本會建立 .NET Core 泛型主機 <xref:Microsoft.Extensions.Hosting.HostBuilder> 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-104">The ASP.NET Core templates create a .NET Core Generic Host, <xref:Microsoft.Extensions.Hosting.HostBuilder>.</span></span>

## <a name="host-definition"></a><span data-ttu-id="8c25b-105">主機定義</span><span class="sxs-lookup"><span data-stu-id="8c25b-105">Host definition</span></span>

<span data-ttu-id="8c25b-106">「主機」\*\* 是封裝所有應用程式資源的物件，例如：</span><span class="sxs-lookup"><span data-stu-id="8c25b-106">A *host* is an object that encapsulates an app's resources, such as:</span></span>

* <span data-ttu-id="8c25b-107">相依性插入 (DI)</span><span class="sxs-lookup"><span data-stu-id="8c25b-107">Dependency injection (DI)</span></span>
* <span data-ttu-id="8c25b-108">記錄</span><span class="sxs-lookup"><span data-stu-id="8c25b-108">Logging</span></span>
* <span data-ttu-id="8c25b-109">組態</span><span class="sxs-lookup"><span data-stu-id="8c25b-109">Configuration</span></span>
* <span data-ttu-id="8c25b-110">`IHostedService` 實作</span><span class="sxs-lookup"><span data-stu-id="8c25b-110">`IHostedService` implementations</span></span>

<span data-ttu-id="8c25b-111">當主機啟動時，它會 <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync%2A?displayProperty=nameWithType> 在 <xref:Microsoft.Extensions.Hosting.IHostedService> 服務容器的託管服務集合中註冊的每個執行上呼叫。</span><span class="sxs-lookup"><span data-stu-id="8c25b-111">When a host starts, it calls <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync%2A?displayProperty=nameWithType> on each implementation of <xref:Microsoft.Extensions.Hosting.IHostedService> registered in the service container's collection of hosted services.</span></span> <span data-ttu-id="8c25b-112">在 Web 應用程式中，其中一個 `IHostedService` 實作是一種 Web 服務，負責啟動 [HTTP 伺服器實作](xref:fundamentals/index#servers)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-112">In a web app, one of the `IHostedService` implementations is a web service that starts an [HTTP server implementation](xref:fundamentals/index#servers).</span></span>

<span data-ttu-id="8c25b-113">在單一物件中包含所有應用程式相互依存資源的主要理由便是生命週期管理：控制應用程式的啟動及順利關機。</span><span class="sxs-lookup"><span data-stu-id="8c25b-113">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="8c25b-114">設定主機</span><span class="sxs-lookup"><span data-stu-id="8c25b-114">Set up a host</span></span>

<span data-ttu-id="8c25b-115">主機通常由 `Program` 類別的程式碼來設定、建置並執行。</span><span class="sxs-lookup"><span data-stu-id="8c25b-115">The host is typically configured, built, and run by code in the `Program` class.</span></span> <span data-ttu-id="8c25b-116">`Main` 方法：</span><span class="sxs-lookup"><span data-stu-id="8c25b-116">The `Main` method:</span></span>

* <span data-ttu-id="8c25b-117">呼叫 `CreateHostBuilder` 方法來建立及設定建立器物件。</span><span class="sxs-lookup"><span data-stu-id="8c25b-117">Calls a `CreateHostBuilder` method to create and configure a builder object.</span></span>
* <span data-ttu-id="8c25b-118">在建立器物件上呼叫 `Build` 和 `Run` 方法。</span><span class="sxs-lookup"><span data-stu-id="8c25b-118">Calls `Build` and `Run` methods on the builder object.</span></span>

<span data-ttu-id="8c25b-119">ASP.NET Core web 範本會產生下列程式碼來建立一般主機：</span><span class="sxs-lookup"><span data-stu-id="8c25b-119">The ASP.NET Core web templates generate the following code to create a Generic Host:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStartup<Startup>();
            });
}
```

<span data-ttu-id="8c25b-120">下列程式碼會使用非 HTTP 工作負載建立泛型主機。</span><span class="sxs-lookup"><span data-stu-id="8c25b-120">The following code creates a Generic Host using non-HTTP workload.</span></span> <span data-ttu-id="8c25b-121">`IHostedService`執行會新增至 DI 容器：</span><span class="sxs-lookup"><span data-stu-id="8c25b-121">The `IHostedService` implementation is added to the DI container:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureServices((hostContext, services) =>
            {
               services.AddHostedService<Worker>();
            });
}
```

<span data-ttu-id="8c25b-122">針對 HTTP 工作負載，`Main` 方法相同，但 `CreateHostBuilder` 會呼叫 `ConfigureWebHostDefaults`：</span><span class="sxs-lookup"><span data-stu-id="8c25b-122">For an HTTP workload, the `Main` method is the same but `CreateHostBuilder` calls `ConfigureWebHostDefaults`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="8c25b-123">上述程式碼是由 ASP.NET Core 範本所產生。</span><span class="sxs-lookup"><span data-stu-id="8c25b-123">The preceding code is generated by the ASP.NET Core templates.</span></span>

<span data-ttu-id="8c25b-124">如果應用程式使用 Entity Framework Core，請勿變更 `CreateHostBuilder` 方法的名稱或簽章。</span><span class="sxs-lookup"><span data-stu-id="8c25b-124">If the app uses Entity Framework Core, don't change the name or signature of the `CreateHostBuilder` method.</span></span> <span data-ttu-id="8c25b-125">[Entity Framework Core 工具](/ef/core/miscellaneous/cli/)預期找到 `CreateHostBuilder` 方法，其在不執行應用程式的情況下設定主機。</span><span class="sxs-lookup"><span data-stu-id="8c25b-125">The [Entity Framework Core tools](/ef/core/miscellaneous/cli/) expect to find a `CreateHostBuilder` method that configures the host without running the app.</span></span> <span data-ttu-id="8c25b-126">如需詳細資訊，請參閱[設計階段 DbContext 建立](/ef/core/miscellaneous/cli/dbcontext-creation)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-126">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

## <a name="default-builder-settings"></a><span data-ttu-id="8c25b-127">預設建立器設定</span><span class="sxs-lookup"><span data-stu-id="8c25b-127">Default builder settings</span></span>

<span data-ttu-id="8c25b-128"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> 方法：</span><span class="sxs-lookup"><span data-stu-id="8c25b-128">The <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> method:</span></span>

* <span data-ttu-id="8c25b-129">將[內容根目錄](xref:fundamentals/index#content-root)設定為所傳回的路徑 <xref:System.IO.Directory.GetCurrentDirectory*> 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-129">Sets the [content root](xref:fundamentals/index#content-root) to the path returned by <xref:System.IO.Directory.GetCurrentDirectory*>.</span></span>
* <span data-ttu-id="8c25b-130">從下列項目載入主機組態：</span><span class="sxs-lookup"><span data-stu-id="8c25b-130">Loads host configuration from:</span></span>
  * <span data-ttu-id="8c25b-131">前面加上的環境變數 `DOTNET_` 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-131">Environment variables prefixed with `DOTNET_`.</span></span>
  * <span data-ttu-id="8c25b-132">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="8c25b-132">Command-line arguments.</span></span>
* <span data-ttu-id="8c25b-133">從下列項目載入應用程式組態：</span><span class="sxs-lookup"><span data-stu-id="8c25b-133">Loads app configuration from:</span></span>
  * <span data-ttu-id="8c25b-134">*appsettings.js*。</span><span class="sxs-lookup"><span data-stu-id="8c25b-134">*appsettings.json*.</span></span>
  * <span data-ttu-id="8c25b-135">*appsettings.{Environment}.json*</span><span class="sxs-lookup"><span data-stu-id="8c25b-135">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="8c25b-136">應用程式在 `Development` 環境中執行時的[祕密管理員](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-136">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="8c25b-137">環境變數。</span><span class="sxs-lookup"><span data-stu-id="8c25b-137">Environment variables.</span></span>
  * <span data-ttu-id="8c25b-138">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="8c25b-138">Command-line arguments.</span></span>
* <span data-ttu-id="8c25b-139">新增下列[記錄](xref:fundamentals/logging/index)提供者：</span><span class="sxs-lookup"><span data-stu-id="8c25b-139">Adds the following [logging](xref:fundamentals/logging/index) providers:</span></span>
  * <span data-ttu-id="8c25b-140">主控台</span><span class="sxs-lookup"><span data-stu-id="8c25b-140">Console</span></span>
  * <span data-ttu-id="8c25b-141">偵錯</span><span class="sxs-lookup"><span data-stu-id="8c25b-141">Debug</span></span>
  * <span data-ttu-id="8c25b-142">EventSource</span><span class="sxs-lookup"><span data-stu-id="8c25b-142">EventSource</span></span>
  * <span data-ttu-id="8c25b-143">EventLog (僅當在 Windows 上執行時)</span><span class="sxs-lookup"><span data-stu-id="8c25b-143">EventLog (only when running on Windows)</span></span>
* <span data-ttu-id="8c25b-144">環境為開發時，會啟用[範圍驗證](xref:fundamentals/dependency-injection#scope-validation)和[相依性驗證](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-144">Enables [scope validation](xref:fundamentals/dependency-injection#scope-validation) and [dependency validation](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) when the environment is Development.</span></span>

<span data-ttu-id="8c25b-145">`ConfigureWebHostDefaults` 方法：</span><span class="sxs-lookup"><span data-stu-id="8c25b-145">The `ConfigureWebHostDefaults` method:</span></span>

* <span data-ttu-id="8c25b-146">從前面加上的環境變數載入主機設定 `ASPNETCORE_` 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-146">Loads host configuration from environment variables prefixed with `ASPNETCORE_`.</span></span>
* <span data-ttu-id="8c25b-147">會將 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器設為網頁伺服器，並使用應用程式的主機組態提供者進行設定。</span><span class="sxs-lookup"><span data-stu-id="8c25b-147">Sets [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and configures it using the app's hosting configuration providers.</span></span> <span data-ttu-id="8c25b-148">如需 Kestrel 伺服器的預設選項，請參閱 <xref:fundamentals/servers/kestrel#kestrel-options>。</span><span class="sxs-lookup"><span data-stu-id="8c25b-148">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="8c25b-149">新增[主機篩選中介軟體](xref:fundamentals/servers/kestrel#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-149">Adds [Host Filtering middleware](xref:fundamentals/servers/kestrel#host-filtering).</span></span>
* <span data-ttu-id="8c25b-150">如果等於，則新增[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) `ASPNETCORE_FORWARDEDHEADERS_ENABLED` `true` 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-150">Adds [Forwarded Headers middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) if `ASPNETCORE_FORWARDEDHEADERS_ENABLED` equals `true`.</span></span>
* <span data-ttu-id="8c25b-151">啟用 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="8c25b-151">Enables IIS integration.</span></span> <span data-ttu-id="8c25b-152">如需 IIS 預設選項，請參閱 <xref:host-and-deploy/iis/index#iis-options>。</span><span class="sxs-lookup"><span data-stu-id="8c25b-152">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>

<span data-ttu-id="8c25b-153">本文稍後的[＜設定所有應用程式類型＞](#settings-for-all-app-types)和[＜Web 應用程式設定＞](#settings-for-web-apps)章節，將說明如何覆寫預設的建立器設定。</span><span class="sxs-lookup"><span data-stu-id="8c25b-153">The [Settings for all app types](#settings-for-all-app-types) and [Settings for web apps](#settings-for-web-apps) sections later in this article show how to override default builder settings.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="8c25b-154">架構提供的服務</span><span class="sxs-lookup"><span data-stu-id="8c25b-154">Framework-provided services</span></span>

<span data-ttu-id="8c25b-155">系統會自動註冊下列服務：</span><span class="sxs-lookup"><span data-stu-id="8c25b-155">The following services are registered automatically:</span></span>

* [<span data-ttu-id="8c25b-156">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="8c25b-156">IHostApplicationLifetime</span></span>](#ihostapplicationlifetime)
* [<span data-ttu-id="8c25b-157">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="8c25b-157">IHostLifetime</span></span>](#ihostlifetime)
* [<span data-ttu-id="8c25b-158">IHostEnvironment / IWebHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="8c25b-158">IHostEnvironment / IWebHostEnvironment</span></span>](#ihostenvironment)

<span data-ttu-id="8c25b-159">如需架構所提供之服務的詳細資訊，請參閱 <xref:fundamentals/dependency-injection#framework-provided-services> 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-159">For more information on framework-provided services, see <xref:fundamentals/dependency-injection#framework-provided-services>.</span></span>

## <a name="ihostapplicationlifetime"></a><span data-ttu-id="8c25b-160">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="8c25b-160">IHostApplicationLifetime</span></span>

<span data-ttu-id="8c25b-161">將 <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (先前稱為 `IApplicationLifetime`) 服務插入任何類別來處理啟動後和順利關機工作。</span><span class="sxs-lookup"><span data-stu-id="8c25b-161">Inject the <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (formerly `IApplicationLifetime`) service into any class to handle post-startup and graceful shutdown tasks.</span></span> <span data-ttu-id="8c25b-162">介面上的三個屬性是用於註冊應用程式啟動和應用程式關閉事件處理程序方法的取消語彙基元。</span><span class="sxs-lookup"><span data-stu-id="8c25b-162">Three properties on the interface are cancellation tokens used to register app start and app stop event handler methods.</span></span> <span data-ttu-id="8c25b-163">介面也包括 `StopApplication` 方法。</span><span class="sxs-lookup"><span data-stu-id="8c25b-163">The interface also includes a `StopApplication` method.</span></span>

<span data-ttu-id="8c25b-164">下列範例是 `IHostedService` 註冊事件的實作為 `IHostApplicationLifetime` ：</span><span class="sxs-lookup"><span data-stu-id="8c25b-164">The following example is an `IHostedService` implementation that registers `IHostApplicationLifetime` events:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a><span data-ttu-id="8c25b-165">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="8c25b-165">IHostLifetime</span></span>

<span data-ttu-id="8c25b-166"><xref:Microsoft.Extensions.Hosting.IHostLifetime> 實作會控制主機啟動及停止的時機。</span><span class="sxs-lookup"><span data-stu-id="8c25b-166">The <xref:Microsoft.Extensions.Hosting.IHostLifetime> implementation controls when the host starts and when it stops.</span></span> <span data-ttu-id="8c25b-167">會使用最後一個註冊的實作。</span><span class="sxs-lookup"><span data-stu-id="8c25b-167">The last implementation registered is used.</span></span>

<span data-ttu-id="8c25b-168">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` 是預設的 `IHostLifetime` 實作。</span><span class="sxs-lookup"><span data-stu-id="8c25b-168">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is the default `IHostLifetime` implementation.</span></span> <span data-ttu-id="8c25b-169">`ConsoleLifetime`:</span><span class="sxs-lookup"><span data-stu-id="8c25b-169">`ConsoleLifetime`:</span></span>

* <span data-ttu-id="8c25b-170">接聽<kbd>Ctrl</kbd> + <kbd>C</kbd>/SIGINT 或 SIGTERM，並呼叫 <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*> 來啟動關機程式。</span><span class="sxs-lookup"><span data-stu-id="8c25b-170">Listens for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*> to start the shutdown process.</span></span>
* <span data-ttu-id="8c25b-171">會解除封鎖 [RunAsync](#runasync) 和 [WaitForShutdownAsync](#waitforshutdownasync) 等延伸模組。</span><span class="sxs-lookup"><span data-stu-id="8c25b-171">Unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span>

## <a name="ihostenvironment"></a><span data-ttu-id="8c25b-172">IHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="8c25b-172">IHostEnvironment</span></span>

<span data-ttu-id="8c25b-173">將服務插入至 <xref:Microsoft.Extensions.Hosting.IHostEnvironment> 類別，以取得下列設定的相關資訊：</span><span class="sxs-lookup"><span data-stu-id="8c25b-173">Inject the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> service into a class to get information about the following settings:</span></span>

* [<span data-ttu-id="8c25b-174">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="8c25b-174">ApplicationName</span></span>](#applicationname)
* [<span data-ttu-id="8c25b-175">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="8c25b-175">EnvironmentName</span></span>](#environmentname)
* [<span data-ttu-id="8c25b-176">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="8c25b-176">ContentRootPath</span></span>](#contentroot)

<span data-ttu-id="8c25b-177">Web apps 會執行 `IWebHostEnvironment` 介面，它會繼承 `IHostEnvironment` 並新增[WebRootPath](#webroot)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-177">Web apps implement the `IWebHostEnvironment` interface, which inherits `IHostEnvironment` and adds the [WebRootPath](#webroot).</span></span>

## <a name="host-configuration"></a><span data-ttu-id="8c25b-178">主機組態</span><span class="sxs-lookup"><span data-stu-id="8c25b-178">Host configuration</span></span>

<span data-ttu-id="8c25b-179">主機組態用於 <xref:Microsoft.Extensions.Hosting.IHostEnvironment> 實作的屬性。</span><span class="sxs-lookup"><span data-stu-id="8c25b-179">Host configuration is used for the properties of the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementation.</span></span>

<span data-ttu-id="8c25b-180">主機組態位於 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 內的 [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-180">Host configuration is available from [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) inside <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span></span> <span data-ttu-id="8c25b-181">在 `ConfigureAppConfiguration` 之後，應用程式組態會取代 `HostBuilderContext.Configuration`。</span><span class="sxs-lookup"><span data-stu-id="8c25b-181">After `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` is replaced with the app config.</span></span>

<span data-ttu-id="8c25b-182">若要新增主機組態，請呼叫 `IHostBuilder` 上的 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>。</span><span class="sxs-lookup"><span data-stu-id="8c25b-182">To add host configuration, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="8c25b-183">`ConfigureHostConfiguration` 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="8c25b-183">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="8c25b-184">主機會使用指定索引鍵上最後設定值的任何選項。</span><span class="sxs-lookup"><span data-stu-id="8c25b-184">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="8c25b-185">具有前置詞 `DOTNET_` 和命令列引數的環境變數提供者是由所包含 `CreateDefaultBuilder` 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-185">The environment variable provider with prefix `DOTNET_` and command-line arguments are included by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="8c25b-186">針對 Web 應用程式，會新增具有前置詞 `ASPNETCORE_` 的環境變數提供者。</span><span class="sxs-lookup"><span data-stu-id="8c25b-186">For web apps, the environment variable provider with prefix `ASPNETCORE_` is added.</span></span> <span data-ttu-id="8c25b-187">讀取環境變數時，就會移除前置詞。</span><span class="sxs-lookup"><span data-stu-id="8c25b-187">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="8c25b-188">例如，`ASPNETCORE_ENVIRONMENT` 的環境變數值會變成 `environment` 索引鍵的主機組態值。</span><span class="sxs-lookup"><span data-stu-id="8c25b-188">For example, the environment variable value for `ASPNETCORE_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="8c25b-189">下列範例會建立主機組態：</span><span class="sxs-lookup"><span data-stu-id="8c25b-189">The following example creates host configuration:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a><span data-ttu-id="8c25b-190">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="8c25b-190">App configuration</span></span>

<span data-ttu-id="8c25b-191">應用程式組態的建立方式是在 `IHostBuilder` 上呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>。</span><span class="sxs-lookup"><span data-stu-id="8c25b-191">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="8c25b-192">`ConfigureAppConfiguration` 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="8c25b-192">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="8c25b-193">應用程式會使用指定索引鍵上最後設定值的任何選項。</span><span class="sxs-lookup"><span data-stu-id="8c25b-193">The app uses whichever option sets a value last on a given key.</span></span> 

<span data-ttu-id="8c25b-194">由 `ConfigureAppConfiguration` 建立的組態位於 [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*)，可用於後續作業並用作 DI 中的服務。</span><span class="sxs-lookup"><span data-stu-id="8c25b-194">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and as a service from DI.</span></span> <span data-ttu-id="8c25b-195">主機組態也會新增至應用程式組態。</span><span class="sxs-lookup"><span data-stu-id="8c25b-195">The host configuration is also added to the app configuration.</span></span>

<span data-ttu-id="8c25b-196">如需詳細資訊，請參閱 [ASP.NET Core 中的組態](xref:fundamentals/configuration/index#configureappconfiguration)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-196">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span></span>

## <a name="settings-for-all-app-types"></a><span data-ttu-id="8c25b-197">所有應用程式類型的設定</span><span class="sxs-lookup"><span data-stu-id="8c25b-197">Settings for all app types</span></span>

<span data-ttu-id="8c25b-198">本節列出適用於 HTTP 和非 HTTP 工作負載的主機設定。</span><span class="sxs-lookup"><span data-stu-id="8c25b-198">This section lists host settings that apply to both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="8c25b-199">根據預設，用來設定這些設定的環境變數可以具有 `DOTNET_` 或 `ASPNETCORE_` 前置詞。</span><span class="sxs-lookup"><span data-stu-id="8c25b-199">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<!-- In the following sections, two spaces at end of line are used to force line breaks in the rendered page. -->

### <a name="applicationname"></a><span data-ttu-id="8c25b-200">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="8c25b-200">ApplicationName</span></span>

<span data-ttu-id="8c25b-201">[IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) 屬性是在主機建構期間從主機組態當中設定。</span><span class="sxs-lookup"><span data-stu-id="8c25b-201">The [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span>

<span data-ttu-id="8c25b-202">機**碼**：`applicationName`</span><span class="sxs-lookup"><span data-stu-id="8c25b-202">**Key**: `applicationName`</span></span>  
<span data-ttu-id="8c25b-203">**類型**：`string`</span><span class="sxs-lookup"><span data-stu-id="8c25b-203">**Type**: `string`</span></span>  
<span data-ttu-id="8c25b-204">**預設值**：包含應用程式進入點的元件名稱。</span><span class="sxs-lookup"><span data-stu-id="8c25b-204">**Default**: The name of the assembly that contains the app's entry point.</span></span>  
<span data-ttu-id="8c25b-205">**環境變數**：`<PREFIX_>APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="8c25b-205">**Environment variable**: `<PREFIX_>APPLICATIONNAME`</span></span>

<span data-ttu-id="8c25b-206">若要設定此值，請使用環境變數。</span><span class="sxs-lookup"><span data-stu-id="8c25b-206">To set this value, use the environment variable.</span></span> 

### <a name="contentroot"></a><span data-ttu-id="8c25b-207">ContentRoot</span><span class="sxs-lookup"><span data-stu-id="8c25b-207">ContentRoot</span></span>

<span data-ttu-id="8c25b-208">[IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) 屬性會決定主機從哪裡開始搜尋內容檔案。</span><span class="sxs-lookup"><span data-stu-id="8c25b-208">The [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) property determines where the host begins searching for content files.</span></span> <span data-ttu-id="8c25b-209">如果路徑不存在，就無法啟動主機。</span><span class="sxs-lookup"><span data-stu-id="8c25b-209">If the path doesn't exist, the host fails to start.</span></span>

<span data-ttu-id="8c25b-210">機**碼**：`contentRoot`</span><span class="sxs-lookup"><span data-stu-id="8c25b-210">**Key**: `contentRoot`</span></span>  
<span data-ttu-id="8c25b-211">**類型**：`string`</span><span class="sxs-lookup"><span data-stu-id="8c25b-211">**Type**: `string`</span></span>  
<span data-ttu-id="8c25b-212">**預設值**：應用程式元件所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="8c25b-212">**Default**: The folder where the app assembly resides.</span></span>  
<span data-ttu-id="8c25b-213">**環境變數**：`<PREFIX_>CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="8c25b-213">**Environment variable**: `<PREFIX_>CONTENTROOT`</span></span>

<span data-ttu-id="8c25b-214">若要設定此值，請使用環境變數或呼叫 `IHostBuilder` 上的 `UseContentRoot`：</span><span class="sxs-lookup"><span data-stu-id="8c25b-214">To set this value, use the environment variable or call `UseContentRoot` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

<span data-ttu-id="8c25b-215">如需詳細資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="8c25b-215">For more information, see:</span></span>

* [<span data-ttu-id="8c25b-216">基本概念：內容根目錄</span><span class="sxs-lookup"><span data-stu-id="8c25b-216">Fundamentals: Content root</span></span>](xref:fundamentals/index#content-root)
* [<span data-ttu-id="8c25b-217">WebRoot</span><span class="sxs-lookup"><span data-stu-id="8c25b-217">WebRoot</span></span>](#webroot)

### <a name="environmentname"></a><span data-ttu-id="8c25b-218">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="8c25b-218">EnvironmentName</span></span>

<span data-ttu-id="8c25b-219">[IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) 屬性可以設為任何值。</span><span class="sxs-lookup"><span data-stu-id="8c25b-219">The [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) property can be set to any value.</span></span> <span data-ttu-id="8c25b-220">架構定義的值包括 `Development`、`Staging` 和 `Production`。</span><span class="sxs-lookup"><span data-stu-id="8c25b-220">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="8c25b-221">值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="8c25b-221">Values aren't case-sensitive.</span></span>

<span data-ttu-id="8c25b-222">機**碼**：`environment`</span><span class="sxs-lookup"><span data-stu-id="8c25b-222">**Key**: `environment`</span></span>  
<span data-ttu-id="8c25b-223">**類型**：`string`</span><span class="sxs-lookup"><span data-stu-id="8c25b-223">**Type**: `string`</span></span>  
<span data-ttu-id="8c25b-224">**預設**：`Production`</span><span class="sxs-lookup"><span data-stu-id="8c25b-224">**Default**: `Production`</span></span>  
<span data-ttu-id="8c25b-225">**環境變數**：`<PREFIX_>ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="8c25b-225">**Environment variable**: `<PREFIX_>ENVIRONMENT`</span></span>

<span data-ttu-id="8c25b-226">若要設定此值，請使用環境變數或呼叫 `IHostBuilder` 上的 `UseEnvironment`：</span><span class="sxs-lookup"><span data-stu-id="8c25b-226">To set this value, use the environment variable or call `UseEnvironment` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a><span data-ttu-id="8c25b-227">ShutdownTimeout</span><span class="sxs-lookup"><span data-stu-id="8c25b-227">ShutdownTimeout</span></span>

<span data-ttu-id="8c25b-228">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) 會設定 <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> 的逾時。</span><span class="sxs-lookup"><span data-stu-id="8c25b-228">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="8c25b-229">預設值是五秒鐘。</span><span class="sxs-lookup"><span data-stu-id="8c25b-229">The default value is five seconds.</span></span>  <span data-ttu-id="8c25b-230">在逾時期間，主機會：</span><span class="sxs-lookup"><span data-stu-id="8c25b-230">During the timeout period, the host:</span></span>

* <span data-ttu-id="8c25b-231">觸發 [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.ihostapplicationlifetime.applicationstopping)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-231">Triggers [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.ihostapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="8c25b-232">嘗試停止託管的服務，並記錄無法停止之服務的錯誤。</span><span class="sxs-lookup"><span data-stu-id="8c25b-232">Attempts to stop hosted services, logging errors for services that fail to stop.</span></span>

<span data-ttu-id="8c25b-233">如果在所有的託管服務停止之前逾時期限已到期，則應用程式關閉時，會停止任何剩餘的作用中服務。</span><span class="sxs-lookup"><span data-stu-id="8c25b-233">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="8c25b-234">即使服務尚未完成處理也會停止。</span><span class="sxs-lookup"><span data-stu-id="8c25b-234">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="8c25b-235">如果服務需要更多時間才能停止，請增加逾時。</span><span class="sxs-lookup"><span data-stu-id="8c25b-235">If services require additional time to stop, increase the timeout.</span></span>

<span data-ttu-id="8c25b-236">機**碼**：`shutdownTimeoutSeconds`</span><span class="sxs-lookup"><span data-stu-id="8c25b-236">**Key**: `shutdownTimeoutSeconds`</span></span>  
<span data-ttu-id="8c25b-237">**類型**：`int`</span><span class="sxs-lookup"><span data-stu-id="8c25b-237">**Type**: `int`</span></span>  
<span data-ttu-id="8c25b-238">**預設值**：5秒</span><span class="sxs-lookup"><span data-stu-id="8c25b-238">**Default**: 5 seconds</span></span>  
<span data-ttu-id="8c25b-239">**環境變數**：`<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="8c25b-239">**Environment variable**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="8c25b-240">若要設定此值，請使用環境變數或設定 `HostOptions`。</span><span class="sxs-lookup"><span data-stu-id="8c25b-240">To set this value, use the environment variable or configure `HostOptions`.</span></span> <span data-ttu-id="8c25b-241">下列範例將逾時設為 20 秒：</span><span class="sxs-lookup"><span data-stu-id="8c25b-241">The following example sets the timeout to 20 seconds:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

## <a name="settings-for-web-apps"></a><span data-ttu-id="8c25b-242">Web 應用程式的設定</span><span class="sxs-lookup"><span data-stu-id="8c25b-242">Settings for web apps</span></span>

<span data-ttu-id="8c25b-243">某些主機設定僅適用於 HTTP 工作負載。</span><span class="sxs-lookup"><span data-stu-id="8c25b-243">Some host settings apply only to HTTP workloads.</span></span> <span data-ttu-id="8c25b-244">根據預設，用來設定這些設定的環境變數可以具有 `DOTNET_` 或 `ASPNETCORE_` 前置詞。</span><span class="sxs-lookup"><span data-stu-id="8c25b-244">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<span data-ttu-id="8c25b-245">`IWebHostBuilder` 上的擴充方法適用於這些設定。</span><span class="sxs-lookup"><span data-stu-id="8c25b-245">Extension methods on `IWebHostBuilder` are available for these settings.</span></span> <span data-ttu-id="8c25b-246">示範如何呼叫擴充方法的程式碼範例假設 `webBuilder` 是 `IWebHostBuilder` 的執行個體，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="8c25b-246">Code samples that show how to call the extension methods assume `webBuilder` is an instance of `IWebHostBuilder`, as in the following example:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a><span data-ttu-id="8c25b-247">CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="8c25b-247">CaptureStartupErrors</span></span>

<span data-ttu-id="8c25b-248">當它為 `false` 時，啟動期間發生的錯誤會導致主機結束。</span><span class="sxs-lookup"><span data-stu-id="8c25b-248">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="8c25b-249">當它為 `true` 時，主機會擷取啟動期間的例外狀況，並嘗試啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="8c25b-249">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

<span data-ttu-id="8c25b-250">機**碼**：`captureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="8c25b-250">**Key**: `captureStartupErrors`</span></span>  
<span data-ttu-id="8c25b-251">**類型**： `bool` （ `true` 或 `1` ）</span><span class="sxs-lookup"><span data-stu-id="8c25b-251">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="8c25b-252">**預設值**：預設為 `false`，除非應用程式執行時在 IIS 背後有 Kestrel，此時預設值即為 `true`。</span><span class="sxs-lookup"><span data-stu-id="8c25b-252">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="8c25b-253">**環境變數**：`<PREFIX_>CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="8c25b-253">**Environment variable**: `<PREFIX_>CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="8c25b-254">若要設定此值，請使用組態或呼叫 `CaptureStartupErrors`：</span><span class="sxs-lookup"><span data-stu-id="8c25b-254">To set this value, use configuration or call `CaptureStartupErrors`:</span></span>

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a><span data-ttu-id="8c25b-255">DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="8c25b-255">DetailedErrors</span></span>

<span data-ttu-id="8c25b-256">啟用時 (或當環境為 `Development` 時)，應用程式會擷取詳細錯誤。</span><span class="sxs-lookup"><span data-stu-id="8c25b-256">When enabled, or when the environment is `Development`, the app captures detailed errors.</span></span>

<span data-ttu-id="8c25b-257">機**碼**：`detailedErrors`</span><span class="sxs-lookup"><span data-stu-id="8c25b-257">**Key**: `detailedErrors`</span></span>  
<span data-ttu-id="8c25b-258">**類型**： `bool` （ `true` 或 `1` ）</span><span class="sxs-lookup"><span data-stu-id="8c25b-258">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="8c25b-259">**預設**：`false`</span><span class="sxs-lookup"><span data-stu-id="8c25b-259">**Default**: `false`</span></span>  
<span data-ttu-id="8c25b-260">**環境變數**：`<PREFIX_>_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="8c25b-260">**Environment variable**: `<PREFIX_>_DETAILEDERRORS`</span></span>

<span data-ttu-id="8c25b-261">若要設定此值，請使用組態或呼叫 `UseSetting`：</span><span class="sxs-lookup"><span data-stu-id="8c25b-261">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a><span data-ttu-id="8c25b-262">HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="8c25b-262">HostingStartupAssemblies</span></span>

<span data-ttu-id="8c25b-263">在啟動時載入的裝載啟動組件字串，以分號分隔。</span><span class="sxs-lookup"><span data-stu-id="8c25b-263">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="8c25b-264">雖然設定值會預設為空字串，但裝載啟動組件一律會包含應用程式的組件。</span><span class="sxs-lookup"><span data-stu-id="8c25b-264">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="8c25b-265">提供裝載啟動組件時，它們會新增至應用程式的組件，以便在應用程式在啟動時建置其通用服務時載入。</span><span class="sxs-lookup"><span data-stu-id="8c25b-265">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

<span data-ttu-id="8c25b-266">機**碼**：`hostingStartupAssemblies`</span><span class="sxs-lookup"><span data-stu-id="8c25b-266">**Key**: `hostingStartupAssemblies`</span></span>  
<span data-ttu-id="8c25b-267">**類型**：`string`</span><span class="sxs-lookup"><span data-stu-id="8c25b-267">**Type**: `string`</span></span>  
<span data-ttu-id="8c25b-268">**預設值**：空字串</span><span class="sxs-lookup"><span data-stu-id="8c25b-268">**Default**: Empty string</span></span>  
<span data-ttu-id="8c25b-269">**環境變數**：`<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="8c25b-269">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="8c25b-270">若要設定此值，請使用組態或呼叫 `UseSetting`：</span><span class="sxs-lookup"><span data-stu-id="8c25b-270">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a><span data-ttu-id="8c25b-271">HostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="8c25b-271">HostingStartupExcludeAssemblies</span></span>

<span data-ttu-id="8c25b-272">在啟動時排除以分號分隔的裝載啟動組件字串。</span><span class="sxs-lookup"><span data-stu-id="8c25b-272">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="8c25b-273">機**碼**：`hostingStartupExcludeAssemblies`</span><span class="sxs-lookup"><span data-stu-id="8c25b-273">**Key**: `hostingStartupExcludeAssemblies`</span></span>  
<span data-ttu-id="8c25b-274">**類型**：`string`</span><span class="sxs-lookup"><span data-stu-id="8c25b-274">**Type**: `string`</span></span>  
<span data-ttu-id="8c25b-275">**預設值**：空字串</span><span class="sxs-lookup"><span data-stu-id="8c25b-275">**Default**: Empty string</span></span>  
<span data-ttu-id="8c25b-276">**環境變數**：`<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="8c25b-276">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="8c25b-277">若要設定此值，請使用組態或呼叫 `UseSetting`：</span><span class="sxs-lookup"><span data-stu-id="8c25b-277">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="https_port"></a><span data-ttu-id="8c25b-278">HTTPS_Port</span><span class="sxs-lookup"><span data-stu-id="8c25b-278">HTTPS_Port</span></span>

<span data-ttu-id="8c25b-279">HTTPS 重新導向連接埠。</span><span class="sxs-lookup"><span data-stu-id="8c25b-279">The HTTPS redirect port.</span></span> <span data-ttu-id="8c25b-280">用於[強制 HTTPS](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-280">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="8c25b-281">機**碼**：`https_port`</span><span class="sxs-lookup"><span data-stu-id="8c25b-281">**Key**: `https_port`</span></span>  
<span data-ttu-id="8c25b-282">**類型**：`string`</span><span class="sxs-lookup"><span data-stu-id="8c25b-282">**Type**: `string`</span></span>  
<span data-ttu-id="8c25b-283">**預設**值：未設定預設值。</span><span class="sxs-lookup"><span data-stu-id="8c25b-283">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="8c25b-284">**環境變數**：`<PREFIX_>HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="8c25b-284">**Environment variable**: `<PREFIX_>HTTPS_PORT`</span></span>

<span data-ttu-id="8c25b-285">若要設定此值，請使用組態或呼叫 `UseSetting`：</span><span class="sxs-lookup"><span data-stu-id="8c25b-285">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a><span data-ttu-id="8c25b-286">PreferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="8c25b-286">PreferHostingUrls</span></span>

<span data-ttu-id="8c25b-287">指出主機是否應接聽以設定的 Url， `IWebHostBuilder` 而不是使用執行時所設定的 url `IServer` 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-287">Indicates whether the host should listen on the URLs configured with the `IWebHostBuilder` instead of those URLs configured with the `IServer` implementation.</span></span>

<span data-ttu-id="8c25b-288">機**碼**：`preferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="8c25b-288">**Key**: `preferHostingUrls`</span></span>  
<span data-ttu-id="8c25b-289">**類型**： `bool` （ `true` 或 `1` ）</span><span class="sxs-lookup"><span data-stu-id="8c25b-289">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="8c25b-290">**預設**：`true`</span><span class="sxs-lookup"><span data-stu-id="8c25b-290">**Default**: `true`</span></span>  
<span data-ttu-id="8c25b-291">**環境變數**：`<PREFIX_>_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="8c25b-291">**Environment variable**: `<PREFIX_>_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="8c25b-292">若要設定此值，請使用環境變數或呼叫 `PreferHostingUrls`：</span><span class="sxs-lookup"><span data-stu-id="8c25b-292">To set this value, use the environment variable or call `PreferHostingUrls`:</span></span>

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a><span data-ttu-id="8c25b-293">PreventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="8c25b-293">PreventHostingStartup</span></span>

<span data-ttu-id="8c25b-294">可防止自動載入裝載啟動組件，包括應用程式組件所設定的裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="8c25b-294">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="8c25b-295">如需詳細資訊，請參閱 <xref:fundamentals/configuration/platform-specific-configuration> 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-295">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="8c25b-296">機**碼**：`preventHostingStartup`</span><span class="sxs-lookup"><span data-stu-id="8c25b-296">**Key**: `preventHostingStartup`</span></span>  
<span data-ttu-id="8c25b-297">**類型**： `bool` （ `true` 或 `1` ）</span><span class="sxs-lookup"><span data-stu-id="8c25b-297">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="8c25b-298">**預設**：`false`</span><span class="sxs-lookup"><span data-stu-id="8c25b-298">**Default**: `false`</span></span>  
<span data-ttu-id="8c25b-299">**環境變數**：`<PREFIX_>_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="8c25b-299">**Environment variable**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="8c25b-300">若要設定此值，請使用環境變數或呼叫 `UseSetting`：</span><span class="sxs-lookup"><span data-stu-id="8c25b-300">To set this value, use the environment variable or call `UseSetting` :</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a><span data-ttu-id="8c25b-301">StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="8c25b-301">StartupAssembly</span></span>

<span data-ttu-id="8c25b-302">要搜尋 `Startup` 類別的組件。</span><span class="sxs-lookup"><span data-stu-id="8c25b-302">The assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="8c25b-303">機**碼**：`startupAssembly`</span><span class="sxs-lookup"><span data-stu-id="8c25b-303">**Key**: `startupAssembly`</span></span>  
<span data-ttu-id="8c25b-304">**類型**：`string`</span><span class="sxs-lookup"><span data-stu-id="8c25b-304">**Type**: `string`</span></span>  
<span data-ttu-id="8c25b-305">**預設值**：應用程式的組件</span><span class="sxs-lookup"><span data-stu-id="8c25b-305">**Default**: The app's assembly</span></span>  
<span data-ttu-id="8c25b-306">**環境變數**：`<PREFIX_>STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="8c25b-306">**Environment variable**: `<PREFIX_>STARTUPASSEMBLY`</span></span>

<span data-ttu-id="8c25b-307">若要設定此值，請使用環境變數或呼叫 `UseStartup`。</span><span class="sxs-lookup"><span data-stu-id="8c25b-307">To set this value, use the environment variable or call `UseStartup`.</span></span> <span data-ttu-id="8c25b-308">`UseStartup` 可以採用組件名稱 (`string`) 或類型 (`TStartup`)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-308">`UseStartup` can take an assembly name (`string`) or a type (`TStartup`).</span></span> <span data-ttu-id="8c25b-309">如果呼叫多個 `UseStartup` 方法，最後一個將會優先。</span><span class="sxs-lookup"><span data-stu-id="8c25b-309">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a><span data-ttu-id="8c25b-310">URL</span><span class="sxs-lookup"><span data-stu-id="8c25b-310">URLs</span></span>

<span data-ttu-id="8c25b-311">以分號分隔的 IP 位址或主機位址，包含伺服器應接聽要求的連接埠和通訊協定。</span><span class="sxs-lookup"><span data-stu-id="8c25b-311">A semicolon-delimited list of IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span> <span data-ttu-id="8c25b-312">例如 `http://localhost:123`。</span><span class="sxs-lookup"><span data-stu-id="8c25b-312">For example, `http://localhost:123`.</span></span> <span data-ttu-id="8c25b-313">使用 "\*"，表示伺服器應接聽任何 IP 位址或主機名稱上的要求，並使用指定的連接埠和通訊協定 (例如，`http://*:5000`)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-313">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="8c25b-314">通訊協定 (`http://` 或 `https://`) 必須包含在每個 URL 中。</span><span class="sxs-lookup"><span data-stu-id="8c25b-314">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="8c25b-315">支援的格式會依伺服器而有所不同。</span><span class="sxs-lookup"><span data-stu-id="8c25b-315">Supported formats vary among servers.</span></span>

<span data-ttu-id="8c25b-316">機**碼**：`urls`</span><span class="sxs-lookup"><span data-stu-id="8c25b-316">**Key**: `urls`</span></span>  
<span data-ttu-id="8c25b-317">**類型**：`string`</span><span class="sxs-lookup"><span data-stu-id="8c25b-317">**Type**: `string`</span></span>  
<span data-ttu-id="8c25b-318">**預設**： `http://localhost:5000` 和`https://localhost:5001`</span><span class="sxs-lookup"><span data-stu-id="8c25b-318">**Default**: `http://localhost:5000` and `https://localhost:5001`</span></span>  
<span data-ttu-id="8c25b-319">**環境變數**：`<PREFIX_>URLS`</span><span class="sxs-lookup"><span data-stu-id="8c25b-319">**Environment variable**: `<PREFIX_>URLS`</span></span>

<span data-ttu-id="8c25b-320">若要設定此值，請使用環境變數或呼叫 `UseUrls`：</span><span class="sxs-lookup"><span data-stu-id="8c25b-320">To set this value, use the environment variable or call `UseUrls`:</span></span>

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

<span data-ttu-id="8c25b-321">Kestrel 有它自己的端點設定 API。</span><span class="sxs-lookup"><span data-stu-id="8c25b-321">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="8c25b-322">如需詳細資訊，請參閱 <xref:fundamentals/servers/kestrel#endpoint-configuration> 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-322">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="webroot"></a><span data-ttu-id="8c25b-323">WebRoot</span><span class="sxs-lookup"><span data-stu-id="8c25b-323">WebRoot</span></span>

<span data-ttu-id="8c25b-324">[IWebHostEnvironment. WebRootPath](xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment.WebRootPath)屬性會決定應用程式靜態資產的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="8c25b-324">The [IWebHostEnvironment.WebRootPath](xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment.WebRootPath) property determines the relative path to the app's static assets.</span></span> <span data-ttu-id="8c25b-325">如果路徑不存在，則會使用無作業檔案提供者。</span><span class="sxs-lookup"><span data-stu-id="8c25b-325">If the path doesn't exist, a no-op file provider is used.</span></span>  

<span data-ttu-id="8c25b-326">機**碼**：`webroot`</span><span class="sxs-lookup"><span data-stu-id="8c25b-326">**Key**: `webroot`</span></span>  
<span data-ttu-id="8c25b-327">**類型**：`string`</span><span class="sxs-lookup"><span data-stu-id="8c25b-327">**Type**: `string`</span></span>  
<span data-ttu-id="8c25b-328">**預設**值：預設值為 `wwwroot` 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-328">**Default**: The default is `wwwroot`.</span></span> <span data-ttu-id="8c25b-329">*{Content root}/wwwroot*的路徑必須存在。</span><span class="sxs-lookup"><span data-stu-id="8c25b-329">The path to *{content root}/wwwroot* must exist.</span></span>  
<span data-ttu-id="8c25b-330">**環境變數**：`<PREFIX_>WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="8c25b-330">**Environment variable**: `<PREFIX_>WEBROOT`</span></span>

<span data-ttu-id="8c25b-331">若要設定此值，請使用環境變數或呼叫 `IWebHostBuilder` 上的 `UseWebRoot`：</span><span class="sxs-lookup"><span data-stu-id="8c25b-331">To set this value, use the environment variable or call `UseWebRoot` on `IWebHostBuilder`:</span></span>

```csharp
webBuilder.UseWebRoot("public");
```

<span data-ttu-id="8c25b-332">如需詳細資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="8c25b-332">For more information, see:</span></span>

* [<span data-ttu-id="8c25b-333">基本概念： Web 根目錄</span><span class="sxs-lookup"><span data-stu-id="8c25b-333">Fundamentals: Web root</span></span>](xref:fundamentals/index#web-root)
* [<span data-ttu-id="8c25b-334">ContentRoot</span><span class="sxs-lookup"><span data-stu-id="8c25b-334">ContentRoot</span></span>](#contentroot)

## <a name="manage-the-host-lifetime"></a><span data-ttu-id="8c25b-335">管理主機存留期</span><span class="sxs-lookup"><span data-stu-id="8c25b-335">Manage the host lifetime</span></span>

<span data-ttu-id="8c25b-336">在建置的 <xref:Microsoft.Extensions.Hosting.IHost> 實作上呼叫方法來啟動和停止應用程式。</span><span class="sxs-lookup"><span data-stu-id="8c25b-336">Call methods on the built <xref:Microsoft.Extensions.Hosting.IHost> implementation to start and stop the app.</span></span> <span data-ttu-id="8c25b-337">這些方法會影響所有在服務容器中註冊的 <xref:Microsoft.Extensions.Hosting.IHostedService> 實作。</span><span class="sxs-lookup"><span data-stu-id="8c25b-337">These methods affect all  <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="8c25b-338">執行</span><span class="sxs-lookup"><span data-stu-id="8c25b-338">Run</span></span>

<span data-ttu-id="8c25b-339"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> 會執行應用程式並封鎖呼叫執行緒，直到主機關閉為止。</span><span class="sxs-lookup"><span data-stu-id="8c25b-339"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down.</span></span>

### <a name="runasync"></a><span data-ttu-id="8c25b-340">RunAsync</span><span class="sxs-lookup"><span data-stu-id="8c25b-340">RunAsync</span></span>

<span data-ttu-id="8c25b-341"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> 會執行應用程式，並傳回觸發取消語彙基元或關機時所完成的 <xref:System.Threading.Tasks.Task>。</span><span class="sxs-lookup"><span data-stu-id="8c25b-341"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span>

### <a name="runconsoleasync"></a><span data-ttu-id="8c25b-342">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="8c25b-342">RunConsoleAsync</span></span>

<span data-ttu-id="8c25b-343"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*>啟用主控台支援、組建和啟動主機，以及等候<kbd>Ctrl</kbd> + <kbd>C</kbd>/SIGINT 或 SIGTERM 關閉。</span><span class="sxs-lookup"><span data-stu-id="8c25b-343"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM to shut down.</span></span>

### <a name="start"></a><span data-ttu-id="8c25b-344">開始</span><span class="sxs-lookup"><span data-stu-id="8c25b-344">Start</span></span>

<span data-ttu-id="8c25b-345"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> 會同步啟動主機。</span><span class="sxs-lookup"><span data-stu-id="8c25b-345"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

### <a name="startasync"></a><span data-ttu-id="8c25b-346">StartAsync</span><span class="sxs-lookup"><span data-stu-id="8c25b-346">StartAsync</span></span>

<span data-ttu-id="8c25b-347"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> 會啟動主機，並傳回觸發取消語彙基元或關機時所完成的 <xref:System.Threading.Tasks.Task>。</span><span class="sxs-lookup"><span data-stu-id="8c25b-347"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the host and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span> 

<span data-ttu-id="8c25b-348"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> 在 `StartAsync` 開始時呼叫，並等到完成後再繼續進行。</span><span class="sxs-lookup"><span data-stu-id="8c25b-348"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of `StartAsync`, which waits until it's complete before continuing.</span></span> <span data-ttu-id="8c25b-349">這可用來將啟動延遲到外部事件發出訊號為止。</span><span class="sxs-lookup"><span data-stu-id="8c25b-349">This can be used to delay startup until signaled by an external event.</span></span>

### <a name="stopasync"></a><span data-ttu-id="8c25b-350">StopAsync</span><span class="sxs-lookup"><span data-stu-id="8c25b-350">StopAsync</span></span>

<span data-ttu-id="8c25b-351"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> 會嘗試在提供的逾時內停止主機。</span><span class="sxs-lookup"><span data-stu-id="8c25b-351"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

### <a name="waitforshutdown"></a><span data-ttu-id="8c25b-352">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="8c25b-352">WaitForShutdown</span></span>

<span data-ttu-id="8c25b-353"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*>封鎖呼叫執行緒，直到 IHostLifetime 觸發關閉為止，例如經由<kbd>Ctrl</kbd> + <kbd>C</kbd>/SIGINT 或 SIGTERM。</span><span class="sxs-lookup"><span data-stu-id="8c25b-353"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blocks the calling thread until shutdown is triggered by the IHostLifetime, such as via <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM.</span></span>

### <a name="waitforshutdownasync"></a><span data-ttu-id="8c25b-354">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="8c25b-354">WaitForShutdownAsync</span></span>

<span data-ttu-id="8c25b-355"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> 會傳回透過指定語彙基元觸發關機時所完成的 <xref:System.Threading.Tasks.Task>，並呼叫 <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>。</span><span class="sxs-lookup"><span data-stu-id="8c25b-355"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

### <a name="external-control"></a><span data-ttu-id="8c25b-356">外部控制</span><span class="sxs-lookup"><span data-stu-id="8c25b-356">External control</span></span>

<span data-ttu-id="8c25b-357">主機存留期直接控制可以使用可從外部呼叫的方法來達成：</span><span class="sxs-lookup"><span data-stu-id="8c25b-357">Direct control of the host lifetime can be achieved using methods that can be called externally:</span></span>

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="8c25b-358">ASP.NET Core 應用程式會設定並啟動主機。</span><span class="sxs-lookup"><span data-stu-id="8c25b-358">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="8c25b-359">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="8c25b-359">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="8c25b-360">本文所涵蓋的 ASP.NET Core 泛型主機 (<xref:Microsoft.Extensions.Hosting.HostBuilder>)，可用來不會處理 HTTP 要求的應用程式。</span><span class="sxs-lookup"><span data-stu-id="8c25b-360">This article covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is used for apps that don't process HTTP requests.</span></span>

<span data-ttu-id="8c25b-361">泛型主機的用途是將 HTTP 管線從 Web 主機 API 分離，以允許更廣泛的主機陣列案例。</span><span class="sxs-lookup"><span data-stu-id="8c25b-361">The purpose of Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="8c25b-362">傳訊、背景工作及其他以泛型主機為基礎的非 HTTP 工作負載都受益於跨領域的功能，例如設定、相依性插入 (DI) 和記錄。</span><span class="sxs-lookup"><span data-stu-id="8c25b-362">Messaging, background tasks, and other non-HTTP workloads based on Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="8c25b-363">泛型主機是 ASP.NET Core 2.1 的新功能，不適用於虛擬主機案例。</span><span class="sxs-lookup"><span data-stu-id="8c25b-363">Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="8c25b-364">針對虛擬主機案例，請使用 [Web 主機](xref:fundamentals/host/web-host)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-364">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="8c25b-365">泛型主機將在未來版本中取代 Web 主機，並成為 HTTP 和非 HTTP 案例中的主要主機 API。</span><span class="sxs-lookup"><span data-stu-id="8c25b-365">Generic Host will replace Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="8c25b-366">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="8c25b-366">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8c25b-367">在 [Visual Studio Code](https://code.visualstudio.com/) 中執行範例應用程式時，請使用「外部或整合式終端機」\*\*。</span><span class="sxs-lookup"><span data-stu-id="8c25b-367">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="8c25b-368">請勿在 `internalConsole` 中執行範例。</span><span class="sxs-lookup"><span data-stu-id="8c25b-368">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="8c25b-369">若要在 Visual Studio Code 中設定主控台：</span><span class="sxs-lookup"><span data-stu-id="8c25b-369">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="8c25b-370">開啟 *.vscode/launch.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="8c25b-370">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="8c25b-371">在 [.NET Core Launch (console)] \(.NET Core 啟動 (主控台)\)\*\*\*\* 設定中，尋找 **console** 項目。</span><span class="sxs-lookup"><span data-stu-id="8c25b-371">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="8c25b-372">將此值設定為 `externalTerminal` 或 `integratedTerminal`。</span><span class="sxs-lookup"><span data-stu-id="8c25b-372">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="8c25b-373">簡介</span><span class="sxs-lookup"><span data-stu-id="8c25b-373">Introduction</span></span>

<span data-ttu-id="8c25b-374">可使用位於 <xref:Microsoft.Extensions.Hosting> 命名空間的泛型主機程式庫，且該程式庫由 [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) 套件提供。</span><span class="sxs-lookup"><span data-stu-id="8c25b-374">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="8c25b-375">[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 或更新版本) 包含 `Microsoft.Extensions.Hosting` 套件。</span><span class="sxs-lookup"><span data-stu-id="8c25b-375">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="8c25b-376"><xref:Microsoft.Extensions.Hosting.IHostedService> 是程式碼執行的進入點。</span><span class="sxs-lookup"><span data-stu-id="8c25b-376"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="8c25b-377">每個 <xref:Microsoft.Extensions.Hosting.IHostedService> 實作會依 [ConfigureServices 中的服務註冊](#configureservices)順序執行。</span><span class="sxs-lookup"><span data-stu-id="8c25b-377">Each <xref:Microsoft.Extensions.Hosting.IHostedService> implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="8c25b-378">當主機啟動時，會在每個 <xref:Microsoft.Extensions.Hosting.IHostedService> 上呼叫 <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*>；當主機順利關閉時，則會依反向註冊順序呼叫 <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*>。</span><span class="sxs-lookup"><span data-stu-id="8c25b-378"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each <xref:Microsoft.Extensions.Hosting.IHostedService> when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="8c25b-379">設定主機</span><span class="sxs-lookup"><span data-stu-id="8c25b-379">Set up a host</span></span>

<span data-ttu-id="8c25b-380"><xref:Microsoft.Extensions.Hosting.IHostBuilder> 是程式庫和應用程式用來初始化、建置及執行主機的主要元件：</span><span class="sxs-lookup"><span data-stu-id="8c25b-380"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a><span data-ttu-id="8c25b-381">選項</span><span class="sxs-lookup"><span data-stu-id="8c25b-381">Options</span></span>

<span data-ttu-id="8c25b-382"><xref:Microsoft.Extensions.Hosting.IHost> 的 <xref:Microsoft.Extensions.Hosting.HostOptions> 設定選項。</span><span class="sxs-lookup"><span data-stu-id="8c25b-382"><xref:Microsoft.Extensions.Hosting.HostOptions> configure options for the <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="8c25b-383">關機逾時</span><span class="sxs-lookup"><span data-stu-id="8c25b-383">Shutdown timeout</span></span>

<span data-ttu-id="8c25b-384"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> 會為 <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> 設定逾時。</span><span class="sxs-lookup"><span data-stu-id="8c25b-384"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="8c25b-385">預設值是五秒鐘。</span><span class="sxs-lookup"><span data-stu-id="8c25b-385">The default value is five seconds.</span></span>

<span data-ttu-id="8c25b-386">中的下列選項 `Program.Main` 設定會將預設的五秒關機超時時間增加為20秒：</span><span class="sxs-lookup"><span data-stu-id="8c25b-386">The following option configuration in `Program.Main` increases the default five-second shutdown timeout to 20 seconds:</span></span>

```csharp
var host = new HostBuilder()
    .ConfigureServices((hostContext, services) =>
    {
        services.Configure<HostOptions>(option =>
        {
            option.ShutdownTimeout = System.TimeSpan.FromSeconds(20);
        });
    })
    .Build();
```

## <a name="default-services"></a><span data-ttu-id="8c25b-387">預設服務</span><span class="sxs-lookup"><span data-stu-id="8c25b-387">Default services</span></span>

<span data-ttu-id="8c25b-388">下列服務會在主機初始化期間註冊：</span><span class="sxs-lookup"><span data-stu-id="8c25b-388">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="8c25b-389">[環境](xref:fundamentals/environments)（ <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> ）</span><span class="sxs-lookup"><span data-stu-id="8c25b-389">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="8c25b-390">[Configuration](xref:fundamentals/configuration/index)設定（ <xref:Microsoft.Extensions.Configuration.IConfiguration> ）</span><span class="sxs-lookup"><span data-stu-id="8c25b-390">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="8c25b-391"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (`Microsoft.Extensions.Hosting.Internal.ApplicationLifetime`)</span><span class="sxs-lookup"><span data-stu-id="8c25b-391"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (`Microsoft.Extensions.Hosting.Internal.ApplicationLifetime`)</span></span>
* <span data-ttu-id="8c25b-392"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime`)</span><span class="sxs-lookup"><span data-stu-id="8c25b-392"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime`)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="8c25b-393">[選項](xref:fundamentals/configuration/options)（ <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*> ）</span><span class="sxs-lookup"><span data-stu-id="8c25b-393">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="8c25b-394">[記錄](xref:fundamentals/logging/index)（ <xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*> ）</span><span class="sxs-lookup"><span data-stu-id="8c25b-394">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="8c25b-395">主機組態</span><span class="sxs-lookup"><span data-stu-id="8c25b-395">Host configuration</span></span>

<span data-ttu-id="8c25b-396">主機組態的建立方式：</span><span class="sxs-lookup"><span data-stu-id="8c25b-396">Host configuration is created by:</span></span>

* <span data-ttu-id="8c25b-397">呼叫 <xref:Microsoft.Extensions.Hosting.IHostBuilder> 上的擴充方法，來設定[內容根目錄](#content-root)及[環境](#environment)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-397">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="8c25b-398">在 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> 中從組態提供者讀取組態。</span><span class="sxs-lookup"><span data-stu-id="8c25b-398">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="8c25b-399">擴充方法</span><span class="sxs-lookup"><span data-stu-id="8c25b-399">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="8c25b-400">應用程式索引鍵 (名稱)</span><span class="sxs-lookup"><span data-stu-id="8c25b-400">Application key (name)</span></span>

<span data-ttu-id="8c25b-401">[IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) 屬性是在主機建構期間從主機設定當中設定。</span><span class="sxs-lookup"><span data-stu-id="8c25b-401">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="8c25b-402">若要明確設定該值，請使用 [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey)：</span><span class="sxs-lookup"><span data-stu-id="8c25b-402">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="8c25b-403">機**碼**：`applicationName`</span><span class="sxs-lookup"><span data-stu-id="8c25b-403">**Key**: `applicationName`</span></span>  
<span data-ttu-id="8c25b-404">**類型**：`string`</span><span class="sxs-lookup"><span data-stu-id="8c25b-404">**Type**: `string`</span></span>  
<span data-ttu-id="8c25b-405">**預設**：包含應用程式進入點的組件名稱。</span><span class="sxs-lookup"><span data-stu-id="8c25b-405">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="8c25b-406">**設定使用**：`HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="8c25b-406">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="8c25b-407">**環境變數**： `<PREFIX_>APPLICATIONNAME` （ `<PREFIX_>` 是[選擇性和使用者定義](#configurehostconfiguration)的）</span><span class="sxs-lookup"><span data-stu-id="8c25b-407">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="8c25b-408">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="8c25b-408">Content root</span></span>

<span data-ttu-id="8c25b-409">此設定可決定主機開始搜尋內容檔案的位置。</span><span class="sxs-lookup"><span data-stu-id="8c25b-409">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="8c25b-410">機**碼**：`contentRoot`</span><span class="sxs-lookup"><span data-stu-id="8c25b-410">**Key**: `contentRoot`</span></span>  
<span data-ttu-id="8c25b-411">**類型**：`string`</span><span class="sxs-lookup"><span data-stu-id="8c25b-411">**Type**: `string`</span></span>  
<span data-ttu-id="8c25b-412">**預設值**：預設為應用程式組件所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="8c25b-412">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="8c25b-413">**設定使用**：`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="8c25b-413">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="8c25b-414">**環境變數**： `<PREFIX_>CONTENTROOT` （ `<PREFIX_>` 是[選擇性和使用者定義](#configurehostconfiguration)的）</span><span class="sxs-lookup"><span data-stu-id="8c25b-414">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="8c25b-415">如果路徑不存在，就無法啟動主機。</span><span class="sxs-lookup"><span data-stu-id="8c25b-415">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

<span data-ttu-id="8c25b-416">如需詳細資訊，請參閱[基本概念：內容根目錄](xref:fundamentals/index#content-root)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-416">For more information, see [Fundamentals: Content root](xref:fundamentals/index#content-root).</span></span>

### <a name="environment"></a><span data-ttu-id="8c25b-417">環境</span><span class="sxs-lookup"><span data-stu-id="8c25b-417">Environment</span></span>

<span data-ttu-id="8c25b-418">設定應用程式的[環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-418">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="8c25b-419">機**碼**：`environment`</span><span class="sxs-lookup"><span data-stu-id="8c25b-419">**Key**: `environment`</span></span>  
<span data-ttu-id="8c25b-420">**類型**：`string`</span><span class="sxs-lookup"><span data-stu-id="8c25b-420">**Type**: `string`</span></span>  
<span data-ttu-id="8c25b-421">**預設**：`Production`</span><span class="sxs-lookup"><span data-stu-id="8c25b-421">**Default**: `Production`</span></span>  
<span data-ttu-id="8c25b-422">**設定使用**：`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="8c25b-422">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="8c25b-423">**環境變數**： `<PREFIX_>ENVIRONMENT` （ `<PREFIX_>` 是[選擇性和使用者定義](#configurehostconfiguration)的）</span><span class="sxs-lookup"><span data-stu-id="8c25b-423">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="8c25b-424">環境可以設定為任何值。</span><span class="sxs-lookup"><span data-stu-id="8c25b-424">The environment can be set to any value.</span></span> <span data-ttu-id="8c25b-425">架構定義的值包括 `Development`、`Staging` 和 `Production`。</span><span class="sxs-lookup"><span data-stu-id="8c25b-425">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="8c25b-426">值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="8c25b-426">Values aren't case-sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="8c25b-427">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="8c25b-427">ConfigureHostConfiguration</span></span>

<span data-ttu-id="8c25b-428"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> 會使用 <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> 來建立主機的 <xref:Microsoft.Extensions.Configuration.IConfiguration>。</span><span class="sxs-lookup"><span data-stu-id="8c25b-428"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="8c25b-429">主機組態會用來將 <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> 初始化，使其在應用程式建置流程中可供使用。</span><span class="sxs-lookup"><span data-stu-id="8c25b-429">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="8c25b-430"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="8c25b-430"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="8c25b-431">主機會使用指定索引鍵上最後設定值的任何選項。</span><span class="sxs-lookup"><span data-stu-id="8c25b-431">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="8c25b-432">預設不會包含任何提供者。</span><span class="sxs-lookup"><span data-stu-id="8c25b-432">No providers are included by default.</span></span> <span data-ttu-id="8c25b-433">您必須明確地指定應用程式在 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> 之中需要哪個組態提供者，包括：</span><span class="sxs-lookup"><span data-stu-id="8c25b-433">You must explicitly specify whatever configuration providers the app requires in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, including:</span></span>

* <span data-ttu-id="8c25b-434">檔案組態 (例如，來自 *hostsettings.json* 的檔案)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-434">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="8c25b-435">環境變數組態。</span><span class="sxs-lookup"><span data-stu-id="8c25b-435">Environment variable configuration.</span></span>
* <span data-ttu-id="8c25b-436">命令列引數組態。</span><span class="sxs-lookup"><span data-stu-id="8c25b-436">Command-line argument configuration.</span></span>
* <span data-ttu-id="8c25b-437">任何其他必要的組態提供者。</span><span class="sxs-lookup"><span data-stu-id="8c25b-437">Any other required configuration providers.</span></span>

<span data-ttu-id="8c25b-438">使用之後呼叫[檔案組態提供者](xref:fundamentals/configuration/index#file-configuration-provider)的 `SetBasePath`，並透過指定應用程式基底路徑，就可使用主機的檔案設定。</span><span class="sxs-lookup"><span data-stu-id="8c25b-438">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="8c25b-439">範例應用程式會使用 JSON 檔案，*hostsettings.json*，並呼叫 <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> 取用檔案的主機組態設定。</span><span class="sxs-lookup"><span data-stu-id="8c25b-439">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="8c25b-440">若要新增主機的[環境變數組態](xref:fundamentals/configuration/index#environment-variables-configuration-provider)，請在主機建立器上呼叫 <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*>。</span><span class="sxs-lookup"><span data-stu-id="8c25b-440">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="8c25b-441">`AddEnvironmentVariables` 可接受選擇性的使用者定義前置詞。</span><span class="sxs-lookup"><span data-stu-id="8c25b-441">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="8c25b-442">範例應用程式會使用前置詞 `PREFIX_`。</span><span class="sxs-lookup"><span data-stu-id="8c25b-442">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="8c25b-443">讀取環境變數時，就會移除前置詞。</span><span class="sxs-lookup"><span data-stu-id="8c25b-443">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="8c25b-444">在設定範例應用程式的主機時，`PREFIX_ENVIRONMENT` 的環境變數值會變成 `environment` 索引鍵的主機組態值。</span><span class="sxs-lookup"><span data-stu-id="8c25b-444">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="8c25b-445">在開發期間使用 [Visual Studio](https://visualstudio.microsoft.com) 或以 `dotnet run` 執行應用程式時，可能會在 *Properties/launchSettings.json* 檔案中設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="8c25b-445">During development when using [Visual Studio](https://visualstudio.microsoft.com) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="8c25b-446">在 [Visual Studio Code](https://code.visualstudio.com/) 中，可以在開發期間於 *.vscode/launch.json* 檔案中設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="8c25b-446">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="8c25b-447">如需詳細資訊，請參閱 <xref:fundamentals/environments> 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-447">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="8c25b-448">[命令列組態](xref:fundamentals/configuration/index#command-line-configuration-provider)可透過呼叫 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 新增。</span><span class="sxs-lookup"><span data-stu-id="8c25b-448">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="8c25b-449">命令列組態會在最後新增，以便命令列引數覆寫由先前組態提供者提供的組態。</span><span class="sxs-lookup"><span data-stu-id="8c25b-449">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="8c25b-450">*hostsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="8c25b-450">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="8c25b-451">您可以使用 [applicationName](#application-key-name) 和 [contentRoot](#content-root) 索引鍵來提供其他組態。</span><span class="sxs-lookup"><span data-stu-id="8c25b-451">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="8c25b-452">使用 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> 的 `HostBuilder` 組態範例：</span><span class="sxs-lookup"><span data-stu-id="8c25b-452">Example `HostBuilder` configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="8c25b-453">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="8c25b-453">ConfigureAppConfiguration</span></span>

<span data-ttu-id="8c25b-454">應用程式組態的建立方式是在 <xref:Microsoft.Extensions.Hosting.IHostBuilder> 實作上呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>。</span><span class="sxs-lookup"><span data-stu-id="8c25b-454">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="8c25b-455"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 會使用 <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> 來建立應用程式的 <xref:Microsoft.Extensions.Configuration.IConfiguration>。</span><span class="sxs-lookup"><span data-stu-id="8c25b-455"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="8c25b-456"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="8c25b-456"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="8c25b-457">應用程式會使用指定索引鍵上最後設定值的任何選項。</span><span class="sxs-lookup"><span data-stu-id="8c25b-457">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="8c25b-458">您可以從後續作業的 [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) 及 <xref:Microsoft.Extensions.Hosting.IHost.Services*> 中存取 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 所建立的組態。</span><span class="sxs-lookup"><span data-stu-id="8c25b-458">The configuration created by <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="8c25b-459">應用程式組態會自動接收由 [ConfigureHostConfiguration](#configurehostconfiguration) 提供的主機組態。</span><span class="sxs-lookup"><span data-stu-id="8c25b-459">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="8c25b-460">使用 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 的應用程式組態範例：</span><span class="sxs-lookup"><span data-stu-id="8c25b-460">Example app configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="8c25b-461">*appsettings.js*：</span><span class="sxs-lookup"><span data-stu-id="8c25b-461">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="8c25b-462">*appsettings.Development.json*：</span><span class="sxs-lookup"><span data-stu-id="8c25b-462">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="8c25b-463">*appsettings.Production.json*：</span><span class="sxs-lookup"><span data-stu-id="8c25b-463">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="8c25b-464">若要將設定檔移至輸出目錄，請在專案檔中將設定檔指定為 [MSBuild 專案項目](/visualstudio/msbuild/common-msbuild-project-items)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-464">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="8c25b-465">範例應用程式使用下列 `<Content>` 項目，移動其 JSON 應用程式設定檔和 *hostsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="8c25b-465">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

> [!NOTE]
> <span data-ttu-id="8c25b-466">組態擴充方法 (例如 <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> 和 <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*>) 需要其他的 NuGet 套件，例如 [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) \(英文\) 和[Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-466">Configuration extension methods, such as <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> and <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> require additional NuGet packages, such as [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) and [Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span></span> <span data-ttu-id="8c25b-467">除非應用程式使用 [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app)，否則，除了核心 [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) \(英文\) 套件，還必須將這些套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="8c25b-467">Unless the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), these packages must be added to the project in addition to the core [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) package.</span></span> <span data-ttu-id="8c25b-468">如需詳細資訊，請參閱 <xref:fundamentals/configuration/index> 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-468">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="configureservices"></a><span data-ttu-id="8c25b-469">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="8c25b-469">ConfigureServices</span></span>

<span data-ttu-id="8c25b-470"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> 會將服務新增至應用程式的[相依性插入](xref:fundamentals/dependency-injection)容器。</span><span class="sxs-lookup"><span data-stu-id="8c25b-470"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="8c25b-471"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="8c25b-471"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="8c25b-472">託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="8c25b-472">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="8c25b-473">如需詳細資訊，請參閱 <xref:fundamentals/host/hosted-services> 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-473">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="8c25b-474">[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)使用 `AddHostedService` 擴充方法，將存留期事件 `LifetimeEventsHostedService` 和計時背景工作 `TimedHostedService` 等服務新增至應用程式：</span><span class="sxs-lookup"><span data-stu-id="8c25b-474">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="8c25b-475">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="8c25b-475">ConfigureLogging</span></span>

<span data-ttu-id="8c25b-476"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> 會新增委派以設定提供的 <xref:Microsoft.Extensions.Logging.ILoggingBuilder>。</span><span class="sxs-lookup"><span data-stu-id="8c25b-476"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="8c25b-477"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="8c25b-477"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="8c25b-478">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="8c25b-478">UseConsoleLifetime</span></span>

<span data-ttu-id="8c25b-479"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*>接聽<kbd>Ctrl</kbd> + <kbd>C</kbd>/SIGINT 或 SIGTERM，並呼叫 <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> 來啟動關機程式。</span><span class="sxs-lookup"><span data-stu-id="8c25b-479"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="8c25b-480"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*>解除封鎖延伸模組，例如[RunAsync](#runasync)和[WaitForShutdownAsync](#waitforshutdownasync)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-480"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="8c25b-481">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` 會預先註冊為預設存留期實作。</span><span class="sxs-lookup"><span data-stu-id="8c25b-481">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="8c25b-482">系統會使用最後一個註冊的存留期。</span><span class="sxs-lookup"><span data-stu-id="8c25b-482">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="8c25b-483">容器設定</span><span class="sxs-lookup"><span data-stu-id="8c25b-483">Container configuration</span></span>

<span data-ttu-id="8c25b-484">為支援插入其他容器，主機可以接受 <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>。</span><span class="sxs-lookup"><span data-stu-id="8c25b-484">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span></span> <span data-ttu-id="8c25b-485">提供處理站不是 DI 容器註冊的一部分，而是用來建立實體 DI 容器的主機內建功能。</span><span class="sxs-lookup"><span data-stu-id="8c25b-485">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="8c25b-486">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) 會覆寫用來建立應用程式服務提供者的預設處理站。</span><span class="sxs-lookup"><span data-stu-id="8c25b-486">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="8c25b-487">自訂容器組態由 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> 方法管理。</span><span class="sxs-lookup"><span data-stu-id="8c25b-487">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="8c25b-488"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> 提供在基礎主機 API 上設定容器的強型別體驗。</span><span class="sxs-lookup"><span data-stu-id="8c25b-488"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="8c25b-489"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="8c25b-489"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="8c25b-490">建立應用程式的服務容器：</span><span class="sxs-lookup"><span data-stu-id="8c25b-490">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="8c25b-491">提供服務容器處理站：</span><span class="sxs-lookup"><span data-stu-id="8c25b-491">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="8c25b-492">使用處理站，並設定應用程式的自訂服務容器：</span><span class="sxs-lookup"><span data-stu-id="8c25b-492">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="8c25b-493">擴充性</span><span class="sxs-lookup"><span data-stu-id="8c25b-493">Extensibility</span></span>

<span data-ttu-id="8c25b-494">主機擴充性是透過 <xref:Microsoft.Extensions.Hosting.IHostBuilder> 上的擴充方法執行。</span><span class="sxs-lookup"><span data-stu-id="8c25b-494">Host extensibility is performed with extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span></span> <span data-ttu-id="8c25b-495">下列範例使用 <xref:fundamentals/host/hosted-services> 中示範的 [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) 範例顯示擴充方法如何擴充 <xref:Microsoft.Extensions.Hosting.IHostBuilder> 實作。</span><span class="sxs-lookup"><span data-stu-id="8c25b-495">The following example shows how an extension method extends an <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="8c25b-496">應用程式會建立 `UseHostedService` 擴充方法以註冊在 `T` 中傳遞的裝載服務：</span><span class="sxs-lookup"><span data-stu-id="8c25b-496">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

```csharp
using System;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

public static class Extensions
{
    public static IHostBuilder UseHostedService<T>(this IHostBuilder hostBuilder)
        where T : class, IHostedService, IDisposable
    {
        return hostBuilder.ConfigureServices(services =>
            services.AddHostedService<T>());
    }
}
```

## <a name="manage-the-host"></a><span data-ttu-id="8c25b-497">管理主機</span><span class="sxs-lookup"><span data-stu-id="8c25b-497">Manage the host</span></span>

<span data-ttu-id="8c25b-498"><xref:Microsoft.Extensions.Hosting.IHost> 實作負責啟動及停止服務容器中已註冊的 <xref:Microsoft.Extensions.Hosting.IHostedService> 實作。</span><span class="sxs-lookup"><span data-stu-id="8c25b-498">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="8c25b-499">執行</span><span class="sxs-lookup"><span data-stu-id="8c25b-499">Run</span></span>

<span data-ttu-id="8c25b-500"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> 會執行應用程式並封鎖呼叫執行緒，直到主機關閉為止：</span><span class="sxs-lookup"><span data-stu-id="8c25b-500"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        host.Run();
    }
}
```

### <a name="runasync"></a><span data-ttu-id="8c25b-501">RunAsync</span><span class="sxs-lookup"><span data-stu-id="8c25b-501">RunAsync</span></span>

<span data-ttu-id="8c25b-502"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> 會執行應用程式，並傳回觸發取消語彙基元或關機時所完成的 <xref:System.Threading.Tasks.Task>：</span><span class="sxs-lookup"><span data-stu-id="8c25b-502"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        await host.RunAsync();
    }
}
```

### <a name="runconsoleasync"></a><span data-ttu-id="8c25b-503">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="8c25b-503">RunConsoleAsync</span></span>

<span data-ttu-id="8c25b-504"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*>啟用主控台支援、組建和啟動主機，以及等候<kbd>Ctrl</kbd> + <kbd>C</kbd>/SIGINT 或 SIGTERM 關閉。</span><span class="sxs-lookup"><span data-stu-id="8c25b-504"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM to shut down.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var hostBuilder = new HostBuilder();

        await hostBuilder.RunConsoleAsync();
    }
}
```

### <a name="start-and-stopasync"></a><span data-ttu-id="8c25b-505">Start 和 StopAsync</span><span class="sxs-lookup"><span data-stu-id="8c25b-505">Start and StopAsync</span></span>

<span data-ttu-id="8c25b-506"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> 會同步啟動主機。</span><span class="sxs-lookup"><span data-stu-id="8c25b-506"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="8c25b-507"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> 會嘗試在提供的逾時內停止主機。</span><span class="sxs-lookup"><span data-stu-id="8c25b-507"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            await host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

### <a name="startasync-and-stopasync"></a><span data-ttu-id="8c25b-508">StartAsync 和 StopAsync</span><span class="sxs-lookup"><span data-stu-id="8c25b-508">StartAsync and StopAsync</span></span>

<span data-ttu-id="8c25b-509"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> 會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="8c25b-509"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="8c25b-510"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> 會停止應用程式。</span><span class="sxs-lookup"><span data-stu-id="8c25b-510"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.StopAsync();
        }
    }
}
```

### <a name="waitforshutdown"></a><span data-ttu-id="8c25b-511">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="8c25b-511">WaitForShutdown</span></span>

<span data-ttu-id="8c25b-512"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*>會透過觸發 <xref:Microsoft.Extensions.Hosting.IHostLifetime> ，例如 `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` （接聽<kbd>Ctrl</kbd> + <kbd>C</kbd>/SIGINT 或 SIGTERM）。</span><span class="sxs-lookup"><span data-stu-id="8c25b-512"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` (listens for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM).</span></span> <span data-ttu-id="8c25b-513"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> 會呼叫 <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>。</span><span class="sxs-lookup"><span data-stu-id="8c25b-513"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            host.WaitForShutdown();
        }
    }
}
```

### <a name="waitforshutdownasync"></a><span data-ttu-id="8c25b-514">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="8c25b-514">WaitForShutdownAsync</span></span>

<span data-ttu-id="8c25b-515"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> 會傳回透過指定語彙基元觸發關機時所完成的 <xref:System.Threading.Tasks.Task>，並呼叫 <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>。</span><span class="sxs-lookup"><span data-stu-id="8c25b-515"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.WaitForShutdownAsync();
        }

    }
}
```

### <a name="external-control"></a><span data-ttu-id="8c25b-516">外部控制</span><span class="sxs-lookup"><span data-stu-id="8c25b-516">External control</span></span>

<span data-ttu-id="8c25b-517">主機的外部控制可以使用可從外部呼叫的方法來達成：</span><span class="sxs-lookup"><span data-stu-id="8c25b-517">External control of the host can be achieved using methods that can be called externally:</span></span>

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

<span data-ttu-id="8c25b-518"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> 在 <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> 開始時呼叫，並等到完成後再繼續進行。</span><span class="sxs-lookup"><span data-stu-id="8c25b-518"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="8c25b-519">這可用來將啟動延遲到外部事件發出訊號為止。</span><span class="sxs-lookup"><span data-stu-id="8c25b-519">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="8c25b-520">IHostingEnvironment 介面</span><span class="sxs-lookup"><span data-stu-id="8c25b-520">IHostingEnvironment interface</span></span>

<span data-ttu-id="8c25b-521"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> 提供應用程式主控環境的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="8c25b-521"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="8c25b-522">使用[建構函式插入](xref:fundamentals/dependency-injection)以取得 <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>，才能使用其屬性和擴充方法：</span><span class="sxs-lookup"><span data-stu-id="8c25b-522">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> in order to use its properties and extension methods:</span></span>

```csharp
public class MyClass
{
    private readonly IHostingEnvironment _env;

    public MyClass(IHostingEnvironment env)
    {
        _env = env;
    }

    public void DoSomething()
    {
        var environmentName = _env.EnvironmentName;
    }
}
```

<span data-ttu-id="8c25b-523">如需詳細資訊，請參閱 <xref:fundamentals/environments> 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-523">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="8c25b-524">IApplicationLifetime 介面</span><span class="sxs-lookup"><span data-stu-id="8c25b-524">IApplicationLifetime interface</span></span>

<span data-ttu-id="8c25b-525"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> 允許啟動後和關機活動，包括順利關機要求。</span><span class="sxs-lookup"><span data-stu-id="8c25b-525"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="8c25b-526">在介面上的三個屬性是用來註冊定義啟動和關閉事件之 <xref:System.Action> 方法的取消語彙基元。</span><span class="sxs-lookup"><span data-stu-id="8c25b-526">Three properties on the interface are cancellation tokens used to register <xref:System.Action> methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="8c25b-527">取消權杖</span><span class="sxs-lookup"><span data-stu-id="8c25b-527">Cancellation Token</span></span> | <span data-ttu-id="8c25b-528">觸發時機&#8230;</span><span class="sxs-lookup"><span data-stu-id="8c25b-528">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="8c25b-529">已完全啟動主機。</span><span class="sxs-lookup"><span data-stu-id="8c25b-529">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="8c25b-530">主機正在完成正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="8c25b-530">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="8c25b-531">應該處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="8c25b-531">All requests should be processed.</span></span> <span data-ttu-id="8c25b-532">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="8c25b-532">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="8c25b-533">主機正在執行正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="8c25b-533">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="8c25b-534">可能仍在處理要求。</span><span class="sxs-lookup"><span data-stu-id="8c25b-534">Requests may still be processing.</span></span> <span data-ttu-id="8c25b-535">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="8c25b-535">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="8c25b-536">建構函式將 <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> 服務插入任何類別。</span><span class="sxs-lookup"><span data-stu-id="8c25b-536">Constructor-inject the <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> service into any class.</span></span> <span data-ttu-id="8c25b-537">[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)使用建構函式插入 `LifetimeEventsHostedService` 類別 (<xref:Microsoft.Extensions.Hosting.IHostedService> 實作) 來註冊事件。</span><span class="sxs-lookup"><span data-stu-id="8c25b-537">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an <xref:Microsoft.Extensions.Hosting.IHostedService> implementation) to register the events.</span></span>

<span data-ttu-id="8c25b-538">*LifetimeEventsHostedService.cs*：</span><span class="sxs-lookup"><span data-stu-id="8c25b-538">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="8c25b-539"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> 會要求應用程式終止。</span><span class="sxs-lookup"><span data-stu-id="8c25b-539"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="8c25b-540">當呼叫類別的 `Shutdown` 方法時，下列類別使用 <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> 來順利關閉應用程式：</span><span class="sxs-lookup"><span data-stu-id="8c25b-540">The following class uses <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

```csharp
public class MyClass
{
    private readonly IApplicationLifetime _appLifetime;

    public MyClass(IApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

::: moniker-end

::: moniker range=">= aspnetcore-5.0"

<span data-ttu-id="8c25b-541">ASP.NET Core 範本會建立 .NET Core 泛型主機（ <xref:Microsoft.Extensions.Hosting.HostBuilder> ）。</span><span class="sxs-lookup"><span data-stu-id="8c25b-541">The ASP.NET Core templates create a .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>).</span></span>

## <a name="host-definition"></a><span data-ttu-id="8c25b-542">主機定義</span><span class="sxs-lookup"><span data-stu-id="8c25b-542">Host definition</span></span>

<span data-ttu-id="8c25b-543">「主機」\*\* 是封裝所有應用程式資源的物件，例如：</span><span class="sxs-lookup"><span data-stu-id="8c25b-543">A *host* is an object that encapsulates an app's resources, such as:</span></span>

* <span data-ttu-id="8c25b-544">相依性插入 (DI)</span><span class="sxs-lookup"><span data-stu-id="8c25b-544">Dependency injection (DI)</span></span>
* <span data-ttu-id="8c25b-545">記錄</span><span class="sxs-lookup"><span data-stu-id="8c25b-545">Logging</span></span>
* <span data-ttu-id="8c25b-546">組態</span><span class="sxs-lookup"><span data-stu-id="8c25b-546">Configuration</span></span>
* <span data-ttu-id="8c25b-547">`IHostedService` 實作</span><span class="sxs-lookup"><span data-stu-id="8c25b-547">`IHostedService` implementations</span></span>

<span data-ttu-id="8c25b-548">當主機啟動時，它會 <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync%2A?displayProperty=nameWithType> 在 <xref:Microsoft.Extensions.Hosting.IHostedService> 服務容器的託管服務集合中註冊的每個執行上呼叫。</span><span class="sxs-lookup"><span data-stu-id="8c25b-548">When a host starts, it calls <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync%2A?displayProperty=nameWithType> on each implementation of <xref:Microsoft.Extensions.Hosting.IHostedService> registered in the service container's collection of hosted services.</span></span> <span data-ttu-id="8c25b-549">在 Web 應用程式中，其中一個 `IHostedService` 實作是一種 Web 服務，負責啟動 [HTTP 伺服器實作](xref:fundamentals/index#servers)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-549">In a web app, one of the `IHostedService` implementations is a web service that starts an [HTTP server implementation](xref:fundamentals/index#servers).</span></span>

<span data-ttu-id="8c25b-550">在單一物件中包含所有應用程式相互依存資源的主要理由便是生命週期管理：控制應用程式的啟動及順利關機。</span><span class="sxs-lookup"><span data-stu-id="8c25b-550">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="8c25b-551">設定主機</span><span class="sxs-lookup"><span data-stu-id="8c25b-551">Set up a host</span></span>

<span data-ttu-id="8c25b-552">主機通常由 `Program` 類別的程式碼來設定、建置並執行。</span><span class="sxs-lookup"><span data-stu-id="8c25b-552">The host is typically configured, built, and run by code in the `Program` class.</span></span> <span data-ttu-id="8c25b-553">`Main` 方法：</span><span class="sxs-lookup"><span data-stu-id="8c25b-553">The `Main` method:</span></span>

* <span data-ttu-id="8c25b-554">呼叫 `CreateHostBuilder` 方法來建立及設定建立器物件。</span><span class="sxs-lookup"><span data-stu-id="8c25b-554">Calls a `CreateHostBuilder` method to create and configure a builder object.</span></span>
* <span data-ttu-id="8c25b-555">在建立器物件上呼叫 `Build` 和 `Run` 方法。</span><span class="sxs-lookup"><span data-stu-id="8c25b-555">Calls `Build` and `Run` methods on the builder object.</span></span>

<span data-ttu-id="8c25b-556">ASP.NET Core web 範本會產生下列程式碼來建立主機：</span><span class="sxs-lookup"><span data-stu-id="8c25b-556">The ASP.NET Core web templates generate the following code to create a host:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStartup<Startup>();
            });
}
```

<span data-ttu-id="8c25b-557">下列程式碼會使用 `IHostedService` 已新增至 DI 容器的執行，來建立非 HTTP 工作負載。</span><span class="sxs-lookup"><span data-stu-id="8c25b-557">The following code creates a non-HTTP workload with a `IHostedService` implementation added to the DI container.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureServices((hostContext, services) =>
            {
               services.AddHostedService<Worker>();
            });
}
```

<span data-ttu-id="8c25b-558">針對 HTTP 工作負載，`Main` 方法相同，但 `CreateHostBuilder` 會呼叫 `ConfigureWebHostDefaults`：</span><span class="sxs-lookup"><span data-stu-id="8c25b-558">For an HTTP workload, the `Main` method is the same but `CreateHostBuilder` calls `ConfigureWebHostDefaults`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="8c25b-559">如果應用程式使用 Entity Framework Core，請勿變更 `CreateHostBuilder` 方法的名稱或簽章。</span><span class="sxs-lookup"><span data-stu-id="8c25b-559">If the app uses Entity Framework Core, don't change the name or signature of the `CreateHostBuilder` method.</span></span> <span data-ttu-id="8c25b-560">[Entity Framework Core 工具](/ef/core/miscellaneous/cli/)預期找到 `CreateHostBuilder` 方法，其在不執行應用程式的情況下設定主機。</span><span class="sxs-lookup"><span data-stu-id="8c25b-560">The [Entity Framework Core tools](/ef/core/miscellaneous/cli/) expect to find a `CreateHostBuilder` method that configures the host without running the app.</span></span> <span data-ttu-id="8c25b-561">如需詳細資訊，請參閱[設計階段 DbContext 建立](/ef/core/miscellaneous/cli/dbcontext-creation)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-561">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

## <a name="default-builder-settings"></a><span data-ttu-id="8c25b-562">預設建立器設定</span><span class="sxs-lookup"><span data-stu-id="8c25b-562">Default builder settings</span></span>

<span data-ttu-id="8c25b-563"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> 方法：</span><span class="sxs-lookup"><span data-stu-id="8c25b-563">The <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> method:</span></span>

* <span data-ttu-id="8c25b-564">將[內容根目錄](xref:fundamentals/index#content-root)設定為所傳回的路徑 <xref:System.IO.Directory.GetCurrentDirectory*> 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-564">Sets the [content root](xref:fundamentals/index#content-root) to the path returned by <xref:System.IO.Directory.GetCurrentDirectory*>.</span></span>
* <span data-ttu-id="8c25b-565">從下列項目載入主機組態：</span><span class="sxs-lookup"><span data-stu-id="8c25b-565">Loads host configuration from:</span></span>
  * <span data-ttu-id="8c25b-566">前面加上的環境變數 `DOTNET_` 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-566">Environment variables prefixed with `DOTNET_`.</span></span>
  * <span data-ttu-id="8c25b-567">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="8c25b-567">Command-line arguments.</span></span>
* <span data-ttu-id="8c25b-568">從下列項目載入應用程式組態：</span><span class="sxs-lookup"><span data-stu-id="8c25b-568">Loads app configuration from:</span></span>
  * <span data-ttu-id="8c25b-569">*appsettings.js*。</span><span class="sxs-lookup"><span data-stu-id="8c25b-569">*appsettings.json*.</span></span>
  * <span data-ttu-id="8c25b-570">*appsettings.{Environment}.json*</span><span class="sxs-lookup"><span data-stu-id="8c25b-570">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="8c25b-571">應用程式在 `Development` 環境中執行時的[祕密管理員](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-571">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="8c25b-572">環境變數。</span><span class="sxs-lookup"><span data-stu-id="8c25b-572">Environment variables.</span></span>
  * <span data-ttu-id="8c25b-573">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="8c25b-573">Command-line arguments.</span></span>
* <span data-ttu-id="8c25b-574">新增下列[記錄](xref:fundamentals/logging/index)提供者：</span><span class="sxs-lookup"><span data-stu-id="8c25b-574">Adds the following [logging](xref:fundamentals/logging/index) providers:</span></span>
  * <span data-ttu-id="8c25b-575">主控台</span><span class="sxs-lookup"><span data-stu-id="8c25b-575">Console</span></span>
  * <span data-ttu-id="8c25b-576">偵錯</span><span class="sxs-lookup"><span data-stu-id="8c25b-576">Debug</span></span>
  * <span data-ttu-id="8c25b-577">EventSource</span><span class="sxs-lookup"><span data-stu-id="8c25b-577">EventSource</span></span>
  * <span data-ttu-id="8c25b-578">EventLog (僅當在 Windows 上執行時)</span><span class="sxs-lookup"><span data-stu-id="8c25b-578">EventLog (only when running on Windows)</span></span>
* <span data-ttu-id="8c25b-579">環境為開發時，會啟用[範圍驗證](xref:fundamentals/dependency-injection#scope-validation)和[相依性驗證](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-579">Enables [scope validation](xref:fundamentals/dependency-injection#scope-validation) and [dependency validation](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) when the environment is Development.</span></span>

<span data-ttu-id="8c25b-580">`ConfigureWebHostDefaults` 方法：</span><span class="sxs-lookup"><span data-stu-id="8c25b-580">The `ConfigureWebHostDefaults` method:</span></span>

* <span data-ttu-id="8c25b-581">從前面加上的環境變數載入主機設定 `ASPNETCORE_` 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-581">Loads host configuration from environment variables prefixed with `ASPNETCORE_`.</span></span>
* <span data-ttu-id="8c25b-582">會將 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器設為網頁伺服器，並使用應用程式的主機組態提供者進行設定。</span><span class="sxs-lookup"><span data-stu-id="8c25b-582">Sets [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and configures it using the app's hosting configuration providers.</span></span> <span data-ttu-id="8c25b-583">如需 Kestrel 伺服器的預設選項，請參閱 <xref:fundamentals/servers/kestrel#kestrel-options>。</span><span class="sxs-lookup"><span data-stu-id="8c25b-583">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="8c25b-584">新增[主機篩選中介軟體](xref:fundamentals/servers/kestrel#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-584">Adds [Host Filtering middleware](xref:fundamentals/servers/kestrel#host-filtering).</span></span>
* <span data-ttu-id="8c25b-585">如果等於，則新增[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) `ASPNETCORE_FORWARDEDHEADERS_ENABLED` `true` 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-585">Adds [Forwarded Headers middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) if `ASPNETCORE_FORWARDEDHEADERS_ENABLED` equals `true`.</span></span>
* <span data-ttu-id="8c25b-586">啟用 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="8c25b-586">Enables IIS integration.</span></span> <span data-ttu-id="8c25b-587">如需 IIS 預設選項，請參閱 <xref:host-and-deploy/iis/index#iis-options>。</span><span class="sxs-lookup"><span data-stu-id="8c25b-587">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>

<span data-ttu-id="8c25b-588">本文稍後的[＜設定所有應用程式類型＞](#settings-for-all-app-types)和[＜Web 應用程式設定＞](#settings-for-web-apps)章節，將說明如何覆寫預設的建立器設定。</span><span class="sxs-lookup"><span data-stu-id="8c25b-588">The [Settings for all app types](#settings-for-all-app-types) and [Settings for web apps](#settings-for-web-apps) sections later in this article show how to override default builder settings.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="8c25b-589">架構提供的服務</span><span class="sxs-lookup"><span data-stu-id="8c25b-589">Framework-provided services</span></span>

<span data-ttu-id="8c25b-590">系統會自動註冊下列服務：</span><span class="sxs-lookup"><span data-stu-id="8c25b-590">The following services are registered automatically:</span></span>

* [<span data-ttu-id="8c25b-591">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="8c25b-591">IHostApplicationLifetime</span></span>](#ihostapplicationlifetime)
* [<span data-ttu-id="8c25b-592">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="8c25b-592">IHostLifetime</span></span>](#ihostlifetime)
* [<span data-ttu-id="8c25b-593">IHostEnvironment / IWebHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="8c25b-593">IHostEnvironment / IWebHostEnvironment</span></span>](#ihostenvironment)

<span data-ttu-id="8c25b-594">如需架構所提供之服務的詳細資訊，請參閱 <xref:fundamentals/dependency-injection#framework-provided-services> 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-594">For more information on framework-provided services, see <xref:fundamentals/dependency-injection#framework-provided-services>.</span></span>

## <a name="ihostapplicationlifetime"></a><span data-ttu-id="8c25b-595">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="8c25b-595">IHostApplicationLifetime</span></span>

<span data-ttu-id="8c25b-596">將 <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (先前稱為 `IApplicationLifetime`) 服務插入任何類別來處理啟動後和順利關機工作。</span><span class="sxs-lookup"><span data-stu-id="8c25b-596">Inject the <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (formerly `IApplicationLifetime`) service into any class to handle post-startup and graceful shutdown tasks.</span></span> <span data-ttu-id="8c25b-597">介面上的三個屬性是用於註冊應用程式啟動和應用程式關閉事件處理程序方法的取消語彙基元。</span><span class="sxs-lookup"><span data-stu-id="8c25b-597">Three properties on the interface are cancellation tokens used to register app start and app stop event handler methods.</span></span> <span data-ttu-id="8c25b-598">介面也包括 `StopApplication` 方法。</span><span class="sxs-lookup"><span data-stu-id="8c25b-598">The interface also includes a `StopApplication` method.</span></span>

<span data-ttu-id="8c25b-599">下列範例是 `IHostedService` 註冊事件的實作為 `IHostApplicationLifetime` ：</span><span class="sxs-lookup"><span data-stu-id="8c25b-599">The following example is an `IHostedService` implementation that registers `IHostApplicationLifetime` events:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a><span data-ttu-id="8c25b-600">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="8c25b-600">IHostLifetime</span></span>

<span data-ttu-id="8c25b-601"><xref:Microsoft.Extensions.Hosting.IHostLifetime> 實作會控制主機啟動及停止的時機。</span><span class="sxs-lookup"><span data-stu-id="8c25b-601">The <xref:Microsoft.Extensions.Hosting.IHostLifetime> implementation controls when the host starts and when it stops.</span></span> <span data-ttu-id="8c25b-602">會使用最後一個註冊的實作。</span><span class="sxs-lookup"><span data-stu-id="8c25b-602">The last implementation registered is used.</span></span>

<span data-ttu-id="8c25b-603">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` 是預設的 `IHostLifetime` 實作。</span><span class="sxs-lookup"><span data-stu-id="8c25b-603">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is the default `IHostLifetime` implementation.</span></span> <span data-ttu-id="8c25b-604">`ConsoleLifetime`:</span><span class="sxs-lookup"><span data-stu-id="8c25b-604">`ConsoleLifetime`:</span></span>

* <span data-ttu-id="8c25b-605">接聽<kbd>Ctrl</kbd> + <kbd>C</kbd>/SIGINT 或 SIGTERM，並呼叫 <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*> 來啟動關機程式。</span><span class="sxs-lookup"><span data-stu-id="8c25b-605">Listens for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*> to start the shutdown process.</span></span>
* <span data-ttu-id="8c25b-606">會解除封鎖 [RunAsync](#runasync) 和 [WaitForShutdownAsync](#waitforshutdownasync) 等延伸模組。</span><span class="sxs-lookup"><span data-stu-id="8c25b-606">Unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span>

## <a name="ihostenvironment"></a><span data-ttu-id="8c25b-607">IHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="8c25b-607">IHostEnvironment</span></span>

<span data-ttu-id="8c25b-608">將服務插入至 <xref:Microsoft.Extensions.Hosting.IHostEnvironment> 類別，以取得下列設定的相關資訊：</span><span class="sxs-lookup"><span data-stu-id="8c25b-608">Inject the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> service into a class to get information about the following settings:</span></span>

* [<span data-ttu-id="8c25b-609">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="8c25b-609">ApplicationName</span></span>](#applicationname)
* [<span data-ttu-id="8c25b-610">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="8c25b-610">EnvironmentName</span></span>](#environmentname)
* [<span data-ttu-id="8c25b-611">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="8c25b-611">ContentRootPath</span></span>](#contentroot)

<span data-ttu-id="8c25b-612">Web apps 會執行 `IWebHostEnvironment` 介面，它會繼承 `IHostEnvironment` 並新增[WebRootPath](#webroot)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-612">Web apps implement the `IWebHostEnvironment` interface, which inherits `IHostEnvironment` and adds the [WebRootPath](#webroot).</span></span>

## <a name="host-configuration"></a><span data-ttu-id="8c25b-613">主機組態</span><span class="sxs-lookup"><span data-stu-id="8c25b-613">Host configuration</span></span>

<span data-ttu-id="8c25b-614">主機組態用於 <xref:Microsoft.Extensions.Hosting.IHostEnvironment> 實作的屬性。</span><span class="sxs-lookup"><span data-stu-id="8c25b-614">Host configuration is used for the properties of the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementation.</span></span>

<span data-ttu-id="8c25b-615">主機組態位於 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 內的 [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-615">Host configuration is available from [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) inside <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span></span> <span data-ttu-id="8c25b-616">在 `ConfigureAppConfiguration` 之後，應用程式組態會取代 `HostBuilderContext.Configuration`。</span><span class="sxs-lookup"><span data-stu-id="8c25b-616">After `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` is replaced with the app config.</span></span>

<span data-ttu-id="8c25b-617">若要新增主機組態，請呼叫 `IHostBuilder` 上的 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>。</span><span class="sxs-lookup"><span data-stu-id="8c25b-617">To add host configuration, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="8c25b-618">`ConfigureHostConfiguration` 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="8c25b-618">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="8c25b-619">主機會使用指定索引鍵上最後設定值的任何選項。</span><span class="sxs-lookup"><span data-stu-id="8c25b-619">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="8c25b-620">具有前置詞 `DOTNET_` 和命令列引數的環境變數提供者是由所包含 `CreateDefaultBuilder` 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-620">The environment variable provider with prefix `DOTNET_` and command-line arguments are included by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="8c25b-621">針對 Web 應用程式，會新增具有前置詞 `ASPNETCORE_` 的環境變數提供者。</span><span class="sxs-lookup"><span data-stu-id="8c25b-621">For web apps, the environment variable provider with prefix `ASPNETCORE_` is added.</span></span> <span data-ttu-id="8c25b-622">讀取環境變數時，就會移除前置詞。</span><span class="sxs-lookup"><span data-stu-id="8c25b-622">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="8c25b-623">例如，`ASPNETCORE_ENVIRONMENT` 的環境變數值會變成 `environment` 索引鍵的主機組態值。</span><span class="sxs-lookup"><span data-stu-id="8c25b-623">For example, the environment variable value for `ASPNETCORE_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="8c25b-624">下列範例會建立主機組態：</span><span class="sxs-lookup"><span data-stu-id="8c25b-624">The following example creates host configuration:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a><span data-ttu-id="8c25b-625">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="8c25b-625">App configuration</span></span>

<span data-ttu-id="8c25b-626">應用程式組態的建立方式是在 `IHostBuilder` 上呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>。</span><span class="sxs-lookup"><span data-stu-id="8c25b-626">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="8c25b-627">`ConfigureAppConfiguration` 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="8c25b-627">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="8c25b-628">應用程式會使用指定索引鍵上最後設定值的任何選項。</span><span class="sxs-lookup"><span data-stu-id="8c25b-628">The app uses whichever option sets a value last on a given key.</span></span> 

<span data-ttu-id="8c25b-629">由 `ConfigureAppConfiguration` 建立的組態位於 [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*)，可用於後續作業並用作 DI 中的服務。</span><span class="sxs-lookup"><span data-stu-id="8c25b-629">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and as a service from DI.</span></span> <span data-ttu-id="8c25b-630">主機組態也會新增至應用程式組態。</span><span class="sxs-lookup"><span data-stu-id="8c25b-630">The host configuration is also added to the app configuration.</span></span>

<span data-ttu-id="8c25b-631">如需詳細資訊，請參閱 [ASP.NET Core 中的組態](xref:fundamentals/configuration/index#configureappconfiguration)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-631">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span></span>

## <a name="settings-for-all-app-types"></a><span data-ttu-id="8c25b-632">所有應用程式類型的設定</span><span class="sxs-lookup"><span data-stu-id="8c25b-632">Settings for all app types</span></span>

<span data-ttu-id="8c25b-633">本節列出適用於 HTTP 和非 HTTP 工作負載的主機設定。</span><span class="sxs-lookup"><span data-stu-id="8c25b-633">This section lists host settings that apply to both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="8c25b-634">根據預設，用來設定這些設定的環境變數可以具有 `DOTNET_` 或 `ASPNETCORE_` 前置詞。</span><span class="sxs-lookup"><span data-stu-id="8c25b-634">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<!-- In the following sections, two spaces at end of line are used to force line breaks in the rendered page. -->

### <a name="applicationname"></a><span data-ttu-id="8c25b-635">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="8c25b-635">ApplicationName</span></span>

<span data-ttu-id="8c25b-636">[IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) 屬性是在主機建構期間從主機組態當中設定。</span><span class="sxs-lookup"><span data-stu-id="8c25b-636">The [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span>

<span data-ttu-id="8c25b-637">機**碼**：`applicationName`</span><span class="sxs-lookup"><span data-stu-id="8c25b-637">**Key**: `applicationName`</span></span>  
<span data-ttu-id="8c25b-638">**類型**：`string`</span><span class="sxs-lookup"><span data-stu-id="8c25b-638">**Type**: `string`</span></span>  
<span data-ttu-id="8c25b-639">**預設值**：包含應用程式進入點的元件名稱。</span><span class="sxs-lookup"><span data-stu-id="8c25b-639">**Default**: The name of the assembly that contains the app's entry point.</span></span>  
<span data-ttu-id="8c25b-640">**環境變數**：`<PREFIX_>APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="8c25b-640">**Environment variable**: `<PREFIX_>APPLICATIONNAME`</span></span>

<span data-ttu-id="8c25b-641">若要設定此值，請使用環境變數。</span><span class="sxs-lookup"><span data-stu-id="8c25b-641">To set this value, use the environment variable.</span></span> 

### <a name="contentroot"></a><span data-ttu-id="8c25b-642">ContentRoot</span><span class="sxs-lookup"><span data-stu-id="8c25b-642">ContentRoot</span></span>

<span data-ttu-id="8c25b-643">[IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) 屬性會決定主機從哪裡開始搜尋內容檔案。</span><span class="sxs-lookup"><span data-stu-id="8c25b-643">The [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) property determines where the host begins searching for content files.</span></span> <span data-ttu-id="8c25b-644">如果路徑不存在，就無法啟動主機。</span><span class="sxs-lookup"><span data-stu-id="8c25b-644">If the path doesn't exist, the host fails to start.</span></span>

<span data-ttu-id="8c25b-645">機**碼**：`contentRoot`</span><span class="sxs-lookup"><span data-stu-id="8c25b-645">**Key**: `contentRoot`</span></span>  
<span data-ttu-id="8c25b-646">**類型**：`string`</span><span class="sxs-lookup"><span data-stu-id="8c25b-646">**Type**: `string`</span></span>  
<span data-ttu-id="8c25b-647">**預設值**：應用程式元件所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="8c25b-647">**Default**: The folder where the app assembly resides.</span></span>  
<span data-ttu-id="8c25b-648">**環境變數**：`<PREFIX_>CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="8c25b-648">**Environment variable**: `<PREFIX_>CONTENTROOT`</span></span>

<span data-ttu-id="8c25b-649">若要設定此值，請使用環境變數或呼叫 `IHostBuilder` 上的 `UseContentRoot`：</span><span class="sxs-lookup"><span data-stu-id="8c25b-649">To set this value, use the environment variable or call `UseContentRoot` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

<span data-ttu-id="8c25b-650">如需詳細資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="8c25b-650">For more information, see:</span></span>

* [<span data-ttu-id="8c25b-651">基本概念：內容根目錄</span><span class="sxs-lookup"><span data-stu-id="8c25b-651">Fundamentals: Content root</span></span>](xref:fundamentals/index#content-root)
* [<span data-ttu-id="8c25b-652">WebRoot</span><span class="sxs-lookup"><span data-stu-id="8c25b-652">WebRoot</span></span>](#webroot)

### <a name="environmentname"></a><span data-ttu-id="8c25b-653">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="8c25b-653">EnvironmentName</span></span>

<span data-ttu-id="8c25b-654">[IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) 屬性可以設為任何值。</span><span class="sxs-lookup"><span data-stu-id="8c25b-654">The [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) property can be set to any value.</span></span> <span data-ttu-id="8c25b-655">架構定義的值包括 `Development`、`Staging` 和 `Production`。</span><span class="sxs-lookup"><span data-stu-id="8c25b-655">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="8c25b-656">值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="8c25b-656">Values aren't case-sensitive.</span></span>

<span data-ttu-id="8c25b-657">機**碼**：`environment`</span><span class="sxs-lookup"><span data-stu-id="8c25b-657">**Key**: `environment`</span></span>  
<span data-ttu-id="8c25b-658">**類型**：`string`</span><span class="sxs-lookup"><span data-stu-id="8c25b-658">**Type**: `string`</span></span>  
<span data-ttu-id="8c25b-659">**預設**：`Production`</span><span class="sxs-lookup"><span data-stu-id="8c25b-659">**Default**: `Production`</span></span>  
<span data-ttu-id="8c25b-660">**環境變數**：`<PREFIX_>ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="8c25b-660">**Environment variable**: `<PREFIX_>ENVIRONMENT`</span></span>

<span data-ttu-id="8c25b-661">若要設定此值，請使用環境變數或呼叫 `IHostBuilder` 上的 `UseEnvironment`：</span><span class="sxs-lookup"><span data-stu-id="8c25b-661">To set this value, use the environment variable or call `UseEnvironment` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a><span data-ttu-id="8c25b-662">ShutdownTimeout</span><span class="sxs-lookup"><span data-stu-id="8c25b-662">ShutdownTimeout</span></span>

<span data-ttu-id="8c25b-663">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) 會設定 <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> 的逾時。</span><span class="sxs-lookup"><span data-stu-id="8c25b-663">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="8c25b-664">預設值是五秒鐘。</span><span class="sxs-lookup"><span data-stu-id="8c25b-664">The default value is five seconds.</span></span>  <span data-ttu-id="8c25b-665">在逾時期間，主機會：</span><span class="sxs-lookup"><span data-stu-id="8c25b-665">During the timeout period, the host:</span></span>

* <span data-ttu-id="8c25b-666">觸發 [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.ihostapplicationlifetime.applicationstopping)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-666">Triggers [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.ihostapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="8c25b-667">嘗試停止託管的服務，並記錄無法停止之服務的錯誤。</span><span class="sxs-lookup"><span data-stu-id="8c25b-667">Attempts to stop hosted services, logging errors for services that fail to stop.</span></span>

<span data-ttu-id="8c25b-668">如果在所有的託管服務停止之前逾時期限已到期，則應用程式關閉時，會停止任何剩餘的作用中服務。</span><span class="sxs-lookup"><span data-stu-id="8c25b-668">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="8c25b-669">即使服務尚未完成處理也會停止。</span><span class="sxs-lookup"><span data-stu-id="8c25b-669">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="8c25b-670">如果服務需要更多時間才能停止，請增加逾時。</span><span class="sxs-lookup"><span data-stu-id="8c25b-670">If services require additional time to stop, increase the timeout.</span></span>

<span data-ttu-id="8c25b-671">機**碼**：`shutdownTimeoutSeconds`</span><span class="sxs-lookup"><span data-stu-id="8c25b-671">**Key**: `shutdownTimeoutSeconds`</span></span>  
<span data-ttu-id="8c25b-672">**類型**：`int`</span><span class="sxs-lookup"><span data-stu-id="8c25b-672">**Type**: `int`</span></span>  
<span data-ttu-id="8c25b-673">**預設值**：5秒</span><span class="sxs-lookup"><span data-stu-id="8c25b-673">**Default**: 5 seconds</span></span>  
<span data-ttu-id="8c25b-674">**環境變數**：`<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="8c25b-674">**Environment variable**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="8c25b-675">若要設定此值，請使用環境變數或設定 `HostOptions`。</span><span class="sxs-lookup"><span data-stu-id="8c25b-675">To set this value, use the environment variable or configure `HostOptions`.</span></span> <span data-ttu-id="8c25b-676">下列範例將逾時設為 20 秒：</span><span class="sxs-lookup"><span data-stu-id="8c25b-676">The following example sets the timeout to 20 seconds:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

### <a name="disable-app-configuration-reload-on-change"></a><span data-ttu-id="8c25b-677">在變更時停用應用程式設定重載</span><span class="sxs-lookup"><span data-stu-id="8c25b-677">Disable app configuration reload on change</span></span>

<span data-ttu-id="8c25b-678">根據[預設](xref:fundamentals/configuration/index#default)， *appsettings.js*和*appsettings。 {環境}. json*會在檔案變更時重載。</span><span class="sxs-lookup"><span data-stu-id="8c25b-678">By [default](xref:fundamentals/configuration/index#default), *appsettings.json* and *appsettings.{Environment}.json* are reloaded when the file changes.</span></span> <span data-ttu-id="8c25b-679">若要在 ASP.NET Core 5.0 Preview 3 或更新版本中停用此重載行為，請將 `hostBuilder:reloadConfigOnChange` 金鑰設定為 `false` 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-679">To disable this reload behavior in ASP.NET Core 5.0 Preview 3 or later, set the `hostBuilder:reloadConfigOnChange` key to `false`.</span></span>

<span data-ttu-id="8c25b-680">機**碼**：`hostBuilder:reloadConfigOnChange`</span><span class="sxs-lookup"><span data-stu-id="8c25b-680">**Key**: `hostBuilder:reloadConfigOnChange`</span></span>  
<span data-ttu-id="8c25b-681">**類型**： `bool` （ `true` 或 `1` ）</span><span class="sxs-lookup"><span data-stu-id="8c25b-681">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="8c25b-682">**預設**：`true`</span><span class="sxs-lookup"><span data-stu-id="8c25b-682">**Default**: `true`</span></span>  
<span data-ttu-id="8c25b-683">**命令列引數**：`hostBuilder:reloadConfigOnChange`</span><span class="sxs-lookup"><span data-stu-id="8c25b-683">**Command-line argument**: `hostBuilder:reloadConfigOnChange`</span></span>  
<span data-ttu-id="8c25b-684">**環境變數**：`<PREFIX_>hostBuilder:reloadConfigOnChange`</span><span class="sxs-lookup"><span data-stu-id="8c25b-684">**Environment variable**: `<PREFIX_>hostBuilder:reloadConfigOnChange`</span></span>

> [!WARNING]
> <span data-ttu-id="8c25b-685">冒號（ `:` ）分隔符號不適用於所有平臺上的環境變數階層式索引鍵。</span><span class="sxs-lookup"><span data-stu-id="8c25b-685">The colon (`:`) separator doesn't work with environment variable hierarchical keys on all platforms.</span></span> <span data-ttu-id="8c25b-686">如需詳細資訊，請參閱[環境變數](xref:fundamentals/configuration/index#environment-variables)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-686">For more information, see [Environment variables](xref:fundamentals/configuration/index#environment-variables).</span></span>

## <a name="settings-for-web-apps"></a><span data-ttu-id="8c25b-687">Web 應用程式的設定</span><span class="sxs-lookup"><span data-stu-id="8c25b-687">Settings for web apps</span></span>

<span data-ttu-id="8c25b-688">某些主機設定僅適用於 HTTP 工作負載。</span><span class="sxs-lookup"><span data-stu-id="8c25b-688">Some host settings apply only to HTTP workloads.</span></span> <span data-ttu-id="8c25b-689">根據預設，用來設定這些設定的環境變數可以具有 `DOTNET_` 或 `ASPNETCORE_` 前置詞。</span><span class="sxs-lookup"><span data-stu-id="8c25b-689">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<span data-ttu-id="8c25b-690">`IWebHostBuilder` 上的擴充方法適用於這些設定。</span><span class="sxs-lookup"><span data-stu-id="8c25b-690">Extension methods on `IWebHostBuilder` are available for these settings.</span></span> <span data-ttu-id="8c25b-691">示範如何呼叫擴充方法的程式碼範例假設 `webBuilder` 是 `IWebHostBuilder` 的執行個體，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="8c25b-691">Code samples that show how to call the extension methods assume `webBuilder` is an instance of `IWebHostBuilder`, as in the following example:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a><span data-ttu-id="8c25b-692">CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="8c25b-692">CaptureStartupErrors</span></span>

<span data-ttu-id="8c25b-693">當它為 `false` 時，啟動期間發生的錯誤會導致主機結束。</span><span class="sxs-lookup"><span data-stu-id="8c25b-693">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="8c25b-694">當它為 `true` 時，主機會擷取啟動期間的例外狀況，並嘗試啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="8c25b-694">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

<span data-ttu-id="8c25b-695">機**碼**：`captureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="8c25b-695">**Key**: `captureStartupErrors`</span></span>  
<span data-ttu-id="8c25b-696">**類型**： `bool` （ `true` 或 `1` ）</span><span class="sxs-lookup"><span data-stu-id="8c25b-696">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="8c25b-697">**預設值**：預設為 `false`，除非應用程式執行時在 IIS 背後有 Kestrel，此時預設值即為 `true`。</span><span class="sxs-lookup"><span data-stu-id="8c25b-697">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="8c25b-698">**環境變數**：`<PREFIX_>CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="8c25b-698">**Environment variable**: `<PREFIX_>CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="8c25b-699">若要設定此值，請使用組態或呼叫 `CaptureStartupErrors`：</span><span class="sxs-lookup"><span data-stu-id="8c25b-699">To set this value, use configuration or call `CaptureStartupErrors`:</span></span>

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a><span data-ttu-id="8c25b-700">DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="8c25b-700">DetailedErrors</span></span>

<span data-ttu-id="8c25b-701">啟用時 (或當環境為 `Development` 時)，應用程式會擷取詳細錯誤。</span><span class="sxs-lookup"><span data-stu-id="8c25b-701">When enabled, or when the environment is `Development`, the app captures detailed errors.</span></span>

<span data-ttu-id="8c25b-702">機**碼**：`detailedErrors`</span><span class="sxs-lookup"><span data-stu-id="8c25b-702">**Key**: `detailedErrors`</span></span>  
<span data-ttu-id="8c25b-703">**類型**： `bool` （ `true` 或 `1` ）</span><span class="sxs-lookup"><span data-stu-id="8c25b-703">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="8c25b-704">**預設**：`false`</span><span class="sxs-lookup"><span data-stu-id="8c25b-704">**Default**: `false`</span></span>  
<span data-ttu-id="8c25b-705">**環境變數**：`<PREFIX_>_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="8c25b-705">**Environment variable**: `<PREFIX_>_DETAILEDERRORS`</span></span>

<span data-ttu-id="8c25b-706">若要設定此值，請使用組態或呼叫 `UseSetting`：</span><span class="sxs-lookup"><span data-stu-id="8c25b-706">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a><span data-ttu-id="8c25b-707">HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="8c25b-707">HostingStartupAssemblies</span></span>

<span data-ttu-id="8c25b-708">在啟動時載入的裝載啟動組件字串，以分號分隔。</span><span class="sxs-lookup"><span data-stu-id="8c25b-708">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="8c25b-709">雖然設定值會預設為空字串，但裝載啟動組件一律會包含應用程式的組件。</span><span class="sxs-lookup"><span data-stu-id="8c25b-709">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="8c25b-710">提供裝載啟動組件時，它們會新增至應用程式的組件，以便在應用程式在啟動時建置其通用服務時載入。</span><span class="sxs-lookup"><span data-stu-id="8c25b-710">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

<span data-ttu-id="8c25b-711">機**碼**：`hostingStartupAssemblies`</span><span class="sxs-lookup"><span data-stu-id="8c25b-711">**Key**: `hostingStartupAssemblies`</span></span>  
<span data-ttu-id="8c25b-712">**類型**：`string`</span><span class="sxs-lookup"><span data-stu-id="8c25b-712">**Type**: `string`</span></span>  
<span data-ttu-id="8c25b-713">**預設值**：空字串</span><span class="sxs-lookup"><span data-stu-id="8c25b-713">**Default**: Empty string</span></span>  
<span data-ttu-id="8c25b-714">**環境變數**：`<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="8c25b-714">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="8c25b-715">若要設定此值，請使用組態或呼叫 `UseSetting`：</span><span class="sxs-lookup"><span data-stu-id="8c25b-715">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a><span data-ttu-id="8c25b-716">HostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="8c25b-716">HostingStartupExcludeAssemblies</span></span>

<span data-ttu-id="8c25b-717">在啟動時排除以分號分隔的裝載啟動組件字串。</span><span class="sxs-lookup"><span data-stu-id="8c25b-717">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="8c25b-718">機**碼**：`hostingStartupExcludeAssemblies`</span><span class="sxs-lookup"><span data-stu-id="8c25b-718">**Key**: `hostingStartupExcludeAssemblies`</span></span>  
<span data-ttu-id="8c25b-719">**類型**：`string`</span><span class="sxs-lookup"><span data-stu-id="8c25b-719">**Type**: `string`</span></span>  
<span data-ttu-id="8c25b-720">**預設值**：空字串</span><span class="sxs-lookup"><span data-stu-id="8c25b-720">**Default**: Empty string</span></span>  
<span data-ttu-id="8c25b-721">**環境變數**：`<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="8c25b-721">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="8c25b-722">若要設定此值，請使用組態或呼叫 `UseSetting`：</span><span class="sxs-lookup"><span data-stu-id="8c25b-722">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="https_port"></a><span data-ttu-id="8c25b-723">HTTPS_Port</span><span class="sxs-lookup"><span data-stu-id="8c25b-723">HTTPS_Port</span></span>

<span data-ttu-id="8c25b-724">HTTPS 重新導向連接埠。</span><span class="sxs-lookup"><span data-stu-id="8c25b-724">The HTTPS redirect port.</span></span> <span data-ttu-id="8c25b-725">用於[強制 HTTPS](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-725">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="8c25b-726">機**碼**：`https_port`</span><span class="sxs-lookup"><span data-stu-id="8c25b-726">**Key**: `https_port`</span></span>  
<span data-ttu-id="8c25b-727">**類型**：`string`</span><span class="sxs-lookup"><span data-stu-id="8c25b-727">**Type**: `string`</span></span>  
<span data-ttu-id="8c25b-728">**預設**值：未設定預設值。</span><span class="sxs-lookup"><span data-stu-id="8c25b-728">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="8c25b-729">**環境變數**：`<PREFIX_>HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="8c25b-729">**Environment variable**: `<PREFIX_>HTTPS_PORT`</span></span>

<span data-ttu-id="8c25b-730">若要設定此值，請使用組態或呼叫 `UseSetting`：</span><span class="sxs-lookup"><span data-stu-id="8c25b-730">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a><span data-ttu-id="8c25b-731">PreferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="8c25b-731">PreferHostingUrls</span></span>

<span data-ttu-id="8c25b-732">指出主機是否應接聽以設定的 Url， `IWebHostBuilder` 而不是使用執行時所設定的 url `IServer` 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-732">Indicates whether the host should listen on the URLs configured with the `IWebHostBuilder` instead of those URLs configured with the `IServer` implementation.</span></span>

<span data-ttu-id="8c25b-733">機**碼**：`preferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="8c25b-733">**Key**: `preferHostingUrls`</span></span>  
<span data-ttu-id="8c25b-734">**類型**： `bool` （ `true` 或 `1` ）</span><span class="sxs-lookup"><span data-stu-id="8c25b-734">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="8c25b-735">**預設**：`true`</span><span class="sxs-lookup"><span data-stu-id="8c25b-735">**Default**: `true`</span></span>  
<span data-ttu-id="8c25b-736">**環境變數**：`<PREFIX_>_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="8c25b-736">**Environment variable**: `<PREFIX_>_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="8c25b-737">若要設定此值，請使用環境變數或呼叫 `PreferHostingUrls`：</span><span class="sxs-lookup"><span data-stu-id="8c25b-737">To set this value, use the environment variable or call `PreferHostingUrls`:</span></span>

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a><span data-ttu-id="8c25b-738">PreventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="8c25b-738">PreventHostingStartup</span></span>

<span data-ttu-id="8c25b-739">可防止自動載入裝載啟動組件，包括應用程式組件所設定的裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="8c25b-739">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="8c25b-740">如需詳細資訊，請參閱 <xref:fundamentals/configuration/platform-specific-configuration> 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-740">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="8c25b-741">機**碼**：`preventHostingStartup`</span><span class="sxs-lookup"><span data-stu-id="8c25b-741">**Key**: `preventHostingStartup`</span></span>  
<span data-ttu-id="8c25b-742">**類型**： `bool` （ `true` 或 `1` ）</span><span class="sxs-lookup"><span data-stu-id="8c25b-742">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="8c25b-743">**預設**：`false`</span><span class="sxs-lookup"><span data-stu-id="8c25b-743">**Default**: `false`</span></span>  
<span data-ttu-id="8c25b-744">**環境變數**：`<PREFIX_>_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="8c25b-744">**Environment variable**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="8c25b-745">若要設定此值，請使用環境變數或呼叫 `UseSetting`：</span><span class="sxs-lookup"><span data-stu-id="8c25b-745">To set this value, use the environment variable or call `UseSetting` :</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a><span data-ttu-id="8c25b-746">StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="8c25b-746">StartupAssembly</span></span>

<span data-ttu-id="8c25b-747">要搜尋 `Startup` 類別的組件。</span><span class="sxs-lookup"><span data-stu-id="8c25b-747">The assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="8c25b-748">機**碼**：`startupAssembly`</span><span class="sxs-lookup"><span data-stu-id="8c25b-748">**Key**: `startupAssembly`</span></span>  
<span data-ttu-id="8c25b-749">**類型**：`string`</span><span class="sxs-lookup"><span data-stu-id="8c25b-749">**Type**: `string`</span></span>  
<span data-ttu-id="8c25b-750">**預設值**：應用程式的組件</span><span class="sxs-lookup"><span data-stu-id="8c25b-750">**Default**: The app's assembly</span></span>  
<span data-ttu-id="8c25b-751">**環境變數**：`<PREFIX_>STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="8c25b-751">**Environment variable**: `<PREFIX_>STARTUPASSEMBLY`</span></span>

<span data-ttu-id="8c25b-752">若要設定此值，請使用環境變數或呼叫 `UseStartup`。</span><span class="sxs-lookup"><span data-stu-id="8c25b-752">To set this value, use the environment variable or call `UseStartup`.</span></span> <span data-ttu-id="8c25b-753">`UseStartup` 可以採用組件名稱 (`string`) 或類型 (`TStartup`)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-753">`UseStartup` can take an assembly name (`string`) or a type (`TStartup`).</span></span> <span data-ttu-id="8c25b-754">如果呼叫多個 `UseStartup` 方法，最後一個將會優先。</span><span class="sxs-lookup"><span data-stu-id="8c25b-754">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a><span data-ttu-id="8c25b-755">URL</span><span class="sxs-lookup"><span data-stu-id="8c25b-755">URLs</span></span>

<span data-ttu-id="8c25b-756">以分號分隔的 IP 位址或主機位址，包含伺服器應接聽要求的連接埠和通訊協定。</span><span class="sxs-lookup"><span data-stu-id="8c25b-756">A semicolon-delimited list of IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span> <span data-ttu-id="8c25b-757">例如 `http://localhost:123`。</span><span class="sxs-lookup"><span data-stu-id="8c25b-757">For example, `http://localhost:123`.</span></span> <span data-ttu-id="8c25b-758">使用 "\*"，表示伺服器應接聽任何 IP 位址或主機名稱上的要求，並使用指定的連接埠和通訊協定 (例如，`http://*:5000`)。</span><span class="sxs-lookup"><span data-stu-id="8c25b-758">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="8c25b-759">通訊協定 (`http://` 或 `https://`) 必須包含在每個 URL 中。</span><span class="sxs-lookup"><span data-stu-id="8c25b-759">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="8c25b-760">支援的格式會依伺服器而有所不同。</span><span class="sxs-lookup"><span data-stu-id="8c25b-760">Supported formats vary among servers.</span></span>

<span data-ttu-id="8c25b-761">機**碼**：`urls`</span><span class="sxs-lookup"><span data-stu-id="8c25b-761">**Key**: `urls`</span></span>  
<span data-ttu-id="8c25b-762">**類型**：`string`</span><span class="sxs-lookup"><span data-stu-id="8c25b-762">**Type**: `string`</span></span>  
<span data-ttu-id="8c25b-763">**預設**： `http://localhost:5000` 和`https://localhost:5001`</span><span class="sxs-lookup"><span data-stu-id="8c25b-763">**Default**: `http://localhost:5000` and `https://localhost:5001`</span></span>  
<span data-ttu-id="8c25b-764">**環境變數**：`<PREFIX_>URLS`</span><span class="sxs-lookup"><span data-stu-id="8c25b-764">**Environment variable**: `<PREFIX_>URLS`</span></span>

<span data-ttu-id="8c25b-765">若要設定此值，請使用環境變數或呼叫 `UseUrls`：</span><span class="sxs-lookup"><span data-stu-id="8c25b-765">To set this value, use the environment variable or call `UseUrls`:</span></span>

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

<span data-ttu-id="8c25b-766">Kestrel 有它自己的端點設定 API。</span><span class="sxs-lookup"><span data-stu-id="8c25b-766">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="8c25b-767">如需詳細資訊，請參閱 <xref:fundamentals/servers/kestrel#endpoint-configuration> 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-767">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="webroot"></a><span data-ttu-id="8c25b-768">WebRoot</span><span class="sxs-lookup"><span data-stu-id="8c25b-768">WebRoot</span></span>

<span data-ttu-id="8c25b-769">[IWebHostEnvironment. WebRootPath](xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment.WebRootPath)屬性會決定應用程式靜態資產的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="8c25b-769">The [IWebHostEnvironment.WebRootPath](xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment.WebRootPath) property determines the relative path to the app's static assets.</span></span> <span data-ttu-id="8c25b-770">如果路徑不存在，則會使用無作業檔案提供者。</span><span class="sxs-lookup"><span data-stu-id="8c25b-770">If the path doesn't exist, a no-op file provider is used.</span></span>  

<span data-ttu-id="8c25b-771">機**碼**：`webroot`</span><span class="sxs-lookup"><span data-stu-id="8c25b-771">**Key**: `webroot`</span></span>  
<span data-ttu-id="8c25b-772">**類型**：`string`</span><span class="sxs-lookup"><span data-stu-id="8c25b-772">**Type**: `string`</span></span>  
<span data-ttu-id="8c25b-773">**預設**值：預設值為 `wwwroot` 。</span><span class="sxs-lookup"><span data-stu-id="8c25b-773">**Default**: The default is `wwwroot`.</span></span> <span data-ttu-id="8c25b-774">*{Content root}/wwwroot*的路徑必須存在。</span><span class="sxs-lookup"><span data-stu-id="8c25b-774">The path to *{content root}/wwwroot* must exist.</span></span>  
<span data-ttu-id="8c25b-775">**環境變數**：`<PREFIX_>WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="8c25b-775">**Environment variable**: `<PREFIX_>WEBROOT`</span></span>

<span data-ttu-id="8c25b-776">若要設定此值，請使用環境變數或呼叫 `IWebHostBuilder` 上的 `UseWebRoot`：</span><span class="sxs-lookup"><span data-stu-id="8c25b-776">To set this value, use the environment variable or call `UseWebRoot` on `IWebHostBuilder`:</span></span>

```csharp
webBuilder.UseWebRoot("public");
```

<span data-ttu-id="8c25b-777">如需詳細資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="8c25b-777">For more information, see:</span></span>

* [<span data-ttu-id="8c25b-778">基本概念： Web 根目錄</span><span class="sxs-lookup"><span data-stu-id="8c25b-778">Fundamentals: Web root</span></span>](xref:fundamentals/index#web-root)
* [<span data-ttu-id="8c25b-779">ContentRoot</span><span class="sxs-lookup"><span data-stu-id="8c25b-779">ContentRoot</span></span>](#contentroot)

## <a name="manage-the-host-lifetime"></a><span data-ttu-id="8c25b-780">管理主機存留期</span><span class="sxs-lookup"><span data-stu-id="8c25b-780">Manage the host lifetime</span></span>

<span data-ttu-id="8c25b-781">在建置的 <xref:Microsoft.Extensions.Hosting.IHost> 實作上呼叫方法來啟動和停止應用程式。</span><span class="sxs-lookup"><span data-stu-id="8c25b-781">Call methods on the built <xref:Microsoft.Extensions.Hosting.IHost> implementation to start and stop the app.</span></span> <span data-ttu-id="8c25b-782">這些方法會影響所有在服務容器中註冊的 <xref:Microsoft.Extensions.Hosting.IHostedService> 實作。</span><span class="sxs-lookup"><span data-stu-id="8c25b-782">These methods affect all  <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="8c25b-783">執行</span><span class="sxs-lookup"><span data-stu-id="8c25b-783">Run</span></span>

<span data-ttu-id="8c25b-784"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> 會執行應用程式並封鎖呼叫執行緒，直到主機關閉為止。</span><span class="sxs-lookup"><span data-stu-id="8c25b-784"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down.</span></span>

### <a name="runasync"></a><span data-ttu-id="8c25b-785">RunAsync</span><span class="sxs-lookup"><span data-stu-id="8c25b-785">RunAsync</span></span>

<span data-ttu-id="8c25b-786"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> 會執行應用程式，並傳回觸發取消語彙基元或關機時所完成的 <xref:System.Threading.Tasks.Task>。</span><span class="sxs-lookup"><span data-stu-id="8c25b-786"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span>

### <a name="runconsoleasync"></a><span data-ttu-id="8c25b-787">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="8c25b-787">RunConsoleAsync</span></span>

<span data-ttu-id="8c25b-788"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*>啟用主控台支援、組建和啟動主機，以及等候<kbd>Ctrl</kbd> + <kbd>C</kbd>/SIGINT 或 SIGTERM 關閉。</span><span class="sxs-lookup"><span data-stu-id="8c25b-788"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM to shut down.</span></span>

### <a name="start"></a><span data-ttu-id="8c25b-789">開始</span><span class="sxs-lookup"><span data-stu-id="8c25b-789">Start</span></span>

<span data-ttu-id="8c25b-790"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> 會同步啟動主機。</span><span class="sxs-lookup"><span data-stu-id="8c25b-790"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

### <a name="startasync"></a><span data-ttu-id="8c25b-791">StartAsync</span><span class="sxs-lookup"><span data-stu-id="8c25b-791">StartAsync</span></span>

<span data-ttu-id="8c25b-792"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> 會啟動主機，並傳回觸發取消語彙基元或關機時所完成的 <xref:System.Threading.Tasks.Task>。</span><span class="sxs-lookup"><span data-stu-id="8c25b-792"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the host and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span> 

<span data-ttu-id="8c25b-793"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> 在 `StartAsync` 開始時呼叫，並等到完成後再繼續進行。</span><span class="sxs-lookup"><span data-stu-id="8c25b-793"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of `StartAsync`, which waits until it's complete before continuing.</span></span> <span data-ttu-id="8c25b-794">這可用來將啟動延遲到外部事件發出訊號為止。</span><span class="sxs-lookup"><span data-stu-id="8c25b-794">This can be used to delay startup until signaled by an external event.</span></span>

### <a name="stopasync"></a><span data-ttu-id="8c25b-795">StopAsync</span><span class="sxs-lookup"><span data-stu-id="8c25b-795">StopAsync</span></span>

<span data-ttu-id="8c25b-796"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> 會嘗試在提供的逾時內停止主機。</span><span class="sxs-lookup"><span data-stu-id="8c25b-796"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

### <a name="waitforshutdown"></a><span data-ttu-id="8c25b-797">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="8c25b-797">WaitForShutdown</span></span>

<span data-ttu-id="8c25b-798"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*>封鎖呼叫執行緒，直到 IHostLifetime 觸發關閉為止，例如經由<kbd>Ctrl</kbd> + <kbd>C</kbd>/SIGINT 或 SIGTERM。</span><span class="sxs-lookup"><span data-stu-id="8c25b-798"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blocks the calling thread until shutdown is triggered by the IHostLifetime, such as via <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM.</span></span>

### <a name="waitforshutdownasync"></a><span data-ttu-id="8c25b-799">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="8c25b-799">WaitForShutdownAsync</span></span>

<span data-ttu-id="8c25b-800"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> 會傳回透過指定語彙基元觸發關機時所完成的 <xref:System.Threading.Tasks.Task>，並呼叫 <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>。</span><span class="sxs-lookup"><span data-stu-id="8c25b-800"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

### <a name="external-control"></a><span data-ttu-id="8c25b-801">外部控制</span><span class="sxs-lookup"><span data-stu-id="8c25b-801">External control</span></span>

<span data-ttu-id="8c25b-802">主機存留期直接控制可以使用可從外部呼叫的方法來達成：</span><span class="sxs-lookup"><span data-stu-id="8c25b-802">Direct control of the host lifetime can be achieved using methods that can be called externally:</span></span>

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="8c25b-803">其他資源</span><span class="sxs-lookup"><span data-stu-id="8c25b-803">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
