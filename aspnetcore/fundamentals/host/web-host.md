---
title: ASP.NET Core Web 主機
author: rick-anderson
description: 了解 ASP.NET Core 中的 Web 主機，其負責啟動應用程式及管理存留期。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/host/web-host
ms.openlocfilehash: 71bca4c0987059efa0e4ff35f25fe7cdb75641d5
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82773987"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="d7bf8-103">ASP.NET Core Web 主機</span><span class="sxs-lookup"><span data-stu-id="d7bf8-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="d7bf8-104">ASP.NET Core 應用程式會設定並啟動*主機*。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-104">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="d7bf8-105">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="d7bf8-106">至少，主機會設定伺服器和要求處理管線。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-106">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="d7bf8-107">主機也可以設定記錄、相依性插入和設定。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-107">The host can also set up logging, dependency injection, and configuration.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d7bf8-108">此文章涵蓋 Web 主機，而且我們僅因為回溯相容性而繼續提供它。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-108">This article covers the Web Host, which remains available only for backward compatibility.</span></span> <span data-ttu-id="d7bf8-109">建議為所有應用程式類型使用[一般主機](xref:fundamentals/host/generic-host)。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-109">The [Generic Host](xref:fundamentals/host/generic-host) is recommended for all app types.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d7bf8-110">此文章涵蓋 Web 主機，它適用於裝載 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-110">This article covers the Web Host, which is for hosting web apps.</span></span> <span data-ttu-id="d7bf8-111">針對其他類型的應用程式，請使用[一般主機](xref:fundamentals/host/generic-host)。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-111">For other kinds of apps, use the [Generic Host](xref:fundamentals/host/generic-host).</span></span>

::: moniker-end

## <a name="set-up-a-host"></a><span data-ttu-id="d7bf8-112">設定主機</span><span class="sxs-lookup"><span data-stu-id="d7bf8-112">Set up a host</span></span>

<span data-ttu-id="d7bf8-113">使用 ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)) 的執行個體建立主機。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-113">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="d7bf8-114">這通常在應用程式的進入點執行，也就是 `Main` 方法。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-114">This is typically performed in the app's entry point, the `Main` method.</span></span>

<span data-ttu-id="d7bf8-115">在專案範本中， `Main`位於*Program.cs*。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-115">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="d7bf8-116">一般的應用程式會呼叫 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 來開始設定主機：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-116">A typical app calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="d7bf8-117">呼叫 `CreateDefaultBuilder` 的程式碼位於名為 `CreateWebHostBuilder` 的方法中，這將它與 `Main` 中呼叫產生器物件上之 `Run` 的方法分開。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-117">The code that calls `CreateDefaultBuilder` is in a method named `CreateWebHostBuilder`, which separates it from the code in `Main` that calls `Run` on the builder object.</span></span> <span data-ttu-id="d7bf8-118">若您使用e [Entity Framework Core 工具](/ef/core/miscellaneous/cli/)，則此分開是必要的。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-118">This separation is required if you use [Entity Framework Core tools](/ef/core/miscellaneous/cli/).</span></span> <span data-ttu-id="d7bf8-119">工具預期找到它們可以在設計階段呼叫的 `CreateWebHostBuilder` 方法，以在不執行應用程式的情況下設定主機。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-119">The tools expect to find a `CreateWebHostBuilder` method that they can call at design time to configure the host without running the app.</span></span> <span data-ttu-id="d7bf8-120">替代方式是實作 `IDesignTimeDbContextFactory`。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-120">An alternative is to implement `IDesignTimeDbContextFactory`.</span></span> <span data-ttu-id="d7bf8-121">如需詳細資訊，請參閱[設計階段 DbContext 建立](/ef/core/miscellaneous/cli/dbcontext-creation)。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-121">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

<span data-ttu-id="d7bf8-122">`CreateDefaultBuilder` 會執行下列工作：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-122">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="d7bf8-123">使用應用程式的主機組態提供者，將 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器設定為網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-123">Configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server using the app's hosting configuration providers.</span></span> <span data-ttu-id="d7bf8-124">如需 Kestrel 伺服器的預設選項，請參閱 <xref:fundamentals/servers/kestrel#kestrel-options>。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-124">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="d7bf8-125">將[內容根目錄](xref:fundamentals/index#content-root)設定為[GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory)所傳回的路徑。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-125">Sets the [content root](xref:fundamentals/index#content-root) to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="d7bf8-126">從下列項目載入[主機組態](#host-configuration-values)：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-126">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="d7bf8-127">前面加上 `ASPNETCORE_` 的環境變數 (例如，`ASPNETCORE_ENVIRONMENT`)。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-127">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="d7bf8-128">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-128">Command-line arguments.</span></span>
* <span data-ttu-id="d7bf8-129">以下列順序載入應用程式組態：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-129">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="d7bf8-130">*appsettings. json*。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-130">*appsettings.json*.</span></span>
  * <span data-ttu-id="d7bf8-131">*appsettings.{Environment}.json*</span><span class="sxs-lookup"><span data-stu-id="d7bf8-131">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="d7bf8-132">應用程式在使用項目組件之 `Development` 環境中執行時的[秘密管理員](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-132">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="d7bf8-133">環境變數。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-133">Environment variables.</span></span>
  * <span data-ttu-id="d7bf8-134">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-134">Command-line arguments.</span></span>
* <span data-ttu-id="d7bf8-135">設定主控台和偵錯輸出的[記錄](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-135">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="d7bf8-136">記錄包含 *appsettings.json* 或 *appsettings.{Environment}.json* 檔案的記錄組態區段中指定的[記錄檔篩選](xref:fundamentals/logging/index#log-filtering)規則。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-136">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="d7bf8-137">使用[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)在 iis 背後執行時， `CreateDefaultBuilder`會啟用[iis 整合](xref:host-and-deploy/iis/index)，這會設定應用程式的基底位址和埠。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-137">When running behind IIS with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` enables [IIS Integration](xref:host-and-deploy/iis/index), which configures the app's base address and port.</span></span> <span data-ttu-id="d7bf8-138">IIS 整合也會設定應用程式以[擷取啟動錯誤](#capture-startup-errors)。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-138">IIS Integration also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="d7bf8-139">如需 IIS 預設選項，請參閱 <xref:host-and-deploy/iis/index#iis-options>。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-139">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="d7bf8-140">如果應用程式的環境是「開發」，請將 [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) 設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-140">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="d7bf8-141">如需詳細資訊，請參閱[範圍驗證](#scope-validation)。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-141">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="d7bf8-142">`CreateDefaultBuilder` 所定義的組態可以透過 [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration)、[ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging)，以及 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) 的其他方法和擴充方法加以覆寫及擴增。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-142">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="d7bf8-143">數個範例如下：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-143">A few examples follow:</span></span>

* <span data-ttu-id="d7bf8-144">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) 用來指定應用程式的其他 `IConfiguration`。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-144">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="d7bf8-145">下列 `ConfigureAppConfiguration` 呼叫會新增委派，以在 *appsettings.xml* 檔案中包含應用程式組態。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-145">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="d7bf8-146">可能會多次呼叫 `ConfigureAppConfiguration`。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-146">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="d7bf8-147">請注意，此組態不適用於主機 (例如，伺服器 URL 或環境)。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-147">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="d7bf8-148">請參閱[主機組態值](#host-configuration-values)一節。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-148">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="d7bf8-149">下列 `ConfigureLogging` 呼叫會新增委派，將最低的記錄層級 ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) 設定為 [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel)。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-149">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="d7bf8-150">此設定會覆寫 appsettings 中的設定 *。開發. json* （`LogLevel.Debug`）和*appsettings。* 由設定的實際`LogLevel.Error`執行的`CreateDefaultBuilder`json （）。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-150">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="d7bf8-151">可能會多次呼叫 `ConfigureLogging`。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-151">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d7bf8-152">下列 `ConfigureKestrel` 呼叫會覆寫當 `CreateDefaultBuilder` 設定 Kestrel 時建立的預設 [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) 30,000,000 位元組：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-152">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="d7bf8-153">下列 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) 呼叫會覆寫當 `CreateDefaultBuilder` 設定 Kestrel 時建立的預設 [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) 30,000,000 位元組：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-153">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

<span data-ttu-id="d7bf8-154">「內容根目錄」[](xref:fundamentals/index#content-root)會決定主機搜尋內容檔案 (例如 MVC 檢視檔案) 的位置。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-154">The [content root](xref:fundamentals/index#content-root) determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="d7bf8-155">從專案的根資料夾啟動應用程式時，會使用專案的根資料夾作為內容根目錄。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-155">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="d7bf8-156">這是 [Visual Studio](https://visualstudio.microsoft.com) 和 [dotnet 新範本](/dotnet/core/tools/dotnet-new)中使用的預設值。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-156">This is the default used in [Visual Studio](https://visualstudio.microsoft.com) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="d7bf8-157">如需應用程式組態的詳細資訊，請參閱 <xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-157">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="d7bf8-158">作為使用靜態 `CreateDefaultBuilder` 方法的替代做法，ASP.NET Core 2.x 支援從 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 建立主機的方法。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-158">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span>

<span data-ttu-id="d7bf8-159">設定主機時，可以提供 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) 和 [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices) 方法。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-159">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices) methods can be provided.</span></span> <span data-ttu-id="d7bf8-160">如果指定 `Startup` 類別，它必須定義 `Configure` 方法。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-160">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="d7bf8-161">如需詳細資訊，請參閱<xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-161">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="d7bf8-162">多次呼叫 `ConfigureServices` 會彼此附加。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-162">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="d7bf8-163">對 `WebHostBuilder` 多次呼叫 `Configure` 或 `UseStartup` 則會取代先前的設定。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-163">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="d7bf8-164">主機組態值</span><span class="sxs-lookup"><span data-stu-id="d7bf8-164">Host configuration values</span></span>

<span data-ttu-id="d7bf8-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 依賴下列方法來設定主機組態值：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="d7bf8-166">主機建立器組態，其中包含 `ASPNETCORE_{configurationKey}` 格式的環境變數。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-166">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="d7bf8-167">例如： `ASPNETCORE_ENVIRONMENT` 。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-167">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="d7bf8-168">[UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) 和 [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 之類的延伸模組 (請參閱[覆寫組態](#override-configuration)一節)。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-168">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="d7bf8-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) 和相關聯的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="d7bf8-170">使用 `UseSetting` 設定值時，此值會設定為字串，而不論其類型為何。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-170">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="d7bf8-171">主機使用設定最後一個值的任何選項。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-171">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="d7bf8-172">如需詳細資訊，請參閱下一節的[覆寫組態](#override-configuration)。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-172">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="d7bf8-173">應用程式索引鍵 (名稱)</span><span class="sxs-lookup"><span data-stu-id="d7bf8-173">Application Key (Name)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d7bf8-174">在`IWebHostEnvironment.ApplicationName`主機結構期間呼叫[UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup)或[設定](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure)時，會自動設定屬性。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-174">The `IWebHostEnvironment.ApplicationName` property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="d7bf8-175">該值會設定為包含應用程式進入點的組件名稱。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-175">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="d7bf8-176">若要明確設定該值，請使用 [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey)：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-176">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d7bf8-177">在主機建構期間呼叫 [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) 或 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) 時，會自動設定 [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) 屬性。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-177">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="d7bf8-178">該值會設定為包含應用程式進入點的組件名稱。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-178">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="d7bf8-179">若要明確設定該值，請使用 [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey)：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-179">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

::: moniker-end

<span data-ttu-id="d7bf8-180">**索引鍵**：applicationName</span><span class="sxs-lookup"><span data-stu-id="d7bf8-180">**Key**: applicationName</span></span>  
<span data-ttu-id="d7bf8-181">**類型**：*字串*</span><span class="sxs-lookup"><span data-stu-id="d7bf8-181">**Type**: *string*</span></span>  
<span data-ttu-id="d7bf8-182">**預設**：包含應用程式進入點的組件名稱。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-182">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="d7bf8-183">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-183">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d7bf8-184">**環境變數**：`ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-184">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a><span data-ttu-id="d7bf8-185">擷取啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="d7bf8-185">Capture Startup Errors</span></span>

<span data-ttu-id="d7bf8-186">此設定會控制啟動錯誤的擷取。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-186">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="d7bf8-187">**索引鍵**：captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="d7bf8-187">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="d7bf8-188">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="d7bf8-188">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d7bf8-189">**預設值**：預設為 `false`，除非應用程式執行時在 IIS 背後有 Kestrel，此時預設值即為 `true`。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-189">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="d7bf8-190">**設定使用**：`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-190">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="d7bf8-191">**環境變數**：`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-191">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="d7bf8-192">當它為 `false` 時，啟動期間發生的錯誤會導致主機結束。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-192">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="d7bf8-193">當它為 `true` 時，主機會擷取啟動期間的例外狀況，並嘗試啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-193">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a><span data-ttu-id="d7bf8-194">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="d7bf8-194">Content root</span></span>

<span data-ttu-id="d7bf8-195">此設定會決定 ASP.NET Core 開始搜尋內容檔案的位置。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-195">This setting determines where ASP.NET Core begins searching for content files.</span></span>

<span data-ttu-id="d7bf8-196">**索引鍵**：contentRoot</span><span class="sxs-lookup"><span data-stu-id="d7bf8-196">**Key**: contentRoot</span></span>  
<span data-ttu-id="d7bf8-197">**類型**：*字串*</span><span class="sxs-lookup"><span data-stu-id="d7bf8-197">**Type**: *string*</span></span>  
<span data-ttu-id="d7bf8-198">**預設值**：預設為應用程式組件所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-198">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="d7bf8-199">**設定使用**：`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-199">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="d7bf8-200">**環境變數**：`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-200">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="d7bf8-201">內容根目錄也會用來做為[web 根目錄](xref:fundamentals/index#web-root)的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-201">The content root is also used as the base path for the [web root](xref:fundamentals/index#web-root).</span></span> <span data-ttu-id="d7bf8-202">如果內容根路徑不存在，則無法啟動主機。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-202">If the content root path doesn't exist, the host fails to start.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

<span data-ttu-id="d7bf8-203">如需詳細資訊，請參閱</span><span class="sxs-lookup"><span data-stu-id="d7bf8-203">For more information, see:</span></span>

* [<span data-ttu-id="d7bf8-204">基本概念：內容根目錄</span><span class="sxs-lookup"><span data-stu-id="d7bf8-204">Fundamentals: Content root</span></span>](xref:fundamentals/index#content-root)
* [<span data-ttu-id="d7bf8-205">Web 根目錄</span><span class="sxs-lookup"><span data-stu-id="d7bf8-205">Web root</span></span>](#web-root)

### <a name="detailed-errors"></a><span data-ttu-id="d7bf8-206">詳細錯誤</span><span class="sxs-lookup"><span data-stu-id="d7bf8-206">Detailed Errors</span></span>

<span data-ttu-id="d7bf8-207">決定是否應該擷取詳細的錯誤。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-207">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="d7bf8-208">**索引鍵**：detailedErrors</span><span class="sxs-lookup"><span data-stu-id="d7bf8-208">**Key**: detailedErrors</span></span>  
<span data-ttu-id="d7bf8-209">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="d7bf8-209">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d7bf8-210">**預設值**： false</span><span class="sxs-lookup"><span data-stu-id="d7bf8-210">**Default**: false</span></span>  
<span data-ttu-id="d7bf8-211">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-211">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d7bf8-212">**環境變數**：`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-212">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="d7bf8-213">啟用時 (或當 <a href="#environment">Environment</a> 設定為 `Development` 時)，應用程式會擷取詳細例外狀況。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-213">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a><span data-ttu-id="d7bf8-214">環境</span><span class="sxs-lookup"><span data-stu-id="d7bf8-214">Environment</span></span>

<span data-ttu-id="d7bf8-215">設定應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-215">Sets the app's environment.</span></span>

<span data-ttu-id="d7bf8-216">**索引鍵**：environment</span><span class="sxs-lookup"><span data-stu-id="d7bf8-216">**Key**: environment</span></span>  
<span data-ttu-id="d7bf8-217">**類型**：*字串*</span><span class="sxs-lookup"><span data-stu-id="d7bf8-217">**Type**: *string*</span></span>  
<span data-ttu-id="d7bf8-218">**預設值**：Production</span><span class="sxs-lookup"><span data-stu-id="d7bf8-218">**Default**: Production</span></span>  
<span data-ttu-id="d7bf8-219">**設定使用**：`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-219">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="d7bf8-220">**環境變數**：`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-220">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="d7bf8-221">環境可以設定為任何值。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-221">The environment can be set to any value.</span></span> <span data-ttu-id="d7bf8-222">架構定義的值包括 `Development`、`Staging` 和 `Production`。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-222">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="d7bf8-223">值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-223">Values aren't case sensitive.</span></span> <span data-ttu-id="d7bf8-224">根據預設，*Environment* 是從 `ASPNETCORE_ENVIRONMENT` 環境變數讀取。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-224">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="d7bf8-225">使用 [Visual Studio](https://visualstudio.microsoft.com) 時，可能會在 *launchSettings.json* 檔案設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-225">When using [Visual Studio](https://visualstudio.microsoft.com), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="d7bf8-226">如需詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-226">For more information, see <xref:fundamentals/environments>.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="d7bf8-227">裝載啟動組件</span><span class="sxs-lookup"><span data-stu-id="d7bf8-227">Hosting Startup Assemblies</span></span>

<span data-ttu-id="d7bf8-228">設定應用程式的裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-228">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="d7bf8-229">**索引鍵**：hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="d7bf8-229">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="d7bf8-230">**類型**：*字串*</span><span class="sxs-lookup"><span data-stu-id="d7bf8-230">**Type**: *string*</span></span>  
<span data-ttu-id="d7bf8-231">**預設值**：空字串</span><span class="sxs-lookup"><span data-stu-id="d7bf8-231">**Default**: Empty string</span></span>  
<span data-ttu-id="d7bf8-232">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-232">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d7bf8-233">**環境變數**：`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-233">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="d7bf8-234">在啟動時載入的裝載啟動組件字串，以分號分隔。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-234">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="d7bf8-235">雖然設定值會預設為空字串，但裝載啟動組件一律會包含應用程式的組件。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-235">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="d7bf8-236">提供裝載啟動組件時，它們會新增至應用程式的組件，以便在應用程式在啟動時建置其通用服務時載入。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-236">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a><span data-ttu-id="d7bf8-237">HTTPS 連接埠</span><span class="sxs-lookup"><span data-stu-id="d7bf8-237">HTTPS Port</span></span>

<span data-ttu-id="d7bf8-238">設定 HTTPS 重新導向連接埠。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-238">Set the HTTPS redirect port.</span></span> <span data-ttu-id="d7bf8-239">用於[強制 HTTPS](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-239">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="d7bf8-240">**機碼**：https_port **類型**：*字串*
**預設值**：未設定預設值。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-240">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="d7bf8-241">**設定使用**： `UseSetting` 
**環境變數**：`ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-241">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="d7bf8-242">裝載啟動排除組件</span><span class="sxs-lookup"><span data-stu-id="d7bf8-242">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="d7bf8-243">在啟動時排除以分號分隔的裝載啟動組件字串。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-243">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="d7bf8-244">**索引鍵**：hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="d7bf8-244">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="d7bf8-245">**類型**：*字串*</span><span class="sxs-lookup"><span data-stu-id="d7bf8-245">**Type**: *string*</span></span>  
<span data-ttu-id="d7bf8-246">**預設值**：空字串</span><span class="sxs-lookup"><span data-stu-id="d7bf8-246">**Default**: Empty string</span></span>  
<span data-ttu-id="d7bf8-247">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-247">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d7bf8-248">**環境變數**：`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-248">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a><span data-ttu-id="d7bf8-249">偏好裝載 URL</span><span class="sxs-lookup"><span data-stu-id="d7bf8-249">Prefer Hosting URLs</span></span>

<span data-ttu-id="d7bf8-250">表示主機是否應接聽使用 `WebHostBuilder` 設定的 URL，而不是 `IServer` 實作所設定的 URL。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-250">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="d7bf8-251">**索引鍵**：preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="d7bf8-251">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="d7bf8-252">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="d7bf8-252">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d7bf8-253">**預設值**： true</span><span class="sxs-lookup"><span data-stu-id="d7bf8-253">**Default**: true</span></span>  
<span data-ttu-id="d7bf8-254">**設定使用**：`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-254">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="d7bf8-255">**環境變數**：`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-255">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a><span data-ttu-id="d7bf8-256">防止裝載啟動</span><span class="sxs-lookup"><span data-stu-id="d7bf8-256">Prevent Hosting Startup</span></span>

<span data-ttu-id="d7bf8-257">可防止自動載入裝載啟動組件，包括應用程式組件所設定的裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-257">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="d7bf8-258">如需詳細資訊，請參閱<xref:fundamentals/configuration/platform-specific-configuration>。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-258">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="d7bf8-259">**索引鍵**preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="d7bf8-259">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="d7bf8-260">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="d7bf8-260">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d7bf8-261">**預設值**： false</span><span class="sxs-lookup"><span data-stu-id="d7bf8-261">**Default**: false</span></span>  
<span data-ttu-id="d7bf8-262">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-262">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d7bf8-263">**環境變數**：`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-263">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a><span data-ttu-id="d7bf8-264">伺服器 URL</span><span class="sxs-lookup"><span data-stu-id="d7bf8-264">Server URLs</span></span>

<span data-ttu-id="d7bf8-265">表示伺服器應該接聽要求的 IP 位址或主機位址，以及連接埠和通訊協定。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-265">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="d7bf8-266">**索引鍵**：urls</span><span class="sxs-lookup"><span data-stu-id="d7bf8-266">**Key**: urls</span></span>  
<span data-ttu-id="d7bf8-267">**類型**：*字串*</span><span class="sxs-lookup"><span data-stu-id="d7bf8-267">**Type**: *string*</span></span>  
<span data-ttu-id="d7bf8-268">**預設**：http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="d7bf8-268">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="d7bf8-269">**設定使用**：`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-269">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="d7bf8-270">**環境變數**：`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-270">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="d7bf8-271">設定為伺服器應該回應的 URL 前置清單，並以分號 (;) 分隔。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-271">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="d7bf8-272">例如： `http://localhost:123` 。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-272">For example, `http://localhost:123`.</span></span> <span data-ttu-id="d7bf8-273">使用 "\*"，表示伺服器應接聽任何 IP 位址或主機名稱上的要求，並使用指定的連接埠和通訊協定 (例如，`http://*:5000`)。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-273">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="d7bf8-274">通訊協定 (`http://` 或 `https://`) 必須包含在每個 URL 中。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-274">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="d7bf8-275">支援的格式會依伺服器而有所不同。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-275">Supported formats vary among servers.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="d7bf8-276">Kestrel 有它自己的端點設定 API。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-276">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="d7bf8-277">如需詳細資訊，請參閱<xref:fundamentals/servers/kestrel#endpoint-configuration>。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-277">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="d7bf8-278">關機逾時</span><span class="sxs-lookup"><span data-stu-id="d7bf8-278">Shutdown Timeout</span></span>

<span data-ttu-id="d7bf8-279">指定等待 Web 主機關機的時間長度。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-279">Specifies the amount of time to wait for Web Host to shut down.</span></span>

<span data-ttu-id="d7bf8-280">**索引鍵**：shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="d7bf8-280">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="d7bf8-281">**類型**： *int*</span><span class="sxs-lookup"><span data-stu-id="d7bf8-281">**Type**: *int*</span></span>  
<span data-ttu-id="d7bf8-282">**預設值**：5</span><span class="sxs-lookup"><span data-stu-id="d7bf8-282">**Default**: 5</span></span>  
<span data-ttu-id="d7bf8-283">**設定使用**：`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-283">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="d7bf8-284">**環境變數**：`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-284">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="d7bf8-285">雖然索引鍵接受 *int* 與 `UseSetting` (例如 `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`)，[UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) 擴充方法會採用 [TimeSpan](/dotnet/api/system.timespan)。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-285">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="d7bf8-286">在逾時期間，裝載會：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-286">During the timeout period, hosting:</span></span>

* <span data-ttu-id="d7bf8-287">觸發 [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping)。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-287">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="d7bf8-288">嘗試停止託管的服務，並記錄無法停止之服務的任何錯誤。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-288">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="d7bf8-289">如果在所有的託管服務停止之前逾時期限已到期，則應用程式關閉時，會停止任何剩餘的作用中服務。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-289">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="d7bf8-290">即使服務尚未完成處理也會停止。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-290">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="d7bf8-291">如果服務需要更多時間才能停止，請增加逾時。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-291">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a><span data-ttu-id="d7bf8-292">啟動組件</span><span class="sxs-lookup"><span data-stu-id="d7bf8-292">Startup Assembly</span></span>

<span data-ttu-id="d7bf8-293">決定要搜尋 `Startup` 類別的組件。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-293">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="d7bf8-294">**索引鍵**：startupAssembly</span><span class="sxs-lookup"><span data-stu-id="d7bf8-294">**Key**: startupAssembly</span></span>  
<span data-ttu-id="d7bf8-295">**類型**：*字串*</span><span class="sxs-lookup"><span data-stu-id="d7bf8-295">**Type**: *string*</span></span>  
<span data-ttu-id="d7bf8-296">**預設值**：應用程式的組件</span><span class="sxs-lookup"><span data-stu-id="d7bf8-296">**Default**: The app's assembly</span></span>  
<span data-ttu-id="d7bf8-297">**設定使用**：`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-297">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="d7bf8-298">**環境變數**：`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-298">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="d7bf8-299">可以參考依名稱 (`string`) 或類型 (`TStartup`) 的組件。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-299">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="d7bf8-300">如果呼叫多個 `UseStartup` 方法，最後一個將會優先。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-300">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a><span data-ttu-id="d7bf8-301">Web 根目錄</span><span class="sxs-lookup"><span data-stu-id="d7bf8-301">Web root</span></span>

<span data-ttu-id="d7bf8-302">設定應用程式靜態資產的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-302">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="d7bf8-303">**索引鍵**：webroot</span><span class="sxs-lookup"><span data-stu-id="d7bf8-303">**Key**: webroot</span></span>  
<span data-ttu-id="d7bf8-304">**類型**：*字串*</span><span class="sxs-lookup"><span data-stu-id="d7bf8-304">**Type**: *string*</span></span>  
<span data-ttu-id="d7bf8-305">**預設**值：預設值`wwwroot`為。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-305">**Default**: The default is `wwwroot`.</span></span> <span data-ttu-id="d7bf8-306">*{Content root}/wwwroot*的路徑必須存在。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-306">The path to *{content root}/wwwroot* must exist.</span></span> <span data-ttu-id="d7bf8-307">如果路徑不存在，則會使用無作業檔案提供者。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-307">If the path doesn't exist, a no-op file provider is used.</span></span>  
<span data-ttu-id="d7bf8-308">**設定使用**：`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-308">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="d7bf8-309">**環境變數**：`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="d7bf8-309">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

<span data-ttu-id="d7bf8-310">如需詳細資訊，請參閱</span><span class="sxs-lookup"><span data-stu-id="d7bf8-310">For more information, see:</span></span>

* [<span data-ttu-id="d7bf8-311">基本概念： Web 根目錄</span><span class="sxs-lookup"><span data-stu-id="d7bf8-311">Fundamentals: Web root</span></span>](xref:fundamentals/index#web-root)
* [<span data-ttu-id="d7bf8-312">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="d7bf8-312">Content root</span></span>](#content-root)

## <a name="override-configuration"></a><span data-ttu-id="d7bf8-313">覆寫組態</span><span class="sxs-lookup"><span data-stu-id="d7bf8-313">Override configuration</span></span>

<span data-ttu-id="d7bf8-314">使用 [Configuration](xref:fundamentals/configuration/index) 來設定 Web 主機。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-314">Use [Configuration](xref:fundamentals/configuration/index) to configure Web Host.</span></span> <span data-ttu-id="d7bf8-315">在下列範例中，主機組態選擇性指定於 *hostsettings.json* 檔案中。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-315">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="d7bf8-316">從 *hostsettings.json* 檔案載入的任何組態，可以用命令列引數加以覆寫。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-316">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="d7bf8-317">建置的組態 (在 `config` 中) 用以使用 [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 來設定主機。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-317">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="d7bf8-318">`IWebHostBuilder` 設定會新增到應用程式的組態，但反之則不然 &mdash;`ConfigureAppConfiguration` 並不會影響 `IWebHostBuilder` 組態。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-318">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

<span data-ttu-id="d7bf8-319">首先使用 *hostsettings.json* 組態來覆寫 `UseUrls` 所提供的組態，其次是命令列引數組態：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-319">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="d7bf8-320">*hostsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-320">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> <span data-ttu-id="d7bf8-321">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration)只會將金鑰從提供`IConfiguration`的複製到主機 builder 設定。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-321">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="d7bf8-322">因此，為 JSON、INI 和 XML 設定檔設定 `reloadOnChange: true` 沒有任何作用。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-322">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="d7bf8-323">若要指定主機在特定 URL 上執行，所要的值可以在執行 [dotnet run](/dotnet/core/tools/dotnet-run) 時從命令提示字元傳入。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-323">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="d7bf8-324">命令列引數會覆寫 *hostsettings.json* 檔案的 `urls` 值，且伺服器會接聽連接埠 8080：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-324">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```dotnetcli
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="d7bf8-325">管理主機</span><span class="sxs-lookup"><span data-stu-id="d7bf8-325">Manage the host</span></span>

<span data-ttu-id="d7bf8-326">**進行**</span><span class="sxs-lookup"><span data-stu-id="d7bf8-326">**Run**</span></span>

<span data-ttu-id="d7bf8-327">`Run` 方法會啟動 Web 應用程式，並且封鎖呼叫執行緒，直到主機關閉為止：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-327">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="d7bf8-328">**啟動**</span><span class="sxs-lookup"><span data-stu-id="d7bf8-328">**Start**</span></span>

<span data-ttu-id="d7bf8-329">藉由呼叫 `Start` 方法，以非封鎖方式執行主機：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-329">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="d7bf8-330">如果 URL 的清單傳遞至 `Start` 方法，它會接聽指定的 URL：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-330">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="d7bf8-331">應用程式可以使用預先設定的 `CreateDefaultBuilder` 預設值，使用便利的靜態方法初始化並啟動新的主機。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-331">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="d7bf8-332">這些方法會啟動伺服器而無主控台輸出，且在使用 [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) 時等候中斷 (Ctrl-C/SIGINT 或 SIGTERM)：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-332">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="d7bf8-333">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="d7bf8-333">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="d7bf8-334">使用 `RequestDelegate` 啟動：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-334">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d7bf8-335">在瀏覽器中提出要求給 `http://localhost:5000`，以收到 "Hello World!" 回應</span><span class="sxs-lookup"><span data-stu-id="d7bf8-335">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="d7bf8-336">`WaitForShutdown` 會封鎖，直到發出中斷 (Ctrl-C/SIGINT 或 SIGTERM)。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-336">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d7bf8-337">應用程式會顯示 `Console.WriteLine` 訊息並等候按鍵動作以便結束。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-337">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d7bf8-338">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="d7bf8-338">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="d7bf8-339">使用 URL 和 `RequestDelegate` 來啟動：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-339">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d7bf8-340">產生與 **Start(RequestDelegate app)** 相同的結果，除了應用程式會在 `http://localhost:8080` 回應。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-340">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="d7bf8-341">**啟動（Action\<IRouteBuilder> routeBuilder）**</span><span class="sxs-lookup"><span data-stu-id="d7bf8-341">**Start(Action\<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="d7bf8-342">使用 `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) 的執行個體來使用路由中介軟體：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-342">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="d7bf8-343">使用下列瀏覽器要求與範例：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-343">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="d7bf8-344">要求</span><span class="sxs-lookup"><span data-stu-id="d7bf8-344">Request</span></span>                                    | <span data-ttu-id="d7bf8-345">回應</span><span class="sxs-lookup"><span data-stu-id="d7bf8-345">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="d7bf8-346">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="d7bf8-346">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="d7bf8-347">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="d7bf8-347">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="d7bf8-348">擲回例外狀況，字串為 "ooops!"</span><span class="sxs-lookup"><span data-stu-id="d7bf8-348">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="d7bf8-349">擲回例外狀況，字串為 "Un oh!"</span><span class="sxs-lookup"><span data-stu-id="d7bf8-349">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="d7bf8-350">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="d7bf8-350">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="d7bf8-351">Hello World!</span><span class="sxs-lookup"><span data-stu-id="d7bf8-351">Hello World!</span></span>                             |

<span data-ttu-id="d7bf8-352">`WaitForShutdown` 會封鎖，直到發出中斷 (Ctrl-C/SIGINT 或 SIGTERM)。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-352">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d7bf8-353">應用程式會顯示 `Console.WriteLine` 訊息並等候按鍵動作以便結束。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-353">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d7bf8-354">**Start （字串 url，Action\<IRouteBuilder> routeBuilder）**</span><span class="sxs-lookup"><span data-stu-id="d7bf8-354">**Start(string url, Action\<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="d7bf8-355">使用 URL 和 `IRouteBuilder` 的執行個體：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-355">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="d7bf8-356">會產生與**Start （Action\<IRouteBuilder> routeBuilder）** 相同的結果，但應用程式會`http://localhost:8080`在回應。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-356">Produces the same result as **Start(Action\<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="d7bf8-357">**與 startwith （動作\<IApplicationBuilder> 應用程式）**</span><span class="sxs-lookup"><span data-stu-id="d7bf8-357">**StartWith(Action\<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="d7bf8-358">提供委派以設定 `IApplicationBuilder`：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-358">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="d7bf8-359">在瀏覽器中提出要求給 `http://localhost:5000`，以收到 "Hello World!" 回應</span><span class="sxs-lookup"><span data-stu-id="d7bf8-359">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="d7bf8-360">`WaitForShutdown` 會封鎖，直到發出中斷 (Ctrl-C/SIGINT 或 SIGTERM)。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-360">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d7bf8-361">應用程式會顯示 `Console.WriteLine` 訊息並等候按鍵動作以便結束。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-361">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d7bf8-362">**與 startwith （字串 url，動作\<IApplicationBuilder> 應用程式）**</span><span class="sxs-lookup"><span data-stu-id="d7bf8-362">**StartWith(string url, Action\<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="d7bf8-363">提供 URL 和委派以設定 `IApplicationBuilder`：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-363">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="d7bf8-364">會產生與**與 startwith （Action\<IApplicationBuilder> 應用程式）** 相同的結果，但應用程式`http://localhost:8080`會在上回應。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-364">Produces the same result as **StartWith(Action\<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="iwebhostenvironment-interface"></a><span data-ttu-id="d7bf8-365">IWebHostEnvironment 介面</span><span class="sxs-lookup"><span data-stu-id="d7bf8-365">IWebHostEnvironment interface</span></span>

<span data-ttu-id="d7bf8-366">`IWebHostEnvironment`介面會提供應用程式的 web 裝載環境相關資訊。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-366">The `IWebHostEnvironment` interface provides information about the app's web hosting environment.</span></span> <span data-ttu-id="d7bf8-367">使用[建構函式插入](xref:fundamentals/dependency-injection)以取得 `IWebHostEnvironment`，才能使用其屬性和擴充方法：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-367">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IWebHostEnvironment` in order to use its properties and extension methods:</span></span>

```csharp
public class CustomFileReader
{
    private readonly IWebHostEnvironment _env;

    public CustomFileReader(IWebHostEnvironment env)
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

<span data-ttu-id="d7bf8-368">[以慣例為基礎的方法](xref:fundamentals/environments#environment-based-startup-class-and-methods)可用來根據環境在啟動時設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-368">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="d7bf8-369">或者，將 `IWebHostEnvironment` 插入至 `Startup` 建構函式，以便用於 `ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-369">Alternatively, inject the `IWebHostEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

```csharp
public class Startup
{
    public Startup(IWebHostEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IWebHostEnvironment HostingEnvironment { get; }

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
> <span data-ttu-id="d7bf8-370">除了 `IsDevelopment` 擴充方法，`IWebHostEnvironment` 也提供 `IsStaging`、`IsProduction` 和 `IsEnvironment(string environmentName)` 方法。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-370">In addition to the `IsDevelopment` extension method, `IWebHostEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="d7bf8-371">如需詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-371">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="d7bf8-372">`IWebHostEnvironment` 服務也可直接插入至 `Configure` 方法，以便設定處理管線：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-372">The `IWebHostEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
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

<span data-ttu-id="d7bf8-373">建立自訂[中介軟體](xref:fundamentals/middleware/write)時，`IWebHostEnvironment` 可以插入至 `Invoke` 方法：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-373">`IWebHostEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/write):</span></span>

```csharp
public async Task Invoke(HttpContext context, IWebHostEnvironment env)
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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="d7bf8-374">IHostingEnvironment 介面</span><span class="sxs-lookup"><span data-stu-id="d7bf8-374">IHostingEnvironment interface</span></span>

<span data-ttu-id="d7bf8-375">[IHostingEnvironment 介面](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment)提供應用程式的 Web 裝載環境相關資訊。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-375">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="d7bf8-376">使用[建構函式插入](xref:fundamentals/dependency-injection)以取得 `IHostingEnvironment`，才能使用其屬性和擴充方法：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-376">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="d7bf8-377">[以慣例為基礎的方法](xref:fundamentals/environments#environment-based-startup-class-and-methods)可用來根據環境在啟動時設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-377">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="d7bf8-378">或者，將 `IHostingEnvironment` 插入至 `Startup` 建構函式，以便用於 `ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-378">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="d7bf8-379">除了 `IsDevelopment` 擴充方法，`IHostingEnvironment` 也提供 `IsStaging`、`IsProduction` 和 `IsEnvironment(string environmentName)` 方法。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-379">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="d7bf8-380">如需詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-380">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="d7bf8-381">`IHostingEnvironment` 服務也可直接插入至 `Configure` 方法，以便設定處理管線：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-381">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="d7bf8-382">建立自訂[中介軟體](xref:fundamentals/middleware/write)時，`IHostingEnvironment` 可以插入至 `Invoke` 方法：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-382">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/write):</span></span>

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

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="ihostapplicationlifetime-interface"></a><span data-ttu-id="d7bf8-383">IHostApplicationLifetime 介面</span><span class="sxs-lookup"><span data-stu-id="d7bf8-383">IHostApplicationLifetime interface</span></span>

<span data-ttu-id="d7bf8-384">`IHostApplicationLifetime`允許啟動後和關機活動。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-384">`IHostApplicationLifetime` allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="d7bf8-385">在介面上的三個屬性是用來註冊定義啟動和關閉事件之 `Action` 方法的取消語彙基元。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-385">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="d7bf8-386">取消權杖</span><span class="sxs-lookup"><span data-stu-id="d7bf8-386">Cancellation Token</span></span>    | <span data-ttu-id="d7bf8-387">觸發時機&#8230;</span><span class="sxs-lookup"><span data-stu-id="d7bf8-387">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="d7bf8-388">已完全啟動主機。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-388">The host has fully started.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="d7bf8-389">主機正在完成正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-389">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="d7bf8-390">應該處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-390">All requests should be processed.</span></span> <span data-ttu-id="d7bf8-391">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-391">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="d7bf8-392">主機正在執行正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-392">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="d7bf8-393">可能仍在處理要求。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-393">Requests may still be processing.</span></span> <span data-ttu-id="d7bf8-394">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-394">Shutdown blocks until this event completes.</span></span> |

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app, IHostApplicationLifetime appLifetime)
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

<span data-ttu-id="d7bf8-395">`StopApplication` 會要求應用程式終止。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-395">`StopApplication` requests termination of the app.</span></span> <span data-ttu-id="d7bf8-396">當呼叫類別的 `Shutdown` 方法時，下列類別使用 `StopApplication` 來順利關閉應用程式：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-396">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

```csharp
public class MyClass
{
    private readonly IHostApplicationLifetime _appLifetime;

    public MyClass(IHostApplicationLifetime appLifetime)
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

::: moniker range="< aspnetcore-3.0"

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="d7bf8-397">IApplicationLifetime 介面</span><span class="sxs-lookup"><span data-stu-id="d7bf8-397">IApplicationLifetime interface</span></span>

<span data-ttu-id="d7bf8-398">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) 允許啟動後和關機活動。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-398">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="d7bf8-399">在介面上的三個屬性是用來註冊定義啟動和關閉事件之 `Action` 方法的取消語彙基元。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-399">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="d7bf8-400">取消權杖</span><span class="sxs-lookup"><span data-stu-id="d7bf8-400">Cancellation Token</span></span>    | <span data-ttu-id="d7bf8-401">觸發時機&#8230;</span><span class="sxs-lookup"><span data-stu-id="d7bf8-401">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="d7bf8-402">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="d7bf8-402">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="d7bf8-403">已完全啟動主機。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-403">The host has fully started.</span></span> |
| [<span data-ttu-id="d7bf8-404">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="d7bf8-404">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="d7bf8-405">主機正在完成正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-405">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="d7bf8-406">應該處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-406">All requests should be processed.</span></span> <span data-ttu-id="d7bf8-407">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-407">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="d7bf8-408">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="d7bf8-408">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="d7bf8-409">主機正在執行正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-409">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="d7bf8-410">可能仍在處理要求。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-410">Requests may still be processing.</span></span> <span data-ttu-id="d7bf8-411">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-411">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="d7bf8-412">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) 會要求應用程式終止。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-412">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="d7bf8-413">當呼叫類別的 `Shutdown` 方法時，下列類別使用 `StopApplication` 來順利關閉應用程式：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-413">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="d7bf8-414">範圍驗證</span><span class="sxs-lookup"><span data-stu-id="d7bf8-414">Scope validation</span></span>

<span data-ttu-id="d7bf8-415">如果應用程式的環境是「開發」，[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 會將 [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) 設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-415">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="d7bf8-416">當 `ValidateScopes` 設定為 `true` 時，預設服務提供者會執行檢查以確認：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-416">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="d7bf8-417">範圍服務不是直接或間接由開機服務提供者解析。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-417">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="d7bf8-418">範圍服務不是直接或間接插入至單一服務。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-418">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="d7bf8-419">根服務提供者會在呼叫 [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) 時建立。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-419">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="d7bf8-420">當提供者啟動應用程式時，根服務提供者的存留期與應用程式/伺服器的存留期一致，並會在應用程式關閉時處置。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-420">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="d7bf8-421">範圍服務會由建立這些服務的容器處置。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-421">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="d7bf8-422">若是在根容器中建立範圍服務，因為當應用程式/伺服器關機時，服務只會由根容器處理，所以服務的存留期會提升為單一服務等級。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-422">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="d7bf8-423">當呼叫 `BuildServiceProvider` 時，驗證服務範圍會攔截到這些情況。</span><span class="sxs-lookup"><span data-stu-id="d7bf8-423">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="d7bf8-424">若要一律驗證範圍，包括在實際執行環境中，請使用主機產生器上的 [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) 來設定 [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions)：</span><span class="sxs-lookup"><span data-stu-id="d7bf8-424">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="additional-resources"></a><span data-ttu-id="d7bf8-425">其他資源</span><span class="sxs-lookup"><span data-stu-id="d7bf8-425">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
