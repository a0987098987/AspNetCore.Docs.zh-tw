---
title: "ASP.NET Core 中的裝載"
author: guardrex
description: "深入了解 ASP.NET Core 中的 Web 主機，其負責啟動應用程式以及管理存留期。"
manager: wpickett
ms.author: riande
ms.date: 09/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosting
ms.openlocfilehash: 004487aebe5262a515e2375c30ccd2a84844dff6
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/01/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="0fc66-103">ASP.NET Core 中的裝載</span><span class="sxs-lookup"><span data-stu-id="0fc66-103">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="0fc66-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0fc66-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0fc66-105">ASP.NET Core 應用程式會設定並啟動「主機」。</span><span class="sxs-lookup"><span data-stu-id="0fc66-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="0fc66-106">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="0fc66-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="0fc66-107">至少，主機會設定伺服器和要求處理管線。</span><span class="sxs-lookup"><span data-stu-id="0fc66-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="0fc66-108">設定主機</span><span class="sxs-lookup"><span data-stu-id="0fc66-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0fc66-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0fc66-110">使用 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 的執行個體建立主機。</span><span class="sxs-lookup"><span data-stu-id="0fc66-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="0fc66-111">這通常在應用程式的進入點執行，也就是 `Main` 方法。</span><span class="sxs-lookup"><span data-stu-id="0fc66-111">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="0fc66-112">在專案範本中，`Main` 位於 *Program.cs*。</span><span class="sxs-lookup"><span data-stu-id="0fc66-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="0fc66-113">一般 *Program.cs* 會呼叫 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 以開始設定主機：</span><span class="sxs-lookup"><span data-stu-id="0fc66-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="0fc66-114">`CreateDefaultBuilder` 會執行下列工作：</span><span class="sxs-lookup"><span data-stu-id="0fc66-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="0fc66-115">設定 [Kestrel](servers/kestrel.md) 作為網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="0fc66-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="0fc66-116">如需 Kestrel 預設選項，請參閱 [ASP.NET Core 中的 Kestrel 網頁伺服器實作的 Kestrel 選項一節](xref:fundamentals/servers/kestrel#kestrel-options)。</span><span class="sxs-lookup"><span data-stu-id="0fc66-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="0fc66-117">設定 [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) 所傳回路徑的內容根目錄。</span><span class="sxs-lookup"><span data-stu-id="0fc66-117">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="0fc66-118">從下列位置載入選擇性組態：</span><span class="sxs-lookup"><span data-stu-id="0fc66-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="0fc66-119">*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="0fc66-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="0fc66-120">*appsettings.{Environment}.json*</span><span class="sxs-lookup"><span data-stu-id="0fc66-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="0fc66-121">應用程式在 `Development` 環境中執行時的[使用者密碼](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="0fc66-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="0fc66-122">環境變數。</span><span class="sxs-lookup"><span data-stu-id="0fc66-122">Environment variables.</span></span>
  * <span data-ttu-id="0fc66-123">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="0fc66-123">Command-line arguments.</span></span>
* <span data-ttu-id="0fc66-124">設定主控台和偵錯輸出的[記錄](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="0fc66-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="0fc66-125">記錄包含 *appsettings.json* 或 *appsettings.{Environment}.json* 檔案的記錄組態區段中指定的[記錄檔篩選](xref:fundamentals/logging/index#log-filtering)規則。</span><span class="sxs-lookup"><span data-stu-id="0fc66-125">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="0fc66-126">在 IIS 背後執行時，啟用 [IIS 整合](xref:host-and-deploy/iis/index)。</span><span class="sxs-lookup"><span data-stu-id="0fc66-126">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="0fc66-127">設定使用 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)時伺服器接聽的基底路徑和連接埠。</span><span class="sxs-lookup"><span data-stu-id="0fc66-127">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="0fc66-128">此模組會在 IIS 和 Kestrel 之間建立反向 Proxy。</span><span class="sxs-lookup"><span data-stu-id="0fc66-128">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="0fc66-129">也會設定應用程式以[擷取啟動錯誤](#capture-startup-errors)。</span><span class="sxs-lookup"><span data-stu-id="0fc66-129">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="0fc66-130">如需 IIS 預設選項，請參閱[使用 IIS 在 Windows 上裝載 ASP.NET Core 的 IIS 選項一節](xref:host-and-deploy/iis/index#iis-options)。</span><span class="sxs-lookup"><span data-stu-id="0fc66-130">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="0fc66-131">「內容根目錄」會決定主機搜尋內容檔案 (例如 MVC 檢視檔案) 的位置。</span><span class="sxs-lookup"><span data-stu-id="0fc66-131">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="0fc66-132">從專案的根資料夾啟動應用程式時，會使用專案的根資料夾作為內容根目錄。</span><span class="sxs-lookup"><span data-stu-id="0fc66-132">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="0fc66-133">這是 [Visual Studio](https://www.visualstudio.com/) 和 [dotnet 新範本](/dotnet/core/tools/dotnet-new)中使用的預設值。</span><span class="sxs-lookup"><span data-stu-id="0fc66-133">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="0fc66-134">如需應用程式設定的詳細資訊，請參閱 [ASP.NET Core 中的組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="0fc66-134">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="0fc66-135">作為使用靜態 `CreateDefaultBuilder` 方法的替代做法，ASP.NET Core 2.x 支援從 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 建立主機的方法。</span><span class="sxs-lookup"><span data-stu-id="0fc66-135">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="0fc66-136">如需詳細資訊，請參閱 ASP.NET Core 1.x 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="0fc66-136">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0fc66-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0fc66-138">使用 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 的執行個體建立主機。</span><span class="sxs-lookup"><span data-stu-id="0fc66-138">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="0fc66-139">建立主機通常在應用程式的進入點執行，也就是 `Main` 方法。</span><span class="sxs-lookup"><span data-stu-id="0fc66-139">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="0fc66-140">在專案範本中，`Main` 位於 *Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="0fc66-140">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="0fc66-141">`WebHostBuilder` 需要[實作 IServer 的伺服器](servers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="0fc66-141">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="0fc66-142">內建的伺服器是 [Kestrel](servers/kestrel.md) 和 [HTTP.sys](servers/httpsys.md) (在 ASP.NET Core 2.0 版之前，HTTP.sys 稱為 [WebListener](xref:fundamentals/servers/weblistener))。</span><span class="sxs-lookup"><span data-stu-id="0fc66-142">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="0fc66-143">在此範例中，[UseKestrel 擴充方法](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)會指定 Kestrel 伺服器。</span><span class="sxs-lookup"><span data-stu-id="0fc66-143">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="0fc66-144">「內容根目錄」會決定主機搜尋內容檔案 (例如 MVC 檢視檔案) 的位置。</span><span class="sxs-lookup"><span data-stu-id="0fc66-144">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="0fc66-145">[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1) 會取得 `UseContentRoot` 的預設內容根目錄。</span><span class="sxs-lookup"><span data-stu-id="0fc66-145">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="0fc66-146">從專案的根資料夾啟動應用程式時，會使用專案的根資料夾作為內容根目錄。</span><span class="sxs-lookup"><span data-stu-id="0fc66-146">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="0fc66-147">這是 [Visual Studio](https://www.visualstudio.com/) 和 [dotnet 新範本](/dotnet/core/tools/dotnet-new)中使用的預設值。</span><span class="sxs-lookup"><span data-stu-id="0fc66-147">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="0fc66-148">若要使用 IIS 作為反向 Proxy，請在建置主機時呼叫 [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions)。</span><span class="sxs-lookup"><span data-stu-id="0fc66-148">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="0fc66-149">`UseIISIntegration` 不會像 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)那樣設定「伺服器」。</span><span class="sxs-lookup"><span data-stu-id="0fc66-149">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="0fc66-150">`UseIISIntegration` 會設定使用 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)以在 Kestrel 和 IIS 之間建立反向 Proxy 時，伺服器接聽的基底路徑和連接埠。</span><span class="sxs-lookup"><span data-stu-id="0fc66-150">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="0fc66-151">若要搭配使用 IIS 與 ASP.NET Core，必須指定 `UseKestrel` 和 `UseIISIntegration`。</span><span class="sxs-lookup"><span data-stu-id="0fc66-151">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="0fc66-152">`UseIISIntegration` 只會在 IIS 或 IIS Express 背後執行時啟動。</span><span class="sxs-lookup"><span data-stu-id="0fc66-152">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="0fc66-153">如需詳細資訊，請參閱 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)和 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="0fc66-153">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="0fc66-154">設定主機 (和 ASP.NET Core 應用程式) 的最小實作，包含指定伺服器和應用程式要求管線的組態：</span><span class="sxs-lookup"><span data-stu-id="0fc66-154">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="0fc66-155">設定主機時，可以提供 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) 和 [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) 方法。</span><span class="sxs-lookup"><span data-stu-id="0fc66-155">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="0fc66-156">如果指定 `Startup` 類別，它必須定義 `Configure` 方法。</span><span class="sxs-lookup"><span data-stu-id="0fc66-156">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="0fc66-157">如需詳細資訊，請參閱 [ASP.NET Core 中的應用程式啟動](startup.md)。</span><span class="sxs-lookup"><span data-stu-id="0fc66-157">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="0fc66-158">多次呼叫 `ConfigureServices` 會彼此附加。</span><span class="sxs-lookup"><span data-stu-id="0fc66-158">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="0fc66-159">對 `WebHostBuilder` 多次呼叫 `Configure` 或 `UseStartup` 則會取代先前的設定。</span><span class="sxs-lookup"><span data-stu-id="0fc66-159">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="0fc66-160">主機組態值</span><span class="sxs-lookup"><span data-stu-id="0fc66-160">Host configuration values</span></span>

<span data-ttu-id="0fc66-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 依賴下列方法來設定主機組態值：</span><span class="sxs-lookup"><span data-stu-id="0fc66-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="0fc66-162">主機建立器組態，其中包含 `ASPNETCORE_{configurationKey}` 格式的環境變數。</span><span class="sxs-lookup"><span data-stu-id="0fc66-162">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="0fc66-163">例如，`ASPNETCORE_URLS`。</span><span class="sxs-lookup"><span data-stu-id="0fc66-163">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="0fc66-164">明確的方法，例如 `CaptureStartupErrors`。</span><span class="sxs-lookup"><span data-stu-id="0fc66-164">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="0fc66-165">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) 和相關聯的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="0fc66-165">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="0fc66-166">使用 `UseSetting` 設定值時，此值會設定為字串，而不論其類型為何。</span><span class="sxs-lookup"><span data-stu-id="0fc66-166">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="0fc66-167">主機使用設定最後一個值的任何選項。</span><span class="sxs-lookup"><span data-stu-id="0fc66-167">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="0fc66-168">如需詳細資訊，請參閱下一節的[覆寫設定](#overriding-configuration)。</span><span class="sxs-lookup"><span data-stu-id="0fc66-168">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="0fc66-169">擷取啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="0fc66-169">Capture Startup Errors</span></span>

<span data-ttu-id="0fc66-170">此設定會控制啟動錯誤的擷取。</span><span class="sxs-lookup"><span data-stu-id="0fc66-170">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="0fc66-171">**索引鍵**：captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="0fc66-171">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="0fc66-172">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="0fc66-172">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="0fc66-173">**預設值**：預設為 `false`，除非應用程式執行時在 IIS 背後有 Kestrel，此時預設值即為 `true`。</span><span class="sxs-lookup"><span data-stu-id="0fc66-173">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="0fc66-174">**設定使用**：`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="0fc66-174">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="0fc66-175">**環境變數**：`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="0fc66-175">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="0fc66-176">當它為 `false` 時，啟動期間發生的錯誤會導致主機結束。</span><span class="sxs-lookup"><span data-stu-id="0fc66-176">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="0fc66-177">當它為 `true` 時，主機會擷取啟動期間的例外狀況，並嘗試啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="0fc66-177">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0fc66-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0fc66-179">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-179">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="0fc66-180">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="0fc66-180">Content Root</span></span>

<span data-ttu-id="0fc66-181">此設定可決定 ASP.NET Core 開始搜尋內容檔案 (例如 MVC 檢視) 的位置。</span><span class="sxs-lookup"><span data-stu-id="0fc66-181">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="0fc66-182">**索引鍵**：contentRoot</span><span class="sxs-lookup"><span data-stu-id="0fc66-182">**Key**: contentRoot</span></span>  
<span data-ttu-id="0fc66-183">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="0fc66-183">**Type**: *string*</span></span>  
<span data-ttu-id="0fc66-184">**預設值**：預設為應用程式組件所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="0fc66-184">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="0fc66-185">**設定使用**：`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="0fc66-185">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="0fc66-186">**環境變數**：`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="0fc66-186">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="0fc66-187">內容根目錄也作為 [Web 根目錄設定](#web-root)的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="0fc66-187">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="0fc66-188">如果路徑不存在，就無法啟動主機。</span><span class="sxs-lookup"><span data-stu-id="0fc66-188">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0fc66-189">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0fc66-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="0fc66-191">詳細錯誤</span><span class="sxs-lookup"><span data-stu-id="0fc66-191">Detailed Errors</span></span>

<span data-ttu-id="0fc66-192">決定是否應該擷取詳細的錯誤。</span><span class="sxs-lookup"><span data-stu-id="0fc66-192">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="0fc66-193">**索引鍵**：detailedErrors</span><span class="sxs-lookup"><span data-stu-id="0fc66-193">**Key**: detailedErrors</span></span>  
<span data-ttu-id="0fc66-194">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="0fc66-194">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="0fc66-195">**預設值**：false</span><span class="sxs-lookup"><span data-stu-id="0fc66-195">**Default**: false</span></span>  
<span data-ttu-id="0fc66-196">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="0fc66-196">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="0fc66-197">**環境變數**：`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="0fc66-197">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="0fc66-198">啟用時 (或當 <a href="#environment">Environment</a> 設定為 `Development` 時)，應用程式會擷取詳細例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0fc66-198">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0fc66-199">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-199">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0fc66-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="0fc66-201">環境</span><span class="sxs-lookup"><span data-stu-id="0fc66-201">Environment</span></span>

<span data-ttu-id="0fc66-202">設定應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="0fc66-202">Sets the app's environment.</span></span>

<span data-ttu-id="0fc66-203">**索引鍵**：environment</span><span class="sxs-lookup"><span data-stu-id="0fc66-203">**Key**: environment</span></span>  
<span data-ttu-id="0fc66-204">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="0fc66-204">**Type**: *string*</span></span>  
<span data-ttu-id="0fc66-205">**預設值**：Production</span><span class="sxs-lookup"><span data-stu-id="0fc66-205">**Default**: Production</span></span>  
<span data-ttu-id="0fc66-206">**設定使用**：`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="0fc66-206">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="0fc66-207">**環境變數**：`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="0fc66-207">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="0fc66-208">環境可以設定為任何值。</span><span class="sxs-lookup"><span data-stu-id="0fc66-208">The environment can be set to any value.</span></span> <span data-ttu-id="0fc66-209">架構定義的值包括 `Development`、`Staging` 和 `Production`。</span><span class="sxs-lookup"><span data-stu-id="0fc66-209">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="0fc66-210">值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="0fc66-210">Values aren't case sensitive.</span></span> <span data-ttu-id="0fc66-211">根據預設，*Environment* 是從 `ASPNETCORE_ENVIRONMENT` 環境變數讀取。</span><span class="sxs-lookup"><span data-stu-id="0fc66-211">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="0fc66-212">使用 [Visual Studio](https://www.visualstudio.com/) 時，可能會在 *launchSettings.json* 檔案設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="0fc66-212">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="0fc66-213">如需詳細資訊，請參閱[使用多個環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="0fc66-213">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0fc66-214">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0fc66-215">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="0fc66-216">裝載啟動組件</span><span class="sxs-lookup"><span data-stu-id="0fc66-216">Hosting Startup Assemblies</span></span>

<span data-ttu-id="0fc66-217">設定應用程式的裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="0fc66-217">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="0fc66-218">**索引鍵**：hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="0fc66-218">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="0fc66-219">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="0fc66-219">**Type**: *string*</span></span>  
<span data-ttu-id="0fc66-220">**預設值**：空字串</span><span class="sxs-lookup"><span data-stu-id="0fc66-220">**Default**: Empty string</span></span>  
<span data-ttu-id="0fc66-221">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="0fc66-221">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="0fc66-222">**環境變數**：`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="0fc66-222">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="0fc66-223">在啟動時載入的裝載啟動組件字串，以分號分隔。</span><span class="sxs-lookup"><span data-stu-id="0fc66-223">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="0fc66-224">這項功能是在 ASP.NET Core 2.0 中新增。</span><span class="sxs-lookup"><span data-stu-id="0fc66-224">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="0fc66-225">雖然設定值會預設為空字串，但裝載啟動組件一律會包含應用程式的組件。</span><span class="sxs-lookup"><span data-stu-id="0fc66-225">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="0fc66-226">提供裝載啟動組件時，它們會新增至應用程式的組件，以便在應用程式在啟動時建置其通用服務時載入。</span><span class="sxs-lookup"><span data-stu-id="0fc66-226">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0fc66-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0fc66-228">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-228">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0fc66-229">這項功能在 ASP.NET Core 1.x 中未提供。</span><span class="sxs-lookup"><span data-stu-id="0fc66-229">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="0fc66-230">偏好裝載 URL</span><span class="sxs-lookup"><span data-stu-id="0fc66-230">Prefer Hosting URLs</span></span>

<span data-ttu-id="0fc66-231">表示主機是否應接聽使用 `WebHostBuilder` 設定的 URL，而不是 `IServer` 實作所設定的 URL。</span><span class="sxs-lookup"><span data-stu-id="0fc66-231">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="0fc66-232">**索引鍵**：preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="0fc66-232">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="0fc66-233">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="0fc66-233">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="0fc66-234">**預設值**：true</span><span class="sxs-lookup"><span data-stu-id="0fc66-234">**Default**: true</span></span>  
<span data-ttu-id="0fc66-235">**設定使用**：`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="0fc66-235">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="0fc66-236">**環境變數**：`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="0fc66-236">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="0fc66-237">這項功能是在 ASP.NET Core 2.0 中新增。</span><span class="sxs-lookup"><span data-stu-id="0fc66-237">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0fc66-238">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-238">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0fc66-239">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-239">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0fc66-240">這項功能在 ASP.NET Core 1.x 中未提供。</span><span class="sxs-lookup"><span data-stu-id="0fc66-240">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="0fc66-241">防止裝載啟動</span><span class="sxs-lookup"><span data-stu-id="0fc66-241">Prevent Hosting Startup</span></span>

<span data-ttu-id="0fc66-242">可防止自動載入裝載啟動組件，包括應用程式組件所設定的裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="0fc66-242">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="0fc66-243">請參閱[使用 IHostingStartup 從外部組件新增應用程式功能](xref:host-and-deploy/ihostingstartup)以取得詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="0fc66-243">See [Add app features from an external assembly using IHostingStartup](xref:host-and-deploy/ihostingstartup) for more information.</span></span>

<span data-ttu-id="0fc66-244">**索引鍵**preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="0fc66-244">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="0fc66-245">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="0fc66-245">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="0fc66-246">**預設值**：false</span><span class="sxs-lookup"><span data-stu-id="0fc66-246">**Default**: false</span></span>  
<span data-ttu-id="0fc66-247">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="0fc66-247">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="0fc66-248">**環境變數**：`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="0fc66-248">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="0fc66-249">這項功能是在 ASP.NET Core 2.0 中新增。</span><span class="sxs-lookup"><span data-stu-id="0fc66-249">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0fc66-250">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-250">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0fc66-251">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-251">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0fc66-252">這項功能在 ASP.NET Core 1.x 中未提供。</span><span class="sxs-lookup"><span data-stu-id="0fc66-252">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="0fc66-253">伺服器 URL</span><span class="sxs-lookup"><span data-stu-id="0fc66-253">Server URLs</span></span>

<span data-ttu-id="0fc66-254">表示伺服器應該接聽要求的 IP 位址或主機位址，以及連接埠和通訊協定。</span><span class="sxs-lookup"><span data-stu-id="0fc66-254">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="0fc66-255">**索引鍵**：urls</span><span class="sxs-lookup"><span data-stu-id="0fc66-255">**Key**: urls</span></span>  
<span data-ttu-id="0fc66-256">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="0fc66-256">**Type**: *string*</span></span>  
<span data-ttu-id="0fc66-257">**預設值**：http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="0fc66-257">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="0fc66-258">**設定使用**：`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="0fc66-258">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="0fc66-259">**環境變數**：`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="0fc66-259">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="0fc66-260">設定為伺服器應該回應的 URL 前置清單，並以分號 (;) 分隔。</span><span class="sxs-lookup"><span data-stu-id="0fc66-260">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="0fc66-261">例如，`http://localhost:123`。</span><span class="sxs-lookup"><span data-stu-id="0fc66-261">For example, `http://localhost:123`.</span></span> <span data-ttu-id="0fc66-262">使用 "\*"，表示伺服器應接聽任何 IP 位址或主機名稱上的要求，並使用指定的連接埠和通訊協定 (例如，`http://*:5000`)。</span><span class="sxs-lookup"><span data-stu-id="0fc66-262">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="0fc66-263">通訊協定 (`http://` 或 `https://`) 必須包含在每個 URL 中。</span><span class="sxs-lookup"><span data-stu-id="0fc66-263">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="0fc66-264">伺服器之間支援的格式有所不同。</span><span class="sxs-lookup"><span data-stu-id="0fc66-264">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0fc66-265">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-265">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="0fc66-266">Kestrel 有它自己的端點設定 API。</span><span class="sxs-lookup"><span data-stu-id="0fc66-266">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="0fc66-267">如需詳細資訊，請參閱[在 ASP.NET Core 中實作 Kestrel 網頁伺服器](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)。</span><span class="sxs-lookup"><span data-stu-id="0fc66-267">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0fc66-268">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-268">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="0fc66-269">關機逾時</span><span class="sxs-lookup"><span data-stu-id="0fc66-269">Shutdown Timeout</span></span>

<span data-ttu-id="0fc66-270">指定等待 Web 主機關機的時間量。</span><span class="sxs-lookup"><span data-stu-id="0fc66-270">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="0fc66-271">**索引鍵**：shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="0fc66-271">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="0fc66-272">**類型**：*int*</span><span class="sxs-lookup"><span data-stu-id="0fc66-272">**Type**: *int*</span></span>  
<span data-ttu-id="0fc66-273">**預設值**：5</span><span class="sxs-lookup"><span data-stu-id="0fc66-273">**Default**: 5</span></span>  
<span data-ttu-id="0fc66-274">**設定使用**：`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="0fc66-274">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="0fc66-275">**環境變數**：`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="0fc66-275">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="0fc66-276">雖然索引鍵接受 *int* 與 `UseSetting` (例如 `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`)，`UseShutdownTimeout` 擴充方法會採用 `TimeSpan`。</span><span class="sxs-lookup"><span data-stu-id="0fc66-276">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="0fc66-277">這項功能是在 ASP.NET Core 2.0 中新增。</span><span class="sxs-lookup"><span data-stu-id="0fc66-277">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0fc66-278">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-278">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0fc66-279">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-279">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0fc66-280">這項功能在 ASP.NET Core 1.x 中未提供。</span><span class="sxs-lookup"><span data-stu-id="0fc66-280">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="0fc66-281">啟動組件</span><span class="sxs-lookup"><span data-stu-id="0fc66-281">Startup Assembly</span></span>

<span data-ttu-id="0fc66-282">決定要搜尋 `Startup` 類別的組件。</span><span class="sxs-lookup"><span data-stu-id="0fc66-282">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="0fc66-283">**索引鍵**：startupAssembly</span><span class="sxs-lookup"><span data-stu-id="0fc66-283">**Key**: startupAssembly</span></span>  
<span data-ttu-id="0fc66-284">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="0fc66-284">**Type**: *string*</span></span>  
<span data-ttu-id="0fc66-285">**預設值**：應用程式的組件</span><span class="sxs-lookup"><span data-stu-id="0fc66-285">**Default**: The app's assembly</span></span>  
<span data-ttu-id="0fc66-286">**設定使用**：`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="0fc66-286">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="0fc66-287">**環境變數**：`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="0fc66-287">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="0fc66-288">可以參考依名稱 (`string`) 或類型 (`TStartup`) 的組件。</span><span class="sxs-lookup"><span data-stu-id="0fc66-288">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="0fc66-289">如果呼叫多個 `UseStartup` 方法，最後一個將會優先。</span><span class="sxs-lookup"><span data-stu-id="0fc66-289">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0fc66-290">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-290">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0fc66-291">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-291">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
    ...
```

---

### <a name="web-root"></a><span data-ttu-id="0fc66-292">Web 根目錄</span><span class="sxs-lookup"><span data-stu-id="0fc66-292">Web Root</span></span>

<span data-ttu-id="0fc66-293">設定應用程式靜態資產的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="0fc66-293">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="0fc66-294">**索引鍵**：webroot</span><span class="sxs-lookup"><span data-stu-id="0fc66-294">**Key**: webroot</span></span>  
<span data-ttu-id="0fc66-295">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="0fc66-295">**Type**: *string*</span></span>  
<span data-ttu-id="0fc66-296">**預設值**：如果未指定，則預設值為 "(Content Root)/wwwroot"，如果該路徑存在的話。</span><span class="sxs-lookup"><span data-stu-id="0fc66-296">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="0fc66-297">如果路徑不存在，則會使用無作業檔案提供者。</span><span class="sxs-lookup"><span data-stu-id="0fc66-297">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="0fc66-298">**設定使用**：`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="0fc66-298">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="0fc66-299">**環境變數**：`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="0fc66-299">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0fc66-300">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-300">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0fc66-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="0fc66-302">覆寫組態</span><span class="sxs-lookup"><span data-stu-id="0fc66-302">Overriding configuration</span></span>

<span data-ttu-id="0fc66-303">使用 [Configuration](xref:fundamentals/configuration/index) 來設定主機。</span><span class="sxs-lookup"><span data-stu-id="0fc66-303">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="0fc66-304">在下列範例中，主機組態選擇性地指定於 *hosting.json* 檔案中。</span><span class="sxs-lookup"><span data-stu-id="0fc66-304">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="0fc66-305">從 *hosting.json* 檔案載入的任何設定，可以用命令列引數覆寫。</span><span class="sxs-lookup"><span data-stu-id="0fc66-305">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="0fc66-306">組建組態 (在 `config` 中) 用以使用 `UseConfiguration` 來設定主機。</span><span class="sxs-lookup"><span data-stu-id="0fc66-306">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0fc66-307">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-307">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0fc66-308">*hosting.json*：</span><span class="sxs-lookup"><span data-stu-id="0fc66-308">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="0fc66-309">首先使用 *hosting.json* 組態來覆寫 `UseUrls` 所提供的設定，其次是命令列引數：</span><span class="sxs-lookup"><span data-stu-id="0fc66-309">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0fc66-310">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-310">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0fc66-311">*hosting.json*：</span><span class="sxs-lookup"><span data-stu-id="0fc66-311">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="0fc66-312">首先使用 *hosting.json* 組態來覆寫 `UseUrls` 所提供的設定，其次是命令列引數：</span><span class="sxs-lookup"><span data-stu-id="0fc66-312">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
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

---

> [!NOTE]
> <span data-ttu-id="0fc66-313">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 擴充方法目前無法剖析 `GetSection` 傳回的組態區段 (例如 `.UseConfiguration(Configuration.GetSection("section"))`。</span><span class="sxs-lookup"><span data-stu-id="0fc66-313">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="0fc66-314">`GetSection` 方法會將設定索引鍵篩選到要求的區段，但保留索引鍵上的區段名稱 (例如 `section:urls`、`section:environment`)。</span><span class="sxs-lookup"><span data-stu-id="0fc66-314">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="0fc66-315">`UseConfiguration` 方法預期索引鍵要符合 `WebHostBuilder` 索引鍵 (例如 `urls`、`environment`)。</span><span class="sxs-lookup"><span data-stu-id="0fc66-315">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="0fc66-316">索引鍵上的區段名稱存在可避免區段的值設定主機。</span><span class="sxs-lookup"><span data-stu-id="0fc66-316">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="0fc66-317">這個問題將在近期的版本中解決。</span><span class="sxs-lookup"><span data-stu-id="0fc66-317">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="0fc66-318">如需詳細資訊和因應措施，請參閱 [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (將設定區段傳遞到 WebHostBuilder.UseConfiguration 會使用完整索引鍵)。</span><span class="sxs-lookup"><span data-stu-id="0fc66-318">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="0fc66-319">若要指定主機在特定 URL 上執行，所要的值可以在執行 `dotnet run` 時從命令提示字元傳入。</span><span class="sxs-lookup"><span data-stu-id="0fc66-319">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="0fc66-320">命令列引數會覆寫 *hosting.json* 檔的 `urls` 值，且伺服器會接聽連接埠 8080：</span><span class="sxs-lookup"><span data-stu-id="0fc66-320">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="0fc66-321">啟動主機</span><span class="sxs-lookup"><span data-stu-id="0fc66-321">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0fc66-322">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-322">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0fc66-323">**執行**</span><span class="sxs-lookup"><span data-stu-id="0fc66-323">**Run**</span></span>

<span data-ttu-id="0fc66-324">`Run` 方法會啟動 Web 應用程式，並且封鎖呼叫執行緒，直到主機關閉為止：</span><span class="sxs-lookup"><span data-stu-id="0fc66-324">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="0fc66-325">**Start**</span><span class="sxs-lookup"><span data-stu-id="0fc66-325">**Start**</span></span>

<span data-ttu-id="0fc66-326">藉由呼叫 `Start` 方法，以非封鎖方式執行主機：</span><span class="sxs-lookup"><span data-stu-id="0fc66-326">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="0fc66-327">如果 URL 的清單傳遞至 `Start` 方法，它會接聽指定的 URL：</span><span class="sxs-lookup"><span data-stu-id="0fc66-327">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="0fc66-328">應用程式可以使用預先設定的 `CreateDefaultBuilder` 預設值，使用便利的靜態方法初始化並啟動新的主機。</span><span class="sxs-lookup"><span data-stu-id="0fc66-328">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="0fc66-329">這些方法會啟動伺服器而無主控台輸出，且在使用 [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) 時等候中斷 (Ctrl-C/SIGINT 或 SIGTERM)：</span><span class="sxs-lookup"><span data-stu-id="0fc66-329">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="0fc66-330">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="0fc66-330">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="0fc66-331">使用 `RequestDelegate` 啟動：</span><span class="sxs-lookup"><span data-stu-id="0fc66-331">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="0fc66-332">在瀏覽器中提出要求給 `http://localhost:5000`，以收到 "Hello World!" 回應</span><span class="sxs-lookup"><span data-stu-id="0fc66-332">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="0fc66-333">`WaitForShutdown` 會封鎖，直到發出中斷 (Ctrl-C/SIGINT 或 SIGTERM)。</span><span class="sxs-lookup"><span data-stu-id="0fc66-333">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="0fc66-334">應用程式會顯示 `Console.WriteLine` 訊息並等候按鍵動作以便結束。</span><span class="sxs-lookup"><span data-stu-id="0fc66-334">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="0fc66-335">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="0fc66-335">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="0fc66-336">使用 URL 和 `RequestDelegate` 來啟動：</span><span class="sxs-lookup"><span data-stu-id="0fc66-336">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="0fc66-337">產生與 **Start(RequestDelegate app)** 相同的結果，除了應用程式會在 `http://localhost:8080` 回應。</span><span class="sxs-lookup"><span data-stu-id="0fc66-337">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="0fc66-338">**Start(Action<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="0fc66-338">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="0fc66-339">使用 `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) 的執行個體來使用路由中介軟體：</span><span class="sxs-lookup"><span data-stu-id="0fc66-339">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="0fc66-340">使用下列瀏覽器要求與範例：</span><span class="sxs-lookup"><span data-stu-id="0fc66-340">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="0fc66-341">要求</span><span class="sxs-lookup"><span data-stu-id="0fc66-341">Request</span></span>                                    | <span data-ttu-id="0fc66-342">回應</span><span class="sxs-lookup"><span data-stu-id="0fc66-342">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="0fc66-343">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="0fc66-343">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="0fc66-344">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="0fc66-344">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="0fc66-345">擲回例外狀況，字串為 "ooops!"</span><span class="sxs-lookup"><span data-stu-id="0fc66-345">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="0fc66-346">擲回例外狀況，字串為 "Un oh!"</span><span class="sxs-lookup"><span data-stu-id="0fc66-346">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="0fc66-347">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="0fc66-347">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="0fc66-348">Hello World!</span><span class="sxs-lookup"><span data-stu-id="0fc66-348">Hello World!</span></span>                             |

<span data-ttu-id="0fc66-349">`WaitForShutdown` 會封鎖，直到發出中斷 (Ctrl-C/SIGINT 或 SIGTERM)。</span><span class="sxs-lookup"><span data-stu-id="0fc66-349">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="0fc66-350">應用程式會顯示 `Console.WriteLine` 訊息並等候按鍵動作以便結束。</span><span class="sxs-lookup"><span data-stu-id="0fc66-350">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="0fc66-351">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="0fc66-351">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="0fc66-352">使用 URL 和 `IRouteBuilder` 的執行個體：</span><span class="sxs-lookup"><span data-stu-id="0fc66-352">Use a URL and an instance of `IRouteBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="0fc66-353">產生與 **Start(Action<IRouteBuilder> routeBuilder)** 相同的結果，除了應用程式在 `http://localhost:8080` 回應。</span><span class="sxs-lookup"><span data-stu-id="0fc66-353">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="0fc66-354">**StartWith(Action<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="0fc66-354">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="0fc66-355">提供委派以設定 `IApplicationBuilder`：</span><span class="sxs-lookup"><span data-stu-id="0fc66-355">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="0fc66-356">在瀏覽器中提出要求給 `http://localhost:5000`，以收到 "Hello World!" 回應</span><span class="sxs-lookup"><span data-stu-id="0fc66-356">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="0fc66-357">`WaitForShutdown` 會封鎖，直到發出中斷 (Ctrl-C/SIGINT 或 SIGTERM)。</span><span class="sxs-lookup"><span data-stu-id="0fc66-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="0fc66-358">應用程式會顯示 `Console.WriteLine` 訊息並等候按鍵動作以便結束。</span><span class="sxs-lookup"><span data-stu-id="0fc66-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="0fc66-359">**StartWith(string url, Action<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="0fc66-359">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="0fc66-360">提供 URL 和委派以設定 `IApplicationBuilder`：</span><span class="sxs-lookup"><span data-stu-id="0fc66-360">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="0fc66-361">產生與 **StartWith Action<IApplicationBuilder> app)** 相同的結果，除了應用程式會在 `http://localhost:8080` 回應。</span><span class="sxs-lookup"><span data-stu-id="0fc66-361">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0fc66-362">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0fc66-362">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0fc66-363">**執行**</span><span class="sxs-lookup"><span data-stu-id="0fc66-363">**Run**</span></span>

<span data-ttu-id="0fc66-364">`Run` 方法會啟動 Web 應用程式，並且封鎖呼叫執行緒，直到主機關閉為止：</span><span class="sxs-lookup"><span data-stu-id="0fc66-364">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="0fc66-365">**Start**</span><span class="sxs-lookup"><span data-stu-id="0fc66-365">**Start**</span></span>

<span data-ttu-id="0fc66-366">藉由呼叫 `Start` 方法，以非封鎖方式執行主機：</span><span class="sxs-lookup"><span data-stu-id="0fc66-366">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="0fc66-367">如果 URL 的清單傳遞至 `Start` 方法，它會接聽指定的 URL：</span><span class="sxs-lookup"><span data-stu-id="0fc66-367">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="0fc66-368">IHostingEnvironment 介面</span><span class="sxs-lookup"><span data-stu-id="0fc66-368">IHostingEnvironment interface</span></span>

<span data-ttu-id="0fc66-369">[IHostingEnvironment 介面](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment)提供應用程式的 Web 裝載環境相關資訊。</span><span class="sxs-lookup"><span data-stu-id="0fc66-369">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="0fc66-370">使用[建構函式插入](xref:fundamentals/dependency-injection)以取得 `IHostingEnvironment`，才能使用其屬性和擴充方法：</span><span class="sxs-lookup"><span data-stu-id="0fc66-370">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="0fc66-371">[以慣例為基礎的方法](xref:fundamentals/environments#startup-conventions)可用來根據環境在啟動時設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="0fc66-371">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="0fc66-372">或者，將 `IHostingEnvironment` 插入至 `Startup` 建構函式，以便用於 `ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="0fc66-372">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="0fc66-373">除了 `IsDevelopment` 擴充方法，`IHostingEnvironment` 也提供 `IsStaging`、`IsProduction` 和 `IsEnvironment(string environmentName)` 方法。</span><span class="sxs-lookup"><span data-stu-id="0fc66-373">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="0fc66-374">如需詳細資料，請參閱[使用多個環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="0fc66-374">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="0fc66-375">`IHostingEnvironment` 服務也可直接插入至 `Configure` 方法，以便設定處理管線：</span><span class="sxs-lookup"><span data-stu-id="0fc66-375">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="0fc66-376">建立自訂[中介軟體](xref:fundamentals/middleware/index#writing-middleware)時，`IHostingEnvironment` 可以插入至 `Invoke` 方法：</span><span class="sxs-lookup"><span data-stu-id="0fc66-376">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="0fc66-377">IApplicationLifetime 介面</span><span class="sxs-lookup"><span data-stu-id="0fc66-377">IApplicationLifetime interface</span></span>

<span data-ttu-id="0fc66-378">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) 允許啟動後和關機活動。</span><span class="sxs-lookup"><span data-stu-id="0fc66-378">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="0fc66-379">在介面上的三個屬性是用來註冊定義啟動和關閉事件之 `Action` 方法的取消語彙基元。</span><span class="sxs-lookup"><span data-stu-id="0fc66-379">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span> <span data-ttu-id="0fc66-380">另外還有 `StopApplication` 方法。</span><span class="sxs-lookup"><span data-stu-id="0fc66-380">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="0fc66-381">取消語彙基元</span><span class="sxs-lookup"><span data-stu-id="0fc66-381">Cancellation Token</span></span>    | <span data-ttu-id="0fc66-382">觸發時機&#8230;</span><span class="sxs-lookup"><span data-stu-id="0fc66-382">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="0fc66-383">已完全啟動主機。</span><span class="sxs-lookup"><span data-stu-id="0fc66-383">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="0fc66-384">主機正在執行正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="0fc66-384">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="0fc66-385">可能仍在處理要求。</span><span class="sxs-lookup"><span data-stu-id="0fc66-385">Requests may still be processing.</span></span> <span data-ttu-id="0fc66-386">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="0fc66-386">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="0fc66-387">主機正在完成正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="0fc66-387">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="0fc66-388">應該處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="0fc66-388">All requests should be processed.</span></span> <span data-ttu-id="0fc66-389">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="0fc66-389">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="0fc66-390">方法</span><span class="sxs-lookup"><span data-stu-id="0fc66-390">Method</span></span>            | <span data-ttu-id="0fc66-391">動作</span><span class="sxs-lookup"><span data-stu-id="0fc66-391">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="0fc66-392">要求目前的應用程式終止。</span><span class="sxs-lookup"><span data-stu-id="0fc66-392">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="0fc66-393">針對 System.ArgumentException 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="0fc66-393">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="0fc66-394">**只適用於 ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="0fc66-394">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="0fc66-395">可以藉由將 `IStartup` 直接插入至相依性插入容器來建置主機，而不是呼叫 `UseStartup` 或 `Configure`：</span><span class="sxs-lookup"><span data-stu-id="0fc66-395">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="0fc66-396">如果主機以此方式建置，可能會發生下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="0fc66-396">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="0fc66-397">發生此情況是因為需要 [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (目前的組件)，才能掃描 `HostingStartupAttributes`。</span><span class="sxs-lookup"><span data-stu-id="0fc66-397">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="0fc66-398">如果應用程式以手動方式將 `IStartup` 插入至相依性插入容器，請將下列呼叫新增至 `WebHostBuilder`，並指定組件名稱：</span><span class="sxs-lookup"><span data-stu-id="0fc66-398">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="0fc66-399">或者，新增假的 `Configure` 至 `WebHostBuilder`，其自動設定 `applicationName` (`ApplicationKey`)：</span><span class="sxs-lookup"><span data-stu-id="0fc66-399">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="0fc66-400">**注意**：只有 ASP.NET Core 2.0 版需要這麼做，且只有在應用程式不會呼叫 `UseStartup` 或 `Configure` 時才需要。</span><span class="sxs-lookup"><span data-stu-id="0fc66-400">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="0fc66-401">如需詳細資訊，請參閱 [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) (公告：已移除 Microsoft.Extensions.PlatformAbstractions (註解)) 和 [StartupInjection 範例](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs)。</span><span class="sxs-lookup"><span data-stu-id="0fc66-401">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0fc66-402">其他資源</span><span class="sxs-lookup"><span data-stu-id="0fc66-402">Additional resources</span></span>

* [<span data-ttu-id="0fc66-403"> Windows 上使用 IIS 的主機</span><span class="sxs-lookup"><span data-stu-id="0fc66-403">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="0fc66-404">Linux 上使用 Nginx 的主機</span><span class="sxs-lookup"><span data-stu-id="0fc66-404">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="0fc66-405">Linux 上使用 Apache 的主機</span><span class="sxs-lookup"><span data-stu-id="0fc66-405">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="0fc66-406">Windows 服務中的主機</span><span class="sxs-lookup"><span data-stu-id="0fc66-406">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
