---
title: ASP.NET Core Web 主機
author: guardrex
description: 深入了解 ASP.NET Core 中的 Web 主機，其負責啟動應用程式以及管理存留期。
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: 8b72376ae4cb608c4df0cf516288188cff862b36
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894240"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="543fb-103">ASP.NET Core Web 主機</span><span class="sxs-lookup"><span data-stu-id="543fb-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="543fb-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="543fb-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="543fb-105">ASP.NET Core 應用程式會設定並啟動「主機」。</span><span class="sxs-lookup"><span data-stu-id="543fb-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="543fb-106">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="543fb-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="543fb-107">至少，主機會設定伺服器和要求處理管線。</span><span class="sxs-lookup"><span data-stu-id="543fb-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="543fb-108">本主題涵蓋 ASP.NET Core Web 主機 ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder))，這對裝載 Web 應用程式很實用。</span><span class="sxs-lookup"><span data-stu-id="543fb-108">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="543fb-109">如需 .NET 泛型主機 ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)) 的說明，請參閱 <xref:fundamentals/host/generic-host>。</span><span class="sxs-lookup"><span data-stu-id="543fb-109">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="543fb-110">設定主機</span><span class="sxs-lookup"><span data-stu-id="543fb-110">Set up a host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="543fb-111">使用 ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)) 的執行個體建立主機。</span><span class="sxs-lookup"><span data-stu-id="543fb-111">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="543fb-112">這通常在應用程式的進入點執行，也就是 `Main` 方法。</span><span class="sxs-lookup"><span data-stu-id="543fb-112">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="543fb-113">在專案範本中，`Main` 位於 *Program.cs*。</span><span class="sxs-lookup"><span data-stu-id="543fb-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="543fb-114">一般 *Program.cs* 會呼叫 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 以開始設定主機：</span><span class="sxs-lookup"><span data-stu-id="543fb-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="543fb-115">`CreateDefaultBuilder` 會執行下列工作：</span><span class="sxs-lookup"><span data-stu-id="543fb-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="543fb-116">將 [Kestrel](xref:fundamentals/servers/kestrel) 設定為網頁伺服器，並使用應用程式主機組態提供者設定伺服器。</span><span class="sxs-lookup"><span data-stu-id="543fb-116">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="543fb-117">如需 Kestrel 預設選項，請參閱 <xref:fundamentals/servers/kestrel#kestrel-options>。</span><span class="sxs-lookup"><span data-stu-id="543fb-117">For the Kestrel default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="543fb-118">設定 [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) 所傳回路徑的內容根目錄。</span><span class="sxs-lookup"><span data-stu-id="543fb-118">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="543fb-119">從下列項目載入[主機組態](#host-configuration-values)：</span><span class="sxs-lookup"><span data-stu-id="543fb-119">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="543fb-120">前面加上 `ASPNETCORE_` 的環境變數 (例如，`ASPNETCORE_ENVIRONMENT`)。</span><span class="sxs-lookup"><span data-stu-id="543fb-120">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="543fb-121">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="543fb-121">Command-line arguments.</span></span>
* <span data-ttu-id="543fb-122">從下列項目載入應用程式組態：</span><span class="sxs-lookup"><span data-stu-id="543fb-122">Loads app configuration from:</span></span>
  * <span data-ttu-id="543fb-123">*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="543fb-123">*appsettings.json*.</span></span>
  * <span data-ttu-id="543fb-124">*appsettings.{Environment}.json*</span><span class="sxs-lookup"><span data-stu-id="543fb-124">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="543fb-125">應用程式在使用輸入組件的 `Development` 環境中執行時的[使用者密碼](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="543fb-125">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="543fb-126">環境變數。</span><span class="sxs-lookup"><span data-stu-id="543fb-126">Environment variables.</span></span>
  * <span data-ttu-id="543fb-127">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="543fb-127">Command-line arguments.</span></span>
* <span data-ttu-id="543fb-128">設定主控台和偵錯輸出的[記錄](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="543fb-128">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="543fb-129">記錄包含 *appsettings.json* 或 *appsettings.{Environment}.json* 檔案的記錄組態區段中指定的[記錄檔篩選](xref:fundamentals/logging/index#log-filtering)規則。</span><span class="sxs-lookup"><span data-stu-id="543fb-129">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="543fb-130">在 IIS 背後執行時，啟用 [IIS 整合](xref:host-and-deploy/iis/index)。</span><span class="sxs-lookup"><span data-stu-id="543fb-130">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="543fb-131">設定使用 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)時伺服器接聽的基底路徑和連接埠。</span><span class="sxs-lookup"><span data-stu-id="543fb-131">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="543fb-132">此模組會在 IIS 和 Kestrel 之間建立反向 Proxy。</span><span class="sxs-lookup"><span data-stu-id="543fb-132">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="543fb-133">也會設定應用程式以[擷取啟動錯誤](#capture-startup-errors)。</span><span class="sxs-lookup"><span data-stu-id="543fb-133">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="543fb-134">如需 IIS 預設選項，請參閱 <xref:host-and-deploy/iis/index#iis-options>。</span><span class="sxs-lookup"><span data-stu-id="543fb-134">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="543fb-135">如果應用程式的環境是「開發」，請將 [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) 設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="543fb-135">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="543fb-136">如需詳細資訊，請參閱[範圍驗證](#scope-validation)。</span><span class="sxs-lookup"><span data-stu-id="543fb-136">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="543fb-137">`CreateDefaultBuilder` 所定義的組態可以透過 [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration)、[ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging)，以及 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) 的其他方法和擴充方法加以覆寫及擴增。</span><span class="sxs-lookup"><span data-stu-id="543fb-137">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="543fb-138">數個範例如下：</span><span class="sxs-lookup"><span data-stu-id="543fb-138">A few examples follow:</span></span>

* <span data-ttu-id="543fb-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) 用來指定應用程式的其他 `IConfiguration`。</span><span class="sxs-lookup"><span data-stu-id="543fb-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="543fb-140">下列 `ConfigureAppConfiguration` 呼叫會新增委派，以在 *appsettings.xml* 檔案中包含應用程式組態。</span><span class="sxs-lookup"><span data-stu-id="543fb-140">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="543fb-141">可能會多次呼叫 `ConfigureAppConfiguration`。</span><span class="sxs-lookup"><span data-stu-id="543fb-141">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="543fb-142">請注意，此組態不適用於主機 (例如，伺服器 URL 或環境)。</span><span class="sxs-lookup"><span data-stu-id="543fb-142">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="543fb-143">請參閱[主機組態值](#host-configuration-values)一節。</span><span class="sxs-lookup"><span data-stu-id="543fb-143">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="543fb-144">下列 `ConfigureLogging` 呼叫會新增委派，將最低的記錄層級 ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) 設定為 [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel)。</span><span class="sxs-lookup"><span data-stu-id="543fb-144">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="543fb-145">此設定會覆寫 `CreateDefaultBuilder` 所設定之 *ppsettings.Development.json* (`LogLevel.Debug`) 和 *appsettings.Production.json* (`LogLevel.Error`) 中的設定。</span><span class="sxs-lookup"><span data-stu-id="543fb-145">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="543fb-146">可能會多次呼叫 `ConfigureLogging`。</span><span class="sxs-lookup"><span data-stu-id="543fb-146">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

* <span data-ttu-id="543fb-147">下列 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) 呼叫會覆寫當 `CreateDefaultBuilder` 設定 Kestrel 時建立的預設 [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) 30,000,000 位元組：</span><span class="sxs-lookup"><span data-stu-id="543fb-147">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

<span data-ttu-id="543fb-148">「內容根目錄」會決定主機搜尋內容檔案 (例如 MVC 檢視檔案) 的位置。</span><span class="sxs-lookup"><span data-stu-id="543fb-148">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="543fb-149">從專案的根資料夾啟動應用程式時，會使用專案的根資料夾作為內容根目錄。</span><span class="sxs-lookup"><span data-stu-id="543fb-149">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="543fb-150">這是 [Visual Studio](https://www.visualstudio.com/) 和 [dotnet 新範本](/dotnet/core/tools/dotnet-new)中使用的預設值。</span><span class="sxs-lookup"><span data-stu-id="543fb-150">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="543fb-151">如需應用程式組態的詳細資訊，請參閱 <xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="543fb-151">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="543fb-152">作為使用靜態 `CreateDefaultBuilder` 方法的替代做法，ASP.NET Core 2.x 支援從 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 建立主機的方法。</span><span class="sxs-lookup"><span data-stu-id="543fb-152">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="543fb-153">如需詳細資訊，請參閱 ASP.NET Core 1.x 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="543fb-153">For more information, see the ASP.NET Core 1.x tab.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="543fb-154">使用 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 的執行個體建立主機。</span><span class="sxs-lookup"><span data-stu-id="543fb-154">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="543fb-155">建立主機通常在應用程式的進入點執行，也就是 `Main` 方法。</span><span class="sxs-lookup"><span data-stu-id="543fb-155">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="543fb-156">在專案範本中，`Main` 位於 *Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="543fb-156">In the project templates, `Main` is located in *Program.cs*:</span></span>

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

<span data-ttu-id="543fb-157">`WebHostBuilder` 需要[實作 IServer 的伺服器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="543fb-157">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="543fb-158">內建的伺服器是 [Kestrel](xref:fundamentals/servers/kestrel) 和 [HTTP.sys](xref:fundamentals/servers/httpsys) (在 ASP.NET Core 2.0 版之前，HTTP.sys 稱為 [WebListener](xref:fundamentals/servers/weblistener))。</span><span class="sxs-lookup"><span data-stu-id="543fb-158">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="543fb-159">在此範例中，[UseKestrel 擴充方法](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)會指定 Kestrel 伺服器。</span><span class="sxs-lookup"><span data-stu-id="543fb-159">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="543fb-160">「內容根目錄」會決定主機搜尋內容檔案 (例如 MVC 檢視檔案) 的位置。</span><span class="sxs-lookup"><span data-stu-id="543fb-160">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="543fb-161">[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1) 會取得 `UseContentRoot` 的預設內容根目錄。</span><span class="sxs-lookup"><span data-stu-id="543fb-161">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="543fb-162">從專案的根資料夾啟動應用程式時，會使用專案的根資料夾作為內容根目錄。</span><span class="sxs-lookup"><span data-stu-id="543fb-162">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="543fb-163">這是 [Visual Studio](https://www.visualstudio.com/) 和 [dotnet 新範本](/dotnet/core/tools/dotnet-new)中使用的預設值。</span><span class="sxs-lookup"><span data-stu-id="543fb-163">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="543fb-164">若要使用 IIS 作為反向 Proxy，請在建置主機時呼叫 [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions)。</span><span class="sxs-lookup"><span data-stu-id="543fb-164">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="543fb-165">`UseIISIntegration` 不會像 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)那樣設定「伺服器」。</span><span class="sxs-lookup"><span data-stu-id="543fb-165">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="543fb-166">`UseIISIntegration` 會設定使用 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)以在 Kestrel 和 IIS 之間建立反向 Proxy 時，伺服器接聽的基底路徑和連接埠。</span><span class="sxs-lookup"><span data-stu-id="543fb-166">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="543fb-167">若要搭配使用 IIS 與 ASP.NET Core，必須指定 `UseKestrel` 和 `UseIISIntegration`。</span><span class="sxs-lookup"><span data-stu-id="543fb-167">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="543fb-168">`UseIISIntegration` 只會在 IIS 或 IIS Express 背後執行時啟動。</span><span class="sxs-lookup"><span data-stu-id="543fb-168">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="543fb-169">如需詳細資訊，請參閱 <xref:fundamentals/servers/aspnet-core-module> 與 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="543fb-169">For more information, see <xref:fundamentals/servers/aspnet-core-module> and <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="543fb-170">設定主機 (和 ASP.NET Core 應用程式) 的最小實作，包含指定伺服器和應用程式要求管線的組態：</span><span class="sxs-lookup"><span data-stu-id="543fb-170">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

::: moniker-end

<span data-ttu-id="543fb-171">設定主機時，可以提供 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) 和 [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) 方法。</span><span class="sxs-lookup"><span data-stu-id="543fb-171">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="543fb-172">如果指定 `Startup` 類別，它必須定義 `Configure` 方法。</span><span class="sxs-lookup"><span data-stu-id="543fb-172">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="543fb-173">如需詳細資訊，請參閱<xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="543fb-173">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="543fb-174">多次呼叫 `ConfigureServices` 會彼此附加。</span><span class="sxs-lookup"><span data-stu-id="543fb-174">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="543fb-175">對 `WebHostBuilder` 多次呼叫 `Configure` 或 `UseStartup` 則會取代先前的設定。</span><span class="sxs-lookup"><span data-stu-id="543fb-175">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="543fb-176">主機組態值</span><span class="sxs-lookup"><span data-stu-id="543fb-176">Host configuration values</span></span>

<span data-ttu-id="543fb-177">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 依賴下列方法來設定主機組態值：</span><span class="sxs-lookup"><span data-stu-id="543fb-177">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="543fb-178">主機建立器組態，其中包含 `ASPNETCORE_{configurationKey}` 格式的環境變數。</span><span class="sxs-lookup"><span data-stu-id="543fb-178">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="543fb-179">例如，`ASPNETCORE_ENVIRONMENT`。</span><span class="sxs-lookup"><span data-stu-id="543fb-179">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="543fb-180">[UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) 和 [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 之類的延伸模組 (請參閱[覆寫組態](#override-configuration)一節)。</span><span class="sxs-lookup"><span data-stu-id="543fb-180">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="543fb-181">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) 和相關聯的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="543fb-181">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="543fb-182">使用 `UseSetting` 設定值時，此值會設定為字串，而不論其類型為何。</span><span class="sxs-lookup"><span data-stu-id="543fb-182">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="543fb-183">主機使用設定最後一個值的任何選項。</span><span class="sxs-lookup"><span data-stu-id="543fb-183">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="543fb-184">如需詳細資訊，請參閱下一節的[覆寫組態](#override-configuration)。</span><span class="sxs-lookup"><span data-stu-id="543fb-184">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="543fb-185">應用程式索引鍵 (名稱)</span><span class="sxs-lookup"><span data-stu-id="543fb-185">Application Key (Name)</span></span>

<span data-ttu-id="543fb-186">在主機建構期間呼叫 [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) 或 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) 時，會自動設定 [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) 屬性。</span><span class="sxs-lookup"><span data-stu-id="543fb-186">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="543fb-187">該值會設定為包含應用程式進入點的組件名稱。</span><span class="sxs-lookup"><span data-stu-id="543fb-187">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="543fb-188">若要明確設定該值，請使用 [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey)：</span><span class="sxs-lookup"><span data-stu-id="543fb-188">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="543fb-189">**索引鍵**：applicationName</span><span class="sxs-lookup"><span data-stu-id="543fb-189">**Key**: applicationName</span></span>  
<span data-ttu-id="543fb-190">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="543fb-190">**Type**: *string*</span></span>  
<span data-ttu-id="543fb-191">**預設**：包含應用程式進入點的組件名稱。</span><span class="sxs-lookup"><span data-stu-id="543fb-191">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="543fb-192">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="543fb-192">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="543fb-193">**環境變數**：`ASPNETCORE_APPLICATIONKEY`</span><span class="sxs-lookup"><span data-stu-id="543fb-193">**Environment variable**: `ASPNETCORE_APPLICATIONKEY`</span></span>

::: moniker range=">= aspnetcore-2.1"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```csharp
var host = new WebHostBuilder()
    .UseSetting("applicationName", "CustomApplicationName")
```

::: moniker-end

### <a name="capture-startup-errors"></a><span data-ttu-id="543fb-194">擷取啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="543fb-194">Capture Startup Errors</span></span>

<span data-ttu-id="543fb-195">此設定會控制啟動錯誤的擷取。</span><span class="sxs-lookup"><span data-stu-id="543fb-195">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="543fb-196">**索引鍵**：captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="543fb-196">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="543fb-197">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="543fb-197">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="543fb-198">**預設值**：預設為 `false`，除非應用程式執行時在 IIS 背後有 Kestrel，此時預設值即為 `true`。</span><span class="sxs-lookup"><span data-stu-id="543fb-198">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="543fb-199">**設定使用**：`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="543fb-199">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="543fb-200">**環境變數**：`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="543fb-200">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="543fb-201">當它為 `false` 時，啟動期間發生的錯誤會導致主機結束。</span><span class="sxs-lookup"><span data-stu-id="543fb-201">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="543fb-202">當它為 `true` 時，主機會擷取啟動期間的例外狀況，並嘗試啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="543fb-202">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

::: moniker-end

### <a name="content-root"></a><span data-ttu-id="543fb-203">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="543fb-203">Content Root</span></span>

<span data-ttu-id="543fb-204">此設定可決定 ASP.NET Core 開始搜尋內容檔案 (例如 MVC 檢視) 的位置。</span><span class="sxs-lookup"><span data-stu-id="543fb-204">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="543fb-205">**索引鍵**：contentRoot</span><span class="sxs-lookup"><span data-stu-id="543fb-205">**Key**: contentRoot</span></span>  
<span data-ttu-id="543fb-206">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="543fb-206">**Type**: *string*</span></span>  
<span data-ttu-id="543fb-207">**預設值**：預設為應用程式組件所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="543fb-207">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="543fb-208">**設定使用**：`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="543fb-208">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="543fb-209">**環境變數**：`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="543fb-209">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="543fb-210">內容根目錄也作為 [Web 根目錄設定](#web-root)的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="543fb-210">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="543fb-211">如果路徑不存在，就無法啟動主機。</span><span class="sxs-lookup"><span data-stu-id="543fb-211">If the path doesn't exist, the host fails to start.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

### <a name="detailed-errors"></a><span data-ttu-id="543fb-212">詳細錯誤</span><span class="sxs-lookup"><span data-stu-id="543fb-212">Detailed Errors</span></span>

<span data-ttu-id="543fb-213">決定是否應該擷取詳細的錯誤。</span><span class="sxs-lookup"><span data-stu-id="543fb-213">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="543fb-214">**索引鍵**：detailedErrors</span><span class="sxs-lookup"><span data-stu-id="543fb-214">**Key**: detailedErrors</span></span>  
<span data-ttu-id="543fb-215">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="543fb-215">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="543fb-216">**預設值**：false</span><span class="sxs-lookup"><span data-stu-id="543fb-216">**Default**: false</span></span>  
<span data-ttu-id="543fb-217">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="543fb-217">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="543fb-218">**環境變數**：`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="543fb-218">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="543fb-219">啟用時 (或當 <a href="#environment">Environment</a> 設定為 `Development` 時)，應用程式會擷取詳細例外狀況。</span><span class="sxs-lookup"><span data-stu-id="543fb-219">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

### <a name="environment"></a><span data-ttu-id="543fb-220">環境</span><span class="sxs-lookup"><span data-stu-id="543fb-220">Environment</span></span>

<span data-ttu-id="543fb-221">設定應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="543fb-221">Sets the app's environment.</span></span>

<span data-ttu-id="543fb-222">**索引鍵**：environment</span><span class="sxs-lookup"><span data-stu-id="543fb-222">**Key**: environment</span></span>  
<span data-ttu-id="543fb-223">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="543fb-223">**Type**: *string*</span></span>  
<span data-ttu-id="543fb-224">**預設值**：Production</span><span class="sxs-lookup"><span data-stu-id="543fb-224">**Default**: Production</span></span>  
<span data-ttu-id="543fb-225">**設定使用**：`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="543fb-225">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="543fb-226">**環境變數**：`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="543fb-226">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="543fb-227">環境可以設定為任何值。</span><span class="sxs-lookup"><span data-stu-id="543fb-227">The environment can be set to any value.</span></span> <span data-ttu-id="543fb-228">架構定義的值包括 `Development`、`Staging` 和 `Production`。</span><span class="sxs-lookup"><span data-stu-id="543fb-228">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="543fb-229">值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="543fb-229">Values aren't case sensitive.</span></span> <span data-ttu-id="543fb-230">根據預設，*Environment* 是從 `ASPNETCORE_ENVIRONMENT` 環境變數讀取。</span><span class="sxs-lookup"><span data-stu-id="543fb-230">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="543fb-231">使用 [Visual Studio](https://www.visualstudio.com/) 時，可能會在 *launchSettings.json* 檔案設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="543fb-231">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="543fb-232">如需詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="543fb-232">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="543fb-233">裝載啟動組件</span><span class="sxs-lookup"><span data-stu-id="543fb-233">Hosting Startup Assemblies</span></span>

<span data-ttu-id="543fb-234">設定應用程式的裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="543fb-234">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="543fb-235">**索引鍵**：hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="543fb-235">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="543fb-236">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="543fb-236">**Type**: *string*</span></span>  
<span data-ttu-id="543fb-237">**預設值**：空字串</span><span class="sxs-lookup"><span data-stu-id="543fb-237">**Default**: Empty string</span></span>  
<span data-ttu-id="543fb-238">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="543fb-238">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="543fb-239">**環境變數**：`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="543fb-239">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="543fb-240">在啟動時載入的裝載啟動組件字串，以分號分隔。</span><span class="sxs-lookup"><span data-stu-id="543fb-240">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="543fb-241">雖然設定值會預設為空字串，但裝載啟動組件一律會包含應用程式的組件。</span><span class="sxs-lookup"><span data-stu-id="543fb-241">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="543fb-242">提供裝載啟動組件時，它們會新增至應用程式的組件，以便在應用程式在啟動時建置其通用服務時載入。</span><span class="sxs-lookup"><span data-stu-id="543fb-242">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prefer-hosting-urls"></a><span data-ttu-id="543fb-243">偏好裝載 URL</span><span class="sxs-lookup"><span data-stu-id="543fb-243">Prefer Hosting URLs</span></span>

<span data-ttu-id="543fb-244">表示主機是否應接聽使用 `WebHostBuilder` 設定的 URL，而不是 `IServer` 實作所設定的 URL。</span><span class="sxs-lookup"><span data-stu-id="543fb-244">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="543fb-245">**索引鍵**：preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="543fb-245">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="543fb-246">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="543fb-246">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="543fb-247">**預設值**：true</span><span class="sxs-lookup"><span data-stu-id="543fb-247">**Default**: true</span></span>  
<span data-ttu-id="543fb-248">**設定使用**：`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="543fb-248">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="543fb-249">**環境變數**：`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="543fb-249">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prevent-hosting-startup"></a><span data-ttu-id="543fb-250">防止裝載啟動</span><span class="sxs-lookup"><span data-stu-id="543fb-250">Prevent Hosting Startup</span></span>

<span data-ttu-id="543fb-251">可防止自動載入裝載啟動組件，包括應用程式組件所設定的裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="543fb-251">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="543fb-252">如需詳細資訊，請參閱<xref:fundamentals/configuration/platform-specific-configuration>。</span><span class="sxs-lookup"><span data-stu-id="543fb-252">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="543fb-253">**索引鍵**preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="543fb-253">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="543fb-254">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="543fb-254">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="543fb-255">**預設值**：false</span><span class="sxs-lookup"><span data-stu-id="543fb-255">**Default**: false</span></span>  
<span data-ttu-id="543fb-256">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="543fb-256">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="543fb-257">**環境變數**：`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="543fb-257">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

::: moniker-end

### <a name="server-urls"></a><span data-ttu-id="543fb-258">伺服器 URL</span><span class="sxs-lookup"><span data-stu-id="543fb-258">Server URLs</span></span>

<span data-ttu-id="543fb-259">表示伺服器應該接聽要求的 IP 位址或主機位址，以及連接埠和通訊協定。</span><span class="sxs-lookup"><span data-stu-id="543fb-259">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="543fb-260">**索引鍵**：urls</span><span class="sxs-lookup"><span data-stu-id="543fb-260">**Key**: urls</span></span>  
<span data-ttu-id="543fb-261">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="543fb-261">**Type**: *string*</span></span>  
<span data-ttu-id="543fb-262">**預設值**：http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="543fb-262">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="543fb-263">**設定使用**：`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="543fb-263">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="543fb-264">**環境變數**：`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="543fb-264">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="543fb-265">設定為伺服器應該回應的 URL 前置清單，並以分號 (;) 分隔。</span><span class="sxs-lookup"><span data-stu-id="543fb-265">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="543fb-266">例如，`http://localhost:123`。</span><span class="sxs-lookup"><span data-stu-id="543fb-266">For example, `http://localhost:123`.</span></span> <span data-ttu-id="543fb-267">使用 "\*"，表示伺服器應接聽任何 IP 位址或主機名稱上的要求，並使用指定的連接埠和通訊協定 (例如，`http://*:5000`)。</span><span class="sxs-lookup"><span data-stu-id="543fb-267">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="543fb-268">通訊協定 (`http://` 或 `https://`) 必須包含在每個 URL 中。</span><span class="sxs-lookup"><span data-stu-id="543fb-268">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="543fb-269">伺服器之間支援的格式有所不同。</span><span class="sxs-lookup"><span data-stu-id="543fb-269">Supported formats vary between servers.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="543fb-270">Kestrel 有它自己的端點設定 API。</span><span class="sxs-lookup"><span data-stu-id="543fb-270">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="543fb-271">如需詳細資訊，請參閱<xref:fundamentals/servers/kestrel#endpoint-configuration>。</span><span class="sxs-lookup"><span data-stu-id="543fb-271">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="shutdown-timeout"></a><span data-ttu-id="543fb-272">關機逾時</span><span class="sxs-lookup"><span data-stu-id="543fb-272">Shutdown Timeout</span></span>

<span data-ttu-id="543fb-273">指定等待 Web 主機關機的時間長度。</span><span class="sxs-lookup"><span data-stu-id="543fb-273">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="543fb-274">**索引鍵**：shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="543fb-274">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="543fb-275">**類型**：*int*</span><span class="sxs-lookup"><span data-stu-id="543fb-275">**Type**: *int*</span></span>  
<span data-ttu-id="543fb-276">**預設值**：5</span><span class="sxs-lookup"><span data-stu-id="543fb-276">**Default**: 5</span></span>  
<span data-ttu-id="543fb-277">**設定使用**：`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="543fb-277">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="543fb-278">**環境變數**：`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="543fb-278">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="543fb-279">雖然索引鍵接受 *int* 與 `UseSetting` (例如 `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`)，[UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) 擴充方法會採用 [TimeSpan](/dotnet/api/system.timespan)。</span><span class="sxs-lookup"><span data-stu-id="543fb-279">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="543fb-280">在逾時期間，裝載會：</span><span class="sxs-lookup"><span data-stu-id="543fb-280">During the timeout period, hosting:</span></span>

* <span data-ttu-id="543fb-281">觸發 [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping)。</span><span class="sxs-lookup"><span data-stu-id="543fb-281">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="543fb-282">嘗試停止託管的服務，並記錄無法停止之服務的任何錯誤。</span><span class="sxs-lookup"><span data-stu-id="543fb-282">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="543fb-283">如果在所有的託管服務停止之前逾時期限已到期，則應用程式關閉時，會停止任何剩餘的作用中服務。</span><span class="sxs-lookup"><span data-stu-id="543fb-283">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="543fb-284">即使服務尚未完成處理也會停止。</span><span class="sxs-lookup"><span data-stu-id="543fb-284">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="543fb-285">如果服務需要更多時間才能停止，請增加逾時。</span><span class="sxs-lookup"><span data-stu-id="543fb-285">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

::: moniker-end

### <a name="startup-assembly"></a><span data-ttu-id="543fb-286">啟動組件</span><span class="sxs-lookup"><span data-stu-id="543fb-286">Startup Assembly</span></span>

<span data-ttu-id="543fb-287">決定要搜尋 `Startup` 類別的組件。</span><span class="sxs-lookup"><span data-stu-id="543fb-287">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="543fb-288">**索引鍵**：startupAssembly</span><span class="sxs-lookup"><span data-stu-id="543fb-288">**Key**: startupAssembly</span></span>  
<span data-ttu-id="543fb-289">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="543fb-289">**Type**: *string*</span></span>  
<span data-ttu-id="543fb-290">**預設值**：應用程式的組件</span><span class="sxs-lookup"><span data-stu-id="543fb-290">**Default**: The app's assembly</span></span>  
<span data-ttu-id="543fb-291">**設定使用**：`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="543fb-291">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="543fb-292">**環境變數**：`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="543fb-292">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="543fb-293">可以參考依名稱 (`string`) 或類型 (`TStartup`) 的組件。</span><span class="sxs-lookup"><span data-stu-id="543fb-293">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="543fb-294">如果呼叫多個 `UseStartup` 方法，最後一個將會優先。</span><span class="sxs-lookup"><span data-stu-id="543fb-294">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

::: moniker-end

### <a name="web-root"></a><span data-ttu-id="543fb-295">Web 根目錄</span><span class="sxs-lookup"><span data-stu-id="543fb-295">Web Root</span></span>

<span data-ttu-id="543fb-296">設定應用程式靜態資產的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="543fb-296">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="543fb-297">**索引鍵**：webroot</span><span class="sxs-lookup"><span data-stu-id="543fb-297">**Key**: webroot</span></span>  
<span data-ttu-id="543fb-298">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="543fb-298">**Type**: *string*</span></span>  
<span data-ttu-id="543fb-299">**預設值**：如果未指定，則預設值為 "(Content Root)/wwwroot"，如果該路徑存在的話。</span><span class="sxs-lookup"><span data-stu-id="543fb-299">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="543fb-300">如果路徑不存在，則會使用無作業檔案提供者。</span><span class="sxs-lookup"><span data-stu-id="543fb-300">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="543fb-301">**設定使用**：`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="543fb-301">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="543fb-302">**環境變數**：`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="543fb-302">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

::: moniker-end

## <a name="override-configuration"></a><span data-ttu-id="543fb-303">覆寫組態</span><span class="sxs-lookup"><span data-stu-id="543fb-303">Override configuration</span></span>

<span data-ttu-id="543fb-304">使用 [Configuration](xref:fundamentals/configuration/index) 來設定 Web 主機。</span><span class="sxs-lookup"><span data-stu-id="543fb-304">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="543fb-305">在下列範例中，主機組態選擇性指定於 *hostsettings.json* 檔案中。</span><span class="sxs-lookup"><span data-stu-id="543fb-305">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="543fb-306">從 *hostsettings.json* 檔案載入的任何組態，可以用命令列引數加以覆寫。</span><span class="sxs-lookup"><span data-stu-id="543fb-306">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="543fb-307">建置的組態 (在 `config` 中) 用以使用 [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 來設定主機。</span><span class="sxs-lookup"><span data-stu-id="543fb-307">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="543fb-308">`IWebHostBuilder` 設定會新增到應用程式的組態，但反之則不然 &mdash;`ConfigureAppConfiguration` 並不會影響 `IWebHostBuilder` 組態。</span><span class="sxs-lookup"><span data-stu-id="543fb-308">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="543fb-309">首先使用 *hostsettings.json* 組態來覆寫 `UseUrls` 所提供的組態，其次是命令列引數組態：</span><span class="sxs-lookup"><span data-stu-id="543fb-309">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="543fb-310">*hostsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="543fb-310">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="543fb-311">首先使用 *hostsettings.json* 組態來覆寫 `UseUrls` 所提供的組態，其次是命令列引數組態：</span><span class="sxs-lookup"><span data-stu-id="543fb-311">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="543fb-312">*hostsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="543fb-312">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="543fb-313">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 擴充方法目前無法剖析 `GetSection` 傳回的組態區段 (例如 `.UseConfiguration(Configuration.GetSection("section"))`。</span><span class="sxs-lookup"><span data-stu-id="543fb-313">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="543fb-314">`GetSection` 方法會將設定索引鍵篩選到要求的區段，但保留索引鍵上的區段名稱 (例如 `section:urls`、`section:environment`)。</span><span class="sxs-lookup"><span data-stu-id="543fb-314">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="543fb-315">`UseConfiguration` 方法預期索引鍵要符合 `WebHostBuilder` 索引鍵 (例如 `urls`、`environment`)。</span><span class="sxs-lookup"><span data-stu-id="543fb-315">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="543fb-316">索引鍵上的區段名稱存在可避免區段的值設定主機。</span><span class="sxs-lookup"><span data-stu-id="543fb-316">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="543fb-317">這個問題將在近期的版本中解決。</span><span class="sxs-lookup"><span data-stu-id="543fb-317">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="543fb-318">如需詳細資訊和因應措施，請參閱 [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (將設定區段傳遞到 WebHostBuilder.UseConfiguration 會使用完整索引鍵)。</span><span class="sxs-lookup"><span data-stu-id="543fb-318">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="543fb-319">`UseConfiguration` 只會將索引鍵從提供的 `IConfiguration` 複製到主機建立器的組態。</span><span class="sxs-lookup"><span data-stu-id="543fb-319">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="543fb-320">因此，為 JSON、INI 和 XML 設定檔設定 `reloadOnChange: true` 沒有任何作用。</span><span class="sxs-lookup"><span data-stu-id="543fb-320">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="543fb-321">若要指定主機在特定 URL 上執行，所要的值可以在執行 [dotnet run](/dotnet/core/tools/dotnet-run) 時從命令提示字元傳入。</span><span class="sxs-lookup"><span data-stu-id="543fb-321">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="543fb-322">命令列引數會覆寫 *hostsettings.json* 檔案的 `urls` 值，且伺服器會接聽連接埠 8080：</span><span class="sxs-lookup"><span data-stu-id="543fb-322">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="543fb-323">管理主機</span><span class="sxs-lookup"><span data-stu-id="543fb-323">Manage the host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="543fb-324">**執行**</span><span class="sxs-lookup"><span data-stu-id="543fb-324">**Run**</span></span>

<span data-ttu-id="543fb-325">`Run` 方法會啟動 Web 應用程式，並且封鎖呼叫執行緒，直到主機關閉為止：</span><span class="sxs-lookup"><span data-stu-id="543fb-325">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="543fb-326">**Start**</span><span class="sxs-lookup"><span data-stu-id="543fb-326">**Start**</span></span>

<span data-ttu-id="543fb-327">藉由呼叫 `Start` 方法，以非封鎖方式執行主機：</span><span class="sxs-lookup"><span data-stu-id="543fb-327">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="543fb-328">如果 URL 的清單傳遞至 `Start` 方法，它會接聽指定的 URL：</span><span class="sxs-lookup"><span data-stu-id="543fb-328">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="543fb-329">應用程式可以使用預先設定的 `CreateDefaultBuilder` 預設值，使用便利的靜態方法初始化並啟動新的主機。</span><span class="sxs-lookup"><span data-stu-id="543fb-329">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="543fb-330">這些方法會啟動伺服器而無主控台輸出，且在使用 [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) 時等候中斷 (Ctrl-C/SIGINT 或 SIGTERM)：</span><span class="sxs-lookup"><span data-stu-id="543fb-330">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="543fb-331">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="543fb-331">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="543fb-332">使用 `RequestDelegate` 啟動：</span><span class="sxs-lookup"><span data-stu-id="543fb-332">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="543fb-333">在瀏覽器中提出要求給 `http://localhost:5000`，以收到 "Hello World!" 回應</span><span class="sxs-lookup"><span data-stu-id="543fb-333">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="543fb-334">`WaitForShutdown` 會封鎖，直到發出中斷 (Ctrl-C/SIGINT 或 SIGTERM)。</span><span class="sxs-lookup"><span data-stu-id="543fb-334">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="543fb-335">應用程式會顯示 `Console.WriteLine` 訊息並等候按鍵動作以便結束。</span><span class="sxs-lookup"><span data-stu-id="543fb-335">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="543fb-336">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="543fb-336">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="543fb-337">使用 URL 和 `RequestDelegate` 來啟動：</span><span class="sxs-lookup"><span data-stu-id="543fb-337">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="543fb-338">產生與 **Start(RequestDelegate app)** 相同的結果，除了應用程式會在 `http://localhost:8080` 回應。</span><span class="sxs-lookup"><span data-stu-id="543fb-338">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="543fb-339">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="543fb-339">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="543fb-340">使用 `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) 的執行個體來使用路由中介軟體：</span><span class="sxs-lookup"><span data-stu-id="543fb-340">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="543fb-341">使用下列瀏覽器要求與範例：</span><span class="sxs-lookup"><span data-stu-id="543fb-341">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="543fb-342">要求</span><span class="sxs-lookup"><span data-stu-id="543fb-342">Request</span></span>                                    | <span data-ttu-id="543fb-343">回應</span><span class="sxs-lookup"><span data-stu-id="543fb-343">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="543fb-344">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="543fb-344">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="543fb-345">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="543fb-345">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="543fb-346">擲回例外狀況，字串為 "ooops!"</span><span class="sxs-lookup"><span data-stu-id="543fb-346">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="543fb-347">擲回例外狀況，字串為 "Un oh!"</span><span class="sxs-lookup"><span data-stu-id="543fb-347">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="543fb-348">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="543fb-348">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="543fb-349">Hello World!</span><span class="sxs-lookup"><span data-stu-id="543fb-349">Hello World!</span></span>                             |

<span data-ttu-id="543fb-350">`WaitForShutdown` 會封鎖，直到發出中斷 (Ctrl-C/SIGINT 或 SIGTERM)。</span><span class="sxs-lookup"><span data-stu-id="543fb-350">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="543fb-351">應用程式會顯示 `Console.WriteLine` 訊息並等候按鍵動作以便結束。</span><span class="sxs-lookup"><span data-stu-id="543fb-351">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="543fb-352">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="543fb-352">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="543fb-353">使用 URL 和 `IRouteBuilder` 的執行個體：</span><span class="sxs-lookup"><span data-stu-id="543fb-353">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="543fb-354">產生與 **Start(Action&lt;IRouteBuilder&gt; routeBuilder)** 相同的結果，除了應用程式會在 `http://localhost:8080` 回應。</span><span class="sxs-lookup"><span data-stu-id="543fb-354">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="543fb-355">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="543fb-355">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="543fb-356">提供委派以設定 `IApplicationBuilder`：</span><span class="sxs-lookup"><span data-stu-id="543fb-356">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="543fb-357">在瀏覽器中提出要求給 `http://localhost:5000`，以收到 "Hello World!" 回應</span><span class="sxs-lookup"><span data-stu-id="543fb-357">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="543fb-358">`WaitForShutdown` 會封鎖，直到發出中斷 (Ctrl-C/SIGINT 或 SIGTERM)。</span><span class="sxs-lookup"><span data-stu-id="543fb-358">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="543fb-359">應用程式會顯示 `Console.WriteLine` 訊息並等候按鍵動作以便結束。</span><span class="sxs-lookup"><span data-stu-id="543fb-359">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="543fb-360">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="543fb-360">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="543fb-361">提供 URL 和委派以設定 `IApplicationBuilder`：</span><span class="sxs-lookup"><span data-stu-id="543fb-361">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="543fb-362">產生與 **StartWith(Action&lt;IApplicationBuilder&gt; app)** 相同的結果，除了應用程式會在 `http://localhost:8080` 回應。</span><span class="sxs-lookup"><span data-stu-id="543fb-362">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="543fb-363">**執行**</span><span class="sxs-lookup"><span data-stu-id="543fb-363">**Run**</span></span>

<span data-ttu-id="543fb-364">`Run` 方法會啟動 Web 應用程式，並且封鎖呼叫執行緒，直到主機關閉為止：</span><span class="sxs-lookup"><span data-stu-id="543fb-364">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="543fb-365">**Start**</span><span class="sxs-lookup"><span data-stu-id="543fb-365">**Start**</span></span>

<span data-ttu-id="543fb-366">藉由呼叫 `Start` 方法，以非封鎖方式執行主機：</span><span class="sxs-lookup"><span data-stu-id="543fb-366">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="543fb-367">如果 URL 的清單傳遞至 `Start` 方法，它會接聽指定的 URL：</span><span class="sxs-lookup"><span data-stu-id="543fb-367">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

::: moniker-end

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="543fb-368">IHostingEnvironment 介面</span><span class="sxs-lookup"><span data-stu-id="543fb-368">IHostingEnvironment interface</span></span>

<span data-ttu-id="543fb-369">[IHostingEnvironment 介面](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment)提供應用程式的 Web 裝載環境相關資訊。</span><span class="sxs-lookup"><span data-stu-id="543fb-369">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="543fb-370">使用[建構函式插入](xref:fundamentals/dependency-injection)以取得 `IHostingEnvironment`，才能使用其屬性和擴充方法：</span><span class="sxs-lookup"><span data-stu-id="543fb-370">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="543fb-371">[以慣例為基礎的方法](xref:fundamentals/environments#environment-based-startup-class-and-methods)可用來根據環境在啟動時設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="543fb-371">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="543fb-372">或者，將 `IHostingEnvironment` 插入至 `Startup` 建構函式，以便用於 `ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="543fb-372">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="543fb-373">除了 `IsDevelopment` 擴充方法，`IHostingEnvironment` 也提供 `IsStaging`、`IsProduction` 和 `IsEnvironment(string environmentName)` 方法。</span><span class="sxs-lookup"><span data-stu-id="543fb-373">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="543fb-374">如需詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="543fb-374">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="543fb-375">`IHostingEnvironment` 服務也可直接插入至 `Configure` 方法，以便設定處理管線：</span><span class="sxs-lookup"><span data-stu-id="543fb-375">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="543fb-376">建立自訂[中介軟體](xref:fundamentals/middleware/index#writing-middleware)時，`IHostingEnvironment` 可以插入至 `Invoke` 方法：</span><span class="sxs-lookup"><span data-stu-id="543fb-376">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="543fb-377">IApplicationLifetime 介面</span><span class="sxs-lookup"><span data-stu-id="543fb-377">IApplicationLifetime interface</span></span>

<span data-ttu-id="543fb-378">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) 允許啟動後和關機活動。</span><span class="sxs-lookup"><span data-stu-id="543fb-378">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="543fb-379">在介面上的三個屬性是用來註冊定義啟動和關閉事件之 `Action` 方法的取消語彙基元。</span><span class="sxs-lookup"><span data-stu-id="543fb-379">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="543fb-380">取消語彙基元</span><span class="sxs-lookup"><span data-stu-id="543fb-380">Cancellation Token</span></span>    | <span data-ttu-id="543fb-381">觸發時機&#8230;</span><span class="sxs-lookup"><span data-stu-id="543fb-381">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="543fb-382">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="543fb-382">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="543fb-383">已完全啟動主機。</span><span class="sxs-lookup"><span data-stu-id="543fb-383">The host has fully started.</span></span> |
| [<span data-ttu-id="543fb-384">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="543fb-384">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="543fb-385">主機正在完成正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="543fb-385">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="543fb-386">應該處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="543fb-386">All requests should be processed.</span></span> <span data-ttu-id="543fb-387">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="543fb-387">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="543fb-388">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="543fb-388">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="543fb-389">主機正在執行正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="543fb-389">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="543fb-390">可能仍在處理要求。</span><span class="sxs-lookup"><span data-stu-id="543fb-390">Requests may still be processing.</span></span> <span data-ttu-id="543fb-391">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="543fb-391">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="543fb-392">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) 會要求應用程式終止。</span><span class="sxs-lookup"><span data-stu-id="543fb-392">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="543fb-393">當呼叫類別的 `Shutdown` 方法時，下列類別使用 `StopApplication` 來順利關閉應用程式：</span><span class="sxs-lookup"><span data-stu-id="543fb-393">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="543fb-394">範圍驗證</span><span class="sxs-lookup"><span data-stu-id="543fb-394">Scope validation</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="543fb-395">如果應用程式的環境是「開發」，[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 會將 [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) 設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="543fb-395">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

::: moniker-end

<span data-ttu-id="543fb-396">當 `ValidateScopes` 設定為 `true` 時，預設服務提供者會執行檢查以確認：</span><span class="sxs-lookup"><span data-stu-id="543fb-396">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="543fb-397">範圍服務不是直接或間接由根服務提供者解析。</span><span class="sxs-lookup"><span data-stu-id="543fb-397">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="543fb-398">範圍服務不是直接或間接插入至單一服務。</span><span class="sxs-lookup"><span data-stu-id="543fb-398">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="543fb-399">根服務提供者會在呼叫 [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) 時建立。</span><span class="sxs-lookup"><span data-stu-id="543fb-399">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="543fb-400">當提供者啟動應用程式時，根服務提供者的存留期與應用程式/伺服器的存留期一致，並會在應用程式關閉時處置。</span><span class="sxs-lookup"><span data-stu-id="543fb-400">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="543fb-401">範圍服務會由建立這些服務的容器處置。</span><span class="sxs-lookup"><span data-stu-id="543fb-401">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="543fb-402">若是在根容器中建立範圍服務，因為當應用程式/伺服器關機時，服務只會由根容器處理，所以服務的存留期會提升為單一服務等級。</span><span class="sxs-lookup"><span data-stu-id="543fb-402">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="543fb-403">當呼叫 `BuildServiceProvider` 時，驗證服務範圍會攔截到這些情況。</span><span class="sxs-lookup"><span data-stu-id="543fb-403">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="543fb-404">若要一律驗證範圍，包括在實際執行環境中，請使用主機產生器上的 [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) 來設定 [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions)：</span><span class="sxs-lookup"><span data-stu-id="543fb-404">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

::: moniker range="= aspnetcore-2.0"

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="543fb-405">針對 System.ArgumentException 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="543fb-405">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="543fb-406">**下列情況僅在 ASP.NET Core 2.0 應用程式未呼叫 `UseStartup` 或 `Configure` 時，適用於該應用程式。**</span><span class="sxs-lookup"><span data-stu-id="543fb-406">**The following only applies to ASP.NET Core 2.0 apps when the app doesn't call `UseStartup` or `Configure`.**</span></span>

<span data-ttu-id="543fb-407">可以藉由將 `IStartup` 直接插入至相依性插入容器來建置主機，而不是呼叫 `UseStartup` 或 `Configure`：</span><span class="sxs-lookup"><span data-stu-id="543fb-407">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="543fb-408">如果主機以此方式建置，可能會發生下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="543fb-408">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="543fb-409">發生此錯誤的原因是，必須具有應用程式名稱 (目前組件的名稱) 才可掃描 `HostingStartupAttributes`。</span><span class="sxs-lookup"><span data-stu-id="543fb-409">This occurs because the app name (the name of the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="543fb-410">如果應用程式以手動方式將 `IStartup` 插入至相依性插入容器，請將下列呼叫新增至 `WebHostBuilder`，並指定組件名稱：</span><span class="sxs-lookup"><span data-stu-id="543fb-410">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "AssemblyName")
```

<span data-ttu-id="543fb-411">或者，將虛設的 `Configure` 新增至 `WebHostBuilder`，以自動設定應用程式名稱：</span><span class="sxs-lookup"><span data-stu-id="543fb-411">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the app name automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
```

<span data-ttu-id="543fb-412">如需詳細資訊，請參閱 [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) (公告：已移除 Microsoft.Extensions.PlatformAbstractions (註解)) 和 [StartupInjection 範例](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs)。</span><span class="sxs-lookup"><span data-stu-id="543fb-412">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="543fb-413">其他資源</span><span class="sxs-lookup"><span data-stu-id="543fb-413">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
