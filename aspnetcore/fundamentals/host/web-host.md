---
title: ASP.NET Core Web 主機
author: guardrex
description: 深入了解 ASP.NET Core 中的 Web 主機，其負責啟動應用程式以及管理存留期。
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: 98070f49c98919e7ebff41ecc69c953249977dcc
ms.sourcegitcommit: e22097b84d26a812cd1380a6b2d12c93e522c125
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/22/2018
ms.locfileid: "36314145"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="be4ff-103">ASP.NET Core Web 主機</span><span class="sxs-lookup"><span data-stu-id="be4ff-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="be4ff-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="be4ff-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="be4ff-105">ASP.NET Core 應用程式會設定並啟動「主機」。</span><span class="sxs-lookup"><span data-stu-id="be4ff-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="be4ff-106">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="be4ff-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="be4ff-107">至少，主機會設定伺服器和要求處理管線。</span><span class="sxs-lookup"><span data-stu-id="be4ff-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="be4ff-108">本主題涵蓋 ASP.NET Core Web 主機 ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder))，這對裝載 Web 應用程式很實用。</span><span class="sxs-lookup"><span data-stu-id="be4ff-108">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="be4ff-109">如需 .NET 泛型主機 ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)) 的內容，請參閱[泛型主機](xref:fundamentals/host/generic-host)主題。</span><span class="sxs-lookup"><span data-stu-id="be4ff-109">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="be4ff-110">設定主機</span><span class="sxs-lookup"><span data-stu-id="be4ff-110">Set up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be4ff-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="be4ff-112">使用 ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)) 的執行個體建立主機。</span><span class="sxs-lookup"><span data-stu-id="be4ff-112">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="be4ff-113">這通常在應用程式的進入點執行，也就是 `Main` 方法。</span><span class="sxs-lookup"><span data-stu-id="be4ff-113">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="be4ff-114">在專案範本中，`Main` 位於 *Program.cs*。</span><span class="sxs-lookup"><span data-stu-id="be4ff-114">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="be4ff-115">一般 *Program.cs* 會呼叫 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 以開始設定主機：</span><span class="sxs-lookup"><span data-stu-id="be4ff-115">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="be4ff-116">`CreateDefaultBuilder` 會執行下列工作：</span><span class="sxs-lookup"><span data-stu-id="be4ff-116">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="be4ff-117">將 [Kestrel](xref:fundamentals/servers/kestrel) 設定為網頁伺服器，並使用應用程式主機組態提供者設定伺服器。</span><span class="sxs-lookup"><span data-stu-id="be4ff-117">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="be4ff-118">如需 Kestrel 預設選項，請參閱 [ASP.NET Core 中的 Kestrel 網頁伺服器實作的 Kestrel 選項一節](xref:fundamentals/servers/kestrel#kestrel-options)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-118">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="be4ff-119">設定 [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) 所傳回路徑的內容根目錄。</span><span class="sxs-lookup"><span data-stu-id="be4ff-119">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="be4ff-120">從下列項目載入[主機組態](#host-configuration-values)：</span><span class="sxs-lookup"><span data-stu-id="be4ff-120">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="be4ff-121">前面加上 `ASPNETCORE_` 的環境變數 (例如，`ASPNETCORE_ENVIRONMENT`)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-121">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="be4ff-122">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="be4ff-122">Command-line arguments.</span></span>
* <span data-ttu-id="be4ff-123">從下列項目載入應用程式組態：</span><span class="sxs-lookup"><span data-stu-id="be4ff-123">Loads app configuration from:</span></span>
  * <span data-ttu-id="be4ff-124">*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="be4ff-124">*appsettings.json*.</span></span>
  * <span data-ttu-id="be4ff-125">*appsettings.{Environment}.json*</span><span class="sxs-lookup"><span data-stu-id="be4ff-125">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="be4ff-126">應用程式在使用輸入組件的 `Development` 環境中執行時的[使用者密碼](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-126">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="be4ff-127">環境變數。</span><span class="sxs-lookup"><span data-stu-id="be4ff-127">Environment variables.</span></span>
  * <span data-ttu-id="be4ff-128">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="be4ff-128">Command-line arguments.</span></span>
* <span data-ttu-id="be4ff-129">設定主控台和偵錯輸出的[記錄](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-129">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="be4ff-130">記錄包含 *appsettings.json* 或 *appsettings.{Environment}.json* 檔案的記錄組態區段中指定的[記錄檔篩選](xref:fundamentals/logging/index#log-filtering)規則。</span><span class="sxs-lookup"><span data-stu-id="be4ff-130">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="be4ff-131">在 IIS 背後執行時，啟用 [IIS 整合](xref:host-and-deploy/iis/index)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-131">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="be4ff-132">設定使用 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)時伺服器接聽的基底路徑和連接埠。</span><span class="sxs-lookup"><span data-stu-id="be4ff-132">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="be4ff-133">此模組會在 IIS 和 Kestrel 之間建立反向 Proxy。</span><span class="sxs-lookup"><span data-stu-id="be4ff-133">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="be4ff-134">也會設定應用程式以[擷取啟動錯誤](#capture-startup-errors)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-134">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="be4ff-135">如需 IIS 預設選項，請參閱[使用 IIS 在 Windows 上裝載 ASP.NET Core 的 IIS 選項一節](xref:host-and-deploy/iis/index#iis-options)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-135">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>
* <span data-ttu-id="be4ff-136">如果應用程式的環境是「開發」，請將 [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) 設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="be4ff-136">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="be4ff-137">如需詳細資訊，請參閱[範圍驗證](#scope-validation)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-137">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="be4ff-138">`CreateDefaultBuilder` 所定義的組態可以透過 [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration)、[ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging)，以及 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) 的其他方法和擴充方法加以覆寫及擴增。</span><span class="sxs-lookup"><span data-stu-id="be4ff-138">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="be4ff-139">數個範例如下：</span><span class="sxs-lookup"><span data-stu-id="be4ff-139">A few examples follow:</span></span>

* <span data-ttu-id="be4ff-140">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) 用來指定應用程式的其他 `IConfiguration`。</span><span class="sxs-lookup"><span data-stu-id="be4ff-140">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="be4ff-141">下列 `ConfigureAppConfiguration` 呼叫會新增委派，以在 *appsettings.xml* 檔案中包含應用程式組態。</span><span class="sxs-lookup"><span data-stu-id="be4ff-141">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="be4ff-142">可能會多次呼叫 `ConfigureAppConfiguration`。</span><span class="sxs-lookup"><span data-stu-id="be4ff-142">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="be4ff-143">請注意，此組態不適用於主機 (例如，伺服器 URL 或環境)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-143">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="be4ff-144">請參閱[主機組態值](#host-configuration-values)一節。</span><span class="sxs-lookup"><span data-stu-id="be4ff-144">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="be4ff-145">下列 `ConfigureLogging` 呼叫會新增委派，將最低的記錄層級 ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) 設定為 [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-145">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="be4ff-146">此設定會覆寫 `CreateDefaultBuilder` 所設定之 *ppsettings.Development.json* (`LogLevel.Debug`) 和 *appsettings.Production.json* (`LogLevel.Error`) 中的設定。</span><span class="sxs-lookup"><span data-stu-id="be4ff-146">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="be4ff-147">可能會多次呼叫 `ConfigureLogging`。</span><span class="sxs-lookup"><span data-stu-id="be4ff-147">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

* <span data-ttu-id="be4ff-148">下列 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) 呼叫會覆寫當 `CreateDefaultBuilder` 設定 Kestrel 時建立的預設 [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) 30,000,000 位元組：</span><span class="sxs-lookup"><span data-stu-id="be4ff-148">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

<span data-ttu-id="be4ff-149">「內容根目錄」會決定主機搜尋內容檔案 (例如 MVC 檢視檔案) 的位置。</span><span class="sxs-lookup"><span data-stu-id="be4ff-149">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="be4ff-150">從專案的根資料夾啟動應用程式時，會使用專案的根資料夾作為內容根目錄。</span><span class="sxs-lookup"><span data-stu-id="be4ff-150">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="be4ff-151">這是 [Visual Studio](https://www.visualstudio.com/) 和 [dotnet 新範本](/dotnet/core/tools/dotnet-new)中使用的預設值。</span><span class="sxs-lookup"><span data-stu-id="be4ff-151">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="be4ff-152">如需應用程式設定的詳細資訊，請參閱 [ASP.NET Core 中的組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-152">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="be4ff-153">作為使用靜態 `CreateDefaultBuilder` 方法的替代做法，ASP.NET Core 2.x 支援從 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 建立主機的方法。</span><span class="sxs-lookup"><span data-stu-id="be4ff-153">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="be4ff-154">如需詳細資訊，請參閱 ASP.NET Core 1.x 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="be4ff-154">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be4ff-155">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-155">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="be4ff-156">使用 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 的執行個體建立主機。</span><span class="sxs-lookup"><span data-stu-id="be4ff-156">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="be4ff-157">建立主機通常在應用程式的進入點執行，也就是 `Main` 方法。</span><span class="sxs-lookup"><span data-stu-id="be4ff-157">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="be4ff-158">在專案範本中，`Main` 位於 *Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="be4ff-158">In the project templates, `Main` is located in *Program.cs*:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>()
            .Build();
}
```

<span data-ttu-id="be4ff-159">`WebHostBuilder` 需要[實作 IServer 的伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-159">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="be4ff-160">內建的伺服器是 [Kestrel](xref:fundamentals/servers/kestrel) 和 [HTTP.sys](xref:fundamentals/servers/httpsys) (在 ASP.NET Core 2.0 版之前，HTTP.sys 稱為 [WebListener](xref:fundamentals/servers/weblistener))。</span><span class="sxs-lookup"><span data-stu-id="be4ff-160">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="be4ff-161">在此範例中，[UseKestrel 擴充方法](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)會指定 Kestrel 伺服器。</span><span class="sxs-lookup"><span data-stu-id="be4ff-161">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="be4ff-162">「內容根目錄」會決定主機搜尋內容檔案 (例如 MVC 檢視檔案) 的位置。</span><span class="sxs-lookup"><span data-stu-id="be4ff-162">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="be4ff-163">[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1) 會取得 `UseContentRoot` 的預設內容根目錄。</span><span class="sxs-lookup"><span data-stu-id="be4ff-163">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="be4ff-164">從專案的根資料夾啟動應用程式時，會使用專案的根資料夾作為內容根目錄。</span><span class="sxs-lookup"><span data-stu-id="be4ff-164">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="be4ff-165">這是 [Visual Studio](https://www.visualstudio.com/) 和 [dotnet 新範本](/dotnet/core/tools/dotnet-new)中使用的預設值。</span><span class="sxs-lookup"><span data-stu-id="be4ff-165">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="be4ff-166">若要使用 IIS 作為反向 Proxy，請在建置主機時呼叫 [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-166">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="be4ff-167">`UseIISIntegration` 不會像 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)那樣設定「伺服器」。</span><span class="sxs-lookup"><span data-stu-id="be4ff-167">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="be4ff-168">`UseIISIntegration` 會設定使用 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)以在 Kestrel 和 IIS 之間建立反向 Proxy 時，伺服器接聽的基底路徑和連接埠。</span><span class="sxs-lookup"><span data-stu-id="be4ff-168">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="be4ff-169">若要搭配使用 IIS 與 ASP.NET Core，必須指定 `UseKestrel` 和 `UseIISIntegration`。</span><span class="sxs-lookup"><span data-stu-id="be4ff-169">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="be4ff-170">`UseIISIntegration` 只會在 IIS 或 IIS Express 背後執行時啟動。</span><span class="sxs-lookup"><span data-stu-id="be4ff-170">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="be4ff-171">如需詳細資訊，請參閱 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)和 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-171">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="be4ff-172">設定主機 (和 ASP.NET Core 應用程式) 的最小實作，包含指定伺服器和應用程式要求管線的組態：</span><span class="sxs-lookup"><span data-stu-id="be4ff-172">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(context => context.Response.WriteAsync("Hello World!"));
    })
    .Build();

host.Run();
```

---

<span data-ttu-id="be4ff-173">設定主機時，可以提供 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) 和 [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) 方法。</span><span class="sxs-lookup"><span data-stu-id="be4ff-173">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="be4ff-174">如果指定 `Startup` 類別，它必須定義 `Configure` 方法。</span><span class="sxs-lookup"><span data-stu-id="be4ff-174">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="be4ff-175">如需詳細資訊，請參閱 [ASP.NET Core 中的應用程式啟動](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-175">For more information, see [Application Startup in ASP.NET Core](xref:fundamentals/startup).</span></span> <span data-ttu-id="be4ff-176">多次呼叫 `ConfigureServices` 會彼此附加。</span><span class="sxs-lookup"><span data-stu-id="be4ff-176">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="be4ff-177">對 `WebHostBuilder` 多次呼叫 `Configure` 或 `UseStartup` 則會取代先前的設定。</span><span class="sxs-lookup"><span data-stu-id="be4ff-177">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="be4ff-178">主機組態值</span><span class="sxs-lookup"><span data-stu-id="be4ff-178">Host configuration values</span></span>

<span data-ttu-id="be4ff-179">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 依賴下列方法來設定主機組態值：</span><span class="sxs-lookup"><span data-stu-id="be4ff-179">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="be4ff-180">主機建立器組態，其中包含 `ASPNETCORE_{configurationKey}` 格式的環境變數。</span><span class="sxs-lookup"><span data-stu-id="be4ff-180">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="be4ff-181">例如，`ASPNETCORE_ENVIRONMENT`。</span><span class="sxs-lookup"><span data-stu-id="be4ff-181">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="be4ff-182">[UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) 和 [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 之類的延伸模組 (請參閱[覆寫組態](#override-configuration)一節)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-182">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="be4ff-183">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) 和相關聯的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="be4ff-183">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="be4ff-184">使用 `UseSetting` 設定值時，此值會設定為字串，而不論其類型為何。</span><span class="sxs-lookup"><span data-stu-id="be4ff-184">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="be4ff-185">主機使用設定最後一個值的任何選項。</span><span class="sxs-lookup"><span data-stu-id="be4ff-185">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="be4ff-186">如需詳細資訊，請參閱下一節的[覆寫組態](#override-configuration)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-186">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="be4ff-187">擷取啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="be4ff-187">Capture Startup Errors</span></span>

<span data-ttu-id="be4ff-188">此設定會控制啟動錯誤的擷取。</span><span class="sxs-lookup"><span data-stu-id="be4ff-188">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="be4ff-189">**索引鍵**：captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="be4ff-189">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="be4ff-190">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="be4ff-190">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="be4ff-191">**預設值**：預設為 `false`，除非應用程式執行時在 IIS 背後有 Kestrel，此時預設值即為 `true`。</span><span class="sxs-lookup"><span data-stu-id="be4ff-191">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="be4ff-192">**設定使用**：`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="be4ff-192">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="be4ff-193">**環境變數**：`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="be4ff-193">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="be4ff-194">當它為 `false` 時，啟動期間發生的錯誤會導致主機結束。</span><span class="sxs-lookup"><span data-stu-id="be4ff-194">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="be4ff-195">當它為 `true` 時，主機會擷取啟動期間的例外狀況，並嘗試啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="be4ff-195">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be4ff-196">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-196">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be4ff-197">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-197">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

---

### <a name="content-root"></a><span data-ttu-id="be4ff-198">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="be4ff-198">Content Root</span></span>

<span data-ttu-id="be4ff-199">此設定可決定 ASP.NET Core 開始搜尋內容檔案 (例如 MVC 檢視) 的位置。</span><span class="sxs-lookup"><span data-stu-id="be4ff-199">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="be4ff-200">**索引鍵**：contentRoot</span><span class="sxs-lookup"><span data-stu-id="be4ff-200">**Key**: contentRoot</span></span>  
<span data-ttu-id="be4ff-201">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="be4ff-201">**Type**: *string*</span></span>  
<span data-ttu-id="be4ff-202">**預設值**：預設為應用程式組件所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="be4ff-202">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="be4ff-203">**設定使用**：`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="be4ff-203">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="be4ff-204">**環境變數**：`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="be4ff-204">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="be4ff-205">內容根目錄也作為 [Web 根目錄設定](#web-root)的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="be4ff-205">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="be4ff-206">如果路徑不存在，就無法啟動主機。</span><span class="sxs-lookup"><span data-stu-id="be4ff-206">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be4ff-207">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-207">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be4ff-208">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-208">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

---

### <a name="detailed-errors"></a><span data-ttu-id="be4ff-209">詳細錯誤</span><span class="sxs-lookup"><span data-stu-id="be4ff-209">Detailed Errors</span></span>

<span data-ttu-id="be4ff-210">決定是否應該擷取詳細的錯誤。</span><span class="sxs-lookup"><span data-stu-id="be4ff-210">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="be4ff-211">**索引鍵**：detailedErrors</span><span class="sxs-lookup"><span data-stu-id="be4ff-211">**Key**: detailedErrors</span></span>  
<span data-ttu-id="be4ff-212">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="be4ff-212">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="be4ff-213">**預設值**：false</span><span class="sxs-lookup"><span data-stu-id="be4ff-213">**Default**: false</span></span>  
<span data-ttu-id="be4ff-214">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="be4ff-214">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="be4ff-215">**環境變數**：`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="be4ff-215">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="be4ff-216">啟用時 (或當 <a href="#environment">Environment</a> 設定為 `Development` 時)，應用程式會擷取詳細例外狀況。</span><span class="sxs-lookup"><span data-stu-id="be4ff-216">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be4ff-217">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-217">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be4ff-218">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-218">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

---

### <a name="environment"></a><span data-ttu-id="be4ff-219">環境</span><span class="sxs-lookup"><span data-stu-id="be4ff-219">Environment</span></span>

<span data-ttu-id="be4ff-220">設定應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="be4ff-220">Sets the app's environment.</span></span>

<span data-ttu-id="be4ff-221">**索引鍵**：environment</span><span class="sxs-lookup"><span data-stu-id="be4ff-221">**Key**: environment</span></span>  
<span data-ttu-id="be4ff-222">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="be4ff-222">**Type**: *string*</span></span>  
<span data-ttu-id="be4ff-223">**預設值**：Production</span><span class="sxs-lookup"><span data-stu-id="be4ff-223">**Default**: Production</span></span>  
<span data-ttu-id="be4ff-224">**設定使用**：`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="be4ff-224">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="be4ff-225">**環境變數**：`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="be4ff-225">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="be4ff-226">環境可以設定為任何值。</span><span class="sxs-lookup"><span data-stu-id="be4ff-226">The environment can be set to any value.</span></span> <span data-ttu-id="be4ff-227">架構定義的值包括 `Development`、`Staging` 和 `Production`。</span><span class="sxs-lookup"><span data-stu-id="be4ff-227">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="be4ff-228">值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="be4ff-228">Values aren't case sensitive.</span></span> <span data-ttu-id="be4ff-229">根據預設，*Environment* 是從 `ASPNETCORE_ENVIRONMENT` 環境變數讀取。</span><span class="sxs-lookup"><span data-stu-id="be4ff-229">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="be4ff-230">使用 [Visual Studio](https://www.visualstudio.com/) 時，可能會在 *launchSettings.json* 檔案設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="be4ff-230">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="be4ff-231">如需詳細資訊，請參閱[使用多重環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-231">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be4ff-232">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-232">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be4ff-233">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-233">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="be4ff-234">裝載啟動組件</span><span class="sxs-lookup"><span data-stu-id="be4ff-234">Hosting Startup Assemblies</span></span>

<span data-ttu-id="be4ff-235">設定應用程式的裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="be4ff-235">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="be4ff-236">**索引鍵**：hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="be4ff-236">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="be4ff-237">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="be4ff-237">**Type**: *string*</span></span>  
<span data-ttu-id="be4ff-238">**預設值**：空字串</span><span class="sxs-lookup"><span data-stu-id="be4ff-238">**Default**: Empty string</span></span>  
<span data-ttu-id="be4ff-239">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="be4ff-239">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="be4ff-240">**環境變數**：`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="be4ff-240">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="be4ff-241">在啟動時載入的裝載啟動組件字串，以分號分隔。</span><span class="sxs-lookup"><span data-stu-id="be4ff-241">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="be4ff-242">這項功能是在 ASP.NET Core 2.0 中新增。</span><span class="sxs-lookup"><span data-stu-id="be4ff-242">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="be4ff-243">雖然設定值會預設為空字串，但裝載啟動組件一律會包含應用程式的組件。</span><span class="sxs-lookup"><span data-stu-id="be4ff-243">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="be4ff-244">提供裝載啟動組件時，它們會新增至應用程式的組件，以便在應用程式在啟動時建置其通用服務時載入。</span><span class="sxs-lookup"><span data-stu-id="be4ff-244">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be4ff-245">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-245">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be4ff-246">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-246">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="be4ff-247">這項功能在 ASP.NET Core 1.x 中未提供。</span><span class="sxs-lookup"><span data-stu-id="be4ff-247">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="be4ff-248">偏好裝載 URL</span><span class="sxs-lookup"><span data-stu-id="be4ff-248">Prefer Hosting URLs</span></span>

<span data-ttu-id="be4ff-249">表示主機是否應接聽使用 `WebHostBuilder` 設定的 URL，而不是 `IServer` 實作所設定的 URL。</span><span class="sxs-lookup"><span data-stu-id="be4ff-249">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="be4ff-250">**索引鍵**：preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="be4ff-250">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="be4ff-251">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="be4ff-251">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="be4ff-252">**預設值**：true</span><span class="sxs-lookup"><span data-stu-id="be4ff-252">**Default**: true</span></span>  
<span data-ttu-id="be4ff-253">**設定使用**：`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="be4ff-253">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="be4ff-254">**環境變數**：`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="be4ff-254">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="be4ff-255">這項功能是在 ASP.NET Core 2.0 中新增。</span><span class="sxs-lookup"><span data-stu-id="be4ff-255">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be4ff-256">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-256">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be4ff-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="be4ff-258">這項功能在 ASP.NET Core 1.x 中未提供。</span><span class="sxs-lookup"><span data-stu-id="be4ff-258">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="be4ff-259">防止裝載啟動</span><span class="sxs-lookup"><span data-stu-id="be4ff-259">Prevent Hosting Startup</span></span>

<span data-ttu-id="be4ff-260">可防止自動載入裝載啟動組件，包括應用程式組件所設定的裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="be4ff-260">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="be4ff-261">如需詳細資訊，請參閱[使用 IHostingStartup 從外部組件增強應用程式](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-261">See [Enhance an app from an external assembly with IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) for more information.</span></span>

<span data-ttu-id="be4ff-262">**索引鍵**preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="be4ff-262">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="be4ff-263">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="be4ff-263">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="be4ff-264">**預設值**：false</span><span class="sxs-lookup"><span data-stu-id="be4ff-264">**Default**: false</span></span>  
<span data-ttu-id="be4ff-265">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="be4ff-265">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="be4ff-266">**環境變數**：`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="be4ff-266">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="be4ff-267">這項功能是在 ASP.NET Core 2.0 中新增。</span><span class="sxs-lookup"><span data-stu-id="be4ff-267">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be4ff-268">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-268">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be4ff-269">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-269">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="be4ff-270">這項功能在 ASP.NET Core 1.x 中未提供。</span><span class="sxs-lookup"><span data-stu-id="be4ff-270">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="be4ff-271">伺服器 URL</span><span class="sxs-lookup"><span data-stu-id="be4ff-271">Server URLs</span></span>

<span data-ttu-id="be4ff-272">表示伺服器應該接聽要求的 IP 位址或主機位址，以及連接埠和通訊協定。</span><span class="sxs-lookup"><span data-stu-id="be4ff-272">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="be4ff-273">**索引鍵**：urls</span><span class="sxs-lookup"><span data-stu-id="be4ff-273">**Key**: urls</span></span>  
<span data-ttu-id="be4ff-274">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="be4ff-274">**Type**: *string*</span></span>  
<span data-ttu-id="be4ff-275">**預設值**：http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="be4ff-275">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="be4ff-276">**設定使用**：`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="be4ff-276">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="be4ff-277">**環境變數**：`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="be4ff-277">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="be4ff-278">設定為伺服器應該回應的 URL 前置清單，並以分號 (;) 分隔。</span><span class="sxs-lookup"><span data-stu-id="be4ff-278">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="be4ff-279">例如，`http://localhost:123`。</span><span class="sxs-lookup"><span data-stu-id="be4ff-279">For example, `http://localhost:123`.</span></span> <span data-ttu-id="be4ff-280">使用 "\*"，表示伺服器應接聽任何 IP 位址或主機名稱上的要求，並使用指定的連接埠和通訊協定 (例如，`http://*:5000`)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-280">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="be4ff-281">通訊協定 (`http://` 或 `https://`) 必須包含在每個 URL 中。</span><span class="sxs-lookup"><span data-stu-id="be4ff-281">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="be4ff-282">伺服器之間支援的格式有所不同。</span><span class="sxs-lookup"><span data-stu-id="be4ff-282">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be4ff-283">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-283">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="be4ff-284">Kestrel 有它自己的端點設定 API。</span><span class="sxs-lookup"><span data-stu-id="be4ff-284">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="be4ff-285">如需詳細資訊，請參閱[在 ASP.NET Core 中實作 Kestrel 網頁伺服器](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-285">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be4ff-286">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-286">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="be4ff-287">關機逾時</span><span class="sxs-lookup"><span data-stu-id="be4ff-287">Shutdown Timeout</span></span>

<span data-ttu-id="be4ff-288">指定等待 Web 主機關機的時間長度。</span><span class="sxs-lookup"><span data-stu-id="be4ff-288">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="be4ff-289">**索引鍵**：shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="be4ff-289">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="be4ff-290">**類型**：*int*</span><span class="sxs-lookup"><span data-stu-id="be4ff-290">**Type**: *int*</span></span>  
<span data-ttu-id="be4ff-291">**預設值**：5</span><span class="sxs-lookup"><span data-stu-id="be4ff-291">**Default**: 5</span></span>  
<span data-ttu-id="be4ff-292">**設定使用**：`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="be4ff-292">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="be4ff-293">**環境變數**：`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="be4ff-293">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="be4ff-294">雖然索引鍵接受 *int* 與 `UseSetting` (例如 `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`)，[UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) 擴充方法會採用 [TimeSpan](/dotnet/api/system.timespan)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-294">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span> <span data-ttu-id="be4ff-295">這項功能是在 ASP.NET Core 2.0 中新增。</span><span class="sxs-lookup"><span data-stu-id="be4ff-295">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="be4ff-296">在逾時期間，裝載會：</span><span class="sxs-lookup"><span data-stu-id="be4ff-296">During the timeout period, hosting:</span></span>

* <span data-ttu-id="be4ff-297">觸發 [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-297">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="be4ff-298">嘗試停止託管的服務，並記錄無法停止之服務的任何錯誤。</span><span class="sxs-lookup"><span data-stu-id="be4ff-298">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="be4ff-299">如果在所有的託管服務停止之前逾時期限已到期，則應用程式關閉時，會停止任何剩餘的作用中服務。</span><span class="sxs-lookup"><span data-stu-id="be4ff-299">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="be4ff-300">即使服務尚未完成處理也會停止。</span><span class="sxs-lookup"><span data-stu-id="be4ff-300">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="be4ff-301">如果服務需要更多時間才能停止，請增加逾時。</span><span class="sxs-lookup"><span data-stu-id="be4ff-301">If services require additional time to stop, increase the timeout.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be4ff-302">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-302">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be4ff-303">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-303">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="be4ff-304">這項功能在 ASP.NET Core 1.x 中未提供。</span><span class="sxs-lookup"><span data-stu-id="be4ff-304">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="be4ff-305">啟動組件</span><span class="sxs-lookup"><span data-stu-id="be4ff-305">Startup Assembly</span></span>

<span data-ttu-id="be4ff-306">決定要搜尋 `Startup` 類別的組件。</span><span class="sxs-lookup"><span data-stu-id="be4ff-306">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="be4ff-307">**索引鍵**：startupAssembly</span><span class="sxs-lookup"><span data-stu-id="be4ff-307">**Key**: startupAssembly</span></span>  
<span data-ttu-id="be4ff-308">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="be4ff-308">**Type**: *string*</span></span>  
<span data-ttu-id="be4ff-309">**預設值**：應用程式的組件</span><span class="sxs-lookup"><span data-stu-id="be4ff-309">**Default**: The app's assembly</span></span>  
<span data-ttu-id="be4ff-310">**設定使用**：`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="be4ff-310">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="be4ff-311">**環境變數**：`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="be4ff-311">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="be4ff-312">可以參考依名稱 (`string`) 或類型 (`TStartup`) 的組件。</span><span class="sxs-lookup"><span data-stu-id="be4ff-312">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="be4ff-313">如果呼叫多個 `UseStartup` 方法，最後一個將會優先。</span><span class="sxs-lookup"><span data-stu-id="be4ff-313">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be4ff-314">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-314">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be4ff-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

---

### <a name="web-root"></a><span data-ttu-id="be4ff-316">Web 根目錄</span><span class="sxs-lookup"><span data-stu-id="be4ff-316">Web Root</span></span>

<span data-ttu-id="be4ff-317">設定應用程式靜態資產的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="be4ff-317">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="be4ff-318">**索引鍵**：webroot</span><span class="sxs-lookup"><span data-stu-id="be4ff-318">**Key**: webroot</span></span>  
<span data-ttu-id="be4ff-319">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="be4ff-319">**Type**: *string*</span></span>  
<span data-ttu-id="be4ff-320">**預設值**：如果未指定，則預設值為 "(Content Root)/wwwroot"，如果該路徑存在的話。</span><span class="sxs-lookup"><span data-stu-id="be4ff-320">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="be4ff-321">如果路徑不存在，則會使用無作業檔案提供者。</span><span class="sxs-lookup"><span data-stu-id="be4ff-321">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="be4ff-322">**設定使用**：`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="be4ff-322">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="be4ff-323">**環境變數**：`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="be4ff-323">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be4ff-324">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-324">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be4ff-325">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-325">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

---

## <a name="override-configuration"></a><span data-ttu-id="be4ff-326">覆寫組態</span><span class="sxs-lookup"><span data-stu-id="be4ff-326">Override configuration</span></span>

<span data-ttu-id="be4ff-327">使用 [Configuration](xref:fundamentals/configuration/index) 來設定 Web 主機。</span><span class="sxs-lookup"><span data-stu-id="be4ff-327">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="be4ff-328">在下列範例中，主機組態選擇性指定於 *hostsettings.json* 檔案中。</span><span class="sxs-lookup"><span data-stu-id="be4ff-328">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="be4ff-329">從 *hostsettings.json* 檔案載入的任何組態，可以用命令列引數加以覆寫。</span><span class="sxs-lookup"><span data-stu-id="be4ff-329">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="be4ff-330">建置的組態 (在 `config` 中) 用以使用 [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 來設定主機。</span><span class="sxs-lookup"><span data-stu-id="be4ff-330">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="be4ff-331">`IWebHostBuilder` 設定會新增到應用程式的組態，但反之則不然 &mdash;`ConfigureAppConfiguration` 並不會影響 `IWebHostBuilder` 組態。</span><span class="sxs-lookup"><span data-stu-id="be4ff-331">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be4ff-332">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-332">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="be4ff-333">首先使用 *hostsettings.json* 組態來覆寫 `UseUrls` 所提供的組態，其次是命令列引數組態：</span><span class="sxs-lookup"><span data-stu-id="be4ff-333">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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
            })
            .Build();
    }
}
```

<span data-ttu-id="be4ff-334">*hostsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="be4ff-334">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be4ff-335">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-335">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="be4ff-336">首先使用 *hostsettings.json* 組態來覆寫 `UseUrls` 所提供的組態，其次是命令列引數組態：</span><span class="sxs-lookup"><span data-stu-id="be4ff-336">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
            .AddCommandLine(args)
            .Build();

        var host = new WebHostBuilder()
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .UseKestrel()
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();

        host.Run();
    }
}
```

<span data-ttu-id="be4ff-337">*hostsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="be4ff-337">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

---

> [!NOTE]
> <span data-ttu-id="be4ff-338">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 擴充方法目前無法剖析 `GetSection` 傳回的組態區段 (例如 `.UseConfiguration(Configuration.GetSection("section"))`。</span><span class="sxs-lookup"><span data-stu-id="be4ff-338">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="be4ff-339">`GetSection` 方法會將設定索引鍵篩選到要求的區段，但保留索引鍵上的區段名稱 (例如 `section:urls`、`section:environment`)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-339">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="be4ff-340">`UseConfiguration` 方法預期索引鍵要符合 `WebHostBuilder` 索引鍵 (例如 `urls`、`environment`)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-340">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="be4ff-341">索引鍵上的區段名稱存在可避免區段的值設定主機。</span><span class="sxs-lookup"><span data-stu-id="be4ff-341">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="be4ff-342">這個問題將在近期的版本中解決。</span><span class="sxs-lookup"><span data-stu-id="be4ff-342">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="be4ff-343">如需詳細資訊和因應措施，請參閱 [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (將設定區段傳遞到 WebHostBuilder.UseConfiguration 會使用完整索引鍵)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-343">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="be4ff-344">`UseConfiguration` 只會將索引鍵從提供的 `IConfiguration` 複製到主機建立器的組態。</span><span class="sxs-lookup"><span data-stu-id="be4ff-344">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="be4ff-345">因此，為 JSON、INI 和 XML 設定檔設定 `reloadOnChange: true` 沒有任何作用。</span><span class="sxs-lookup"><span data-stu-id="be4ff-345">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="be4ff-346">若要指定主機在特定 URL 上執行，所要的值可以在執行 [dotnet run](/dotnet/core/tools/dotnet-run) 時從命令提示字元傳入。</span><span class="sxs-lookup"><span data-stu-id="be4ff-346">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="be4ff-347">命令列引數會覆寫 *hostsettings.json* 檔案的 `urls` 值，且伺服器會接聽連接埠 8080：</span><span class="sxs-lookup"><span data-stu-id="be4ff-347">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="be4ff-348">管理主機</span><span class="sxs-lookup"><span data-stu-id="be4ff-348">Manage the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be4ff-349">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-349">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="be4ff-350">**執行**</span><span class="sxs-lookup"><span data-stu-id="be4ff-350">**Run**</span></span>

<span data-ttu-id="be4ff-351">`Run` 方法會啟動 Web 應用程式，並且封鎖呼叫執行緒，直到主機關閉為止：</span><span class="sxs-lookup"><span data-stu-id="be4ff-351">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="be4ff-352">**Start**</span><span class="sxs-lookup"><span data-stu-id="be4ff-352">**Start**</span></span>

<span data-ttu-id="be4ff-353">藉由呼叫 `Start` 方法，以非封鎖方式執行主機：</span><span class="sxs-lookup"><span data-stu-id="be4ff-353">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="be4ff-354">如果 URL 的清單傳遞至 `Start` 方法，它會接聽指定的 URL：</span><span class="sxs-lookup"><span data-stu-id="be4ff-354">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="be4ff-355">應用程式可以使用預先設定的 `CreateDefaultBuilder` 預設值，使用便利的靜態方法初始化並啟動新的主機。</span><span class="sxs-lookup"><span data-stu-id="be4ff-355">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="be4ff-356">這些方法會啟動伺服器而無主控台輸出，且在使用 [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) 時等候中斷 (Ctrl-C/SIGINT 或 SIGTERM)：</span><span class="sxs-lookup"><span data-stu-id="be4ff-356">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="be4ff-357">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="be4ff-357">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="be4ff-358">使用 `RequestDelegate` 啟動：</span><span class="sxs-lookup"><span data-stu-id="be4ff-358">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="be4ff-359">在瀏覽器中提出要求給 `http://localhost:5000`，以收到 "Hello World!" 回應</span><span class="sxs-lookup"><span data-stu-id="be4ff-359">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="be4ff-360">`WaitForShutdown` 會封鎖，直到發出中斷 (Ctrl-C/SIGINT 或 SIGTERM)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-360">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="be4ff-361">應用程式會顯示 `Console.WriteLine` 訊息並等候按鍵動作以便結束。</span><span class="sxs-lookup"><span data-stu-id="be4ff-361">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="be4ff-362">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="be4ff-362">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="be4ff-363">使用 URL 和 `RequestDelegate` 來啟動：</span><span class="sxs-lookup"><span data-stu-id="be4ff-363">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="be4ff-364">產生與 **Start(RequestDelegate app)** 相同的結果，除了應用程式會在 `http://localhost:8080` 回應。</span><span class="sxs-lookup"><span data-stu-id="be4ff-364">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="be4ff-365">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="be4ff-365">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="be4ff-366">使用 `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) 的執行個體來使用路由中介軟體：</span><span class="sxs-lookup"><span data-stu-id="be4ff-366">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="be4ff-367">使用下列瀏覽器要求與範例：</span><span class="sxs-lookup"><span data-stu-id="be4ff-367">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="be4ff-368">要求</span><span class="sxs-lookup"><span data-stu-id="be4ff-368">Request</span></span>                                    | <span data-ttu-id="be4ff-369">回應</span><span class="sxs-lookup"><span data-stu-id="be4ff-369">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="be4ff-370">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="be4ff-370">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="be4ff-371">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="be4ff-371">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="be4ff-372">擲回例外狀況，字串為 "ooops!"</span><span class="sxs-lookup"><span data-stu-id="be4ff-372">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="be4ff-373">擲回例外狀況，字串為 "Un oh!"</span><span class="sxs-lookup"><span data-stu-id="be4ff-373">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="be4ff-374">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="be4ff-374">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="be4ff-375">Hello World!</span><span class="sxs-lookup"><span data-stu-id="be4ff-375">Hello World!</span></span>                             |

<span data-ttu-id="be4ff-376">`WaitForShutdown` 會封鎖，直到發出中斷 (Ctrl-C/SIGINT 或 SIGTERM)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-376">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="be4ff-377">應用程式會顯示 `Console.WriteLine` 訊息並等候按鍵動作以便結束。</span><span class="sxs-lookup"><span data-stu-id="be4ff-377">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="be4ff-378">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="be4ff-378">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="be4ff-379">使用 URL 和 `IRouteBuilder` 的執行個體：</span><span class="sxs-lookup"><span data-stu-id="be4ff-379">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="be4ff-380">產生與 **Start(Action&lt;IRouteBuilder&gt; routeBuilder)** 相同的結果，除了應用程式會在 `http://localhost:8080` 回應。</span><span class="sxs-lookup"><span data-stu-id="be4ff-380">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="be4ff-381">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="be4ff-381">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="be4ff-382">提供委派以設定 `IApplicationBuilder`：</span><span class="sxs-lookup"><span data-stu-id="be4ff-382">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="be4ff-383">在瀏覽器中提出要求給 `http://localhost:5000`，以收到 "Hello World!" 回應</span><span class="sxs-lookup"><span data-stu-id="be4ff-383">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="be4ff-384">`WaitForShutdown` 會封鎖，直到發出中斷 (Ctrl-C/SIGINT 或 SIGTERM)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-384">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="be4ff-385">應用程式會顯示 `Console.WriteLine` 訊息並等候按鍵動作以便結束。</span><span class="sxs-lookup"><span data-stu-id="be4ff-385">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="be4ff-386">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="be4ff-386">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="be4ff-387">提供 URL 和委派以設定 `IApplicationBuilder`：</span><span class="sxs-lookup"><span data-stu-id="be4ff-387">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="be4ff-388">產生與 **StartWith(Action&lt;IApplicationBuilder&gt; app)** 相同的結果，除了應用程式會在 `http://localhost:8080` 回應。</span><span class="sxs-lookup"><span data-stu-id="be4ff-388">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be4ff-389">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be4ff-389">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="be4ff-390">**執行**</span><span class="sxs-lookup"><span data-stu-id="be4ff-390">**Run**</span></span>

<span data-ttu-id="be4ff-391">`Run` 方法會啟動 Web 應用程式，並且封鎖呼叫執行緒，直到主機關閉為止：</span><span class="sxs-lookup"><span data-stu-id="be4ff-391">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="be4ff-392">**Start**</span><span class="sxs-lookup"><span data-stu-id="be4ff-392">**Start**</span></span>

<span data-ttu-id="be4ff-393">藉由呼叫 `Start` 方法，以非封鎖方式執行主機：</span><span class="sxs-lookup"><span data-stu-id="be4ff-393">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="be4ff-394">如果 URL 的清單傳遞至 `Start` 方法，它會接聽指定的 URL：</span><span class="sxs-lookup"><span data-stu-id="be4ff-394">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

---

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="be4ff-395">IHostingEnvironment 介面</span><span class="sxs-lookup"><span data-stu-id="be4ff-395">IHostingEnvironment interface</span></span>

<span data-ttu-id="be4ff-396">[IHostingEnvironment 介面](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment)提供應用程式的 Web 裝載環境相關資訊。</span><span class="sxs-lookup"><span data-stu-id="be4ff-396">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="be4ff-397">使用[建構函式插入](xref:fundamentals/dependency-injection)以取得 `IHostingEnvironment`，才能使用其屬性和擴充方法：</span><span class="sxs-lookup"><span data-stu-id="be4ff-397">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="be4ff-398">[以慣例為基礎的方法](xref:fundamentals/environments#environment-based-startup-class-and-methods)可用來根據環境在啟動時設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="be4ff-398">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="be4ff-399">或者，將 `IHostingEnvironment` 插入至 `Startup` 建構函式，以便用於 `ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="be4ff-399">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="be4ff-400">除了 `IsDevelopment` 擴充方法，`IHostingEnvironment` 也提供 `IsStaging`、`IsProduction` 和 `IsEnvironment(string environmentName)` 方法。</span><span class="sxs-lookup"><span data-stu-id="be4ff-400">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="be4ff-401">如需詳細資料，請參閱[使用多個環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-401">See [Use multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="be4ff-402">`IHostingEnvironment` 服務也可直接插入至 `Configure` 方法，以便設定處理管線：</span><span class="sxs-lookup"><span data-stu-id="be4ff-402">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
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

<span data-ttu-id="be4ff-403">建立自訂[中介軟體](xref:fundamentals/middleware/index#writing-middleware)時，`IHostingEnvironment` 可以插入至 `Invoke` 方法：</span><span class="sxs-lookup"><span data-stu-id="be4ff-403">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="be4ff-404">IApplicationLifetime 介面</span><span class="sxs-lookup"><span data-stu-id="be4ff-404">IApplicationLifetime interface</span></span>

<span data-ttu-id="be4ff-405">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) 允許啟動後和關機活動。</span><span class="sxs-lookup"><span data-stu-id="be4ff-405">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="be4ff-406">在介面上的三個屬性是用來註冊定義啟動和關閉事件之 `Action` 方法的取消語彙基元。</span><span class="sxs-lookup"><span data-stu-id="be4ff-406">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="be4ff-407">取消語彙基元</span><span class="sxs-lookup"><span data-stu-id="be4ff-407">Cancellation Token</span></span>    | <span data-ttu-id="be4ff-408">觸發時機&#8230;</span><span class="sxs-lookup"><span data-stu-id="be4ff-408">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="be4ff-409">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="be4ff-409">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="be4ff-410">已完全啟動主機。</span><span class="sxs-lookup"><span data-stu-id="be4ff-410">The host has fully started.</span></span> |
| [<span data-ttu-id="be4ff-411">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="be4ff-411">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="be4ff-412">主機正在完成正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="be4ff-412">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="be4ff-413">應該處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="be4ff-413">All requests should be processed.</span></span> <span data-ttu-id="be4ff-414">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="be4ff-414">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="be4ff-415">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="be4ff-415">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="be4ff-416">主機正在執行正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="be4ff-416">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="be4ff-417">可能仍在處理要求。</span><span class="sxs-lookup"><span data-stu-id="be4ff-417">Requests may still be processing.</span></span> <span data-ttu-id="be4ff-418">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="be4ff-418">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="be4ff-419">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) 會要求應用程式終止。</span><span class="sxs-lookup"><span data-stu-id="be4ff-419">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="be4ff-420">當呼叫類別的 `Shutdown` 方法時，下列類別使用 `StopApplication` 來順利關閉應用程式：</span><span class="sxs-lookup"><span data-stu-id="be4ff-420">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="be4ff-421">範圍驗證</span><span class="sxs-lookup"><span data-stu-id="be4ff-421">Scope validation</span></span>

<span data-ttu-id="be4ff-422">在 ASP.NET Core 2.0 或更新版本，如果應用程式的環境是「開發」，[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 會將 [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) 設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="be4ff-422">In ASP.NET Core 2.0 or later, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="be4ff-423">當 `ValidateScopes` 設定為 `true` 時，預設服務提供者會執行檢查以確認：</span><span class="sxs-lookup"><span data-stu-id="be4ff-423">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="be4ff-424">範圍服務不是直接或間接由根服務提供者解析。</span><span class="sxs-lookup"><span data-stu-id="be4ff-424">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="be4ff-425">範圍服務不是直接或間接插入至單一服務。</span><span class="sxs-lookup"><span data-stu-id="be4ff-425">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="be4ff-426">根服務提供者會在呼叫 [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) 時建立。</span><span class="sxs-lookup"><span data-stu-id="be4ff-426">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="be4ff-427">當提供者啟動應用程式時，根服務提供者的存留期與應用程式/伺服器的存留期一致，並會在應用程式關閉時處置。</span><span class="sxs-lookup"><span data-stu-id="be4ff-427">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="be4ff-428">範圍服務會由建立這些服務的容器處置。</span><span class="sxs-lookup"><span data-stu-id="be4ff-428">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="be4ff-429">若是在根容器中建立範圍服務，因為當應用程式/伺服器關機時，服務只會由根容器處理，所以服務的存留期會提升為單一服務等級。</span><span class="sxs-lookup"><span data-stu-id="be4ff-429">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="be4ff-430">當呼叫 `BuildServiceProvider` 時，驗證服務範圍會攔截到這些情況。</span><span class="sxs-lookup"><span data-stu-id="be4ff-430">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="be4ff-431">若要一律驗證範圍，包括在實際執行環境中，請使用主機產生器上的 [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) 來設定 [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions)：</span><span class="sxs-lookup"><span data-stu-id="be4ff-431">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="be4ff-432">針對 System.ArgumentException 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="be4ff-432">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="be4ff-433">**只適用於 ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="be4ff-433">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="be4ff-434">可以藉由將 `IStartup` 直接插入至相依性插入容器來建置主機，而不是呼叫 `UseStartup` 或 `Configure`：</span><span class="sxs-lookup"><span data-stu-id="be4ff-434">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="be4ff-435">如果主機以此方式建置，可能會發生下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="be4ff-435">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="be4ff-436">發生此情況是因為需要 [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (目前的組件)，才能掃描 `HostingStartupAttributes`。</span><span class="sxs-lookup"><span data-stu-id="be4ff-436">This occurs because the [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="be4ff-437">如果應用程式以手動方式將 `IStartup` 插入至相依性插入容器，請將下列呼叫新增至 `WebHostBuilder`，並指定組件名稱：</span><span class="sxs-lookup"><span data-stu-id="be4ff-437">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="be4ff-438">或者，新增假的 `Configure` 至 `WebHostBuilder`，其自動設定 `applicationName` (`ApplicationKey`)：</span><span class="sxs-lookup"><span data-stu-id="be4ff-438">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="be4ff-439">**注意**：只有 ASP.NET Core 2.0 版需要這麼做，且只有在應用程式不會呼叫 `UseStartup` 或 `Configure` 時才需要。</span><span class="sxs-lookup"><span data-stu-id="be4ff-439">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="be4ff-440">如需詳細資訊，請參閱 [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) (公告：已移除 Microsoft.Extensions.PlatformAbstractions (註解)) 和 [StartupInjection 範例](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs)。</span><span class="sxs-lookup"><span data-stu-id="be4ff-440">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="be4ff-441">其他資源</span><span class="sxs-lookup"><span data-stu-id="be4ff-441">Additional resources</span></span>

* [<span data-ttu-id="be4ff-442"> Windows 上使用 IIS 的主機</span><span class="sxs-lookup"><span data-stu-id="be4ff-442">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="be4ff-443">Linux 上使用 Nginx 的主機</span><span class="sxs-lookup"><span data-stu-id="be4ff-443">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="be4ff-444">Linux 上使用 Apache 的主機</span><span class="sxs-lookup"><span data-stu-id="be4ff-444">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="be4ff-445">Windows 服務中的主機</span><span class="sxs-lookup"><span data-stu-id="be4ff-445">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
