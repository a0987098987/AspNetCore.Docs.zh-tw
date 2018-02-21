---
title: "ASP.NET Core 基本概念"
author: rick-anderson
description: "探索用於建置 ASP.NET Core 應用程式的基本概念。"
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: fundamentals/index
ms.openlocfilehash: 85d3eaf033eafbd24c71110ccd7f21ffcc8b0c82
ms.sourcegitcommit: 9f758b1550fcae88ab1eb284798a89e6320548a5
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2018
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="75481-103">ASP.NET Core 基本概念</span><span class="sxs-lookup"><span data-stu-id="75481-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="75481-104">ASP.NET Core 應用程式是一種主控台應用程式，可使用其 `Main` 方法建立網頁伺服器：</span><span class="sxs-lookup"><span data-stu-id="75481-104">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="75481-105">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="75481-105">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

<span data-ttu-id="75481-106">`Main` 方法會叫用 `WebHost.CreateDefaultBuilder`，這會遵循產生器模式來建立 Web 應用程式主機。</span><span class="sxs-lookup"><span data-stu-id="75481-106">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="75481-107">產生器具有定義網頁伺服器 (例如，`UseKestrel`) 和啟動類別 (`UseStartup`) 的方法。</span><span class="sxs-lookup"><span data-stu-id="75481-107">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="75481-108">以前述範例而言，會自動配置 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="75481-108">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="75481-109">ASP.NET Core 的 Web 主機會嘗試在 IIS 上執行 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="75481-109">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="75481-110">其他網頁伺服器 (例如 [HTTP.sys](xref:fundamentals/servers/httpsys)) 則可透過叫用適當的擴充方法來使用。</span><span class="sxs-lookup"><span data-stu-id="75481-110">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="75481-111">`UseStartup` 將於下一節進一步說明。</span><span class="sxs-lookup"><span data-stu-id="75481-111">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="75481-112">`IWebHostBuilder` 是 `WebHost.CreateDefaultBuilder` 叫用的傳回型別，提供了許多選擇性方法。</span><span class="sxs-lookup"><span data-stu-id="75481-112">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="75481-113">其中某些方法包括用來在 HTTP.sys 中裝載應用程式的 `UseHttpSys`，以及用於指定根內容目錄的 `UseContentRoot`。</span><span class="sxs-lookup"><span data-stu-id="75481-113">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="75481-114">`Build` 與 `Run` 方法則會建置 `IWebHost` 物件，裝載應用程式並開始接聽 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="75481-114">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="75481-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="75481-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]

<span data-ttu-id="75481-116">`Main` 方法會使用 `WebHostBuilder`，這會遵循產生器模式來建立 Web 應用程式主機。</span><span class="sxs-lookup"><span data-stu-id="75481-116">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="75481-117">產生器具有定義網頁伺服器 (例如，`UseKestrel`) 和啟動類別 (`UseStartup`) 的方法。</span><span class="sxs-lookup"><span data-stu-id="75481-117">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="75481-118">在上述範例中，會使用 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="75481-118">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="75481-119">其他網頁伺服器 (例如 [WebListener](xref:fundamentals/servers/weblistener)) 則可透過叫用適當的擴充方法來使用。</span><span class="sxs-lookup"><span data-stu-id="75481-119">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="75481-120">`UseStartup` 將於下一節進一步說明。</span><span class="sxs-lookup"><span data-stu-id="75481-120">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="75481-121">`WebHostBuilder` 提供許多選擇性方法，包括用來裝載於 IIS 和 IIS Express 中的 `UseIISIntegration`，以及用於指定根內容目錄的 `UseContentRoot`。</span><span class="sxs-lookup"><span data-stu-id="75481-121">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="75481-122">`Build` 與 `Run` 方法則會建置 `IWebHost` 物件，裝載應用程式並開始接聽 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="75481-122">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

---

## <a name="startup"></a><span data-ttu-id="75481-123">啟動</span><span class="sxs-lookup"><span data-stu-id="75481-123">Startup</span></span>

<span data-ttu-id="75481-124">`WebHostBuilder` 上的 `UseStartup` 方法可為應用程式指定 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="75481-124">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="75481-125">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="75481-125">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="75481-126">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="75481-126">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

<span data-ttu-id="75481-127">`Startup` 類別是您用來定義要求處理管線以及設定應用程式所需之所有服務的位置。</span><span class="sxs-lookup"><span data-stu-id="75481-127">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="75481-128">`Startup` 必須是公用類別，而且包含下列方法：</span><span class="sxs-lookup"><span data-stu-id="75481-128">The `Startup` class must be public and contain the following methods:</span></span>

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method
    // to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method
    // to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

<span data-ttu-id="75481-129">`ConfigureServices` 會定義應用程式所使用的[服務](#dependency-injection-services) (例如 ASP.NET Core MVC、Entity Framework Core、Identity 等)。</span><span class="sxs-lookup"><span data-stu-id="75481-129">`ConfigureServices` defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="75481-130">`Configure` 則會定義要求管線的[中介軟體](xref:fundamentals/middleware/index)。</span><span class="sxs-lookup"><span data-stu-id="75481-130">`Configure` defines the [middleware](xref:fundamentals/middleware/index) for the request pipeline.</span></span>

<span data-ttu-id="75481-131">如需詳細資訊，請參閱[應用程式啟動](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="75481-131">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="content-root"></a><span data-ttu-id="75481-132">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="75481-132">Content root</span></span>

<span data-ttu-id="75481-133">內容根目錄是應用程式所使用的任何內容的基底路徑，例如檢視、[Razor 頁面](xref:mvc/razor-pages/index)，以及靜態資產。</span><span class="sxs-lookup"><span data-stu-id="75481-133">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:mvc/razor-pages/index), and static assets.</span></span> <span data-ttu-id="75481-134">根據預設，內容根目錄會與裝載應用程式之可執行檔的應用程式基底路徑相同。</span><span class="sxs-lookup"><span data-stu-id="75481-134">By default, the content root is the same as application base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="75481-135">Web 根目錄</span><span class="sxs-lookup"><span data-stu-id="75481-135">Web root</span></span>

<span data-ttu-id="75481-136">應用程式的 Web 根目錄是專案中包含公用、靜態資源 (例如 CSS、JavaScript 和影像檔) 的目錄。</span><span class="sxs-lookup"><span data-stu-id="75481-136">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="75481-137">相依性插入 (服務)</span><span class="sxs-lookup"><span data-stu-id="75481-137">Dependency Injection (Services)</span></span>

<span data-ttu-id="75481-138">服務是一種在應用程式中常用的元件。</span><span class="sxs-lookup"><span data-stu-id="75481-138">A service is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="75481-139">服務可透過[相依性插入 (DI)](xref:fundamentals/dependency-injection) 提供。</span><span class="sxs-lookup"><span data-stu-id="75481-139">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="75481-140">ASP.NET Core 包含原生逆轉控制 (IoC) 容器，根據預設，其會支援[建構函式插入](xref:mvc/controllers/dependency-injection#constructor-injection)。</span><span class="sxs-lookup"><span data-stu-id="75481-140">ASP.NET Core includes a native **I**nversion **o**f **C**ontrol (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="75481-141">您可視需要取代掉預設的原生容器。</span><span class="sxs-lookup"><span data-stu-id="75481-141">You can replace the default native container if you wish.</span></span> <span data-ttu-id="75481-142">DI 除了具有鬆散結合的優點之外，還能夠讓整個應用程式皆可使用服務。(例如：[記錄](xref:fundamentals/logging/index))。</span><span class="sxs-lookup"><span data-stu-id="75481-142">In addition to its loose coupling benefit, DI makes services available throughout your app (for example, [logging](xref:fundamentals/logging/index)).</span></span>

<span data-ttu-id="75481-143">如需詳細資訊，請參閱[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="75481-143">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="75481-144">中介軟體</span><span class="sxs-lookup"><span data-stu-id="75481-144">Middleware</span></span>

<span data-ttu-id="75481-145">在 ASP.NET Core 中，您可以使用[中介軟體](xref:fundamentals/middleware/index)來撰寫要求管線。</span><span class="sxs-lookup"><span data-stu-id="75481-145">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="75481-146">ASP.NET Core 中介軟體會對 `HttpContext` 執行非同步邏輯，然後叫用序列中的下一個中介軟體或直接終止要求。</span><span class="sxs-lookup"><span data-stu-id="75481-146">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="75481-147">透過在 `Configure` 方法中叫用 `UseXYZ` 擴充方法，新增稱為 "XYZ" 的中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="75481-147">A middleware component called "XYZ" is added by invoking an `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="75481-148">ASP.NET Core 內含一組豐富的內建中介軟體：</span><span class="sxs-lookup"><span data-stu-id="75481-148">ASP.NET Core includes a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="75481-149">靜態檔案</span><span class="sxs-lookup"><span data-stu-id="75481-149">Static files</span></span>](xref:fundamentals/static-files)
* [<span data-ttu-id="75481-150">路由傳送</span><span class="sxs-lookup"><span data-stu-id="75481-150">Routing</span></span>](xref:fundamentals/routing)
* [<span data-ttu-id="75481-151">驗證</span><span class="sxs-lookup"><span data-stu-id="75481-151">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="75481-152">回應壓縮中介軟體</span><span class="sxs-lookup"><span data-stu-id="75481-152">Response Compression Middleware</span></span>](xref:performance/response-compression)
* [<span data-ttu-id="75481-153">URL 重寫中介軟體</span><span class="sxs-lookup"><span data-stu-id="75481-153">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)

<span data-ttu-id="75481-154">ASP.NET Core 應用程式可使用以 [OWIN](http://owin.org) 為基礎的中介軟體，您也可以自行撰寫自訂的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="75481-154">[OWIN](http://owin.org)-based middleware is available for ASP.NET Core apps, and you can write your own custom middleware.</span></span>

<span data-ttu-id="75481-155">如需詳細資訊，請參閱[中介軟體](xref:fundamentals/middleware/index)和 [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin)。</span><span class="sxs-lookup"><span data-stu-id="75481-155">For more information, see [Middleware](xref:fundamentals/middleware/index) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="environments"></a><span data-ttu-id="75481-156">環境</span><span class="sxs-lookup"><span data-stu-id="75481-156">Environments</span></span>

<span data-ttu-id="75481-157">如「開發」與「生產」等環境是 ASP.NET Core 中的第一級概念，可使用環境變數加以設定。</span><span class="sxs-lookup"><span data-stu-id="75481-157">Environments, such as "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="75481-158">如需詳細資訊，請參閱[使用多個環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="75481-158">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="configuration"></a><span data-ttu-id="75481-159">組態</span><span class="sxs-lookup"><span data-stu-id="75481-159">Configuration</span></span>

<span data-ttu-id="75481-160">ASP.NET Core 會使用以成對的名稱/值為基礎的組態模型。</span><span class="sxs-lookup"><span data-stu-id="75481-160">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="75481-161">而非以 `System.Configuration` 或 *web.config* 為基礎的組態模型。組態會從組態提供者經排序的集合中取得設定。</span><span class="sxs-lookup"><span data-stu-id="75481-161">The configuration model isn't based on `System.Configuration` or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="75481-162">內建的組態提供者支援各種檔案格式 (XML、JSON、INI) 和環境變數，可啟用以環境為基礎的組態。</span><span class="sxs-lookup"><span data-stu-id="75481-162">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="75481-163">您也可以撰寫您自己的自訂組態提供者。</span><span class="sxs-lookup"><span data-stu-id="75481-163">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="75481-164">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="75481-164">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="logging"></a><span data-ttu-id="75481-165">記錄</span><span class="sxs-lookup"><span data-stu-id="75481-165">Logging</span></span>

<span data-ttu-id="75481-166">ASP.NET Core 支援可搭配各種記錄提供者的記錄 API。</span><span class="sxs-lookup"><span data-stu-id="75481-166">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="75481-167">內建提供者支援將記錄檔傳送至一或多個目的地。</span><span class="sxs-lookup"><span data-stu-id="75481-167">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="75481-168">可以使用協力廠商記錄架構。</span><span class="sxs-lookup"><span data-stu-id="75481-168">Third-party logging frameworks can be used.</span></span>

[<span data-ttu-id="75481-169">記錄</span><span class="sxs-lookup"><span data-stu-id="75481-169">Logging</span></span>](xref:fundamentals/logging/index)

## <a name="error-handling"></a><span data-ttu-id="75481-170">錯誤處理</span><span class="sxs-lookup"><span data-stu-id="75481-170">Error handling</span></span>

<span data-ttu-id="75481-171">ASP.NET CoreASP.NET Core 具有內建功能，可處理應用程式中的錯誤，包括開發人員例外狀況頁面、自訂錯誤頁面、靜態狀態字碼頁，以及啟動例外狀況處理。</span><span class="sxs-lookup"><span data-stu-id="75481-171">ASP.NET Core has built-in features for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="75481-172">如需詳細資訊，請參閱[錯誤處理](xref:fundamentals/error-handling)。</span><span class="sxs-lookup"><span data-stu-id="75481-172">For more information, see [Error Handling](xref:fundamentals/error-handling).</span></span>

## <a name="routing"></a><span data-ttu-id="75481-173">路由</span><span class="sxs-lookup"><span data-stu-id="75481-173">Routing</span></span>

<span data-ttu-id="75481-174">ASP.NET Core 提供用來將應用程式要求路由至路由處理常式的功能。</span><span class="sxs-lookup"><span data-stu-id="75481-174">ASP.NET Core offers features for routing of app requests to route handlers.</span></span>

<span data-ttu-id="75481-175">如需詳細資訊，請參閱[路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="75481-175">For more information, see [Routing](xref:fundamentals/routing).</span></span>

## <a name="file-providers"></a><span data-ttu-id="75481-176">檔案提供者</span><span class="sxs-lookup"><span data-stu-id="75481-176">File providers</span></span>

<span data-ttu-id="75481-177">ASP.NET Core 透過使用檔案提供者，將檔案系統存取抽象化，而檔案提供者則提供通用介面，讓您可跨平台使用檔案。</span><span class="sxs-lookup"><span data-stu-id="75481-177">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="75481-178">如需詳細資訊，請參閱[檔案提供者](xref:fundamentals/file-providers)。</span><span class="sxs-lookup"><span data-stu-id="75481-178">For more information, see [File Providers](xref:fundamentals/file-providers).</span></span>

## <a name="static-files"></a><span data-ttu-id="75481-179">靜態檔案</span><span class="sxs-lookup"><span data-stu-id="75481-179">Static files</span></span>

<span data-ttu-id="75481-180">靜態檔案中介軟體負責提供靜態檔案，例如 HTML、CSS、影像和 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="75481-180">Static files middleware serves static files, such as HTML, CSS, image, and JavaScript.</span></span>

<span data-ttu-id="75481-181">如需詳細資訊，請參閱[使用靜態檔案](xref:fundamentals/static-files)。</span><span class="sxs-lookup"><span data-stu-id="75481-181">For more information, see [Working with static files](xref:fundamentals/static-files).</span></span>

## <a name="hosting"></a><span data-ttu-id="75481-182">裝載</span><span class="sxs-lookup"><span data-stu-id="75481-182">Hosting</span></span>

<span data-ttu-id="75481-183">ASP.NET Core 應用程式會設定並啟動*主機*，其負責啟動應用程式以及管理存留期。</span><span class="sxs-lookup"><span data-stu-id="75481-183">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="75481-184">如需詳細資訊，請參閱[裝載](xref:fundamentals/hosting)。</span><span class="sxs-lookup"><span data-stu-id="75481-184">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

## <a name="session-and-application-state"></a><span data-ttu-id="75481-185">工作階段與應用程式狀態</span><span class="sxs-lookup"><span data-stu-id="75481-185">Session and application state</span></span>

<span data-ttu-id="75481-186">在 ASP.NET Core 中的工作階段狀態功能，可用於在使用者瀏覽您的 Web 應用程式時，儲存及存放使用者資料。</span><span class="sxs-lookup"><span data-stu-id="75481-186">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span>

<span data-ttu-id="75481-187">如需詳細資訊，請參閱[工作階段與應用程式狀態](xref:fundamentals/app-state)。</span><span class="sxs-lookup"><span data-stu-id="75481-187">For more information, see [Session and application state](xref:fundamentals/app-state).</span></span>

## <a name="servers"></a><span data-ttu-id="75481-188">伺服器</span><span class="sxs-lookup"><span data-stu-id="75481-188">Servers</span></span>

<span data-ttu-id="75481-189">ASP.NET Core 裝載模型不會直接接聽要求。</span><span class="sxs-lookup"><span data-stu-id="75481-189">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="75481-190">裝載模型需透過 HTTP 伺服器實作，才可將要求轉寄至應用程式。</span><span class="sxs-lookup"><span data-stu-id="75481-190">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="75481-191">轉寄的要求會包裝成一組可透過介面來存取的功能物件。</span><span class="sxs-lookup"><span data-stu-id="75481-191">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="75481-192">ASP.NET Core 包含一個受管理的跨平台網頁伺服器，稱為 [Kestrel](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="75481-192">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="75481-193">Kestrel 通常會在生產 Web 伺服器 (例如 [IIS](https://www.iis.net/) 或 [Nginx](http://nginx.org)) 背後執行。</span><span class="sxs-lookup"><span data-stu-id="75481-193">Kestrel is often run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org).</span></span> <span data-ttu-id="75481-194">Kestrel 可執行為 Edge Server。</span><span class="sxs-lookup"><span data-stu-id="75481-194">Kestrel can be run as an edge server.</span></span>

<span data-ttu-id="75481-195">如需詳細資訊，請參閱[伺服器](xref:fundamentals/servers/index)及下列主題：</span><span class="sxs-lookup"><span data-stu-id="75481-195">For more information, see [Servers](xref:fundamentals/servers/index) and the following topics:</span></span>

* [<span data-ttu-id="75481-196">Kestrel</span><span class="sxs-lookup"><span data-stu-id="75481-196">Kestrel</span></span>](xref:fundamentals/servers/kestrel)
* [<span data-ttu-id="75481-197">ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="75481-197">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* <span data-ttu-id="75481-198">[HTTP.sys](xref:fundamentals/servers/httpsys) (先前稱為 [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="75481-198">[HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="75481-199">全球化和當地語系化</span><span class="sxs-lookup"><span data-stu-id="75481-199">Globalization and localization</span></span>

<span data-ttu-id="75481-200">使用 ASP.NET Core 建立多語系網站，讓更廣大的群眾得以使用您的網站。</span><span class="sxs-lookup"><span data-stu-id="75481-200">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="75481-201">ASP.NET Core 提供服務與中介軟體，可將網站當地語系化成不同的語言與文化特性。</span><span class="sxs-lookup"><span data-stu-id="75481-201">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="75481-202">如需詳細資訊，請參閱[全球化與當地語系化](xref:fundamentals/localization)。</span><span class="sxs-lookup"><span data-stu-id="75481-202">For more information, see [Globalization and localization](xref:fundamentals/localization).</span></span>

## <a name="request-features"></a><span data-ttu-id="75481-203">要求功能</span><span class="sxs-lookup"><span data-stu-id="75481-203">Request features</span></span>

<span data-ttu-id="75481-204">有關 HTTP 要求和回應的網頁伺服器實作詳細資料，定義於介面中。</span><span class="sxs-lookup"><span data-stu-id="75481-204">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="75481-205">伺服器實作與中介軟體會使用這些介面，建立及修改應用程式的裝載管線。</span><span class="sxs-lookup"><span data-stu-id="75481-205">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="75481-206">如需詳細資訊，請參閱[要求功能](xref:fundamentals/request-features)。</span><span class="sxs-lookup"><span data-stu-id="75481-206">For more information, see [Request Features](xref:fundamentals/request-features).</span></span>

## <a name="background-tasks"></a><span data-ttu-id="75481-207">背景工作</span><span class="sxs-lookup"><span data-stu-id="75481-207">Background tasks</span></span>

<span data-ttu-id="75481-208">背景工作會實作為*託管服務*。</span><span class="sxs-lookup"><span data-stu-id="75481-208">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="75481-209">託管服務是具有背景工作邏輯的類別，能夠實作 [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) 介面。</span><span class="sxs-lookup"><span data-stu-id="75481-209">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span>

<span data-ttu-id="75481-210">如需詳細資訊，請參閱[搭配託管服務的背景工作](xref:fundamentals/hosted-services)。</span><span class="sxs-lookup"><span data-stu-id="75481-210">For more information, see [Background tasks with hosted services](xref:fundamentals/hosted-services).</span></span>

## <a name="open-web-interface-for-net-owin"></a><span data-ttu-id="75481-211">Open Web Interface for .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="75481-211">Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="75481-212">ASP.NET Core 支援Open Web Interface for .NET (OWIN)。</span><span class="sxs-lookup"><span data-stu-id="75481-212">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="75481-213">OWIN 可讓 Web 應用程式獨立於網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="75481-213">OWIN allows web apps to be decoupled from web servers.</span></span>

<span data-ttu-id="75481-214">如需詳細資訊，請參閱 [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin)。</span><span class="sxs-lookup"><span data-stu-id="75481-214">For more information, see [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="websockets"></a><span data-ttu-id="75481-215">WebSockets</span><span class="sxs-lookup"><span data-stu-id="75481-215">WebSockets</span></span>

<span data-ttu-id="75481-216">[WebSocket](https://wikipedia.org/wiki/WebSocket) 為通訊協定，其可在 TCP 連線下啟用雙向的持續性通訊通道。</span><span class="sxs-lookup"><span data-stu-id="75481-216">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="75481-217">其可用於像是聊天、股票行情、遊戲等應用程式，以及您希望在 Web 應用程式中使用即時功能的任何位置。</span><span class="sxs-lookup"><span data-stu-id="75481-217">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="75481-218">ASP.NET Core 支援網路通訊端功能。</span><span class="sxs-lookup"><span data-stu-id="75481-218">ASP.NET Core supports web socket features.</span></span>

<span data-ttu-id="75481-219">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="75481-219">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="75481-220">Microsoft.AspNetCore.All 中繼套件</span><span class="sxs-lookup"><span data-stu-id="75481-220">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="75481-221">ASP.NET Core 的 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) 中繼套件包括：</span><span class="sxs-lookup"><span data-stu-id="75481-221">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="75481-222">所有由 ASP.NET Core 小組支援的套件。</span><span class="sxs-lookup"><span data-stu-id="75481-222">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="75481-223">所有由 Entity Framework Core 支援的套件。</span><span class="sxs-lookup"><span data-stu-id="75481-223">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="75481-224">ASP.NET Core 與 Entity Framework Core 所使用的內部與第三人相依性。</span><span class="sxs-lookup"><span data-stu-id="75481-224">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="75481-225">如需詳細資訊，請參閱 [Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)。</span><span class="sxs-lookup"><span data-stu-id="75481-225">For more information, see [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="75481-226">.NET Core 與 .NET Framework 執行階段</span><span class="sxs-lookup"><span data-stu-id="75481-226">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="75481-227">ASP.NET Core 應用程式可將目標設為 .NET Core 或 .NET Framework 執行階段。</span><span class="sxs-lookup"><span data-stu-id="75481-227">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="75481-228">如需詳細資訊，請參閱[在 .NET Core 和 .NET Framework 之間進行選擇](/dotnet/articles/standard/choosing-core-framework-server)。</span><span class="sxs-lookup"><span data-stu-id="75481-228">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="75481-229">在 ASP.NET Core 與 ASP.NET 之間選擇</span><span class="sxs-lookup"><span data-stu-id="75481-229">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="75481-230">如需在 ASP.NET Core 與 ASP.NET 之間選擇的詳細資訊，請參閱[在 ASP.NET Core 與 ASP.NET 之間選擇](xref:fundamentals/choose-between-aspnet-and-aspnetcore)。</span><span class="sxs-lookup"><span data-stu-id="75481-230">For more information on choosing between ASP.NET Core and ASP.NET, see [Choose between ASP.NET Core and ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span></span>
