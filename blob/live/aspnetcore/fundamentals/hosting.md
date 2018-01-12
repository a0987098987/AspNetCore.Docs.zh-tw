---
title: "在 ASP.NET Core 中裝載"
author: guardrex
description: "深入了解 ASP.NET Core，負責啟動與存留期管理的應用程式中的 web 主機。"
keywords: "ASP.NET Core web 主機 IWebHost、 WebHostBuilder、 IHostingEnvironment、 IApplicationLifetime"
ms.author: riande
manager: wpickett
ms.date: 09/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.openlocfilehash: 8adc58d67f103e8d1fc8fe197cf392752bdaf660
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/11/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="ed9da-104">在 ASP.NET Core 中裝載</span><span class="sxs-lookup"><span data-stu-id="ed9da-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="ed9da-105">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ed9da-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ed9da-106">ASP.NET Core 應用程式設定和啟動*主機*。</span><span class="sxs-lookup"><span data-stu-id="ed9da-106">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="ed9da-107">負責應用程式啟動和生命週期管理的主機。</span><span class="sxs-lookup"><span data-stu-id="ed9da-107">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="ed9da-108">至少一部伺服器和要求處理管線，會設定主應用程式。</span><span class="sxs-lookup"><span data-stu-id="ed9da-108">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="ed9da-109">設定主機</span><span class="sxs-lookup"><span data-stu-id="ed9da-109">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ed9da-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ed9da-111">建立主應用程式使用的執行個體[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)。</span><span class="sxs-lookup"><span data-stu-id="ed9da-111">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="ed9da-112">這通常執行中應用程式的進入點，`Main`方法。</span><span class="sxs-lookup"><span data-stu-id="ed9da-112">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="ed9da-113">在專案範本中，`Main`位於*Program.cs*。</span><span class="sxs-lookup"><span data-stu-id="ed9da-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="ed9da-114">一般*Program.cs*呼叫[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)啟動主機的設定：</span><span class="sxs-lookup"><span data-stu-id="ed9da-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="ed9da-115">`CreateDefaultBuilder`會執行下列工作：</span><span class="sxs-lookup"><span data-stu-id="ed9da-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="ed9da-116">設定[Kestrel](servers/kestrel.md)做為 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="ed9da-116">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="ed9da-117">Kestrel 預設選項，請參閱[Kestrel 選項 > 一節中 ASP.NET Core Kestrel web 伺服器實作的](xref:fundamentals/servers/kestrel#kestrel-options)。</span><span class="sxs-lookup"><span data-stu-id="ed9da-117">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="ed9da-118">設定所傳回的路徑內容的根[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory)。</span><span class="sxs-lookup"><span data-stu-id="ed9da-118">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="ed9da-119">從負載選擇性設定：</span><span class="sxs-lookup"><span data-stu-id="ed9da-119">Loads optional configuration from:</span></span>
  * <span data-ttu-id="ed9da-120">*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="ed9da-120">*appsettings.json*.</span></span>
  * <span data-ttu-id="ed9da-121">*appsettings。{環境}.json*。</span><span class="sxs-lookup"><span data-stu-id="ed9da-121">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="ed9da-122">[使用者密碼](xref:security/app-secrets)的應用程式執行時`Development`環境。</span><span class="sxs-lookup"><span data-stu-id="ed9da-122">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="ed9da-123">環境變數。</span><span class="sxs-lookup"><span data-stu-id="ed9da-123">Environment variables.</span></span>
  * <span data-ttu-id="ed9da-124">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="ed9da-124">Command-line arguments.</span></span>
* <span data-ttu-id="ed9da-125">設定[記錄](xref:fundamentals/logging/index)主控台和偵錯輸出。</span><span class="sxs-lookup"><span data-stu-id="ed9da-125">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="ed9da-126">記錄將包含[記錄檔篩選](xref:fundamentals/logging/index#log-filtering)記錄組態區段中指定的規則*appsettings.json*或*appsettings。 {環境}.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="ed9da-126">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="ed9da-127">當 IIS 背景執行，可讓[IIS integration](xref:host-and-deploy/iis/index)。</span><span class="sxs-lookup"><span data-stu-id="ed9da-127">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="ed9da-128">設定基底路徑和伺服器使用時聆聽的連接埠[ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="ed9da-128">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="ed9da-129">此模組會建立 IIS 與 Kestrel 之間的反向 proxy。</span><span class="sxs-lookup"><span data-stu-id="ed9da-129">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="ed9da-130">也會設定應用程式[擷取啟動錯誤](#capture-startup-errors)。</span><span class="sxs-lookup"><span data-stu-id="ed9da-130">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="ed9da-131">對於 IIS 的預設選項，請參閱[IIS 選項 > 一節的主控件與 IIS 的 Windows 上的 ASP.NET Core](xref:host-and-deploy/iis/index#iis-options)。</span><span class="sxs-lookup"><span data-stu-id="ed9da-131">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="ed9da-132">*內容的根*決定主機會為內容檔案，例如 MVC 檢視的搜尋。</span><span class="sxs-lookup"><span data-stu-id="ed9da-132">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="ed9da-133">應用程式啟動時從專案的根資料夾，會使用專案的根資料夾，做為內容的根。</span><span class="sxs-lookup"><span data-stu-id="ed9da-133">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="ed9da-134">這是預設值用於[Visual Studio](https://www.visualstudio.com/)和[dotnet 新範本](/dotnet/core/tools/dotnet-new)。</span><span class="sxs-lookup"><span data-stu-id="ed9da-134">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="ed9da-135">如需有關應用程式組態的詳細資訊，請參閱[組態中 ASP.NET Core](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="ed9da-135">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="ed9da-136">做為使用靜態替代`CreateDefaultBuilder`方法，建立從主機[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)是支援的方法與 ASP.NET Core 2.x。</span><span class="sxs-lookup"><span data-stu-id="ed9da-136">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="ed9da-137">如需詳細資訊，請參閱 ASP.NET Core 1.x 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="ed9da-137">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ed9da-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ed9da-139">建立主應用程式使用的執行個體[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)。</span><span class="sxs-lookup"><span data-stu-id="ed9da-139">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="ed9da-140">建立主控件應用程式的進入點，通常執行`Main`方法。</span><span class="sxs-lookup"><span data-stu-id="ed9da-140">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="ed9da-141">在專案範本中，`Main`位於*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="ed9da-141">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="ed9da-142">`WebHostBuilder`需要[伺服器實作 IServer](servers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="ed9da-142">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="ed9da-143">內建的伺服器是[Kestrel](servers/kestrel.md)和[HTTP.sys](servers/httpsys.md) (ASP.NET Core 2.0 版本中之前, 已呼叫 HTTP.sys [WebListener](xref:fundamentals/servers/weblistener))。</span><span class="sxs-lookup"><span data-stu-id="ed9da-143">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="ed9da-144">在此範例中， [UseKestrel 擴充方法](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)指定 Kestrel 伺服器。</span><span class="sxs-lookup"><span data-stu-id="ed9da-144">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="ed9da-145">*內容的根*決定主機會為內容檔案，例如 MVC 檢視的搜尋。</span><span class="sxs-lookup"><span data-stu-id="ed9da-145">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="ed9da-146">取得預設內容根`UseContentRoot`由[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1)。</span><span class="sxs-lookup"><span data-stu-id="ed9da-146">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="ed9da-147">應用程式啟動時從專案的根資料夾，會使用專案的根資料夾，做為內容的根。</span><span class="sxs-lookup"><span data-stu-id="ed9da-147">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="ed9da-148">這是預設值用於[Visual Studio](https://www.visualstudio.com/)和[dotnet 新範本](/dotnet/core/tools/dotnet-new)。</span><span class="sxs-lookup"><span data-stu-id="ed9da-148">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="ed9da-149">若要使用 IIS 做為反向 proxy，呼叫[UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions)建置主應用程式的一部分。</span><span class="sxs-lookup"><span data-stu-id="ed9da-149">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="ed9da-150">`UseIISIntegration`未設定*伺服器*、 like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)沒有。</span><span class="sxs-lookup"><span data-stu-id="ed9da-150">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="ed9da-151">`UseIISIntegration`設定基底路徑和伺服器使用時聆聽的連接埠[ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)建立 Kestrel 與 IIS 之間的反向 proxy。</span><span class="sxs-lookup"><span data-stu-id="ed9da-151">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="ed9da-152">若要使用 IIS 與 ASP.NET Core`UseKestrel`和`UseIISIntegration`必須指定。</span><span class="sxs-lookup"><span data-stu-id="ed9da-152">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="ed9da-153">`UseIISIntegration`只會啟動執行 IIS 或 IIS Express 後面時。</span><span class="sxs-lookup"><span data-stu-id="ed9da-153">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="ed9da-154">如需詳細資訊，請參閱[簡介 ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)和[ASP.NET 核心模組的組態參考](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="ed9da-154">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="ed9da-155">最簡單的實作，以設定主應用程式 （和 ASP.NET Core 應用程式） 包含指定伺服器和應用程式的要求管線的組態：</span><span class="sxs-lookup"><span data-stu-id="ed9da-155">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="ed9da-156">當設定主機，[設定](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1)和[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1)可以提供的方法。</span><span class="sxs-lookup"><span data-stu-id="ed9da-156">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="ed9da-157">如果`Startup`類別已指定，則必須定義`Configure`方法。</span><span class="sxs-lookup"><span data-stu-id="ed9da-157">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="ed9da-158">如需詳細資訊，請參閱[中 ASP.NET Core 應用程式啟動](startup.md)。</span><span class="sxs-lookup"><span data-stu-id="ed9da-158">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="ed9da-159">多個呼叫`ConfigureServices`附加至另一個。</span><span class="sxs-lookup"><span data-stu-id="ed9da-159">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="ed9da-160">多個呼叫`Configure`或`UseStartup`上`WebHostBuilder`取代先前的設定。</span><span class="sxs-lookup"><span data-stu-id="ed9da-160">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="ed9da-161">主機組態值</span><span class="sxs-lookup"><span data-stu-id="ed9da-161">Host configuration values</span></span>

<span data-ttu-id="ed9da-162">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)依賴下列方法來設定主機的組態值：</span><span class="sxs-lookup"><span data-stu-id="ed9da-162">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="ed9da-163">主控件產生器設定，其中包含環境變數與格式`ASPNETCORE_{configurationKey}`。</span><span class="sxs-lookup"><span data-stu-id="ed9da-163">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="ed9da-164">例如，`ASPNETCORE_URLS`。</span><span class="sxs-lookup"><span data-stu-id="ed9da-164">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="ed9da-165">明確的方法，例如`CaptureStartupErrors`。</span><span class="sxs-lookup"><span data-stu-id="ed9da-165">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="ed9da-166">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting)和相關聯的金鑰。</span><span class="sxs-lookup"><span data-stu-id="ed9da-166">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="ed9da-167">設定的值時`UseSetting`，此值設定為字串，不論類型為何。</span><span class="sxs-lookup"><span data-stu-id="ed9da-167">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="ed9da-168">主機會使用任何選項設定值上, 一次。</span><span class="sxs-lookup"><span data-stu-id="ed9da-168">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="ed9da-169">如需詳細資訊，請參閱[正在覆寫組態](#overriding-configuration)下一節。</span><span class="sxs-lookup"><span data-stu-id="ed9da-169">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="ed9da-170">擷取啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="ed9da-170">Capture Startup Errors</span></span>

<span data-ttu-id="ed9da-171">此設定會控制擷取的啟動錯誤。</span><span class="sxs-lookup"><span data-stu-id="ed9da-171">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="ed9da-172">**索引鍵**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="ed9da-172">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="ed9da-173">**型別**: *bool* (`true`或`1`)</span><span class="sxs-lookup"><span data-stu-id="ed9da-173">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="ed9da-174">**預設**： 預設為`false`Kestrel 背後 IIS，其中預設值是以執行應用程式除非`true`。</span><span class="sxs-lookup"><span data-stu-id="ed9da-174">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="ed9da-175">**使用設定**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="ed9da-175">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="ed9da-176">**環境變數**:`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="ed9da-176">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="ed9da-177">當`false`，啟動導致主機結束期間發生的錯誤。</span><span class="sxs-lookup"><span data-stu-id="ed9da-177">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="ed9da-178">當`true`，主應用程式在啟動期間擷取例外狀況，並嘗試啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="ed9da-178">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ed9da-179">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ed9da-180">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-180">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="ed9da-181">內容的根</span><span class="sxs-lookup"><span data-stu-id="ed9da-181">Content Root</span></span>

<span data-ttu-id="ed9da-182">此設定可決定 ASP.NET Core 開始搜尋的內容檔案，例如 MVC 檢視的位置。</span><span class="sxs-lookup"><span data-stu-id="ed9da-182">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="ed9da-183">**索引鍵**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="ed9da-183">**Key**: contentRoot</span></span>  
<span data-ttu-id="ed9da-184">**型別**:*字串*</span><span class="sxs-lookup"><span data-stu-id="ed9da-184">**Type**: *string*</span></span>  
<span data-ttu-id="ed9da-185">**預設**： 預設為應用程式組件所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="ed9da-185">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="ed9da-186">**使用設定**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="ed9da-186">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="ed9da-187">**環境變數**:`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="ed9da-187">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="ed9da-188">內容的根也作為基底路徑[Web 根目錄下設定](#web-root)。</span><span class="sxs-lookup"><span data-stu-id="ed9da-188">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="ed9da-189">如果路徑不存在，就無法啟動主機。</span><span class="sxs-lookup"><span data-stu-id="ed9da-189">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ed9da-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ed9da-191">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="ed9da-192">詳細的錯誤</span><span class="sxs-lookup"><span data-stu-id="ed9da-192">Detailed Errors</span></span>

<span data-ttu-id="ed9da-193">判斷詳細的錯誤應該擷取。</span><span class="sxs-lookup"><span data-stu-id="ed9da-193">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="ed9da-194">**索引鍵**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="ed9da-194">**Key**: detailedErrors</span></span>  
<span data-ttu-id="ed9da-195">**型別**: *bool* (`true`或`1`)</span><span class="sxs-lookup"><span data-stu-id="ed9da-195">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="ed9da-196">**預設**: false</span><span class="sxs-lookup"><span data-stu-id="ed9da-196">**Default**: false</span></span>  
<span data-ttu-id="ed9da-197">**使用設定**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="ed9da-197">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="ed9da-198">**環境變數**:`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="ed9da-198">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="ed9da-199">當啟用 (或當<a href="#environment">環境</a>設`Development`)，應用程式會擷取詳細例外狀況。</span><span class="sxs-lookup"><span data-stu-id="ed9da-199">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ed9da-200">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-200">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ed9da-201">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-201">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="ed9da-202">環境</span><span class="sxs-lookup"><span data-stu-id="ed9da-202">Environment</span></span>

<span data-ttu-id="ed9da-203">設定應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="ed9da-203">Sets the app's environment.</span></span>

<span data-ttu-id="ed9da-204">**索引鍵**： 環境</span><span class="sxs-lookup"><span data-stu-id="ed9da-204">**Key**: environment</span></span>  
<span data-ttu-id="ed9da-205">**型別**:*字串*</span><span class="sxs-lookup"><span data-stu-id="ed9da-205">**Type**: *string*</span></span>  
<span data-ttu-id="ed9da-206">**預設**： 生產環境</span><span class="sxs-lookup"><span data-stu-id="ed9da-206">**Default**: Production</span></span>  
<span data-ttu-id="ed9da-207">**使用設定**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="ed9da-207">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="ed9da-208">**環境變數**:`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="ed9da-208">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="ed9da-209">環境可以設定為任何值。</span><span class="sxs-lookup"><span data-stu-id="ed9da-209">The environment can be set to any value.</span></span> <span data-ttu-id="ed9da-210">架構定義的值包括`Development`， `Staging`，和`Production`。</span><span class="sxs-lookup"><span data-stu-id="ed9da-210">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="ed9da-211">值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="ed9da-211">Values aren't case sensitive.</span></span> <span data-ttu-id="ed9da-212">根據預設，*環境*讀取從`ASPNETCORE_ENVIRONMENT`環境變數。</span><span class="sxs-lookup"><span data-stu-id="ed9da-212">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="ed9da-213">當使用[Visual Studio](https://www.visualstudio.com/)，可能會設定環境變數*launchSettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="ed9da-213">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="ed9da-214">如需詳細資訊，請參閱[使用多個環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="ed9da-214">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ed9da-215">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-215">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ed9da-216">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-216">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="ed9da-217">裝載啟動的組件</span><span class="sxs-lookup"><span data-stu-id="ed9da-217">Hosting Startup Assemblies</span></span>

<span data-ttu-id="ed9da-218">設定應用程式的裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="ed9da-218">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="ed9da-219">**索引鍵**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="ed9da-219">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="ed9da-220">**型別**:*字串*</span><span class="sxs-lookup"><span data-stu-id="ed9da-220">**Type**: *string*</span></span>  
<span data-ttu-id="ed9da-221">**預設**： 空字串</span><span class="sxs-lookup"><span data-stu-id="ed9da-221">**Default**: Empty string</span></span>  
<span data-ttu-id="ed9da-222">**使用設定**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="ed9da-222">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="ed9da-223">**環境變數**:`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="ed9da-223">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="ed9da-224">裝載啟動在啟動時載入的組件的字串，以分號分隔。</span><span class="sxs-lookup"><span data-stu-id="ed9da-224">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="ed9da-225">這項功能的新 ASP.NET Core 2.0。</span><span class="sxs-lookup"><span data-stu-id="ed9da-225">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="ed9da-226">雖然組態值會預設為空字串，則裝載的啟動組件永遠會包含應用程式的組件。</span><span class="sxs-lookup"><span data-stu-id="ed9da-226">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="ed9da-227">當未提供裝載啟動組件時，它們被加入至應用程式的組件時應用程式建置其通用服務在啟動時載入。</span><span class="sxs-lookup"><span data-stu-id="ed9da-227">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ed9da-228">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-228">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ed9da-229">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-229">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ed9da-230">這項功能已無法在 ASP.NET Core 1.x。</span><span class="sxs-lookup"><span data-stu-id="ed9da-230">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="ed9da-231">偏好裝載 Url</span><span class="sxs-lookup"><span data-stu-id="ed9da-231">Prefer Hosting URLs</span></span>

<span data-ttu-id="ed9da-232">表示主機是否應接聽設定的 Url`WebHostBuilder`而不是與`IServer`實作。</span><span class="sxs-lookup"><span data-stu-id="ed9da-232">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="ed9da-233">**索引鍵**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="ed9da-233">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="ed9da-234">**型別**: *bool* (`true`或`1`)</span><span class="sxs-lookup"><span data-stu-id="ed9da-234">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="ed9da-235">**預設**: true</span><span class="sxs-lookup"><span data-stu-id="ed9da-235">**Default**: true</span></span>  
<span data-ttu-id="ed9da-236">**使用設定**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="ed9da-236">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="ed9da-237">**環境變數**:`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="ed9da-237">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="ed9da-238">這項功能的新 ASP.NET Core 2.0。</span><span class="sxs-lookup"><span data-stu-id="ed9da-238">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ed9da-239">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-239">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ed9da-240">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-240">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ed9da-241">這項功能已無法在 ASP.NET Core 1.x。</span><span class="sxs-lookup"><span data-stu-id="ed9da-241">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="ed9da-242">防止裝載啟動</span><span class="sxs-lookup"><span data-stu-id="ed9da-242">Prevent Hosting Startup</span></span>

<span data-ttu-id="ed9da-243">可防止自動載入裝載啟動的組件，包括裝載應用程式的組件所設定的啟動組件。</span><span class="sxs-lookup"><span data-stu-id="ed9da-243">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="ed9da-244">請參閱[使用 IHostingStartup 外部組件加入應用程式功能](xref:host-and-deploy/ihostingstartup)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="ed9da-244">See [Add app features from an external assembly using IHostingStartup](xref:host-and-deploy/ihostingstartup) for more information.</span></span>

<span data-ttu-id="ed9da-245">**索引鍵**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="ed9da-245">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="ed9da-246">**型別**: *bool* (`true`或`1`)</span><span class="sxs-lookup"><span data-stu-id="ed9da-246">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="ed9da-247">**預設**: false</span><span class="sxs-lookup"><span data-stu-id="ed9da-247">**Default**: false</span></span>  
<span data-ttu-id="ed9da-248">**使用設定**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="ed9da-248">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="ed9da-249">**環境變數**:`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="ed9da-249">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="ed9da-250">這項功能的新 ASP.NET Core 2.0。</span><span class="sxs-lookup"><span data-stu-id="ed9da-250">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ed9da-251">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-251">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ed9da-252">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-252">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ed9da-253">這項功能已無法在 ASP.NET Core 1.x。</span><span class="sxs-lookup"><span data-stu-id="ed9da-253">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="ed9da-254">伺服器 Url</span><span class="sxs-lookup"><span data-stu-id="ed9da-254">Server URLs</span></span>

<span data-ttu-id="ed9da-255">表示 IP 位址或連接埠和通訊協定，伺服器應該接聽之要求的主機位址。</span><span class="sxs-lookup"><span data-stu-id="ed9da-255">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="ed9da-256">**索引鍵**: url</span><span class="sxs-lookup"><span data-stu-id="ed9da-256">**Key**: urls</span></span>  
<span data-ttu-id="ed9da-257">**型別**:*字串*</span><span class="sxs-lookup"><span data-stu-id="ed9da-257">**Type**: *string*</span></span>  
<span data-ttu-id="ed9da-258">**預設**: http://localhost:5000/</span><span class="sxs-lookup"><span data-stu-id="ed9da-258">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="ed9da-259">**使用設定**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="ed9da-259">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="ed9da-260">**環境變數**:`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="ed9da-260">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="ed9da-261">設定為以分號分隔 （;） 應該回應伺服器的前置詞的 URL 清單。</span><span class="sxs-lookup"><span data-stu-id="ed9da-261">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="ed9da-262">例如，`http://localhost:123`。</span><span class="sxs-lookup"><span data-stu-id="ed9da-262">For example, `http://localhost:123`.</span></span> <span data-ttu-id="ed9da-263">使用 「\*"，表示伺服器應接聽任何 IP 位址或主機名稱使用指定的連接埠和通訊協定上的要求 (例如， `http://*:5000`)。</span><span class="sxs-lookup"><span data-stu-id="ed9da-263">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="ed9da-264">通訊協定 (`http://`或`https://`) 必須包含以每個 URL。</span><span class="sxs-lookup"><span data-stu-id="ed9da-264">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="ed9da-265">伺服器之間的不支援的格式。</span><span class="sxs-lookup"><span data-stu-id="ed9da-265">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ed9da-266">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-266">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="ed9da-267">Kestrel 有它自己的端點設定應用程式開發介面。</span><span class="sxs-lookup"><span data-stu-id="ed9da-267">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="ed9da-268">如需詳細資訊，請參閱[在 ASP.NET Core 中實作 Kestrel 網頁伺服器](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)。</span><span class="sxs-lookup"><span data-stu-id="ed9da-268">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ed9da-269">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-269">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="ed9da-270">關閉逾時</span><span class="sxs-lookup"><span data-stu-id="ed9da-270">Shutdown Timeout</span></span>

<span data-ttu-id="ed9da-271">指定 web 主機關閉的時間量。</span><span class="sxs-lookup"><span data-stu-id="ed9da-271">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="ed9da-272">**索引鍵**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="ed9da-272">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="ed9da-273">**型別**: *int*</span><span class="sxs-lookup"><span data-stu-id="ed9da-273">**Type**: *int*</span></span>  
<span data-ttu-id="ed9da-274">**預設**: 5</span><span class="sxs-lookup"><span data-stu-id="ed9da-274">**Default**: 5</span></span>  
<span data-ttu-id="ed9da-275">**使用設定**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="ed9da-275">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="ed9da-276">**環境變數**:`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="ed9da-276">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="ed9da-277">雖然索引鍵接受*int*與`UseSetting`(例如， `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`)、`UseShutdownTimeout`擴充方法會採用`TimeSpan`。</span><span class="sxs-lookup"><span data-stu-id="ed9da-277">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="ed9da-278">這項功能的新 ASP.NET Core 2.0。</span><span class="sxs-lookup"><span data-stu-id="ed9da-278">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ed9da-279">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-279">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ed9da-280">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-280">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ed9da-281">這項功能已無法在 ASP.NET Core 1.x。</span><span class="sxs-lookup"><span data-stu-id="ed9da-281">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="ed9da-282">啟動組件</span><span class="sxs-lookup"><span data-stu-id="ed9da-282">Startup Assembly</span></span>

<span data-ttu-id="ed9da-283">決定要搜尋的組件`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="ed9da-283">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="ed9da-284">**索引鍵**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="ed9da-284">**Key**: startupAssembly</span></span>  
<span data-ttu-id="ed9da-285">**型別**:*字串*</span><span class="sxs-lookup"><span data-stu-id="ed9da-285">**Type**: *string*</span></span>  
<span data-ttu-id="ed9da-286">**預設**： 應用程式的組件</span><span class="sxs-lookup"><span data-stu-id="ed9da-286">**Default**: The app's assembly</span></span>  
<span data-ttu-id="ed9da-287">**使用設定**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="ed9da-287">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="ed9da-288">**環境變數**:`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="ed9da-288">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="ed9da-289">依名稱的組件 (`string`) 或型別 (`TStartup`) 可以參考。</span><span class="sxs-lookup"><span data-stu-id="ed9da-289">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="ed9da-290">若為多個`UseStartup`呼叫的方法，最後一個的優先順序。</span><span class="sxs-lookup"><span data-stu-id="ed9da-290">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ed9da-291">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-291">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ed9da-292">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-292">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="ed9da-293">Web 根目錄</span><span class="sxs-lookup"><span data-stu-id="ed9da-293">Web Root</span></span>

<span data-ttu-id="ed9da-294">設定應用程式的靜態資產的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="ed9da-294">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="ed9da-295">**索引鍵**: webroot</span><span class="sxs-lookup"><span data-stu-id="ed9da-295">**Key**: webroot</span></span>  
<span data-ttu-id="ed9da-296">**型別**:*字串*</span><span class="sxs-lookup"><span data-stu-id="ed9da-296">**Type**: *string*</span></span>  
<span data-ttu-id="ed9da-297">**預設**： 如果未指定，預設值是"(Content Root)/wwwroot"，則該路徑存在。</span><span class="sxs-lookup"><span data-stu-id="ed9da-297">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="ed9da-298">如果路徑不存在，則會使用任何作業檔案提供者。</span><span class="sxs-lookup"><span data-stu-id="ed9da-298">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="ed9da-299">**使用設定**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="ed9da-299">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="ed9da-300">**環境變數**:`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="ed9da-300">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ed9da-301">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-301">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ed9da-302">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-302">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="ed9da-303">覆寫設定</span><span class="sxs-lookup"><span data-stu-id="ed9da-303">Overriding configuration</span></span>

<span data-ttu-id="ed9da-304">使用[組態](xref:fundamentals/configuration/index)設定主控件。</span><span class="sxs-lookup"><span data-stu-id="ed9da-304">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="ed9da-305">在下列範例中，主機設定 （選擇性） 指定於*hosting.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="ed9da-305">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="ed9da-306">從載入任何組態*hosting.json*命令列引數可能會覆寫檔案。</span><span class="sxs-lookup"><span data-stu-id="ed9da-306">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="ed9da-307">內建的設定 (在`config`) 用來設定與主機`UseConfiguration`。</span><span class="sxs-lookup"><span data-stu-id="ed9da-307">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ed9da-308">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-308">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ed9da-309">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="ed9da-309">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="ed9da-310">覆寫所提供的組態`UseUrls`與*hosting.json*設定第一個命令列引數設定第二個：</span><span class="sxs-lookup"><span data-stu-id="ed9da-310">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ed9da-311">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-311">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ed9da-312">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="ed9da-312">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="ed9da-313">覆寫所提供的組態`UseUrls`與*hosting.json*設定第一個命令列引數設定第二個：</span><span class="sxs-lookup"><span data-stu-id="ed9da-313">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="ed9da-314">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration)擴充方法不是目前可以剖析所傳回的組態區段`GetSection`(例如， `.UseConfiguration(Configuration.GetSection("section"))`。</span><span class="sxs-lookup"><span data-stu-id="ed9da-314">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="ed9da-315">`GetSection`方法篩選到要求的區段之組態機碼，但會保留的區段名稱索引鍵 (例如， `section:urls`， `section:environment`)。</span><span class="sxs-lookup"><span data-stu-id="ed9da-315">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="ed9da-316">`UseConfiguration`方法預期要比對的索引鍵`WebHostBuilder`索引鍵 (例如， `urls`， `environment`)。</span><span class="sxs-lookup"><span data-stu-id="ed9da-316">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="ed9da-317">索引鍵的區段名稱存在主控件設定防止區段的值。</span><span class="sxs-lookup"><span data-stu-id="ed9da-317">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="ed9da-318">這個問題將在近期的版本中解決。</span><span class="sxs-lookup"><span data-stu-id="ed9da-318">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="ed9da-319">如需詳細資訊和因應措施，請參閱[傳遞到 WebHostBuilder.UseConfiguration 組態區段會使用完整金鑰](https://github.com/aspnet/Hosting/issues/839)。</span><span class="sxs-lookup"><span data-stu-id="ed9da-319">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="ed9da-320">若要指定特定 URL 上所執行的主機，所要的值中可傳遞的命令提示字元執行時`dotnet run`。</span><span class="sxs-lookup"><span data-stu-id="ed9da-320">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="ed9da-321">命令列引數會覆寫`urls`值*hosting.json*檔和伺服器會接聽連接埠 8080:</span><span class="sxs-lookup"><span data-stu-id="ed9da-321">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="ed9da-322">正在啟動主機</span><span class="sxs-lookup"><span data-stu-id="ed9da-322">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ed9da-323">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-323">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ed9da-324">**執行**</span><span class="sxs-lookup"><span data-stu-id="ed9da-324">**Run**</span></span>

<span data-ttu-id="ed9da-325">`Run`方法會啟動 web 應用程式，並且封鎖呼叫執行緒，直到主機已關閉：</span><span class="sxs-lookup"><span data-stu-id="ed9da-325">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="ed9da-326">**Start**</span><span class="sxs-lookup"><span data-stu-id="ed9da-326">**Start**</span></span>

<span data-ttu-id="ed9da-327">主機執行的非封鎖方式，藉由呼叫其`Start`方法：</span><span class="sxs-lookup"><span data-stu-id="ed9da-327">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="ed9da-328">如果 Url 的清單傳遞至`Start`方法，它會接聽指定的 Url:</span><span class="sxs-lookup"><span data-stu-id="ed9da-328">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="ed9da-329">應用程式可以初始化和啟動新的主機使用的預先設定的預設值`CreateDefaultBuilder`使用靜態便利的方法。</span><span class="sxs-lookup"><span data-stu-id="ed9da-329">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="ed9da-330">啟動伺服器未主控台輸出的情況下，這些方法[WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown)等候中斷 （Ctrl-C/SIGINT 或 SIGTERM）：</span><span class="sxs-lookup"><span data-stu-id="ed9da-330">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="ed9da-331">**開始 （RequestDelegate 應用程式）**</span><span class="sxs-lookup"><span data-stu-id="ed9da-331">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="ed9da-332">開頭`RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="ed9da-332">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="ed9da-333">若要在瀏覽器中提出要求`http://localhost:5000`接收"Hello World ！"的回應</span><span class="sxs-lookup"><span data-stu-id="ed9da-333">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="ed9da-334">`WaitForShutdown`直到發出中斷 （Ctrl-C/SIGINT 或 SIGTERM） 的區塊。</span><span class="sxs-lookup"><span data-stu-id="ed9da-334">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="ed9da-335">應用程式會顯示`Console.WriteLine`訊息並等候 keypress 結束。</span><span class="sxs-lookup"><span data-stu-id="ed9da-335">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="ed9da-336">**開始 （以字串 url、 RequestDelegate 應用程式）**</span><span class="sxs-lookup"><span data-stu-id="ed9da-336">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="ed9da-337">URL 的開頭和`RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="ed9da-337">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="ed9da-338">會產生相同結果**開始 （RequestDelegate 應用程式）**，除了應用程式回應`http://localhost:8080`。</span><span class="sxs-lookup"><span data-stu-id="ed9da-338">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="ed9da-339">**啟動 (動作<IRouteBuilder>routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="ed9da-339">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="ed9da-340">使用的執行個體`IRouteBuilder`([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) 使用路由的中介軟體：</span><span class="sxs-lookup"><span data-stu-id="ed9da-340">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="ed9da-341">使用下列瀏覽器要求範例：</span><span class="sxs-lookup"><span data-stu-id="ed9da-341">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="ed9da-342">要求</span><span class="sxs-lookup"><span data-stu-id="ed9da-342">Request</span></span>                                    | <span data-ttu-id="ed9da-343">回應</span><span class="sxs-lookup"><span data-stu-id="ed9da-343">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="ed9da-344">Hello，Martin ！</span><span class="sxs-lookup"><span data-stu-id="ed9da-344">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="ed9da-345">Catrina 布宜諾 dias ！</span><span class="sxs-lookup"><span data-stu-id="ed9da-345">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="ed9da-346">擲回例外狀況的字串"ooops ！ 」</span><span class="sxs-lookup"><span data-stu-id="ed9da-346">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="ed9da-347">擲回例外狀況的字串"Uh 喔 ！ 」</span><span class="sxs-lookup"><span data-stu-id="ed9da-347">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="ed9da-348">Sante，Kevin ！</span><span class="sxs-lookup"><span data-stu-id="ed9da-348">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="ed9da-349">Hello World!</span><span class="sxs-lookup"><span data-stu-id="ed9da-349">Hello World!</span></span>                             |

<span data-ttu-id="ed9da-350">`WaitForShutdown`直到發出中斷 （Ctrl-C/SIGINT 或 SIGTERM） 的區塊。</span><span class="sxs-lookup"><span data-stu-id="ed9da-350">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="ed9da-351">應用程式會顯示`Console.WriteLine`訊息並等候 keypress 結束。</span><span class="sxs-lookup"><span data-stu-id="ed9da-351">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="ed9da-352">**啟動 (string url，動作<IRouteBuilder>routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="ed9da-352">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="ed9da-353">使用 URL 和執行個體`IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="ed9da-353">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="ed9da-354">會產生相同結果**開始 (動作<IRouteBuilder>routeBuilder)**，除了應用程式會回應在`http://localhost:8080`。</span><span class="sxs-lookup"><span data-stu-id="ed9da-354">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="ed9da-355">**StartWith (動作<IApplicationBuilder>應用程式)**</span><span class="sxs-lookup"><span data-stu-id="ed9da-355">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="ed9da-356">提供設定委派`IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="ed9da-356">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="ed9da-357">若要在瀏覽器中提出要求`http://localhost:5000`接收"Hello World ！"的回應</span><span class="sxs-lookup"><span data-stu-id="ed9da-357">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="ed9da-358">`WaitForShutdown`直到發出中斷 （Ctrl-C/SIGINT 或 SIGTERM） 的區塊。</span><span class="sxs-lookup"><span data-stu-id="ed9da-358">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="ed9da-359">應用程式會顯示`Console.WriteLine`訊息並等候 keypress 結束。</span><span class="sxs-lookup"><span data-stu-id="ed9da-359">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="ed9da-360">**StartWith (string url，動作<IApplicationBuilder>應用程式)**</span><span class="sxs-lookup"><span data-stu-id="ed9da-360">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="ed9da-361">提供 URL，若要設定委派`IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="ed9da-361">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="ed9da-362">會產生相同結果**StartWith (動作<IApplicationBuilder>應用程式)**，除了應用程式回應`http://localhost:8080`。</span><span class="sxs-lookup"><span data-stu-id="ed9da-362">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ed9da-363">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ed9da-363">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ed9da-364">**執行**</span><span class="sxs-lookup"><span data-stu-id="ed9da-364">**Run**</span></span>

<span data-ttu-id="ed9da-365">`Run`方法會啟動 web 應用程式，並且封鎖呼叫執行緒，直到主機關機：</span><span class="sxs-lookup"><span data-stu-id="ed9da-365">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="ed9da-366">**Start**</span><span class="sxs-lookup"><span data-stu-id="ed9da-366">**Start**</span></span>

<span data-ttu-id="ed9da-367">主機執行的非封鎖方式，藉由呼叫其`Start`方法：</span><span class="sxs-lookup"><span data-stu-id="ed9da-367">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="ed9da-368">如果 Url 的清單傳遞至`Start`方法，它會接聽指定的 Url:</span><span class="sxs-lookup"><span data-stu-id="ed9da-368">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="ed9da-369">IHostingEnvironment 介面</span><span class="sxs-lookup"><span data-stu-id="ed9da-369">IHostingEnvironment interface</span></span>

<span data-ttu-id="ed9da-370">[IHostingEnvironment 介面](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment)提供應用程式的 web 主機環境的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="ed9da-370">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="ed9da-371">使用[建構函式插入](xref:fundamentals/dependency-injection)取得`IHostingEnvironment`才能使用其屬性和擴充方法：</span><span class="sxs-lookup"><span data-stu-id="ed9da-371">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="ed9da-372">A[慣例為基礎的方法](xref:fundamentals/environments#startup-conventions)可用來根據環境的啟動時設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="ed9da-372">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="ed9da-373">或者，插入`IHostingEnvironment`到`Startup`建構函式以用於`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ed9da-373">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="ed9da-374">除了`IsDevelopment`擴充方法，`IHostingEnvironment`提供`IsStaging`， `IsProduction`，和`IsEnvironment(string environmentName)`方法。</span><span class="sxs-lookup"><span data-stu-id="ed9da-374">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="ed9da-375">請參閱[使用多個環境](xref:fundamentals/environments)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="ed9da-375">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="ed9da-376">`IHostingEnvironment`服務也可直接插入`Configure`設定處理管線的方法：</span><span class="sxs-lookup"><span data-stu-id="ed9da-376">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="ed9da-377">`IHostingEnvironment`可以插入到`Invoke`方法時建立自訂[中介軟體](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="ed9da-377">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="ed9da-378">IApplicationLifetime 介面</span><span class="sxs-lookup"><span data-stu-id="ed9da-378">IApplicationLifetime interface</span></span>

<span data-ttu-id="ed9da-379">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime)允許後啟動和關閉的活動。</span><span class="sxs-lookup"><span data-stu-id="ed9da-379">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="ed9da-380">在介面上的三個屬性是用來註冊的取消語彙基元`Action`定義啟動和關閉事件的方法。</span><span class="sxs-lookup"><span data-stu-id="ed9da-380">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span> <span data-ttu-id="ed9da-381">另外還有`StopApplication`方法。</span><span class="sxs-lookup"><span data-stu-id="ed9da-381">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="ed9da-382">取消語彙基元</span><span class="sxs-lookup"><span data-stu-id="ed9da-382">Cancellation Token</span></span>    | <span data-ttu-id="ed9da-383">觸發時 &#8230;</span><span class="sxs-lookup"><span data-stu-id="ed9da-383">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="ed9da-384">已完全啟動主機。</span><span class="sxs-lookup"><span data-stu-id="ed9da-384">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="ed9da-385">主機正在執行正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="ed9da-385">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="ed9da-386">可能仍在處理要求。</span><span class="sxs-lookup"><span data-stu-id="ed9da-386">Requests may still be processing.</span></span> <span data-ttu-id="ed9da-387">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="ed9da-387">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="ed9da-388">主應用程式即將完成正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="ed9da-388">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="ed9da-389">應該處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="ed9da-389">All requests should be processed.</span></span> <span data-ttu-id="ed9da-390">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="ed9da-390">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="ed9da-391">方法</span><span class="sxs-lookup"><span data-stu-id="ed9da-391">Method</span></span>            | <span data-ttu-id="ed9da-392">動作</span><span class="sxs-lookup"><span data-stu-id="ed9da-392">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="ed9da-393">目前的應用程式要求終止。</span><span class="sxs-lookup"><span data-stu-id="ed9da-393">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="ed9da-394">疑難排解 System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="ed9da-394">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="ed9da-395">**ASP.NET Core 2.0 只適用於**</span><span class="sxs-lookup"><span data-stu-id="ed9da-395">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="ed9da-396">主機可能會建立將，以便`IStartup`直接將相依性插入容器，而不是呼叫`UseStartup`或`Configure`:</span><span class="sxs-lookup"><span data-stu-id="ed9da-396">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="ed9da-397">如果主應用程式建立的如此一來，可能會發生下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="ed9da-397">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="ed9da-398">這是因為[applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) （目前的組件），才能掃描`HostingStartupAttributes`。</span><span class="sxs-lookup"><span data-stu-id="ed9da-398">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="ed9da-399">如果應用程式以手動方式插入`IStartup`到相依性插入容器中，加入下列呼叫`WebHostBuilder`具有所指定的組件名稱：</span><span class="sxs-lookup"><span data-stu-id="ed9da-399">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="ed9da-400">或者，新增假`Configure`至`WebHostBuilder`，將`applicationName`(`ApplicationKey`) 自動：</span><span class="sxs-lookup"><span data-stu-id="ed9da-400">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="ed9da-401">**請注意**： 時，這只需要 ASP.NET Core 2.0 版，而且只應用程式不會呼叫`UseStartup`或`Configure`。</span><span class="sxs-lookup"><span data-stu-id="ed9da-401">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="ed9da-402">如需詳細資訊，請參閱[公告： Microsoft.Extensions.PlatformAbstractions 已移除 （註解）](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938)和[StartupInjection 範例](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs)。</span><span class="sxs-lookup"><span data-stu-id="ed9da-402">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ed9da-403">其他資源</span><span class="sxs-lookup"><span data-stu-id="ed9da-403">Additional resources</span></span>

* [<span data-ttu-id="ed9da-404"> Windows 上使用 IIS 的主機</span><span class="sxs-lookup"><span data-stu-id="ed9da-404">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="ed9da-405">Linux 上使用 Nginx 的主機</span><span class="sxs-lookup"><span data-stu-id="ed9da-405">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="ed9da-406">Linux 上使用 Apache 的主機</span><span class="sxs-lookup"><span data-stu-id="ed9da-406">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="ed9da-407">在 Windows 服務的主機</span><span class="sxs-lookup"><span data-stu-id="ed9da-407">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
