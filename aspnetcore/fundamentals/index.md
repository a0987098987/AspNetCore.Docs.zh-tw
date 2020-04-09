---
title: ASP.NET Core 基本概念
author: rick-anderson
description: 了解建置 ASP.NET Core 應用程式的基本概念。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/30/2020
uid: fundamentals/index
ms.openlocfilehash: da2b42a7cf5d116a36d1dd9fa586d40ab31fc52d
ms.sourcegitcommit: 72792e349458190b4158fcbacb87caf3fc605268
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80417647"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="32d4f-103">ASP.NET Core 基本概念</span><span class="sxs-lookup"><span data-stu-id="32d4f-103">ASP.NET Core fundamentals</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="32d4f-104">本文概述了如何開發ASP.NET核心應用的關鍵主題。</span><span class="sxs-lookup"><span data-stu-id="32d4f-104">This article provides an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="32d4f-105">Startup 類別</span><span class="sxs-lookup"><span data-stu-id="32d4f-105">The Startup class</span></span>

<span data-ttu-id="32d4f-106">`Startup` 類別是：</span><span class="sxs-lookup"><span data-stu-id="32d4f-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="32d4f-107">已設定應用程式所需的服務。</span><span class="sxs-lookup"><span data-stu-id="32d4f-107">Services required by the app are configured.</span></span>
* <span data-ttu-id="32d4f-108">應用的請求處理管道定義為一系列中間件元件。</span><span class="sxs-lookup"><span data-stu-id="32d4f-108">The app's request handling pipeline is defined, as a series of middleware components.</span></span>

<span data-ttu-id="32d4f-109">以下是 `Startup` 類別範例：</span><span class="sxs-lookup"><span data-stu-id="32d4f-109">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/samples_snapshot/3.x/Startup.cs?highlight=3,12)]

<span data-ttu-id="32d4f-110">如需詳細資訊，請參閱 <xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-110">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="32d4f-111">相依性插入 (服務)</span><span class="sxs-lookup"><span data-stu-id="32d4f-111">Dependency injection (services)</span></span>

<span data-ttu-id="32d4f-112">ASP.NET核心包括一個內置的依賴項注入 (DI) 框架,該框架使配置的服務在整個應用中可用。</span><span class="sxs-lookup"><span data-stu-id="32d4f-112">ASP.NET Core includes a built-in dependency injection (DI) framework that makes configured services available throughout an app.</span></span> <span data-ttu-id="32d4f-113">例如，記錄元件即為一項服務。</span><span class="sxs-lookup"><span data-stu-id="32d4f-113">For example, a logging component is a service.</span></span>

<span data-ttu-id="32d4f-114">設定 (或「註冊」\*\*) 服務的程式碼會新增至 `Startup.ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="32d4f-114">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="32d4f-115">例如：</span><span class="sxs-lookup"><span data-stu-id="32d4f-115">For example:</span></span>

[!code-csharp[](index/samples_snapshot/3.x/ConfigureServices.cs)]

<span data-ttu-id="32d4f-116">服務通常使用建構函數注入從 DI 解析。</span><span class="sxs-lookup"><span data-stu-id="32d4f-116">Services are typically resolved from DI using constructor injection.</span></span> <span data-ttu-id="32d4f-117">使用建構函數注入時,類聲明所需類型的構造函數參數或介面。</span><span class="sxs-lookup"><span data-stu-id="32d4f-117">With constructor injection, a class declares a constructor parameter of either the required type or an interface.</span></span> <span data-ttu-id="32d4f-118">DI 框架在運行時提供此服務的實例。</span><span class="sxs-lookup"><span data-stu-id="32d4f-118">The DI framework provides an instance of this service at runtime.</span></span>

<span data-ttu-id="32d4f-119">下面的範例使用建構函數注入來解決`RazorPagesMovieContext`DI 中的 a:</span><span class="sxs-lookup"><span data-stu-id="32d4f-119">The following example uses constructor injection to resolve a `RazorPagesMovieContext` from DI:</span></span>

[!code-csharp[](index/samples_snapshot/3.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="32d4f-120">如果內置的 Control 反轉 (IoC) 容器無法滿足應用的所有需求,則可以改用第三方 IoC 容器。</span><span class="sxs-lookup"><span data-stu-id="32d4f-120">If the built-in Inversion of Control (IoC) container doesn't meet all of an app's needs, a third-party IoC container can be used instead.</span></span>

<span data-ttu-id="32d4f-121">如需詳細資訊，請參閱 <xref:fundamentals/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-121">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="32d4f-122">中介軟體</span><span class="sxs-lookup"><span data-stu-id="32d4f-122">Middleware</span></span>

<span data-ttu-id="32d4f-123">要求處理管線是以一系列中介軟體元件組成。</span><span class="sxs-lookup"><span data-stu-id="32d4f-123">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="32d4f-124">每個元件對 執行操作`HttpContext`,並調用管道中的下一個中間件或終止請求。</span><span class="sxs-lookup"><span data-stu-id="32d4f-124">Each component performs operations on an `HttpContext` and either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="32d4f-125">按照慣例,通過調用`Use...``Startup.Configure`方法中的擴展方法,將中間件元件添加到管道中。</span><span class="sxs-lookup"><span data-stu-id="32d4f-125">By convention, a middleware component is added to the pipeline by invoking a `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="32d4f-126">例如，若要啟用靜態檔案轉譯，請呼叫 `UseStaticFiles`。</span><span class="sxs-lookup"><span data-stu-id="32d4f-126">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="32d4f-127">以下範例設定要求處理導管:</span><span class="sxs-lookup"><span data-stu-id="32d4f-127">The following example configures a request handling pipeline:</span></span>

[!code-csharp[](index/samples_snapshot/3.x/Configure.cs)]

<span data-ttu-id="32d4f-128">ASP.NET核心包括一組豐富的內置中間件。</span><span class="sxs-lookup"><span data-stu-id="32d4f-128">ASP.NET Core includes a rich set of built-in middleware.</span></span> <span data-ttu-id="32d4f-129">也可以編寫自定義中間件元件。</span><span class="sxs-lookup"><span data-stu-id="32d4f-129">Custom middleware components can also be written.</span></span>

<span data-ttu-id="32d4f-130">如需詳細資訊，請參閱 <xref:fundamentals/middleware/index>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-130">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="32d4f-131">Host</span><span class="sxs-lookup"><span data-stu-id="32d4f-131">Host</span></span>

<span data-ttu-id="32d4f-132">啟動時,ASP.NET核心應用將產生*主機*。</span><span class="sxs-lookup"><span data-stu-id="32d4f-132">On startup, an ASP.NET Core app builds a *host*.</span></span> <span data-ttu-id="32d4f-133">主機封裝應用的所有資源,例如:</span><span class="sxs-lookup"><span data-stu-id="32d4f-133">The host encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="32d4f-134">HTTP 伺服器實作</span><span class="sxs-lookup"><span data-stu-id="32d4f-134">An HTTP server implementation</span></span>
* <span data-ttu-id="32d4f-135">中介軟體元件</span><span class="sxs-lookup"><span data-stu-id="32d4f-135">Middleware components</span></span>
* <span data-ttu-id="32d4f-136">記錄</span><span class="sxs-lookup"><span data-stu-id="32d4f-136">Logging</span></span>
* <span data-ttu-id="32d4f-137">相依項(DI) 服務</span><span class="sxs-lookup"><span data-stu-id="32d4f-137">Dependency injection (DI) services</span></span>
* <span data-ttu-id="32d4f-138">組態</span><span class="sxs-lookup"><span data-stu-id="32d4f-138">Configuration</span></span>

<span data-ttu-id="32d4f-139">有兩個不同的主機:</span><span class="sxs-lookup"><span data-stu-id="32d4f-139">There are two different hosts:</span></span> 

* <span data-ttu-id="32d4f-140">.NET 泛型主機</span><span class="sxs-lookup"><span data-stu-id="32d4f-140">.NET Generic Host</span></span>
* <span data-ttu-id="32d4f-141">ASP.NET Core Web 主機</span><span class="sxs-lookup"><span data-stu-id="32d4f-141">ASP.NET Core Web Host</span></span>

<span data-ttu-id="32d4f-142">建議使用 .NET 通用主機。</span><span class="sxs-lookup"><span data-stu-id="32d4f-142">The .NET Generic Host is recommended.</span></span> <span data-ttu-id="32d4f-143">ASP.NET核心 Web 主機僅適用於向後相容性。</span><span class="sxs-lookup"><span data-stu-id="32d4f-143">The ASP.NET Core Web Host is available only for backwards compatibility.</span></span>

<span data-ttu-id="32d4f-144">下面的範例建立一個 .NET 通用主機:</span><span class="sxs-lookup"><span data-stu-id="32d4f-144">The following example creates a .NET Generic Host:</span></span>

[!code-csharp[](index/samples_snapshot/3.x/Program.cs)]

<span data-ttu-id="32d4f-145">`CreateDefaultBuilder`與`ConfigureWebHostDefaults`方法使用一組預設選項設定主機,例如:</span><span class="sxs-lookup"><span data-stu-id="32d4f-145">The `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods configure a host with a set of default options, such as:</span></span>

* <span data-ttu-id="32d4f-146">使用 [Kestrel](#servers) 作為網頁伺服器，並啟用 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="32d4f-146">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="32d4f-147">從 *appsettings.json*、*appsettings.{Environment Name}.json*、環境變數與命令列引數載入設定。</span><span class="sxs-lookup"><span data-stu-id="32d4f-147">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="32d4f-148">將記錄輸出傳送到主控台及偵錯提供者。</span><span class="sxs-lookup"><span data-stu-id="32d4f-148">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="32d4f-149">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-149">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="non-web-scenarios"></a><span data-ttu-id="32d4f-150">非 Web 案例</span><span class="sxs-lookup"><span data-stu-id="32d4f-150">Non-web scenarios</span></span>

<span data-ttu-id="32d4f-151">一般主機允許其他類型的應用程式，使用交叉剪輯架構延伸模組，例如記錄、相依性插入 (DI)、設定與應用程式存留期管理。</span><span class="sxs-lookup"><span data-stu-id="32d4f-151">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="32d4f-152">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host> 和 <xref:fundamentals/host/hosted-services>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-152">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="32d4f-153">伺服器</span><span class="sxs-lookup"><span data-stu-id="32d4f-153">Servers</span></span>

<span data-ttu-id="32d4f-154">ASP.NET Core 應用程式使用 HTTP 伺服器實作來接聽 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="32d4f-154">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="32d4f-155">伺服器會把向應用程式發出的要求作為一組[要求功能](xref:fundamentals/request-features)，合併成一個 `HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="32d4f-155">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

# <a name="windows"></a>[<span data-ttu-id="32d4f-156">Windows</span><span class="sxs-lookup"><span data-stu-id="32d4f-156">Windows</span></span>](#tab/windows)

<span data-ttu-id="32d4f-157">ASP.NET Core 隨附下列伺服器實作：</span><span class="sxs-lookup"><span data-stu-id="32d4f-157">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="32d4f-158">*Kestrel* 是跨平台的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="32d4f-158">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="32d4f-159">Kestrel 通常會使用 [IIS](https://www.iis.net/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="32d4f-159">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="32d4f-160">在 ASP.NET Core 2.0 或更新版本中，Kestrel 可以作為直接向網際網路公開的公眾 Edge Server 執行。</span><span class="sxs-lookup"><span data-stu-id="32d4f-160">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="32d4f-161">*IIS HTTP 伺服器*是使用 IIS 的 Windows 伺服器。</span><span class="sxs-lookup"><span data-stu-id="32d4f-161">*IIS HTTP Server* is a server for Windows that uses IIS.</span></span> <span data-ttu-id="32d4f-162">透過此伺服器，ASP.NET Core 應用程式及 IIS 便可以在相同處理序中執行。</span><span class="sxs-lookup"><span data-stu-id="32d4f-162">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="32d4f-163">*HTTP.sys* 則是適用於並未搭配 IIS 使用 Windows 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="32d4f-163">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="32d4f-164">macOS</span><span class="sxs-lookup"><span data-stu-id="32d4f-164">macOS</span></span>](#tab/macos)

<span data-ttu-id="32d4f-165">ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="32d4f-165">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="32d4f-166">在ASP.NET Core 2.0 或更高版本中,Kestrel 可以作為直接暴露到 Internet 的面向公共的邊緣伺服器運行。</span><span class="sxs-lookup"><span data-stu-id="32d4f-166">In ASP.NET Core 2.0 or later, Kestrel can run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="32d4f-167">Kestrel 通常會使用 [Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="32d4f-167">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="32d4f-168">Linux</span><span class="sxs-lookup"><span data-stu-id="32d4f-168">Linux</span></span>](#tab/linux)

<span data-ttu-id="32d4f-169">ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="32d4f-169">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="32d4f-170">在ASP.NET Core 2.0 或更高版本中,Kestrel 可以作為直接暴露到 Internet 的面向公共的邊緣伺服器運行。</span><span class="sxs-lookup"><span data-stu-id="32d4f-170">In ASP.NET Core 2.0 or later, Kestrel can run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="32d4f-171">Kestrel 通常會使用 [Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="32d4f-171">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

<span data-ttu-id="32d4f-172">如需詳細資訊，請參閱 <xref:fundamentals/servers/index>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-172">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="32d4f-173">組態</span><span class="sxs-lookup"><span data-stu-id="32d4f-173">Configuration</span></span>

<span data-ttu-id="32d4f-174">ASP.NET Core 提供組態架構，可從組態提供者的已排序集合中，以成對名稱和數值的形式取得設定。</span><span class="sxs-lookup"><span data-stu-id="32d4f-174">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="32d4f-175">內建配置提供者可用於各種源,如 *.json*檔 *、.xml*檔、環境變數和命令列參數。</span><span class="sxs-lookup"><span data-stu-id="32d4f-175">Built-in configuration providers are available for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="32d4f-176">編寫自定義配置提供程式以支援其他源。</span><span class="sxs-lookup"><span data-stu-id="32d4f-176">Write custom configuration providers to support other sources.</span></span>

<span data-ttu-id="32d4f-177">[默認情況下](xref:fundamentals/configuration/index#default),ASP.NET核心應用配置為從*appsettings.json、* 環境變數、命令列等讀取。</span><span class="sxs-lookup"><span data-stu-id="32d4f-177">By [default](xref:fundamentals/configuration/index#default), ASP.NET Core apps are configured to read from *appsettings.json*, environment variables, the command line, and more.</span></span> <span data-ttu-id="32d4f-178">載入應用的配置時,環境變數的值將覆蓋*appsettings.json*中的值。</span><span class="sxs-lookup"><span data-stu-id="32d4f-178">When the app's configuration is loaded, values from environment variables override values from *appsettings.json*.</span></span>

<span data-ttu-id="32d4f-179">讀取相關設定值的偏好方法是使用[選項模式](xref:fundamentals/configuration/options)。</span><span class="sxs-lookup"><span data-stu-id="32d4f-179">The preferred way to read related configuration values is using the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="32d4f-180">關於詳細資訊,請參考[使用選項模式繫結分層設定資料](xref:fundamentals/configuration/index#optpat)。</span><span class="sxs-lookup"><span data-stu-id="32d4f-180">For more information, see [Bind hierarchical configuration data using the options pattern](xref:fundamentals/configuration/index#optpat).</span></span>

<span data-ttu-id="32d4f-181">對管理機密設定資料(如密碼),ASP.NET核心提供[機密管理員](xref:security/app-secrets#secret-manager)。</span><span class="sxs-lookup"><span data-stu-id="32d4f-181">For managing confidential configuration data such as passwords, ASP.NET Core provides the [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="32d4f-182">針對生產祕密，我們建議使用 [Azure Key Vault](xref:security/key-vault-configuration)。</span><span class="sxs-lookup"><span data-stu-id="32d4f-182">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="32d4f-183">如需詳細資訊，請參閱 <xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-183">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="environments"></a><span data-ttu-id="32d4f-184">環境</span><span class="sxs-lookup"><span data-stu-id="32d4f-184">Environments</span></span>

<span data-ttu-id="32d4f-185">執行環境(如`Development`、`Staging``Production`和) 是 ASP.NET Core 中的一流概念。</span><span class="sxs-lookup"><span data-stu-id="32d4f-185">Execution environments, such as `Development`, `Staging`, and `Production`, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="32d4f-186">通過設置`ASPNETCORE_ENVIRONMENT`環境變數指定應用正在運行的環境。</span><span class="sxs-lookup"><span data-stu-id="32d4f-186">Specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="32d4f-187">ASP.NET Core 會在應用程式啟動時讀取環境變數，然後將值儲存在 `IWebHostEnvironment` 實作中。</span><span class="sxs-lookup"><span data-stu-id="32d4f-187">ASP.NET Core reads that environment variable at app startup and stores the value in an `IWebHostEnvironment` implementation.</span></span> <span data-ttu-id="32d4f-188">此實現可通過依賴項注入 (DI) 在應用中的任意位置可用。</span><span class="sxs-lookup"><span data-stu-id="32d4f-188">This implementation is available anywhere in an app via dependency injection (DI).</span></span>

<span data-ttu-id="32d4f-189">以下範例將應用設定為在`Development`環境中執行時提供詳細的錯誤資訊:</span><span class="sxs-lookup"><span data-stu-id="32d4f-189">The following example configures the app to provide detailed error information when running in the `Development` environment:</span></span>

[!code-csharp[](index/samples_snapshot/3.x/StartupConfigure.cs?highlight=3-6)]

<span data-ttu-id="32d4f-190">如需詳細資訊，請參閱 <xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-190">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="32d4f-191">記錄</span><span class="sxs-lookup"><span data-stu-id="32d4f-191">Logging</span></span>

<span data-ttu-id="32d4f-192">ASP.NET Core 支援適用於各種內建和協力廠商記錄提供者的記錄 API。</span><span class="sxs-lookup"><span data-stu-id="32d4f-192">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="32d4f-193">可用的供應商包括:</span><span class="sxs-lookup"><span data-stu-id="32d4f-193">Available providers include:</span></span>

* <span data-ttu-id="32d4f-194">主控台</span><span class="sxs-lookup"><span data-stu-id="32d4f-194">Console</span></span>
* <span data-ttu-id="32d4f-195">偵錯</span><span class="sxs-lookup"><span data-stu-id="32d4f-195">Debug</span></span>
* <span data-ttu-id="32d4f-196">Windows 上的事件追蹤</span><span class="sxs-lookup"><span data-stu-id="32d4f-196">Event Tracing on Windows</span></span>
* <span data-ttu-id="32d4f-197">Windows 事件記錄檔</span><span class="sxs-lookup"><span data-stu-id="32d4f-197">Windows Event Log</span></span>
* <span data-ttu-id="32d4f-198">TraceSource</span><span class="sxs-lookup"><span data-stu-id="32d4f-198">TraceSource</span></span>
* <span data-ttu-id="32d4f-199">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="32d4f-199">Azure App Service</span></span>
* <span data-ttu-id="32d4f-200">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="32d4f-200">Azure Application Insights</span></span>

<span data-ttu-id="32d4f-201">要建立紀錄,請從<xref:Microsoft.Extensions.Logging.ILogger%601>相依相依支援項 (DI) 解析服務<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>,並呼叫紀錄紀錄方法(如 。</span><span class="sxs-lookup"><span data-stu-id="32d4f-201">To create logs, resolve an <xref:Microsoft.Extensions.Logging.ILogger%601> service from dependency injection (DI) and call logging methods such as <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>.</span></span> <span data-ttu-id="32d4f-202">例如：</span><span class="sxs-lookup"><span data-stu-id="32d4f-202">For example:</span></span>

[!code-csharp[](index/samples_snapshot/3.x/TodoController.cs?highlight=5,13,19)]

<span data-ttu-id="32d4f-203">日誌記錄方法,如`LogInformation`支援任意數量的欄位。</span><span class="sxs-lookup"><span data-stu-id="32d4f-203">Logging methods such as `LogInformation` support any number of fields.</span></span> <span data-ttu-id="32d4f-204">這些欄位通常用於建構消息`string`,但某些日誌記錄提供程式將這些欄位作為單獨的欄位發送到資料存儲。</span><span class="sxs-lookup"><span data-stu-id="32d4f-204">These fields are commonly used to construct a message `string`, but some logging providers send these to a data store as separate fields.</span></span> <span data-ttu-id="32d4f-205">這項功能可讓記錄提供者實作 [semantic logging (語意記錄)，又稱為 structured logging (結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。</span><span class="sxs-lookup"><span data-stu-id="32d4f-205">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="32d4f-206">如需詳細資訊，請參閱 <xref:fundamentals/logging/index>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-206">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="32d4f-207">路由</span><span class="sxs-lookup"><span data-stu-id="32d4f-207">Routing</span></span>

<span data-ttu-id="32d4f-208">「路由」\*\* 是一種對應到處理常式的 URL 模式。</span><span class="sxs-lookup"><span data-stu-id="32d4f-208">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="32d4f-209">處理常式通常是 Razor 頁面、MVC 控制器中的動作方法，或是中介軟體。</span><span class="sxs-lookup"><span data-stu-id="32d4f-209">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="32d4f-210">ASP.NET Core 路由可讓您控制您應用程式使用的 URL。</span><span class="sxs-lookup"><span data-stu-id="32d4f-210">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="32d4f-211">如需詳細資訊，請參閱 <xref:fundamentals/routing>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-211">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="32d4f-212">錯誤處理</span><span class="sxs-lookup"><span data-stu-id="32d4f-212">Error handling</span></span>

<span data-ttu-id="32d4f-213">ASP.NET Core 具有處理錯誤的內建功能，例如：</span><span class="sxs-lookup"><span data-stu-id="32d4f-213">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="32d4f-214">開發人員例外狀況頁面</span><span class="sxs-lookup"><span data-stu-id="32d4f-214">A developer exception page</span></span>
* <span data-ttu-id="32d4f-215">自訂錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="32d4f-215">Custom error pages</span></span>
* <span data-ttu-id="32d4f-216">靜態狀態碼頁面</span><span class="sxs-lookup"><span data-stu-id="32d4f-216">Static status code pages</span></span>
* <span data-ttu-id="32d4f-217">啟動例外狀況處理</span><span class="sxs-lookup"><span data-stu-id="32d4f-217">Startup exception handling</span></span>

<span data-ttu-id="32d4f-218">如需詳細資訊，請參閱 <xref:fundamentals/error-handling>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-218">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="32d4f-219">發出 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="32d4f-219">Make HTTP requests</span></span>

<span data-ttu-id="32d4f-220">`IHttpClientFactory` 的實作可用於建立 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="32d4f-220">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="32d4f-221">Factory：</span><span class="sxs-lookup"><span data-stu-id="32d4f-221">The factory:</span></span>

* <span data-ttu-id="32d4f-222">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="32d4f-222">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="32d4f-223">例如,註冊和配置*github*用戶端以訪問 GitHub。</span><span class="sxs-lookup"><span data-stu-id="32d4f-223">For example, register and configure a *github* client for accessing GitHub.</span></span> <span data-ttu-id="32d4f-224">註冊和配置默認用戶端以用於其他目的。</span><span class="sxs-lookup"><span data-stu-id="32d4f-224">Register and configure a default client for other purposes.</span></span>
* <span data-ttu-id="32d4f-225">支援註冊及多個委派處理常式的鏈結，以用於建置傳出要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="32d4f-225">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="32d4f-226">此模式類似於ASP.NET Core 的入站中間件管道。</span><span class="sxs-lookup"><span data-stu-id="32d4f-226">This pattern is similar to ASP.NET Core's inbound middleware pipeline.</span></span> <span data-ttu-id="32d4f-227">該模式提供了一種機制來管理 HTTP 請求的跨領域問題,包括緩存、錯誤處理、序列化和日誌記錄。</span><span class="sxs-lookup"><span data-stu-id="32d4f-227">The pattern provides a mechanism to manage cross-cutting concerns for HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="32d4f-228">與 *Polly* 整合，Polly 是一種熱門的協力廠商程式庫，用於進行暫時性的錯誤處理。</span><span class="sxs-lookup"><span data-stu-id="32d4f-228">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="32d4f-229">管理基礎`HttpClientHandler`實例的池和存留期,以避免手`HttpClient`動管理 存留期時出現常見的 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="32d4f-229">Manages the pooling and lifetime of underlying `HttpClientHandler` instances to avoid common DNS problems that occur when managing `HttpClient` lifetimes manually.</span></span>
* <span data-ttu-id="32d4f-230">通過<xref:Microsoft.Extensions.Logging.ILogger>工廠創建的所有用戶端發送的所有請求添加可配置的日誌記錄體驗。</span><span class="sxs-lookup"><span data-stu-id="32d4f-230">Adds a configurable logging experience via <xref:Microsoft.Extensions.Logging.ILogger> for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="32d4f-231">如需詳細資訊，請參閱 <xref:fundamentals/http-requests>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-231">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="32d4f-232">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="32d4f-232">Content root</span></span>

<span data-ttu-id="32d4f-233">內容根是以下基本路徑:</span><span class="sxs-lookup"><span data-stu-id="32d4f-233">The content root is the base path for:</span></span>

* <span data-ttu-id="32d4f-234">託管應用程式的可執行檔 *(.exe*)。</span><span class="sxs-lookup"><span data-stu-id="32d4f-234">The executable hosting the app (*.exe*).</span></span>
* <span data-ttu-id="32d4f-235">組成應用程式的編譯程式集(*.dll*)。</span><span class="sxs-lookup"><span data-stu-id="32d4f-235">Compiled assemblies that make up the app (*.dll*).</span></span>
* <span data-ttu-id="32d4f-236">套用內容檔案,例如:</span><span class="sxs-lookup"><span data-stu-id="32d4f-236">Content files used by the app, such as:</span></span>
  * <span data-ttu-id="32d4f-237">剃刀檔 *(.cshtml*, *.razor*)</span><span class="sxs-lookup"><span data-stu-id="32d4f-237">Razor files (*.cshtml*, *.razor*)</span></span>
  * <span data-ttu-id="32d4f-238">設定檔 *(.json*, *.xml*)</span><span class="sxs-lookup"><span data-stu-id="32d4f-238">Configuration files (*.json*, *.xml*)</span></span>
  * <span data-ttu-id="32d4f-239">資料檔案(*.db*)</span><span class="sxs-lookup"><span data-stu-id="32d4f-239">Data files (*.db*)</span></span>
* <span data-ttu-id="32d4f-240">[Web 根](#web-root),通常是*wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="32d4f-240">The [Web root](#web-root), typically the *wwwroot* folder.</span></span>

<span data-ttu-id="32d4f-241">在開發過程中,內容根預設為專案的根目錄。</span><span class="sxs-lookup"><span data-stu-id="32d4f-241">During development, the content root defaults to the project's root directory.</span></span> <span data-ttu-id="32d4f-242">此目錄也是應用的內容檔和[Web 根](#web-root)目錄的基本路徑。</span><span class="sxs-lookup"><span data-stu-id="32d4f-242">This directory is also the base path for both the app's content files and the [Web root](#web-root).</span></span> <span data-ttu-id="32d4f-243">通過在[構建主機](#host)時設置其路徑來指定其他內容根。</span><span class="sxs-lookup"><span data-stu-id="32d4f-243">Specify a different content root by setting its path when [building the host](#host).</span></span> <span data-ttu-id="32d4f-244">如需詳細資訊，請參閱[內容根](xref:fundamentals/host/generic-host#contentrootpath-1)。</span><span class="sxs-lookup"><span data-stu-id="32d4f-244">For more information, see [Content root](xref:fundamentals/host/generic-host#contentrootpath-1).</span></span>

## <a name="web-root"></a><span data-ttu-id="32d4f-245">Web 根目錄</span><span class="sxs-lookup"><span data-stu-id="32d4f-245">Web root</span></span>

<span data-ttu-id="32d4f-246">Web 根是公共靜態資源檔的基本路徑,例如:</span><span class="sxs-lookup"><span data-stu-id="32d4f-246">The web root is the base path for public, static resource files, such as:</span></span>

* <span data-ttu-id="32d4f-247">樣式表 (*.css*)</span><span class="sxs-lookup"><span data-stu-id="32d4f-247">Stylesheets (*.css*)</span></span>
* <span data-ttu-id="32d4f-248">JavaScript (*.js*)</span><span class="sxs-lookup"><span data-stu-id="32d4f-248">JavaScript (*.js*)</span></span>
* <span data-ttu-id="32d4f-249">圖片 (*.png*, *.jpg*)</span><span class="sxs-lookup"><span data-stu-id="32d4f-249">Images (*.png*, *.jpg*)</span></span>

<span data-ttu-id="32d4f-250">預設情況下,靜態檔僅從 Web 根目錄及其子目錄提供。</span><span class="sxs-lookup"><span data-stu-id="32d4f-250">By default, static files are served only from the web root directory and its sub-directories.</span></span> <span data-ttu-id="32d4f-251">Web 根路徑預設為 *[內容根]/wwwroot*。</span><span class="sxs-lookup"><span data-stu-id="32d4f-251">The web root path defaults to *{content root}/wwwroot*.</span></span> <span data-ttu-id="32d4f-252">通過在[構建主機](#host)時設置其路徑來指定其他 Web 根。</span><span class="sxs-lookup"><span data-stu-id="32d4f-252">Specify a different web root by setting its path when [building the host](#host).</span></span> <span data-ttu-id="32d4f-253">如需詳細資訊，請參閱 [Web 根目錄](xref:fundamentals/host/generic-host#webroot-1)。</span><span class="sxs-lookup"><span data-stu-id="32d4f-253">For more information, see [Web root](xref:fundamentals/host/generic-host#webroot-1).</span></span>

<span data-ttu-id="32d4f-254">防止在*wwwroot*中使用專案檔中[\<的內容>專案項](/visualstudio/msbuild/common-msbuild-project-items#content)發佈檔。</span><span class="sxs-lookup"><span data-stu-id="32d4f-254">Prevent publishing files in *wwwroot* with the [\<Content> project item](/visualstudio/msbuild/common-msbuild-project-items#content) in the project file.</span></span> <span data-ttu-id="32d4f-255">以下範例防止在*wwwroot/local*及其子目錄中發表內容:</span><span class="sxs-lookup"><span data-stu-id="32d4f-255">The following example prevents publishing content in *wwwroot/local* and its sub-directories:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="32d4f-256">在 Razor *.cshtml*檔中,`~/`波浪斜 槓 ( ) 指向 Web 根。</span><span class="sxs-lookup"><span data-stu-id="32d4f-256">In Razor *.cshtml* files, tilde-slash (`~/`) points to the web root.</span></span> <span data-ttu-id="32d4f-257">以 開`~/`頭的路徑稱為*虛擬路徑*。</span><span class="sxs-lookup"><span data-stu-id="32d4f-257">A path beginning with `~/` is referred to as a *virtual path*.</span></span>

<span data-ttu-id="32d4f-258">如需詳細資訊，請參閱 <xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-258">For more information, see <xref:fundamentals/static-files>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="32d4f-259">本文是了解如何開發 ASP.NET Core 應用程式的關鍵主題概觀。</span><span class="sxs-lookup"><span data-stu-id="32d4f-259">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="32d4f-260">Startup 類別</span><span class="sxs-lookup"><span data-stu-id="32d4f-260">The Startup class</span></span>

<span data-ttu-id="32d4f-261">`Startup` 類別是：</span><span class="sxs-lookup"><span data-stu-id="32d4f-261">The `Startup` class is where:</span></span>

* <span data-ttu-id="32d4f-262">已設定應用程式所需的服務。</span><span class="sxs-lookup"><span data-stu-id="32d4f-262">Services required by the app are configured.</span></span>
* <span data-ttu-id="32d4f-263">定義要求處理管線的位置。</span><span class="sxs-lookup"><span data-stu-id="32d4f-263">The request handling pipeline is defined.</span></span>

<span data-ttu-id="32d4f-264">「服務」\*\* 是應用程式所使用的元件。</span><span class="sxs-lookup"><span data-stu-id="32d4f-264">*Services* are components that are used by the app.</span></span> <span data-ttu-id="32d4f-265">例如，記錄元件即為一項服務。</span><span class="sxs-lookup"><span data-stu-id="32d4f-265">For example, a logging component is a service.</span></span> <span data-ttu-id="32d4f-266">設定 (或「註冊」\*\*) 服務的程式碼會新增至 `Startup.ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="32d4f-266">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="32d4f-267">請求處理管道由一系列*中間件*元件組成。</span><span class="sxs-lookup"><span data-stu-id="32d4f-267">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="32d4f-268">例如，中介軟體可能會處理靜態檔案的要求，或是將 HTTP 要求重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="32d4f-268">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="32d4f-269">每個中介軟體會在 `HttpContext` 上執行非同步操作，然後叫用管線中下一個中介軟體或終止要求。</span><span class="sxs-lookup"><span data-stu-id="32d4f-269">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="32d4f-270">設定要求處理管線的程式碼會新增至 `Startup.Configure` 方法。</span><span class="sxs-lookup"><span data-stu-id="32d4f-270">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="32d4f-271">以下是 `Startup` 類別範例：</span><span class="sxs-lookup"><span data-stu-id="32d4f-271">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/samples_snapshot/2.x/Startup.cs?highlight=3,12)]

<span data-ttu-id="32d4f-272">如需詳細資訊，請參閱 <xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-272">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="32d4f-273">相依性插入 (服務)</span><span class="sxs-lookup"><span data-stu-id="32d4f-273">Dependency injection (services)</span></span>

<span data-ttu-id="32d4f-274">ASP.NET Core 具有內建的相依性插入 (DI) 架構，可讓應用程式的類別使用所設定服務。</span><span class="sxs-lookup"><span data-stu-id="32d4f-274">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="32d4f-275">取得類別中一項服務執行個體的其中一種方式，便是使用所需類型的參數來建立建構函式。</span><span class="sxs-lookup"><span data-stu-id="32d4f-275">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="32d4f-276">參數可以是服務類型或是介面。</span><span class="sxs-lookup"><span data-stu-id="32d4f-276">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="32d4f-277">DI 系統會在執行階段提供服務。</span><span class="sxs-lookup"><span data-stu-id="32d4f-277">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="32d4f-278">以下是使用 DI 取得 Entity Framework Core 內容物件的類別。</span><span class="sxs-lookup"><span data-stu-id="32d4f-278">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="32d4f-279">醒目提示的那一行便是建構函式插入的範例：</span><span class="sxs-lookup"><span data-stu-id="32d4f-279">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/samples_snapshot/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="32d4f-280">雖然 DI 為內建，但其設計用於讓您插入協力廠商的控制反轉 (IoC) 容器 (若您想要的話)。</span><span class="sxs-lookup"><span data-stu-id="32d4f-280">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="32d4f-281">如需詳細資訊，請參閱 <xref:fundamentals/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-281">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="32d4f-282">中介軟體</span><span class="sxs-lookup"><span data-stu-id="32d4f-282">Middleware</span></span>

<span data-ttu-id="32d4f-283">要求處理管線是以一系列中介軟體元件組成。</span><span class="sxs-lookup"><span data-stu-id="32d4f-283">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="32d4f-284">每個元件會在 `HttpContext` 上執行非同步操作，然後叫用管線中下一個中介軟體或終止要求。</span><span class="sxs-lookup"><span data-stu-id="32d4f-284">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="32d4f-285">依照慣例，中介軟體元件會透過叫用其 `Startup.Configure` 方法中的 `Use...` 延伸模組方法來新增至管線。</span><span class="sxs-lookup"><span data-stu-id="32d4f-285">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="32d4f-286">例如，若要啟用靜態檔案轉譯，請呼叫 `UseStaticFiles`。</span><span class="sxs-lookup"><span data-stu-id="32d4f-286">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="32d4f-287">下列範例中醒目提示的程式碼會設定要求處理管線：</span><span class="sxs-lookup"><span data-stu-id="32d4f-287">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/samples_snapshot/2.x/Startup.cs?highlight=14-16)]

<span data-ttu-id="32d4f-288">ASP.NET Core 包含一組豐富的內建中介軟體，您也可以撰寫自訂中介軟體。</span><span class="sxs-lookup"><span data-stu-id="32d4f-288">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="32d4f-289">如需詳細資訊，請參閱 <xref:fundamentals/middleware/index>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-289">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="32d4f-290">Host</span><span class="sxs-lookup"><span data-stu-id="32d4f-290">Host</span></span>

<span data-ttu-id="32d4f-291">ASP.NET Core 應用程式會在啟動時建置一個「主機」\*\*。</span><span class="sxs-lookup"><span data-stu-id="32d4f-291">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="32d4f-292">主機是封裝所有應用程式資源的物件，例如：</span><span class="sxs-lookup"><span data-stu-id="32d4f-292">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="32d4f-293">HTTP 伺服器實作</span><span class="sxs-lookup"><span data-stu-id="32d4f-293">An HTTP server implementation</span></span>
* <span data-ttu-id="32d4f-294">中介軟體元件</span><span class="sxs-lookup"><span data-stu-id="32d4f-294">Middleware components</span></span>
* <span data-ttu-id="32d4f-295">記錄</span><span class="sxs-lookup"><span data-stu-id="32d4f-295">Logging</span></span>
* <span data-ttu-id="32d4f-296">DI</span><span class="sxs-lookup"><span data-stu-id="32d4f-296">DI</span></span>
* <span data-ttu-id="32d4f-297">組態</span><span class="sxs-lookup"><span data-stu-id="32d4f-297">Configuration</span></span>

<span data-ttu-id="32d4f-298">在單一物件中包含所有應用程式相互依存資源的主要理由便是生命週期管理：控制應用程式的啟動及順利關機。</span><span class="sxs-lookup"><span data-stu-id="32d4f-298">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="32d4f-299">可以使用兩種主機：Web 主機與一般主機。</span><span class="sxs-lookup"><span data-stu-id="32d4f-299">Two hosts are available: the Web Host and the Generic Host.</span></span> <span data-ttu-id="32d4f-300">在 ASP.NET Core 2.x 中，一般主機僅適用於非 Web 案例。</span><span class="sxs-lookup"><span data-stu-id="32d4f-300">In ASP.NET Core 2.x, the Generic Host is only for non-web scenarios.</span></span>

<span data-ttu-id="32d4f-301">建立主機的程式碼位於 `Program.Main` 中：</span><span class="sxs-lookup"><span data-stu-id="32d4f-301">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/samples_snapshot/2.x/Program.cs)]

<span data-ttu-id="32d4f-302">`CreateDefaultBuilder` 方法會設定具備一般常用選項的主機，如下所示：</span><span class="sxs-lookup"><span data-stu-id="32d4f-302">The `CreateDefaultBuilder` method configures a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="32d4f-303">使用 [Kestrel](#servers) 作為網頁伺服器，並啟用 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="32d4f-303">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="32d4f-304">從 *appsettings.json*、*appsettings.{Environment Name}.json*、環境變數與命令列引數載入設定。</span><span class="sxs-lookup"><span data-stu-id="32d4f-304">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="32d4f-305">將記錄輸出傳送到主控台及偵錯提供者。</span><span class="sxs-lookup"><span data-stu-id="32d4f-305">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="32d4f-306">如需詳細資訊，請參閱 <xref:fundamentals/host/web-host>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-306">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="non-web-scenarios"></a><span data-ttu-id="32d4f-307">非 Web 案例</span><span class="sxs-lookup"><span data-stu-id="32d4f-307">Non-web scenarios</span></span>

<span data-ttu-id="32d4f-308">一般主機允許其他類型的應用程式，使用交叉剪輯架構延伸模組，例如記錄、相依性插入 (DI)、設定與應用程式存留期管理。</span><span class="sxs-lookup"><span data-stu-id="32d4f-308">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="32d4f-309">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host> 和 <xref:fundamentals/host/hosted-services>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-309">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="32d4f-310">伺服器</span><span class="sxs-lookup"><span data-stu-id="32d4f-310">Servers</span></span>

<span data-ttu-id="32d4f-311">ASP.NET Core 應用程式使用 HTTP 伺服器實作來接聽 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="32d4f-311">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="32d4f-312">伺服器會把向應用程式發出的要求作為一組[要求功能](xref:fundamentals/request-features)，合併成一個 `HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="32d4f-312">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

# <a name="windows"></a>[<span data-ttu-id="32d4f-313">Windows</span><span class="sxs-lookup"><span data-stu-id="32d4f-313">Windows</span></span>](#tab/windows)

<span data-ttu-id="32d4f-314">ASP.NET Core 隨附下列伺服器實作：</span><span class="sxs-lookup"><span data-stu-id="32d4f-314">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="32d4f-315">*Kestrel* 是跨平台的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="32d4f-315">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="32d4f-316">Kestrel 通常會使用 [IIS](https://www.iis.net/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="32d4f-316">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="32d4f-317">Kestrel 可以作為直接暴露到 Internet 的面向公共的邊緣伺服器運行。</span><span class="sxs-lookup"><span data-stu-id="32d4f-317">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="32d4f-318">*IIS HTTP 伺服器*則是適用於使用 IIS Windows 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="32d4f-318">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="32d4f-319">透過此伺服器，ASP.NET Core 應用程式及 IIS 便可以在相同處理序中執行。</span><span class="sxs-lookup"><span data-stu-id="32d4f-319">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="32d4f-320">*HTTP.sys* 則是適用於並未搭配 IIS 使用 Windows 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="32d4f-320">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="32d4f-321">macOS</span><span class="sxs-lookup"><span data-stu-id="32d4f-321">macOS</span></span>](#tab/macos)

<span data-ttu-id="32d4f-322">ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="32d4f-322">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="32d4f-323">Kestrel 可以作為直接暴露到 Internet 的面向公共的邊緣伺服器運行。</span><span class="sxs-lookup"><span data-stu-id="32d4f-323">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="32d4f-324">Kestrel 通常會使用 [Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="32d4f-324">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="32d4f-325">Linux</span><span class="sxs-lookup"><span data-stu-id="32d4f-325">Linux</span></span>](#tab/linux)

<span data-ttu-id="32d4f-326">ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="32d4f-326">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="32d4f-327">Kestrel 可以作為直接暴露到 Internet 的面向公共的邊緣伺服器運行。</span><span class="sxs-lookup"><span data-stu-id="32d4f-327">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="32d4f-328">Kestrel 通常會使用 [Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="32d4f-328">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windows"></a>[<span data-ttu-id="32d4f-329">Windows</span><span class="sxs-lookup"><span data-stu-id="32d4f-329">Windows</span></span>](#tab/windows)

<span data-ttu-id="32d4f-330">ASP.NET Core 隨附下列伺服器實作：</span><span class="sxs-lookup"><span data-stu-id="32d4f-330">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="32d4f-331">*Kestrel* 是跨平台的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="32d4f-331">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="32d4f-332">Kestrel 通常會使用 [IIS](https://www.iis.net/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="32d4f-332">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="32d4f-333">Kestrel 可以作為直接暴露到 Internet 的面向公共的邊緣伺服器運行。</span><span class="sxs-lookup"><span data-stu-id="32d4f-333">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="32d4f-334">*HTTP.sys* 則是適用於並未搭配 IIS 使用 Windows 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="32d4f-334">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="32d4f-335">macOS</span><span class="sxs-lookup"><span data-stu-id="32d4f-335">macOS</span></span>](#tab/macos)

<span data-ttu-id="32d4f-336">ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="32d4f-336">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="32d4f-337">Kestrel 可以作為直接暴露到 Internet 的面向公共的邊緣伺服器運行。</span><span class="sxs-lookup"><span data-stu-id="32d4f-337">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="32d4f-338">Kestrel 通常會使用 [Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="32d4f-338">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="32d4f-339">Linux</span><span class="sxs-lookup"><span data-stu-id="32d4f-339">Linux</span></span>](#tab/linux)

<span data-ttu-id="32d4f-340">ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="32d4f-340">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="32d4f-341">Kestrel 可以作為直接暴露到 Internet 的面向公共的邊緣伺服器運行。</span><span class="sxs-lookup"><span data-stu-id="32d4f-341">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="32d4f-342">Kestrel 通常會使用 [Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="32d4f-342">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="32d4f-343">如需詳細資訊，請參閱 <xref:fundamentals/servers/index>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-343">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="32d4f-344">組態</span><span class="sxs-lookup"><span data-stu-id="32d4f-344">Configuration</span></span>

<span data-ttu-id="32d4f-345">ASP.NET Core 提供組態架構，可從組態提供者的已排序集合中，以成對名稱和數值的形式取得設定。</span><span class="sxs-lookup"><span data-stu-id="32d4f-345">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="32d4f-346">您可以使用各種來源的內建組態提供者，例如 *.json* 檔案、*.xml* 檔案、環境變數及命令列引數。</span><span class="sxs-lookup"><span data-stu-id="32d4f-346">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="32d4f-347">您也可以撰寫自訂組態提供者。</span><span class="sxs-lookup"><span data-stu-id="32d4f-347">You can also write custom configuration providers.</span></span>

<span data-ttu-id="32d4f-348">例如，您可以指定組態來自 *appsettings.json* 和環境變數。</span><span class="sxs-lookup"><span data-stu-id="32d4f-348">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="32d4f-349">然後當要求 *ConnectionString* 的值時，架構便會先在 *appsettings.json* 檔案中尋找。</span><span class="sxs-lookup"><span data-stu-id="32d4f-349">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="32d4f-350">若有找到值，但在環境變數中也存在該值，則會優先使用來自環境變數的值。</span><span class="sxs-lookup"><span data-stu-id="32d4f-350">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="32d4f-351">針對管理保密組態資料 (例如密碼)，ASP.NET Core 提供[祕密管理員工具](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="32d4f-351">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="32d4f-352">針對生產祕密，我們建議使用 [Azure Key Vault](xref:security/key-vault-configuration)。</span><span class="sxs-lookup"><span data-stu-id="32d4f-352">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="32d4f-353">如需詳細資訊，請參閱 <xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-353">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="32d4f-354">選項。</span><span class="sxs-lookup"><span data-stu-id="32d4f-354">Options</span></span>

<span data-ttu-id="32d4f-355">在可能的情況下，ASP.NET Core 會遵循「選項模式」\*\* 來儲存及擷取組態值。</span><span class="sxs-lookup"><span data-stu-id="32d4f-355">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="32d4f-356">選項模式使用類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="32d4f-356">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="32d4f-357">例如，下列程式碼會設定 WebSocket 選項：</span><span class="sxs-lookup"><span data-stu-id="32d4f-357">For example, the following code sets WebSockets options:</span></span>

[!code-csharp[](index/samples_snapshot/2.x/UseWebSockets.cs)]

<span data-ttu-id="32d4f-358">如需詳細資訊，請參閱 <xref:fundamentals/configuration/options>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-358">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="32d4f-359">環境</span><span class="sxs-lookup"><span data-stu-id="32d4f-359">Environments</span></span>

<span data-ttu-id="32d4f-360">執行環境 (例如「開發」**、「預備」** 及「生產」\*\*) 是 ASP.NET Core 中的第一級概念。</span><span class="sxs-lookup"><span data-stu-id="32d4f-360">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="32d4f-361">您可以透過設定 `ASPNETCORE_ENVIRONMENT` 環境變數，指定應用程式執行的環境。</span><span class="sxs-lookup"><span data-stu-id="32d4f-361">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="32d4f-362">ASP.NET Core 會在應用程式啟動時讀取環境變數，然後將值儲存在 `IHostingEnvironment` 實作中。</span><span class="sxs-lookup"><span data-stu-id="32d4f-362">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="32d4f-363">環境物件可在應用程式中的任何位置，透過 DI 使用。</span><span class="sxs-lookup"><span data-stu-id="32d4f-363">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="32d4f-364">下列 `Startup` 類別的範例程式碼會設定應用程式，只在於「開發」環境中執行時提供詳細的錯誤資訊：</span><span class="sxs-lookup"><span data-stu-id="32d4f-364">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/samples_snapshot/2.x/StartupConfigure.cs?highlight=3-6)]

<span data-ttu-id="32d4f-365">如需詳細資訊，請參閱 <xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-365">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="32d4f-366">記錄</span><span class="sxs-lookup"><span data-stu-id="32d4f-366">Logging</span></span>

<span data-ttu-id="32d4f-367">ASP.NET Core 支援適用於各種內建和協力廠商記錄提供者的記錄 API。</span><span class="sxs-lookup"><span data-stu-id="32d4f-367">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="32d4f-368">可用的提供者包括如下：</span><span class="sxs-lookup"><span data-stu-id="32d4f-368">Available providers include the following:</span></span>

* <span data-ttu-id="32d4f-369">主控台</span><span class="sxs-lookup"><span data-stu-id="32d4f-369">Console</span></span>
* <span data-ttu-id="32d4f-370">偵錯</span><span class="sxs-lookup"><span data-stu-id="32d4f-370">Debug</span></span>
* <span data-ttu-id="32d4f-371">Windows 上的事件追蹤</span><span class="sxs-lookup"><span data-stu-id="32d4f-371">Event Tracing on Windows</span></span>
* <span data-ttu-id="32d4f-372">Windows 事件記錄檔</span><span class="sxs-lookup"><span data-stu-id="32d4f-372">Windows Event Log</span></span>
* <span data-ttu-id="32d4f-373">TraceSource</span><span class="sxs-lookup"><span data-stu-id="32d4f-373">TraceSource</span></span>
* <span data-ttu-id="32d4f-374">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="32d4f-374">Azure App Service</span></span>
* <span data-ttu-id="32d4f-375">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="32d4f-375">Azure Application Insights</span></span>

<span data-ttu-id="32d4f-376">透過從 DI 取得 `ILogger` 物件並呼叫記錄方法，從應用程式程式碼中的任何位置寫入記錄。</span><span class="sxs-lookup"><span data-stu-id="32d4f-376">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="32d4f-377">以下是使用 `ILogger` 物件的範例程式碼，其中建構函式插入及記錄方法呼叫都已醒目提示。</span><span class="sxs-lookup"><span data-stu-id="32d4f-377">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/samples_snapshot/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="32d4f-378">`ILogger` 介面可讓您將任何數量的欄位傳遞給記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="32d4f-378">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="32d4f-379">欄位常用於建構訊息字串，但提供者也可以將它們作為個別欄位，傳送至資料存放區。</span><span class="sxs-lookup"><span data-stu-id="32d4f-379">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="32d4f-380">這項功能可讓記錄提供者實作 [semantic logging (語意記錄)，又稱為 structured logging (結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。</span><span class="sxs-lookup"><span data-stu-id="32d4f-380">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="32d4f-381">如需詳細資訊，請參閱 <xref:fundamentals/logging/index>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-381">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="32d4f-382">路由</span><span class="sxs-lookup"><span data-stu-id="32d4f-382">Routing</span></span>

<span data-ttu-id="32d4f-383">「路由」\*\* 是一種對應到處理常式的 URL 模式。</span><span class="sxs-lookup"><span data-stu-id="32d4f-383">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="32d4f-384">處理常式通常是 Razor 頁面、MVC 控制器中的動作方法，或是中介軟體。</span><span class="sxs-lookup"><span data-stu-id="32d4f-384">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="32d4f-385">ASP.NET Core 路由可讓您控制您應用程式使用的 URL。</span><span class="sxs-lookup"><span data-stu-id="32d4f-385">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="32d4f-386">如需詳細資訊，請參閱 <xref:fundamentals/routing>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-386">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="32d4f-387">錯誤處理</span><span class="sxs-lookup"><span data-stu-id="32d4f-387">Error handling</span></span>

<span data-ttu-id="32d4f-388">ASP.NET Core 具有處理錯誤的內建功能，例如：</span><span class="sxs-lookup"><span data-stu-id="32d4f-388">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="32d4f-389">開發人員例外狀況頁面</span><span class="sxs-lookup"><span data-stu-id="32d4f-389">A developer exception page</span></span>
* <span data-ttu-id="32d4f-390">自訂錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="32d4f-390">Custom error pages</span></span>
* <span data-ttu-id="32d4f-391">靜態狀態碼頁面</span><span class="sxs-lookup"><span data-stu-id="32d4f-391">Static status code pages</span></span>
* <span data-ttu-id="32d4f-392">啟動例外狀況處理</span><span class="sxs-lookup"><span data-stu-id="32d4f-392">Startup exception handling</span></span>

<span data-ttu-id="32d4f-393">如需詳細資訊，請參閱 <xref:fundamentals/error-handling>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-393">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="32d4f-394">發出 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="32d4f-394">Make HTTP requests</span></span>

<span data-ttu-id="32d4f-395">`IHttpClientFactory` 的實作可用於建立 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="32d4f-395">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="32d4f-396">Factory：</span><span class="sxs-lookup"><span data-stu-id="32d4f-396">The factory:</span></span>

* <span data-ttu-id="32d4f-397">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="32d4f-397">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="32d4f-398">例如,可以註冊和配置*github*用戶端以訪問 GitHub。</span><span class="sxs-lookup"><span data-stu-id="32d4f-398">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="32d4f-399">預設用戶端可以註冊用於其他用途。</span><span class="sxs-lookup"><span data-stu-id="32d4f-399">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="32d4f-400">支援註冊及多個委派處理常式的鏈結，以用於建置傳出要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="32d4f-400">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="32d4f-401">此模式與 ASP.NET Core 中的輸入中介軟體管線相似。</span><span class="sxs-lookup"><span data-stu-id="32d4f-401">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="32d4f-402">模式提供一個機制來管理 HTTP 要求的跨領域關注，包括快取、錯誤處理、序列化和記錄。</span><span class="sxs-lookup"><span data-stu-id="32d4f-402">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="32d4f-403">與 *Polly* 整合，Polly 是一種熱門的協力廠商程式庫，用於進行暫時性的錯誤處理。</span><span class="sxs-lookup"><span data-stu-id="32d4f-403">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="32d4f-404">管理基礎 `HttpClientHandler` 執行個體的共用和存留期，以避免在手動管理 `HttpClient` 存留期時，發生的常見 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="32d4f-404">Manages the pooling and lifetime of underlying `HttpClientHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="32d4f-405">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="32d4f-405">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="32d4f-406">如需詳細資訊，請參閱 <xref:fundamentals/http-requests>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-406">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="32d4f-407">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="32d4f-407">Content root</span></span>

<span data-ttu-id="32d4f-408">內容根是到:</span><span class="sxs-lookup"><span data-stu-id="32d4f-408">The content root is the base path to the:</span></span>

* <span data-ttu-id="32d4f-409">可執行主機應用程式 *(.exe*)。</span><span class="sxs-lookup"><span data-stu-id="32d4f-409">Executable hosting the app (*.exe*).</span></span>
* <span data-ttu-id="32d4f-410">組成應用程式的編譯程式集(*.dll*)。</span><span class="sxs-lookup"><span data-stu-id="32d4f-410">Compiled assemblies that make up the app (*.dll*).</span></span>
* <span data-ttu-id="32d4f-411">套用使用的非程式碼內容檔,例如:</span><span class="sxs-lookup"><span data-stu-id="32d4f-411">Non-code content files used by the app, such as:</span></span>
  * <span data-ttu-id="32d4f-412">剃刀檔 *(.cshtml*, *.razor*)</span><span class="sxs-lookup"><span data-stu-id="32d4f-412">Razor files (*.cshtml*, *.razor*)</span></span>
  * <span data-ttu-id="32d4f-413">設定檔 *(.json*, *.xml*)</span><span class="sxs-lookup"><span data-stu-id="32d4f-413">Configuration files (*.json*, *.xml*)</span></span>
  * <span data-ttu-id="32d4f-414">資料檔案(*.db*)</span><span class="sxs-lookup"><span data-stu-id="32d4f-414">Data files (*.db*)</span></span>
* <span data-ttu-id="32d4f-415">[Web 根](#web-root),通常是已發布*的 wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="32d4f-415">[Web root](#web-root), typically the published *wwwroot* folder.</span></span>

<span data-ttu-id="32d4f-416">在開發過程中:</span><span class="sxs-lookup"><span data-stu-id="32d4f-416">During development:</span></span>

* <span data-ttu-id="32d4f-417">內容根預設為專案的根目錄。</span><span class="sxs-lookup"><span data-stu-id="32d4f-417">The content root defaults to the project's root directory.</span></span>
* <span data-ttu-id="32d4f-418">專案的根目錄用於建立:</span><span class="sxs-lookup"><span data-stu-id="32d4f-418">The project's root directory is used to create the:</span></span>
  * <span data-ttu-id="32d4f-419">專案根目錄中應用的非代碼內容檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="32d4f-419">Path to the app's non-code content files in the project's root directory.</span></span>
  * <span data-ttu-id="32d4f-420">[Web 根](#web-root),通常是專案根目錄中的*wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="32d4f-420">[Web root](#web-root), typically the *wwwroot* folder in the project's root directory.</span></span>

<span data-ttu-id="32d4f-421">[在構建主機](#host)時,可以指定替代內容根路徑。</span><span class="sxs-lookup"><span data-stu-id="32d4f-421">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="32d4f-422">如需詳細資訊，請參閱 <xref:fundamentals/host/web-host#content-root>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-422">For more information, see <xref:fundamentals/host/web-host#content-root>.</span></span>

## <a name="web-root"></a><span data-ttu-id="32d4f-423">Web 根目錄</span><span class="sxs-lookup"><span data-stu-id="32d4f-423">Web root</span></span>

<span data-ttu-id="32d4f-424">Web 根是公共、非代碼、靜態資源檔的基本路徑,例如:</span><span class="sxs-lookup"><span data-stu-id="32d4f-424">The web root is the base path to public, non-code, static resource files, such as:</span></span>

* <span data-ttu-id="32d4f-425">樣式表 (*.css*)</span><span class="sxs-lookup"><span data-stu-id="32d4f-425">Stylesheets (*.css*)</span></span>
* <span data-ttu-id="32d4f-426">JavaScript (*.js*)</span><span class="sxs-lookup"><span data-stu-id="32d4f-426">JavaScript (*.js*)</span></span>
* <span data-ttu-id="32d4f-427">圖片 (*.png*, *.jpg*)</span><span class="sxs-lookup"><span data-stu-id="32d4f-427">Images (*.png*, *.jpg*)</span></span>

<span data-ttu-id="32d4f-428">靜態檔僅預設從 Web 根目錄(和子目錄)提供。</span><span class="sxs-lookup"><span data-stu-id="32d4f-428">Static files are only served by default from the web root directory (and sub-directories).</span></span>

<span data-ttu-id="32d4f-429">Web 根路徑預設為 *[內容根]/wwwroot,* 但在[構建主機](#host)時可以指定不同的 Web 根。</span><span class="sxs-lookup"><span data-stu-id="32d4f-429">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="32d4f-430">如需詳細資訊，請參閱 [Web 根目錄](xref:fundamentals/host/web-host#web-root)。</span><span class="sxs-lookup"><span data-stu-id="32d4f-430">For more information, see [Web root](xref:fundamentals/host/web-host#web-root).</span></span>

<span data-ttu-id="32d4f-431">防止在*wwwroot*中使用專案檔中[\<的內容>專案項](/visualstudio/msbuild/common-msbuild-project-items#content)發佈檔。</span><span class="sxs-lookup"><span data-stu-id="32d4f-431">Prevent publishing files in *wwwroot* with the [\<Content> project item](/visualstudio/msbuild/common-msbuild-project-items#content) in the project file.</span></span> <span data-ttu-id="32d4f-432">以下範例防止在*wwwroot/local*目錄和子目錄中發表內容:</span><span class="sxs-lookup"><span data-stu-id="32d4f-432">The following example prevents publishing content in the *wwwroot/local* directory and sub-directories:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="32d4f-433">在 Razor(*.cshtml*)`~/`檔中, 波浪斜線 ( ) 指向 Web 根。</span><span class="sxs-lookup"><span data-stu-id="32d4f-433">In Razor (*.cshtml*) files, the tilde-slash (`~/`) points to the web root.</span></span> <span data-ttu-id="32d4f-434">以 開`~/`頭的路徑稱為*虛擬路徑*。</span><span class="sxs-lookup"><span data-stu-id="32d4f-434">A path beginning with `~/` is referred to as a *virtual path*.</span></span>

<span data-ttu-id="32d4f-435">如需詳細資訊，請參閱 <xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="32d4f-435">For more information, see <xref:fundamentals/static-files>.</span></span>

::: moniker-end
