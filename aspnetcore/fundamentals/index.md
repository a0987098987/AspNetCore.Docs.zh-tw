---
title: ASP.NET Core 基本概念
author: rick-anderson
description: 探索 ASP.NET Core 應用程式的基本建置概念。
ms.author: riande
ms.custom: mvc
ms.date: 08/20/2018
uid: fundamentals/index
ms.openlocfilehash: 68760f179c4d6e806510b727e2284f8c2c4a4ff6
ms.sourcegitcommit: d27317c16f113e7c111583042ec7e4c5a26adf6f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/20/2018
ms.locfileid: "41746060"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="c89f1-103">ASP.NET Core 基本概念</span><span class="sxs-lookup"><span data-stu-id="c89f1-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="c89f1-104">ASP.NET Core 應用程式是一種主控台應用程式，可使用其 `Main` 方法建立網頁伺服器：</span><span class="sxs-lookup"><span data-stu-id="c89f1-104">An ASP.NET Core app is a console app that creates a web server in its `Main` method:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

<span data-ttu-id="c89f1-105">`Main` 方法會叫用 [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*)，這會遵循[建立器模式](https://wikipedia.org/wiki/Builder_pattern)來建立 Web 主機。</span><span class="sxs-lookup"><span data-stu-id="c89f1-105">The `Main` method invokes [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web host.</span></span> <span data-ttu-id="c89f1-106">產生器具有定義網頁伺服器 (例如，<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) 和啟動類別 (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>) 的方法。</span><span class="sxs-lookup"><span data-stu-id="c89f1-106">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="c89f1-107">以前述範例而言，會自動配置 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="c89f1-107">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="c89f1-108">ASP.NET Core 的 Web 主機會嘗試在 IIS 上執行 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="c89f1-108">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="c89f1-109">其他網頁伺服器 (例如 [HTTP.sys](xref:fundamentals/servers/httpsys)) 則可透過叫用適當的擴充方法來使用。</span><span class="sxs-lookup"><span data-stu-id="c89f1-109">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="c89f1-110">`UseStartup` 將於下一節進一步說明。</span><span class="sxs-lookup"><span data-stu-id="c89f1-110">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="c89f1-111"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> 是 `WebHost.CreateDefaultBuilder` 叫用的傳回型別，提供了許多選擇性方法。</span><span class="sxs-lookup"><span data-stu-id="c89f1-111"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="c89f1-112">其中某些方法包括用來在 HTTP.sys 中裝載應用程式的 `UseHttpSys`，以及用於指定根內容目錄的 <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*>。</span><span class="sxs-lookup"><span data-stu-id="c89f1-112">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="c89f1-113"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> 與 <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> 方法則會建置 <xref:Microsoft.AspNetCore.Hosting.IWebHost> 物件，裝載應用程式並開始接聽 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="c89f1-113">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

<span data-ttu-id="c89f1-114">`Main` 方法會使用 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>，這會遵循[建立器模式](https://wikipedia.org/wiki/Builder_pattern)來建立 Web 應用程式主機。</span><span class="sxs-lookup"><span data-stu-id="c89f1-114">The `Main` method uses <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web app host.</span></span> <span data-ttu-id="c89f1-115">產生器具有定義網頁伺服器 (例如，<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) 和啟動類別 (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>) 的方法。</span><span class="sxs-lookup"><span data-stu-id="c89f1-115">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="c89f1-116">在上述範例中，會使用 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="c89f1-116">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="c89f1-117">其他網頁伺服器 (例如 [WebListener](xref:fundamentals/servers/weblistener)) 則可透過叫用適當的擴充方法來使用。</span><span class="sxs-lookup"><span data-stu-id="c89f1-117">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="c89f1-118">`UseStartup` 將於下一節進一步說明。</span><span class="sxs-lookup"><span data-stu-id="c89f1-118">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="c89f1-119">`WebHostBuilder` 提供許多選擇性方法，包括用來裝載於 IIS 和 IIS Express 中的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*>，以及用於指定根內容目錄的 <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*>。</span><span class="sxs-lookup"><span data-stu-id="c89f1-119">`WebHostBuilder` provides many optional methods, including <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> for hosting in IIS and IIS Express and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="c89f1-120"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> 與 <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> 方法則會建置 <xref:Microsoft.AspNetCore.Hosting.IWebHost> 物件，裝載應用程式並開始接聽 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="c89f1-120">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

## <a name="startup"></a><span data-ttu-id="c89f1-121">啟動</span><span class="sxs-lookup"><span data-stu-id="c89f1-121">Startup</span></span>

<span data-ttu-id="c89f1-122">`WebHostBuilder` 上的 `UseStartup` 方法可為應用程式指定 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="c89f1-122">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

<span data-ttu-id="c89f1-123">`Startup` 類別是您用來定義要求處理管線以及設定應用程式所需之所有服務的位置。</span><span class="sxs-lookup"><span data-stu-id="c89f1-123">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="c89f1-124">`Startup` 必須是公用類別，而且包含下列方法：</span><span class="sxs-lookup"><span data-stu-id="c89f1-124">The `Startup` class must be public and contain the following methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<span data-ttu-id="c89f1-125"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 會定義應用程式所使用的[服務](#dependency-injection-services) (例如 ASP.NET Core MVC、Entity Framework Core、Identity 等)。</span><span class="sxs-lookup"><span data-stu-id="c89f1-125"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="c89f1-126"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> 定義在要求管線中呼叫的[中介軟體](xref:fundamentals/middleware/index)。</span><span class="sxs-lookup"><span data-stu-id="c89f1-126"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> defines the [middleware](xref:fundamentals/middleware/index) called in the request pipeline.</span></span>

<span data-ttu-id="c89f1-127">如需詳細資訊，請參閱<xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="c89f1-127">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="content-root"></a><span data-ttu-id="c89f1-128">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="c89f1-128">Content root</span></span>

<span data-ttu-id="c89f1-129">內容根目錄是應用程式所使用任何內容的基底路徑，例如 [Razor 頁面](xref:razor-pages/index)、MVC 檢視和靜態資產。</span><span class="sxs-lookup"><span data-stu-id="c89f1-129">The content root is the base path to any content used by the app, such as [Razor Pages](xref:razor-pages/index), MVC views, and static assets.</span></span> <span data-ttu-id="c89f1-130">根據預設，內容根目錄與裝載應用程式之可執行檔的應用程式基底路徑相同。</span><span class="sxs-lookup"><span data-stu-id="c89f1-130">By default, the content root is the same location as the app base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="c89f1-131">Web 根目錄</span><span class="sxs-lookup"><span data-stu-id="c89f1-131">Web root</span></span>

<span data-ttu-id="c89f1-132">應用程式的 Web 根目錄是專案中包含公用、靜態資源 (例如 CSS、JavaScript 和影像檔) 的目錄。</span><span class="sxs-lookup"><span data-stu-id="c89f1-132">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="c89f1-133">相依性插入 (服務)</span><span class="sxs-lookup"><span data-stu-id="c89f1-133">Dependency injection (services)</span></span>

<span data-ttu-id="c89f1-134">「服務」是一種在應用程式中常用的元件。</span><span class="sxs-lookup"><span data-stu-id="c89f1-134">A *service* is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="c89f1-135">服務可透過[相依性插入 (DI)](xref:fundamentals/dependency-injection) 提供。</span><span class="sxs-lookup"><span data-stu-id="c89f1-135">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c89f1-136">ASP.NET Core 包含原生逆轉控制 (IoC) 容器，其根據預設支援[建構函式插入](xref:mvc/controllers/dependency-injection#constructor-injection)。</span><span class="sxs-lookup"><span data-stu-id="c89f1-136">ASP.NET Core includes a native Inversion of Control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="c89f1-137">您可視需要取代預設容器。</span><span class="sxs-lookup"><span data-stu-id="c89f1-137">You can replace the default container if you wish.</span></span> <span data-ttu-id="c89f1-138">DI 除了具有[鬆散結合的優點](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation)之外，還能夠讓整個應用程式皆可使用服務，例如[記錄](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="c89f1-138">In addition to its [loose coupling benefit](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI makes services, such as [logging](xref:fundamentals/logging/index), available throughout your app.</span></span>

<span data-ttu-id="c89f1-139">如需詳細資訊，請參閱<xref:fundamentals/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="c89f1-139">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="c89f1-140">中介軟體</span><span class="sxs-lookup"><span data-stu-id="c89f1-140">Middleware</span></span>

<span data-ttu-id="c89f1-141">在 ASP.NET Core 中，您可以使用[中介軟體](xref:fundamentals/middleware/index)來撰寫要求管線。</span><span class="sxs-lookup"><span data-stu-id="c89f1-141">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="c89f1-142">ASP.NET Core 中介軟體會在 `HttpContext` 上執行非同步作業，然後叫用管線中下一個中介軟體或終止要求。</span><span class="sxs-lookup"><span data-stu-id="c89f1-142">ASP.NET Core middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="c89f1-143">依照慣例，將稱為 "XYZ" 之中介軟體元件新增至管線的方法是，在 `Configure` 方法中叫用 `UseXYZ` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="c89f1-143">By convention, a middleware component called "XYZ" is added to the pipeline by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="c89f1-144">ASP.NET Core 包含一組豐富的內建中介軟體，您也可以自行撰寫自訂中介軟體。</span><span class="sxs-lookup"><span data-stu-id="c89f1-144">ASP.NET Core includes a rich set of built-in middleware, and you can write your own custom middleware.</span></span> <span data-ttu-id="c89f1-145">ASP.NET Core 應用程式支援 [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin)，這可讓 Web 應用程式與網頁伺服器分離。</span><span class="sxs-lookup"><span data-stu-id="c89f1-145">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), which allows web apps to be decoupled from web servers, is supported in ASP.NET Core apps.</span></span>

<span data-ttu-id="c89f1-146">如需詳細資訊，請參閱 <xref:fundamentals/middleware/index> 與 <xref:fundamentals/owin>。</span><span class="sxs-lookup"><span data-stu-id="c89f1-146">For more information, see <xref:fundamentals/middleware/index> and <xref:fundamentals/owin>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="c89f1-147">初始化 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="c89f1-147">Initiate HTTP requests</span></span>

<span data-ttu-id="c89f1-148"><xref:System.Net.Http.IHttpClientFactory> 可用來存取 <xref:System.Net.Http.HttpClient> 執行個體以發出 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="c89f1-148"><xref:System.Net.Http.IHttpClientFactory> is available to access <xref:System.Net.Http.HttpClient> instances to make HTTP requests.</span></span>

<span data-ttu-id="c89f1-149">如需詳細資訊，請參閱<xref:fundamentals/http-requests>。</span><span class="sxs-lookup"><span data-stu-id="c89f1-149">For more information, see <xref:fundamentals/http-requests>.</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="c89f1-150">環境</span><span class="sxs-lookup"><span data-stu-id="c89f1-150">Environments</span></span>

<span data-ttu-id="c89f1-151">「開發」與「生產」等環境是 ASP.NET Core 中的第一級概念，並可使用環境變數、設定檔案和命令列引數加以設定。</span><span class="sxs-lookup"><span data-stu-id="c89f1-151">Environments, such as *Development* and *Production*, are a first-class notion in ASP.NET Core and can be set using an environment variable, settings file, and command-line argument.</span></span>

<span data-ttu-id="c89f1-152">如需詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="c89f1-152">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="hosting"></a><span data-ttu-id="c89f1-153">裝載</span><span class="sxs-lookup"><span data-stu-id="c89f1-153">Hosting</span></span>

<span data-ttu-id="c89f1-154">ASP.NET Core 應用程式會設定並啟動*主機*，其負責啟動應用程式以及管理存留期。</span><span class="sxs-lookup"><span data-stu-id="c89f1-154">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="c89f1-155">如需詳細資訊，請參閱<xref:fundamentals/host/index>。</span><span class="sxs-lookup"><span data-stu-id="c89f1-155">For more information, see <xref:fundamentals/host/index>.</span></span>

## <a name="servers"></a><span data-ttu-id="c89f1-156">伺服器</span><span class="sxs-lookup"><span data-stu-id="c89f1-156">Servers</span></span>

<span data-ttu-id="c89f1-157">ASP.NET Core 裝載模型不會直接接聽要求。</span><span class="sxs-lookup"><span data-stu-id="c89f1-157">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="c89f1-158">裝載模型需透過 HTTP 伺服器實作，才可將要求轉寄至應用程式。</span><span class="sxs-lookup"><span data-stu-id="c89f1-158">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="c89f1-159">轉寄的要求會包裝成一組可透過介面來存取的功能物件。</span><span class="sxs-lookup"><span data-stu-id="c89f1-159">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="c89f1-160">ASP.NET Core 包含一個受管理的跨平台網頁伺服器，稱為 [Kestrel](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="c89f1-160">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="c89f1-161">Kestrel 通常會在生產網頁伺服器 (例如反向 Proxy 組態中的 [IIS](https://www.iis.net/) 或 [Nginx](http://nginx.org)) 後面執行。</span><span class="sxs-lookup"><span data-stu-id="c89f1-161">Kestrel is commonly run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org) in a reverse proxy configuration.</span></span> <span data-ttu-id="c89f1-162">Kestrel 也可在 ASP.NET Core 2.0 或更新版本中作為直接向網際網路公開的邊緣伺服器執行。</span><span class="sxs-lookup"><span data-stu-id="c89f1-162">Kestrel can also be run as an edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

<span data-ttu-id="c89f1-163">如需詳細資訊，請參閱<xref:fundamentals/servers/index>。</span><span class="sxs-lookup"><span data-stu-id="c89f1-163">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="c89f1-164">Configuration</span><span class="sxs-lookup"><span data-stu-id="c89f1-164">Configuration</span></span>

<span data-ttu-id="c89f1-165">ASP.NET Core 會使用以成對的名稱/值為基礎的組態模型。</span><span class="sxs-lookup"><span data-stu-id="c89f1-165">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="c89f1-166">而非以 <xref:System.Configuration> 或 *web.config* 為基礎的組態模型。組態會從組態提供者經排序的集合中取得設定。</span><span class="sxs-lookup"><span data-stu-id="c89f1-166">The configuration model isn't based on <xref:System.Configuration> or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="c89f1-167">內建的組態提供者支援各種檔案格式 (XML、JSON、INI)、環境變數和命令列引數。</span><span class="sxs-lookup"><span data-stu-id="c89f1-167">The built-in configuration providers support a variety of file formats (XML, JSON, INI), environment variables, and command-line arguments.</span></span> <span data-ttu-id="c89f1-168">您也可以撰寫您自己的自訂組態提供者。</span><span class="sxs-lookup"><span data-stu-id="c89f1-168">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="c89f1-169">如需詳細資訊，請參閱<xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="c89f1-169">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="logging"></a><span data-ttu-id="c89f1-170">記錄</span><span class="sxs-lookup"><span data-stu-id="c89f1-170">Logging</span></span>

<span data-ttu-id="c89f1-171">ASP.NET Core 支援可搭配各種記錄提供者的記錄 API。</span><span class="sxs-lookup"><span data-stu-id="c89f1-171">ASP.NET Core supports a Logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="c89f1-172">內建提供者支援將記錄檔傳送至一或多個目的地。</span><span class="sxs-lookup"><span data-stu-id="c89f1-172">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="c89f1-173">可以使用協力廠商記錄架構。</span><span class="sxs-lookup"><span data-stu-id="c89f1-173">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="c89f1-174">如需詳細資訊，請參閱<xref:fundamentals/logging/index>。</span><span class="sxs-lookup"><span data-stu-id="c89f1-174">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="c89f1-175">錯誤處理</span><span class="sxs-lookup"><span data-stu-id="c89f1-175">Error handling</span></span>

<span data-ttu-id="c89f1-176">ASP.NET Core 具有內建案例，可處理應用程式中的錯誤，包括開發人員例外狀況頁面、自訂錯誤頁面、靜態狀態碼頁面，以及啟動例外狀況處理。</span><span class="sxs-lookup"><span data-stu-id="c89f1-176">ASP.NET Core has built-in scenarios for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="c89f1-177">如需詳細資訊，請參閱<xref:fundamentals/error-handling>。</span><span class="sxs-lookup"><span data-stu-id="c89f1-177">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="routing"></a><span data-ttu-id="c89f1-178">路由</span><span class="sxs-lookup"><span data-stu-id="c89f1-178">Routing</span></span>

<span data-ttu-id="c89f1-179">ASP.NET Core 提供用來將應用程式要求路由至路由處理常式的案例。</span><span class="sxs-lookup"><span data-stu-id="c89f1-179">ASP.NET Core offers scenarios for routing of app requests to route handlers.</span></span>

<span data-ttu-id="c89f1-180">如需詳細資訊，請參閱<xref:fundamentals/routing>。</span><span class="sxs-lookup"><span data-stu-id="c89f1-180">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="file-providers"></a><span data-ttu-id="c89f1-181">檔案提供者</span><span class="sxs-lookup"><span data-stu-id="c89f1-181">File Providers</span></span>

<span data-ttu-id="c89f1-182">ASP.NET Core 透過使用檔案提供者，將檔案系統存取抽象化，而檔案提供者則提供通用介面，讓您可跨平台使用檔案。</span><span class="sxs-lookup"><span data-stu-id="c89f1-182">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="c89f1-183">如需詳細資訊，請參閱<xref:fundamentals/file-providers>。</span><span class="sxs-lookup"><span data-stu-id="c89f1-183">For more information, see <xref:fundamentals/file-providers>.</span></span>

## <a name="static-files"></a><span data-ttu-id="c89f1-184">靜態檔案</span><span class="sxs-lookup"><span data-stu-id="c89f1-184">Static files</span></span>

<span data-ttu-id="c89f1-185">靜態檔案中介軟體負責提供靜態檔案，例如 HTML、CSS、影像和 JavaScript 檔案。</span><span class="sxs-lookup"><span data-stu-id="c89f1-185">Static Files Middleware serves static files, such as HTML, CSS, image, and JavaScript files.</span></span>

<span data-ttu-id="c89f1-186">如需詳細資訊，請參閱<xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="c89f1-186">For more information, see <xref:fundamentals/static-files>.</span></span>

## <a name="session-and-app-state"></a><span data-ttu-id="c89f1-187">工作階段和應用程式狀態</span><span class="sxs-lookup"><span data-stu-id="c89f1-187">Session and app state</span></span>

<span data-ttu-id="c89f1-188">ASP.NET Core 提供數種方法，可在使用者瀏覽 Web 應用程式時保留工作階段與應用程式狀態。</span><span class="sxs-lookup"><span data-stu-id="c89f1-188">ASP.NET Core offers several approaches to preserve session and app state while a user browses a web app.</span></span>

<span data-ttu-id="c89f1-189">如需詳細資訊，請參閱<xref:fundamentals/app-state>。</span><span class="sxs-lookup"><span data-stu-id="c89f1-189">For more information, see <xref:fundamentals/app-state>.</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="c89f1-190">全球化和當地語系化</span><span class="sxs-lookup"><span data-stu-id="c89f1-190">Globalization and localization</span></span>

<span data-ttu-id="c89f1-191">使用 ASP.NET Core 建立多語系網站，讓更廣大的群眾得以使用您的網站。</span><span class="sxs-lookup"><span data-stu-id="c89f1-191">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="c89f1-192">ASP.NET Core 可提供服務與中介軟體，用以將內容當地語系化成不同的語言與文化特性。</span><span class="sxs-lookup"><span data-stu-id="c89f1-192">ASP.NET Core provides services and middleware for localizing content into different languages and cultures.</span></span>

<span data-ttu-id="c89f1-193">如需詳細資訊，請參閱<xref:fundamentals/localization>。</span><span class="sxs-lookup"><span data-stu-id="c89f1-193">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="request-features"></a><span data-ttu-id="c89f1-194">要求功能</span><span class="sxs-lookup"><span data-stu-id="c89f1-194">Request features</span></span>

<span data-ttu-id="c89f1-195">有關 HTTP 要求和回應的網頁伺服器實作詳細資料，定義於介面中。</span><span class="sxs-lookup"><span data-stu-id="c89f1-195">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="c89f1-196">伺服器實作與中介軟體會使用這些介面，建立及修改應用程式的裝載管線。</span><span class="sxs-lookup"><span data-stu-id="c89f1-196">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="c89f1-197">如需詳細資訊，請參閱<xref:fundamentals/request-features>。</span><span class="sxs-lookup"><span data-stu-id="c89f1-197">For more information, see <xref:fundamentals/request-features>.</span></span>

## <a name="background-tasks"></a><span data-ttu-id="c89f1-198">背景工作</span><span class="sxs-lookup"><span data-stu-id="c89f1-198">Background tasks</span></span>

<span data-ttu-id="c89f1-199">背景工作會實作為*託管服務*。</span><span class="sxs-lookup"><span data-stu-id="c89f1-199">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="c89f1-200">託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="c89f1-200">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span>

<span data-ttu-id="c89f1-201">如需詳細資訊，請參閱<xref:fundamentals/host/hosted-services>。</span><span class="sxs-lookup"><span data-stu-id="c89f1-201">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="c89f1-202">存取 HttpContext</span><span class="sxs-lookup"><span data-stu-id="c89f1-202">Access HttpContext</span></span>

<span data-ttu-id="c89f1-203">在使用 Razor 頁面和 MVC 處理要求時，會自動提供 `HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="c89f1-203">`HttpContext` is automatically available when processing requests with Razor Pages and MVC.</span></span> <span data-ttu-id="c89f1-204">在 `HttpContext` 尚無法使用的情況下，您可以透過 <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> 介面及其預設實作 <xref:Microsoft.AspNetCore.Http.HttpContextAccessor> 存取 `HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="c89f1-204">In circumstances where `HttpContext` isn't readily available, you can access the `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span>

<span data-ttu-id="c89f1-205">如需詳細資訊，請參閱<xref:fundamentals/httpcontext>。</span><span class="sxs-lookup"><span data-stu-id="c89f1-205">For more information, see <xref:fundamentals/httpcontext>.</span></span>

## <a name="websockets"></a><span data-ttu-id="c89f1-206">WebSockets</span><span class="sxs-lookup"><span data-stu-id="c89f1-206">WebSockets</span></span>

<span data-ttu-id="c89f1-207">[WebSocket](https://wikipedia.org/wiki/WebSocket) 為通訊協定，其可在 TCP 連線下啟用雙向的持續性通訊通道。</span><span class="sxs-lookup"><span data-stu-id="c89f1-207">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="c89f1-208">其可用於像是聊天、股票行情、遊戲等應用程式，以及您希望在 Web 應用程式中使用即時功能的任何位置。</span><span class="sxs-lookup"><span data-stu-id="c89f1-208">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="c89f1-209">ASP.NET Core 支援 Web 通訊端案例。</span><span class="sxs-lookup"><span data-stu-id="c89f1-209">ASP.NET Core supports web socket scenarios.</span></span>

<span data-ttu-id="c89f1-210">如需詳細資訊，請參閱<xref:fundamentals/websockets>。</span><span class="sxs-lookup"><span data-stu-id="c89f1-210">For more information, see <xref:fundamentals/websockets>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="microsoftaspnetcoreapp-metapackage"></a><span data-ttu-id="c89f1-211">Microsoft.AspNetCore.App 中繼套件</span><span class="sxs-lookup"><span data-stu-id="c89f1-211">Microsoft.AspNetCore.App metapackage</span></span>

<span data-ttu-id="c89f1-212">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) 中繼套件可簡化套件管理。</span><span class="sxs-lookup"><span data-stu-id="c89f1-212">The [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) metapackage simplifies package management.</span></span>

<span data-ttu-id="c89f1-213">如需詳細資訊，請參閱<xref:fundamentals/metapackage-app>。</span><span class="sxs-lookup"><span data-stu-id="c89f1-213">For more information, see <xref:fundamentals/metapackage-app>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="c89f1-214">Microsoft.AspNetCore.All 中繼套件</span><span class="sxs-lookup"><span data-stu-id="c89f1-214">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="c89f1-215">ASP.NET Core 的 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) 中繼套件包括：</span><span class="sxs-lookup"><span data-stu-id="c89f1-215">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="c89f1-216">所有由 ASP.NET Core 小組支援的套件。</span><span class="sxs-lookup"><span data-stu-id="c89f1-216">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="c89f1-217">Entity Framework Core 支援的所有套件。</span><span class="sxs-lookup"><span data-stu-id="c89f1-217">All supported packages by Entity Framework Core.</span></span>
* <span data-ttu-id="c89f1-218">ASP.NET Core 與 Entity Framework Core 所使用的內部與第三人相依性。</span><span class="sxs-lookup"><span data-stu-id="c89f1-218">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="c89f1-219">如需詳細資訊，請參閱<xref:fundamentals/metapackage>。</span><span class="sxs-lookup"><span data-stu-id="c89f1-219">For more information, see <xref:fundamentals/metapackage>.</span></span>

::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="c89f1-220">.NET Core 與 .NET Framework 執行階段</span><span class="sxs-lookup"><span data-stu-id="c89f1-220">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="c89f1-221">ASP.NET Core 應用程式可將目標設為 .NET Core 或 .NET Framework 執行階段。</span><span class="sxs-lookup"><span data-stu-id="c89f1-221">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="c89f1-222">如需詳細資訊，請參閱[在 .NET Core 和 .NET Framework 之間進行選擇](/dotnet/articles/standard/choosing-core-framework-server)。</span><span class="sxs-lookup"><span data-stu-id="c89f1-222">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="c89f1-223">在 ASP.NET Core 與 ASP.NET 之間選擇</span><span class="sxs-lookup"><span data-stu-id="c89f1-223">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="c89f1-224">如需在 ASP.NET Core 與 ASP.NET 之間選擇的詳細資訊，請參閱 <xref:fundamentals/choose-between-aspnet-and-aspnetcore>。</span><span class="sxs-lookup"><span data-stu-id="c89f1-224">For more information on choosing between ASP.NET Core and ASP.NET, see <xref:fundamentals/choose-between-aspnet-and-aspnetcore>.</span></span>
