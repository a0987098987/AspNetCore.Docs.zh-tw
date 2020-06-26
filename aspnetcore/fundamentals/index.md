---
title: ASP.NET Core 基本概念
author: rick-anderson
description: 了解建置 ASP.NET Core 應用程式的基本概念。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/30/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/index
ms.openlocfilehash: c797ce8bcb22aec2b56df2f3b108da4cbfde263d
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85403296"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="7eada-103">ASP.NET Core 基本概念</span><span class="sxs-lookup"><span data-stu-id="7eada-103">ASP.NET Core fundamentals</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7eada-104">本文概述瞭解如何開發 ASP.NET Core 應用程式的重要主題。</span><span class="sxs-lookup"><span data-stu-id="7eada-104">This article provides an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="7eada-105">Startup 類別</span><span class="sxs-lookup"><span data-stu-id="7eada-105">The Startup class</span></span>

<span data-ttu-id="7eada-106">`Startup` 類別是：</span><span class="sxs-lookup"><span data-stu-id="7eada-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="7eada-107">已設定應用程式所需的服務。</span><span class="sxs-lookup"><span data-stu-id="7eada-107">Services required by the app are configured.</span></span>
* <span data-ttu-id="7eada-108">應用程式的要求處理管線會定義為一系列中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="7eada-108">The app's request handling pipeline is defined, as a series of middleware components.</span></span>

<span data-ttu-id="7eada-109">以下是 `Startup` 類別範例：</span><span class="sxs-lookup"><span data-stu-id="7eada-109">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/samples_snapshot/3.x/Startup.cs?highlight=3,12)]

<span data-ttu-id="7eada-110">如需詳細資訊，請參閱 <xref:fundamentals/startup> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-110">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="7eada-111">相依性插入 (服務)</span><span class="sxs-lookup"><span data-stu-id="7eada-111">Dependency injection (services)</span></span>

<span data-ttu-id="7eada-112">ASP.NET Core 包含內建的相依性插入（DI）架構，可讓您在整個應用程式中使用已設定的服務。</span><span class="sxs-lookup"><span data-stu-id="7eada-112">ASP.NET Core includes a built-in dependency injection (DI) framework that makes configured services available throughout an app.</span></span> <span data-ttu-id="7eada-113">例如，記錄元件即為一項服務。</span><span class="sxs-lookup"><span data-stu-id="7eada-113">For example, a logging component is a service.</span></span>

<span data-ttu-id="7eada-114">設定 (或「註冊」\*\*) 服務的程式碼會新增至 `Startup.ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="7eada-114">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="7eada-115">例如：</span><span class="sxs-lookup"><span data-stu-id="7eada-115">For example:</span></span>

[!code-csharp[](index/samples_snapshot/3.x/ConfigureServices.cs)]

<span data-ttu-id="7eada-116">服務通常會使用「函式插入」從 DI 解析。</span><span class="sxs-lookup"><span data-stu-id="7eada-116">Services are typically resolved from DI using constructor injection.</span></span> <span data-ttu-id="7eada-117">使用函式插入時，類別會宣告所需類型或介面的函式參數。</span><span class="sxs-lookup"><span data-stu-id="7eada-117">With constructor injection, a class declares a constructor parameter of either the required type or an interface.</span></span> <span data-ttu-id="7eada-118">DI 架構會在執行時間提供此服務的實例。</span><span class="sxs-lookup"><span data-stu-id="7eada-118">The DI framework provides an instance of this service at runtime.</span></span>

<span data-ttu-id="7eada-119">下列範例會使用從 DI 來解析的「處理函式」插入 `RazorPagesMovieContext` ：</span><span class="sxs-lookup"><span data-stu-id="7eada-119">The following example uses constructor injection to resolve a `RazorPagesMovieContext` from DI:</span></span>

[!code-csharp[](index/samples_snapshot/3.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="7eada-120">如果內建的反轉控制（IoC）容器不符合應用程式的所有需求，則可以改用協力廠商 IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="7eada-120">If the built-in Inversion of Control (IoC) container doesn't meet all of an app's needs, a third-party IoC container can be used instead.</span></span>

<span data-ttu-id="7eada-121">如需詳細資訊，請參閱 <xref:fundamentals/dependency-injection> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-121">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="7eada-122">中介軟體</span><span class="sxs-lookup"><span data-stu-id="7eada-122">Middleware</span></span>

<span data-ttu-id="7eada-123">要求處理管線是以一系列中介軟體元件組成。</span><span class="sxs-lookup"><span data-stu-id="7eada-123">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="7eada-124">每個元件都會在上執行作業 `HttpContext` ，並叫用管線中的下一個中介軟體或終止要求。</span><span class="sxs-lookup"><span data-stu-id="7eada-124">Each component performs operations on an `HttpContext` and either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="7eada-125">依照慣例，會在方法中叫用擴充方法，將中介軟體元件新增至管線 `Use...` `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="7eada-125">By convention, a middleware component is added to the pipeline by invoking a `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="7eada-126">例如，若要啟用靜態檔案轉譯，請呼叫 `UseStaticFiles`。</span><span class="sxs-lookup"><span data-stu-id="7eada-126">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="7eada-127">下列範例會設定要求處理管線：</span><span class="sxs-lookup"><span data-stu-id="7eada-127">The following example configures a request handling pipeline:</span></span>

[!code-csharp[](index/samples_snapshot/3.x/Configure.cs)]

<span data-ttu-id="7eada-128">ASP.NET Core 包含一組豐富的內建中介軟體。</span><span class="sxs-lookup"><span data-stu-id="7eada-128">ASP.NET Core includes a rich set of built-in middleware.</span></span> <span data-ttu-id="7eada-129">也可以寫入自訂中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="7eada-129">Custom middleware components can also be written.</span></span>

<span data-ttu-id="7eada-130">如需詳細資訊，請參閱 <xref:fundamentals/middleware/index> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-130">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="7eada-131">Host</span><span class="sxs-lookup"><span data-stu-id="7eada-131">Host</span></span>

<span data-ttu-id="7eada-132">在啟動時，ASP.NET Core 應用程式會建立*主機*。</span><span class="sxs-lookup"><span data-stu-id="7eada-132">On startup, an ASP.NET Core app builds a *host*.</span></span> <span data-ttu-id="7eada-133">主機會封裝所有應用程式的資源，例如：</span><span class="sxs-lookup"><span data-stu-id="7eada-133">The host encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="7eada-134">HTTP 伺服器實作</span><span class="sxs-lookup"><span data-stu-id="7eada-134">An HTTP server implementation</span></span>
* <span data-ttu-id="7eada-135">中介軟體元件</span><span class="sxs-lookup"><span data-stu-id="7eada-135">Middleware components</span></span>
* <span data-ttu-id="7eada-136">記錄</span><span class="sxs-lookup"><span data-stu-id="7eada-136">Logging</span></span>
* <span data-ttu-id="7eada-137">相依性插入（DI）服務</span><span class="sxs-lookup"><span data-stu-id="7eada-137">Dependency injection (DI) services</span></span>
* <span data-ttu-id="7eada-138">設定</span><span class="sxs-lookup"><span data-stu-id="7eada-138">Configuration</span></span>

<span data-ttu-id="7eada-139">有兩個不同的主機：</span><span class="sxs-lookup"><span data-stu-id="7eada-139">There are two different hosts:</span></span> 

* <span data-ttu-id="7eada-140">.NET 泛型主機</span><span class="sxs-lookup"><span data-stu-id="7eada-140">.NET Generic Host</span></span>
* <span data-ttu-id="7eada-141">ASP.NET Core Web 主機</span><span class="sxs-lookup"><span data-stu-id="7eada-141">ASP.NET Core Web Host</span></span>

<span data-ttu-id="7eada-142">建議使用 .NET 泛型主機。</span><span class="sxs-lookup"><span data-stu-id="7eada-142">The .NET Generic Host is recommended.</span></span> <span data-ttu-id="7eada-143">ASP.NET Core Web 主機僅供回溯相容性之用。</span><span class="sxs-lookup"><span data-stu-id="7eada-143">The ASP.NET Core Web Host is available only for backwards compatibility.</span></span>

<span data-ttu-id="7eada-144">下列範例會建立 .NET 泛型主機：</span><span class="sxs-lookup"><span data-stu-id="7eada-144">The following example creates a .NET Generic Host:</span></span>

[!code-csharp[](index/samples_snapshot/3.x/Program.cs)]

<span data-ttu-id="7eada-145">`CreateDefaultBuilder`和 `ConfigureWebHostDefaults` 方法會使用一組預設選項來設定主機，例如：</span><span class="sxs-lookup"><span data-stu-id="7eada-145">The `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods configure a host with a set of default options, such as:</span></span>

* <span data-ttu-id="7eada-146">使用 [Kestrel](#servers) 作為網頁伺服器，並啟用 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="7eada-146">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="7eada-147">從 *appsettings.json*、*appsettings.{Environment Name}.json*、環境變數與命令列引數載入設定。</span><span class="sxs-lookup"><span data-stu-id="7eada-147">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="7eada-148">將記錄輸出傳送到主控台及偵錯提供者。</span><span class="sxs-lookup"><span data-stu-id="7eada-148">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="7eada-149">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-149">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="non-web-scenarios"></a><span data-ttu-id="7eada-150">非 Web 案例</span><span class="sxs-lookup"><span data-stu-id="7eada-150">Non-web scenarios</span></span>

<span data-ttu-id="7eada-151">一般主機允許其他類型的應用程式，使用交叉剪輯架構延伸模組，例如記錄、相依性插入 (DI)、設定與應用程式存留期管理。</span><span class="sxs-lookup"><span data-stu-id="7eada-151">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="7eada-152">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host> 和 <xref:fundamentals/host/hosted-services>。</span><span class="sxs-lookup"><span data-stu-id="7eada-152">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="7eada-153">伺服器</span><span class="sxs-lookup"><span data-stu-id="7eada-153">Servers</span></span>

<span data-ttu-id="7eada-154">ASP.NET Core 應用程式使用 HTTP 伺服器實作來接聽 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="7eada-154">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="7eada-155">伺服器會把向應用程式發出的要求作為一組[要求功能](xref:fundamentals/request-features)，合併成一個 `HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="7eada-155">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

# <a name="windows"></a>[<span data-ttu-id="7eada-156">Windows</span><span class="sxs-lookup"><span data-stu-id="7eada-156">Windows</span></span>](#tab/windows)

<span data-ttu-id="7eada-157">ASP.NET Core 隨附下列伺服器實作：</span><span class="sxs-lookup"><span data-stu-id="7eada-157">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="7eada-158">*Kestrel* 是跨平台的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="7eada-158">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="7eada-159">Kestrel 通常會使用 [IIS](https://www.iis.net/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="7eada-159">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="7eada-160">在 ASP.NET Core 2.0 或更新版本中，Kestrel 可以作為直接向網際網路公開的公眾 Edge Server 執行。</span><span class="sxs-lookup"><span data-stu-id="7eada-160">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="7eada-161">*IIS HTTP 伺服器*是使用 Iis 之 Windows 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="7eada-161">*IIS HTTP Server* is a server for Windows that uses IIS.</span></span> <span data-ttu-id="7eada-162">透過此伺服器，ASP.NET Core 應用程式及 IIS 便可以在相同處理序中執行。</span><span class="sxs-lookup"><span data-stu-id="7eada-162">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="7eada-163">*HTTP.sys* 則是適用於並未搭配 IIS 使用 Windows 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="7eada-163">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="7eada-164">macOS</span><span class="sxs-lookup"><span data-stu-id="7eada-164">macOS</span></span>](#tab/macos)

<span data-ttu-id="7eada-165">ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="7eada-165">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7eada-166">在 ASP.NET Core 2.0 或更新版本中，Kestrel 可以當做直接向網際網路公開的公眾邊緣伺服器來執行。</span><span class="sxs-lookup"><span data-stu-id="7eada-166">In ASP.NET Core 2.0 or later, Kestrel can run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7eada-167">Kestrel 通常會使用 [Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="7eada-167">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="7eada-168">Linux</span><span class="sxs-lookup"><span data-stu-id="7eada-168">Linux</span></span>](#tab/linux)

<span data-ttu-id="7eada-169">ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="7eada-169">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7eada-170">在 ASP.NET Core 2.0 或更新版本中，Kestrel 可以當做直接向網際網路公開的公眾邊緣伺服器來執行。</span><span class="sxs-lookup"><span data-stu-id="7eada-170">In ASP.NET Core 2.0 or later, Kestrel can run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7eada-171">Kestrel 通常會使用 [Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="7eada-171">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

<span data-ttu-id="7eada-172">如需詳細資訊，請參閱 <xref:fundamentals/servers/index> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-172">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="7eada-173">設定</span><span class="sxs-lookup"><span data-stu-id="7eada-173">Configuration</span></span>

<span data-ttu-id="7eada-174">ASP.NET Core 提供組態架構，可從組態提供者的已排序集合中，以成對名稱和數值的形式取得設定。</span><span class="sxs-lookup"><span data-stu-id="7eada-174">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="7eada-175">內建的設定提供者適用于各種來源，例如*json*檔案、 *.xml*檔案、環境變數和命令列引數。</span><span class="sxs-lookup"><span data-stu-id="7eada-175">Built-in configuration providers are available for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="7eada-176">撰寫自訂設定提供者以支援其他來源。</span><span class="sxs-lookup"><span data-stu-id="7eada-176">Write custom configuration providers to support other sources.</span></span>

<span data-ttu-id="7eada-177">根據[預設](xref:fundamentals/configuration/index#default)，ASP.NET Core 應用程式會設定為從*appsettings.js*、環境變數、命令列等進行讀取。</span><span class="sxs-lookup"><span data-stu-id="7eada-177">By [default](xref:fundamentals/configuration/index#default), ASP.NET Core apps are configured to read from *appsettings.json*, environment variables, the command line, and more.</span></span> <span data-ttu-id="7eada-178">載入應用程式的設定時，來自環境變數的值會覆寫來自*appsettings.js的*值。</span><span class="sxs-lookup"><span data-stu-id="7eada-178">When the app's configuration is loaded, values from environment variables override values from *appsettings.json*.</span></span>

<span data-ttu-id="7eada-179">讀取相關設定值的慣用方法是使用[選項模式](xref:fundamentals/configuration/options)。</span><span class="sxs-lookup"><span data-stu-id="7eada-179">The preferred way to read related configuration values is using the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="7eada-180">如需詳細資訊，請參閱[使用選項模式](xref:fundamentals/configuration/index#optpat)系結階層式設定資料。</span><span class="sxs-lookup"><span data-stu-id="7eada-180">For more information, see [Bind hierarchical configuration data using the options pattern](xref:fundamentals/configuration/index#optpat).</span></span>

<span data-ttu-id="7eada-181">若要管理機密設定資料（例如密碼），ASP.NET Core 提供[秘密管理員](xref:security/app-secrets#secret-manager)。</span><span class="sxs-lookup"><span data-stu-id="7eada-181">For managing confidential configuration data such as passwords, ASP.NET Core provides the [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="7eada-182">針對生產祕密，我們建議使用 [Azure Key Vault](xref:security/key-vault-configuration)。</span><span class="sxs-lookup"><span data-stu-id="7eada-182">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="7eada-183">如需詳細資訊，請參閱 <xref:fundamentals/configuration/index> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-183">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="environments"></a><span data-ttu-id="7eada-184">環境</span><span class="sxs-lookup"><span data-stu-id="7eada-184">Environments</span></span>

<span data-ttu-id="7eada-185">執行環境（例如 `Development` 、 `Staging` 和 `Production` ）是 ASP.NET Core 中的第一級概念。</span><span class="sxs-lookup"><span data-stu-id="7eada-185">Execution environments, such as `Development`, `Staging`, and `Production`, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="7eada-186">藉由設定環境變數，來指定應用程式正在執行的環境 `ASPNETCORE_ENVIRONMENT` 。</span><span class="sxs-lookup"><span data-stu-id="7eada-186">Specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="7eada-187">ASP.NET Core 會在應用程式啟動時讀取環境變數，然後將值儲存在 `IWebHostEnvironment` 實作中。</span><span class="sxs-lookup"><span data-stu-id="7eada-187">ASP.NET Core reads that environment variable at app startup and stores the value in an `IWebHostEnvironment` implementation.</span></span> <span data-ttu-id="7eada-188">透過相依性插入（DI），即可在應用程式中的任何位置使用此實作為。</span><span class="sxs-lookup"><span data-stu-id="7eada-188">This implementation is available anywhere in an app via dependency injection (DI).</span></span>

<span data-ttu-id="7eada-189">下列範例會設定應用程式，以在環境中執行時提供詳細的錯誤資訊 `Development` ：</span><span class="sxs-lookup"><span data-stu-id="7eada-189">The following example configures the app to provide detailed error information when running in the `Development` environment:</span></span>

[!code-csharp[](index/samples_snapshot/3.x/StartupConfigure.cs?highlight=3-6)]

<span data-ttu-id="7eada-190">如需詳細資訊，請參閱 <xref:fundamentals/environments> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-190">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="7eada-191">記錄</span><span class="sxs-lookup"><span data-stu-id="7eada-191">Logging</span></span>

<span data-ttu-id="7eada-192">ASP.NET Core 支援適用於各種內建和協力廠商記錄提供者的記錄 API。</span><span class="sxs-lookup"><span data-stu-id="7eada-192">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="7eada-193">可用的提供者包括：</span><span class="sxs-lookup"><span data-stu-id="7eada-193">Available providers include:</span></span>

* <span data-ttu-id="7eada-194">主控台</span><span class="sxs-lookup"><span data-stu-id="7eada-194">Console</span></span>
* <span data-ttu-id="7eada-195">偵錯</span><span class="sxs-lookup"><span data-stu-id="7eada-195">Debug</span></span>
* <span data-ttu-id="7eada-196">Windows 上的事件追蹤</span><span class="sxs-lookup"><span data-stu-id="7eada-196">Event Tracing on Windows</span></span>
* <span data-ttu-id="7eada-197">Windows 事件日誌</span><span class="sxs-lookup"><span data-stu-id="7eada-197">Windows Event Log</span></span>
* <span data-ttu-id="7eada-198">TraceSource</span><span class="sxs-lookup"><span data-stu-id="7eada-198">TraceSource</span></span>
* <span data-ttu-id="7eada-199">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="7eada-199">Azure App Service</span></span>
* <span data-ttu-id="7eada-200">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="7eada-200">Azure Application Insights</span></span>

<span data-ttu-id="7eada-201">若要建立記錄，請 <xref:Microsoft.Extensions.Logging.ILogger%601> 從相依性插入（DI）解析服務，然後通話記錄方法（例如） <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-201">To create logs, resolve an <xref:Microsoft.Extensions.Logging.ILogger%601> service from dependency injection (DI) and call logging methods such as <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>.</span></span> <span data-ttu-id="7eada-202">例如：</span><span class="sxs-lookup"><span data-stu-id="7eada-202">For example:</span></span>

[!code-csharp[](index/samples_snapshot/3.x/TodoController.cs?highlight=5,13,19)]

<span data-ttu-id="7eada-203">記錄方法（例如 `LogInformation` 支援任意數目的欄位）。</span><span class="sxs-lookup"><span data-stu-id="7eada-203">Logging methods such as `LogInformation` support any number of fields.</span></span> <span data-ttu-id="7eada-204">這些欄位通常用來建立訊息 `string` ，但是某些記錄提供者會以個別欄位的形式，將它們傳送到資料存放區。</span><span class="sxs-lookup"><span data-stu-id="7eada-204">These fields are commonly used to construct a message `string`, but some logging providers send these to a data store as separate fields.</span></span> <span data-ttu-id="7eada-205">這項功能可讓記錄提供者實作 [semantic logging (語意記錄)，又稱為 structured logging (結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。</span><span class="sxs-lookup"><span data-stu-id="7eada-205">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="7eada-206">如需詳細資訊，請參閱 <xref:fundamentals/logging/index> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-206">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="7eada-207">路由</span><span class="sxs-lookup"><span data-stu-id="7eada-207">Routing</span></span>

<span data-ttu-id="7eada-208">「路由」\*\* 是一種對應到處理常式的 URL 模式。</span><span class="sxs-lookup"><span data-stu-id="7eada-208">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="7eada-209">處理常式通常是 Razor 頁面、MVC 控制器中的動作方法或中介軟體。</span><span class="sxs-lookup"><span data-stu-id="7eada-209">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="7eada-210">ASP.NET Core 路由可讓您控制您應用程式使用的 URL。</span><span class="sxs-lookup"><span data-stu-id="7eada-210">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="7eada-211">如需詳細資訊，請參閱 <xref:fundamentals/routing> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-211">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="7eada-212">錯誤處理</span><span class="sxs-lookup"><span data-stu-id="7eada-212">Error handling</span></span>

<span data-ttu-id="7eada-213">ASP.NET Core 具有處理錯誤的內建功能，例如：</span><span class="sxs-lookup"><span data-stu-id="7eada-213">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="7eada-214">開發人員例外狀況頁面</span><span class="sxs-lookup"><span data-stu-id="7eada-214">A developer exception page</span></span>
* <span data-ttu-id="7eada-215">自訂錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="7eada-215">Custom error pages</span></span>
* <span data-ttu-id="7eada-216">靜態狀態碼頁面</span><span class="sxs-lookup"><span data-stu-id="7eada-216">Static status code pages</span></span>
* <span data-ttu-id="7eada-217">啟動例外狀況處理</span><span class="sxs-lookup"><span data-stu-id="7eada-217">Startup exception handling</span></span>

<span data-ttu-id="7eada-218">如需詳細資訊，請參閱 <xref:fundamentals/error-handling> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-218">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="7eada-219">發出 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="7eada-219">Make HTTP requests</span></span>

<span data-ttu-id="7eada-220">`IHttpClientFactory` 的實作可用於建立 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="7eada-220">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="7eada-221">Factory：</span><span class="sxs-lookup"><span data-stu-id="7eada-221">The factory:</span></span>

* <span data-ttu-id="7eada-222">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="7eada-222">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="7eada-223">例如，註冊並設定*github*用戶端以存取 github。</span><span class="sxs-lookup"><span data-stu-id="7eada-223">For example, register and configure a *github* client for accessing GitHub.</span></span> <span data-ttu-id="7eada-224">針對其他用途，註冊並設定預設用戶端。</span><span class="sxs-lookup"><span data-stu-id="7eada-224">Register and configure a default client for other purposes.</span></span>
* <span data-ttu-id="7eada-225">支援註冊及多個委派處理常式的鏈結，以用於建置傳出要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="7eada-225">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="7eada-226">此模式類似于 ASP.NET Core 的輸入中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="7eada-226">This pattern is similar to ASP.NET Core's inbound middleware pipeline.</span></span> <span data-ttu-id="7eada-227">模式提供一種機制來管理 HTTP 要求的跨領域考慮，包括快取、錯誤處理、序列化和記錄。</span><span class="sxs-lookup"><span data-stu-id="7eada-227">The pattern provides a mechanism to manage cross-cutting concerns for HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="7eada-228">與 *Polly* 整合，Polly 是一種熱門的協力廠商程式庫，用於進行暫時性的錯誤處理。</span><span class="sxs-lookup"><span data-stu-id="7eada-228">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="7eada-229">管理基礎實例的共用和存留期， `HttpClientHandler` 以避免手動管理存留期時所發生的常見 DNS 問題 `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="7eada-229">Manages the pooling and lifetime of underlying `HttpClientHandler` instances to avoid common DNS problems that occur when managing `HttpClient` lifetimes manually.</span></span>
* <span data-ttu-id="7eada-230">透過處理站 <xref:Microsoft.Extensions.Logging.ILogger> 所建立的用戶端所傳送的所有要求，新增可設定的記錄體驗。</span><span class="sxs-lookup"><span data-stu-id="7eada-230">Adds a configurable logging experience via <xref:Microsoft.Extensions.Logging.ILogger> for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="7eada-231">如需詳細資訊，請參閱 <xref:fundamentals/http-requests> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-231">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="7eada-232">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="7eada-232">Content root</span></span>

<span data-ttu-id="7eada-233">內容根目錄是的基底路徑：</span><span class="sxs-lookup"><span data-stu-id="7eada-233">The content root is the base path for:</span></span>

* <span data-ttu-id="7eada-234">裝載應用程式的可執行檔（*.exe*）。</span><span class="sxs-lookup"><span data-stu-id="7eada-234">The executable hosting the app (*.exe*).</span></span>
* <span data-ttu-id="7eada-235">組成應用程式的已編譯元件（*.dll*）。</span><span class="sxs-lookup"><span data-stu-id="7eada-235">Compiled assemblies that make up the app (*.dll*).</span></span>
* <span data-ttu-id="7eada-236">應用程式所使用的內容檔案，例如：</span><span class="sxs-lookup"><span data-stu-id="7eada-236">Content files used by the app, such as:</span></span>
  * Razor<span data-ttu-id="7eada-237">files （*. cshtml*， *razor*）</span><span class="sxs-lookup"><span data-stu-id="7eada-237"> files (*.cshtml*, *.razor*)</span></span>
  * <span data-ttu-id="7eada-238">設定檔（*. json*、 *.xml*）</span><span class="sxs-lookup"><span data-stu-id="7eada-238">Configuration files (*.json*, *.xml*)</span></span>
  * <span data-ttu-id="7eada-239">資料檔案（*.db*）</span><span class="sxs-lookup"><span data-stu-id="7eada-239">Data files (*.db*)</span></span>
* <span data-ttu-id="7eada-240">[Web 根目錄](#web-root)，通常是*wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="7eada-240">The [Web root](#web-root), typically the *wwwroot* folder.</span></span>

<span data-ttu-id="7eada-241">在開發期間，內容根目錄預設為專案的根目錄。</span><span class="sxs-lookup"><span data-stu-id="7eada-241">During development, the content root defaults to the project's root directory.</span></span> <span data-ttu-id="7eada-242">此目錄也是應用程式內容檔案和[Web 根目錄](#web-root)的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="7eada-242">This directory is also the base path for both the app's content files and the [Web root](#web-root).</span></span> <span data-ttu-id="7eada-243">[建立主機](#host)時，請設定其路徑，以指定不同的內容根目錄。</span><span class="sxs-lookup"><span data-stu-id="7eada-243">Specify a different content root by setting its path when [building the host](#host).</span></span> <span data-ttu-id="7eada-244">如需詳細資訊，請參閱[內容根](xref:fundamentals/host/generic-host#contentroot)。</span><span class="sxs-lookup"><span data-stu-id="7eada-244">For more information, see [Content root](xref:fundamentals/host/generic-host#contentroot).</span></span>

## <a name="web-root"></a><span data-ttu-id="7eada-245">Web 根目錄</span><span class="sxs-lookup"><span data-stu-id="7eada-245">Web root</span></span>

<span data-ttu-id="7eada-246">Web 根目錄是公用、靜態資源檔的基底路徑，例如：</span><span class="sxs-lookup"><span data-stu-id="7eada-246">The web root is the base path for public, static resource files, such as:</span></span>

* <span data-ttu-id="7eada-247">樣式表單（*.css*）</span><span class="sxs-lookup"><span data-stu-id="7eada-247">Stylesheets (*.css*)</span></span>
* <span data-ttu-id="7eada-248">JavaScript （*.js*）</span><span class="sxs-lookup"><span data-stu-id="7eada-248">JavaScript (*.js*)</span></span>
* <span data-ttu-id="7eada-249">影像（*.png*、 *.jpg*）</span><span class="sxs-lookup"><span data-stu-id="7eada-249">Images (*.png*, *.jpg*)</span></span>

<span data-ttu-id="7eada-250">根據預設，靜態檔案只會從 web 根目錄和其子目錄中提供。</span><span class="sxs-lookup"><span data-stu-id="7eada-250">By default, static files are served only from the web root directory and its sub-directories.</span></span> <span data-ttu-id="7eada-251">Web 根目錄路徑預設為 *{content root}/wwwroot*。</span><span class="sxs-lookup"><span data-stu-id="7eada-251">The web root path defaults to *{content root}/wwwroot*.</span></span> <span data-ttu-id="7eada-252">[建立主機](#host)時，請設定其路徑，以指定不同的 web 根目錄。</span><span class="sxs-lookup"><span data-stu-id="7eada-252">Specify a different web root by setting its path when [building the host](#host).</span></span> <span data-ttu-id="7eada-253">如需詳細資訊，請參閱 [Web 根目錄](xref:fundamentals/host/generic-host#webroot)。</span><span class="sxs-lookup"><span data-stu-id="7eada-253">For more information, see [Web root](xref:fundamentals/host/generic-host#webroot).</span></span>

<span data-ttu-id="7eada-254">防止在*wwwroot*中使用專案檔中的[ \<Content> 專案專案](/visualstudio/msbuild/common-msbuild-project-items#content)發行檔案。</span><span class="sxs-lookup"><span data-stu-id="7eada-254">Prevent publishing files in *wwwroot* with the [\<Content> project item](/visualstudio/msbuild/common-msbuild-project-items#content) in the project file.</span></span> <span data-ttu-id="7eada-255">下列範例會防止在*wwwroot/local*和其子目錄中發佈內容：</span><span class="sxs-lookup"><span data-stu-id="7eada-255">The following example prevents publishing content in *wwwroot/local* and its sub-directories:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="7eada-256">在 cshtml 檔案中，波狀符號 Razor *.cshtml* -斜線（ `~/` ）會指向 web 根目錄。</span><span class="sxs-lookup"><span data-stu-id="7eada-256">In Razor *.cshtml* files, tilde-slash (`~/`) points to the web root.</span></span> <span data-ttu-id="7eada-257">開頭為的路徑 `~/` 稱為*虛擬路徑*。</span><span class="sxs-lookup"><span data-stu-id="7eada-257">A path beginning with `~/` is referred to as a *virtual path*.</span></span>

<span data-ttu-id="7eada-258">如需詳細資訊，請參閱 <xref:fundamentals/static-files> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-258">For more information, see <xref:fundamentals/static-files>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7eada-259">本文是了解如何開發 ASP.NET Core 應用程式的關鍵主題概觀。</span><span class="sxs-lookup"><span data-stu-id="7eada-259">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="7eada-260">Startup 類別</span><span class="sxs-lookup"><span data-stu-id="7eada-260">The Startup class</span></span>

<span data-ttu-id="7eada-261">`Startup` 類別是：</span><span class="sxs-lookup"><span data-stu-id="7eada-261">The `Startup` class is where:</span></span>

* <span data-ttu-id="7eada-262">已設定應用程式所需的服務。</span><span class="sxs-lookup"><span data-stu-id="7eada-262">Services required by the app are configured.</span></span>
* <span data-ttu-id="7eada-263">定義要求處理管線的位置。</span><span class="sxs-lookup"><span data-stu-id="7eada-263">The request handling pipeline is defined.</span></span>

<span data-ttu-id="7eada-264">「服務」\*\* 是應用程式所使用的元件。</span><span class="sxs-lookup"><span data-stu-id="7eada-264">*Services* are components that are used by the app.</span></span> <span data-ttu-id="7eada-265">例如，記錄元件即為一項服務。</span><span class="sxs-lookup"><span data-stu-id="7eada-265">For example, a logging component is a service.</span></span> <span data-ttu-id="7eada-266">設定 (或「註冊」\*\*) 服務的程式碼會新增至 `Startup.ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="7eada-266">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="7eada-267">要求處理管線是由一系列*中介軟體*元件所組成。</span><span class="sxs-lookup"><span data-stu-id="7eada-267">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="7eada-268">例如，中介軟體可能會處理靜態檔案的要求，或是將 HTTP 要求重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="7eada-268">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="7eada-269">每個中介軟體會在 `HttpContext` 上執行非同步操作，然後叫用管線中下一個中介軟體或終止要求。</span><span class="sxs-lookup"><span data-stu-id="7eada-269">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="7eada-270">設定要求處理管線的程式碼會新增至 `Startup.Configure` 方法。</span><span class="sxs-lookup"><span data-stu-id="7eada-270">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="7eada-271">以下是 `Startup` 類別範例：</span><span class="sxs-lookup"><span data-stu-id="7eada-271">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/samples_snapshot/2.x/Startup.cs?highlight=3,12)]

<span data-ttu-id="7eada-272">如需詳細資訊，請參閱 <xref:fundamentals/startup> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-272">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="7eada-273">相依性插入 (服務)</span><span class="sxs-lookup"><span data-stu-id="7eada-273">Dependency injection (services)</span></span>

<span data-ttu-id="7eada-274">ASP.NET Core 具有內建的相依性插入 (DI) 架構，可讓應用程式的類別使用所設定服務。</span><span class="sxs-lookup"><span data-stu-id="7eada-274">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="7eada-275">取得類別中一項服務執行個體的其中一種方式，便是使用所需類型的參數來建立建構函式。</span><span class="sxs-lookup"><span data-stu-id="7eada-275">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="7eada-276">參數可以是服務類型或是介面。</span><span class="sxs-lookup"><span data-stu-id="7eada-276">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="7eada-277">DI 系統會在執行階段提供服務。</span><span class="sxs-lookup"><span data-stu-id="7eada-277">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="7eada-278">以下是使用 DI 取得 Entity Framework Core 內容物件的類別。</span><span class="sxs-lookup"><span data-stu-id="7eada-278">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="7eada-279">醒目提示的那一行便是建構函式插入的範例：</span><span class="sxs-lookup"><span data-stu-id="7eada-279">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/samples_snapshot/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="7eada-280">雖然 DI 為內建，但其設計用於讓您插入協力廠商的控制反轉 (IoC) 容器 (若您想要的話)。</span><span class="sxs-lookup"><span data-stu-id="7eada-280">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="7eada-281">如需詳細資訊，請參閱 <xref:fundamentals/dependency-injection> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-281">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="7eada-282">中介軟體</span><span class="sxs-lookup"><span data-stu-id="7eada-282">Middleware</span></span>

<span data-ttu-id="7eada-283">要求處理管線是以一系列中介軟體元件組成。</span><span class="sxs-lookup"><span data-stu-id="7eada-283">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="7eada-284">每個元件會在 `HttpContext` 上執行非同步操作，然後叫用管線中下一個中介軟體或終止要求。</span><span class="sxs-lookup"><span data-stu-id="7eada-284">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="7eada-285">依照慣例，中介軟體元件會透過叫用其 `Startup.Configure` 方法中的 `Use...` 延伸模組方法來新增至管線。</span><span class="sxs-lookup"><span data-stu-id="7eada-285">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="7eada-286">例如，若要啟用靜態檔案轉譯，請呼叫 `UseStaticFiles`。</span><span class="sxs-lookup"><span data-stu-id="7eada-286">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="7eada-287">下列範例中醒目提示的程式碼會設定要求處理管線：</span><span class="sxs-lookup"><span data-stu-id="7eada-287">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/samples_snapshot/2.x/Startup.cs?highlight=14-16)]

<span data-ttu-id="7eada-288">ASP.NET Core 包含一組豐富的內建中介軟體，您也可以撰寫自訂中介軟體。</span><span class="sxs-lookup"><span data-stu-id="7eada-288">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="7eada-289">如需詳細資訊，請參閱 <xref:fundamentals/middleware/index> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-289">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="7eada-290">Host</span><span class="sxs-lookup"><span data-stu-id="7eada-290">Host</span></span>

<span data-ttu-id="7eada-291">ASP.NET Core 應用程式會在啟動時建置一個「主機」\*\*。</span><span class="sxs-lookup"><span data-stu-id="7eada-291">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="7eada-292">主機是封裝所有應用程式資源的物件，例如：</span><span class="sxs-lookup"><span data-stu-id="7eada-292">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="7eada-293">HTTP 伺服器實作</span><span class="sxs-lookup"><span data-stu-id="7eada-293">An HTTP server implementation</span></span>
* <span data-ttu-id="7eada-294">中介軟體元件</span><span class="sxs-lookup"><span data-stu-id="7eada-294">Middleware components</span></span>
* <span data-ttu-id="7eada-295">記錄</span><span class="sxs-lookup"><span data-stu-id="7eada-295">Logging</span></span>
* <span data-ttu-id="7eada-296">DI</span><span class="sxs-lookup"><span data-stu-id="7eada-296">DI</span></span>
* <span data-ttu-id="7eada-297">設定</span><span class="sxs-lookup"><span data-stu-id="7eada-297">Configuration</span></span>

<span data-ttu-id="7eada-298">在單一物件中包含所有應用程式相互依存資源的主要理由便是生命週期管理：控制應用程式的啟動及順利關機。</span><span class="sxs-lookup"><span data-stu-id="7eada-298">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="7eada-299">可以使用兩種主機：Web 主機與一般主機。</span><span class="sxs-lookup"><span data-stu-id="7eada-299">Two hosts are available: the Web Host and the Generic Host.</span></span> <span data-ttu-id="7eada-300">在 ASP.NET Core 2.x 中，一般主機僅適用於非 Web 案例。</span><span class="sxs-lookup"><span data-stu-id="7eada-300">In ASP.NET Core 2.x, the Generic Host is only for non-web scenarios.</span></span>

<span data-ttu-id="7eada-301">建立主機的程式碼位於 `Program.Main` 中：</span><span class="sxs-lookup"><span data-stu-id="7eada-301">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/samples_snapshot/2.x/Program.cs)]

<span data-ttu-id="7eada-302">`CreateDefaultBuilder` 方法會設定具備一般常用選項的主機，如下所示：</span><span class="sxs-lookup"><span data-stu-id="7eada-302">The `CreateDefaultBuilder` method configures a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="7eada-303">使用 [Kestrel](#servers) 作為網頁伺服器，並啟用 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="7eada-303">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="7eada-304">從 *appsettings.json*、*appsettings.{Environment Name}.json*、環境變數與命令列引數載入設定。</span><span class="sxs-lookup"><span data-stu-id="7eada-304">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="7eada-305">將記錄輸出傳送到主控台及偵錯提供者。</span><span class="sxs-lookup"><span data-stu-id="7eada-305">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="7eada-306">如需詳細資訊，請參閱 <xref:fundamentals/host/web-host> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-306">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="non-web-scenarios"></a><span data-ttu-id="7eada-307">非 Web 案例</span><span class="sxs-lookup"><span data-stu-id="7eada-307">Non-web scenarios</span></span>

<span data-ttu-id="7eada-308">一般主機允許其他類型的應用程式，使用交叉剪輯架構延伸模組，例如記錄、相依性插入 (DI)、設定與應用程式存留期管理。</span><span class="sxs-lookup"><span data-stu-id="7eada-308">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="7eada-309">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host> 和 <xref:fundamentals/host/hosted-services>。</span><span class="sxs-lookup"><span data-stu-id="7eada-309">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="7eada-310">伺服器</span><span class="sxs-lookup"><span data-stu-id="7eada-310">Servers</span></span>

<span data-ttu-id="7eada-311">ASP.NET Core 應用程式使用 HTTP 伺服器實作來接聽 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="7eada-311">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="7eada-312">伺服器會把向應用程式發出的要求作為一組[要求功能](xref:fundamentals/request-features)，合併成一個 `HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="7eada-312">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

# <a name="windows"></a>[<span data-ttu-id="7eada-313">Windows</span><span class="sxs-lookup"><span data-stu-id="7eada-313">Windows</span></span>](#tab/windows)

<span data-ttu-id="7eada-314">ASP.NET Core 隨附下列伺服器實作：</span><span class="sxs-lookup"><span data-stu-id="7eada-314">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="7eada-315">*Kestrel* 是跨平台的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="7eada-315">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="7eada-316">Kestrel 通常會使用 [IIS](https://www.iis.net/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="7eada-316">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="7eada-317">Kestrel 可以當做直接向網際網路公開的公眾面向邊緣伺服器來執行。</span><span class="sxs-lookup"><span data-stu-id="7eada-317">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="7eada-318">*IIS HTTP 伺服器*則是適用於使用 IIS Windows 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="7eada-318">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="7eada-319">透過此伺服器，ASP.NET Core 應用程式及 IIS 便可以在相同處理序中執行。</span><span class="sxs-lookup"><span data-stu-id="7eada-319">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="7eada-320">*HTTP.sys* 則是適用於並未搭配 IIS 使用 Windows 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="7eada-320">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="7eada-321">macOS</span><span class="sxs-lookup"><span data-stu-id="7eada-321">macOS</span></span>](#tab/macos)

<span data-ttu-id="7eada-322">ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="7eada-322">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7eada-323">Kestrel 可以當做直接向網際網路公開的公眾面向邊緣伺服器來執行。</span><span class="sxs-lookup"><span data-stu-id="7eada-323">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7eada-324">Kestrel 通常會使用 [Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="7eada-324">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="7eada-325">Linux</span><span class="sxs-lookup"><span data-stu-id="7eada-325">Linux</span></span>](#tab/linux)

<span data-ttu-id="7eada-326">ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="7eada-326">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7eada-327">Kestrel 可以當做直接向網際網路公開的公眾面向邊緣伺服器來執行。</span><span class="sxs-lookup"><span data-stu-id="7eada-327">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7eada-328">Kestrel 通常會使用 [Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="7eada-328">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windows"></a>[<span data-ttu-id="7eada-329">Windows</span><span class="sxs-lookup"><span data-stu-id="7eada-329">Windows</span></span>](#tab/windows)

<span data-ttu-id="7eada-330">ASP.NET Core 隨附下列伺服器實作：</span><span class="sxs-lookup"><span data-stu-id="7eada-330">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="7eada-331">*Kestrel* 是跨平台的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="7eada-331">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="7eada-332">Kestrel 通常會使用 [IIS](https://www.iis.net/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="7eada-332">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="7eada-333">Kestrel 可以當做直接向網際網路公開的公眾面向邊緣伺服器來執行。</span><span class="sxs-lookup"><span data-stu-id="7eada-333">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="7eada-334">*HTTP.sys* 則是適用於並未搭配 IIS 使用 Windows 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="7eada-334">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="7eada-335">macOS</span><span class="sxs-lookup"><span data-stu-id="7eada-335">macOS</span></span>](#tab/macos)

<span data-ttu-id="7eada-336">ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="7eada-336">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7eada-337">Kestrel 可以當做直接向網際網路公開的公眾面向邊緣伺服器來執行。</span><span class="sxs-lookup"><span data-stu-id="7eada-337">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7eada-338">Kestrel 通常會使用 [Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="7eada-338">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="7eada-339">Linux</span><span class="sxs-lookup"><span data-stu-id="7eada-339">Linux</span></span>](#tab/linux)

<span data-ttu-id="7eada-340">ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="7eada-340">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="7eada-341">Kestrel 可以當做直接向網際網路公開的公眾面向邊緣伺服器來執行。</span><span class="sxs-lookup"><span data-stu-id="7eada-341">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="7eada-342">Kestrel 通常會使用 [Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="7eada-342">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7eada-343">如需詳細資訊，請參閱 <xref:fundamentals/servers/index> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-343">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="7eada-344">設定</span><span class="sxs-lookup"><span data-stu-id="7eada-344">Configuration</span></span>

<span data-ttu-id="7eada-345">ASP.NET Core 提供組態架構，可從組態提供者的已排序集合中，以成對名稱和數值的形式取得設定。</span><span class="sxs-lookup"><span data-stu-id="7eada-345">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="7eada-346">您可以使用各種來源的內建組態提供者，例如 *.json* 檔案、*.xml* 檔案、環境變數及命令列引數。</span><span class="sxs-lookup"><span data-stu-id="7eada-346">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="7eada-347">您也可以撰寫自訂組態提供者。</span><span class="sxs-lookup"><span data-stu-id="7eada-347">You can also write custom configuration providers.</span></span>

<span data-ttu-id="7eada-348">例如，您可以指定組態來自 *appsettings.json* 和環境變數。</span><span class="sxs-lookup"><span data-stu-id="7eada-348">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="7eada-349">然後當要求 *ConnectionString* 的值時，架構便會先在 *appsettings.json* 檔案中尋找。</span><span class="sxs-lookup"><span data-stu-id="7eada-349">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="7eada-350">若有找到值，但在環境變數中也存在該值，則會優先使用來自環境變數的值。</span><span class="sxs-lookup"><span data-stu-id="7eada-350">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="7eada-351">針對管理保密組態資料 (例如密碼)，ASP.NET Core 提供[祕密管理員工具](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="7eada-351">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="7eada-352">針對生產祕密，我們建議使用 [Azure Key Vault](xref:security/key-vault-configuration)。</span><span class="sxs-lookup"><span data-stu-id="7eada-352">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="7eada-353">如需詳細資訊，請參閱 <xref:fundamentals/configuration/index> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-353">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="7eada-354">選項</span><span class="sxs-lookup"><span data-stu-id="7eada-354">Options</span></span>

<span data-ttu-id="7eada-355">在可能的情況下，ASP.NET Core 會遵循「選項模式」\*\* 來儲存及擷取組態值。</span><span class="sxs-lookup"><span data-stu-id="7eada-355">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="7eada-356">選項模式使用類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="7eada-356">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="7eada-357">例如，下列程式碼會設定 WebSocket 選項：</span><span class="sxs-lookup"><span data-stu-id="7eada-357">For example, the following code sets WebSockets options:</span></span>

[!code-csharp[](index/samples_snapshot/2.x/UseWebSockets.cs)]

<span data-ttu-id="7eada-358">如需詳細資訊，請參閱 <xref:fundamentals/configuration/options> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-358">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="7eada-359">環境</span><span class="sxs-lookup"><span data-stu-id="7eada-359">Environments</span></span>

<span data-ttu-id="7eada-360">執行環境 (例如「開發」**、「預備」** 及「生產」\*\*) 是 ASP.NET Core 中的第一級概念。</span><span class="sxs-lookup"><span data-stu-id="7eada-360">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="7eada-361">您可以透過設定 `ASPNETCORE_ENVIRONMENT` 環境變數，指定應用程式執行的環境。</span><span class="sxs-lookup"><span data-stu-id="7eada-361">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="7eada-362">ASP.NET Core 會在應用程式啟動時讀取環境變數，然後將值儲存在 `IHostingEnvironment` 實作中。</span><span class="sxs-lookup"><span data-stu-id="7eada-362">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="7eada-363">環境物件可在應用程式中的任何位置，透過 DI 使用。</span><span class="sxs-lookup"><span data-stu-id="7eada-363">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="7eada-364">下列 `Startup` 類別的範例程式碼會設定應用程式，只在於「開發」環境中執行時提供詳細的錯誤資訊：</span><span class="sxs-lookup"><span data-stu-id="7eada-364">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/samples_snapshot/2.x/StartupConfigure.cs?highlight=3-6)]

<span data-ttu-id="7eada-365">如需詳細資訊，請參閱 <xref:fundamentals/environments> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-365">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="7eada-366">記錄</span><span class="sxs-lookup"><span data-stu-id="7eada-366">Logging</span></span>

<span data-ttu-id="7eada-367">ASP.NET Core 支援適用於各種內建和協力廠商記錄提供者的記錄 API。</span><span class="sxs-lookup"><span data-stu-id="7eada-367">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="7eada-368">可用的提供者包括如下：</span><span class="sxs-lookup"><span data-stu-id="7eada-368">Available providers include the following:</span></span>

* <span data-ttu-id="7eada-369">主控台</span><span class="sxs-lookup"><span data-stu-id="7eada-369">Console</span></span>
* <span data-ttu-id="7eada-370">偵錯</span><span class="sxs-lookup"><span data-stu-id="7eada-370">Debug</span></span>
* <span data-ttu-id="7eada-371">Windows 上的事件追蹤</span><span class="sxs-lookup"><span data-stu-id="7eada-371">Event Tracing on Windows</span></span>
* <span data-ttu-id="7eada-372">Windows 事件日誌</span><span class="sxs-lookup"><span data-stu-id="7eada-372">Windows Event Log</span></span>
* <span data-ttu-id="7eada-373">TraceSource</span><span class="sxs-lookup"><span data-stu-id="7eada-373">TraceSource</span></span>
* <span data-ttu-id="7eada-374">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="7eada-374">Azure App Service</span></span>
* <span data-ttu-id="7eada-375">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="7eada-375">Azure Application Insights</span></span>

<span data-ttu-id="7eada-376">透過從 DI 取得 `ILogger` 物件並呼叫記錄方法，從應用程式程式碼中的任何位置寫入記錄。</span><span class="sxs-lookup"><span data-stu-id="7eada-376">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="7eada-377">以下是使用 `ILogger` 物件的範例程式碼，其中建構函式插入及記錄方法呼叫都已醒目提示。</span><span class="sxs-lookup"><span data-stu-id="7eada-377">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/samples_snapshot/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="7eada-378">`ILogger` 介面可讓您將任何數量的欄位傳遞給記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="7eada-378">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="7eada-379">欄位常用於建構訊息字串，但提供者也可以將它們作為個別欄位，傳送至資料存放區。</span><span class="sxs-lookup"><span data-stu-id="7eada-379">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="7eada-380">這項功能可讓記錄提供者實作 [semantic logging (語意記錄)，又稱為 structured logging (結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。</span><span class="sxs-lookup"><span data-stu-id="7eada-380">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="7eada-381">如需詳細資訊，請參閱 <xref:fundamentals/logging/index> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-381">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="7eada-382">路由</span><span class="sxs-lookup"><span data-stu-id="7eada-382">Routing</span></span>

<span data-ttu-id="7eada-383">「路由」\*\* 是一種對應到處理常式的 URL 模式。</span><span class="sxs-lookup"><span data-stu-id="7eada-383">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="7eada-384">處理常式通常是 Razor 頁面、MVC 控制器中的動作方法或中介軟體。</span><span class="sxs-lookup"><span data-stu-id="7eada-384">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="7eada-385">ASP.NET Core 路由可讓您控制您應用程式使用的 URL。</span><span class="sxs-lookup"><span data-stu-id="7eada-385">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="7eada-386">如需詳細資訊，請參閱 <xref:fundamentals/routing> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-386">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="7eada-387">錯誤處理</span><span class="sxs-lookup"><span data-stu-id="7eada-387">Error handling</span></span>

<span data-ttu-id="7eada-388">ASP.NET Core 具有處理錯誤的內建功能，例如：</span><span class="sxs-lookup"><span data-stu-id="7eada-388">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="7eada-389">開發人員例外狀況頁面</span><span class="sxs-lookup"><span data-stu-id="7eada-389">A developer exception page</span></span>
* <span data-ttu-id="7eada-390">自訂錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="7eada-390">Custom error pages</span></span>
* <span data-ttu-id="7eada-391">靜態狀態碼頁面</span><span class="sxs-lookup"><span data-stu-id="7eada-391">Static status code pages</span></span>
* <span data-ttu-id="7eada-392">啟動例外狀況處理</span><span class="sxs-lookup"><span data-stu-id="7eada-392">Startup exception handling</span></span>

<span data-ttu-id="7eada-393">如需詳細資訊，請參閱 <xref:fundamentals/error-handling> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-393">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="7eada-394">發出 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="7eada-394">Make HTTP requests</span></span>

<span data-ttu-id="7eada-395">`IHttpClientFactory` 的實作可用於建立 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="7eada-395">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="7eada-396">Factory：</span><span class="sxs-lookup"><span data-stu-id="7eada-396">The factory:</span></span>

* <span data-ttu-id="7eada-397">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="7eada-397">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="7eada-398">例如，您可以註冊並設定*github*用戶端來存取 github。</span><span class="sxs-lookup"><span data-stu-id="7eada-398">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="7eada-399">預設用戶端可以註冊用於其他用途。</span><span class="sxs-lookup"><span data-stu-id="7eada-399">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="7eada-400">支援註冊及多個委派處理常式的鏈結，以用於建置傳出要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="7eada-400">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="7eada-401">此模式與 ASP.NET Core 中的輸入中介軟體管線相似。</span><span class="sxs-lookup"><span data-stu-id="7eada-401">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="7eada-402">模式提供一個機制來管理 HTTP 要求的跨領域關注，包括快取、錯誤處理、序列化和記錄。</span><span class="sxs-lookup"><span data-stu-id="7eada-402">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="7eada-403">與 *Polly* 整合，Polly 是一種熱門的協力廠商程式庫，用於進行暫時性的錯誤處理。</span><span class="sxs-lookup"><span data-stu-id="7eada-403">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="7eada-404">管理基礎 `HttpClientHandler` 執行個體的共用和存留期，以避免在手動管理 `HttpClient` 存留期時，發生的常見 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="7eada-404">Manages the pooling and lifetime of underlying `HttpClientHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="7eada-405">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="7eada-405">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="7eada-406">如需詳細資訊，請參閱 <xref:fundamentals/http-requests> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-406">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="7eada-407">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="7eada-407">Content root</span></span>

<span data-ttu-id="7eada-408">內容根目錄是的基底路徑：</span><span class="sxs-lookup"><span data-stu-id="7eada-408">The content root is the base path to the:</span></span>

* <span data-ttu-id="7eada-409">裝載應用程式的可執行檔（*.exe*）。</span><span class="sxs-lookup"><span data-stu-id="7eada-409">Executable hosting the app (*.exe*).</span></span>
* <span data-ttu-id="7eada-410">組成應用程式的已編譯元件（*.dll*）。</span><span class="sxs-lookup"><span data-stu-id="7eada-410">Compiled assemblies that make up the app (*.dll*).</span></span>
* <span data-ttu-id="7eada-411">應用程式所使用的非程式碼內容檔案，例如：</span><span class="sxs-lookup"><span data-stu-id="7eada-411">Non-code content files used by the app, such as:</span></span>
  * Razor<span data-ttu-id="7eada-412">files （*. cshtml*， *razor*）</span><span class="sxs-lookup"><span data-stu-id="7eada-412"> files (*.cshtml*, *.razor*)</span></span>
  * <span data-ttu-id="7eada-413">設定檔（*. json*、 *.xml*）</span><span class="sxs-lookup"><span data-stu-id="7eada-413">Configuration files (*.json*, *.xml*)</span></span>
  * <span data-ttu-id="7eada-414">資料檔案（*.db*）</span><span class="sxs-lookup"><span data-stu-id="7eada-414">Data files (*.db*)</span></span>
* <span data-ttu-id="7eada-415">[Web 根目錄](#web-root)，通常是已發佈的*wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="7eada-415">[Web root](#web-root), typically the published *wwwroot* folder.</span></span>

<span data-ttu-id="7eada-416">在開發期間：</span><span class="sxs-lookup"><span data-stu-id="7eada-416">During development:</span></span>

* <span data-ttu-id="7eada-417">內容根目錄預設為專案的根目錄。</span><span class="sxs-lookup"><span data-stu-id="7eada-417">The content root defaults to the project's root directory.</span></span>
* <span data-ttu-id="7eada-418">專案的根目錄是用來建立：</span><span class="sxs-lookup"><span data-stu-id="7eada-418">The project's root directory is used to create the:</span></span>
  * <span data-ttu-id="7eada-419">應用程式在專案根目錄中的非程式碼內容檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="7eada-419">Path to the app's non-code content files in the project's root directory.</span></span>
  * <span data-ttu-id="7eada-420">[Web 根目錄](#web-root)，通常是專案根目錄中的*wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="7eada-420">[Web root](#web-root), typically the *wwwroot* folder in the project's root directory.</span></span>

<span data-ttu-id="7eada-421">[建立主機](#host)時，可以指定替代的內容根路徑。</span><span class="sxs-lookup"><span data-stu-id="7eada-421">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="7eada-422">如需詳細資訊，請參閱 <xref:fundamentals/host/web-host#content-root> 。</span><span class="sxs-lookup"><span data-stu-id="7eada-422">For more information, see <xref:fundamentals/host/web-host#content-root>.</span></span>

## <a name="web-root"></a><span data-ttu-id="7eada-423">Web 根目錄</span><span class="sxs-lookup"><span data-stu-id="7eada-423">Web root</span></span>

<span data-ttu-id="7eada-424">Web 根目錄是公用、非程式碼、靜態資源檔的基底路徑，例如：</span><span class="sxs-lookup"><span data-stu-id="7eada-424">The web root is the base path to public, non-code, static resource files, such as:</span></span>

* <span data-ttu-id="7eada-425">樣式表單（*.css*）</span><span class="sxs-lookup"><span data-stu-id="7eada-425">Stylesheets (*.css*)</span></span>
* <span data-ttu-id="7eada-426">JavaScript （*.js*）</span><span class="sxs-lookup"><span data-stu-id="7eada-426">JavaScript (*.js*)</span></span>
* <span data-ttu-id="7eada-427">影像（*.png*、 *.jpg*）</span><span class="sxs-lookup"><span data-stu-id="7eada-427">Images (*.png*, *.jpg*)</span></span>

<span data-ttu-id="7eada-428">根據預設，靜態檔案只會從 web 根目錄（和子目錄）提供。</span><span class="sxs-lookup"><span data-stu-id="7eada-428">Static files are only served by default from the web root directory (and sub-directories).</span></span>

<span data-ttu-id="7eada-429">Web 根目錄路徑預設為 *{content root}/wwwroot*，但在[建立主機](#host)時，可以指定不同的 web 根目錄。</span><span class="sxs-lookup"><span data-stu-id="7eada-429">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="7eada-430">如需詳細資訊，請參閱 [Web 根目錄](xref:fundamentals/host/web-host#web-root)。</span><span class="sxs-lookup"><span data-stu-id="7eada-430">For more information, see [Web root](xref:fundamentals/host/web-host#web-root).</span></span>

<span data-ttu-id="7eada-431">防止在*wwwroot*中使用專案檔中的[ \<Content> 專案專案](/visualstudio/msbuild/common-msbuild-project-items#content)發行檔案。</span><span class="sxs-lookup"><span data-stu-id="7eada-431">Prevent publishing files in *wwwroot* with the [\<Content> project item](/visualstudio/msbuild/common-msbuild-project-items#content) in the project file.</span></span> <span data-ttu-id="7eada-432">下列範例會防止在*wwwroot/本機*目錄和子目錄中發佈內容：</span><span class="sxs-lookup"><span data-stu-id="7eada-432">The following example prevents publishing content in the *wwwroot/local* directory and sub-directories:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="7eada-433">在 Razor （*. cshtml*）檔案中，波狀符號-斜線（ `~/` ）會指向 web 根目錄。</span><span class="sxs-lookup"><span data-stu-id="7eada-433">In Razor (*.cshtml*) files, the tilde-slash (`~/`) points to the web root.</span></span> <span data-ttu-id="7eada-434">開頭為的路徑 `~/` 稱為*虛擬路徑*。</span><span class="sxs-lookup"><span data-stu-id="7eada-434">A path beginning with `~/` is referred to as a *virtual path*.</span></span>

<span data-ttu-id="7eada-435">如需詳細資訊，請參閱<xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="7eada-435">For more information, see <xref:fundamentals/static-files>.</span></span>

::: moniker-end
