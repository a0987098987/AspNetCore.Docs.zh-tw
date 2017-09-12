---
title: "在 ASP.NET Core 中裝載"
author: guardrex
description: "深入了解 ASP.NET Core，負責啟動與存留期管理的應用程式中的 web 主機。"
keywords: "ASP.NET Core web 主機 IWebHost、 WebHostBuilder、 IHostingEnvironment、 IApplicationLifetime"
ms.author: riande
manager: wpickett
ms.date: 09/10/2017
ms.topic: article
ms.assetid: 4e45311d-8d56-46e2-b99d-6f65b648a277
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a1a789ff1bc6b3e3af99419e7d74d3fb46bb2345
ms.sourcegitcommit: 368aabde4de3728a8e5a8c016a2ec61f9c0854bf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="b2851-104">在 ASP.NET Core 中裝載</span><span class="sxs-lookup"><span data-stu-id="b2851-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="b2851-105">由[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b2851-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b2851-106">ASP.NET Core 應用程式設定和啟動*主機*，這是負責使用應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="b2851-106">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="b2851-107">至少一部伺服器和要求處理管線，會設定主應用程式。</span><span class="sxs-lookup"><span data-stu-id="b2851-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="b2851-108">設定主機</span><span class="sxs-lookup"><span data-stu-id="b2851-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b2851-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b2851-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b2851-110">建立主應用程式使用的執行個體[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)。</span><span class="sxs-lookup"><span data-stu-id="b2851-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="b2851-111">這通常在您的應用程式進入點，執行`Main`方法。</span><span class="sxs-lookup"><span data-stu-id="b2851-111">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="b2851-112">在專案範本中，`Main`位於*Program.cs*。</span><span class="sxs-lookup"><span data-stu-id="b2851-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="b2851-113">一般*Program.cs*呼叫[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)啟動主機的設定：</span><span class="sxs-lookup"><span data-stu-id="b2851-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="b2851-114">`CreateDefaultBuilder`會執行下列工作：</span><span class="sxs-lookup"><span data-stu-id="b2851-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="b2851-115">設定[Kestrel](servers/kestrel.md)做為 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="b2851-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span>
* <span data-ttu-id="b2851-116">若要設定內容的根[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory)。</span><span class="sxs-lookup"><span data-stu-id="b2851-116">Sets the content root to [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="b2851-117">從負載選擇性設定：</span><span class="sxs-lookup"><span data-stu-id="b2851-117">Loads optional configuration from:</span></span>
  * <span data-ttu-id="b2851-118">*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="b2851-118">*appsettings.json*.</span></span>
  * <span data-ttu-id="b2851-119">*appsettings。{環境}.json*。</span><span class="sxs-lookup"><span data-stu-id="b2851-119">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="b2851-120">[使用者密碼](xref:security/app-secrets)的應用程式執行時`Development`環境。</span><span class="sxs-lookup"><span data-stu-id="b2851-120">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="b2851-121">環境變數。</span><span class="sxs-lookup"><span data-stu-id="b2851-121">Environment variables.</span></span>
  * <span data-ttu-id="b2851-122">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="b2851-122">Command-line arguments.</span></span>
* <span data-ttu-id="b2851-123">設定[記錄](xref:fundamentals/logging)主控台和偵錯輸出與[記錄檔篩選](xref:fundamentals/logging#log-filtering)記錄組態區段中指定的規則*appsettings.json*或*appsettings。{環境}.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="b2851-123">Configures [logging](xref:fundamentals/logging) for console and debug output with [log filtering](xref:fundamentals/logging#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="b2851-124">IIS 背景執行，可讓 IIS 整合所設定的基底路徑和伺服器使用時應接聽的連接埠[ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="b2851-124">When running behind IIS, enables IIS integration by configuring the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="b2851-125">此模組會建立 Kestrel 與 IIS 之間的反向 proxy。</span><span class="sxs-lookup"><span data-stu-id="b2851-125">The module creates a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="b2851-126">也會設定應用程式[擷取啟動錯誤](#capture-startup-errors)。</span><span class="sxs-lookup"><span data-stu-id="b2851-126">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span>

<span data-ttu-id="b2851-127">*內容的根*決定主機會為內容檔案，例如 MVC 檢視的搜尋。</span><span class="sxs-lookup"><span data-stu-id="b2851-127">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="b2851-128">預設內容的根是[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory)。</span><span class="sxs-lookup"><span data-stu-id="b2851-128">The default content root is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span> <span data-ttu-id="b2851-129">這會導致在根資料夾中啟動應用程式時，使用 web 專案的根資料夾做為內容的根目錄 (例如，呼叫[dotnet 執行](/dotnet/core/tools/dotnet-run)來自專案資料夾)。</span><span class="sxs-lookup"><span data-stu-id="b2851-129">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="b2851-130">這是預設值用於[Visual Studio](https://www.visualstudio.com/)和[dotnet 新範本](/dotnet/core/tools/dotnet-new)。</span><span class="sxs-lookup"><span data-stu-id="b2851-130">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="b2851-131">請參閱[組態中 ASP.NET Core](xref:fundamentals/configuration)如需有關應用程式組態。</span><span class="sxs-lookup"><span data-stu-id="b2851-131">See [Configuration in ASP.NET Core](xref:fundamentals/configuration) for more information on app configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="b2851-132">做為使用靜態替代`CreateDefaultBuilder`方法，建立從主機[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)是支援的方法與 ASP.NET Core 2.x。</span><span class="sxs-lookup"><span data-stu-id="b2851-132">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="b2851-133">請參閱 ASP.NET Core 1.x 索引標籤的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="b2851-133">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b2851-134">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b2851-134">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b2851-135">建立主應用程式使用的執行個體[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)。</span><span class="sxs-lookup"><span data-stu-id="b2851-135">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="b2851-136">這通常在您的應用程式進入點，執行`Main`方法。</span><span class="sxs-lookup"><span data-stu-id="b2851-136">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="b2851-137">在專案範本中，`Main`位於*Program.cs*。</span><span class="sxs-lookup"><span data-stu-id="b2851-137">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="b2851-138">下列*Program.cs*示範如何使用`WebHostBuilder`建置主應用程式：</span><span class="sxs-lookup"><span data-stu-id="b2851-138">The following *Program.cs* demonstrates how to use `WebHostBuilder` to build the host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="b2851-139">`WebHostBuilder`需要[伺服器實作 IServer](servers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="b2851-139">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="b2851-140">內建的伺服器是[Kestrel](servers/kestrel.md)和[HTTP.sys](servers/httpsys.md) (ASP.NET Core 2.0 版本中之前, 已呼叫 HTTP.sys [WebListener](xref:fundamentals/servers/weblistener))。</span><span class="sxs-lookup"><span data-stu-id="b2851-140">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="b2851-141">在此範例中， [UseKestrel 擴充方法](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)指定 Kestrel 伺服器。</span><span class="sxs-lookup"><span data-stu-id="b2851-141">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="b2851-142">*內容的根*決定主機會為內容檔案，例如 MVC 檢視的搜尋。</span><span class="sxs-lookup"><span data-stu-id="b2851-142">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="b2851-143">預設內容根提供給`UseContentRoot`是[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1)。</span><span class="sxs-lookup"><span data-stu-id="b2851-143">The default content root supplied to `UseContentRoot` is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="b2851-144">這會導致在根資料夾中啟動應用程式時，使用 web 專案的根資料夾做為內容的根目錄 (例如，呼叫[dotnet 執行](/dotnet/core/tools/dotnet-run)來自專案資料夾)。</span><span class="sxs-lookup"><span data-stu-id="b2851-144">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="b2851-145">這是預設值用於[Visual Studio](https://www.visualstudio.com/)和[dotnet 新範本](/dotnet/core/tools/dotnet-new)。</span><span class="sxs-lookup"><span data-stu-id="b2851-145">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="b2851-146">若要使用 IIS 做為反向 proxy，呼叫[UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions)建置主應用程式的一部分。</span><span class="sxs-lookup"><span data-stu-id="b2851-146">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="b2851-147">`UseIISIntegration`未設定*伺服器*、 like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)沒有。</span><span class="sxs-lookup"><span data-stu-id="b2851-147">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="b2851-148">`UseIISIntegration`設定基底路徑和伺服器使用時應接聽的連接埠[ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)建立 Kestrel 與 IIS 之間的反向 proxy。</span><span class="sxs-lookup"><span data-stu-id="b2851-148">`UseIISIntegration` configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="b2851-149">若要使用 IIS 與 ASP.NET Core，您必須同時指定`UseKestrel`和`UseIISIntegration`。</span><span class="sxs-lookup"><span data-stu-id="b2851-149">To use IIS with ASP.NET Core, you must specify both `UseKestrel` and `UseIISIntegration`.</span></span> <span data-ttu-id="b2851-150">`UseIISIntegration`只會啟動執行 IIS 或 IIS Express 後面時。</span><span class="sxs-lookup"><span data-stu-id="b2851-150">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="b2851-151">如需詳細資訊，請參閱[簡介 ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)和[ASP.NET 核心模組的組態參考](xref:hosting/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="b2851-151">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

<span data-ttu-id="b2851-152">最簡單的實作，以設定主應用程式 （和 ASP.NET Core 應用程式） 包含指定伺服器和應用程式的要求管線的組態：</span><span class="sxs-lookup"><span data-stu-id="b2851-152">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="b2851-153">當設定主機，您可以提供[設定](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1)和[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1)方法。</span><span class="sxs-lookup"><span data-stu-id="b2851-153">When setting up a host, you can provide [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods.</span></span> <span data-ttu-id="b2851-154">如果您指定`Startup`類別，則必須定義`Configure`方法。</span><span class="sxs-lookup"><span data-stu-id="b2851-154">If you specify a `Startup` class, it must define a `Configure` method.</span></span> <span data-ttu-id="b2851-155">如需詳細資訊，請參閱[中 ASP.NET Core 應用程式啟動](startup.md)。</span><span class="sxs-lookup"><span data-stu-id="b2851-155">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="b2851-156">多個呼叫`ConfigureServices`附加至另一個。</span><span class="sxs-lookup"><span data-stu-id="b2851-156">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="b2851-157">多個呼叫`Configure`或`UseStartup`上`WebHostBuilder`取代先前的設定。</span><span class="sxs-lookup"><span data-stu-id="b2851-157">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="b2851-158">主機組態值</span><span class="sxs-lookup"><span data-stu-id="b2851-158">Host configuration values</span></span>

<span data-ttu-id="b2851-159">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)提供方法來設定大部分可用的設定值，主機也可以直接與設定[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting)和相關聯的金鑰。</span><span class="sxs-lookup"><span data-stu-id="b2851-159">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) provides methods for setting most of the available configuration values for the host, which can also be set directly with [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="b2851-160">設定的值時`UseSetting`，此值設定為字串 （以引號括住），不論類型為何。</span><span class="sxs-lookup"><span data-stu-id="b2851-160">When setting a value with `UseSetting`, the value is set as a string (in quotes) regardless of the type.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="b2851-161">擷取啟動錯誤</span><span class="sxs-lookup"><span data-stu-id="b2851-161">Capture Startup Errors</span></span>

<span data-ttu-id="b2851-162">此設定會控制擷取的啟動錯誤。</span><span class="sxs-lookup"><span data-stu-id="b2851-162">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="b2851-163">**索引鍵**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="b2851-163">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="b2851-164">**型別**: *bool* (`true`或`1`)</span><span class="sxs-lookup"><span data-stu-id="b2851-164">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="b2851-165">**預設**： 預設為`false`Kestrel 背後 IIS，其中預設值是以執行應用程式除非`true`。</span><span class="sxs-lookup"><span data-stu-id="b2851-165">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="b2851-166">**使用設定**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="b2851-166">**Set using**: `CaptureStartupErrors`</span></span>

<span data-ttu-id="b2851-167">當`false`，啟動導致主機結束期間發生的錯誤。</span><span class="sxs-lookup"><span data-stu-id="b2851-167">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="b2851-168">當`true`，主應用程式在啟動期間擷取例外狀況，並嘗試啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="b2851-168">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b2851-169">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b2851-169">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b2851-170">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b2851-170">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="b2851-171">內容的根</span><span class="sxs-lookup"><span data-stu-id="b2851-171">Content Root</span></span>

<span data-ttu-id="b2851-172">此設定可決定 ASP.NET Core 開始搜尋的內容檔案，例如 MVC 檢視的位置。</span><span class="sxs-lookup"><span data-stu-id="b2851-172">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="b2851-173">**索引鍵**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="b2851-173">**Key**: contentRoot</span></span>  
<span data-ttu-id="b2851-174">**型別**:*字串*</span><span class="sxs-lookup"><span data-stu-id="b2851-174">**Type**: *string*</span></span>  
<span data-ttu-id="b2851-175">**預設**： 預設為應用程式組件所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="b2851-175">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="b2851-176">**使用設定**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="b2851-176">**Set using**: `UseContentRoot`</span></span>

<span data-ttu-id="b2851-177">內容的根也作為基底路徑[Web 根目錄下設定](#web-root)。</span><span class="sxs-lookup"><span data-stu-id="b2851-177">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="b2851-178">如果路徑不存在，就無法啟動主機。</span><span class="sxs-lookup"><span data-stu-id="b2851-178">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b2851-179">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b2851-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b2851-180">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b2851-180">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="b2851-181">詳細的錯誤</span><span class="sxs-lookup"><span data-stu-id="b2851-181">Detailed Errors</span></span>

<span data-ttu-id="b2851-182">判斷詳細的錯誤應該擷取。</span><span class="sxs-lookup"><span data-stu-id="b2851-182">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="b2851-183">**索引鍵**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="b2851-183">**Key**: detailedErrors</span></span>  
<span data-ttu-id="b2851-184">**型別**: *bool* (`true`或`1`)</span><span class="sxs-lookup"><span data-stu-id="b2851-184">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="b2851-185">**預設**: false</span><span class="sxs-lookup"><span data-stu-id="b2851-185">**Default**: false</span></span>  
<span data-ttu-id="b2851-186">**使用設定**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="b2851-186">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="b2851-187">當啟用 (或當<a href="#environment">環境</a>設`Development`)，應用程式會擷取詳細例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b2851-187">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b2851-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b2851-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b2851-189">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b2851-189">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="b2851-190">環境</span><span class="sxs-lookup"><span data-stu-id="b2851-190">Environment</span></span>

<span data-ttu-id="b2851-191">設定應用程式的環境。</span><span class="sxs-lookup"><span data-stu-id="b2851-191">Sets the app's environment.</span></span>

<span data-ttu-id="b2851-192">**索引鍵**： 環境</span><span class="sxs-lookup"><span data-stu-id="b2851-192">**Key**: environment</span></span>  
<span data-ttu-id="b2851-193">**型別**:*字串*</span><span class="sxs-lookup"><span data-stu-id="b2851-193">**Type**: *string*</span></span>  
<span data-ttu-id="b2851-194">**預設**： 生產環境</span><span class="sxs-lookup"><span data-stu-id="b2851-194">**Default**: Production</span></span>  
<span data-ttu-id="b2851-195">**使用設定**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="b2851-195">**Set using**: `UseEnvironment`</span></span>

<span data-ttu-id="b2851-196">您可以設定*環境*為任何值。</span><span class="sxs-lookup"><span data-stu-id="b2851-196">You can set the *Environment* to any value.</span></span> <span data-ttu-id="b2851-197">架構定義的值包括`Development`， `Staging`，和`Production`。</span><span class="sxs-lookup"><span data-stu-id="b2851-197">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="b2851-198">值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="b2851-198">Values aren't case sensitive.</span></span> <span data-ttu-id="b2851-199">根據預設，*環境*讀取從`ASPNETCORE_ENVIRONMENT`環境變數。</span><span class="sxs-lookup"><span data-stu-id="b2851-199">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="b2851-200">當使用[Visual Studio](https://www.visualstudio.com/)，可能會設定環境變數*launchSettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="b2851-200">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="b2851-201">如需詳細資訊，請參閱[使用多個環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="b2851-201">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b2851-202">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b2851-202">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b2851-203">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b2851-203">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="b2851-204">裝載啟動的組件</span><span class="sxs-lookup"><span data-stu-id="b2851-204">Hosting Startup Assemblies</span></span>

<span data-ttu-id="b2851-205">設定應用程式的裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="b2851-205">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="b2851-206">**索引鍵**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="b2851-206">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="b2851-207">**型別**:*字串*</span><span class="sxs-lookup"><span data-stu-id="b2851-207">**Type**: *string*</span></span>  
<span data-ttu-id="b2851-208">**預設**： 空字串</span><span class="sxs-lookup"><span data-stu-id="b2851-208">**Default**: Empty string</span></span>  
<span data-ttu-id="b2851-209">**使用設定**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="b2851-209">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="b2851-210">裝載啟動在啟動時載入的組件的字串，以分號分隔。</span><span class="sxs-lookup"><span data-stu-id="b2851-210">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="b2851-211">這項功能的新 ASP.NET Core 2.0。</span><span class="sxs-lookup"><span data-stu-id="b2851-211">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="b2851-212">雖然組態值會預設為空字串，則裝載的啟動組件永遠會包含應用程式的組件。</span><span class="sxs-lookup"><span data-stu-id="b2851-212">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="b2851-213">當您提供裝載啟動組件時，這些被加入至應用程式的組件載入應用程式在啟動時建置其常見的服務時。</span><span class="sxs-lookup"><span data-stu-id="b2851-213">When you provide hosting startup assemblies, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b2851-214">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b2851-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b2851-215">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b2851-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b2851-216">這項功能已無法在 ASP.NET Core 1.x。</span><span class="sxs-lookup"><span data-stu-id="b2851-216">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="b2851-217">偏好裝載 Url</span><span class="sxs-lookup"><span data-stu-id="b2851-217">Prefer Hosting URLs</span></span>

<span data-ttu-id="b2851-218">表示主機是否應接聽設定的 Url`WebHostBuilder`而不是與`IServer`實作。</span><span class="sxs-lookup"><span data-stu-id="b2851-218">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="b2851-219">**索引鍵**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="b2851-219">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="b2851-220">**型別**: *bool* (`true`或`1`)</span><span class="sxs-lookup"><span data-stu-id="b2851-220">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="b2851-221">**預設**: true</span><span class="sxs-lookup"><span data-stu-id="b2851-221">**Default**: true</span></span>  
<span data-ttu-id="b2851-222">**使用設定**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="b2851-222">**Set using**: `PreferHostingUrls`</span></span>

<span data-ttu-id="b2851-223">這項功能的新 ASP.NET Core 2.0。</span><span class="sxs-lookup"><span data-stu-id="b2851-223">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b2851-224">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b2851-224">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b2851-225">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b2851-225">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b2851-226">這項功能已無法在 ASP.NET Core 1.x。</span><span class="sxs-lookup"><span data-stu-id="b2851-226">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="b2851-227">防止裝載啟動</span><span class="sxs-lookup"><span data-stu-id="b2851-227">Prevent Hosting Startup</span></span>

<span data-ttu-id="b2851-228">可防止自動載入裝載啟動的組件，包括應用程式的組件。</span><span class="sxs-lookup"><span data-stu-id="b2851-228">Prevents the automatic loading of hosting startup assemblies, including the app's assembly.</span></span>

<span data-ttu-id="b2851-229">**索引鍵**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="b2851-229">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="b2851-230">**型別**: *bool* (`true`或`1`)</span><span class="sxs-lookup"><span data-stu-id="b2851-230">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="b2851-231">**預設**: false</span><span class="sxs-lookup"><span data-stu-id="b2851-231">**Default**: false</span></span>  
<span data-ttu-id="b2851-232">**使用設定**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="b2851-232">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="b2851-233">這項功能的新 ASP.NET Core 2.0。</span><span class="sxs-lookup"><span data-stu-id="b2851-233">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b2851-234">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b2851-234">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b2851-235">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b2851-235">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b2851-236">這項功能已無法在 ASP.NET Core 1.x。</span><span class="sxs-lookup"><span data-stu-id="b2851-236">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="b2851-237">伺服器 Url</span><span class="sxs-lookup"><span data-stu-id="b2851-237">Server URLs</span></span>

<span data-ttu-id="b2851-238">表示 IP 位址或連接埠和通訊協定，伺服器應該接聽之要求的主機位址。</span><span class="sxs-lookup"><span data-stu-id="b2851-238">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="b2851-239">**索引鍵**: url</span><span class="sxs-lookup"><span data-stu-id="b2851-239">**Key**: urls</span></span>  
<span data-ttu-id="b2851-240">**型別**:*字串*</span><span class="sxs-lookup"><span data-stu-id="b2851-240">**Type**: *string*</span></span>  
<span data-ttu-id="b2851-241">**預設**: http://localhost:5000/</span><span class="sxs-lookup"><span data-stu-id="b2851-241">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="b2851-242">**使用設定**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="b2851-242">**Set using**: `UseUrls`</span></span>

<span data-ttu-id="b2851-243">設定為以分號分隔 （;） 應該回應伺服器的前置詞的 URL 清單。</span><span class="sxs-lookup"><span data-stu-id="b2851-243">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="b2851-244">例如，`http://localhost:123`。</span><span class="sxs-lookup"><span data-stu-id="b2851-244">For example, `http://localhost:123`.</span></span> <span data-ttu-id="b2851-245">使用 「\*"，表示伺服器應接聽任何 IP 位址或主機名稱使用指定的連接埠和通訊協定上的要求 (例如， `http://*:5000`)。</span><span class="sxs-lookup"><span data-stu-id="b2851-245">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="b2851-246">通訊協定 (`http://`或`https://`) 必須包含以每個 URL。</span><span class="sxs-lookup"><span data-stu-id="b2851-246">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="b2851-247">伺服器之間的不支援的格式。</span><span class="sxs-lookup"><span data-stu-id="b2851-247">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b2851-248">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b2851-248">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="b2851-249">Kestrel 有它自己的端點設定應用程式開發介面。</span><span class="sxs-lookup"><span data-stu-id="b2851-249">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="b2851-250">如需詳細資訊，請參閱[在 ASP.NET Core 中實作 Kestrel 網頁伺服器](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)。</span><span class="sxs-lookup"><span data-stu-id="b2851-250">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b2851-251">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b2851-251">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="b2851-252">關閉逾時</span><span class="sxs-lookup"><span data-stu-id="b2851-252">Shutdown Timeout</span></span>

<span data-ttu-id="b2851-253">指定 web 主機關閉的時間量。</span><span class="sxs-lookup"><span data-stu-id="b2851-253">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="b2851-254">**索引鍵**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="b2851-254">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="b2851-255">**型別**: *int*</span><span class="sxs-lookup"><span data-stu-id="b2851-255">**Type**: *int*</span></span>  
<span data-ttu-id="b2851-256">**預設**: 5</span><span class="sxs-lookup"><span data-stu-id="b2851-256">**Default**: 5</span></span>  
<span data-ttu-id="b2851-257">**使用設定**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="b2851-257">**Set using**: `UseShutdownTimeout`</span></span>

<span data-ttu-id="b2851-258">雖然索引鍵接受*int*與`UseSetting`(例如， `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`)、`UseShutdownTimeout`擴充方法會採用`TimeSpan`。</span><span class="sxs-lookup"><span data-stu-id="b2851-258">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="b2851-259">這項功能的新 ASP.NET Core 2.0。</span><span class="sxs-lookup"><span data-stu-id="b2851-259">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b2851-260">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b2851-260">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b2851-261">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b2851-261">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b2851-262">這項功能已無法在 ASP.NET Core 1.x。</span><span class="sxs-lookup"><span data-stu-id="b2851-262">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="b2851-263">啟動組件</span><span class="sxs-lookup"><span data-stu-id="b2851-263">Startup Assembly</span></span>

<span data-ttu-id="b2851-264">決定要搜尋的組件`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="b2851-264">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="b2851-265">**索引鍵**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="b2851-265">**Key**: startupAssembly</span></span>  
<span data-ttu-id="b2851-266">**型別**:*字串*</span><span class="sxs-lookup"><span data-stu-id="b2851-266">**Type**: *string*</span></span>  
<span data-ttu-id="b2851-267">**預設**： 應用程式的組件</span><span class="sxs-lookup"><span data-stu-id="b2851-267">**Default**: The app's assembly</span></span>  
<span data-ttu-id="b2851-268">**使用設定**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="b2851-268">**Set using**: `UseStartup`</span></span>

<span data-ttu-id="b2851-269">您可以依名稱參考組件 (`string`) 或型別 (`TStartup`)。</span><span class="sxs-lookup"><span data-stu-id="b2851-269">You can reference the assembly by name (`string`) or type (`TStartup`).</span></span> <span data-ttu-id="b2851-270">若為多個`UseStartup`呼叫的方法，最後一個的優先順序。</span><span class="sxs-lookup"><span data-stu-id="b2851-270">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b2851-271">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b2851-271">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b2851-272">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b2851-272">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="b2851-273">Web 根目錄</span><span class="sxs-lookup"><span data-stu-id="b2851-273">Web Root</span></span>

<span data-ttu-id="b2851-274">設定應用程式的靜態資產的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="b2851-274">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="b2851-275">**索引鍵**: webroot</span><span class="sxs-lookup"><span data-stu-id="b2851-275">**Key**: webroot</span></span>  
<span data-ttu-id="b2851-276">**型別**:*字串*</span><span class="sxs-lookup"><span data-stu-id="b2851-276">**Type**: *string*</span></span>  
<span data-ttu-id="b2851-277">**預設**： 如果未指定，預設值是"(Content Root)/wwwroot"，則該路徑存在。</span><span class="sxs-lookup"><span data-stu-id="b2851-277">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="b2851-278">如果路徑不存在，則會使用任何作業檔案提供者。</span><span class="sxs-lookup"><span data-stu-id="b2851-278">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="b2851-279">**使用設定**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="b2851-279">**Set using**: `UseWebRoot`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b2851-280">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b2851-280">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b2851-281">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b2851-281">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="b2851-282">覆寫設定</span><span class="sxs-lookup"><span data-stu-id="b2851-282">Overriding configuration</span></span>

<span data-ttu-id="b2851-283">使用[組態](configuration.md)設定主控件。</span><span class="sxs-lookup"><span data-stu-id="b2851-283">Use [Configuration](configuration.md) to configure the host.</span></span> <span data-ttu-id="b2851-284">在下列範例中，主機設定 （選擇性） 指定於*hosting.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="b2851-284">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="b2851-285">從載入任何組態*hosting.json*命令列引數可能會覆寫檔案。</span><span class="sxs-lookup"><span data-stu-id="b2851-285">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="b2851-286">內建的設定 (在`config`) 用來設定與主機`UseConfiguration`。</span><span class="sxs-lookup"><span data-stu-id="b2851-286">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b2851-287">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b2851-287">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b2851-288">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="b2851-288">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="b2851-289">覆寫所提供的組態`UseUrls`與*hosting.json*設定第一個命令列引數設定第二個：</span><span class="sxs-lookup"><span data-stu-id="b2851-289">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b2851-290">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b2851-290">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b2851-291">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="b2851-291">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="b2851-292">覆寫所提供的組態`UseUrls`與*hosting.json*設定第一個命令列引數設定第二個：</span><span class="sxs-lookup"><span data-stu-id="b2851-292">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="b2851-293">`UseConfiguration`擴充方法不是目前可以剖析所傳回的組態區段`GetSection`(例如， `.UseConfiguration(Configuration.GetSection("section"))`。</span><span class="sxs-lookup"><span data-stu-id="b2851-293">The `UseConfiguration` extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="b2851-294">`GetSection`方法篩選到要求的區段之組態機碼，但會保留的區段名稱索引鍵 (例如， `section:urls`， `section:environment`)。</span><span class="sxs-lookup"><span data-stu-id="b2851-294">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="b2851-295">`UseConfiguration`方法預期要比對的索引鍵`WebHostBuilder`索引鍵 (例如， `urls`， `environment`)。</span><span class="sxs-lookup"><span data-stu-id="b2851-295">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="b2851-296">索引鍵的區段名稱存在主控件設定防止區段的值。</span><span class="sxs-lookup"><span data-stu-id="b2851-296">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="b2851-297">這個問題將在近期的版本中解決。</span><span class="sxs-lookup"><span data-stu-id="b2851-297">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="b2851-298">如需詳細資訊和因應措施，請參閱[傳遞到 WebHostBuilder.UseConfiguration 組態區段會使用完整金鑰](https://github.com/aspnet/Hosting/issues/839)。</span><span class="sxs-lookup"><span data-stu-id="b2851-298">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="b2851-299">若要指定特定 URL 上所執行的主機，您可以傳入所要的值從命令提示字元執行時`dotnet run`。</span><span class="sxs-lookup"><span data-stu-id="b2851-299">To specify the host run on a particular URL, you could pass in the desired value from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="b2851-300">命令列引數會覆寫`urls`值*hosting.json*檔和伺服器會接聽連接埠 8080:</span><span class="sxs-lookup"><span data-stu-id="b2851-300">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="ordering-importance"></a><span data-ttu-id="b2851-301">排序重要性</span><span class="sxs-lookup"><span data-stu-id="b2851-301">Ordering importance</span></span>

<span data-ttu-id="b2851-302">部分`WebHostBuilder`如果先設定讀取來自環境變數設定。</span><span class="sxs-lookup"><span data-stu-id="b2851-302">Some of the `WebHostBuilder` settings are first read from environment variables, if set.</span></span> <span data-ttu-id="b2851-303">這些環境變數使用格式`ASPNETCORE_{configurationKey}`。</span><span class="sxs-lookup"><span data-stu-id="b2851-303">These environment variables use the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="b2851-304">若要設定伺服器接聽預設的 Url，您將設定`ASPNETCORE_URLS`。</span><span class="sxs-lookup"><span data-stu-id="b2851-304">To set the URLs that the server listens on by default, you set `ASPNETCORE_URLS`.</span></span>

<span data-ttu-id="b2851-305">您可以指定覆寫這些環境變數值的任何組態 (使用`UseConfiguration`) 或明確地將此值設定 (使用`UseSetting`或其中一個明確的擴充方法，例如`UseUrls`)。</span><span class="sxs-lookup"><span data-stu-id="b2851-305">You can override any of these environment variable values by specifying configuration (using `UseConfiguration`) or by setting the value explicitly (using `UseSetting` or one of the explicit extension methods, such as `UseUrls`).</span></span> <span data-ttu-id="b2851-306">主機會使用任何選項設定的值上一次。</span><span class="sxs-lookup"><span data-stu-id="b2851-306">The host uses whichever option sets the value last.</span></span> <span data-ttu-id="b2851-307">如果您想要以程式設計方式設定的預設 URL 為一個值，但允許它會覆寫的組態，您可以使用命令列組態之後設定 URL。</span><span class="sxs-lookup"><span data-stu-id="b2851-307">If you want to programmatically set the default URL to one value but allow it to be overridden with configuration, you can use command-line configuration after setting the URL.</span></span> <span data-ttu-id="b2851-308">請參閱[正在覆寫組態](#overriding-configuration)。</span><span class="sxs-lookup"><span data-stu-id="b2851-308">See [Overriding configuration](#overriding-configuration).</span></span>

## <a name="starting-the-host"></a><span data-ttu-id="b2851-309">正在啟動主機</span><span class="sxs-lookup"><span data-stu-id="b2851-309">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b2851-310">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b2851-310">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b2851-311">**執行**</span><span class="sxs-lookup"><span data-stu-id="b2851-311">**Run**</span></span>

<span data-ttu-id="b2851-312">`Run`方法會啟動 web 應用程式，並且封鎖呼叫執行緒，直到主機已關閉：</span><span class="sxs-lookup"><span data-stu-id="b2851-312">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="b2851-313">**Start**</span><span class="sxs-lookup"><span data-stu-id="b2851-313">**Start**</span></span>

<span data-ttu-id="b2851-314">您可以藉由呼叫非封鎖方式執行主機其`Start`方法：</span><span class="sxs-lookup"><span data-stu-id="b2851-314">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="b2851-315">如果您要傳入的 Url 清單`Start`方法，它會接聽指定的 Url:</span><span class="sxs-lookup"><span data-stu-id="b2851-315">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="b2851-316">您可以初始化並開始使用預先設定的預設值的新主控件`CreateDefaultBuilder`使用靜態便利的方法。</span><span class="sxs-lookup"><span data-stu-id="b2851-316">You can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="b2851-317">啟動伺服器未主控台輸出的情況下，這些方法[WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown)等候中斷 （Ctrl-C/SIGINT 或 SIGTERM）：</span><span class="sxs-lookup"><span data-stu-id="b2851-317">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="b2851-318">**開始 （RequestDelegate 應用程式）**</span><span class="sxs-lookup"><span data-stu-id="b2851-318">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="b2851-319">開頭`RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="b2851-319">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="b2851-320">若要在瀏覽器中提出要求`http://localhost:5000`接收"Hello World ！"的回應</span><span class="sxs-lookup"><span data-stu-id="b2851-320">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="b2851-321">`WaitForShutdown`直到發出中斷 （Ctrl-C/SIGINT 或 SIGTERM） 的區塊。</span><span class="sxs-lookup"><span data-stu-id="b2851-321">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="b2851-322">應用程式會顯示`Console.WriteLine`訊息並等候 keypress 結束。</span><span class="sxs-lookup"><span data-stu-id="b2851-322">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="b2851-323">**開始 （以字串 url、 RequestDelegate 應用程式）**</span><span class="sxs-lookup"><span data-stu-id="b2851-323">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="b2851-324">URL 的開頭和`RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="b2851-324">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="b2851-325">會產生相同結果**開始 （RequestDelegate 應用程式）**，除了應用程式回應`http://localhost:8080`。</span><span class="sxs-lookup"><span data-stu-id="b2851-325">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="b2851-326">**啟動 (動作<IRouteBuilder>routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="b2851-326">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="b2851-327">使用的執行個體`IRouteBuilder`([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) 使用路由的中介軟體：</span><span class="sxs-lookup"><span data-stu-id="b2851-327">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="b2851-328">使用下列瀏覽器要求範例：</span><span class="sxs-lookup"><span data-stu-id="b2851-328">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="b2851-329">要求</span><span class="sxs-lookup"><span data-stu-id="b2851-329">Request</span></span>                                    | <span data-ttu-id="b2851-330">回應</span><span class="sxs-lookup"><span data-stu-id="b2851-330">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="b2851-331">Hello，Martin ！</span><span class="sxs-lookup"><span data-stu-id="b2851-331">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="b2851-332">Catrina 布宜諾 dias ！</span><span class="sxs-lookup"><span data-stu-id="b2851-332">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="b2851-333">擲回例外狀況的字串"ooops ！ 」</span><span class="sxs-lookup"><span data-stu-id="b2851-333">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="b2851-334">擲回例外狀況的字串"Uh 喔 ！ 」</span><span class="sxs-lookup"><span data-stu-id="b2851-334">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="b2851-335">Sante，Kevin ！</span><span class="sxs-lookup"><span data-stu-id="b2851-335">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="b2851-336">Hello World!</span><span class="sxs-lookup"><span data-stu-id="b2851-336">Hello World!</span></span>                             |

<span data-ttu-id="b2851-337">`WaitForShutdown`直到發出中斷 （Ctrl-C/SIGINT 或 SIGTERM） 的區塊。</span><span class="sxs-lookup"><span data-stu-id="b2851-337">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="b2851-338">應用程式會顯示`Console.WriteLine`訊息並等候 keypress 結束。</span><span class="sxs-lookup"><span data-stu-id="b2851-338">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="b2851-339">**啟動 (string url，動作<IRouteBuilder>routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="b2851-339">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="b2851-340">使用 URL 和執行個體`IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="b2851-340">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="b2851-341">會產生相同結果**開始 (動作<IRouteBuilder>routeBuilder)**，除了應用程式會回應在`http://localhost:8080`。</span><span class="sxs-lookup"><span data-stu-id="b2851-341">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="b2851-342">**StartWith (動作<IApplicationBuilder>應用程式)**</span><span class="sxs-lookup"><span data-stu-id="b2851-342">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="b2851-343">提供設定委派`IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="b2851-343">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="b2851-344">若要在瀏覽器中提出要求`http://localhost:5000`接收"Hello World ！"的回應</span><span class="sxs-lookup"><span data-stu-id="b2851-344">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="b2851-345">`WaitForShutdown`直到發出中斷 （Ctrl-C/SIGINT 或 SIGTERM） 的區塊。</span><span class="sxs-lookup"><span data-stu-id="b2851-345">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="b2851-346">應用程式會顯示`Console.WriteLine`訊息並等候 keypress 結束。</span><span class="sxs-lookup"><span data-stu-id="b2851-346">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="b2851-347">**StartWith (string url，動作<IApplicationBuilder>應用程式)**</span><span class="sxs-lookup"><span data-stu-id="b2851-347">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="b2851-348">提供 URL，若要設定委派`IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="b2851-348">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="b2851-349">會產生相同結果**StartWith (動作<IApplicationBuilder>應用程式)**，除了應用程式回應`http://localhost:8080`。</span><span class="sxs-lookup"><span data-stu-id="b2851-349">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b2851-350">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b2851-350">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b2851-351">**執行**</span><span class="sxs-lookup"><span data-stu-id="b2851-351">**Run**</span></span>

<span data-ttu-id="b2851-352">`Run`方法會啟動 web 應用程式，並且封鎖呼叫執行緒，直到主機已關閉：</span><span class="sxs-lookup"><span data-stu-id="b2851-352">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="b2851-353">**Start**</span><span class="sxs-lookup"><span data-stu-id="b2851-353">**Start**</span></span>

<span data-ttu-id="b2851-354">您可以藉由呼叫非封鎖方式執行主機其`Start`方法：</span><span class="sxs-lookup"><span data-stu-id="b2851-354">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="b2851-355">如果您要傳入的 Url 清單`Start`方法，它會接聽指定的 Url:</span><span class="sxs-lookup"><span data-stu-id="b2851-355">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="b2851-356">IHostingEnvironment 介面</span><span class="sxs-lookup"><span data-stu-id="b2851-356">IHostingEnvironment interface</span></span>

<span data-ttu-id="b2851-357">[IHostingEnvironment 介面](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment)提供應用程式的 web 主機環境的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="b2851-357">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="b2851-358">您可以使用[建構函式插入](xref:fundamentals/dependency-injection)取得`IHostingEnvironment`才能使用其屬性和擴充方法：</span><span class="sxs-lookup"><span data-stu-id="b2851-358">You can use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="b2851-359">您可以使用[慣例為基礎的方法](xref:fundamentals/environments#startup-conventions)根據環境的啟動時設定您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b2851-359">You can use a [convention-based approach](xref:fundamentals/environments#startup-conventions) to configure your app at startup based on the environment.</span></span> <span data-ttu-id="b2851-360">或者，您可以將插入`IHostingEnvironment`到`Startup`建構函式以用於`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b2851-360">Alternatively, you can inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="b2851-361">除了`IsDevelopment`擴充方法，`IHostingEnvironment`提供`IsStaging`， `IsProduction`，和`IsEnvironment(string environmentName)`方法。</span><span class="sxs-lookup"><span data-stu-id="b2851-361">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="b2851-362">請參閱[使用多個環境](xref:fundamentals/environments)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="b2851-362">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="b2851-363">`IHostingEnvironment`服務也可直接插入`Configure`設定處理管線的方法：</span><span class="sxs-lookup"><span data-stu-id="b2851-363">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up your processing pipeline:</span></span>

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

<span data-ttu-id="b2851-364">您可以將插入`IHostingEnvironment`到`Invoke`方法時建立自訂[中介軟體](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="b2851-364">You can inject `IHostingEnvironment` into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="b2851-365">IApplicationLifetime 介面</span><span class="sxs-lookup"><span data-stu-id="b2851-365">IApplicationLifetime interface</span></span>

<span data-ttu-id="b2851-366">[IApplicationLifetime 介面](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime)可讓您執行後啟動和關閉的活動。</span><span class="sxs-lookup"><span data-stu-id="b2851-366">The [IApplicationLifetime interface](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows you to perform post-startup and shutdown activities.</span></span> <span data-ttu-id="b2851-367">介面上的三個屬性都是您可以使用註冊的取消語彙基元`Action`定義啟動和關閉事件的方法。</span><span class="sxs-lookup"><span data-stu-id="b2851-367">Three properties on the interface are cancellation tokens that you can register with `Action` methods to define startup and shutdown events.</span></span> <span data-ttu-id="b2851-368">另外還有`StopApplication`方法。</span><span class="sxs-lookup"><span data-stu-id="b2851-368">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="b2851-369">取消語彙基元</span><span class="sxs-lookup"><span data-stu-id="b2851-369">Cancellation Token</span></span>    | <span data-ttu-id="b2851-370">觸發時 &#8230;</span><span class="sxs-lookup"><span data-stu-id="b2851-370">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="b2851-371">已完全啟動主機。</span><span class="sxs-lookup"><span data-stu-id="b2851-371">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="b2851-372">主機正在執行正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="b2851-372">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="b2851-373">可能仍在處理要求。</span><span class="sxs-lookup"><span data-stu-id="b2851-373">Requests may still be processing.</span></span> <span data-ttu-id="b2851-374">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="b2851-374">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="b2851-375">主應用程式即將完成正常關機程序。</span><span class="sxs-lookup"><span data-stu-id="b2851-375">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="b2851-376">應該完全處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="b2851-376">All requests should be completely processed.</span></span> <span data-ttu-id="b2851-377">關機封鎖，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="b2851-377">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="b2851-378">方法</span><span class="sxs-lookup"><span data-stu-id="b2851-378">Method</span></span>            | <span data-ttu-id="b2851-379">動作</span><span class="sxs-lookup"><span data-stu-id="b2851-379">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="b2851-380">目前的應用程式要求終止。</span><span class="sxs-lookup"><span data-stu-id="b2851-380">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="b2851-381">疑難排解 System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="b2851-381">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="b2851-382">**ASP.NET Core 2.0 只適用於**</span><span class="sxs-lookup"><span data-stu-id="b2851-382">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="b2851-383">如果您將，以便建置主機`IStartup`直接將相依性插入容器，而不是呼叫`UseStartup`或`Configure`，您可能會遇到下列錯誤： `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`。</span><span class="sxs-lookup"><span data-stu-id="b2851-383">If you build the host by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`, you may encounter the following error: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span></span>

<span data-ttu-id="b2851-384">這是因為[applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) （目前的組件），才能掃描`HostingStartupAttributes`。</span><span class="sxs-lookup"><span data-stu-id="b2851-384">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="b2851-385">如果您手動插入`IStartup`到相依性插入容器中，加入下列呼叫您`WebHostBuilder`具有所指定的組件名稱：</span><span class="sxs-lookup"><span data-stu-id="b2851-385">If you manually inject `IStartup` into the dependency injection container, add the following call to your `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="b2851-386">或者，新增假`Configure`到您`WebHostBuilder`，將`applicationName`(`ApplicationKey`) 自動：</span><span class="sxs-lookup"><span data-stu-id="b2851-386">Alternatively, add a dummy `Configure` to your `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="b2851-387">**請注意**： 時，這只需要 ASP.NET Core 2.0 版，而且只在未呼叫`UseStartup`或`Configure`。</span><span class="sxs-lookup"><span data-stu-id="b2851-387">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when you don't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="b2851-388">如需詳細資訊，請參閱[公告： Microsoft.Extensions.PlatformAbstractions 已移除 （註解）](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938)和[StartupInjection 範例](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs)。</span><span class="sxs-lookup"><span data-stu-id="b2851-388">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b2851-389">其他資源</span><span class="sxs-lookup"><span data-stu-id="b2851-389">Additional resources</span></span>

* [<span data-ttu-id="b2851-390">發行到使用 IIS 的 Windows</span><span class="sxs-lookup"><span data-stu-id="b2851-390">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="b2851-391">發行至使用 Nginx Linux</span><span class="sxs-lookup"><span data-stu-id="b2851-391">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="b2851-392">若要使用 Apache 的 Linux 發行</span><span class="sxs-lookup"><span data-stu-id="b2851-392">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="b2851-393">在 Windows 服務的主機</span><span class="sxs-lookup"><span data-stu-id="b2851-393">Host in a Windows Service</span></span>](xref:hosting/windows-service)
