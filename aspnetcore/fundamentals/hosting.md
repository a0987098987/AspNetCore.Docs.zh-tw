---
title: ASP.NET Core 中的裝載
author: guardrex
description: 深入了解 ASP.NET Core 中的 Web 主機，其負責啟動應用程式以及管理存留期。
manager: wpickett
ms.author: riande
ms.date: 09/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosting
ms.openlocfilehash: 344bf5f0917f4c33d67eeb14176ff2aae3ae75da
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="b0cb9-103">ASP.NET Core 中的裝載</span><span class="sxs-lookup"><span data-stu-id="b0cb9-103">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="b0cb9-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b0cb9-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b0cb9-105">ASP.NET Core 應用程式會設定並啟動「主機」。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="b0cb9-106">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="b0cb9-107">至少，主機會設定伺服器和要求處理管線。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="b0cb9-108">設定主機</span><span class="sxs-lookup"><span data-stu-id="b0cb9-108">Setting up a host</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b0cb9-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="b0cb9-110">使用 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 的執行個體建立主機。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="b0cb9-111">這通常在應用程式的進入點執行，也就是 `Main` 方法。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-111">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="b0cb9-112">在專案範本中，`Main` 位於 *Program.cs*。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="b0cb9-113">一般 *Program.cs* 會呼叫 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 以開始設定主機：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="b0cb9-114">`CreateDefaultBuilder` 會執行下列工作：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="b0cb9-115">設定 [Kestrel](servers/kestrel.md) 作為網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="b0cb9-116">如需 Kestrel 預設選項，請參閱 [ASP.NET Core 中的 Kestrel 網頁伺服器實作的 Kestrel 選項一節](xref:fundamentals/servers/kestrel#kestrel-options)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="b0cb9-117">設定 [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) 所傳回路徑的內容根目錄。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-117">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="b0cb9-118">從下列位置載入選擇性組態：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="b0cb9-119">*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="b0cb9-120">*appsettings.{Environment}.json*</span><span class="sxs-lookup"><span data-stu-id="b0cb9-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="b0cb9-121">應用程式在 `Development` 環境中執行時的[使用者密碼](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="b0cb9-122">環境變數。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-122">Environment variables.</span></span>
  * <span data-ttu-id="b0cb9-123">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-123">Command-line arguments.</span></span>
* <span data-ttu-id="b0cb9-124">設定主控台和偵錯輸出的[記錄](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="b0cb9-125">記錄包含 *appsettings.json* 或 *appsettings.{Environment}.json* 檔案的記錄組態區段中指定的[記錄檔篩選](xref:fundamentals/logging/index#log-filtering)規則。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-125">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="b0cb9-126">在 IIS 背後執行時，啟用 [IIS 整合](xref:host-and-deploy/iis/index)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-126">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="b0cb9-127">設定使用 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)時伺服器接聽的基底路徑和連接埠。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-127">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="b0cb9-128">此模組會在 IIS 和 Kestrel 之間建立反向 Proxy。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-128">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="b0cb9-129">也會設定應用程式以[擷取啟動錯誤](#capture-startup-errors)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-129">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="b0cb9-130">如需 IIS 預設選項，請參閱[使用 IIS 在 Windows 上裝載 ASP.NET Core 的 IIS 選項一節](xref:host-and-deploy/iis/index#iis-options)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-130">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>
* <span data-ttu-id="b0cb9-131">如果應用程式的環境是「開發」，請將 [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) 設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-131">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="b0cb9-132">如需詳細資訊，請參閱[範圍驗證](#scope-validation)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-132">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="b0cb9-133">「內容根目錄」會決定主機搜尋內容檔案 (例如 MVC 檢視檔案) 的位置。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-133">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="b0cb9-134">從專案的根資料夾啟動應用程式時，會使用專案的根資料夾作為內容根目錄。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-134">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="b0cb9-135">這是 [Visual Studio](https://www.visualstudio.com/) 和 [dotnet 新範本](/dotnet/core/tools/dotnet-new)中使用的預設值。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-135">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="b0cb9-136">如需應用程式設定的詳細資訊，請參閱 [ASP.NET Core 中的組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-136">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="b0cb9-137">作為使用靜態 `CreateDefaultBuilder` 方法的替代做法，ASP.NET Core 2.x 支援從 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 建立主機的方法。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-137">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="b0cb9-138">如需詳細資訊，請參閱 ASP.NET Core 1.x 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-138">For more information, see the ASP.NET Core 1.x tab.</span></span>

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b0cb9-139">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-139">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="b0cb9-140">使用 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 的執行個體建立主機。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-140">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="b0cb9-141">建立主機通常在應用程式的進入點執行，也就是 `Main` 方法。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-141">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="b0cb9-142">在專案範本中，`Main` 位於 *Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-142">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="b0cb9-143">`WebHostBuilder` 需要[實作 IServer 的伺服器](servers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-143">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="b0cb9-144">內建的伺服器是 [Kestrel](servers/kestrel.md) 和 [HTTP.sys](servers/httpsys.md) (在 ASP.NET Core 2.0 版之前，HTTP.sys 稱為 [WebListener](xref:fundamentals/servers/weblistener))。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-144">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="b0cb9-145">在此範例中，[UseKestrel 擴充方法](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)會指定 Kestrel 伺服器。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-145">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="b0cb9-146">「內容根目錄」會決定主機搜尋內容檔案 (例如 MVC 檢視檔案) 的位置。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-146">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="b0cb9-147">[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1) 會取得 `UseContentRoot` 的預設內容根目錄。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-147">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="b0cb9-148">從專案的根資料夾啟動應用程式時，會使用專案的根資料夾作為內容根目錄。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-148">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="b0cb9-149">這是 [Visual Studio](https://www.visualstudio.com/) 和 [dotnet 新範本](/dotnet/core/tools/dotnet-new)中使用的預設值。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-149">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="b0cb9-150">若要使用 IIS 作為反向 Proxy，請在建置主機時呼叫 [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-150">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="b0cb9-151">`UseIISIntegration` 不會像 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)那樣設定「伺服器」。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-151">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="b0cb9-152">`UseIISIntegration` 會設定使用 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)以在 Kestrel 和 IIS 之間建立反向 Proxy 時，伺服器接聽的基底路徑和連接埠。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-152">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="b0cb9-153">若要搭配使用 IIS 與 ASP.NET Core，必須指定 `UseKestrel` 和 `UseIISIntegration`。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-153">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="b0cb9-154">`UseIISIntegration` 只會在 IIS 或 IIS Express 背後執行時啟動。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-154">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="b0cb9-155">如需詳細資訊，請參閱 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)和 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-155">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="b0cb9-156">設定主機 (和 ASP.NET Core 應用程式) 的最小實作，包含指定伺服器和應用程式要求管線的組態：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-156">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

* * *
<span data-ttu-id="b0cb9-157">設定主機時，可以提供 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) 和 [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) 方法。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-157">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="b0cb9-158">如果指定 `Startup` 類別，它必須定義 `Configure` 方法。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-158">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="b0cb9-159">如需詳細資訊，請參閱 [ASP.NET Core 中的應用程式啟動](startup.md)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-159">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="b0cb9-160">多次呼叫 `ConfigureServices` 會彼此附加。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-160">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="b0cb9-161">對 `WebHostBuilder` 多次呼叫 `Configure` 或 `UseStartup` 則會取代先前的設定。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-161">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="b0cb9-162">主機組態值</span><span class="sxs-lookup"><span data-stu-id="b0cb9-162">Host configuration values</span></span>

<span data-ttu-id="b0cb9-163">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 依賴下列方法來設定主機組態值：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-163">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="b0cb9-164">主機建立器組態，其中包含 `ASPNETCORE_{configurationKey}` 格式的環境變數。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-164">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="b0cb9-165">例如，`ASPNETCORE_URLS`。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-165">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="b0cb9-166">明確的方法，例如 `CaptureStartupErrors`。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-166">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="b0cb9-167">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) 和相關聯的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-167">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="b0cb9-168">使用 `UseSetting` 設定值時，此值會設定為字串，而不論其類型為何。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-168">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="b0cb9-169">主機使用設定最後一個值的任何選項。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-169">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="b0cb9-170">如需詳細資訊，請參閱下一節的[覆寫設定](#overriding-configuration)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-170">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="b0cb9-171">擷取啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="b0cb9-171">Capture Startup Errors</span></span>

<span data-ttu-id="b0cb9-172">此設定會控制啟動錯誤的擷取。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-172">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="b0cb9-173">**索引鍵**：captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="b0cb9-173">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="b0cb9-174">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="b0cb9-174">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="b0cb9-175">**預設值**：預設為 `false`，除非應用程式執行時在 IIS 背後有 Kestrel，此時預設值即為 `true`。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-175">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="b0cb9-176">**設定使用**：`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="b0cb9-176">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="b0cb9-177">**環境變數**：`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="b0cb9-177">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="b0cb9-178">當它為 `false` 時，啟動期間發生的錯誤會導致主機結束。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-178">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="b0cb9-179">當它為 `true` 時，主機會擷取啟動期間的例外狀況，並嘗試啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-179">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b0cb9-180">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-180">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b0cb9-181">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-181">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="b0cb9-182">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="b0cb9-182">Content Root</span></span>

<span data-ttu-id="b0cb9-183">此設定可決定 ASP.NET Core 開始搜尋內容檔案 (例如 MVC 檢視) 的位置。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-183">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="b0cb9-184">**索引鍵**：contentRoot</span><span class="sxs-lookup"><span data-stu-id="b0cb9-184">**Key**: contentRoot</span></span>  
<span data-ttu-id="b0cb9-185">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="b0cb9-185">**Type**: *string*</span></span>  
<span data-ttu-id="b0cb9-186">**預設值**：預設為應用程式組件所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-186">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="b0cb9-187">**設定使用**：`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="b0cb9-187">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="b0cb9-188">**環境變數**：`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="b0cb9-188">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="b0cb9-189">內容根目錄也作為 [Web 根目錄設定](#web-root)的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-189">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="b0cb9-190">如果路徑不存在，就無法啟動主機。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-190">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b0cb9-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b0cb9-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="b0cb9-193">詳細錯誤</span><span class="sxs-lookup"><span data-stu-id="b0cb9-193">Detailed Errors</span></span>

<span data-ttu-id="b0cb9-194">決定是否應該擷取詳細的錯誤。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-194">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="b0cb9-195">**索引鍵**：detailedErrors</span><span class="sxs-lookup"><span data-stu-id="b0cb9-195">**Key**: detailedErrors</span></span>  
<span data-ttu-id="b0cb9-196">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="b0cb9-196">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="b0cb9-197">**預設值**：false</span><span class="sxs-lookup"><span data-stu-id="b0cb9-197">**Default**: false</span></span>  
<span data-ttu-id="b0cb9-198">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="b0cb9-198">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="b0cb9-199">**環境變數**：`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="b0cb9-199">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="b0cb9-200">啟用時 (或當 <a href="#environment">Environment</a> 設定為 `Development` 時)，應用程式會擷取詳細例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-200">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b0cb9-201">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-201">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b0cb9-202">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-202">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="b0cb9-203">環境</span><span class="sxs-lookup"><span data-stu-id="b0cb9-203">Environment</span></span>

<span data-ttu-id="b0cb9-204">設定應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-204">Sets the app's environment.</span></span>

<span data-ttu-id="b0cb9-205">**索引鍵**：environment</span><span class="sxs-lookup"><span data-stu-id="b0cb9-205">**Key**: environment</span></span>  
<span data-ttu-id="b0cb9-206">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="b0cb9-206">**Type**: *string*</span></span>  
<span data-ttu-id="b0cb9-207">**預設值**：Production</span><span class="sxs-lookup"><span data-stu-id="b0cb9-207">**Default**: Production</span></span>  
<span data-ttu-id="b0cb9-208">**設定使用**：`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="b0cb9-208">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="b0cb9-209">**環境變數**：`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="b0cb9-209">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="b0cb9-210">環境可以設定為任何值。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-210">The environment can be set to any value.</span></span> <span data-ttu-id="b0cb9-211">架構定義的值包括 `Development`、`Staging` 和 `Production`。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-211">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="b0cb9-212">值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-212">Values aren't case sensitive.</span></span> <span data-ttu-id="b0cb9-213">根據預設，*Environment* 是從 `ASPNETCORE_ENVIRONMENT` 環境變數讀取。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-213">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="b0cb9-214">使用 [Visual Studio](https://www.visualstudio.com/) 時，可能會在 *launchSettings.json* 檔案設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-214">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="b0cb9-215">如需詳細資訊，請參閱[使用多個環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-215">For more information, see [Work with multiple environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b0cb9-216">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-216">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b0cb9-217">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-217">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="b0cb9-218">裝載啟動組件</span><span class="sxs-lookup"><span data-stu-id="b0cb9-218">Hosting Startup Assemblies</span></span>

<span data-ttu-id="b0cb9-219">設定應用程式的裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-219">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="b0cb9-220">**索引鍵**：hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="b0cb9-220">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="b0cb9-221">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="b0cb9-221">**Type**: *string*</span></span>  
<span data-ttu-id="b0cb9-222">**預設值**：空字串</span><span class="sxs-lookup"><span data-stu-id="b0cb9-222">**Default**: Empty string</span></span>  
<span data-ttu-id="b0cb9-223">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="b0cb9-223">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="b0cb9-224">**環境變數**：`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="b0cb9-224">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="b0cb9-225">在啟動時載入的裝載啟動組件字串，以分號分隔。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-225">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="b0cb9-226">這項功能是在 ASP.NET Core 2.0 中新增。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-226">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="b0cb9-227">雖然設定值會預設為空字串，但裝載啟動組件一律會包含應用程式的組件。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-227">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="b0cb9-228">提供裝載啟動組件時，它們會新增至應用程式的組件，以便在應用程式在啟動時建置其通用服務時載入。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-228">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b0cb9-229">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-229">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b0cb9-230">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-230">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b0cb9-231">這項功能在 ASP.NET Core 1.x 中未提供。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-231">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="b0cb9-232">偏好裝載 URL</span><span class="sxs-lookup"><span data-stu-id="b0cb9-232">Prefer Hosting URLs</span></span>

<span data-ttu-id="b0cb9-233">表示主機是否應接聽使用 `WebHostBuilder` 設定的 URL，而不是 `IServer` 實作所設定的 URL。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-233">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="b0cb9-234">**索引鍵**：preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="b0cb9-234">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="b0cb9-235">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="b0cb9-235">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="b0cb9-236">**預設值**：true</span><span class="sxs-lookup"><span data-stu-id="b0cb9-236">**Default**: true</span></span>  
<span data-ttu-id="b0cb9-237">**設定使用**：`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="b0cb9-237">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="b0cb9-238">**環境變數**：`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="b0cb9-238">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="b0cb9-239">這項功能是在 ASP.NET Core 2.0 中新增。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-239">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b0cb9-240">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-240">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b0cb9-241">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-241">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b0cb9-242">這項功能在 ASP.NET Core 1.x 中未提供。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-242">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="b0cb9-243">防止裝載啟動</span><span class="sxs-lookup"><span data-stu-id="b0cb9-243">Prevent Hosting Startup</span></span>

<span data-ttu-id="b0cb9-244">可防止自動載入裝載啟動組件，包括應用程式組件所設定的裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-244">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="b0cb9-245">如需詳細資 訊，請參閱[使用平台特定組態來新增應用程式功能](xref:host-and-deploy/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-245">See [Add app features using a platform-specific configuration](xref:host-and-deploy/platform-specific-configuration) for more information.</span></span>

<span data-ttu-id="b0cb9-246">**索引鍵**preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="b0cb9-246">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="b0cb9-247">**類型**：*bool* (`true` 或 `1`)</span><span class="sxs-lookup"><span data-stu-id="b0cb9-247">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="b0cb9-248">**預設值**：false</span><span class="sxs-lookup"><span data-stu-id="b0cb9-248">**Default**: false</span></span>  
<span data-ttu-id="b0cb9-249">**設定使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="b0cb9-249">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="b0cb9-250">**環境變數**：`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="b0cb9-250">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="b0cb9-251">這項功能是在 ASP.NET Core 2.0 中新增。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-251">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b0cb9-252">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-252">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b0cb9-253">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-253">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b0cb9-254">這項功能在 ASP.NET Core 1.x 中未提供。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-254">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="b0cb9-255">伺服器 URL</span><span class="sxs-lookup"><span data-stu-id="b0cb9-255">Server URLs</span></span>

<span data-ttu-id="b0cb9-256">表示伺服器應該接聽要求的 IP 位址或主機位址，以及連接埠和通訊協定。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-256">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="b0cb9-257">**索引鍵**：urls</span><span class="sxs-lookup"><span data-stu-id="b0cb9-257">**Key**: urls</span></span>  
<span data-ttu-id="b0cb9-258">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="b0cb9-258">**Type**: *string*</span></span>  
<span data-ttu-id="b0cb9-259">**預設值**：http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="b0cb9-259">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="b0cb9-260">**設定使用**：`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="b0cb9-260">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="b0cb9-261">**環境變數**：`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="b0cb9-261">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="b0cb9-262">設定為伺服器應該回應的 URL 前置清單，並以分號 (;) 分隔。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-262">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="b0cb9-263">例如，`http://localhost:123`。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-263">For example, `http://localhost:123`.</span></span> <span data-ttu-id="b0cb9-264">使用 "\*"，表示伺服器應接聽任何 IP 位址或主機名稱上的要求，並使用指定的連接埠和通訊協定 (例如，`http://*:5000`)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-264">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="b0cb9-265">通訊協定 (`http://` 或 `https://`) 必須包含在每個 URL 中。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-265">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="b0cb9-266">伺服器之間支援的格式有所不同。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-266">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b0cb9-267">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-267">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="b0cb9-268">Kestrel 有它自己的端點設定 API。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-268">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="b0cb9-269">如需詳細資訊，請參閱[在 ASP.NET Core 中實作 Kestrel 網頁伺服器](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-269">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b0cb9-270">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-270">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="b0cb9-271">關機逾時</span><span class="sxs-lookup"><span data-stu-id="b0cb9-271">Shutdown Timeout</span></span>

<span data-ttu-id="b0cb9-272">指定等待 Web 主機關機的時間量。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-272">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="b0cb9-273">**索引鍵**：shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="b0cb9-273">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="b0cb9-274">**類型**：*int*</span><span class="sxs-lookup"><span data-stu-id="b0cb9-274">**Type**: *int*</span></span>  
<span data-ttu-id="b0cb9-275">**預設值**：5</span><span class="sxs-lookup"><span data-stu-id="b0cb9-275">**Default**: 5</span></span>  
<span data-ttu-id="b0cb9-276">**設定使用**：`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="b0cb9-276">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="b0cb9-277">**環境變數**：`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="b0cb9-277">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="b0cb9-278">雖然索引鍵接受 *int* 與 `UseSetting` (例如 `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`)，[UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) 擴充方法會採用 [TimeSpan](/dotnet/api/system.timespan)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-278">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span> <span data-ttu-id="b0cb9-279">這項功能是在 ASP.NET Core 2.0 中新增。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-279">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="b0cb9-280">在逾時期間，裝載會：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-280">During the timeout period, hosting:</span></span>

* <span data-ttu-id="b0cb9-281">觸發 [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-281">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="b0cb9-282">嘗試停止託管的服務，並記錄無法停止之服務的任何錯誤。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-282">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="b0cb9-283">如果在所有的託管服務停止之前逾時期限已到期，則應用程式關閉時，會停止任何剩餘的作用中服務。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-283">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="b0cb9-284">即使服務尚未完成處理也會停止。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-284">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="b0cb9-285">如果服務需要更多時間才能停止，請增加逾時。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-285">If services require additional time to stop, increase the timeout.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b0cb9-286">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-286">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b0cb9-287">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-287">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b0cb9-288">這項功能在 ASP.NET Core 1.x 中未提供。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-288">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="b0cb9-289">啟動組件</span><span class="sxs-lookup"><span data-stu-id="b0cb9-289">Startup Assembly</span></span>

<span data-ttu-id="b0cb9-290">決定要搜尋 `Startup` 類別的組件。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-290">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="b0cb9-291">**索引鍵**：startupAssembly</span><span class="sxs-lookup"><span data-stu-id="b0cb9-291">**Key**: startupAssembly</span></span>  
<span data-ttu-id="b0cb9-292">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="b0cb9-292">**Type**: *string*</span></span>  
<span data-ttu-id="b0cb9-293">**預設值**：應用程式的組件</span><span class="sxs-lookup"><span data-stu-id="b0cb9-293">**Default**: The app's assembly</span></span>  
<span data-ttu-id="b0cb9-294">**設定使用**：`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="b0cb9-294">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="b0cb9-295">**環境變數**：`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="b0cb9-295">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="b0cb9-296">可以參考依名稱 (`string`) 或類型 (`TStartup`) 的組件。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-296">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="b0cb9-297">如果呼叫多個 `UseStartup` 方法，最後一個將會優先。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-297">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b0cb9-298">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-298">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b0cb9-299">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-299">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="b0cb9-300">Web 根目錄</span><span class="sxs-lookup"><span data-stu-id="b0cb9-300">Web Root</span></span>

<span data-ttu-id="b0cb9-301">設定應用程式靜態資產的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-301">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="b0cb9-302">**索引鍵**：webroot</span><span class="sxs-lookup"><span data-stu-id="b0cb9-302">**Key**: webroot</span></span>  
<span data-ttu-id="b0cb9-303">**類型**：*string*</span><span class="sxs-lookup"><span data-stu-id="b0cb9-303">**Type**: *string*</span></span>  
<span data-ttu-id="b0cb9-304">**預設值**：如果未指定，則預設值為 "(Content Root)/wwwroot"，如果該路徑存在的話。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-304">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="b0cb9-305">如果路徑不存在，則會使用無作業檔案提供者。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-305">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="b0cb9-306">**設定使用**：`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="b0cb9-306">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="b0cb9-307">**環境變數**：`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="b0cb9-307">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b0cb9-308">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-308">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b0cb9-309">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-309">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="b0cb9-310">覆寫組態</span><span class="sxs-lookup"><span data-stu-id="b0cb9-310">Overriding configuration</span></span>

<span data-ttu-id="b0cb9-311">使用 [Configuration](xref:fundamentals/configuration/index) 來設定主機。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-311">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="b0cb9-312">在下列範例中，主機組態選擇性地指定於 *hosting.json* 檔案中。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-312">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="b0cb9-313">從 *hosting.json* 檔案載入的任何設定，可以用命令列引數覆寫。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-313">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="b0cb9-314">組建組態 (在 `config` 中) 用以使用 `UseConfiguration` 來設定主機。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-314">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b0cb9-315">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-315">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b0cb9-316">*hosting.json*：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-316">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="b0cb9-317">首先使用 *hosting.json* 組態來覆寫 `UseUrls` 所提供的設定，其次是命令列引數：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-317">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b0cb9-318">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-318">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b0cb9-319">*hosting.json*：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-319">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="b0cb9-320">首先使用 *hosting.json* 組態來覆寫 `UseUrls` 所提供的設定，其次是命令列引數：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-320">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="b0cb9-321">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 擴充方法目前無法剖析 `GetSection` 傳回的組態區段 (例如 `.UseConfiguration(Configuration.GetSection("section"))`。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-321">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="b0cb9-322">`GetSection` 方法會將設定索引鍵篩選到要求的區段，但保留索引鍵上的區段名稱 (例如 `section:urls`、`section:environment`)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-322">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="b0cb9-323">`UseConfiguration` 方法預期索引鍵要符合 `WebHostBuilder` 索引鍵 (例如 `urls`、`environment`)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-323">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="b0cb9-324">索引鍵上的區段名稱存在可避免區段的值設定主機。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-324">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="b0cb9-325">這個問題將在近期的版本中解決。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-325">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="b0cb9-326">如需詳細資訊和因應措施，請參閱 [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (將設定區段傳遞到 WebHostBuilder.UseConfiguration 會使用完整索引鍵)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-326">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="b0cb9-327">若要指定主機在特定 URL 上執行，所要的值可以在執行 [dotnet run](/dotnet/core/tools/dotnet-run) 時從命令提示字元傳入。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-327">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="b0cb9-328">命令列引數會覆寫 *hosting.json* 檔的 `urls` 值，且伺服器會接聽連接埠 8080：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-328">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="b0cb9-329">啟動主機</span><span class="sxs-lookup"><span data-stu-id="b0cb9-329">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b0cb9-330">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-330">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b0cb9-331">**執行**</span><span class="sxs-lookup"><span data-stu-id="b0cb9-331">**Run**</span></span>

<span data-ttu-id="b0cb9-332">`Run` 方法會啟動 Web 應用程式，並且封鎖呼叫執行緒，直到主機關閉為止：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-332">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="b0cb9-333">**Start**</span><span class="sxs-lookup"><span data-stu-id="b0cb9-333">**Start**</span></span>

<span data-ttu-id="b0cb9-334">藉由呼叫 `Start` 方法，以非封鎖方式執行主機：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-334">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="b0cb9-335">如果 URL 的清單傳遞至 `Start` 方法，它會接聽指定的 URL：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-335">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="b0cb9-336">應用程式可以使用預先設定的 `CreateDefaultBuilder` 預設值，使用便利的靜態方法初始化並啟動新的主機。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-336">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="b0cb9-337">這些方法會啟動伺服器而無主控台輸出，且在使用 [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) 時等候中斷 (Ctrl-C/SIGINT 或 SIGTERM)：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-337">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="b0cb9-338">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="b0cb9-338">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="b0cb9-339">使用 `RequestDelegate` 啟動：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-339">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="b0cb9-340">在瀏覽器中提出要求給 `http://localhost:5000`，以收到 "Hello World!" 回應</span><span class="sxs-lookup"><span data-stu-id="b0cb9-340">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="b0cb9-341">`WaitForShutdown` 會封鎖，直到發出中斷 (Ctrl-C/SIGINT 或 SIGTERM)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-341">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="b0cb9-342">應用程式會顯示 `Console.WriteLine` 訊息並等候按鍵動作以便結束。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-342">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="b0cb9-343">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="b0cb9-343">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="b0cb9-344">使用 URL 和 `RequestDelegate` 來啟動：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-344">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="b0cb9-345">產生與 **Start(RequestDelegate app)** 相同的結果，除了應用程式會在 `http://localhost:8080` 回應。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-345">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="b0cb9-346">**Start(Action<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="b0cb9-346">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="b0cb9-347">使用 `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) 的執行個體來使用路由中介軟體：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-347">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="b0cb9-348">使用下列瀏覽器要求與範例：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-348">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="b0cb9-349">要求</span><span class="sxs-lookup"><span data-stu-id="b0cb9-349">Request</span></span>                                    | <span data-ttu-id="b0cb9-350">回應</span><span class="sxs-lookup"><span data-stu-id="b0cb9-350">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="b0cb9-351">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="b0cb9-351">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="b0cb9-352">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="b0cb9-352">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="b0cb9-353">擲回例外狀況，字串為 "ooops!"</span><span class="sxs-lookup"><span data-stu-id="b0cb9-353">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="b0cb9-354">擲回例外狀況，字串為 "Un oh!"</span><span class="sxs-lookup"><span data-stu-id="b0cb9-354">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="b0cb9-355">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="b0cb9-355">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="b0cb9-356">Hello World!</span><span class="sxs-lookup"><span data-stu-id="b0cb9-356">Hello World!</span></span>                             |

<span data-ttu-id="b0cb9-357">`WaitForShutdown` 會封鎖，直到發出中斷 (Ctrl-C/SIGINT 或 SIGTERM)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="b0cb9-358">應用程式會顯示 `Console.WriteLine` 訊息並等候按鍵動作以便結束。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="b0cb9-359">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="b0cb9-359">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="b0cb9-360">使用 URL 和 `IRouteBuilder` 的執行個體：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-360">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="b0cb9-361">產生與 **Start(Action<IRouteBuilder> routeBuilder)** 相同的結果，除了應用程式在 `http://localhost:8080` 回應。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-361">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="b0cb9-362">**StartWith(Action<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="b0cb9-362">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="b0cb9-363">提供委派以設定 `IApplicationBuilder`：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-363">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="b0cb9-364">在瀏覽器中提出要求給 `http://localhost:5000`，以收到 "Hello World!" 回應</span><span class="sxs-lookup"><span data-stu-id="b0cb9-364">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="b0cb9-365">`WaitForShutdown` 會封鎖，直到發出中斷 (Ctrl-C/SIGINT 或 SIGTERM)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-365">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="b0cb9-366">應用程式會顯示 `Console.WriteLine` 訊息並等候按鍵動作以便結束。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-366">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="b0cb9-367">**StartWith(string url, Action<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="b0cb9-367">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="b0cb9-368">提供 URL 和委派以設定 `IApplicationBuilder`：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-368">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="b0cb9-369">產生與 **StartWith Action<IApplicationBuilder> app)** 相同的結果，除了應用程式會在 `http://localhost:8080` 回應。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-369">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b0cb9-370">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b0cb9-370">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b0cb9-371">**執行**</span><span class="sxs-lookup"><span data-stu-id="b0cb9-371">**Run**</span></span>

<span data-ttu-id="b0cb9-372">`Run` 方法會啟動 Web 應用程式，並且封鎖呼叫執行緒，直到主機關閉為止：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-372">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="b0cb9-373">**Start**</span><span class="sxs-lookup"><span data-stu-id="b0cb9-373">**Start**</span></span>

<span data-ttu-id="b0cb9-374">藉由呼叫 `Start` 方法，以非封鎖方式執行主機：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-374">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="b0cb9-375">如果 URL 的清單傳遞至 `Start` 方法，它會接聽指定的 URL：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-375">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="b0cb9-376">IHostingEnvironment 介面</span><span class="sxs-lookup"><span data-stu-id="b0cb9-376">IHostingEnvironment interface</span></span>

<span data-ttu-id="b0cb9-377">[IHostingEnvironment 介面](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment)提供應用程式的 Web 裝載環境相關資訊。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-377">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="b0cb9-378">使用[建構函式插入](xref:fundamentals/dependency-injection)以取得 `IHostingEnvironment`，才能使用其屬性和擴充方法：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-378">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="b0cb9-379">[以慣例為基礎的方法](xref:fundamentals/environments#startup-conventions)可用來根據環境在啟動時設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-379">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="b0cb9-380">或者，將 `IHostingEnvironment` 插入至 `Startup` 建構函式，以便用於 `ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-380">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="b0cb9-381">除了 `IsDevelopment` 擴充方法，`IHostingEnvironment` 也提供 `IsStaging`、`IsProduction` 和 `IsEnvironment(string environmentName)` 方法。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-381">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="b0cb9-382">如需詳細資料，請參閱[使用多個環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-382">See [Work with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="b0cb9-383">`IHostingEnvironment` 服務也可直接插入至 `Configure` 方法，以便設定處理管線：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-383">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="b0cb9-384">建立自訂[中介軟體](xref:fundamentals/middleware/index#writing-middleware)時，`IHostingEnvironment` 可以插入至 `Invoke` 方法：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-384">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="b0cb9-385">IApplicationLifetime 介面</span><span class="sxs-lookup"><span data-stu-id="b0cb9-385">IApplicationLifetime interface</span></span>

<span data-ttu-id="b0cb9-386">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) 允許啟動後和關機活動。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-386">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="b0cb9-387">在介面上的三個屬性是用來註冊定義啟動和關閉事件之 `Action` 方法的取消語彙基元。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-387">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="b0cb9-388">取消語彙基元</span><span class="sxs-lookup"><span data-stu-id="b0cb9-388">Cancellation Token</span></span>    | <span data-ttu-id="b0cb9-389">觸發時機&#8230;</span><span class="sxs-lookup"><span data-stu-id="b0cb9-389">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="b0cb9-390">已完全啟動主機。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-390">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="b0cb9-391">主機正在執行正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-391">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="b0cb9-392">可能仍在處理要求。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-392">Requests may still be processing.</span></span> <span data-ttu-id="b0cb9-393">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-393">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="b0cb9-394">主機正在完成正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-394">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="b0cb9-395">應該處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-395">All requests should be processed.</span></span> <span data-ttu-id="b0cb9-396">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-396">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="b0cb9-397">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) 會要求應用程式終止。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-397">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="b0cb9-398">當呼叫類別的 `Shutdown` 方法時，下列類別使用 `StopApplication` 來正常地關閉應用程式：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-398">The following class uses `StopApplication` to gracefully shutdown an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="b0cb9-399">範圍驗證</span><span class="sxs-lookup"><span data-stu-id="b0cb9-399">Scope validation</span></span>

<span data-ttu-id="b0cb9-400">在 ASP.NET Core 2.0 或更新版本，如果應用程式的環境是「開發」，[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 會將 [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) 設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-400">In ASP.NET Core 2.0 or later, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="b0cb9-401">當 `ValidateScopes` 設定為 `true` 時，預設服務提供者會執行檢查以確認：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-401">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="b0cb9-402">範圍服務不是直接或間接由根服務提供者解析。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-402">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="b0cb9-403">範圍服務不是直接或間接插入至單一服務。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-403">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="b0cb9-404">根服務提供者會在呼叫 [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) 時建立。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-404">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="b0cb9-405">當提供者啟動應用程式時，根服務提供者的存留期與應用程式/伺服器的存留期一致，並會在應用程式關閉時處置。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-405">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="b0cb9-406">範圍服務會由建立這些服務的容器處置。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-406">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="b0cb9-407">若是在根容器中建立範圍服務，因為當應用程式/伺服器關機時，服務只會由根容器處理，所以服務的存留期會提升為單一服務等級。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-407">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="b0cb9-408">當呼叫 `BuildServiceProvider` 時，驗證服務範圍會攔截到這些情況。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-408">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="b0cb9-409">若要一律驗證範圍，包括在實際執行環境中，請使用主機產生器上的 [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) 來設定 [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions)：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-409">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="b0cb9-410">針對 System.ArgumentException 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="b0cb9-410">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="b0cb9-411">**只適用於 ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="b0cb9-411">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="b0cb9-412">可以藉由將 `IStartup` 直接插入至相依性插入容器來建置主機，而不是呼叫 `UseStartup` 或 `Configure`：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-412">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="b0cb9-413">如果主機以此方式建置，可能會發生下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-413">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="b0cb9-414">發生此情況是因為需要 [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (目前的組件)，才能掃描 `HostingStartupAttributes`。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-414">This occurs because the [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="b0cb9-415">如果應用程式以手動方式將 `IStartup` 插入至相依性插入容器，請將下列呼叫新增至 `WebHostBuilder`，並指定組件名稱：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-415">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="b0cb9-416">或者，新增假的 `Configure` 至 `WebHostBuilder`，其自動設定 `applicationName` (`ApplicationKey`)：</span><span class="sxs-lookup"><span data-stu-id="b0cb9-416">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="b0cb9-417">**注意**：只有 ASP.NET Core 2.0 版需要這麼做，且只有在應用程式不會呼叫 `UseStartup` 或 `Configure` 時才需要。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-417">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="b0cb9-418">如需詳細資訊，請參閱 [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) (公告：已移除 Microsoft.Extensions.PlatformAbstractions (註解)) 和 [StartupInjection 範例](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs)。</span><span class="sxs-lookup"><span data-stu-id="b0cb9-418">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b0cb9-419">其他資源</span><span class="sxs-lookup"><span data-stu-id="b0cb9-419">Additional resources</span></span>

* [<span data-ttu-id="b0cb9-420"> Windows 上使用 IIS 的主機</span><span class="sxs-lookup"><span data-stu-id="b0cb9-420">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="b0cb9-421">Linux 上使用 Nginx 的主機</span><span class="sxs-lookup"><span data-stu-id="b0cb9-421">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="b0cb9-422">Linux 上使用 Apache 的主機</span><span class="sxs-lookup"><span data-stu-id="b0cb9-422">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="b0cb9-423">Windows 服務中的主機</span><span class="sxs-lookup"><span data-stu-id="b0cb9-423">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
