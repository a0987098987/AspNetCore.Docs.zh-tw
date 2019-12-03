---
title: .NET 泛型主機
author: rick-anderson
description: 了解 .NET Core 的泛型主機，其負責啟動應用程式及管理存留期。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/02/2019
uid: fundamentals/host/generic-host
ms.openlocfilehash: 2ed4af109b5ccd303a03a0d9167649dda7793126
ms.sourcegitcommit: 3b6b0a54b20dc99b0c8c5978400c60adf431072f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2019
ms.locfileid: "74717018"
---
# <a name="net-generic-host"></a><span data-ttu-id="dc124-103">.NET 泛型主機</span><span class="sxs-lookup"><span data-stu-id="dc124-103">.NET Generic Host</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="dc124-104">本文介紹 .NET Core 的泛型主機 (<xref:Microsoft.Extensions.Hosting.HostBuilder>)，並提供指導。</span><span class="sxs-lookup"><span data-stu-id="dc124-104">This article introduces the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) and provides guidance on how to use it.</span></span>

## <a name="whats-a-host"></a><span data-ttu-id="dc124-105">什麼是主機？</span><span class="sxs-lookup"><span data-stu-id="dc124-105">What's a host?</span></span>

<span data-ttu-id="dc124-106">「主機」是封裝所有應用程式資源的物件，例如：</span><span class="sxs-lookup"><span data-stu-id="dc124-106">A *host* is an object that encapsulates an app's resources, such as:</span></span>

* <span data-ttu-id="dc124-107">相依性插入 (DI)</span><span class="sxs-lookup"><span data-stu-id="dc124-107">Dependency injection (DI)</span></span>
* <span data-ttu-id="dc124-108">記錄</span><span class="sxs-lookup"><span data-stu-id="dc124-108">Logging</span></span>
* <span data-ttu-id="dc124-109">組態</span><span class="sxs-lookup"><span data-stu-id="dc124-109">Configuration</span></span>
* <span data-ttu-id="dc124-110">`IHostedService` 實作</span><span class="sxs-lookup"><span data-stu-id="dc124-110">`IHostedService` implementations</span></span>

<span data-ttu-id="dc124-111">當啟動主機時，會在 DI 容器中找到的每個 `IHostedService.StartAsync` 實作上呼叫 <xref:Microsoft.Extensions.Hosting.IHostedService>。</span><span class="sxs-lookup"><span data-stu-id="dc124-111">When a host starts, it calls `IHostedService.StartAsync` on each implementation of <xref:Microsoft.Extensions.Hosting.IHostedService> that it finds in the DI container.</span></span> <span data-ttu-id="dc124-112">在 Web 應用程式中，其中一個 `IHostedService` 實作是一種 Web 服務，負責啟動 [HTTP 伺服器實作](xref:fundamentals/index#servers)。</span><span class="sxs-lookup"><span data-stu-id="dc124-112">In a web app, one of the `IHostedService` implementations is a web service that starts an [HTTP server implementation](xref:fundamentals/index#servers).</span></span>

<span data-ttu-id="dc124-113">在單一物件中包含所有應用程式相互依存資源的主要理由便是生命週期管理：控制應用程式的啟動及順利關機。</span><span class="sxs-lookup"><span data-stu-id="dc124-113">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="dc124-114">在 3.0 之前版本的 ASP.NET Core 中，[Web 主機](xref:fundamentals/host/web-host)用於 HTTP 工作負載。</span><span class="sxs-lookup"><span data-stu-id="dc124-114">In versions of ASP.NET Core earlier than 3.0, the [Web Host](xref:fundamentals/host/web-host) is used for HTTP workloads.</span></span> <span data-ttu-id="dc124-115">不再建議將 Web 應用程式用於 Web 主機，且僅維持適用於回溯相容性。</span><span class="sxs-lookup"><span data-stu-id="dc124-115">The Web Host is no longer recommended for web apps and remains available only for backward compatibility.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="dc124-116">設定主機</span><span class="sxs-lookup"><span data-stu-id="dc124-116">Set up a host</span></span>

<span data-ttu-id="dc124-117">主機通常由 `Program` 類別的程式碼來設定、建置並執行。</span><span class="sxs-lookup"><span data-stu-id="dc124-117">The host is typically configured, built, and run by code in the `Program` class.</span></span> <span data-ttu-id="dc124-118">`Main` 方法：</span><span class="sxs-lookup"><span data-stu-id="dc124-118">The `Main` method:</span></span>

* <span data-ttu-id="dc124-119">呼叫 `CreateHostBuilder` 方法來建立及設定建立器物件。</span><span class="sxs-lookup"><span data-stu-id="dc124-119">Calls a `CreateHostBuilder` method to create and configure a builder object.</span></span>
* <span data-ttu-id="dc124-120">在建立器物件上呼叫 `Build` 和 `Run` 方法。</span><span class="sxs-lookup"><span data-stu-id="dc124-120">Calls `Build` and `Run` methods on the builder object.</span></span>

<span data-ttu-id="dc124-121">以下是用於非 HTTP 工作負載的 *Program.cs* 程式碼，其中的單一 `IHostedService` 實作會新增至 DI 容器。</span><span class="sxs-lookup"><span data-stu-id="dc124-121">Here's *Program.cs* code for a non-HTTP workload, with a single `IHostedService` implementation added to the DI container.</span></span> 

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

<span data-ttu-id="dc124-122">針對 HTTP 工作負載，`Main` 方法相同，但 `CreateHostBuilder` 會呼叫 `ConfigureWebHostDefaults`：</span><span class="sxs-lookup"><span data-stu-id="dc124-122">For an HTTP workload, the `Main` method is the same but `CreateHostBuilder` calls `ConfigureWebHostDefaults`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="dc124-123">如果應用程式使用 Entity Framework Core，請勿變更 `CreateHostBuilder` 方法的名稱或簽章。</span><span class="sxs-lookup"><span data-stu-id="dc124-123">If the app uses Entity Framework Core, don't change the name or signature of the `CreateHostBuilder` method.</span></span> <span data-ttu-id="dc124-124">[Entity Framework Core 工具](/ef/core/miscellaneous/cli/)預期找到 `CreateHostBuilder` 方法，其在不執行應用程式的情況下設定主機。</span><span class="sxs-lookup"><span data-stu-id="dc124-124">The [Entity Framework Core tools](/ef/core/miscellaneous/cli/) expect to find a `CreateHostBuilder` method that configures the host without running the app.</span></span> <span data-ttu-id="dc124-125">如需詳細資訊，請參閱[設計階段 DbContext 建立](/ef/core/miscellaneous/cli/dbcontext-creation)。</span><span class="sxs-lookup"><span data-stu-id="dc124-125">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

## <a name="default-builder-settings"></a><span data-ttu-id="dc124-126">預設建立器設定</span><span class="sxs-lookup"><span data-stu-id="dc124-126">Default builder settings</span></span>

<span data-ttu-id="dc124-127"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> 方法：</span><span class="sxs-lookup"><span data-stu-id="dc124-127">The <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> method:</span></span>

* <span data-ttu-id="dc124-128">將[內容根目錄](xref:fundamentals/index#content-root)設定為 <xref:System.IO.Directory.GetCurrentDirectory*>所傳回的路徑。</span><span class="sxs-lookup"><span data-stu-id="dc124-128">Sets the [content root](xref:fundamentals/index#content-root) to the path returned by <xref:System.IO.Directory.GetCurrentDirectory*>.</span></span>
* <span data-ttu-id="dc124-129">從下列項目載入主機組態：</span><span class="sxs-lookup"><span data-stu-id="dc124-129">Loads host configuration from:</span></span>
  * <span data-ttu-id="dc124-130">前面加上 "DOTNET_" 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="dc124-130">Environment variables prefixed with "DOTNET_".</span></span>
  * <span data-ttu-id="dc124-131">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="dc124-131">Command-line arguments.</span></span>
* <span data-ttu-id="dc124-132">從下列項目載入應用程式組態：</span><span class="sxs-lookup"><span data-stu-id="dc124-132">Loads app configuration from:</span></span>
  * <span data-ttu-id="dc124-133">*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="dc124-133">*appsettings.json*.</span></span>
  * <span data-ttu-id="dc124-134">*appsettings.{Environment}.json*</span><span class="sxs-lookup"><span data-stu-id="dc124-134">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="dc124-135">應用程式在 `Development` 環境中執行時的[祕密管理員](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="dc124-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="dc124-136">環境變數。</span><span class="sxs-lookup"><span data-stu-id="dc124-136">Environment variables.</span></span>
  * <span data-ttu-id="dc124-137">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="dc124-137">Command-line arguments.</span></span>
* <span data-ttu-id="dc124-138">新增下列[記錄](xref:fundamentals/logging/index)提供者：</span><span class="sxs-lookup"><span data-stu-id="dc124-138">Adds the following [logging](xref:fundamentals/logging/index) providers:</span></span>
  * <span data-ttu-id="dc124-139">主控台</span><span class="sxs-lookup"><span data-stu-id="dc124-139">Console</span></span>
  * <span data-ttu-id="dc124-140">偵錯</span><span class="sxs-lookup"><span data-stu-id="dc124-140">Debug</span></span>
  * <span data-ttu-id="dc124-141">EventSource</span><span class="sxs-lookup"><span data-stu-id="dc124-141">EventSource</span></span>
  * <span data-ttu-id="dc124-142">EventLog (僅當在 Windows 上執行時)</span><span class="sxs-lookup"><span data-stu-id="dc124-142">EventLog (only when running on Windows)</span></span>
* <span data-ttu-id="dc124-143">環境為開發時，會啟用[範圍驗證](xref:fundamentals/dependency-injection#scope-validation)和[相依性驗證](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild)。</span><span class="sxs-lookup"><span data-stu-id="dc124-143">Enables [scope validation](xref:fundamentals/dependency-injection#scope-validation) and [dependency validation](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) when the environment is Development.</span></span>

<span data-ttu-id="dc124-144">`ConfigureWebHostDefaults` 方法：</span><span class="sxs-lookup"><span data-stu-id="dc124-144">The `ConfigureWebHostDefaults` method:</span></span>

* <span data-ttu-id="dc124-145">會從環境變數中載入前置詞為 "ASPNETCORE_" 的主機組態。</span><span class="sxs-lookup"><span data-stu-id="dc124-145">Loads host configuration from environment variables prefixed with "ASPNETCORE_".</span></span>
* <span data-ttu-id="dc124-146">會將 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器設為網頁伺服器，並使用應用程式的主機組態提供者進行設定。</span><span class="sxs-lookup"><span data-stu-id="dc124-146">Sets [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and configures it using the app's hosting configuration providers.</span></span> <span data-ttu-id="dc124-147">如需 Kestrel 伺服器的預設選項，請參閱 <xref:fundamentals/servers/kestrel#kestrel-options>。</span><span class="sxs-lookup"><span data-stu-id="dc124-147">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="dc124-148">新增[主機篩選中介軟體](xref:fundamentals/servers/kestrel#host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="dc124-148">Adds [Host Filtering middleware](xref:fundamentals/servers/kestrel#host-filtering).</span></span>
* <span data-ttu-id="dc124-149">如果 ASPNETCORE_FORWARDEDHEADERS_ENABLED=true，則新增[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer#forwarded-headers)。</span><span class="sxs-lookup"><span data-stu-id="dc124-149">Adds [Forwarded Headers middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) if ASPNETCORE_FORWARDEDHEADERS_ENABLED=true.</span></span>
* <span data-ttu-id="dc124-150">啟用 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="dc124-150">Enables IIS integration.</span></span> <span data-ttu-id="dc124-151">如需 IIS 預設選項，請參閱 <xref:host-and-deploy/iis/index#iis-options>。</span><span class="sxs-lookup"><span data-stu-id="dc124-151">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>

<span data-ttu-id="dc124-152">本文稍後的[＜設定所有應用程式類型＞](#settings-for-all-app-types)和[＜Web 應用程式設定＞](#settings-for-web-apps)章節，將說明如何覆寫預設的建立器設定。</span><span class="sxs-lookup"><span data-stu-id="dc124-152">The [Settings for all app types](#settings-for-all-app-types) and [Settings for web apps](#settings-for-web-apps) sections later in this article show how to override default builder settings.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="dc124-153">架構提供的服務</span><span class="sxs-lookup"><span data-stu-id="dc124-153">Framework-provided services</span></span>

<span data-ttu-id="dc124-154">自動註冊的服務包括下列各項：</span><span class="sxs-lookup"><span data-stu-id="dc124-154">Services that are registered automatically include the following:</span></span>

* [<span data-ttu-id="dc124-155">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="dc124-155">IHostApplicationLifetime</span></span>](#ihostapplicationlifetime)
* [<span data-ttu-id="dc124-156">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="dc124-156">IHostLifetime</span></span>](#ihostlifetime)
* [<span data-ttu-id="dc124-157">IHostEnvironment / IWebHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="dc124-157">IHostEnvironment / IWebHostEnvironment</span></span>](#ihostenvironment)

<span data-ttu-id="dc124-158">如需架構所提供之服務的詳細資訊，請參閱 <xref:fundamentals/dependency-injection#framework-provided-services>。</span><span class="sxs-lookup"><span data-stu-id="dc124-158">For more information on framework-provided services, see <xref:fundamentals/dependency-injection#framework-provided-services>.</span></span>

## <a name="ihostapplicationlifetime"></a><span data-ttu-id="dc124-159">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="dc124-159">IHostApplicationLifetime</span></span>

<span data-ttu-id="dc124-160">將 <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (先前稱為 `IApplicationLifetime`) 服務插入任何類別來處理啟動後和順利關機工作。</span><span class="sxs-lookup"><span data-stu-id="dc124-160">Inject the <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (formerly `IApplicationLifetime`) service into any class to handle post-startup and graceful shutdown tasks.</span></span> <span data-ttu-id="dc124-161">介面上的三個屬性是用於註冊應用程式啟動和應用程式關閉事件處理程序方法的取消語彙基元。</span><span class="sxs-lookup"><span data-stu-id="dc124-161">Three properties on the interface are cancellation tokens used to register app start and app stop event handler methods.</span></span> <span data-ttu-id="dc124-162">介面也包括 `StopApplication` 方法。</span><span class="sxs-lookup"><span data-stu-id="dc124-162">The interface also includes a `StopApplication` method.</span></span>

<span data-ttu-id="dc124-163">下列範例是註冊 `IHostApplicationLifetime` 事件的 `IHostedService` 實作為：</span><span class="sxs-lookup"><span data-stu-id="dc124-163">The following example is an `IHostedService` implementation that registers `IHostApplicationLifetime` events:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a><span data-ttu-id="dc124-164">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="dc124-164">IHostLifetime</span></span>

<span data-ttu-id="dc124-165"><xref:Microsoft.Extensions.Hosting.IHostLifetime> 實作會控制主機啟動及停止的時機。</span><span class="sxs-lookup"><span data-stu-id="dc124-165">The <xref:Microsoft.Extensions.Hosting.IHostLifetime> implementation controls when the host starts and when it stops.</span></span> <span data-ttu-id="dc124-166">會使用最後一個註冊的實作。</span><span class="sxs-lookup"><span data-stu-id="dc124-166">The last implementation registered is used.</span></span>

<span data-ttu-id="dc124-167">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` 是預設的 `IHostLifetime` 實作。</span><span class="sxs-lookup"><span data-stu-id="dc124-167">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is the default `IHostLifetime` implementation.</span></span> <span data-ttu-id="dc124-168">`ConsoleLifetime`:</span><span class="sxs-lookup"><span data-stu-id="dc124-168">`ConsoleLifetime`:</span></span>

* <span data-ttu-id="dc124-169">接聽 Ctrl + C/SIGINT 或 SIGTERM，並呼叫 <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*> 以開始關機程式。</span><span class="sxs-lookup"><span data-stu-id="dc124-169">Listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*> to start the shutdown process.</span></span>
* <span data-ttu-id="dc124-170">會解除封鎖 [RunAsync](#runasync) 和 [WaitForShutdownAsync](#waitforshutdownasync) 等延伸模組。</span><span class="sxs-lookup"><span data-stu-id="dc124-170">Unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span>

## <a name="ihostenvironment"></a><span data-ttu-id="dc124-171">IHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="dc124-171">IHostEnvironment</span></span>

<span data-ttu-id="dc124-172">會將 <xref:Microsoft.Extensions.Hosting.IHostEnvironment> 服務插入類別以取得下列資訊：</span><span class="sxs-lookup"><span data-stu-id="dc124-172">Inject the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> service into a class to get information about the following:</span></span>

* [<span data-ttu-id="dc124-173">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="dc124-173">ApplicationName</span></span>](#applicationname)
* [<span data-ttu-id="dc124-174">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="dc124-174">EnvironmentName</span></span>](#environmentname)
* [<span data-ttu-id="dc124-175">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="dc124-175">ContentRootPath</span></span>](#contentrootpath)

<span data-ttu-id="dc124-176">Web apps 會執行 `IWebHostEnvironment` 介面，它會繼承 `IHostEnvironment` 並新增[WebRootPath](#webroot)。</span><span class="sxs-lookup"><span data-stu-id="dc124-176">Web apps implement the `IWebHostEnvironment` interface, which inherits `IHostEnvironment` and adds the [WebRootPath](#webroot).</span></span>

## <a name="host-configuration"></a><span data-ttu-id="dc124-177">主機組態</span><span class="sxs-lookup"><span data-stu-id="dc124-177">Host configuration</span></span>

<span data-ttu-id="dc124-178">主機組態用於 <xref:Microsoft.Extensions.Hosting.IHostEnvironment> 實作的屬性。</span><span class="sxs-lookup"><span data-stu-id="dc124-178">Host configuration is used for the properties of the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementation.</span></span>

<span data-ttu-id="dc124-179">主機組態位於 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 內的 [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration)。</span><span class="sxs-lookup"><span data-stu-id="dc124-179">Host configuration is available from [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) inside <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span></span> <span data-ttu-id="dc124-180">在 `ConfigureAppConfiguration` 之後，應用程式組態會取代 `HostBuilderContext.Configuration`。</span><span class="sxs-lookup"><span data-stu-id="dc124-180">After `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` is replaced with the app config.</span></span>

<span data-ttu-id="dc124-181">若要新增主機組態，請呼叫 `IHostBuilder` 上的 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>。</span><span class="sxs-lookup"><span data-stu-id="dc124-181">To add host configuration, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="dc124-182">`ConfigureHostConfiguration` 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="dc124-182">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="dc124-183">主機會使用指定索引鍵上最後設定值的任何選項。</span><span class="sxs-lookup"><span data-stu-id="dc124-183">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="dc124-184">CreateDefaultBuilder 包含具有前置詞 `DOTNET_` 的環境變數提供者和命令列引數。</span><span class="sxs-lookup"><span data-stu-id="dc124-184">The environment variable provider with prefix `DOTNET_` and command line args are included by CreateDefaultBuilder.</span></span> <span data-ttu-id="dc124-185">針對 Web 應用程式，會新增具有前置詞 `ASPNETCORE_` 的環境變數提供者。</span><span class="sxs-lookup"><span data-stu-id="dc124-185">For web apps, the environment variable provider with prefix `ASPNETCORE_` is added.</span></span> <span data-ttu-id="dc124-186">讀取環境變數時，就會移除前置詞。</span><span class="sxs-lookup"><span data-stu-id="dc124-186">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="dc124-187">例如，`ASPNETCORE_ENVIRONMENT` 的環境變數值會變成 `environment` 索引鍵的主機組態值。</span><span class="sxs-lookup"><span data-stu-id="dc124-187">For example, the environment variable value for `ASPNETCORE_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="dc124-188">下列範例會建立主機組態：</span><span class="sxs-lookup"><span data-stu-id="dc124-188">The following example creates host configuration:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a><span data-ttu-id="dc124-189">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="dc124-189">App configuration</span></span>

<span data-ttu-id="dc124-190">應用程式組態的建立方式是在 `IHostBuilder` 上呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>。</span><span class="sxs-lookup"><span data-stu-id="dc124-190">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="dc124-191">`ConfigureAppConfiguration` 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="dc124-191">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="dc124-192">應用程式會使用指定索引鍵上最後設定值的任何選項。</span><span class="sxs-lookup"><span data-stu-id="dc124-192">The app uses whichever option sets a value last on a given key.</span></span> 

<span data-ttu-id="dc124-193">由 `ConfigureAppConfiguration` 建立的組態位於 [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*)，可用於後續作業並用作 DI 中的服務。</span><span class="sxs-lookup"><span data-stu-id="dc124-193">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and as a service from DI.</span></span> <span data-ttu-id="dc124-194">主機組態也會新增至應用程式組態。</span><span class="sxs-lookup"><span data-stu-id="dc124-194">The host configuration is also added to the app configuration.</span></span>

<span data-ttu-id="dc124-195">如需詳細資訊，請參閱 [ASP.NET Core 中的組態](xref:fundamentals/configuration/index#configureappconfiguration)。</span><span class="sxs-lookup"><span data-stu-id="dc124-195">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span></span>

## <a name="settings-for-all-app-types"></a><span data-ttu-id="dc124-196">所有應用程式類型的設定</span><span class="sxs-lookup"><span data-stu-id="dc124-196">Settings for all app types</span></span>

<span data-ttu-id="dc124-197">本節列出適用於 HTTP 和非 HTTP 工作負載的主機設定。</span><span class="sxs-lookup"><span data-stu-id="dc124-197">This section lists host settings that apply to both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="dc124-198">根據預設，用來設定這些設定的環境變數可以具有 `DOTNET_` 或 `ASPNETCORE_` 前置詞。</span><span class="sxs-lookup"><span data-stu-id="dc124-198">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<!-- In the following sections, two spaces at end of line are used to force line breaks in the rendered page. -->

### <a name="applicationname"></a><span data-ttu-id="dc124-199">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="dc124-199">ApplicationName</span></span>

<span data-ttu-id="dc124-200">[IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) 屬性是在主機建構期間從主機組態當中設定。</span><span class="sxs-lookup"><span data-stu-id="dc124-200">The [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span>

<span data-ttu-id="dc124-201">**索引鍵**：applicationName</span><span class="sxs-lookup"><span data-stu-id="dc124-201">**Key**: applicationName</span></span>  
<span data-ttu-id="dc124-202">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="dc124-202">**Type**: *string*</span></span>  
<span data-ttu-id="dc124-203">**預設值**：包含應用程式進入點的元件名稱。</span><span class="sxs-lookup"><span data-stu-id="dc124-203">**Default**: The name of the assembly that contains the app's entry point.</span></span>
<span data-ttu-id="dc124-204">**環境變數**：`<PREFIX_>APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="dc124-204">**Environment variable**: `<PREFIX_>APPLICATIONNAME`</span></span>

<span data-ttu-id="dc124-205">若要設定此值，請使用環境變數。</span><span class="sxs-lookup"><span data-stu-id="dc124-205">To set this value, use the environment variable.</span></span> 

### <a name="contentrootpath"></a><span data-ttu-id="dc124-206">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="dc124-206">ContentRootPath</span></span>

<span data-ttu-id="dc124-207">[IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) 屬性會決定主機從哪裡開始搜尋內容檔案。</span><span class="sxs-lookup"><span data-stu-id="dc124-207">The [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) property determines where the host begins searching for content files.</span></span> <span data-ttu-id="dc124-208">如果路徑不存在，就無法啟動主機。</span><span class="sxs-lookup"><span data-stu-id="dc124-208">If the path doesn't exist, the host fails to start.</span></span>

<span data-ttu-id="dc124-209">**索引鍵**：contentRoot</span><span class="sxs-lookup"><span data-stu-id="dc124-209">**Key**: contentRoot</span></span>  
<span data-ttu-id="dc124-210">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="dc124-210">**Type**: *string*</span></span>  
<span data-ttu-id="dc124-211">**預設值**：應用程式元件所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="dc124-211">**Default**: The folder where the app assembly resides.</span></span>  
<span data-ttu-id="dc124-212">**環境變數**：`<PREFIX_>CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="dc124-212">**Environment variable**: `<PREFIX_>CONTENTROOT`</span></span>

<span data-ttu-id="dc124-213">若要設定此值，請使用環境變數或呼叫 `IHostBuilder` 上的 `UseContentRoot`：</span><span class="sxs-lookup"><span data-stu-id="dc124-213">To set this value, use the environment variable or call `UseContentRoot` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

<span data-ttu-id="dc124-214">如需詳細資訊，請參閱＜＞。</span><span class="sxs-lookup"><span data-stu-id="dc124-214">For more information, see:</span></span>

* [<span data-ttu-id="dc124-215">基本概念：內容根目錄</span><span class="sxs-lookup"><span data-stu-id="dc124-215">Fundamentals: Content root</span></span>](xref:fundamentals/index#content-root)
* [<span data-ttu-id="dc124-216">WebRoot</span><span class="sxs-lookup"><span data-stu-id="dc124-216">WebRoot</span></span>](#webroot)

### <a name="environmentname"></a><span data-ttu-id="dc124-217">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="dc124-217">EnvironmentName</span></span>

<span data-ttu-id="dc124-218">[IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) 屬性可以設為任何值。</span><span class="sxs-lookup"><span data-stu-id="dc124-218">The [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) property can be set to any value.</span></span> <span data-ttu-id="dc124-219">架構定義的值包括 `Development`、`Staging` 和 `Production`。</span><span class="sxs-lookup"><span data-stu-id="dc124-219">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="dc124-220">值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="dc124-220">Values aren't case sensitive.</span></span>

<span data-ttu-id="dc124-221">**索引鍵**：environment</span><span class="sxs-lookup"><span data-stu-id="dc124-221">**Key**: environment</span></span>  
<span data-ttu-id="dc124-222">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="dc124-222">**Type**: *string*</span></span>  
<span data-ttu-id="dc124-223">**預設值**：Production</span><span class="sxs-lookup"><span data-stu-id="dc124-223">**Default**: Production</span></span>  
<span data-ttu-id="dc124-224">**環境變數**：`<PREFIX_>ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="dc124-224">**Environment variable**: `<PREFIX_>ENVIRONMENT`</span></span>

<span data-ttu-id="dc124-225">若要設定此值，請使用環境變數或呼叫 `IHostBuilder` 上的 `UseEnvironment`：</span><span class="sxs-lookup"><span data-stu-id="dc124-225">To set this value, use the environment variable or call `UseEnvironment` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a><span data-ttu-id="dc124-226">ShutdownTimeout</span><span class="sxs-lookup"><span data-stu-id="dc124-226">ShutdownTimeout</span></span>

<span data-ttu-id="dc124-227">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) 會設定 <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> 的逾時。</span><span class="sxs-lookup"><span data-stu-id="dc124-227">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="dc124-228">預設值是五秒鐘。</span><span class="sxs-lookup"><span data-stu-id="dc124-228">The default value is five seconds.</span></span>  <span data-ttu-id="dc124-229">在逾時期間，主機會：</span><span class="sxs-lookup"><span data-stu-id="dc124-229">During the timeout period, the host:</span></span>

* <span data-ttu-id="dc124-230">觸發 [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.ihostapplicationlifetime.applicationstopping)。</span><span class="sxs-lookup"><span data-stu-id="dc124-230">Triggers [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.ihostapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="dc124-231">嘗試停止託管的服務，並記錄無法停止之服務的錯誤。</span><span class="sxs-lookup"><span data-stu-id="dc124-231">Attempts to stop hosted services, logging errors for services that fail to stop.</span></span>

<span data-ttu-id="dc124-232">如果在所有的託管服務停止之前逾時期限已到期，則應用程式關閉時，會停止任何剩餘的作用中服務。</span><span class="sxs-lookup"><span data-stu-id="dc124-232">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="dc124-233">即使服務尚未完成處理也會停止。</span><span class="sxs-lookup"><span data-stu-id="dc124-233">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="dc124-234">如果服務需要更多時間才能停止，請增加逾時。</span><span class="sxs-lookup"><span data-stu-id="dc124-234">If services require additional time to stop, increase the timeout.</span></span>

<span data-ttu-id="dc124-235">**索引鍵**：shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="dc124-235">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="dc124-236">**類型**：*int*</span><span class="sxs-lookup"><span data-stu-id="dc124-236">**Type**: *int*</span></span>  
<span data-ttu-id="dc124-237">**預設值**：5秒的**環境變數**： `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="dc124-237">**Default**: 5 seconds **Environment variable**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="dc124-238">若要設定此值，請使用環境變數或設定 `HostOptions`。</span><span class="sxs-lookup"><span data-stu-id="dc124-238">To set this value, use the environment variable or configure `HostOptions`.</span></span> <span data-ttu-id="dc124-239">下列範例將逾時設為 20 秒：</span><span class="sxs-lookup"><span data-stu-id="dc124-239">The following example sets the timeout to 20 seconds:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

## <a name="settings-for-web-apps"></a><span data-ttu-id="dc124-240">Web 應用程式的設定</span><span class="sxs-lookup"><span data-stu-id="dc124-240">Settings for web apps</span></span>

<span data-ttu-id="dc124-241">某些主機設定僅適用於 HTTP 工作負載。</span><span class="sxs-lookup"><span data-stu-id="dc124-241">Some host settings apply only to HTTP workloads.</span></span> <span data-ttu-id="dc124-242">根據預設，用來設定這些設定的環境變數可以具有 `DOTNET_` 或 `ASPNETCORE_` 前置詞。</span><span class="sxs-lookup"><span data-stu-id="dc124-242">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<span data-ttu-id="dc124-243">`IWebHostBuilder` 上的擴充方法適用於這些設定。</span><span class="sxs-lookup"><span data-stu-id="dc124-243">Extension methods on `IWebHostBuilder` are available for these settings.</span></span> <span data-ttu-id="dc124-244">示範如何呼叫擴充方法的程式碼範例假設 `webBuilder` 是 `IWebHostBuilder` 的執行個體，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="dc124-244">Code samples that show how to call the extension methods assume `webBuilder` is an instance of `IWebHostBuilder`, as in the following example:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a><span data-ttu-id="dc124-245">CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="dc124-245">CaptureStartupErrors</span></span>

<span data-ttu-id="dc124-246">當它為 `false` 時，啟動期間發生的錯誤會導致主機結束。</span><span class="sxs-lookup"><span data-stu-id="dc124-246">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="dc124-247">當它為 `true` 時，主機會擷取啟動期間的例外狀況，並嘗試啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="dc124-247">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

<span data-ttu-id="dc124-248">**索引鍵**：captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="dc124-248">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="dc124-249">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="dc124-249">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="dc124-250">**預設值**：預設為 `false`，除非應用程式執行時在 IIS 背後有 Kestrel，此時預設值即為 `true`。</span><span class="sxs-lookup"><span data-stu-id="dc124-250">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="dc124-251">**環境變數**：`<PREFIX_>CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="dc124-251">**Environment variable**: `<PREFIX_>CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="dc124-252">若要設定此值，請使用組態或呼叫 `CaptureStartupErrors`：</span><span class="sxs-lookup"><span data-stu-id="dc124-252">To set this value, use configuration or call `CaptureStartupErrors`:</span></span>

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a><span data-ttu-id="dc124-253">DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="dc124-253">DetailedErrors</span></span>

<span data-ttu-id="dc124-254">啟用時 (或當環境為 `Development` 時)，應用程式會擷取詳細錯誤。</span><span class="sxs-lookup"><span data-stu-id="dc124-254">When enabled, or when the environment is `Development`, the app captures detailed errors.</span></span>

<span data-ttu-id="dc124-255">**索引鍵**：detailedErrors</span><span class="sxs-lookup"><span data-stu-id="dc124-255">**Key**: detailedErrors</span></span>  
<span data-ttu-id="dc124-256">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="dc124-256">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="dc124-257">**預設值**：false</span><span class="sxs-lookup"><span data-stu-id="dc124-257">**Default**: false</span></span>  
<span data-ttu-id="dc124-258">**環境變數**：`<PREFIX_>_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="dc124-258">**Environment variable**: `<PREFIX_>_DETAILEDERRORS`</span></span>

<span data-ttu-id="dc124-259">若要設定此值，請使用組態或呼叫 `UseSetting`：</span><span class="sxs-lookup"><span data-stu-id="dc124-259">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a><span data-ttu-id="dc124-260">HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="dc124-260">HostingStartupAssemblies</span></span>

<span data-ttu-id="dc124-261">在啟動時載入的裝載啟動組件字串，以分號分隔。</span><span class="sxs-lookup"><span data-stu-id="dc124-261">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="dc124-262">雖然設定值會預設為空字串，但裝載啟動組件一律會包含應用程式的組件。</span><span class="sxs-lookup"><span data-stu-id="dc124-262">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="dc124-263">提供裝載啟動組件時，它們會新增至應用程式的組件，以便在應用程式在啟動時建置其通用服務時載入。</span><span class="sxs-lookup"><span data-stu-id="dc124-263">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

<span data-ttu-id="dc124-264">**索引鍵**：hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="dc124-264">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="dc124-265">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="dc124-265">**Type**: *string*</span></span>  
<span data-ttu-id="dc124-266">**預設值**：空字串</span><span class="sxs-lookup"><span data-stu-id="dc124-266">**Default**: Empty string</span></span>  
<span data-ttu-id="dc124-267">**環境變數**：`<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="dc124-267">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="dc124-268">若要設定此值，請使用組態或呼叫 `UseSetting`：</span><span class="sxs-lookup"><span data-stu-id="dc124-268">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a><span data-ttu-id="dc124-269">HostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="dc124-269">HostingStartupExcludeAssemblies</span></span>

<span data-ttu-id="dc124-270">在啟動時排除以分號分隔的裝載啟動組件字串。</span><span class="sxs-lookup"><span data-stu-id="dc124-270">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="dc124-271">**索引鍵**：hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="dc124-271">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="dc124-272">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="dc124-272">**Type**: *string*</span></span>  
<span data-ttu-id="dc124-273">**預設值**：空字串</span><span class="sxs-lookup"><span data-stu-id="dc124-273">**Default**: Empty string</span></span>  
<span data-ttu-id="dc124-274">**環境變數**：`<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="dc124-274">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="dc124-275">若要設定此值，請使用組態或呼叫 `UseSetting`：</span><span class="sxs-lookup"><span data-stu-id="dc124-275">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="https_port"></a><span data-ttu-id="dc124-276">HTTPS_Port</span><span class="sxs-lookup"><span data-stu-id="dc124-276">HTTPS_Port</span></span>

<span data-ttu-id="dc124-277">HTTPS 重新導向連接埠。</span><span class="sxs-lookup"><span data-stu-id="dc124-277">The HTTPS redirect port.</span></span> <span data-ttu-id="dc124-278">用於[強制 HTTPS](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="dc124-278">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="dc124-279">機**碼**： HTTPs_port</span><span class="sxs-lookup"><span data-stu-id="dc124-279">**Key**: https_port</span></span>  
<span data-ttu-id="dc124-280">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="dc124-280">**Type**: *string*</span></span>  
<span data-ttu-id="dc124-281">**預設**值：未設定預設值。</span><span class="sxs-lookup"><span data-stu-id="dc124-281">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="dc124-282">**環境變數**：`<PREFIX_>HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="dc124-282">**Environment variable**: `<PREFIX_>HTTPS_PORT`</span></span>

<span data-ttu-id="dc124-283">若要設定此值，請使用組態或呼叫 `UseSetting`：</span><span class="sxs-lookup"><span data-stu-id="dc124-283">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a><span data-ttu-id="dc124-284">PreferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="dc124-284">PreferHostingUrls</span></span>

<span data-ttu-id="dc124-285">表示主機是否應接聽使用 `IWebHostBuilder` 設定的 URL，而不是 `IServer` 實作所設定的 URL。</span><span class="sxs-lookup"><span data-stu-id="dc124-285">Indicates whether the host should listen on the URLs configured with the `IWebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="dc124-286">**索引鍵**：preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="dc124-286">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="dc124-287">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="dc124-287">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="dc124-288">**預設值**：true</span><span class="sxs-lookup"><span data-stu-id="dc124-288">**Default**: true</span></span>  
<span data-ttu-id="dc124-289">**環境變數**：`<PREFIX_>_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="dc124-289">**Environment variable**: `<PREFIX_>_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="dc124-290">若要設定此值，請使用環境變數或呼叫 `PreferHostingUrls`：</span><span class="sxs-lookup"><span data-stu-id="dc124-290">To set this value, use the environment variable or call `PreferHostingUrls`:</span></span>

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a><span data-ttu-id="dc124-291">PreventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="dc124-291">PreventHostingStartup</span></span>

<span data-ttu-id="dc124-292">可防止自動載入裝載啟動組件，包括應用程式組件所設定的裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="dc124-292">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="dc124-293">如需詳細資訊，請參閱<xref:fundamentals/configuration/platform-specific-configuration>。</span><span class="sxs-lookup"><span data-stu-id="dc124-293">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="dc124-294">**索引鍵**preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="dc124-294">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="dc124-295">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="dc124-295">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="dc124-296">**預設值**：false</span><span class="sxs-lookup"><span data-stu-id="dc124-296">**Default**: false</span></span>  
<span data-ttu-id="dc124-297">**環境變數**：`<PREFIX_>_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="dc124-297">**Environment variable**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="dc124-298">若要設定此值，請使用環境變數或呼叫 `UseSetting`：</span><span class="sxs-lookup"><span data-stu-id="dc124-298">To set this value, use the environment variable or call `UseSetting` :</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a><span data-ttu-id="dc124-299">StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="dc124-299">StartupAssembly</span></span>

<span data-ttu-id="dc124-300">要搜尋 `Startup` 類別的組件。</span><span class="sxs-lookup"><span data-stu-id="dc124-300">The assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="dc124-301">**索引鍵**：startupAssembly</span><span class="sxs-lookup"><span data-stu-id="dc124-301">**Key**: startupAssembly</span></span>  
<span data-ttu-id="dc124-302">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="dc124-302">**Type**: *string*</span></span>  
<span data-ttu-id="dc124-303">**預設值**：應用程式的組件</span><span class="sxs-lookup"><span data-stu-id="dc124-303">**Default**: The app's assembly</span></span>  
<span data-ttu-id="dc124-304">**環境變數**：`<PREFIX_>STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="dc124-304">**Environment variable**: `<PREFIX_>STARTUPASSEMBLY`</span></span>

<span data-ttu-id="dc124-305">若要設定此值，請使用環境變數或呼叫 `UseStartup`。</span><span class="sxs-lookup"><span data-stu-id="dc124-305">To set this value, use the environment variable or call `UseStartup`.</span></span> <span data-ttu-id="dc124-306">`UseStartup` 可以採用組件名稱 (`string`) 或類型 (`TStartup`)。</span><span class="sxs-lookup"><span data-stu-id="dc124-306">`UseStartup` can take an assembly name (`string`) or a type (`TStartup`).</span></span> <span data-ttu-id="dc124-307">如果呼叫多個 `UseStartup` 方法，最後一個將會優先。</span><span class="sxs-lookup"><span data-stu-id="dc124-307">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a><span data-ttu-id="dc124-308">URL</span><span class="sxs-lookup"><span data-stu-id="dc124-308">URLs</span></span>

<span data-ttu-id="dc124-309">以分號分隔的 IP 位址或主機位址，包含伺服器應接聽要求的連接埠和通訊協定。</span><span class="sxs-lookup"><span data-stu-id="dc124-309">A semicolon-delimited list of IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span> <span data-ttu-id="dc124-310">例如，`http://localhost:123`。</span><span class="sxs-lookup"><span data-stu-id="dc124-310">For example, `http://localhost:123`.</span></span> <span data-ttu-id="dc124-311">使用 "\*"，表示伺服器應接聽任何 IP 位址或主機名稱上的要求，並使用指定的連接埠和通訊協定 (例如，`http://*:5000`)。</span><span class="sxs-lookup"><span data-stu-id="dc124-311">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="dc124-312">通訊協定 (`http://` 或 `https://`) 必須包含在每個 URL 中。</span><span class="sxs-lookup"><span data-stu-id="dc124-312">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="dc124-313">支援的格式會依伺服器而有所不同。</span><span class="sxs-lookup"><span data-stu-id="dc124-313">Supported formats vary among servers.</span></span>

<span data-ttu-id="dc124-314">**索引鍵**：urls</span><span class="sxs-lookup"><span data-stu-id="dc124-314">**Key**: urls</span></span>  
<span data-ttu-id="dc124-315">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="dc124-315">**Type**: *string*</span></span>  
<span data-ttu-id="dc124-316">**預設值**： `http://localhost:5000` 和 `https://localhost:5001`</span><span class="sxs-lookup"><span data-stu-id="dc124-316">**Default**: `http://localhost:5000` and `https://localhost:5001`</span></span>  
<span data-ttu-id="dc124-317">**環境變數**：`<PREFIX_>URLS`</span><span class="sxs-lookup"><span data-stu-id="dc124-317">**Environment variable**: `<PREFIX_>URLS`</span></span>

<span data-ttu-id="dc124-318">若要設定此值，請使用環境變數或呼叫 `UseUrls`：</span><span class="sxs-lookup"><span data-stu-id="dc124-318">To set this value, use the environment variable or call `UseUrls`:</span></span>

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

<span data-ttu-id="dc124-319">Kestrel 有它自己的端點設定 API。</span><span class="sxs-lookup"><span data-stu-id="dc124-319">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="dc124-320">如需詳細資訊，請參閱<xref:fundamentals/servers/kestrel#endpoint-configuration>。</span><span class="sxs-lookup"><span data-stu-id="dc124-320">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="webroot"></a><span data-ttu-id="dc124-321">WebRoot</span><span class="sxs-lookup"><span data-stu-id="dc124-321">WebRoot</span></span>

<span data-ttu-id="dc124-322">應用程式靜態資產的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="dc124-322">The relative path to the app's static assets.</span></span>

<span data-ttu-id="dc124-323">**索引鍵**：webroot</span><span class="sxs-lookup"><span data-stu-id="dc124-323">**Key**: webroot</span></span>  
<span data-ttu-id="dc124-324">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="dc124-324">**Type**: *string*</span></span>  
<span data-ttu-id="dc124-325">**預設**值：預設值為 `wwwroot`。</span><span class="sxs-lookup"><span data-stu-id="dc124-325">**Default**: The default is `wwwroot`.</span></span> <span data-ttu-id="dc124-326">*{Content root}/wwwroot*的路徑必須存在。</span><span class="sxs-lookup"><span data-stu-id="dc124-326">The path to *{content root}/wwwroot* must exist.</span></span> <span data-ttu-id="dc124-327">如果路徑不存在，則會使用無作業檔案提供者。</span><span class="sxs-lookup"><span data-stu-id="dc124-327">If the path doesn't exist, a no-op file provider is used.</span></span>  
<span data-ttu-id="dc124-328">**環境變數**：`<PREFIX_>WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="dc124-328">**Environment variable**: `<PREFIX_>WEBROOT`</span></span>

<span data-ttu-id="dc124-329">若要設定此值，請使用環境變數或呼叫 `UseWebRoot`：</span><span class="sxs-lookup"><span data-stu-id="dc124-329">To set this value, use the environment variable or call `UseWebRoot`:</span></span>

```csharp
webBuilder.UseWebRoot("public");
```

<span data-ttu-id="dc124-330">如需詳細資訊，請參閱＜＞。</span><span class="sxs-lookup"><span data-stu-id="dc124-330">For more information, see:</span></span>

* [<span data-ttu-id="dc124-331">基本概念： Web 根目錄</span><span class="sxs-lookup"><span data-stu-id="dc124-331">Fundamentals: Web root</span></span>](xref:fundamentals/index#web-root)
* [<span data-ttu-id="dc124-332">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="dc124-332">ContentRootPath</span></span>](#contentrootpath)

## <a name="manage-the-host-lifetime"></a><span data-ttu-id="dc124-333">管理主機存留期</span><span class="sxs-lookup"><span data-stu-id="dc124-333">Manage the host lifetime</span></span>

<span data-ttu-id="dc124-334">在建置的 <xref:Microsoft.Extensions.Hosting.IHost> 實作上呼叫方法來啟動和停止應用程式。</span><span class="sxs-lookup"><span data-stu-id="dc124-334">Call methods on the built <xref:Microsoft.Extensions.Hosting.IHost> implementation to start and stop the app.</span></span> <span data-ttu-id="dc124-335">這些方法會影響所有在服務容器中註冊的 <xref:Microsoft.Extensions.Hosting.IHostedService> 實作。</span><span class="sxs-lookup"><span data-stu-id="dc124-335">These methods affect all  <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="dc124-336">執行</span><span class="sxs-lookup"><span data-stu-id="dc124-336">Run</span></span>

<span data-ttu-id="dc124-337"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> 會執行應用程式並封鎖呼叫執行緒，直到主機關閉為止。</span><span class="sxs-lookup"><span data-stu-id="dc124-337"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down.</span></span>

### <a name="runasync"></a><span data-ttu-id="dc124-338">RunAsync</span><span class="sxs-lookup"><span data-stu-id="dc124-338">RunAsync</span></span>

<span data-ttu-id="dc124-339"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> 會執行應用程式，並傳回觸發取消語彙基元或關機時所完成的 <xref:System.Threading.Tasks.Task>。</span><span class="sxs-lookup"><span data-stu-id="dc124-339"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span>

### <a name="runconsoleasync"></a><span data-ttu-id="dc124-340">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="dc124-340">RunConsoleAsync</span></span>

<span data-ttu-id="dc124-341"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> 會啟用主控台支援、建置和啟動主機，以及等候 Ctrl+C/SIGINT 或 SIGTERM 關機。</span><span class="sxs-lookup"><span data-stu-id="dc124-341"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

### <a name="start"></a><span data-ttu-id="dc124-342">啟動</span><span class="sxs-lookup"><span data-stu-id="dc124-342">Start</span></span>

<span data-ttu-id="dc124-343"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> 會同步啟動主機。</span><span class="sxs-lookup"><span data-stu-id="dc124-343"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

### <a name="startasync"></a><span data-ttu-id="dc124-344">StartAsync</span><span class="sxs-lookup"><span data-stu-id="dc124-344">StartAsync</span></span>

<span data-ttu-id="dc124-345"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> 會啟動主機，並傳回觸發取消語彙基元或關機時所完成的 <xref:System.Threading.Tasks.Task>。</span><span class="sxs-lookup"><span data-stu-id="dc124-345"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the host and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span> 

<span data-ttu-id="dc124-346"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> 在 `StartAsync` 開始時呼叫，並等到完成後再繼續進行。</span><span class="sxs-lookup"><span data-stu-id="dc124-346"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of `StartAsync`, which waits until it's complete before continuing.</span></span> <span data-ttu-id="dc124-347">這可用來將啟動延遲到外部事件發出訊號為止。</span><span class="sxs-lookup"><span data-stu-id="dc124-347">This can be used to delay startup until signaled by an external event.</span></span>

### <a name="stopasync"></a><span data-ttu-id="dc124-348">StopAsync</span><span class="sxs-lookup"><span data-stu-id="dc124-348">StopAsync</span></span>

<span data-ttu-id="dc124-349"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> 會嘗試在提供的逾時內停止主機。</span><span class="sxs-lookup"><span data-stu-id="dc124-349"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

### <a name="waitforshutdown"></a><span data-ttu-id="dc124-350">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="dc124-350">WaitForShutdown</span></span>

<span data-ttu-id="dc124-351"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> 會封鎖呼叫執行緒，直到 IHostLifetime 觸發關閉為止，例如透過 CTRL-C/SIGINT 或 SIGTERM。</span><span class="sxs-lookup"><span data-stu-id="dc124-351"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blocks the calling thread until shutdown is triggered by the IHostLifetime, such as via Ctrl+C/SIGINT or SIGTERM.</span></span>

### <a name="waitforshutdownasync"></a><span data-ttu-id="dc124-352">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="dc124-352">WaitForShutdownAsync</span></span>

<span data-ttu-id="dc124-353"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> 會傳回透過指定語彙基元觸發關機時所完成的 <xref:System.Threading.Tasks.Task>，並呼叫 <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>。</span><span class="sxs-lookup"><span data-stu-id="dc124-353"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

### <a name="external-control"></a><span data-ttu-id="dc124-354">外部控制</span><span class="sxs-lookup"><span data-stu-id="dc124-354">External control</span></span>

<span data-ttu-id="dc124-355">主機存留期直接控制可以使用可從外部呼叫的方法來達成：</span><span class="sxs-lookup"><span data-stu-id="dc124-355">Direct control of the host lifetime can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="dc124-356">ASP.NET Core 應用程式會設定並啟動主機。</span><span class="sxs-lookup"><span data-stu-id="dc124-356">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="dc124-357">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="dc124-357">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="dc124-358">本文所涵蓋的 ASP.NET Core 泛型主機 (<xref:Microsoft.Extensions.Hosting.HostBuilder>)，可用來不會處理 HTTP 要求的應用程式。</span><span class="sxs-lookup"><span data-stu-id="dc124-358">This article covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is used for apps that don't process HTTP requests.</span></span>

<span data-ttu-id="dc124-359">泛型主機的用途是將 HTTP 管線從 Web 主機 API 分離，以允許更廣泛的主機陣列案例。</span><span class="sxs-lookup"><span data-stu-id="dc124-359">The purpose of Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="dc124-360">傳訊、背景工作及其他以泛型主機為基礎的非 HTTP 工作負載都受益於跨領域的功能，例如設定、相依性插入 (DI) 和記錄。</span><span class="sxs-lookup"><span data-stu-id="dc124-360">Messaging, background tasks, and other non-HTTP workloads based on Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="dc124-361">泛型主機是 ASP.NET Core 2.1 的新功能，不適用於虛擬主機案例。</span><span class="sxs-lookup"><span data-stu-id="dc124-361">Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="dc124-362">針對虛擬主機案例，請使用 [Web 主機](xref:fundamentals/host/web-host)。</span><span class="sxs-lookup"><span data-stu-id="dc124-362">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="dc124-363">泛型主機將在未來版本中取代 Web 主機，並成為 HTTP 和非 HTTP 案例中的主要主機 API。</span><span class="sxs-lookup"><span data-stu-id="dc124-363">Generic Host will replace Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="dc124-364">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dc124-364">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="dc124-365">在 [Visual Studio Code](https://code.visualstudio.com/) 中執行範例應用程式時，請使用「外部或整合式終端機」。</span><span class="sxs-lookup"><span data-stu-id="dc124-365">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="dc124-366">請勿在 `internalConsole` 中執行範例。</span><span class="sxs-lookup"><span data-stu-id="dc124-366">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="dc124-367">若要在 Visual Studio Code 中設定主控台：</span><span class="sxs-lookup"><span data-stu-id="dc124-367">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="dc124-368">開啟 *.vscode/launch.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="dc124-368">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="dc124-369">在 [.NET Core Launch (console)] \(.NET Core 啟動 (主控台)\) 設定中，尋找 **console** 項目。</span><span class="sxs-lookup"><span data-stu-id="dc124-369">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="dc124-370">將此值設定為 `externalTerminal` 或 `integratedTerminal`。</span><span class="sxs-lookup"><span data-stu-id="dc124-370">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="dc124-371">簡介</span><span class="sxs-lookup"><span data-stu-id="dc124-371">Introduction</span></span>

<span data-ttu-id="dc124-372">可使用位於 <xref:Microsoft.Extensions.Hosting> 命名空間的泛型主機程式庫，且該程式庫由 [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) 套件提供。</span><span class="sxs-lookup"><span data-stu-id="dc124-372">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="dc124-373">[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 或更新版本) 包含 `Microsoft.Extensions.Hosting` 套件。</span><span class="sxs-lookup"><span data-stu-id="dc124-373">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="dc124-374"><xref:Microsoft.Extensions.Hosting.IHostedService> 是程式碼執行的進入點。</span><span class="sxs-lookup"><span data-stu-id="dc124-374"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="dc124-375">每個 <xref:Microsoft.Extensions.Hosting.IHostedService> 實作會依 [ConfigureServices 中的服務註冊](#configureservices)順序執行。</span><span class="sxs-lookup"><span data-stu-id="dc124-375">Each <xref:Microsoft.Extensions.Hosting.IHostedService> implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="dc124-376">當主機啟動時，會在每個 <xref:Microsoft.Extensions.Hosting.IHostedService> 上呼叫 <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*>；當主機順利關閉時，則會依反向註冊順序呼叫 <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*>。</span><span class="sxs-lookup"><span data-stu-id="dc124-376"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each <xref:Microsoft.Extensions.Hosting.IHostedService> when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="dc124-377">設定主機</span><span class="sxs-lookup"><span data-stu-id="dc124-377">Set up a host</span></span>

<span data-ttu-id="dc124-378"><xref:Microsoft.Extensions.Hosting.IHostBuilder> 是程式庫和應用程式用來初始化、建置及執行主機的主要元件：</span><span class="sxs-lookup"><span data-stu-id="dc124-378"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a><span data-ttu-id="dc124-379">選項</span><span class="sxs-lookup"><span data-stu-id="dc124-379">Options</span></span>

<span data-ttu-id="dc124-380"><xref:Microsoft.Extensions.Hosting.IHost> 的 <xref:Microsoft.Extensions.Hosting.HostOptions> 設定選項。</span><span class="sxs-lookup"><span data-stu-id="dc124-380"><xref:Microsoft.Extensions.Hosting.HostOptions> configure options for the <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="dc124-381">關機逾時</span><span class="sxs-lookup"><span data-stu-id="dc124-381">Shutdown timeout</span></span>

<span data-ttu-id="dc124-382"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> 會為 <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> 設定逾時。</span><span class="sxs-lookup"><span data-stu-id="dc124-382"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="dc124-383">預設值是五秒鐘。</span><span class="sxs-lookup"><span data-stu-id="dc124-383">The default value is five seconds.</span></span>

<span data-ttu-id="dc124-384">`Program.Main` 中的下列選項設定可將預設的五秒鐘關機逾時增加到 20 秒：</span><span class="sxs-lookup"><span data-stu-id="dc124-384">The following option configuration in `Program.Main` increases the default five second shutdown timeout to 20 seconds:</span></span>

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

## <a name="default-services"></a><span data-ttu-id="dc124-385">預設服務</span><span class="sxs-lookup"><span data-stu-id="dc124-385">Default services</span></span>

<span data-ttu-id="dc124-386">下列服務會在主機初始化期間註冊：</span><span class="sxs-lookup"><span data-stu-id="dc124-386">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="dc124-387">[環境](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="dc124-387">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="dc124-388">[組態](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="dc124-388">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="dc124-389"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (`Microsoft.Extensions.Hosting.Internal.ApplicationLifetime`)</span><span class="sxs-lookup"><span data-stu-id="dc124-389"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (`Microsoft.Extensions.Hosting.Internal.ApplicationLifetime`)</span></span>
* <span data-ttu-id="dc124-390"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime`)</span><span class="sxs-lookup"><span data-stu-id="dc124-390"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime`)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="dc124-391">[選項](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="dc124-391">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="dc124-392">[記錄](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="dc124-392">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="dc124-393">主機組態</span><span class="sxs-lookup"><span data-stu-id="dc124-393">Host configuration</span></span>

<span data-ttu-id="dc124-394">主機組態的建立方式：</span><span class="sxs-lookup"><span data-stu-id="dc124-394">Host configuration is created by:</span></span>

* <span data-ttu-id="dc124-395">呼叫 <xref:Microsoft.Extensions.Hosting.IHostBuilder> 上的擴充方法，來設定[內容根目錄](#content-root)及[環境](#environment)。</span><span class="sxs-lookup"><span data-stu-id="dc124-395">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="dc124-396">在 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> 中從組態提供者讀取組態。</span><span class="sxs-lookup"><span data-stu-id="dc124-396">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="dc124-397">擴充方法</span><span class="sxs-lookup"><span data-stu-id="dc124-397">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="dc124-398">應用程式索引鍵 (名稱)</span><span class="sxs-lookup"><span data-stu-id="dc124-398">Application key (name)</span></span>

<span data-ttu-id="dc124-399">[IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) 屬性是在主機建構期間從主機設定當中設定。</span><span class="sxs-lookup"><span data-stu-id="dc124-399">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="dc124-400">若要明確設定該值，請使用 [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey)：</span><span class="sxs-lookup"><span data-stu-id="dc124-400">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="dc124-401">**索引鍵**：applicationName</span><span class="sxs-lookup"><span data-stu-id="dc124-401">**Key**: applicationName</span></span>  
<span data-ttu-id="dc124-402">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="dc124-402">**Type**: *string*</span></span>  
<span data-ttu-id="dc124-403">**預設**：包含應用程式進入點的組件名稱。</span><span class="sxs-lookup"><span data-stu-id="dc124-403">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="dc124-404">**設定使用**：`HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="dc124-404">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="dc124-405">**環境變數**：`<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` 是[選擇性和使用者定義的](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="dc124-405">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="dc124-406">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="dc124-406">Content root</span></span>

<span data-ttu-id="dc124-407">此設定可決定主機開始搜尋內容檔案的位置。</span><span class="sxs-lookup"><span data-stu-id="dc124-407">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="dc124-408">**索引鍵**：contentRoot</span><span class="sxs-lookup"><span data-stu-id="dc124-408">**Key**: contentRoot</span></span>  
<span data-ttu-id="dc124-409">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="dc124-409">**Type**: *string*</span></span>  
<span data-ttu-id="dc124-410">**預設值**：預設為應用程式組件所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="dc124-410">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="dc124-411">**設定使用**：`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="dc124-411">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="dc124-412">**環境變數**：`<PREFIX_>CONTENTROOT` (`<PREFIX_>` 是[選擇性和使用者定義的](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="dc124-412">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="dc124-413">如果路徑不存在，就無法啟動主機。</span><span class="sxs-lookup"><span data-stu-id="dc124-413">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

<span data-ttu-id="dc124-414">如需詳細資訊，請參閱[基本概念：內容根目錄](xref:fundamentals/index#content-root)。</span><span class="sxs-lookup"><span data-stu-id="dc124-414">For more information, see [Fundamentals: Content root](xref:fundamentals/index#content-root).</span></span>

### <a name="environment"></a><span data-ttu-id="dc124-415">環境</span><span class="sxs-lookup"><span data-stu-id="dc124-415">Environment</span></span>

<span data-ttu-id="dc124-416">設定應用程式的[環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="dc124-416">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="dc124-417">**索引鍵**：environment</span><span class="sxs-lookup"><span data-stu-id="dc124-417">**Key**: environment</span></span>  
<span data-ttu-id="dc124-418">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="dc124-418">**Type**: *string*</span></span>  
<span data-ttu-id="dc124-419">**預設值**：Production</span><span class="sxs-lookup"><span data-stu-id="dc124-419">**Default**: Production</span></span>  
<span data-ttu-id="dc124-420">**設定使用**：`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="dc124-420">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="dc124-421">**環境變數**：`<PREFIX_>ENVIRONMENT` (`<PREFIX_>` 是[選擇性和使用者定義的](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="dc124-421">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="dc124-422">環境可以設定為任何值。</span><span class="sxs-lookup"><span data-stu-id="dc124-422">The environment can be set to any value.</span></span> <span data-ttu-id="dc124-423">架構定義的值包括 `Development`、`Staging` 和 `Production`。</span><span class="sxs-lookup"><span data-stu-id="dc124-423">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="dc124-424">值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="dc124-424">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="dc124-425">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="dc124-425">ConfigureHostConfiguration</span></span>

<span data-ttu-id="dc124-426"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> 會使用 <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> 來建立主機的 <xref:Microsoft.Extensions.Configuration.IConfiguration>。</span><span class="sxs-lookup"><span data-stu-id="dc124-426"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="dc124-427">主機組態會用來將 <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> 初始化，使其在應用程式建置流程中可供使用。</span><span class="sxs-lookup"><span data-stu-id="dc124-427">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="dc124-428"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="dc124-428"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="dc124-429">主機會使用指定索引鍵上最後設定值的任何選項。</span><span class="sxs-lookup"><span data-stu-id="dc124-429">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="dc124-430">預設不會包含任何提供者。</span><span class="sxs-lookup"><span data-stu-id="dc124-430">No providers are included by default.</span></span> <span data-ttu-id="dc124-431">您必須明確地指定應用程式在 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> 之中需要哪個組態提供者，包括：</span><span class="sxs-lookup"><span data-stu-id="dc124-431">You must explicitly specify whatever configuration providers the app requires in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, including:</span></span>

* <span data-ttu-id="dc124-432">檔案組態 (例如，來自 *hostsettings.json* 的檔案)。</span><span class="sxs-lookup"><span data-stu-id="dc124-432">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="dc124-433">環境變數組態。</span><span class="sxs-lookup"><span data-stu-id="dc124-433">Environment variable configuration.</span></span>
* <span data-ttu-id="dc124-434">命令列引數組態。</span><span class="sxs-lookup"><span data-stu-id="dc124-434">Command-line argument configuration.</span></span>
* <span data-ttu-id="dc124-435">任何其他必要的組態提供者。</span><span class="sxs-lookup"><span data-stu-id="dc124-435">Any other required configuration providers.</span></span>

<span data-ttu-id="dc124-436">使用之後呼叫[檔案組態提供者](xref:fundamentals/configuration/index#file-configuration-provider)的 `SetBasePath`，並透過指定應用程式基底路徑，就可使用主機的檔案設定。</span><span class="sxs-lookup"><span data-stu-id="dc124-436">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="dc124-437">範例應用程式會使用 JSON 檔案，*hostsettings.json*，並呼叫 <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> 取用檔案的主機組態設定。</span><span class="sxs-lookup"><span data-stu-id="dc124-437">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="dc124-438">若要新增主機的[環境變數組態](xref:fundamentals/configuration/index#environment-variables-configuration-provider)，請在主機建立器上呼叫 <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*>。</span><span class="sxs-lookup"><span data-stu-id="dc124-438">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="dc124-439">`AddEnvironmentVariables` 可接受選擇性的使用者定義前置詞。</span><span class="sxs-lookup"><span data-stu-id="dc124-439">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="dc124-440">範例應用程式會使用前置詞 `PREFIX_`。</span><span class="sxs-lookup"><span data-stu-id="dc124-440">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="dc124-441">讀取環境變數時，就會移除前置詞。</span><span class="sxs-lookup"><span data-stu-id="dc124-441">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="dc124-442">在設定範例應用程式的主機時，`PREFIX_ENVIRONMENT` 的環境變數值會變成 `environment` 索引鍵的主機組態值。</span><span class="sxs-lookup"><span data-stu-id="dc124-442">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="dc124-443">在開發期間使用 [Visual Studio](https://visualstudio.microsoft.com) 或以 `dotnet run` 執行應用程式時，可能會在 *Properties/launchSettings.json* 檔案中設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="dc124-443">During development when using [Visual Studio](https://visualstudio.microsoft.com) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="dc124-444">在 [Visual Studio Code](https://code.visualstudio.com/) 中，可以在開發期間於 *.vscode/launch.json* 檔案中設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="dc124-444">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="dc124-445">如需詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="dc124-445">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="dc124-446">[命令列組態](xref:fundamentals/configuration/index#command-line-configuration-provider)可透過呼叫 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 新增。</span><span class="sxs-lookup"><span data-stu-id="dc124-446">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="dc124-447">命令列組態會在最後新增，以便命令列引數覆寫由先前組態提供者提供的組態。</span><span class="sxs-lookup"><span data-stu-id="dc124-447">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="dc124-448">*hostsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="dc124-448">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="dc124-449">您可以使用 [applicationName](#application-key-name) 和 [contentRoot](#content-root) 索引鍵來提供其他組態。</span><span class="sxs-lookup"><span data-stu-id="dc124-449">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="dc124-450">使用 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> 的 `HostBuilder` 組態範例：</span><span class="sxs-lookup"><span data-stu-id="dc124-450">Example `HostBuilder` configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="dc124-451">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="dc124-451">ConfigureAppConfiguration</span></span>

<span data-ttu-id="dc124-452">應用程式組態的建立方式是在 <xref:Microsoft.Extensions.Hosting.IHostBuilder> 實作上呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>。</span><span class="sxs-lookup"><span data-stu-id="dc124-452">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="dc124-453"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 會使用 <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> 來建立應用程式的 <xref:Microsoft.Extensions.Configuration.IConfiguration>。</span><span class="sxs-lookup"><span data-stu-id="dc124-453"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="dc124-454"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="dc124-454"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="dc124-455">應用程式會使用指定索引鍵上最後設定值的任何選項。</span><span class="sxs-lookup"><span data-stu-id="dc124-455">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="dc124-456">您可以從後續作業的 [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) 及 <xref:Microsoft.Extensions.Hosting.IHost.Services*> 中存取 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 所建立的組態。</span><span class="sxs-lookup"><span data-stu-id="dc124-456">The configuration created by <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="dc124-457">應用程式組態會自動接收由 [ConfigureHostConfiguration](#configurehostconfiguration) 提供的主機組態。</span><span class="sxs-lookup"><span data-stu-id="dc124-457">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="dc124-458">使用 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 的應用程式組態範例：</span><span class="sxs-lookup"><span data-stu-id="dc124-458">Example app configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="dc124-459">*appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="dc124-459">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="dc124-460">*appsettings.Development.json*：</span><span class="sxs-lookup"><span data-stu-id="dc124-460">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="dc124-461">*appsettings.Production.json*：</span><span class="sxs-lookup"><span data-stu-id="dc124-461">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="dc124-462">若要將設定檔移至輸出目錄，請在專案檔中將設定檔指定為 [MSBuild 專案項目](/visualstudio/msbuild/common-msbuild-project-items)。</span><span class="sxs-lookup"><span data-stu-id="dc124-462">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="dc124-463">範例應用程式使用下列 `<Content>` 項目，移動其 JSON 應用程式設定檔和 *hostsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="dc124-463">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

> [!NOTE]
> <span data-ttu-id="dc124-464">組態擴充方法 (例如 <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> 和 <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*>) 需要其他的 NuGet 套件，例如 [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) \(英文\) 和[Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="dc124-464">Configuration extension methods, such as <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> and <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> require additional NuGet packages, such as [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) and [Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span></span> <span data-ttu-id="dc124-465">除非應用程式使用 [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app)，否則，除了核心 [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) \(英文\) 套件，還必須將這些套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="dc124-465">Unless the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), these packages must be added to the project in addition to the core [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) package.</span></span> <span data-ttu-id="dc124-466">如需詳細資訊，請參閱<xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="dc124-466">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="configureservices"></a><span data-ttu-id="dc124-467">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="dc124-467">ConfigureServices</span></span>

<span data-ttu-id="dc124-468"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> 會將服務新增至應用程式的[相依性插入](xref:fundamentals/dependency-injection)容器。</span><span class="sxs-lookup"><span data-stu-id="dc124-468"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="dc124-469"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="dc124-469"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="dc124-470">託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="dc124-470">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="dc124-471">如需詳細資訊，請參閱<xref:fundamentals/host/hosted-services>。</span><span class="sxs-lookup"><span data-stu-id="dc124-471">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="dc124-472">[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)使用 `AddHostedService` 擴充方法，將存留期事件 `LifetimeEventsHostedService` 和計時背景工作 `TimedHostedService` 等服務新增至應用程式：</span><span class="sxs-lookup"><span data-stu-id="dc124-472">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="dc124-473">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="dc124-473">ConfigureLogging</span></span>

<span data-ttu-id="dc124-474"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> 會新增委派以設定提供的 <xref:Microsoft.Extensions.Logging.ILoggingBuilder>。</span><span class="sxs-lookup"><span data-stu-id="dc124-474"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="dc124-475"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="dc124-475"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="dc124-476">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="dc124-476">UseConsoleLifetime</span></span>

<span data-ttu-id="dc124-477"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> 會接聽 Ctrl+C/SIGINT 或 SIGTERM，並呼叫 <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> 以啟動關機程序。</span><span class="sxs-lookup"><span data-stu-id="dc124-477"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="dc124-478"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> 會解除封鎖 [RunAsync](#runasync) 和 [WaitForShutdownAsync](#waitforshutdownasync) 等延伸模組。</span><span class="sxs-lookup"><span data-stu-id="dc124-478"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="dc124-479">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` 會預先註冊為預設存留期實作。</span><span class="sxs-lookup"><span data-stu-id="dc124-479">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="dc124-480">系統會使用最後一個註冊的存留期。</span><span class="sxs-lookup"><span data-stu-id="dc124-480">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="dc124-481">容器設定</span><span class="sxs-lookup"><span data-stu-id="dc124-481">Container configuration</span></span>

<span data-ttu-id="dc124-482">為支援插入其他容器，主機可以接受 <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>。</span><span class="sxs-lookup"><span data-stu-id="dc124-482">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span></span> <span data-ttu-id="dc124-483">提供處理站不是 DI 容器註冊的一部分，而是用來建立實體 DI 容器的主機內建功能。</span><span class="sxs-lookup"><span data-stu-id="dc124-483">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="dc124-484">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) 會覆寫用來建立應用程式服務提供者的預設處理站。</span><span class="sxs-lookup"><span data-stu-id="dc124-484">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="dc124-485">自訂容器組態由 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> 方法管理。</span><span class="sxs-lookup"><span data-stu-id="dc124-485">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="dc124-486"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> 提供在基礎主機 API 上設定容器的強型別體驗。</span><span class="sxs-lookup"><span data-stu-id="dc124-486"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="dc124-487"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="dc124-487"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="dc124-488">建立應用程式的服務容器：</span><span class="sxs-lookup"><span data-stu-id="dc124-488">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="dc124-489">提供服務容器處理站：</span><span class="sxs-lookup"><span data-stu-id="dc124-489">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="dc124-490">使用處理站，並設定應用程式的自訂服務容器：</span><span class="sxs-lookup"><span data-stu-id="dc124-490">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="dc124-491">擴充性</span><span class="sxs-lookup"><span data-stu-id="dc124-491">Extensibility</span></span>

<span data-ttu-id="dc124-492">主機擴充性是透過 <xref:Microsoft.Extensions.Hosting.IHostBuilder> 上的擴充方法執行。</span><span class="sxs-lookup"><span data-stu-id="dc124-492">Host extensibility is performed with extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span></span> <span data-ttu-id="dc124-493">下列範例使用 <xref:fundamentals/host/hosted-services> 中示範的 [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) 範例顯示擴充方法如何擴充 <xref:Microsoft.Extensions.Hosting.IHostBuilder> 實作。</span><span class="sxs-lookup"><span data-stu-id="dc124-493">The following example shows how an extension method extends an <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="dc124-494">應用程式會建立 `UseHostedService` 擴充方法以註冊在 `T` 中傳遞的裝載服務：</span><span class="sxs-lookup"><span data-stu-id="dc124-494">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

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

## <a name="manage-the-host"></a><span data-ttu-id="dc124-495">管理主機</span><span class="sxs-lookup"><span data-stu-id="dc124-495">Manage the host</span></span>

<span data-ttu-id="dc124-496"><xref:Microsoft.Extensions.Hosting.IHost> 實作負責啟動及停止服務容器中已註冊的 <xref:Microsoft.Extensions.Hosting.IHostedService> 實作。</span><span class="sxs-lookup"><span data-stu-id="dc124-496">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="dc124-497">執行</span><span class="sxs-lookup"><span data-stu-id="dc124-497">Run</span></span>

<span data-ttu-id="dc124-498"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> 會執行應用程式並封鎖呼叫執行緒，直到主機關閉為止：</span><span class="sxs-lookup"><span data-stu-id="dc124-498"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="dc124-499">RunAsync</span><span class="sxs-lookup"><span data-stu-id="dc124-499">RunAsync</span></span>

<span data-ttu-id="dc124-500"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> 會執行應用程式，並傳回觸發取消語彙基元或關機時所完成的 <xref:System.Threading.Tasks.Task>：</span><span class="sxs-lookup"><span data-stu-id="dc124-500"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="dc124-501">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="dc124-501">RunConsoleAsync</span></span>

<span data-ttu-id="dc124-502"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> 會啟用主控台支援、建置和啟動主機，以及等候 Ctrl+C/SIGINT 或 SIGTERM 關機。</span><span class="sxs-lookup"><span data-stu-id="dc124-502"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="dc124-503">Start 和 StopAsync</span><span class="sxs-lookup"><span data-stu-id="dc124-503">Start and StopAsync</span></span>

<span data-ttu-id="dc124-504"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> 會同步啟動主機。</span><span class="sxs-lookup"><span data-stu-id="dc124-504"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="dc124-505"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> 會嘗試在提供的逾時內停止主機。</span><span class="sxs-lookup"><span data-stu-id="dc124-505"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="dc124-506">StartAsync 和 StopAsync</span><span class="sxs-lookup"><span data-stu-id="dc124-506">StartAsync and StopAsync</span></span>

<span data-ttu-id="dc124-507"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> 會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="dc124-507"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="dc124-508"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> 會停止應用程式。</span><span class="sxs-lookup"><span data-stu-id="dc124-508"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="dc124-509">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="dc124-509">WaitForShutdown</span></span>

<span data-ttu-id="dc124-510"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> 會透過 <xref:Microsoft.Extensions.Hosting.IHostLifetime> 觸發，例如 `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` (會接聽 Ctrl+C/SIGINT 或 SIGTERM)。</span><span class="sxs-lookup"><span data-stu-id="dc124-510"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` (listens for Ctrl+C/SIGINT or SIGTERM).</span></span> <span data-ttu-id="dc124-511"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> 呼叫 <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>。</span><span class="sxs-lookup"><span data-stu-id="dc124-511"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="dc124-512">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="dc124-512">WaitForShutdownAsync</span></span>

<span data-ttu-id="dc124-513"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> 會傳回透過指定語彙基元觸發關機時所完成的 <xref:System.Threading.Tasks.Task>，並呼叫 <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>。</span><span class="sxs-lookup"><span data-stu-id="dc124-513"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="external-control"></a><span data-ttu-id="dc124-514">外部控制</span><span class="sxs-lookup"><span data-stu-id="dc124-514">External control</span></span>

<span data-ttu-id="dc124-515">主機的外部控制可以使用可從外部呼叫的方法來達成：</span><span class="sxs-lookup"><span data-stu-id="dc124-515">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="dc124-516"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> 在 <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> 開始時呼叫，並等到完成後再繼續進行。</span><span class="sxs-lookup"><span data-stu-id="dc124-516"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="dc124-517">這可用來將啟動延遲到外部事件發出訊號為止。</span><span class="sxs-lookup"><span data-stu-id="dc124-517">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="dc124-518">IHostingEnvironment 介面</span><span class="sxs-lookup"><span data-stu-id="dc124-518">IHostingEnvironment interface</span></span>

<span data-ttu-id="dc124-519"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> 提供應用程式主控環境的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="dc124-519"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="dc124-520">使用[建構函式插入](xref:fundamentals/dependency-injection)以取得 <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>，才能使用其屬性和擴充方法：</span><span class="sxs-lookup"><span data-stu-id="dc124-520">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="dc124-521">如需詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="dc124-521">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="dc124-522">IApplicationLifetime 介面</span><span class="sxs-lookup"><span data-stu-id="dc124-522">IApplicationLifetime interface</span></span>

<span data-ttu-id="dc124-523"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> 允許啟動後和關機活動，包括順利關機要求。</span><span class="sxs-lookup"><span data-stu-id="dc124-523"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="dc124-524">在介面上的三個屬性是用來註冊定義啟動和關閉事件之 <xref:System.Action> 方法的取消語彙基元。</span><span class="sxs-lookup"><span data-stu-id="dc124-524">Three properties on the interface are cancellation tokens used to register <xref:System.Action> methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="dc124-525">取消語彙基元</span><span class="sxs-lookup"><span data-stu-id="dc124-525">Cancellation Token</span></span> | <span data-ttu-id="dc124-526">觸發時機&#8230;</span><span class="sxs-lookup"><span data-stu-id="dc124-526">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="dc124-527">已完全啟動主機。</span><span class="sxs-lookup"><span data-stu-id="dc124-527">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="dc124-528">主機正在完成正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="dc124-528">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="dc124-529">應該處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="dc124-529">All requests should be processed.</span></span> <span data-ttu-id="dc124-530">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="dc124-530">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="dc124-531">主機正在執行正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="dc124-531">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="dc124-532">可能仍在處理要求。</span><span class="sxs-lookup"><span data-stu-id="dc124-532">Requests may still be processing.</span></span> <span data-ttu-id="dc124-533">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="dc124-533">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="dc124-534">建構函式將 <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> 服務插入任何類別。</span><span class="sxs-lookup"><span data-stu-id="dc124-534">Constructor-inject the <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> service into any class.</span></span> <span data-ttu-id="dc124-535">[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)使用建構函式插入 `LifetimeEventsHostedService` 類別 (<xref:Microsoft.Extensions.Hosting.IHostedService> 實作) 來註冊事件。</span><span class="sxs-lookup"><span data-stu-id="dc124-535">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an <xref:Microsoft.Extensions.Hosting.IHostedService> implementation) to register the events.</span></span>

<span data-ttu-id="dc124-536">*LifetimeEventsHostedService.cs*：</span><span class="sxs-lookup"><span data-stu-id="dc124-536">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="dc124-537"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> 會要求應用程式終止。</span><span class="sxs-lookup"><span data-stu-id="dc124-537"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="dc124-538">當呼叫類別的 `Shutdown` 方法時，下列類別使用 <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> 來順利關閉應用程式：</span><span class="sxs-lookup"><span data-stu-id="dc124-538">The following class uses <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="dc124-539">其他資源</span><span class="sxs-lookup"><span data-stu-id="dc124-539">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
