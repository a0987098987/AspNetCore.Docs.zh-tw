---
title: ASP.NET Core Web 主機
author: guardrex
description: 了解 ASP.NET Core 中的 Web 主機，其負責啟動應用程式及管理存留期。
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: fundamentals/host/web-host
ms.openlocfilehash: c5d5b723b31a5c211a47e378e50be858fda0b2bd
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313790"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="7b02d-103">ASP.NET Core Web 主機</span><span class="sxs-lookup"><span data-stu-id="7b02d-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="7b02d-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7b02d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7b02d-105">ASP.NET Core 應用程式會設定並啟動「主機」  。</span><span class="sxs-lookup"><span data-stu-id="7b02d-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="7b02d-106">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="7b02d-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="7b02d-107">至少，主機會設定伺服器和要求處理管線。</span><span class="sxs-lookup"><span data-stu-id="7b02d-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="7b02d-108">主機也可以設定記錄、相依性插入和設定。</span><span class="sxs-lookup"><span data-stu-id="7b02d-108">The host can also set up logging, dependency injection, and configuration.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7b02d-109">此文章涵蓋 Web 主機，而且我們僅因為回溯相容性而繼續提供它。</span><span class="sxs-lookup"><span data-stu-id="7b02d-109">This article covers the Web Host, which remains available only for backward compatibility.</span></span> <span data-ttu-id="7b02d-110">建議為所有應用程式類型使用[一般主機](xref:fundamentals/host/generic-host)。</span><span class="sxs-lookup"><span data-stu-id="7b02d-110">The [Generic Host](xref:fundamentals/host/generic-host) is recommended for all app types.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="7b02d-111">此文章涵蓋 Web 主機，它適用於裝載 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7b02d-111">This article covers the Web Host, which is for hosting web apps.</span></span> <span data-ttu-id="7b02d-112">針對其他類型的應用程式，請使用[一般主機](xref:fundamentals/host/generic-host)。</span><span class="sxs-lookup"><span data-stu-id="7b02d-112">For other kinds of apps, use the [Generic Host](xref:fundamentals/host/generic-host).</span></span>

::: moniker-end

## <a name="set-up-a-host"></a><span data-ttu-id="7b02d-113">設定主機</span><span class="sxs-lookup"><span data-stu-id="7b02d-113">Set up a host</span></span>

<span data-ttu-id="7b02d-114">使用 ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)) 的執行個體建立主機。</span><span class="sxs-lookup"><span data-stu-id="7b02d-114">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="7b02d-115">這通常在應用程式的進入點執行，也就是 `Main` 方法。</span><span class="sxs-lookup"><span data-stu-id="7b02d-115">This is typically performed in the app's entry point, the `Main` method.</span></span>

<span data-ttu-id="7b02d-116">在專案範本中，`Main` 位於 *Program.cs*。</span><span class="sxs-lookup"><span data-stu-id="7b02d-116">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="7b02d-117">一般的應用程式會呼叫 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 來開始設定主機：</span><span class="sxs-lookup"><span data-stu-id="7b02d-117">A typical app calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

<span data-ttu-id="7b02d-118">呼叫 `CreateDefaultBuilder` 的程式碼位於名為 `CreateWebHostBuilder` 的方法中，這將它與 `Main` 中呼叫產生器物件上之 `Run` 的方法分開。</span><span class="sxs-lookup"><span data-stu-id="7b02d-118">The code that calls `CreateDefaultBuilder` is in a method named `CreateWebHostBuilder`, which separates it from the code in `Main` that calls `Run` on the builder object.</span></span> <span data-ttu-id="7b02d-119">若您使用e [Entity Framework Core 工具](/ef/core/miscellaneous/cli/)，則此分開是必要的。</span><span class="sxs-lookup"><span data-stu-id="7b02d-119">This separation is required if you use [Entity Framework Core tools](/ef/core/miscellaneous/cli/).</span></span> <span data-ttu-id="7b02d-120">工具預期找到它們可以在設計階段呼叫的 `CreateWebHostBuilder` 方法，以在不執行應用程式的情況下設定主機。</span><span class="sxs-lookup"><span data-stu-id="7b02d-120">The tools expect to find a `CreateWebHostBuilder` method that they can call at design time to configure the host without running the app.</span></span> <span data-ttu-id="7b02d-121">替代方式是實作 `IDesignTimeDbContextFactory`。</span><span class="sxs-lookup"><span data-stu-id="7b02d-121">An alternative is to implement `IDesignTimeDbContextFactory`.</span></span> <span data-ttu-id="7b02d-122">如需詳細資訊，請參閱[設計階段 DbContext 建立](/ef/core/miscellaneous/cli/dbcontext-creation)。</span><span class="sxs-lookup"><span data-stu-id="7b02d-122">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

<span data-ttu-id="7b02d-123">`CreateDefaultBuilder` 會執行下列工作：</span><span class="sxs-lookup"><span data-stu-id="7b02d-123">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="7b02d-124">使用應用程式的主機組態提供者，將 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器設定為網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="7b02d-124">Configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server using the app's hosting configuration providers.</span></span> <span data-ttu-id="7b02d-125">如需 Kestrel 伺服器的預設選項，請參閱 <xref:fundamentals/servers/kestrel#kestrel-options>。</span><span class="sxs-lookup"><span data-stu-id="7b02d-125">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="7b02d-126">設定 [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) 所傳回路徑的內容根目錄。</span><span class="sxs-lookup"><span data-stu-id="7b02d-126">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="7b02d-127">從下列項目載入[主機組態](#host-configuration-values)：</span><span class="sxs-lookup"><span data-stu-id="7b02d-127">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="7b02d-128">前面加上 `ASPNETCORE_` 的環境變數 (例如，`ASPNETCORE_ENVIRONMENT`)。</span><span class="sxs-lookup"><span data-stu-id="7b02d-128">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="7b02d-129">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="7b02d-129">Command-line arguments.</span></span>
* <span data-ttu-id="7b02d-130">以下列順序載入應用程式組態：</span><span class="sxs-lookup"><span data-stu-id="7b02d-130">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="7b02d-131">*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="7b02d-131">*appsettings.json*.</span></span>
  * <span data-ttu-id="7b02d-132">*appsettings.{Environment}.json*</span><span class="sxs-lookup"><span data-stu-id="7b02d-132">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="7b02d-133">應用程式在使用項目組件之 `Development` 環境中執行時的[秘密管理員](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="7b02d-133">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="7b02d-134">環境變數。</span><span class="sxs-lookup"><span data-stu-id="7b02d-134">Environment variables.</span></span>
  * <span data-ttu-id="7b02d-135">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="7b02d-135">Command-line arguments.</span></span>
* <span data-ttu-id="7b02d-136">設定主控台和偵錯輸出的[記錄](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="7b02d-136">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="7b02d-137">記錄包含 *appsettings.json* 或 *appsettings.{Environment}.json* 檔案的記錄組態區段中指定的[記錄檔篩選](xref:fundamentals/logging/index#log-filtering)規則。</span><span class="sxs-lookup"><span data-stu-id="7b02d-137">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="7b02d-138">以 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)在 IIS 背後執行時，`CreateDefaultBuilder` 會啟用 [IIS 整合](xref:host-and-deploy/iis/index)，以設定應用程式的基底位址和連接埠。</span><span class="sxs-lookup"><span data-stu-id="7b02d-138">When running behind IIS with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` enables [IIS Integration](xref:host-and-deploy/iis/index), which configures the app's base address and port.</span></span> <span data-ttu-id="7b02d-139">IIS 整合也會設定應用程式以[擷取啟動錯誤](#capture-startup-errors)。</span><span class="sxs-lookup"><span data-stu-id="7b02d-139">IIS Integration also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="7b02d-140">如需 IIS 預設選項，請參閱 <xref:host-and-deploy/iis/index#iis-options>。</span><span class="sxs-lookup"><span data-stu-id="7b02d-140">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="7b02d-141">如果應用程式的環境是「開發」，請將 [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) 設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="7b02d-141">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="7b02d-142">如需詳細資訊，請參閱[範圍驗證](#scope-validation)。</span><span class="sxs-lookup"><span data-stu-id="7b02d-142">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="7b02d-143">`CreateDefaultBuilder` 所定義的組態可以透過 [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration)、[ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging)，以及 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) 的其他方法和擴充方法加以覆寫及擴增。</span><span class="sxs-lookup"><span data-stu-id="7b02d-143">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="7b02d-144">數個範例如下：</span><span class="sxs-lookup"><span data-stu-id="7b02d-144">A few examples follow:</span></span>

* <span data-ttu-id="7b02d-145">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) 用來指定應用程式的其他 `IConfiguration`。</span><span class="sxs-lookup"><span data-stu-id="7b02d-145">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="7b02d-146">下列 `ConfigureAppConfiguration` 呼叫會新增委派，以在 *appsettings.xml* 檔案中包含應用程式組態。</span><span class="sxs-lookup"><span data-stu-id="7b02d-146">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="7b02d-147">可能會多次呼叫 `ConfigureAppConfiguration`。</span><span class="sxs-lookup"><span data-stu-id="7b02d-147">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="7b02d-148">請注意，此組態不適用於主機 (例如，伺服器 URL 或環境)。</span><span class="sxs-lookup"><span data-stu-id="7b02d-148">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="7b02d-149">請參閱[主機組態值](#host-configuration-values)一節。</span><span class="sxs-lookup"><span data-stu-id="7b02d-149">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="7b02d-150">下列 `ConfigureLogging` 呼叫會新增委派，將最低的記錄層級 ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) 設定為 [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel)。</span><span class="sxs-lookup"><span data-stu-id="7b02d-150">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="7b02d-151">此設定會覆寫 `CreateDefaultBuilder` 所設定之 *ppsettings.Development.json* (`LogLevel.Debug`) 和 *appsettings.Production.json* (`LogLevel.Error`) 中的設定。</span><span class="sxs-lookup"><span data-stu-id="7b02d-151">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="7b02d-152">可能會多次呼叫 `ConfigureLogging`。</span><span class="sxs-lookup"><span data-stu-id="7b02d-152">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="7b02d-153">下列 `ConfigureKestrel` 呼叫會覆寫當 `CreateDefaultBuilder` 設定 Kestrel 時建立的預設 [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) 30,000,000 位元組：</span><span class="sxs-lookup"><span data-stu-id="7b02d-153">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="7b02d-154">下列 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) 呼叫會覆寫當 `CreateDefaultBuilder` 設定 Kestrel 時建立的預設 [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) 30,000,000 位元組：</span><span class="sxs-lookup"><span data-stu-id="7b02d-154">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

<span data-ttu-id="7b02d-155">「內容根目錄」  會決定主機搜尋內容檔案 (例如 MVC 檢視檔案) 的位置。</span><span class="sxs-lookup"><span data-stu-id="7b02d-155">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="7b02d-156">從專案的根資料夾啟動應用程式時，會使用專案的根資料夾作為內容根目錄。</span><span class="sxs-lookup"><span data-stu-id="7b02d-156">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="7b02d-157">這是 [Visual Studio](https://visualstudio.microsoft.com) 和 [dotnet 新範本](/dotnet/core/tools/dotnet-new)中使用的預設值。</span><span class="sxs-lookup"><span data-stu-id="7b02d-157">This is the default used in [Visual Studio](https://visualstudio.microsoft.com) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="7b02d-158">如需應用程式組態的詳細資訊，請參閱 <xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="7b02d-158">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="7b02d-159">作為使用靜態 `CreateDefaultBuilder` 方法的替代做法，ASP.NET Core 2.x 支援從 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 建立主機的方法。</span><span class="sxs-lookup"><span data-stu-id="7b02d-159">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span>

<span data-ttu-id="7b02d-160">設定主機時，可以提供 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) 和 [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices) 方法。</span><span class="sxs-lookup"><span data-stu-id="7b02d-160">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices) methods can be provided.</span></span> <span data-ttu-id="7b02d-161">如果指定 `Startup` 類別，它必須定義 `Configure` 方法。</span><span class="sxs-lookup"><span data-stu-id="7b02d-161">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="7b02d-162">如需詳細資訊，請參閱 <xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="7b02d-162">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="7b02d-163">多次呼叫 `ConfigureServices` 會彼此附加。</span><span class="sxs-lookup"><span data-stu-id="7b02d-163">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="7b02d-164">對 `WebHostBuilder` 多次呼叫 `Configure` 或 `UseStartup` 則會取代先前的設定。</span><span class="sxs-lookup"><span data-stu-id="7b02d-164">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="7b02d-165">主機組態值</span><span class="sxs-lookup"><span data-stu-id="7b02d-165">Host configuration values</span></span>

<span data-ttu-id="7b02d-166">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 依賴下列方法來設定主機組態值：</span><span class="sxs-lookup"><span data-stu-id="7b02d-166">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="7b02d-167">主機建立器組態，其中包含 `ASPNETCORE_{configurationKey}` 格式的環境變數。</span><span class="sxs-lookup"><span data-stu-id="7b02d-167">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="7b02d-168">例如，`ASPNETCORE_ENVIRONMENT`。</span><span class="sxs-lookup"><span data-stu-id="7b02d-168">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="7b02d-169">[UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) 和 [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 之類的延伸模組 (請參閱[覆寫組態](#override-configuration)一節)。</span><span class="sxs-lookup"><span data-stu-id="7b02d-169">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="7b02d-170">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) 和相關聯的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="7b02d-170">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="7b02d-171">使用 `UseSetting` 設定值時，此值會設定為字串，而不論其類型為何。</span><span class="sxs-lookup"><span data-stu-id="7b02d-171">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="7b02d-172">主機使用設定最後一個值的任何選項。</span><span class="sxs-lookup"><span data-stu-id="7b02d-172">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="7b02d-173">如需詳細資訊，請參閱下一節的[覆寫組態](#override-configuration)。</span><span class="sxs-lookup"><span data-stu-id="7b02d-173">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="7b02d-174">應用程式索引鍵 (名稱)</span><span class="sxs-lookup"><span data-stu-id="7b02d-174">Application Key (Name)</span></span>

<span data-ttu-id="7b02d-175">在主機建構期間呼叫 [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) 或 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) 時，會自動設定 [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) 屬性。</span><span class="sxs-lookup"><span data-stu-id="7b02d-175">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="7b02d-176">該值會設定為包含應用程式進入點的組件名稱。</span><span class="sxs-lookup"><span data-stu-id="7b02d-176">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="7b02d-177">若要明確設定該值，請使用 [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey)：</span><span class="sxs-lookup"><span data-stu-id="7b02d-177">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="7b02d-178">**索引鍵**：applicationName</span><span class="sxs-lookup"><span data-stu-id="7b02d-178">**Key**: applicationName</span></span>  
<span data-ttu-id="7b02d-179">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="7b02d-179">**Type**: *string*</span></span>  
<span data-ttu-id="7b02d-180">**預設**：包含應用程式進入點的組件名稱。</span><span class="sxs-lookup"><span data-stu-id="7b02d-180">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="7b02d-181">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="7b02d-181">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="7b02d-182">**環境變數**：`ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="7b02d-182">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a><span data-ttu-id="7b02d-183">擷取啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="7b02d-183">Capture Startup Errors</span></span>

<span data-ttu-id="7b02d-184">此設定會控制啟動錯誤的擷取。</span><span class="sxs-lookup"><span data-stu-id="7b02d-184">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="7b02d-185">**索引鍵**：captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="7b02d-185">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="7b02d-186">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="7b02d-186">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="7b02d-187">**預設**：預設為 `false`，除非應用程式執行時在 IIS 背後有 Kestrel，此時預設即為 `true`。</span><span class="sxs-lookup"><span data-stu-id="7b02d-187">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="7b02d-188">**設定使用**：`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="7b02d-188">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="7b02d-189">**環境變數**：`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="7b02d-189">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="7b02d-190">當它為 `false` 時，啟動期間發生的錯誤會導致主機結束。</span><span class="sxs-lookup"><span data-stu-id="7b02d-190">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="7b02d-191">當它為 `true` 時，主機會擷取啟動期間的例外狀況，並嘗試啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="7b02d-191">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a><span data-ttu-id="7b02d-192">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="7b02d-192">Content Root</span></span>

<span data-ttu-id="7b02d-193">此設定可決定 ASP.NET Core 開始搜尋內容檔案 (例如 MVC 檢視) 的位置。</span><span class="sxs-lookup"><span data-stu-id="7b02d-193">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="7b02d-194">**索引鍵**：contentRoot</span><span class="sxs-lookup"><span data-stu-id="7b02d-194">**Key**: contentRoot</span></span>  
<span data-ttu-id="7b02d-195">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="7b02d-195">**Type**: *string*</span></span>  
<span data-ttu-id="7b02d-196">**預設**：預設為應用程式組件所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="7b02d-196">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="7b02d-197">**設定使用**：`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="7b02d-197">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="7b02d-198">**環境變數**：`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="7b02d-198">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="7b02d-199">內容根目錄也作為 [Web 根目錄設定](#web-root)的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="7b02d-199">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="7b02d-200">如果路徑不存在，就無法啟動主機。</span><span class="sxs-lookup"><span data-stu-id="7b02d-200">If the path doesn't exist, the host fails to start.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

### <a name="detailed-errors"></a><span data-ttu-id="7b02d-201">詳細錯誤</span><span class="sxs-lookup"><span data-stu-id="7b02d-201">Detailed Errors</span></span>

<span data-ttu-id="7b02d-202">決定是否應該擷取詳細的錯誤。</span><span class="sxs-lookup"><span data-stu-id="7b02d-202">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="7b02d-203">**索引鍵**：detailedErrors</span><span class="sxs-lookup"><span data-stu-id="7b02d-203">**Key**: detailedErrors</span></span>  
<span data-ttu-id="7b02d-204">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="7b02d-204">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="7b02d-205">**預設值**：false</span><span class="sxs-lookup"><span data-stu-id="7b02d-205">**Default**: false</span></span>  
<span data-ttu-id="7b02d-206">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="7b02d-206">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="7b02d-207">**環境變數**：`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="7b02d-207">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="7b02d-208">啟用時 (或當 <a href="#environment">Environment</a> 設定為 `Development` 時)，應用程式會擷取詳細例外狀況。</span><span class="sxs-lookup"><span data-stu-id="7b02d-208">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a><span data-ttu-id="7b02d-209">環境</span><span class="sxs-lookup"><span data-stu-id="7b02d-209">Environment</span></span>

<span data-ttu-id="7b02d-210">設定應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="7b02d-210">Sets the app's environment.</span></span>

<span data-ttu-id="7b02d-211">**索引鍵**：environment</span><span class="sxs-lookup"><span data-stu-id="7b02d-211">**Key**: environment</span></span>  
<span data-ttu-id="7b02d-212">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="7b02d-212">**Type**: *string*</span></span>  
<span data-ttu-id="7b02d-213">**預設**：生產環境</span><span class="sxs-lookup"><span data-stu-id="7b02d-213">**Default**: Production</span></span>  
<span data-ttu-id="7b02d-214">**設定使用**：`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="7b02d-214">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="7b02d-215">**環境變數**：`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="7b02d-215">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="7b02d-216">環境可以設定為任何值。</span><span class="sxs-lookup"><span data-stu-id="7b02d-216">The environment can be set to any value.</span></span> <span data-ttu-id="7b02d-217">架構定義的值包括 `Development`、`Staging` 和 `Production`。</span><span class="sxs-lookup"><span data-stu-id="7b02d-217">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="7b02d-218">值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="7b02d-218">Values aren't case sensitive.</span></span> <span data-ttu-id="7b02d-219">根據預設，*Environment* 是從 `ASPNETCORE_ENVIRONMENT` 環境變數讀取。</span><span class="sxs-lookup"><span data-stu-id="7b02d-219">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="7b02d-220">使用 [Visual Studio](https://visualstudio.microsoft.com) 時，可能會在 *launchSettings.json* 檔案設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="7b02d-220">When using [Visual Studio](https://visualstudio.microsoft.com), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="7b02d-221">如需詳細資訊，請參閱 <xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="7b02d-221">For more information, see <xref:fundamentals/environments>.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="7b02d-222">裝載啟動組件</span><span class="sxs-lookup"><span data-stu-id="7b02d-222">Hosting Startup Assemblies</span></span>

<span data-ttu-id="7b02d-223">設定應用程式的裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="7b02d-223">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="7b02d-224">**索引鍵**：hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="7b02d-224">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="7b02d-225">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="7b02d-225">**Type**: *string*</span></span>  
<span data-ttu-id="7b02d-226">**預設**：空字串</span><span class="sxs-lookup"><span data-stu-id="7b02d-226">**Default**: Empty string</span></span>  
<span data-ttu-id="7b02d-227">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="7b02d-227">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="7b02d-228">**環境變數**：`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="7b02d-228">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="7b02d-229">在啟動時載入的裝載啟動組件字串，以分號分隔。</span><span class="sxs-lookup"><span data-stu-id="7b02d-229">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="7b02d-230">雖然設定值會預設為空字串，但裝載啟動組件一律會包含應用程式的組件。</span><span class="sxs-lookup"><span data-stu-id="7b02d-230">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="7b02d-231">提供裝載啟動組件時，它們會新增至應用程式的組件，以便在應用程式在啟動時建置其通用服務時載入。</span><span class="sxs-lookup"><span data-stu-id="7b02d-231">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a><span data-ttu-id="7b02d-232">HTTPS 連接埠</span><span class="sxs-lookup"><span data-stu-id="7b02d-232">HTTPS Port</span></span>

<span data-ttu-id="7b02d-233">設定 HTTPS 重新導向連接埠。</span><span class="sxs-lookup"><span data-stu-id="7b02d-233">Set the HTTPS redirect port.</span></span> <span data-ttu-id="7b02d-234">用於[強制 HTTPS](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="7b02d-234">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="7b02d-235">**索引鍵**：https_port **類型**：*string*
**預設**：未設定預設值。</span><span class="sxs-lookup"><span data-stu-id="7b02d-235">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="7b02d-236">**設定使用**：`UseSetting`
**環境變數**：`ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="7b02d-236">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="7b02d-237">裝載啟動排除組件</span><span class="sxs-lookup"><span data-stu-id="7b02d-237">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="7b02d-238">在啟動時排除以分號分隔的裝載啟動組件字串。</span><span class="sxs-lookup"><span data-stu-id="7b02d-238">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="7b02d-239">**索引鍵**：hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="7b02d-239">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="7b02d-240">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="7b02d-240">**Type**: *string*</span></span>  
<span data-ttu-id="7b02d-241">**預設**：空字串</span><span class="sxs-lookup"><span data-stu-id="7b02d-241">**Default**: Empty string</span></span>  
<span data-ttu-id="7b02d-242">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="7b02d-242">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="7b02d-243">**環境變數**：`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="7b02d-243">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a><span data-ttu-id="7b02d-244">偏好裝載 URL</span><span class="sxs-lookup"><span data-stu-id="7b02d-244">Prefer Hosting URLs</span></span>

<span data-ttu-id="7b02d-245">表示主機是否應接聽使用 `WebHostBuilder` 設定的 URL，而不是 `IServer` 實作所設定的 URL。</span><span class="sxs-lookup"><span data-stu-id="7b02d-245">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="7b02d-246">**索引鍵**：preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="7b02d-246">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="7b02d-247">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="7b02d-247">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="7b02d-248">**預設值**：true</span><span class="sxs-lookup"><span data-stu-id="7b02d-248">**Default**: true</span></span>  
<span data-ttu-id="7b02d-249">**設定使用**：`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="7b02d-249">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="7b02d-250">**環境變數**：`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="7b02d-250">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a><span data-ttu-id="7b02d-251">防止裝載啟動</span><span class="sxs-lookup"><span data-stu-id="7b02d-251">Prevent Hosting Startup</span></span>

<span data-ttu-id="7b02d-252">可防止自動載入裝載啟動組件，包括應用程式組件所設定的裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="7b02d-252">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="7b02d-253">如需詳細資訊，請參閱 <xref:fundamentals/configuration/platform-specific-configuration>。</span><span class="sxs-lookup"><span data-stu-id="7b02d-253">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="7b02d-254">**索引鍵**preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="7b02d-254">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="7b02d-255">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="7b02d-255">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="7b02d-256">**預設值**：false</span><span class="sxs-lookup"><span data-stu-id="7b02d-256">**Default**: false</span></span>  
<span data-ttu-id="7b02d-257">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="7b02d-257">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="7b02d-258">**環境變數**：`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="7b02d-258">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a><span data-ttu-id="7b02d-259">伺服器 URL</span><span class="sxs-lookup"><span data-stu-id="7b02d-259">Server URLs</span></span>

<span data-ttu-id="7b02d-260">表示伺服器應該接聽要求的 IP 位址或主機位址，以及連接埠和通訊協定。</span><span class="sxs-lookup"><span data-stu-id="7b02d-260">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="7b02d-261">**索引鍵**：urls</span><span class="sxs-lookup"><span data-stu-id="7b02d-261">**Key**: urls</span></span>  
<span data-ttu-id="7b02d-262">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="7b02d-262">**Type**: *string*</span></span>  
<span data-ttu-id="7b02d-263">**預設值**： http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="7b02d-263">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="7b02d-264">**設定使用**：`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="7b02d-264">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="7b02d-265">**環境變數**：`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="7b02d-265">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="7b02d-266">設定為伺服器應該回應的 URL 前置清單，並以分號 (;) 分隔。</span><span class="sxs-lookup"><span data-stu-id="7b02d-266">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="7b02d-267">例如，`http://localhost:123`。</span><span class="sxs-lookup"><span data-stu-id="7b02d-267">For example, `http://localhost:123`.</span></span> <span data-ttu-id="7b02d-268">使用 "\*"，表示伺服器應接聽任何 IP 位址或主機名稱上的要求，並使用指定的連接埠和通訊協定 (例如，`http://*:5000`)。</span><span class="sxs-lookup"><span data-stu-id="7b02d-268">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="7b02d-269">通訊協定 (`http://` 或 `https://`) 必須包含在每個 URL 中。</span><span class="sxs-lookup"><span data-stu-id="7b02d-269">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="7b02d-270">支援的格式會依伺服器而有所不同。</span><span class="sxs-lookup"><span data-stu-id="7b02d-270">Supported formats vary among servers.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="7b02d-271">Kestrel 有它自己的端點設定 API。</span><span class="sxs-lookup"><span data-stu-id="7b02d-271">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="7b02d-272">如需詳細資訊，請參閱 <xref:fundamentals/servers/kestrel#endpoint-configuration>。</span><span class="sxs-lookup"><span data-stu-id="7b02d-272">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="7b02d-273">關機逾時</span><span class="sxs-lookup"><span data-stu-id="7b02d-273">Shutdown Timeout</span></span>

<span data-ttu-id="7b02d-274">指定等待 Web 主機關機的時間長度。</span><span class="sxs-lookup"><span data-stu-id="7b02d-274">Specifies the amount of time to wait for Web Host to shut down.</span></span>

<span data-ttu-id="7b02d-275">**索引鍵**：shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="7b02d-275">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="7b02d-276">**類型**：*int*</span><span class="sxs-lookup"><span data-stu-id="7b02d-276">**Type**: *int*</span></span>  
<span data-ttu-id="7b02d-277">**預設**：5</span><span class="sxs-lookup"><span data-stu-id="7b02d-277">**Default**: 5</span></span>  
<span data-ttu-id="7b02d-278">**設定使用**：`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="7b02d-278">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="7b02d-279">**環境變數**：`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="7b02d-279">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="7b02d-280">雖然索引鍵接受 *int* 與 `UseSetting` (例如 `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`)，[UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) 擴充方法會採用 [TimeSpan](/dotnet/api/system.timespan)。</span><span class="sxs-lookup"><span data-stu-id="7b02d-280">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="7b02d-281">在逾時期間，裝載會：</span><span class="sxs-lookup"><span data-stu-id="7b02d-281">During the timeout period, hosting:</span></span>

* <span data-ttu-id="7b02d-282">觸發 [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping)。</span><span class="sxs-lookup"><span data-stu-id="7b02d-282">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="7b02d-283">嘗試停止託管的服務，並記錄無法停止之服務的任何錯誤。</span><span class="sxs-lookup"><span data-stu-id="7b02d-283">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="7b02d-284">如果在所有的託管服務停止之前逾時期限已到期，則應用程式關閉時，會停止任何剩餘的作用中服務。</span><span class="sxs-lookup"><span data-stu-id="7b02d-284">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="7b02d-285">即使服務尚未完成處理也會停止。</span><span class="sxs-lookup"><span data-stu-id="7b02d-285">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="7b02d-286">如果服務需要更多時間才能停止，請增加逾時。</span><span class="sxs-lookup"><span data-stu-id="7b02d-286">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a><span data-ttu-id="7b02d-287">啟動組件</span><span class="sxs-lookup"><span data-stu-id="7b02d-287">Startup Assembly</span></span>

<span data-ttu-id="7b02d-288">決定要搜尋 `Startup` 類別的組件。</span><span class="sxs-lookup"><span data-stu-id="7b02d-288">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="7b02d-289">**索引鍵**：startupAssembly</span><span class="sxs-lookup"><span data-stu-id="7b02d-289">**Key**: startupAssembly</span></span>  
<span data-ttu-id="7b02d-290">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="7b02d-290">**Type**: *string*</span></span>  
<span data-ttu-id="7b02d-291">**預設**：應用程式的組件</span><span class="sxs-lookup"><span data-stu-id="7b02d-291">**Default**: The app's assembly</span></span>  
<span data-ttu-id="7b02d-292">**設定使用**：`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="7b02d-292">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="7b02d-293">**環境變數**：`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="7b02d-293">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="7b02d-294">可以參考依名稱 (`string`) 或類型 (`TStartup`) 的組件。</span><span class="sxs-lookup"><span data-stu-id="7b02d-294">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="7b02d-295">如果呼叫多個 `UseStartup` 方法，最後一個將會優先。</span><span class="sxs-lookup"><span data-stu-id="7b02d-295">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a><span data-ttu-id="7b02d-296">Web 根目錄</span><span class="sxs-lookup"><span data-stu-id="7b02d-296">Web Root</span></span>

<span data-ttu-id="7b02d-297">設定應用程式靜態資產的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="7b02d-297">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="7b02d-298">**索引鍵**：webroot</span><span class="sxs-lookup"><span data-stu-id="7b02d-298">**Key**: webroot</span></span>  
<span data-ttu-id="7b02d-299">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="7b02d-299">**Type**: *string*</span></span>  
<span data-ttu-id="7b02d-300">**預設**：如果未指定，則預設為 "(Content Root)/wwwroot" (若該路徑存在的話)。</span><span class="sxs-lookup"><span data-stu-id="7b02d-300">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="7b02d-301">如果路徑不存在，則會使用無作業檔案提供者。</span><span class="sxs-lookup"><span data-stu-id="7b02d-301">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="7b02d-302">**設定使用**：`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="7b02d-302">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="7b02d-303">**環境變數**：`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="7b02d-303">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

## <a name="override-configuration"></a><span data-ttu-id="7b02d-304">覆寫組態</span><span class="sxs-lookup"><span data-stu-id="7b02d-304">Override configuration</span></span>

<span data-ttu-id="7b02d-305">使用 [Configuration](xref:fundamentals/configuration/index) 來設定 Web 主機。</span><span class="sxs-lookup"><span data-stu-id="7b02d-305">Use [Configuration](xref:fundamentals/configuration/index) to configure Web Host.</span></span> <span data-ttu-id="7b02d-306">在下列範例中，主機組態選擇性指定於 *hostsettings.json* 檔案中。</span><span class="sxs-lookup"><span data-stu-id="7b02d-306">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="7b02d-307">從 *hostsettings.json* 檔案載入的任何組態，可以用命令列引數加以覆寫。</span><span class="sxs-lookup"><span data-stu-id="7b02d-307">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="7b02d-308">建置的組態 (在 `config` 中) 用以使用 [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 來設定主機。</span><span class="sxs-lookup"><span data-stu-id="7b02d-308">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="7b02d-309">`IWebHostBuilder` 設定會新增到應用程式的組態，但反之則不然 &mdash;`ConfigureAppConfiguration` 並不會影響 `IWebHostBuilder` 組態。</span><span class="sxs-lookup"><span data-stu-id="7b02d-309">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

<span data-ttu-id="7b02d-310">首先使用 *hostsettings.json* 組態來覆寫 `UseUrls` 所提供的組態，其次是命令列引數組態：</span><span class="sxs-lookup"><span data-stu-id="7b02d-310">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            });
    }
}
```

<span data-ttu-id="7b02d-311">*hostsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="7b02d-311">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> <span data-ttu-id="7b02d-312">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 擴充方法目前無法剖析 `GetSection` 傳回的組態區段 (例如 `.UseConfiguration(Configuration.GetSection("section"))`。</span><span class="sxs-lookup"><span data-stu-id="7b02d-312">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="7b02d-313">`GetSection` 方法會將設定索引鍵篩選到要求的區段，但保留索引鍵上的區段名稱 (例如 `section:urls`、`section:environment`)。</span><span class="sxs-lookup"><span data-stu-id="7b02d-313">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="7b02d-314">`UseConfiguration` 方法預期索引鍵要符合 `WebHostBuilder` 索引鍵 (例如 `urls`、`environment`)。</span><span class="sxs-lookup"><span data-stu-id="7b02d-314">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="7b02d-315">索引鍵上的區段名稱存在可避免區段的值設定主機。</span><span class="sxs-lookup"><span data-stu-id="7b02d-315">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="7b02d-316">這個問題將在近期的版本中解決。</span><span class="sxs-lookup"><span data-stu-id="7b02d-316">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="7b02d-317">如需詳細資訊和因應措施，請參閱 [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (將設定區段傳遞到 WebHostBuilder.UseConfiguration 會使用完整索引鍵)。</span><span class="sxs-lookup"><span data-stu-id="7b02d-317">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="7b02d-318">`UseConfiguration` 只會將索引鍵從提供的 `IConfiguration` 複製到主機建立器的組態。</span><span class="sxs-lookup"><span data-stu-id="7b02d-318">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="7b02d-319">因此，為 JSON、INI 和 XML 設定檔設定 `reloadOnChange: true` 沒有任何作用。</span><span class="sxs-lookup"><span data-stu-id="7b02d-319">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="7b02d-320">若要指定主機在特定 URL 上執行，所要的值可以在執行 [dotnet run](/dotnet/core/tools/dotnet-run) 時從命令提示字元傳入。</span><span class="sxs-lookup"><span data-stu-id="7b02d-320">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="7b02d-321">命令列引數會覆寫 *hostsettings.json* 檔案的 `urls` 值，且伺服器會接聽連接埠 8080：</span><span class="sxs-lookup"><span data-stu-id="7b02d-321">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="7b02d-322">管理主機</span><span class="sxs-lookup"><span data-stu-id="7b02d-322">Manage the host</span></span>

<span data-ttu-id="7b02d-323">**執行**</span><span class="sxs-lookup"><span data-stu-id="7b02d-323">**Run**</span></span>

<span data-ttu-id="7b02d-324">`Run` 方法會啟動 Web 應用程式，並且封鎖呼叫執行緒，直到主機關閉為止：</span><span class="sxs-lookup"><span data-stu-id="7b02d-324">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="7b02d-325">**啟動**</span><span class="sxs-lookup"><span data-stu-id="7b02d-325">**Start**</span></span>

<span data-ttu-id="7b02d-326">藉由呼叫 `Start` 方法，以非封鎖方式執行主機：</span><span class="sxs-lookup"><span data-stu-id="7b02d-326">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="7b02d-327">如果 URL 的清單傳遞至 `Start` 方法，它會接聽指定的 URL：</span><span class="sxs-lookup"><span data-stu-id="7b02d-327">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

<span data-ttu-id="7b02d-328">應用程式可以使用預先設定的 `CreateDefaultBuilder` 預設值，使用便利的靜態方法初始化並啟動新的主機。</span><span class="sxs-lookup"><span data-stu-id="7b02d-328">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="7b02d-329">這些方法會啟動伺服器而無主控台輸出，且在使用 [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) 時等候中斷 (Ctrl-C/SIGINT 或 SIGTERM)：</span><span class="sxs-lookup"><span data-stu-id="7b02d-329">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="7b02d-330">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="7b02d-330">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="7b02d-331">使用 `RequestDelegate` 啟動：</span><span class="sxs-lookup"><span data-stu-id="7b02d-331">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="7b02d-332">在瀏覽器中提出要求給 `http://localhost:5000`，以收到 "Hello World!" 回應</span><span class="sxs-lookup"><span data-stu-id="7b02d-332">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="7b02d-333">`WaitForShutdown` 會封鎖，直到發出中斷 (Ctrl-C/SIGINT 或 SIGTERM)。</span><span class="sxs-lookup"><span data-stu-id="7b02d-333">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="7b02d-334">應用程式會顯示 `Console.WriteLine` 訊息並等候按鍵動作以便結束。</span><span class="sxs-lookup"><span data-stu-id="7b02d-334">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="7b02d-335">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="7b02d-335">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="7b02d-336">使用 URL 和 `RequestDelegate` 來啟動：</span><span class="sxs-lookup"><span data-stu-id="7b02d-336">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="7b02d-337">產生與 **Start(RequestDelegate app)** 相同的結果，除了應用程式會在 `http://localhost:8080` 回應。</span><span class="sxs-lookup"><span data-stu-id="7b02d-337">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="7b02d-338">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="7b02d-338">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="7b02d-339">使用 `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) 的執行個體來使用路由中介軟體：</span><span class="sxs-lookup"><span data-stu-id="7b02d-339">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="7b02d-340">使用下列瀏覽器要求與範例：</span><span class="sxs-lookup"><span data-stu-id="7b02d-340">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="7b02d-341">要求</span><span class="sxs-lookup"><span data-stu-id="7b02d-341">Request</span></span>                                    | <span data-ttu-id="7b02d-342">回應</span><span class="sxs-lookup"><span data-stu-id="7b02d-342">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="7b02d-343">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="7b02d-343">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="7b02d-344">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="7b02d-344">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="7b02d-345">擲回例外狀況，字串為 "ooops!"</span><span class="sxs-lookup"><span data-stu-id="7b02d-345">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="7b02d-346">擲回例外狀況，字串為 "Un oh!"</span><span class="sxs-lookup"><span data-stu-id="7b02d-346">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="7b02d-347">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="7b02d-347">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="7b02d-348">Hello World!</span><span class="sxs-lookup"><span data-stu-id="7b02d-348">Hello World!</span></span>                             |

<span data-ttu-id="7b02d-349">`WaitForShutdown` 會封鎖，直到發出中斷 (Ctrl-C/SIGINT 或 SIGTERM)。</span><span class="sxs-lookup"><span data-stu-id="7b02d-349">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="7b02d-350">應用程式會顯示 `Console.WriteLine` 訊息並等候按鍵動作以便結束。</span><span class="sxs-lookup"><span data-stu-id="7b02d-350">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="7b02d-351">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="7b02d-351">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="7b02d-352">使用 URL 和 `IRouteBuilder` 的執行個體：</span><span class="sxs-lookup"><span data-stu-id="7b02d-352">Use a URL and an instance of `IRouteBuilder`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="7b02d-353">產生與 **Start(Action&lt;IRouteBuilder&gt; routeBuilder)** 相同的結果，除了應用程式會在 `http://localhost:8080` 回應。</span><span class="sxs-lookup"><span data-stu-id="7b02d-353">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="7b02d-354">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="7b02d-354">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="7b02d-355">提供委派以設定 `IApplicationBuilder`：</span><span class="sxs-lookup"><span data-stu-id="7b02d-355">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="7b02d-356">在瀏覽器中提出要求給 `http://localhost:5000`，以收到 "Hello World!" 回應</span><span class="sxs-lookup"><span data-stu-id="7b02d-356">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="7b02d-357">`WaitForShutdown` 會封鎖，直到發出中斷 (Ctrl-C/SIGINT 或 SIGTERM)。</span><span class="sxs-lookup"><span data-stu-id="7b02d-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="7b02d-358">應用程式會顯示 `Console.WriteLine` 訊息並等候按鍵動作以便結束。</span><span class="sxs-lookup"><span data-stu-id="7b02d-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="7b02d-359">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="7b02d-359">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="7b02d-360">提供 URL 和委派以設定 `IApplicationBuilder`：</span><span class="sxs-lookup"><span data-stu-id="7b02d-360">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="7b02d-361">產生與 **StartWith(Action&lt;IApplicationBuilder&gt; app)** 相同的結果，除了應用程式會在 `http://localhost:8080` 回應。</span><span class="sxs-lookup"><span data-stu-id="7b02d-361">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="7b02d-362">IHostingEnvironment 介面</span><span class="sxs-lookup"><span data-stu-id="7b02d-362">IHostingEnvironment interface</span></span>

<span data-ttu-id="7b02d-363">[IHostingEnvironment 介面](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment)提供應用程式的 Web 裝載環境相關資訊。</span><span class="sxs-lookup"><span data-stu-id="7b02d-363">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="7b02d-364">使用[建構函式插入](xref:fundamentals/dependency-injection)以取得 `IHostingEnvironment`，才能使用其屬性和擴充方法：</span><span class="sxs-lookup"><span data-stu-id="7b02d-364">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

<span data-ttu-id="7b02d-365">[以慣例為基礎的方法](xref:fundamentals/environments#environment-based-startup-class-and-methods)可用來根據環境在啟動時設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="7b02d-365">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="7b02d-366">或者，將 `IHostingEnvironment` 插入至 `Startup` 建構函式，以便用於 `ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="7b02d-366">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> <span data-ttu-id="7b02d-367">除了 `IsDevelopment` 擴充方法，`IHostingEnvironment` 也提供 `IsStaging`、`IsProduction` 和 `IsEnvironment(string environmentName)` 方法。</span><span class="sxs-lookup"><span data-stu-id="7b02d-367">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="7b02d-368">如需詳細資訊，請參閱 <xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="7b02d-368">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="7b02d-369">`IHostingEnvironment` 服務也可直接插入至 `Configure` 方法，以便設定處理管線：</span><span class="sxs-lookup"><span data-stu-id="7b02d-369">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the Developer Exception Page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

<span data-ttu-id="7b02d-370">建立自訂[中介軟體](xref:fundamentals/middleware/write)時，`IHostingEnvironment` 可以插入至 `Invoke` 方法：</span><span class="sxs-lookup"><span data-stu-id="7b02d-370">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/write):</span></span>

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="7b02d-371">IApplicationLifetime 介面</span><span class="sxs-lookup"><span data-stu-id="7b02d-371">IApplicationLifetime interface</span></span>

<span data-ttu-id="7b02d-372">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) 允許啟動後和關機活動。</span><span class="sxs-lookup"><span data-stu-id="7b02d-372">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="7b02d-373">在介面上的三個屬性是用來註冊定義啟動和關閉事件之 `Action` 方法的取消語彙基元。</span><span class="sxs-lookup"><span data-stu-id="7b02d-373">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="7b02d-374">取消語彙基元</span><span class="sxs-lookup"><span data-stu-id="7b02d-374">Cancellation Token</span></span>    | <span data-ttu-id="7b02d-375">觸發時機&#8230;</span><span class="sxs-lookup"><span data-stu-id="7b02d-375">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="7b02d-376">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="7b02d-376">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="7b02d-377">已完全啟動主機。</span><span class="sxs-lookup"><span data-stu-id="7b02d-377">The host has fully started.</span></span> |
| [<span data-ttu-id="7b02d-378">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="7b02d-378">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="7b02d-379">主機正在完成正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="7b02d-379">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="7b02d-380">應該處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="7b02d-380">All requests should be processed.</span></span> <span data-ttu-id="7b02d-381">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="7b02d-381">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="7b02d-382">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="7b02d-382">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="7b02d-383">主機正在執行正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="7b02d-383">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="7b02d-384">可能仍在處理要求。</span><span class="sxs-lookup"><span data-stu-id="7b02d-384">Requests may still be processing.</span></span> <span data-ttu-id="7b02d-385">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="7b02d-385">Shutdown blocks until this event completes.</span></span> |

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime)
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

<span data-ttu-id="7b02d-386">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) 會要求應用程式終止。</span><span class="sxs-lookup"><span data-stu-id="7b02d-386">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="7b02d-387">當呼叫類別的 `Shutdown` 方法時，下列類別使用 `StopApplication` 來順利關閉應用程式：</span><span class="sxs-lookup"><span data-stu-id="7b02d-387">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="7b02d-388">範圍驗證</span><span class="sxs-lookup"><span data-stu-id="7b02d-388">Scope validation</span></span>

<span data-ttu-id="7b02d-389">如果應用程式的環境是「開發」，[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 會將 [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) 設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="7b02d-389">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="7b02d-390">當 `ValidateScopes` 設定為 `true` 時，預設服務提供者會執行檢查以確認：</span><span class="sxs-lookup"><span data-stu-id="7b02d-390">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="7b02d-391">範圍服務不是直接或間接由根服務提供者解析。</span><span class="sxs-lookup"><span data-stu-id="7b02d-391">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="7b02d-392">範圍服務不是直接或間接插入至單一服務。</span><span class="sxs-lookup"><span data-stu-id="7b02d-392">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="7b02d-393">根服務提供者會在呼叫 [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) 時建立。</span><span class="sxs-lookup"><span data-stu-id="7b02d-393">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="7b02d-394">當提供者啟動應用程式時，根服務提供者的存留期與應用程式/伺服器的存留期一致，並會在應用程式關閉時處置。</span><span class="sxs-lookup"><span data-stu-id="7b02d-394">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="7b02d-395">範圍服務會由建立這些服務的容器處置。</span><span class="sxs-lookup"><span data-stu-id="7b02d-395">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="7b02d-396">若是在根容器中建立範圍服務，因為當應用程式/伺服器關機時，服務只會由根容器處理，所以服務的存留期會提升為單一服務等級。</span><span class="sxs-lookup"><span data-stu-id="7b02d-396">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="7b02d-397">當呼叫 `BuildServiceProvider` 時，驗證服務範圍會攔截到這些情況。</span><span class="sxs-lookup"><span data-stu-id="7b02d-397">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="7b02d-398">若要一律驗證範圍，包括在實際執行環境中，請使用主機產生器上的 [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) 來設定 [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions)：</span><span class="sxs-lookup"><span data-stu-id="7b02d-398">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="additional-resources"></a><span data-ttu-id="7b02d-399">其他資源</span><span class="sxs-lookup"><span data-stu-id="7b02d-399">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
