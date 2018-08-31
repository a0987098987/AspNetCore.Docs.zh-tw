---
title: .NET 泛型主機
author: guardrex
description: 了解 .NET 中的泛型主機，其負責啟動應用程式以及管理存留期。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: de9044875c8ebc62c80a129d721e7d37be5d846d
ms.sourcegitcommit: 25150f4398de83132965a89f12d3a030f6cce48d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/25/2018
ms.locfileid: "42927805"
---
# <a name="net-generic-host"></a><span data-ttu-id="2ac1a-103">.NET 泛型主機</span><span class="sxs-lookup"><span data-stu-id="2ac1a-103">.NET Generic Host</span></span>

<span data-ttu-id="2ac1a-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2ac1a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2ac1a-105">.NET Core 應用程式會設定並啟動「主機」。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-105">.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="2ac1a-106">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="2ac1a-107">本主題包含 ASP.NET Core 泛型主機 ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder))，該主機適用於裝載未處理 HTTP 要求的應用程式。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-107">This topic covers the ASP.NET Core Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="2ac1a-108">如需 Web 主機 ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)) 的說明，請參閱 <xref:fundamentals/host/web-host>。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-108">For coverage of the Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), see <xref:fundamentals/host/web-host>.</span></span>

<span data-ttu-id="2ac1a-109">泛型主機的目標是將 HTTP 管線與 Web 主機 API 分離，以允許更廣泛的主機陣列案例。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="2ac1a-110">傳訊、背景工作及其他以泛型主機為基礎的非 HTTP 工作負載都可利用跨領域功能，例如設定、相依性插入 (DI) 和記錄。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="2ac1a-111">泛型主機是 ASP.NET Core 2.1 的新功能，不適用於虛擬主機案例。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="2ac1a-112">針對虛擬主機案例，請使用 [Web 主機](xref:fundamentals/host/web-host)。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="2ac1a-113">泛型主機目前正在開發中，未來版本將取代 Web 主機並成為 HTTP 和非 HTTP 案例中的主要主機 API。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="2ac1a-114">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2ac1a-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2ac1a-115">在 [Visual Studio Code](https://code.visualstudio.com/) 中執行範例應用程式時，請使用「外部或整合式終端機」。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="2ac1a-116">請勿在 `internalConsole` 中執行範例。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="2ac1a-117">若要在 Visual Studio Code 中設定主控台：</span><span class="sxs-lookup"><span data-stu-id="2ac1a-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="2ac1a-118">開啟 *.vscode/launch.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="2ac1a-119">在 [.NET Core Launch (console)] \(.NET Core 啟動 (主控台)\) 設定中，尋找 **console** 項目。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="2ac1a-120">將此值設定為 `externalTerminal` 或 `integratedTerminal`。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="2ac1a-121">簡介</span><span class="sxs-lookup"><span data-stu-id="2ac1a-121">Introduction</span></span>

<span data-ttu-id="2ac1a-122">泛型主機程式庫位於 [Microsoft.Extensions.Hosting 命名空間](/dotnet/api/microsoft.extensions.hosting)，並由 [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) 套件提供。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-122">The Generic Host library is available in the [Microsoft.Extensions.Hosting namespace](/dotnet/api/microsoft.extensions.hosting) and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="2ac1a-123">[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 或更新版本) 包含 `Microsoft.Extensions.Hosting` 套件。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="2ac1a-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) 是程式碼執行的進入點。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) is the entry point to code execution.</span></span> <span data-ttu-id="2ac1a-125">每個 `IHostedService` 實作會依 [ConfigureServices 中的服務註冊](#configureservices)順序執行。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="2ac1a-126">當主機啟動時，會在每個 `IHostedService` 上呼叫 [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync)；當主機順利關閉時，則會依反向註冊順序呼叫 [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync)。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) is called on each `IHostedService` when the host starts, and [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="2ac1a-127">設定主機</span><span class="sxs-lookup"><span data-stu-id="2ac1a-127">Set up a host</span></span>

<span data-ttu-id="2ac1a-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) 是程式庫和應用程式用來初始化、建置及執行主機的主要元件：</span><span class="sxs-lookup"><span data-stu-id="2ac1a-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a><span data-ttu-id="2ac1a-129">主機組態</span><span class="sxs-lookup"><span data-stu-id="2ac1a-129">Host configuration</span></span>

<span data-ttu-id="2ac1a-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) 依賴下列方法來設定主機組態值：</span><span class="sxs-lookup"><span data-stu-id="2ac1a-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="2ac1a-131">設定產生器</span><span class="sxs-lookup"><span data-stu-id="2ac1a-131">Configuration builder</span></span>
* <span data-ttu-id="2ac1a-132">擴充方法組態</span><span class="sxs-lookup"><span data-stu-id="2ac1a-132">Extension method configuration</span></span>

### <a name="configuration-builder"></a><span data-ttu-id="2ac1a-133">組態產生器</span><span class="sxs-lookup"><span data-stu-id="2ac1a-133">Configuration builder</span></span>

<span data-ttu-id="2ac1a-134">主機產生器組態是透過在 [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) 實作上呼叫 [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) 來建立。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-134">Host builder configuration is created by calling [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="2ac1a-135">`ConfigureHostConfiguration` 使用 [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) 來建立主機的 [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration)。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-135">`ConfigureHostConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the host.</span></span> <span data-ttu-id="2ac1a-136">設定產生器會初始化 [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment)，以供應用程式的建置程序使用。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-136">The configuration builder initializes the [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) for use in the app's build process.</span></span>

<span data-ttu-id="2ac1a-137">預設不會新增環境變數組態。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-137">Environment variable configuration isn't added by default.</span></span> <span data-ttu-id="2ac1a-138">請在主機建立器上呼叫 [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables)，以從環境變數設定主機。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-138">Call [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) on the host builder to configure the host from environment variables.</span></span> <span data-ttu-id="2ac1a-139">`AddEnvironmentVariables` 可接受選擇性的使用者定義前置詞。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-139">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="2ac1a-140">範例應用程式會使用前置詞 `PREFIX_`。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-140">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="2ac1a-141">讀取環境變數時，就會移除前置詞。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-141">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="2ac1a-142">在設定範例應用程式的主機時，`PREFIX_ENVIRONMENT` 的環境變數值會變成 `environment` 索引鍵的主機組態值。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-142">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="2ac1a-143">在開發期間使用 [Visual Studio](https://www.visualstudio.com/) 或以 `dotnet run` 執行應用程式時，可能會在 *Properties/launchSettings.json* 檔案中設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-143">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="2ac1a-144">在 [Visual Studio Code](https://code.visualstudio.com/) 中，可以在開發期間於 *.vscode/launch.json* 檔案中設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-144">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="2ac1a-145">如需詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-145">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="2ac1a-146">`ConfigureHostConfiguration` 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-146">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="2ac1a-147">主機使用設定最後一個值的任何選項。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-147">The host uses whichever option sets a value last.</span></span>

<span data-ttu-id="2ac1a-148">*hostsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="2ac1a-148">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="2ac1a-149">使用 `ConfigureHostConfiguration` 的 `HostBuilder` 組態範例：</span><span class="sxs-lookup"><span data-stu-id="2ac1a-149">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> <span data-ttu-id="2ac1a-150">[AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) 擴充方法目前無法剖析 [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) 傳回的組態區段 (例如 `.AddConfiguration(Configuration.GetSection("section"))`)。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-150">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="2ac1a-151">`GetSection` 方法會將組態索引鍵篩選到要求的區段，但保留索引鍵上的區段名稱 (例如 `section:environment`)。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-151">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:environment`).</span></span> <span data-ttu-id="2ac1a-152">`AddConfiguration` 方法預期索引鍵要符合 `HostBuilder` 索引鍵 (例如 `environment`)。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-152">The `AddConfiguration` method expects the keys to match the `HostBuilder` keys (for example, `environment`).</span></span> <span data-ttu-id="2ac1a-153">索引鍵上的區段名稱存在可避免區段的值設定主機。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-153">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="2ac1a-154">這個問題將在近期的版本中解決。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-154">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="2ac1a-155">如需詳細資訊和因應措施，請參閱 [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (將設定區段傳遞到 WebHostBuilder.UseConfiguration 會使用完整索引鍵)。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-155">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="extension-method-configuration"></a><span data-ttu-id="2ac1a-156">擴充方法組態</span><span class="sxs-lookup"><span data-stu-id="2ac1a-156">Extension method configuration</span></span>

<span data-ttu-id="2ac1a-157">在 `IHostBuilder` 實作上呼叫擴充方法可設定內容根目錄和環境。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-157">Extension methods are called on the `IHostBuilder` implementation to configure the content root and the environment.</span></span>

#### <a name="application-key-name"></a><span data-ttu-id="2ac1a-158">應用程式索引鍵 (名稱)</span><span class="sxs-lookup"><span data-stu-id="2ac1a-158">Application Key (Name)</span></span>

<span data-ttu-id="2ac1a-159">[IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) 屬性是在主機建構期間從主機設定當中設定。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-159">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is set from host configuration during host construction.</span></span> <span data-ttu-id="2ac1a-160">若要明確設定該值，請使用 [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey)：</span><span class="sxs-lookup"><span data-stu-id="2ac1a-160">To set the value explicitly, use the [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey):</span></span>

<span data-ttu-id="2ac1a-161">**索引鍵**：applicationName</span><span class="sxs-lookup"><span data-stu-id="2ac1a-161">**Key**: applicationName</span></span>  
<span data-ttu-id="2ac1a-162">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="2ac1a-162">**Type**: *string*</span></span>  
<span data-ttu-id="2ac1a-163">**預設**：包含應用程式進入點的組件名稱。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-163">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="2ac1a-164">**設定使用**：`HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="2ac1a-164">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="2ac1a-165">**環境變數**：`<PREFIX_>APPLICATIONKEY` (`<PREFIX_>` 是[選擇性和使用者定義的](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="2ac1a-165">**Environment variable**: `<PREFIX_>APPLICATIONKEY` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

```csharp
var host = new HostBuilder()
    .ConfigureAppConfiguration((hostContext, configApp) =>
    {
        hostContext.HostingEnvironment.ApplicationName = "CustomApplicationName";
    })
```

#### <a name="content-root"></a><span data-ttu-id="2ac1a-166">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="2ac1a-166">Content Root</span></span>

<span data-ttu-id="2ac1a-167">此設定可決定主機開始搜尋內容檔案的位置。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-167">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="2ac1a-168">**索引鍵**：contentRoot</span><span class="sxs-lookup"><span data-stu-id="2ac1a-168">**Key**: contentRoot</span></span>  
<span data-ttu-id="2ac1a-169">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="2ac1a-169">**Type**: *string*</span></span>  
<span data-ttu-id="2ac1a-170">**預設值**：預設為應用程式組件所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-170">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="2ac1a-171">**設定使用**：`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="2ac1a-171">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="2ac1a-172">**環境變數**：`<PREFIX_>CONTENTROOT` (`<PREFIX_>` 是[選擇性和使用者定義的](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="2ac1a-172">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="2ac1a-173">如果路徑不存在，就無法啟動主機。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-173">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a><span data-ttu-id="2ac1a-174">環境</span><span class="sxs-lookup"><span data-stu-id="2ac1a-174">Environment</span></span>

<span data-ttu-id="2ac1a-175">設定應用程式的[環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-175">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="2ac1a-176">**索引鍵**：environment</span><span class="sxs-lookup"><span data-stu-id="2ac1a-176">**Key**: environment</span></span>  
<span data-ttu-id="2ac1a-177">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="2ac1a-177">**Type**: *string*</span></span>  
<span data-ttu-id="2ac1a-178">**預設值**：Production</span><span class="sxs-lookup"><span data-stu-id="2ac1a-178">**Default**: Production</span></span>  
<span data-ttu-id="2ac1a-179">**設定使用**：`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="2ac1a-179">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="2ac1a-180">**環境變數**：`<PREFIX_>ENVIRONMENT` (`<PREFIX_>` 是[選擇性和使用者定義的](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="2ac1a-180">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="2ac1a-181">環境可以設定為任何值。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-181">The environment can be set to any value.</span></span> <span data-ttu-id="2ac1a-182">架構定義的值包括 `Development`、`Staging` 和 `Production`。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-182">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="2ac1a-183">值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-183">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a><span data-ttu-id="2ac1a-184">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="2ac1a-184">ConfigureAppConfiguration</span></span>

<span data-ttu-id="2ac1a-185">應用程式產生器設定是透過在 [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) 實作上呼叫 [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) 來建立。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-185">App builder configuration is created by calling [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="2ac1a-186">`ConfigureAppConfiguration` 使用 [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) 來建立應用程式的 [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration)。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-186">`ConfigureAppConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the app.</span></span> <span data-ttu-id="2ac1a-187">`ConfigureAppConfiguration` 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-187">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="2ac1a-188">應用程式使用設定最後一個值的任何選項。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-188">The app uses whichever option sets a value last.</span></span> <span data-ttu-id="2ac1a-189">您可以從後續作業的 [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) 及 [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services) 中存取 `ConfigureAppConfiguration` 所建立的設定。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-189">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) for subsequent operations and in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span></span>

<span data-ttu-id="2ac1a-190">使用 `ConfigureAppConfiguration` 的應用程式組態範例：</span><span class="sxs-lookup"><span data-stu-id="2ac1a-190">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="2ac1a-191">*appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="2ac1a-191">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="2ac1a-192">*appsettings.Development.json*：</span><span class="sxs-lookup"><span data-stu-id="2ac1a-192">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="2ac1a-193">*appsettings.Production.json*：</span><span class="sxs-lookup"><span data-stu-id="2ac1a-193">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> <span data-ttu-id="2ac1a-194">[AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) 擴充方法目前無法剖析 [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) 傳回的組態區段 (例如 `.AddConfiguration(Configuration.GetSection("section"))`)。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-194">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="2ac1a-195">`GetSection` 方法會將組態索引鍵篩選到要求的區段，但保留索引鍵上的區段名稱 (例如 `section:Logging:LogLevel:Default`)。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-195">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:Logging:LogLevel:Default`).</span></span> <span data-ttu-id="2ac1a-196">`AddConfiguration` 方法預期完全符合組態索引鍵 (例如 `Logging:LogLevel:Default`)。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-196">The `AddConfiguration` method expects an exact match to configuration keys (for example, `Logging:LogLevel:Default`).</span></span> <span data-ttu-id="2ac1a-197">存在於索引鍵上的區段名稱可避免區段的值設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-197">The presence of the section name on the keys prevents the section's values from configuring the app.</span></span> <span data-ttu-id="2ac1a-198">這個問題將在近期的版本中解決。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-198">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="2ac1a-199">如需詳細資訊和因應措施，請參閱 [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (將設定區段傳遞到 WebHostBuilder.UseConfiguration 會使用完整索引鍵)。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-199">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="2ac1a-200">若要將設定檔移至輸出目錄，請在專案檔中將設定檔指定為 [MSBuild 專案項目](/visualstudio/msbuild/common-msbuild-project-items)。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-200">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="2ac1a-201">範例應用程式使用下列的 **&lt;Content:&gt;** 項目，移動其 JSON 應用程式設定檔和 *hostsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="2ac1a-201">The sample app moves its JSON app settings files and *hostsettings.json* with the following **&lt;Content:&gt;** item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a><span data-ttu-id="2ac1a-202">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="2ac1a-202">ConfigureServices</span></span>

<span data-ttu-id="2ac1a-203">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) 會將服務新增至應用程式的[相依性插入](xref:fundamentals/dependency-injection)容器。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-203">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="2ac1a-204">`ConfigureServices` 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-204">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="2ac1a-205">託管服務是具有背景工作邏輯的類別，能夠實作 [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) 介面。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-205">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="2ac1a-206">如需詳細資訊，請參閱<xref:fundamentals/host/hosted-services>。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-206">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="2ac1a-207">[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)使用 `AddHostedService` 擴充方法，將存留期事件 `LifetimeEventsHostedService` 和計時背景工作 `TimedHostedService` 等服務新增至應用程式：</span><span class="sxs-lookup"><span data-stu-id="2ac1a-207">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="2ac1a-208">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="2ac1a-208">ConfigureLogging</span></span>

<span data-ttu-id="2ac1a-209">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) 會新增用來設定所提供 [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder) 的委派。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-209">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adds a delegate for configuring the provided [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span></span> <span data-ttu-id="2ac1a-210">`ConfigureLogging` 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-210">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="2ac1a-211">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="2ac1a-211">UseConsoleLifetime</span></span>

<span data-ttu-id="2ac1a-212">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) 會接聽 `Ctrl+C`/SIGINT 或 SIGTERM，並呼叫 [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) 以啟動關機程序。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-212">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) listens for `Ctrl+C`/SIGINT or SIGTERM and calls [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) to start the shutdown process.</span></span> <span data-ttu-id="2ac1a-213">`UseConsoleLifetime` 會解除封鎖 [RunAsync](#runasync) 和 [WaitForShutdownAsync](#waitforshutdownasync) 等延伸模組。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-213">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="2ac1a-214">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) 會預先註冊為預設存留期實作。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-214">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="2ac1a-215">系統會使用最後一個註冊的存留期。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-215">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="2ac1a-216">容器設定</span><span class="sxs-lookup"><span data-stu-id="2ac1a-216">Container configuration</span></span>

<span data-ttu-id="2ac1a-217">為支援插入其他容器，主機可以接受 [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1)。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-217">To support plugging in other containers, the host can accept an [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span></span> <span data-ttu-id="2ac1a-218">提供處理站不是 DI 容器註冊的一部分，而是用來建立實體 DI 容器的主機內建功能。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-218">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="2ac1a-219">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) 會覆寫用來建立應用程式服務提供者的預設處理站。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-219">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="2ac1a-220">自訂容器設定是由 [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) 方法管理。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-220">Custom container configuration is managed by the [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) method.</span></span> <span data-ttu-id="2ac1a-221">`ConfigureContainer` 提供在基礎主機 API 上設定容器的強型別體驗。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-221">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="2ac1a-222">`ConfigureContainer` 可以多次呼叫，其結果是累加的。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-222">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="2ac1a-223">建立應用程式的服務容器：</span><span class="sxs-lookup"><span data-stu-id="2ac1a-223">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="2ac1a-224">提供服務容器處理站：</span><span class="sxs-lookup"><span data-stu-id="2ac1a-224">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="2ac1a-225">使用處理站，並設定應用程式的自訂服務容器：</span><span class="sxs-lookup"><span data-stu-id="2ac1a-225">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="2ac1a-226">擴充性</span><span class="sxs-lookup"><span data-stu-id="2ac1a-226">Extensibility</span></span>

<span data-ttu-id="2ac1a-227">主機擴充性是透過 `IHostBuilder` 上的擴充方法執行。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-227">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="2ac1a-228">下列範例示範擴充方法如何使用 [RabbitMQ](https://www.rabbitmq.com/) 擴充 `IHostBuilder` 實作。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-228">The following example shows how an extension method extends an `IHostBuilder` implementation with [RabbitMQ](https://www.rabbitmq.com/).</span></span> <span data-ttu-id="2ac1a-229">此擴充方法 (位於應用程式中的其他位置) 會註冊 RabbitMQ `IHostedService`：</span><span class="sxs-lookup"><span data-stu-id="2ac1a-229">The extension method (elsewhere in the app) registers a RabbitMQ `IHostedService`:</span></span>

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a><span data-ttu-id="2ac1a-230">管理主機</span><span class="sxs-lookup"><span data-stu-id="2ac1a-230">Manage the host</span></span>

<span data-ttu-id="2ac1a-231">[IHost](/dotnet/api/microsoft.extensions.hosting.ihost) 實作都負責啟動及停止服務容器中已註冊的 `IHostedService` 實作。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-231">The [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="2ac1a-232">執行</span><span class="sxs-lookup"><span data-stu-id="2ac1a-232">Run</span></span>

<span data-ttu-id="2ac1a-233">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) 會執行應用程式並封鎖呼叫執行緒，直到主機關閉為止：</span><span class="sxs-lookup"><span data-stu-id="2ac1a-233">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="2ac1a-234">RunAsync</span><span class="sxs-lookup"><span data-stu-id="2ac1a-234">RunAsync</span></span>

<span data-ttu-id="2ac1a-235">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) 會執行應用程式，並傳回觸發取消語彙基元或關機時所完成的 `Task`：</span><span class="sxs-lookup"><span data-stu-id="2ac1a-235">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="2ac1a-236">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="2ac1a-236">RunConsoleAsync</span></span>

<span data-ttu-id="2ac1a-237">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) 會啟用主控台支援、建置和啟動主機，以及等候 `Ctrl+C`/SIGINT 或 SIGTERM 關機。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-237">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="2ac1a-238">Start 和 StopAsync</span><span class="sxs-lookup"><span data-stu-id="2ac1a-238">Start and StopAsync</span></span>

<span data-ttu-id="2ac1a-239">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) 會同步啟動主機。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-239">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) starts the host synchronously.</span></span>

<span data-ttu-id="2ac1a-240">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) 會嘗試在提供的逾時內停止主機。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-240">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="2ac1a-241">StartAsync 和 StopAsync</span><span class="sxs-lookup"><span data-stu-id="2ac1a-241">StartAsync and StopAsync</span></span>

<span data-ttu-id="2ac1a-242">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) 會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-242">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) starts the app.</span></span>

<span data-ttu-id="2ac1a-243">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) 會停止應用程式。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-243">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="2ac1a-244">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="2ac1a-244">WaitForShutdown</span></span>

<span data-ttu-id="2ac1a-245">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) 是透過 [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime) 觸發，例如 [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (接聽 `Ctrl+C`/SIGINT 或 SIGTERM)。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-245">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) is triggered via the [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), such as [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="2ac1a-246">`WaitForShutdown` 會呼叫 [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync)。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-246">`WaitForShutdown` calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="2ac1a-247">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="2ac1a-247">WaitForShutdownAsync</span></span>

<span data-ttu-id="2ac1a-248">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) 會傳回透過指定語彙基元觸發關機時所完成的 `Task`，並呼叫 [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync)。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-248">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) returns a `Task` that completes when shutdown is triggered via the given token and calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="external-control"></a><span data-ttu-id="2ac1a-249">外部控制</span><span class="sxs-lookup"><span data-stu-id="2ac1a-249">External control</span></span>

<span data-ttu-id="2ac1a-250">主機的外部控制可以使用可從外部呼叫的方法來達成：</span><span class="sxs-lookup"><span data-stu-id="2ac1a-250">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="2ac1a-251">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) 是在 [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) 開始時呼叫，並等到完成後再繼續進行。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-251">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) is called at the start of [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), which waits until it's complete before continuing.</span></span> <span data-ttu-id="2ac1a-252">這可用來將啟動延遲到外部事件發出訊號為止。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-252">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="2ac1a-253">IHostingEnvironment 介面</span><span class="sxs-lookup"><span data-stu-id="2ac1a-253">IHostingEnvironment interface</span></span>

<span data-ttu-id="2ac1a-254">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) 提供應用程式主控環境的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-254">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) provides information about the app's hosting environment.</span></span> <span data-ttu-id="2ac1a-255">使用[建構函式插入](xref:fundamentals/dependency-injection)以取得 `IHostingEnvironment`，才能使用其屬性和擴充方法：</span><span class="sxs-lookup"><span data-stu-id="2ac1a-255">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="2ac1a-256">如需詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-256">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="2ac1a-257">IApplicationLifetime 介面</span><span class="sxs-lookup"><span data-stu-id="2ac1a-257">IApplicationLifetime interface</span></span>

<span data-ttu-id="2ac1a-258">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) 允許啟動後和關機活動，包括順利關機要求。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-258">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="2ac1a-259">在介面上的三個屬性是用來註冊定義啟動和關閉事件之 `Action` 方法的取消語彙基元。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-259">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="2ac1a-260">取消語彙基元</span><span class="sxs-lookup"><span data-stu-id="2ac1a-260">Cancellation Token</span></span> | <span data-ttu-id="2ac1a-261">觸發時機&#8230;</span><span class="sxs-lookup"><span data-stu-id="2ac1a-261">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| [<span data-ttu-id="2ac1a-262">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="2ac1a-262">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="2ac1a-263">已完全啟動主機。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-263">The host has fully started.</span></span> |
| [<span data-ttu-id="2ac1a-264">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="2ac1a-264">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="2ac1a-265">主機正在完成正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-265">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="2ac1a-266">應該處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-266">All requests should be processed.</span></span> <span data-ttu-id="2ac1a-267">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-267">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="2ac1a-268">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="2ac1a-268">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="2ac1a-269">主機正在執行正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-269">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="2ac1a-270">可能仍在處理要求。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-270">Requests may still be processing.</span></span> <span data-ttu-id="2ac1a-271">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-271">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="2ac1a-272">建構函式將 `IApplicationLifetime` 服務插入任何類別。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-272">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="2ac1a-273">[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)使用建構函式插入 `LifetimeEventsHostedService` 類別 (`IHostedService` 實作) 來註冊事件。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-273">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="2ac1a-274">*LifetimeEventsHostedService.cs*：</span><span class="sxs-lookup"><span data-stu-id="2ac1a-274">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="2ac1a-275">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) 會要求應用程式終止。</span><span class="sxs-lookup"><span data-stu-id="2ac1a-275">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="2ac1a-276">當呼叫類別的 `Shutdown` 方法時，下列類別使用 `StopApplication` 來順利關閉應用程式：</span><span class="sxs-lookup"><span data-stu-id="2ac1a-276">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="2ac1a-277">其他資源</span><span class="sxs-lookup"><span data-stu-id="2ac1a-277">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
* [<span data-ttu-id="2ac1a-278">GitHub 上的主控存放庫範例</span><span class="sxs-lookup"><span data-stu-id="2ac1a-278">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
