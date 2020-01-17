---
title: ASP.NET Core 基本概念
author: rick-anderson
description: 了解建置 ASP.NET Core 應用程式的基本概念。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2020
uid: fundamentals/index
ms.openlocfilehash: 3fbfc7c4c0d5e568339bc00a7cbe84a3932acf1f
ms.sourcegitcommit: cbd30479f42cbb3385000ef834d9c7d021fd218d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/16/2020
ms.locfileid: "76146351"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="5efea-103">ASP.NET Core 基本概念</span><span class="sxs-lookup"><span data-stu-id="5efea-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="5efea-104">本文是了解如何開發 ASP.NET Core 應用程式的關鍵主題概觀。</span><span class="sxs-lookup"><span data-stu-id="5efea-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="5efea-105">Startup 類別</span><span class="sxs-lookup"><span data-stu-id="5efea-105">The Startup class</span></span>

<span data-ttu-id="5efea-106">`Startup` 類別是：</span><span class="sxs-lookup"><span data-stu-id="5efea-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="5efea-107">已設定應用程式所需的服務。</span><span class="sxs-lookup"><span data-stu-id="5efea-107">Services required by the app are configured.</span></span>
* <span data-ttu-id="5efea-108">定義要求處理管線的位置。</span><span class="sxs-lookup"><span data-stu-id="5efea-108">The request handling pipeline is defined.</span></span>

<span data-ttu-id="5efea-109">「服務」是應用程式所使用的元件。</span><span class="sxs-lookup"><span data-stu-id="5efea-109">*Services* are components that are used by the app.</span></span> <span data-ttu-id="5efea-110">例如，記錄元件即為一項服務。</span><span class="sxs-lookup"><span data-stu-id="5efea-110">For example, a logging component is a service.</span></span> <span data-ttu-id="5efea-111">設定 (或「註冊」) 服務的程式碼會新增至 `Startup.ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="5efea-111">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="5efea-112">要求處理管線由一系列*中介軟體*元件所組成。</span><span class="sxs-lookup"><span data-stu-id="5efea-112">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="5efea-113">例如，中介軟體可能會處理靜態檔案的要求，或是將 HTTP 要求重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="5efea-113">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="5efea-114">每個中介軟體會在 `HttpContext` 上執行非同步操作，然後叫用管線中下一個中介軟體或終止要求。</span><span class="sxs-lookup"><span data-stu-id="5efea-114">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="5efea-115">設定要求處理管線的程式碼會新增至 `Startup.Configure` 方法。</span><span class="sxs-lookup"><span data-stu-id="5efea-115">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="5efea-116">以下是 `Startup` 類別範例：</span><span class="sxs-lookup"><span data-stu-id="5efea-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="5efea-117">如需詳細資訊，請參閱<xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="5efea-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="5efea-118">相依性插入 (服務)</span><span class="sxs-lookup"><span data-stu-id="5efea-118">Dependency injection (services)</span></span>

<span data-ttu-id="5efea-119">ASP.NET Core 具有內建的相依性插入 (DI) 架構，可讓應用程式的類別使用所設定服務。</span><span class="sxs-lookup"><span data-stu-id="5efea-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="5efea-120">取得類別中一項服務執行個體的其中一種方式，便是使用所需類型的參數來建立建構函式。</span><span class="sxs-lookup"><span data-stu-id="5efea-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="5efea-121">參數可以是服務類型或是介面。</span><span class="sxs-lookup"><span data-stu-id="5efea-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="5efea-122">DI 系統會在執行階段提供服務。</span><span class="sxs-lookup"><span data-stu-id="5efea-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="5efea-123">以下是使用 DI 取得 Entity Framework Core 內容物件的類別。</span><span class="sxs-lookup"><span data-stu-id="5efea-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="5efea-124">醒目提示的那一行便是建構函式插入的範例：</span><span class="sxs-lookup"><span data-stu-id="5efea-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="5efea-125">雖然 DI 為內建，但其設計用於讓您插入協力廠商的控制反轉 (IoC) 容器 (若您想要的話)。</span><span class="sxs-lookup"><span data-stu-id="5efea-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="5efea-126">如需詳細資訊，請參閱<xref:fundamentals/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="5efea-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="5efea-127">中介軟體</span><span class="sxs-lookup"><span data-stu-id="5efea-127">Middleware</span></span>

<span data-ttu-id="5efea-128">要求處理管線是以一系列中介軟體元件組成。</span><span class="sxs-lookup"><span data-stu-id="5efea-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="5efea-129">每個元件會在 `HttpContext` 上執行非同步操作，然後叫用管線中下一個中介軟體或終止要求。</span><span class="sxs-lookup"><span data-stu-id="5efea-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="5efea-130">依照慣例，中介軟體元件會透過叫用其 `Startup.Configure` 方法中的 `Use...` 延伸模組方法來新增至管線。</span><span class="sxs-lookup"><span data-stu-id="5efea-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="5efea-131">例如，若要啟用靜態檔案轉譯，請呼叫 `UseStaticFiles`。</span><span class="sxs-lookup"><span data-stu-id="5efea-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="5efea-132">下列範例中醒目提示的程式碼會設定要求處理管線：</span><span class="sxs-lookup"><span data-stu-id="5efea-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="5efea-133">ASP.NET Core 包含一組豐富的內建中介軟體，您也可以撰寫自訂中介軟體。</span><span class="sxs-lookup"><span data-stu-id="5efea-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="5efea-134">如需詳細資訊，請參閱<xref:fundamentals/middleware/index>。</span><span class="sxs-lookup"><span data-stu-id="5efea-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="5efea-135">主機</span><span class="sxs-lookup"><span data-stu-id="5efea-135">Host</span></span>

<span data-ttu-id="5efea-136">ASP.NET Core 應用程式會在啟動時建置一個「主機」。</span><span class="sxs-lookup"><span data-stu-id="5efea-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="5efea-137">主機是封裝所有應用程式資源的物件，例如：</span><span class="sxs-lookup"><span data-stu-id="5efea-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="5efea-138">HTTP 伺服器實作</span><span class="sxs-lookup"><span data-stu-id="5efea-138">An HTTP server implementation</span></span>
* <span data-ttu-id="5efea-139">中介軟體元件</span><span class="sxs-lookup"><span data-stu-id="5efea-139">Middleware components</span></span>
* <span data-ttu-id="5efea-140">記錄</span><span class="sxs-lookup"><span data-stu-id="5efea-140">Logging</span></span>
* <span data-ttu-id="5efea-141">DI</span><span class="sxs-lookup"><span data-stu-id="5efea-141">DI</span></span>
* <span data-ttu-id="5efea-142">組態</span><span class="sxs-lookup"><span data-stu-id="5efea-142">Configuration</span></span>

<span data-ttu-id="5efea-143">在單一物件中包含所有應用程式相互依存資源的主要理由便是生命週期管理：控制應用程式的啟動及順利關機。</span><span class="sxs-lookup"><span data-stu-id="5efea-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5efea-144">可以使用兩種主機：一般主機與 Web 主機。</span><span class="sxs-lookup"><span data-stu-id="5efea-144">Two hosts are available: the Generic Host and the Web Host.</span></span> <span data-ttu-id="5efea-145">建議您使用一般主機，Web 主機只適用於回溯相容性。</span><span class="sxs-lookup"><span data-stu-id="5efea-145">The Generic Host is recommended, and the Web Host is available only for backwards compatibility.</span></span>

<span data-ttu-id="5efea-146">建立主機的程式碼位於 `Program.Main` 中：</span><span class="sxs-lookup"><span data-stu-id="5efea-146">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs)]

<span data-ttu-id="5efea-147">`CreateDefaultBuilder` 和 `ConfigureWebHostDefaults` 方法會設定具備一般常用選項的主機，如下所示：</span><span class="sxs-lookup"><span data-stu-id="5efea-147">The `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods configure a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="5efea-148">使用 [Kestrel](#servers) 作為網頁伺服器，並啟用 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="5efea-148">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="5efea-149">從 *appsettings.json*、*appsettings.{Environment Name}.json*、環境變數與命令列引數載入設定。</span><span class="sxs-lookup"><span data-stu-id="5efea-149">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="5efea-150">將記錄輸出傳送到主控台及偵錯提供者。</span><span class="sxs-lookup"><span data-stu-id="5efea-150">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="5efea-151">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host>。</span><span class="sxs-lookup"><span data-stu-id="5efea-151">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5efea-152">可以使用兩種主機：Web 主機與一般主機。</span><span class="sxs-lookup"><span data-stu-id="5efea-152">Two hosts are available: the Web Host and the Generic Host.</span></span> <span data-ttu-id="5efea-153">在 ASP.NET Core 2.x 中，一般主機僅適用於非 Web 案例。</span><span class="sxs-lookup"><span data-stu-id="5efea-153">In ASP.NET Core 2.x, the Generic Host is only for non-web scenarios.</span></span>

<span data-ttu-id="5efea-154">建立主機的程式碼位於 `Program.Main` 中：</span><span class="sxs-lookup"><span data-stu-id="5efea-154">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs)]

<span data-ttu-id="5efea-155">`CreateDefaultBuilder` 方法會設定具備一般常用選項的主機，如下所示：</span><span class="sxs-lookup"><span data-stu-id="5efea-155">The `CreateDefaultBuilder` method configures a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="5efea-156">使用 [Kestrel](#servers) 作為網頁伺服器，並啟用 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="5efea-156">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="5efea-157">從 *appsettings.json*、*appsettings.{Environment Name}.json*、環境變數與命令列引數載入設定。</span><span class="sxs-lookup"><span data-stu-id="5efea-157">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="5efea-158">將記錄輸出傳送到主控台及偵錯提供者。</span><span class="sxs-lookup"><span data-stu-id="5efea-158">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="5efea-159">如需詳細資訊，請參閱<xref:fundamentals/host/web-host>。</span><span class="sxs-lookup"><span data-stu-id="5efea-159">For more information, see <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

### <a name="non-web-scenarios"></a><span data-ttu-id="5efea-160">非 Web 案例</span><span class="sxs-lookup"><span data-stu-id="5efea-160">Non-web scenarios</span></span>

<span data-ttu-id="5efea-161">一般主機允許其他類型的應用程式，使用交叉剪輯架構延伸模組，例如記錄、相依性插入 (DI)、設定與應用程式存留期管理。</span><span class="sxs-lookup"><span data-stu-id="5efea-161">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="5efea-162">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host>和<xref:fundamentals/host/hosted-services>。</span><span class="sxs-lookup"><span data-stu-id="5efea-162">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="5efea-163">伺服器</span><span class="sxs-lookup"><span data-stu-id="5efea-163">Servers</span></span>

<span data-ttu-id="5efea-164">ASP.NET Core 應用程式使用 HTTP 伺服器實作來接聽 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="5efea-164">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="5efea-165">伺服器會把向應用程式發出的要求作為一組[要求功能](xref:fundamentals/request-features)，合併成一個 `HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="5efea-165">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="5efea-166">Windows</span><span class="sxs-lookup"><span data-stu-id="5efea-166">Windows</span></span>](#tab/windows)

<span data-ttu-id="5efea-167">ASP.NET Core 隨附下列伺服器實作：</span><span class="sxs-lookup"><span data-stu-id="5efea-167">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="5efea-168">*Kestrel* 是跨平台的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="5efea-168">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="5efea-169">Kestrel 通常會使用 [IIS](https://www.iis.net/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="5efea-169">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="5efea-170">在 ASP.NET Core 2.0 或更新版本中，Kestrel 可以作為直接向網際網路公開的公眾 Edge Server 執行。</span><span class="sxs-lookup"><span data-stu-id="5efea-170">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="5efea-171">*IIS HTTP 伺服器*則是適用於使用 IIS Windows 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="5efea-171">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="5efea-172">透過此伺服器，ASP.NET Core 應用程式及 IIS 便可以在相同處理序中執行。</span><span class="sxs-lookup"><span data-stu-id="5efea-172">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="5efea-173">*HTTP.sys* 則是適用於並未搭配 IIS 使用 Windows 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="5efea-173">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="5efea-174">macOS</span><span class="sxs-lookup"><span data-stu-id="5efea-174">macOS</span></span>](#tab/macos)

<span data-ttu-id="5efea-175">ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="5efea-175">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="5efea-176">在 ASP.NET Core 2.0 或更新版本中，Kestrel 可以作為直接向網際網路公開的公眾 Edge Server 執行。</span><span class="sxs-lookup"><span data-stu-id="5efea-176">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="5efea-177">Kestrel 通常會使用 [Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="5efea-177">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="5efea-178">Linux</span><span class="sxs-lookup"><span data-stu-id="5efea-178">Linux</span></span>](#tab/linux)

<span data-ttu-id="5efea-179">ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="5efea-179">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="5efea-180">在 ASP.NET Core 2.0 或更新版本中，Kestrel 可以作為直接向網際網路公開的公眾 Edge Server 執行。</span><span class="sxs-lookup"><span data-stu-id="5efea-180">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="5efea-181">Kestrel 通常會使用 [Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="5efea-181">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="5efea-182">Windows</span><span class="sxs-lookup"><span data-stu-id="5efea-182">Windows</span></span>](#tab/windows)

<span data-ttu-id="5efea-183">ASP.NET Core 隨附下列伺服器實作：</span><span class="sxs-lookup"><span data-stu-id="5efea-183">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="5efea-184">*Kestrel* 是跨平台的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="5efea-184">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="5efea-185">Kestrel 通常會使用 [IIS](https://www.iis.net/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="5efea-185">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="5efea-186">在 ASP.NET Core 2.0 或更新版本中，Kestrel 可以作為直接向網際網路公開的公眾 Edge Server 執行。</span><span class="sxs-lookup"><span data-stu-id="5efea-186">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="5efea-187">*HTTP.sys* 則是適用於並未搭配 IIS 使用 Windows 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="5efea-187">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="5efea-188">macOS</span><span class="sxs-lookup"><span data-stu-id="5efea-188">macOS</span></span>](#tab/macos)

<span data-ttu-id="5efea-189">ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="5efea-189">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="5efea-190">在 ASP.NET Core 2.0 或更新版本中，Kestrel 可以作為直接向網際網路公開的公眾 Edge Server 執行。</span><span class="sxs-lookup"><span data-stu-id="5efea-190">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="5efea-191">Kestrel 通常會使用 [Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="5efea-191">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="5efea-192">Linux</span><span class="sxs-lookup"><span data-stu-id="5efea-192">Linux</span></span>](#tab/linux)

<span data-ttu-id="5efea-193">ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="5efea-193">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="5efea-194">在 ASP.NET Core 2.0 或更新版本中，Kestrel 可以作為直接向網際網路公開的公眾 Edge Server 執行。</span><span class="sxs-lookup"><span data-stu-id="5efea-194">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="5efea-195">Kestrel 通常會使用 [Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="5efea-195">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

<span data-ttu-id="5efea-196">如需詳細資訊，請參閱<xref:fundamentals/servers/index>。</span><span class="sxs-lookup"><span data-stu-id="5efea-196">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="5efea-197">組態</span><span class="sxs-lookup"><span data-stu-id="5efea-197">Configuration</span></span>

<span data-ttu-id="5efea-198">ASP.NET Core 提供組態架構，可從組態提供者的已排序集合中，以成對名稱和數值的形式取得設定。</span><span class="sxs-lookup"><span data-stu-id="5efea-198">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="5efea-199">您可以使用各種來源的內建組態提供者，例如 *.json* 檔案、 *.xml* 檔案、環境變數及命令列引數。</span><span class="sxs-lookup"><span data-stu-id="5efea-199">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="5efea-200">您也可以撰寫自訂組態提供者。</span><span class="sxs-lookup"><span data-stu-id="5efea-200">You can also write custom configuration providers.</span></span>

<span data-ttu-id="5efea-201">例如，您可以指定組態來自 *appsettings.json* 和環境變數。</span><span class="sxs-lookup"><span data-stu-id="5efea-201">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="5efea-202">然後當要求 *ConnectionString* 的值時，架構便會先在 *appsettings.json* 檔案中尋找。</span><span class="sxs-lookup"><span data-stu-id="5efea-202">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="5efea-203">若有找到值，但在環境變數中也存在該值，則會優先使用來自環境變數的值。</span><span class="sxs-lookup"><span data-stu-id="5efea-203">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="5efea-204">針對管理保密組態資料 (例如密碼)，ASP.NET Core 提供[祕密管理員工具](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="5efea-204">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="5efea-205">針對生產祕密，我們建議使用 [Azure Key Vault](xref:security/key-vault-configuration)。</span><span class="sxs-lookup"><span data-stu-id="5efea-205">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="5efea-206">如需詳細資訊，請參閱<xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="5efea-206">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="5efea-207">選項</span><span class="sxs-lookup"><span data-stu-id="5efea-207">Options</span></span>

<span data-ttu-id="5efea-208">在可能的情況下，ASP.NET Core 會遵循「選項模式」來儲存及擷取組態值。</span><span class="sxs-lookup"><span data-stu-id="5efea-208">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="5efea-209">選項模式使用類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="5efea-209">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="5efea-210">例如，下列程式碼會設定 WebSocket 選項：</span><span class="sxs-lookup"><span data-stu-id="5efea-210">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="5efea-211">如需詳細資訊，請參閱<xref:fundamentals/configuration/options>。</span><span class="sxs-lookup"><span data-stu-id="5efea-211">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="5efea-212">環境</span><span class="sxs-lookup"><span data-stu-id="5efea-212">Environments</span></span>

<span data-ttu-id="5efea-213">執行環境 (例如「開發」、「預備」及「生產」) 是 ASP.NET Core 中的第一級概念。</span><span class="sxs-lookup"><span data-stu-id="5efea-213">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="5efea-214">您可以透過設定 `ASPNETCORE_ENVIRONMENT` 環境變數，指定應用程式執行的環境。</span><span class="sxs-lookup"><span data-stu-id="5efea-214">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="5efea-215">ASP.NET Core 會在應用程式啟動時讀取環境變數，然後將值儲存在 `IHostingEnvironment` 實作中。</span><span class="sxs-lookup"><span data-stu-id="5efea-215">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="5efea-216">環境物件可在應用程式中的任何位置，透過 DI 使用。</span><span class="sxs-lookup"><span data-stu-id="5efea-216">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="5efea-217">下列 `Startup` 類別的範例程式碼會設定應用程式，只在於「開發」環境中執行時提供詳細的錯誤資訊：</span><span class="sxs-lookup"><span data-stu-id="5efea-217">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="5efea-218">如需詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="5efea-218">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="5efea-219">記錄</span><span class="sxs-lookup"><span data-stu-id="5efea-219">Logging</span></span>

<span data-ttu-id="5efea-220">ASP.NET Core 支援記錄 API，此 API 能與各種內建和第三方記錄提供者搭配使用。</span><span class="sxs-lookup"><span data-stu-id="5efea-220">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="5efea-221">可用的提供者包括如下：</span><span class="sxs-lookup"><span data-stu-id="5efea-221">Available providers include the following:</span></span>

* <span data-ttu-id="5efea-222">主控台</span><span class="sxs-lookup"><span data-stu-id="5efea-222">Console</span></span>
* <span data-ttu-id="5efea-223">偵錯</span><span class="sxs-lookup"><span data-stu-id="5efea-223">Debug</span></span>
* <span data-ttu-id="5efea-224">Windows 上的事件追蹤</span><span class="sxs-lookup"><span data-stu-id="5efea-224">Event Tracing on Windows</span></span>
* <span data-ttu-id="5efea-225">Windows 事件日誌</span><span class="sxs-lookup"><span data-stu-id="5efea-225">Windows Event Log</span></span>
* <span data-ttu-id="5efea-226">TraceSource</span><span class="sxs-lookup"><span data-stu-id="5efea-226">TraceSource</span></span>
* <span data-ttu-id="5efea-227">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5efea-227">Azure App Service</span></span>
* <span data-ttu-id="5efea-228">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="5efea-228">Azure Application Insights</span></span>

<span data-ttu-id="5efea-229">透過從 DI 取得 `ILogger` 物件並呼叫記錄方法，從應用程式程式碼中的任何位置寫入記錄。</span><span class="sxs-lookup"><span data-stu-id="5efea-229">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="5efea-230">以下是使用 `ILogger` 物件的範例程式碼，其中建構函式插入及記錄方法呼叫都已醒目提示。</span><span class="sxs-lookup"><span data-stu-id="5efea-230">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="5efea-231">`ILogger` 介面可讓您將任何數量的欄位傳遞給記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="5efea-231">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="5efea-232">欄位常用於建構訊息字串，但提供者也可以將它們作為個別欄位，傳送至資料存放區。</span><span class="sxs-lookup"><span data-stu-id="5efea-232">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="5efea-233">這項功能可讓記錄提供者實作 [semantic logging (語意記錄)，又稱為 structured logging (結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。</span><span class="sxs-lookup"><span data-stu-id="5efea-233">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="5efea-234">如需詳細資訊，請參閱<xref:fundamentals/logging/index>。</span><span class="sxs-lookup"><span data-stu-id="5efea-234">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="5efea-235">路由</span><span class="sxs-lookup"><span data-stu-id="5efea-235">Routing</span></span>

<span data-ttu-id="5efea-236">「路由」是一種對應到處理常式的 URL 模式。</span><span class="sxs-lookup"><span data-stu-id="5efea-236">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="5efea-237">處理常式通常是 Razor 頁面、MVC 控制器中的動作方法，或是中介軟體。</span><span class="sxs-lookup"><span data-stu-id="5efea-237">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="5efea-238">ASP.NET Core 路由可讓您控制您應用程式使用的 URL。</span><span class="sxs-lookup"><span data-stu-id="5efea-238">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="5efea-239">如需詳細資訊，請參閱<xref:fundamentals/routing>。</span><span class="sxs-lookup"><span data-stu-id="5efea-239">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="5efea-240">錯誤處理</span><span class="sxs-lookup"><span data-stu-id="5efea-240">Error handling</span></span>

<span data-ttu-id="5efea-241">ASP.NET Core 具有處理錯誤的內建功能，例如：</span><span class="sxs-lookup"><span data-stu-id="5efea-241">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="5efea-242">開發人員例外狀況頁面</span><span class="sxs-lookup"><span data-stu-id="5efea-242">A developer exception page</span></span>
* <span data-ttu-id="5efea-243">自訂錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="5efea-243">Custom error pages</span></span>
* <span data-ttu-id="5efea-244">靜態狀態碼頁面</span><span class="sxs-lookup"><span data-stu-id="5efea-244">Static status code pages</span></span>
* <span data-ttu-id="5efea-245">啟動例外狀況處理</span><span class="sxs-lookup"><span data-stu-id="5efea-245">Startup exception handling</span></span>

<span data-ttu-id="5efea-246">如需詳細資訊，請參閱<xref:fundamentals/error-handling>。</span><span class="sxs-lookup"><span data-stu-id="5efea-246">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="5efea-247">發出 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="5efea-247">Make HTTP requests</span></span>

<span data-ttu-id="5efea-248">`IHttpClientFactory` 的實作可用於建立 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="5efea-248">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="5efea-249">Factory：</span><span class="sxs-lookup"><span data-stu-id="5efea-249">The factory:</span></span>

* <span data-ttu-id="5efea-250">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="5efea-250">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="5efea-251">例如，您可以註冊 *github* 並設定用戶端以存取 GitHub。</span><span class="sxs-lookup"><span data-stu-id="5efea-251">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="5efea-252">預設用戶端可以註冊用於其他用途。</span><span class="sxs-lookup"><span data-stu-id="5efea-252">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="5efea-253">支援註冊及多個委派處理常式的鏈結，以用於建置傳出要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="5efea-253">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="5efea-254">此模式與 ASP.NET Core 中的輸入中介軟體管線相似。</span><span class="sxs-lookup"><span data-stu-id="5efea-254">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="5efea-255">模式提供一個機制來管理 HTTP 要求的跨領域關注，包括快取、錯誤處理、序列化和記錄。</span><span class="sxs-lookup"><span data-stu-id="5efea-255">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="5efea-256">與 *Polly* 整合，Polly 是一種熱門的協力廠商程式庫，用於進行暫時性的錯誤處理。</span><span class="sxs-lookup"><span data-stu-id="5efea-256">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="5efea-257">管理基礎 `HttpClientMessageHandler` 執行個體的共用和存留期，以避免在手動管理 `HttpClient` 存留期時，發生的常見 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="5efea-257">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="5efea-258">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="5efea-258">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="5efea-259">如需詳細資訊，請參閱<xref:fundamentals/http-requests>。</span><span class="sxs-lookup"><span data-stu-id="5efea-259">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="5efea-260">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="5efea-260">Content root</span></span>

<span data-ttu-id="5efea-261">內容根目錄是的基底路徑：</span><span class="sxs-lookup"><span data-stu-id="5efea-261">The content root is the base path to the:</span></span>

* <span data-ttu-id="5efea-262">裝載應用程式的可執行檔（ *.exe*）。</span><span class="sxs-lookup"><span data-stu-id="5efea-262">Executable hosting the app (*.exe*).</span></span>
* <span data-ttu-id="5efea-263">組成應用程式的已編譯元件（ *.dll*）。</span><span class="sxs-lookup"><span data-stu-id="5efea-263">Compiled assemblies that make up the app (*.dll*).</span></span>
* <span data-ttu-id="5efea-264">應用程式所使用的非程式碼內容檔案，例如：</span><span class="sxs-lookup"><span data-stu-id="5efea-264">Non-code content files used by the app, such as:</span></span>
  * <span data-ttu-id="5efea-265">Razor 檔案（ *. cshtml*， *razor*）</span><span class="sxs-lookup"><span data-stu-id="5efea-265">Razor files (*.cshtml*, *.razor*)</span></span>
  * <span data-ttu-id="5efea-266">設定檔（ *. json*、 *.xml*）</span><span class="sxs-lookup"><span data-stu-id="5efea-266">Configuration files (*.json*, *.xml*)</span></span>
  * <span data-ttu-id="5efea-267">資料檔案（ *.db*）</span><span class="sxs-lookup"><span data-stu-id="5efea-267">Data files (*.db*)</span></span>
* <span data-ttu-id="5efea-268">[Web 根目錄](#web-root)，通常是已發佈的*wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="5efea-268">[Web root](#web-root), typically the published *wwwroot* folder.</span></span>

<span data-ttu-id="5efea-269">在開發期間：</span><span class="sxs-lookup"><span data-stu-id="5efea-269">During development:</span></span>

* <span data-ttu-id="5efea-270">內容根目錄預設為專案的根目錄。</span><span class="sxs-lookup"><span data-stu-id="5efea-270">The content root defaults to the project's root directory.</span></span>
* <span data-ttu-id="5efea-271">專案的根目錄是用來建立：</span><span class="sxs-lookup"><span data-stu-id="5efea-271">The project's root directory is used to create the:</span></span>
  * <span data-ttu-id="5efea-272">應用程式在專案根目錄中的非程式碼內容檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="5efea-272">Path to the app's non-code content files in the project's root directory.</span></span>
  * <span data-ttu-id="5efea-273">[Web 根目錄](#web-root)，通常是專案根目錄中的*wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="5efea-273">[Web root](#web-root), typically the *wwwroot* folder in the project's root directory.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5efea-274">[建立主機](#host)時，可以指定替代的內容根路徑。</span><span class="sxs-lookup"><span data-stu-id="5efea-274">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="5efea-275">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host#contentrootpath>。</span><span class="sxs-lookup"><span data-stu-id="5efea-275">For more information, see <xref:fundamentals/host/generic-host#contentrootpath>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5efea-276">[建立主機](#host)時，可以指定替代的內容根路徑。</span><span class="sxs-lookup"><span data-stu-id="5efea-276">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="5efea-277">如需詳細資訊，請參閱<xref:fundamentals/host/web-host#content-root>。</span><span class="sxs-lookup"><span data-stu-id="5efea-277">For more information, see <xref:fundamentals/host/web-host#content-root>.</span></span>

::: moniker-end

## <a name="web-root"></a><span data-ttu-id="5efea-278">Web 根目錄</span><span class="sxs-lookup"><span data-stu-id="5efea-278">Web root</span></span>

<span data-ttu-id="5efea-279">Web 根目錄是公用、非程式碼、靜態資源檔的基底路徑，例如：</span><span class="sxs-lookup"><span data-stu-id="5efea-279">The web root is the base path to public, non-code, static resource files, such as:</span></span>

* <span data-ttu-id="5efea-280">樣式表單（ *.css*）</span><span class="sxs-lookup"><span data-stu-id="5efea-280">Stylesheets (*.css*)</span></span>
* <span data-ttu-id="5efea-281">JavaScript （ *.js*）</span><span class="sxs-lookup"><span data-stu-id="5efea-281">JavaScript (*.js*)</span></span>
* <span data-ttu-id="5efea-282">影像（ *.png*、 *.jpg*）</span><span class="sxs-lookup"><span data-stu-id="5efea-282">Images (*.png*, *.jpg*)</span></span>

<span data-ttu-id="5efea-283">根據預設，靜態檔案只會從 web 根目錄（和子目錄）提供。</span><span class="sxs-lookup"><span data-stu-id="5efea-283">Static files are only served by default from the web root directory (and sub-directories).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5efea-284">Web 根目錄路徑預設為 *{content root}/wwwroot*，但在[建立主機](#host)時，可以指定不同的 web 根目錄。</span><span class="sxs-lookup"><span data-stu-id="5efea-284">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="5efea-285">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host#webroot>。</span><span class="sxs-lookup"><span data-stu-id="5efea-285">For more information, see <xref:fundamentals/host/generic-host#webroot>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5efea-286">Web 根目錄路徑預設為 *{content root}/wwwroot*，但在[建立主機](#host)時，可以指定不同的 web 根目錄。</span><span class="sxs-lookup"><span data-stu-id="5efea-286">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="5efea-287">如需詳細資訊，請參閱 [Web 根目錄](xref:fundamentals/host/web-host#web-root)。</span><span class="sxs-lookup"><span data-stu-id="5efea-287">For more information, see [Web root](xref:fundamentals/host/web-host#web-root).</span></span>

::: moniker-end

<span data-ttu-id="5efea-288">使用專案檔中的[\<內容 > 專案專案](/visualstudio/msbuild/common-msbuild-project-items#content)，防止在*wwwroot*中發行檔案。</span><span class="sxs-lookup"><span data-stu-id="5efea-288">Prevent publishing files in *wwwroot* with the [\<Content> project item](/visualstudio/msbuild/common-msbuild-project-items#content) in the project file.</span></span> <span data-ttu-id="5efea-289">下列範例會防止在*wwwroot/本機*目錄和子目錄中發佈內容：</span><span class="sxs-lookup"><span data-stu-id="5efea-289">The following example prevents publishing content in the *wwwroot/local* directory and sub-directories:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5efea-290">若要防止將靜態身分識別資產發行至 web 根目錄，請參閱 <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>。</span><span class="sxs-lookup"><span data-stu-id="5efea-290">To prevent publishing static Identity assets to the web root, see <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span></span>

::: moniker-end

<span data-ttu-id="5efea-291">在 Razor （*cshtml*）檔案中，波狀符號斜線（`~/`）會指向 web 根目錄。</span><span class="sxs-lookup"><span data-stu-id="5efea-291">In Razor (*.cshtml*) files, the tilde-slash (`~/`) points to the web root.</span></span> <span data-ttu-id="5efea-292">以 `~/` 開頭的路徑稱為*虛擬路徑*。</span><span class="sxs-lookup"><span data-stu-id="5efea-292">A path beginning with `~/` is referred to as a *virtual path*.</span></span>

<span data-ttu-id="5efea-293">如需詳細資訊，請參閱<xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="5efea-293">For more information, see <xref:fundamentals/static-files>.</span></span>
