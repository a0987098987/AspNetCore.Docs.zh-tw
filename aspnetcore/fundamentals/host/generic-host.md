---
title: .NET 泛型主機
author: guardrex
description: 了解 ASP.NET Core 的泛型主機，其負責啟動應用程式及管理存留期。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: a128b7c19d544d1dd28ab16f7a208ceef680ce81
ms.sourcegitcommit: b3894b65e313570e97a2ab78b8addd22f427cac8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/23/2019
ms.locfileid: "56743838"
---
# <a name="net-generic-host"></a><span data-ttu-id="be6a0-103">.NET 泛型主機</span><span class="sxs-lookup"><span data-stu-id="be6a0-103">.NET Generic Host</span></span>

<span data-ttu-id="be6a0-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="be6a0-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="be6a0-105">ASP.NET Core 應用程式會設定並啟動主機。</span><span class="sxs-lookup"><span data-stu-id="be6a0-105">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="be6a0-106">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="be6a0-106">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="be6a0-107">本文所涵蓋的 ASP.NET Core 泛型主機 (<xref:Microsoft.Extensions.Hosting.HostBuilder>)，可用來不會處理 HTTP 要求的應用程式。</span><span class="sxs-lookup"><span data-stu-id="be6a0-107">This article covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is used for apps that don't process HTTP requests.</span></span>

<span data-ttu-id="be6a0-108">泛型主機的用途是將 HTTP 管線從 Web 主機 API 分離，以允許更廣泛的主機陣列案例。</span><span class="sxs-lookup"><span data-stu-id="be6a0-108">The purpose of Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="be6a0-109">傳訊、背景工作及其他以泛型主機為基礎的非 HTTP 工作負載都受益於跨領域的功能，例如設定、相依性插入 (DI) 和記錄。</span><span class="sxs-lookup"><span data-stu-id="be6a0-109">Messaging, background tasks, and other non-HTTP workloads based on Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="be6a0-110">泛型主機是 ASP.NET Core 2.1 的新功能，不適用於虛擬主機案例。</span><span class="sxs-lookup"><span data-stu-id="be6a0-110">Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="be6a0-111">針對虛擬主機案例，請使用 [Web 主機](xref:fundamentals/host/web-host)。</span><span class="sxs-lookup"><span data-stu-id="be6a0-111">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="be6a0-112">泛型主機將在未來版本中取代 Web 主機，並成為 HTTP 和非 HTTP 案例中的主要主機 API。</span><span class="sxs-lookup"><span data-stu-id="be6a0-112">Generic Host will replace Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

<span data-ttu-id="be6a0-113">ASP.NET Core 應用程式會設定並啟動主機。</span><span class="sxs-lookup"><span data-stu-id="be6a0-113">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="be6a0-114">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="be6a0-114">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="be6a0-115">本文涵蓋 .NET Core 泛型主機 (<xref:Microsoft.Extensions.Hosting.HostBuilder>)。</span><span class="sxs-lookup"><span data-stu-id="be6a0-115">This article covers the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>).</span></span>

<span data-ttu-id="be6a0-116">泛型主機與 Web 主機不同，其會將 HTTP 管線從 Web 主機 API 分離，以允許更廣泛的主機陣列案例。</span><span class="sxs-lookup"><span data-stu-id="be6a0-116">Generic Host differs from Web Host in that it decouples the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="be6a0-117">傳訊、背景工作及其他非 HTTP 工作負載都可使用泛型主機並受益於跨領域的功能，例如設定、相依性插入 (DI) 和記錄。</span><span class="sxs-lookup"><span data-stu-id="be6a0-117">Messaging, background tasks, and other non-HTTP workloads can use Generic Host and benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="be6a0-118">從 ASP.NET Core 3.0 開始，建議針對 HTTP 和非 HTTP 工作負載使用泛型主機。</span><span class="sxs-lookup"><span data-stu-id="be6a0-118">Starting in ASP.NET Core 3.0, Generic Host is recommended for both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="be6a0-119">HTTP 伺服器實作 (如果包含) 會以 <xref:Microsoft.Extensions.Hosting.IHostedService> 的實作來執行。</span><span class="sxs-lookup"><span data-stu-id="be6a0-119">An HTTP server implementation, if included, runs as an implementation of <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="be6a0-120">`IHostedService` 是也可針對其他工作負載使用的介面。</span><span class="sxs-lookup"><span data-stu-id="be6a0-120">`IHostedService` is an interface that can be used for other workloads as well.</span></span>

<span data-ttu-id="be6a0-121">不再針對 Web 應用程式建議使用 Web 主機，但仍然適用於回溯相容性。</span><span class="sxs-lookup"><span data-stu-id="be6a0-121">Web Host is no longer recommended for web apps but remains available for backward compatibility.</span></span>

> [!NOTE]
> <span data-ttu-id="be6a0-122">本文的其餘部分尚未更新為 3.0。</span><span class="sxs-lookup"><span data-stu-id="be6a0-122">This remainder of this article has not yet been updated for 3.0.</span></span>

::: moniker-end

<span data-ttu-id="be6a0-123">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="be6a0-123">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="be6a0-124">在 [Visual Studio Code](https://code.visualstudio.com/) 中執行範例應用程式時，請使用「外部或整合式終端機」。</span><span class="sxs-lookup"><span data-stu-id="be6a0-124">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="be6a0-125">請勿在 `internalConsole` 中執行範例。</span><span class="sxs-lookup"><span data-stu-id="be6a0-125">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="be6a0-126">若要在 Visual Studio Code 中設定主控台：</span><span class="sxs-lookup"><span data-stu-id="be6a0-126">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="be6a0-127">開啟 *.vscode/launch.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="be6a0-127">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="be6a0-128">在 [.NET Core Launch (console)] \(.NET Core 啟動 (主控台)\) 設定中，尋找 **console** 項目。</span><span class="sxs-lookup"><span data-stu-id="be6a0-128">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="be6a0-129">將此值設定為 `externalTerminal` 或 `integratedTerminal`。</span><span class="sxs-lookup"><span data-stu-id="be6a0-129">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="be6a0-130">簡介</span><span class="sxs-lookup"><span data-stu-id="be6a0-130">Introduction</span></span>

<span data-ttu-id="be6a0-131">可使用位於 <xref:Microsoft.Extensions.Hosting> 命名空間的泛型主機程式庫，且該程式庫由 [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) 套件提供。</span><span class="sxs-lookup"><span data-stu-id="be6a0-131">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="be6a0-132">[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 或更新版本) 包含 `Microsoft.Extensions.Hosting` 套件。</span><span class="sxs-lookup"><span data-stu-id="be6a0-132">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="be6a0-133"><xref:Microsoft.Extensions.Hosting.IHostedService> 是程式碼執行的進入點。</span><span class="sxs-lookup"><span data-stu-id="be6a0-133"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="be6a0-134">每個 `IHostedService` 實作會依 [ConfigureServices 中的服務註冊](#configureservices)順序執行。</span><span class="sxs-lookup"><span data-stu-id="be6a0-134">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="be6a0-135">當主機啟動時，會在每個 `IHostedService` 上呼叫 <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*>；當主機順利關閉時，則會依反向註冊順序呼叫 <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*>。</span><span class="sxs-lookup"><span data-stu-id="be6a0-135"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each `IHostedService` when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="be6a0-136">設定主機</span><span class="sxs-lookup"><span data-stu-id="be6a0-136">Set up a host</span></span>

<span data-ttu-id="be6a0-137"><xref:Microsoft.Extensions.Hosting.IHostBuilder> 是程式庫和應用程式用來初始化、建置及執行主機的主要元件：</span><span class="sxs-lookup"><span data-stu-id="be6a0-137"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a><span data-ttu-id="be6a0-138">選項</span><span class="sxs-lookup"><span data-stu-id="be6a0-138">Options</span></span>

<span data-ttu-id="be6a0-139"><xref:Microsoft.Extensions.Hosting.IHost> 的 <xref:Microsoft.Extensions.Hosting.HostOptions> 設定選項。</span><span class="sxs-lookup"><span data-stu-id="be6a0-139"><xref:Microsoft.Extensions.Hosting.HostOptions> configure options for the <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="be6a0-140">關機逾時</span><span class="sxs-lookup"><span data-stu-id="be6a0-140">Shutdown timeout</span></span>

<span data-ttu-id="be6a0-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> 會為 <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> 設定逾時。</span><span class="sxs-lookup"><span data-stu-id="be6a0-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="be6a0-142">預設值是五秒鐘。</span><span class="sxs-lookup"><span data-stu-id="be6a0-142">The default value is five seconds.</span></span>

<span data-ttu-id="be6a0-143">`Program.Main` 中的下列選項設定可將預設的五秒鐘關機逾時增加到 20 秒：</span><span class="sxs-lookup"><span data-stu-id="be6a0-143">The following option configuration in `Program.Main` increases the default five second shutdown timeout to 20 seconds:</span></span>

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

## <a name="default-services"></a><span data-ttu-id="be6a0-144">預設服務</span><span class="sxs-lookup"><span data-stu-id="be6a0-144">Default services</span></span>

<span data-ttu-id="be6a0-145">下列服務會在主機初始化期間註冊：</span><span class="sxs-lookup"><span data-stu-id="be6a0-145">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="be6a0-146">[環境](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="be6a0-146">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="be6a0-147">[組態](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="be6a0-147">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="be6a0-148"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span><span class="sxs-lookup"><span data-stu-id="be6a0-148"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span></span>
* <span data-ttu-id="be6a0-149"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span><span class="sxs-lookup"><span data-stu-id="be6a0-149"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="be6a0-150">[選項](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="be6a0-150">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="be6a0-151">[記錄](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="be6a0-151">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="be6a0-152">主機組態</span><span class="sxs-lookup"><span data-stu-id="be6a0-152">Host configuration</span></span>

<span data-ttu-id="be6a0-153">主機組態的建立方式：</span><span class="sxs-lookup"><span data-stu-id="be6a0-153">Host configuration is created by:</span></span>

* <span data-ttu-id="be6a0-154">呼叫 <xref:Microsoft.Extensions.Hosting.IHostBuilder> 上的擴充方法，來設定[內容根目錄](#content-root)及[環境](#environment)。</span><span class="sxs-lookup"><span data-stu-id="be6a0-154">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="be6a0-155">在 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> 中從組態提供者讀取組態。</span><span class="sxs-lookup"><span data-stu-id="be6a0-155">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="be6a0-156">擴充方法</span><span class="sxs-lookup"><span data-stu-id="be6a0-156">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="be6a0-157">應用程式索引鍵 (名稱)</span><span class="sxs-lookup"><span data-stu-id="be6a0-157">Application key (name)</span></span>

<span data-ttu-id="be6a0-158">[IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) 屬性是在主機建構期間從主機設定當中設定。</span><span class="sxs-lookup"><span data-stu-id="be6a0-158">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="be6a0-159">若要明確設定該值，請使用 [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey)：</span><span class="sxs-lookup"><span data-stu-id="be6a0-159">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="be6a0-160">**索引鍵**：applicationName</span><span class="sxs-lookup"><span data-stu-id="be6a0-160">**Key**: applicationName</span></span>  
<span data-ttu-id="be6a0-161">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="be6a0-161">**Type**: *string*</span></span>  
<span data-ttu-id="be6a0-162">**預設**：包含應用程式進入點的組件名稱。</span><span class="sxs-lookup"><span data-stu-id="be6a0-162">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="be6a0-163">**設定使用**：`HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="be6a0-163">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="be6a0-164">**環境變數**：`<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` 是[選擇性和使用者定義的](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="be6a0-164">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="be6a0-165">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="be6a0-165">Content root</span></span>

<span data-ttu-id="be6a0-166">此設定可決定主機開始搜尋內容檔案的位置。</span><span class="sxs-lookup"><span data-stu-id="be6a0-166">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="be6a0-167">**索引鍵**：contentRoot</span><span class="sxs-lookup"><span data-stu-id="be6a0-167">**Key**: contentRoot</span></span>  
<span data-ttu-id="be6a0-168">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="be6a0-168">**Type**: *string*</span></span>  
<span data-ttu-id="be6a0-169">**預設**：預設為應用程式組件所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="be6a0-169">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="be6a0-170">**設定使用**：`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="be6a0-170">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="be6a0-171">**環境變數**：`<PREFIX_>CONTENTROOT` (`<PREFIX_>` 是[選擇性和使用者定義的](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="be6a0-171">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="be6a0-172">如果路徑不存在，就無法啟動主機。</span><span class="sxs-lookup"><span data-stu-id="be6a0-172">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

### <a name="environment"></a><span data-ttu-id="be6a0-173">環境</span><span class="sxs-lookup"><span data-stu-id="be6a0-173">Environment</span></span>

<span data-ttu-id="be6a0-174">設定應用程式的[環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="be6a0-174">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="be6a0-175">**索引鍵**：environment</span><span class="sxs-lookup"><span data-stu-id="be6a0-175">**Key**: environment</span></span>  
<span data-ttu-id="be6a0-176">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="be6a0-176">**Type**: *string*</span></span>  
<span data-ttu-id="be6a0-177">**預設**：生產環境</span><span class="sxs-lookup"><span data-stu-id="be6a0-177">**Default**: Production</span></span>  
<span data-ttu-id="be6a0-178">**設定使用**：`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="be6a0-178">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="be6a0-179">**環境變數**：`<PREFIX_>ENVIRONMENT` (`<PREFIX_>` 是[選擇性和使用者定義的](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="be6a0-179">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="be6a0-180">環境可以設定為任何值。</span><span class="sxs-lookup"><span data-stu-id="be6a0-180">The environment can be set to any value.</span></span> <span data-ttu-id="be6a0-181">架構定義的值包括 `Development`、`Staging` 和 `Production`。</span><span class="sxs-lookup"><span data-stu-id="be6a0-181">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="be6a0-182">值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="be6a0-182">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="be6a0-183">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="be6a0-183">ConfigureHostConfiguration</span></span>

<span data-ttu-id="be6a0-184">`ConfigureHostConfiguration` 會使用 <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> 來建立主機的 <xref:Microsoft.Extensions.Configuration.IConfiguration>。</span><span class="sxs-lookup"><span data-stu-id="be6a0-184">`ConfigureHostConfiguration` uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="be6a0-185">主機組態會用來將 <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> 初始化，使其在應用程式建置流程中可供使用。</span><span class="sxs-lookup"><span data-stu-id="be6a0-185">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="be6a0-186">`ConfigureHostConfiguration` 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="be6a0-186">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="be6a0-187">主機會使用指定索引鍵上最後設定值的任何選項。</span><span class="sxs-lookup"><span data-stu-id="be6a0-187">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="be6a0-188">主機組態會自動流向應用程式組態 ([ConfigureAppConfiguration](#configureappconfiguration) 及剩餘的所有應用程式)。</span><span class="sxs-lookup"><span data-stu-id="be6a0-188">Host configuration automatically flows to app configuration ([ConfigureAppConfiguration](#configureappconfiguration) and the rest of the app).</span></span>

<span data-ttu-id="be6a0-189">預設不會包含任何提供者。</span><span class="sxs-lookup"><span data-stu-id="be6a0-189">No providers are included by default.</span></span> <span data-ttu-id="be6a0-190">您必須明確地指定應用程式在 `ConfigureHostConfiguration` 之中需要哪個組態提供者，包括：</span><span class="sxs-lookup"><span data-stu-id="be6a0-190">You must explicitly specify whatever configuration providers the app requires in `ConfigureHostConfiguration`, including:</span></span>

* <span data-ttu-id="be6a0-191">檔案組態 (例如，來自 *hostsettings.json* 的檔案)。</span><span class="sxs-lookup"><span data-stu-id="be6a0-191">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="be6a0-192">環境變數組態。</span><span class="sxs-lookup"><span data-stu-id="be6a0-192">Environment variable configuration.</span></span>
* <span data-ttu-id="be6a0-193">命令列引數組態。</span><span class="sxs-lookup"><span data-stu-id="be6a0-193">Command-line argument configuration.</span></span>
* <span data-ttu-id="be6a0-194">任何其他必要的組態提供者。</span><span class="sxs-lookup"><span data-stu-id="be6a0-194">Any other required configuration providers.</span></span>

<span data-ttu-id="be6a0-195">使用之後呼叫[檔案組態提供者](xref:fundamentals/configuration/index#file-configuration-provider)的 `SetBasePath`，並透過指定應用程式基底路徑，就可使用主機的檔案設定。</span><span class="sxs-lookup"><span data-stu-id="be6a0-195">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="be6a0-196">範例應用程式會使用 JSON 檔案，*hostsettings.json*，並呼叫 <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> 取用檔案的主機組態設定。</span><span class="sxs-lookup"><span data-stu-id="be6a0-196">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="be6a0-197">若要新增主機的[環境變數組態](xref:fundamentals/configuration/index#environment-variables-configuration-provider)，請在主機建立器上呼叫 <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*>。</span><span class="sxs-lookup"><span data-stu-id="be6a0-197">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="be6a0-198">`AddEnvironmentVariables` 可接受選擇性的使用者定義前置詞。</span><span class="sxs-lookup"><span data-stu-id="be6a0-198">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="be6a0-199">範例應用程式會使用前置詞 `PREFIX_`。</span><span class="sxs-lookup"><span data-stu-id="be6a0-199">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="be6a0-200">讀取環境變數時，就會移除前置詞。</span><span class="sxs-lookup"><span data-stu-id="be6a0-200">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="be6a0-201">在設定範例應用程式的主機時，`PREFIX_ENVIRONMENT` 的環境變數值會變成 `environment` 索引鍵的主機組態值。</span><span class="sxs-lookup"><span data-stu-id="be6a0-201">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="be6a0-202">在開發期間使用 [Visual Studio](https://www.visualstudio.com/) 或以 `dotnet run` 執行應用程式時，可能會在 *Properties/launchSettings.json* 檔案中設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="be6a0-202">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="be6a0-203">在 [Visual Studio Code](https://code.visualstudio.com/) 中，可以在開發期間於 *.vscode/launch.json* 檔案中設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="be6a0-203">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="be6a0-204">如需詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="be6a0-204">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="be6a0-205">[命令列組態](xref:fundamentals/configuration/index#command-line-configuration-provider)可透過呼叫 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 新增。</span><span class="sxs-lookup"><span data-stu-id="be6a0-205">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="be6a0-206">命令列組態會在最後新增，以便命令列引數覆寫由先前組態提供者提供的組態。</span><span class="sxs-lookup"><span data-stu-id="be6a0-206">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="be6a0-207">*hostsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="be6a0-207">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="be6a0-208">您可以使用 [applicationName](#application-key-name) 和 [contentRoot](#content-root) 索引鍵來提供其他組態。</span><span class="sxs-lookup"><span data-stu-id="be6a0-208">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="be6a0-209">使用 `ConfigureHostConfiguration` 的 `HostBuilder` 組態範例：</span><span class="sxs-lookup"><span data-stu-id="be6a0-209">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="be6a0-210">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="be6a0-210">ConfigureAppConfiguration</span></span>

<span data-ttu-id="be6a0-211">應用程式組態的建立方式是在 <xref:Microsoft.Extensions.Hosting.IHostBuilder> 實作上呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>。</span><span class="sxs-lookup"><span data-stu-id="be6a0-211">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="be6a0-212">`ConfigureAppConfiguration` 會使用 <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> 來建立應用程式的 <xref:Microsoft.Extensions.Configuration.IConfiguration>。</span><span class="sxs-lookup"><span data-stu-id="be6a0-212">`ConfigureAppConfiguration` uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="be6a0-213">`ConfigureAppConfiguration` 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="be6a0-213">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="be6a0-214">應用程式會使用指定索引鍵上最後設定值的任何選項。</span><span class="sxs-lookup"><span data-stu-id="be6a0-214">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="be6a0-215">您可以從後續作業的 [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) 及 <xref:Microsoft.Extensions.Hosting.IHost.Services*> 中存取 `ConfigureAppConfiguration` 所建立的組態。</span><span class="sxs-lookup"><span data-stu-id="be6a0-215">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="be6a0-216">應用程式組態會自動接收由 [ConfigureHostConfiguration](#configurehostconfiguration) 提供的主機組態。</span><span class="sxs-lookup"><span data-stu-id="be6a0-216">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="be6a0-217">使用 `ConfigureAppConfiguration` 的應用程式組態範例：</span><span class="sxs-lookup"><span data-stu-id="be6a0-217">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="be6a0-218">*appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="be6a0-218">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="be6a0-219">*appsettings.Development.json*：</span><span class="sxs-lookup"><span data-stu-id="be6a0-219">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="be6a0-220">*appsettings.Production.json*：</span><span class="sxs-lookup"><span data-stu-id="be6a0-220">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="be6a0-221">若要將設定檔移至輸出目錄，請在專案檔中將設定檔指定為 [MSBuild 專案項目](/visualstudio/msbuild/common-msbuild-project-items)。</span><span class="sxs-lookup"><span data-stu-id="be6a0-221">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="be6a0-222">範例應用程式使用下列 `<Content>` 項目，移動其 JSON 應用程式設定檔和 *hostsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="be6a0-222">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a><span data-ttu-id="be6a0-223">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="be6a0-223">ConfigureServices</span></span>

<span data-ttu-id="be6a0-224"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> 會將服務新增至應用程式的[相依性插入](xref:fundamentals/dependency-injection)容器。</span><span class="sxs-lookup"><span data-stu-id="be6a0-224"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="be6a0-225">`ConfigureServices` 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="be6a0-225">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="be6a0-226">託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="be6a0-226">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="be6a0-227">如需詳細資訊，請參閱<xref:fundamentals/host/hosted-services>。</span><span class="sxs-lookup"><span data-stu-id="be6a0-227">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="be6a0-228">[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)使用 `AddHostedService` 擴充方法，將存留期事件 `LifetimeEventsHostedService` 和計時背景工作 `TimedHostedService` 等服務新增至應用程式：</span><span class="sxs-lookup"><span data-stu-id="be6a0-228">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="be6a0-229">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="be6a0-229">ConfigureLogging</span></span>

<span data-ttu-id="be6a0-230"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> 會新增委派以設定提供的 <xref:Microsoft.Extensions.Logging.ILoggingBuilder>。</span><span class="sxs-lookup"><span data-stu-id="be6a0-230"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="be6a0-231">`ConfigureLogging` 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="be6a0-231">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="be6a0-232">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="be6a0-232">UseConsoleLifetime</span></span>

<span data-ttu-id="be6a0-233"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> 會接聽 `Ctrl+C`/SIGINT 或 SIGTERM，並呼叫 <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> 以啟動關機程序。</span><span class="sxs-lookup"><span data-stu-id="be6a0-233"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for `Ctrl+C`/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="be6a0-234">`UseConsoleLifetime` 會解除封鎖 [RunAsync](#runasync) 和 [WaitForShutdownAsync](#waitforshutdownasync) 等延伸模組。</span><span class="sxs-lookup"><span data-stu-id="be6a0-234">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="be6a0-235"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> 會預先註冊為預設存留期實作。</span><span class="sxs-lookup"><span data-stu-id="be6a0-235"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="be6a0-236">系統會使用最後一個註冊的存留期。</span><span class="sxs-lookup"><span data-stu-id="be6a0-236">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="be6a0-237">容器設定</span><span class="sxs-lookup"><span data-stu-id="be6a0-237">Container configuration</span></span>

<span data-ttu-id="be6a0-238">為支援插入其他容器，主機可以接受 <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory`1>。</span><span class="sxs-lookup"><span data-stu-id="be6a0-238">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory`1>.</span></span> <span data-ttu-id="be6a0-239">提供處理站不是 DI 容器註冊的一部分，而是用來建立實體 DI 容器的主機內建功能。</span><span class="sxs-lookup"><span data-stu-id="be6a0-239">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="be6a0-240">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) 會覆寫用來建立應用程式服務提供者的預設處理站。</span><span class="sxs-lookup"><span data-stu-id="be6a0-240">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="be6a0-241">自訂容器組態由 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> 方法管理。</span><span class="sxs-lookup"><span data-stu-id="be6a0-241">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="be6a0-242">`ConfigureContainer` 提供在基礎主機 API 上設定容器的強型別體驗。</span><span class="sxs-lookup"><span data-stu-id="be6a0-242">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="be6a0-243">`ConfigureContainer` 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="be6a0-243">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="be6a0-244">建立應用程式的服務容器：</span><span class="sxs-lookup"><span data-stu-id="be6a0-244">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="be6a0-245">提供服務容器處理站：</span><span class="sxs-lookup"><span data-stu-id="be6a0-245">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="be6a0-246">使用處理站，並設定應用程式的自訂服務容器：</span><span class="sxs-lookup"><span data-stu-id="be6a0-246">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="be6a0-247">擴充性</span><span class="sxs-lookup"><span data-stu-id="be6a0-247">Extensibility</span></span>

<span data-ttu-id="be6a0-248">主機擴充性是透過 `IHostBuilder` 上的擴充方法執行。</span><span class="sxs-lookup"><span data-stu-id="be6a0-248">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="be6a0-249">下列範例使用 <xref:fundamentals/host/hosted-services> 中示範的 [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) 範例顯示擴充方法如何擴充 `IHostBuilder` 實作。</span><span class="sxs-lookup"><span data-stu-id="be6a0-249">The following example shows how an extension method extends an `IHostBuilder` implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="be6a0-250">應用程式會建立 `UseHostedService` 擴充方法以註冊在 `T` 中傳遞的裝載服務：</span><span class="sxs-lookup"><span data-stu-id="be6a0-250">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

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

## <a name="manage-the-host"></a><span data-ttu-id="be6a0-251">管理主機</span><span class="sxs-lookup"><span data-stu-id="be6a0-251">Manage the host</span></span>

<span data-ttu-id="be6a0-252"><xref:Microsoft.Extensions.Hosting.IHost> 實作負責啟動及停止服務容器中已註冊的 `IHostedService` 實作。</span><span class="sxs-lookup"><span data-stu-id="be6a0-252">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="be6a0-253">執行</span><span class="sxs-lookup"><span data-stu-id="be6a0-253">Run</span></span>

<span data-ttu-id="be6a0-254"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> 會執行應用程式並封鎖呼叫執行緒，直到主機關閉為止：</span><span class="sxs-lookup"><span data-stu-id="be6a0-254"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="be6a0-255">RunAsync</span><span class="sxs-lookup"><span data-stu-id="be6a0-255">RunAsync</span></span>

<span data-ttu-id="be6a0-256"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> 會執行應用程式，並傳回觸發取消語彙基元或關機時所完成的 `Task`：</span><span class="sxs-lookup"><span data-stu-id="be6a0-256"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="be6a0-257">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="be6a0-257">RunConsoleAsync</span></span>

<span data-ttu-id="be6a0-258"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> 會啟用主控台支援、建置和啟動主機，以及等候 `Ctrl+C`/SIGINT 或 SIGTERM 關機。</span><span class="sxs-lookup"><span data-stu-id="be6a0-258"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="be6a0-259">Start 和 StopAsync</span><span class="sxs-lookup"><span data-stu-id="be6a0-259">Start and StopAsync</span></span>

<span data-ttu-id="be6a0-260"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> 會同步啟動主機。</span><span class="sxs-lookup"><span data-stu-id="be6a0-260"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="be6a0-261"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> 會嘗試在提供的逾時內停止主機。</span><span class="sxs-lookup"><span data-stu-id="be6a0-261"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="be6a0-262">StartAsync 和 StopAsync</span><span class="sxs-lookup"><span data-stu-id="be6a0-262">StartAsync and StopAsync</span></span>

<span data-ttu-id="be6a0-263"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> 會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="be6a0-263"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="be6a0-264"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> 會停止應用程式。</span><span class="sxs-lookup"><span data-stu-id="be6a0-264"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="be6a0-265">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="be6a0-265">WaitForShutdown</span></span>

<span data-ttu-id="be6a0-266"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> 會透過 <xref:Microsoft.Extensions.Hosting.IHostLifetime> 觸發，像是 <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (會接聽 `Ctrl+C`/SIGINT 或 SIGTERM)。</span><span class="sxs-lookup"><span data-stu-id="be6a0-266"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="be6a0-267">`WaitForShutdown` 會呼叫 <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>。</span><span class="sxs-lookup"><span data-stu-id="be6a0-267">`WaitForShutdown` calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="be6a0-268">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="be6a0-268">WaitForShutdownAsync</span></span>

<span data-ttu-id="be6a0-269"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> 會傳回透過指定語彙基元觸發關機時所完成的 `Task`，並呼叫 <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>。</span><span class="sxs-lookup"><span data-stu-id="be6a0-269"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a `Task` that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="external-control"></a><span data-ttu-id="be6a0-270">外部控制</span><span class="sxs-lookup"><span data-stu-id="be6a0-270">External control</span></span>

<span data-ttu-id="be6a0-271">主機的外部控制可以使用可從外部呼叫的方法來達成：</span><span class="sxs-lookup"><span data-stu-id="be6a0-271">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="be6a0-272"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> 在 <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> 開始時呼叫，並等到完成後再繼續進行。</span><span class="sxs-lookup"><span data-stu-id="be6a0-272"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="be6a0-273">這可用來將啟動延遲到外部事件發出訊號為止。</span><span class="sxs-lookup"><span data-stu-id="be6a0-273">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="be6a0-274">IHostingEnvironment 介面</span><span class="sxs-lookup"><span data-stu-id="be6a0-274">IHostingEnvironment interface</span></span>

<span data-ttu-id="be6a0-275"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> 提供應用程式主控環境的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="be6a0-275"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="be6a0-276">使用[建構函式插入](xref:fundamentals/dependency-injection)以取得 `IHostingEnvironment`，才能使用其屬性和擴充方法：</span><span class="sxs-lookup"><span data-stu-id="be6a0-276">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="be6a0-277">如需詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="be6a0-277">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="be6a0-278">IApplicationLifetime 介面</span><span class="sxs-lookup"><span data-stu-id="be6a0-278">IApplicationLifetime interface</span></span>

<span data-ttu-id="be6a0-279"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> 允許啟動後和關機活動，包括順利關機要求。</span><span class="sxs-lookup"><span data-stu-id="be6a0-279"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="be6a0-280">在介面上的三個屬性是用來註冊定義啟動和關閉事件之 `Action` 方法的取消語彙基元。</span><span class="sxs-lookup"><span data-stu-id="be6a0-280">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="be6a0-281">取消語彙基元</span><span class="sxs-lookup"><span data-stu-id="be6a0-281">Cancellation Token</span></span> | <span data-ttu-id="be6a0-282">觸發時機&#8230;</span><span class="sxs-lookup"><span data-stu-id="be6a0-282">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="be6a0-283">已完全啟動主機。</span><span class="sxs-lookup"><span data-stu-id="be6a0-283">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="be6a0-284">主機正在完成正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="be6a0-284">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="be6a0-285">應該處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="be6a0-285">All requests should be processed.</span></span> <span data-ttu-id="be6a0-286">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="be6a0-286">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="be6a0-287">主機正在執行正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="be6a0-287">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="be6a0-288">可能仍在處理要求。</span><span class="sxs-lookup"><span data-stu-id="be6a0-288">Requests may still be processing.</span></span> <span data-ttu-id="be6a0-289">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="be6a0-289">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="be6a0-290">建構函式將 `IApplicationLifetime` 服務插入任何類別。</span><span class="sxs-lookup"><span data-stu-id="be6a0-290">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="be6a0-291">[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)使用建構函式插入 `LifetimeEventsHostedService` 類別 (`IHostedService` 實作) 來註冊事件。</span><span class="sxs-lookup"><span data-stu-id="be6a0-291">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="be6a0-292">*LifetimeEventsHostedService.cs*：</span><span class="sxs-lookup"><span data-stu-id="be6a0-292">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="be6a0-293"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> 會要求應用程式終止。</span><span class="sxs-lookup"><span data-stu-id="be6a0-293"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="be6a0-294">當呼叫類別的 `Shutdown` 方法時，下列類別使用 `StopApplication` 來順利關閉應用程式：</span><span class="sxs-lookup"><span data-stu-id="be6a0-294">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="be6a0-295">其他資源</span><span class="sxs-lookup"><span data-stu-id="be6a0-295">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
* [<span data-ttu-id="be6a0-296">GitHub 上的主控存放庫範例</span><span class="sxs-lookup"><span data-stu-id="be6a0-296">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
