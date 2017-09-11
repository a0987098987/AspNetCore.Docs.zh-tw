---
title: "ASP.NET Core 基本概念"
author: rick-anderson
description: "本文提供了建置 ASP.NET Core 應用程式時需了解之基本概念的高階概觀。"
keywords: "ASP.NET Core, 基本概念, 概觀"
ms.author: riande
manager: wpickett
ms.date: 08/18/2017
ms.topic: get-started-article
ms.assetid: a19b7836-63e4-44e8-8250-50d426dd1070
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/index
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 99fbe0e02be27a0fbbb7ff65bc15713aab58c003
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/25/2017
---
# <a name="aspnet-core-fundamentals-overview"></a><span data-ttu-id="32413-104">ASP.NET Core 基本概念的概觀</span><span class="sxs-lookup"><span data-stu-id="32413-104">ASP.NET Core fundamentals overview</span></span>

<span data-ttu-id="32413-105">ASP.NET Core 應用程式是一種主控台應用程式，可使用其 `Main` 方法建立網頁伺服器：</span><span class="sxs-lookup"><span data-stu-id="32413-105">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="32413-106">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="32413-106">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="32413-107">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]</span><span class="sxs-lookup"><span data-stu-id="32413-107">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]</span></span>

<span data-ttu-id="32413-108">`Main` 方法會叫用 `WebHost.CreateDefaultBuilder`，這會遵循產生器模式來建立 Web 應用程式主機。</span><span class="sxs-lookup"><span data-stu-id="32413-108">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="32413-109">產生器具有定義網頁伺服器 (例如，`UseKestrel`) 和啟動類別 (`UseStartup`) 的方法。</span><span class="sxs-lookup"><span data-stu-id="32413-109">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="32413-110">在上述範例中，會自動配置 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="32413-110">In the preceding example, a [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="32413-111">ASP.NET Core 的 Web 主機將嘗試在 IIS 上執行 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="32413-111">ASP.NET Core's web host will attempt to run on IIS, if it is available.</span></span> <span data-ttu-id="32413-112">其他網頁伺服器 (例如 [HTTP.sys](xref:fundamentals/servers/httpsys)) 則可透過叫用適當的擴充方法來使用。</span><span class="sxs-lookup"><span data-stu-id="32413-112">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="32413-113">`UseStartup` 將於下一節進一步說明。</span><span class="sxs-lookup"><span data-stu-id="32413-113">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="32413-114">`IWebHostBuilder` 是 `WebHost.CreateDefaultBuilder` 叫用的傳回型別，提供了許多選擇性方法。</span><span class="sxs-lookup"><span data-stu-id="32413-114">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="32413-115">其中某些方法包括用來在 HTTP.sys 中裝載應用程式的 `UseHttpSys`，以及用於指定內容根目錄的 `UseContentRoot`。</span><span class="sxs-lookup"><span data-stu-id="32413-115">Some of these methods include `UseHttpSys` for hosting the application in HTTP.sys, and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="32413-116">`Build` 和 `Run` 方法則會建置 `IWebHost` 物件，以裝載應用程式並開始接聽 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="32413-116">The `Build` and `Run` methods build the `IWebHost` object that will host the application and begin listening for HTTP requests.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="32413-117">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="32413-117">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="32413-118">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="32413-118">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]</span></span>

<span data-ttu-id="32413-119">`Main` 方法會使用 `WebHostBuilder`，這會遵循產生器模式來建立 Web 應用程式主機。</span><span class="sxs-lookup"><span data-stu-id="32413-119">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="32413-120">產生器具有定義網頁伺服器 (例如，`UseKestrel`) 和啟動類別 (`UseStartup`) 的方法。</span><span class="sxs-lookup"><span data-stu-id="32413-120">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="32413-121">在上述範例中，會使用 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="32413-121">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="32413-122">其他網頁伺服器 (例如 [WebListener](xref:fundamentals/servers/weblistener)) 則可透過叫用適當的擴充方法來使用。</span><span class="sxs-lookup"><span data-stu-id="32413-122">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="32413-123">`UseStartup` 將於下一節進一步說明。</span><span class="sxs-lookup"><span data-stu-id="32413-123">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="32413-124">`WebHostBuilder` 提供許多選擇性方法，包括用來在 IIS 和 IIS Express 中進行裝載的 `UseIISIntegration`，以及用於指定內容根目錄的 `UseContentRoot`。</span><span class="sxs-lookup"><span data-stu-id="32413-124">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express, and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="32413-125">`Build` 和 `Run` 方法則會建置 `IWebHost` 物件，以裝載應用程式並開始接聽 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="32413-125">The `Build` and `Run` methods build the `IWebHost` object that will host the application and begin listening for HTTP requests.</span></span>

---

## <a name="startup"></a><span data-ttu-id="32413-126">啟動</span><span class="sxs-lookup"><span data-stu-id="32413-126">Startup</span></span>

<span data-ttu-id="32413-127">`WebHostBuilder` 上的 `UseStartup` 方法可為應用程式指定 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="32413-127">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="32413-128">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="32413-128">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="32413-129">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]</span><span class="sxs-lookup"><span data-stu-id="32413-129">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="32413-130">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="32413-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="32413-131">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]</span><span class="sxs-lookup"><span data-stu-id="32413-131">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]</span></span>

---

<span data-ttu-id="32413-132">`Startup` 類別是您用來定義要求處理管線和設定應用程式所需之任何服務的位置。</span><span class="sxs-lookup"><span data-stu-id="32413-132">The `Startup` class is where you define the request handling pipeline and where any services needed by the application are configured.</span></span> <span data-ttu-id="32413-133">`Startup` 必須是公用類別，而且包含下列方法：</span><span class="sxs-lookup"><span data-stu-id="32413-133">The `Startup` class must be public and contain the following methods:</span></span>

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

* <span data-ttu-id="32413-134">`ConfigureServices` 定義應用程式所使用的[服務](#services) (例如 ASP.NET Core MVC、Entity Framework Core、Identity 等)。</span><span class="sxs-lookup"><span data-stu-id="32413-134">`ConfigureServices` defines the [Services](#services) used by your application (such as ASP.NET Core MVC, Entity Framework Core, Identity, etc.).</span></span>

* <span data-ttu-id="32413-135">`Configure` 定義要求管線中的[中介軟體](xref:fundamentals/middleware)。</span><span class="sxs-lookup"><span data-stu-id="32413-135">`Configure` defines the [middleware](xref:fundamentals/middleware) in the request pipeline.</span></span>

<span data-ttu-id="32413-136">如需詳細資訊，請參閱[應用程式啟動](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="32413-136">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="services"></a><span data-ttu-id="32413-137">服務</span><span class="sxs-lookup"><span data-stu-id="32413-137">Services</span></span>

<span data-ttu-id="32413-138">服務是一種可在應用程式中共用使用的元件。</span><span class="sxs-lookup"><span data-stu-id="32413-138">A service is a component that is intended for common consumption in an application.</span></span> <span data-ttu-id="32413-139">服務是透過[相依性插入](xref:fundamentals/dependency-injection) (DI) 提供。</span><span class="sxs-lookup"><span data-stu-id="32413-139">Services are made available through [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="32413-140">ASP.NET Core 包含原生控制反轉 (IoC) 容器，預設支援[建構函式插入](xref:mvc/controllers/dependency-injection#constructor-injection)。</span><span class="sxs-lookup"><span data-stu-id="32413-140">ASP.NET Core includes a native inversion of control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="32413-141">原生容器可取代為您所選擇的容器。</span><span class="sxs-lookup"><span data-stu-id="32413-141">The native container can be replaced with your container of choice.</span></span> <span data-ttu-id="32413-142">除了其鬆散結合的益處之外，DI 還能夠讓服務可以在整個應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="32413-142">In addition to its loose coupling benefit, DI makes services available throughout your application.</span></span> <span data-ttu-id="32413-143">例如，可以在整個應用程式中使用[記錄](xref:fundamentals/logging)。</span><span class="sxs-lookup"><span data-stu-id="32413-143">For example, [logging](xref:fundamentals/logging) is available throughout your application.</span></span>

<span data-ttu-id="32413-144">如需詳細資訊，請參閱[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="32413-144">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="32413-145">中介軟體</span><span class="sxs-lookup"><span data-stu-id="32413-145">Middleware</span></span>

<span data-ttu-id="32413-146">在 ASP.NET Core 中，您可以使用[中介軟體](xref:fundamentals/middleware)來撰寫要求管線。</span><span class="sxs-lookup"><span data-stu-id="32413-146">In ASP.NET Core, you compose your request pipeline using [Middleware](xref:fundamentals/middleware).</span></span> <span data-ttu-id="32413-147">ASP.NET Core 中介軟體會對 `HttpContext` 執行非同步邏輯，然後叫用序列中的下一個中介軟體或直接終止要求。</span><span class="sxs-lookup"><span data-stu-id="32413-147">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="32413-148">透過在 `Configure` 方法中叫用 `UseXYZ` 擴充方法，新增稱為 "XYZ" 的中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="32413-148">A middleware component called "XYZ" is added by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="32413-149">ASP.NET Core 隨附一組豐富的內建中介軟體：</span><span class="sxs-lookup"><span data-stu-id="32413-149">ASP.NET Core comes with a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="32413-150">靜態檔案</span><span class="sxs-lookup"><span data-stu-id="32413-150">Static files</span></span>](xref:fundamentals/static-files)

* [<span data-ttu-id="32413-151">路由傳送</span><span class="sxs-lookup"><span data-stu-id="32413-151">Routing</span></span>](xref:fundamentals/routing)

* [<span data-ttu-id="32413-152">驗證</span><span class="sxs-lookup"><span data-stu-id="32413-152">Authentication</span></span>](xref:security/authentication/index)

<span data-ttu-id="32413-153">您可以搭配使用任何以 [OWIN](http://owin.org) 為基礎的中介軟體與 ASP.NET Core，也可以撰寫您自己的自訂中介軟體。</span><span class="sxs-lookup"><span data-stu-id="32413-153">You can use any [OWIN](http://owin.org)-based middleware with ASP.NET Core, and you can write your own custom middleware.</span></span>

<span data-ttu-id="32413-154">如需詳細資訊，請參閱[中介軟體](xref:fundamentals/middleware)和 [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin)。</span><span class="sxs-lookup"><span data-stu-id="32413-154">For more information, see [Middleware](xref:fundamentals/middleware) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="servers"></a><span data-ttu-id="32413-155">伺服器</span><span class="sxs-lookup"><span data-stu-id="32413-155">Servers</span></span>

<span data-ttu-id="32413-156">裝載模型的 ASP.NET Core 不會直接接聽要求；相反地，它依賴 HTTP 伺服器實作將要求轉送至應用程式。</span><span class="sxs-lookup"><span data-stu-id="32413-156">The ASP.NET Core hosting model does not directly listen for requests; rather, it relies on an HTTP server implementation to forward the request to the application.</span></span> <span data-ttu-id="32413-157">轉送的要求會包裝成一組您可以透過介面存取的功能物件。</span><span class="sxs-lookup"><span data-stu-id="32413-157">The forwarded request is wrapped as a set of feature objects that you can access through interfaces.</span></span> <span data-ttu-id="32413-158">應用程式會將此設定撰寫成 `HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="32413-158">The application composes this set into an `HttpContext`.</span></span> <span data-ttu-id="32413-159">ASP.NET Core 包含一個受管理的跨平台網頁伺服器，稱為 [Kestrel](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="32413-159">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="32413-160">Kestrel 通常會在生產網頁伺服器 (例如 [IIS](https://iis.net) 或 [nginx](http://nginx.org)) 背後執行。</span><span class="sxs-lookup"><span data-stu-id="32413-160">Kestrel is typically run behind a production web server like [IIS](https://iis.net) or [nginx](http://nginx.org).</span></span>

<span data-ttu-id="32413-161">如需詳細資訊，請參閱[伺服器](xref:fundamentals/servers/index)和[裝載](xref:fundamentals/hosting)。</span><span class="sxs-lookup"><span data-stu-id="32413-161">For more information, see [Servers](xref:fundamentals/servers/index) and [Hosting](xref:fundamentals/hosting).</span></span>

## <a name="content-root"></a><span data-ttu-id="32413-162">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="32413-162">Content root</span></span>

<span data-ttu-id="32413-163">內容根目錄是應用程式所使用的任何內容的基底路徑，例如檢視、[Razor 頁面](xref:mvc/razor-pages/index)，以及靜態資產。</span><span class="sxs-lookup"><span data-stu-id="32413-163">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:mvc/razor-pages/index), and static assets.</span></span> <span data-ttu-id="32413-164">根據預設，內容根目錄與裝載應用程式之可執行檔的應用程式基底路徑相同。</span><span class="sxs-lookup"><span data-stu-id="32413-164">By default, the content root is the same as application base path for the executable hosting the application.</span></span> <span data-ttu-id="32413-165">內容根目錄的其他位置則由 `WebHostBuilder` 指定。</span><span class="sxs-lookup"><span data-stu-id="32413-165">An alternative location for content root is specified with `WebHostBuilder`.</span></span>

## <a name="web-root"></a><span data-ttu-id="32413-166">Web 根目錄</span><span class="sxs-lookup"><span data-stu-id="32413-166">Web root</span></span>

<span data-ttu-id="32413-167">應用程式的 Web 根目錄是專案中包含公用、靜態資源 (例如 CSS、JavaScript 和影像檔) 的目錄。</span><span class="sxs-lookup"><span data-stu-id="32413-167">The web root of an application is the directory in the project containing public, static resources like CSS, JavaScript, and image files.</span></span> <span data-ttu-id="32413-168">根據預設，靜態檔案中介軟體只會處理來自 Web 根目錄及其子目錄的檔案。</span><span class="sxs-lookup"><span data-stu-id="32413-168">By default, the static files middleware will only serve files from the web root directory and its sub-directories.</span></span> <span data-ttu-id="32413-169">如需詳細資訊，請參閱[使用靜態檔案](xref:fundamentals/static-files)。</span><span class="sxs-lookup"><span data-stu-id="32413-169">See [working with static files](xref:fundamentals/static-files) for more info.</span></span> <span data-ttu-id="32413-170">Web 根目錄的路徑預設為 */wwwroot*，但是您可以使用 `WebHostBuilder` 指定不同的位置。</span><span class="sxs-lookup"><span data-stu-id="32413-170">The web root path defaults to */wwwroot*, but you can specify a different location using the `WebHostBuilder`.</span></span>

## <a name="configuration"></a><span data-ttu-id="32413-171">組態</span><span class="sxs-lookup"><span data-stu-id="32413-171">Configuration</span></span>

<span data-ttu-id="32413-172">ASP.NET Core 使用新的組態模型來處理簡單的名稱/值對。</span><span class="sxs-lookup"><span data-stu-id="32413-172">ASP.NET Core uses a new configuration model for handling simple name-value pairs.</span></span> <span data-ttu-id="32413-173">新的組態模型不是根據 `System.Configuration` 或 *web.config*；相反地，它會從組態提供者的已排序集合中提取。</span><span class="sxs-lookup"><span data-stu-id="32413-173">The new configuration model is not based on `System.Configuration` or *web.config*; rather, it pulls from an ordered set of configuration providers.</span></span> <span data-ttu-id="32413-174">內建的組態提供者支援各種檔案格式 (XML、JSON、INI) 和環境變數，可啟用以環境為基礎的組態。</span><span class="sxs-lookup"><span data-stu-id="32413-174">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="32413-175">您也可以撰寫您自己的自訂組態提供者。</span><span class="sxs-lookup"><span data-stu-id="32413-175">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="32413-176">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration)。</span><span class="sxs-lookup"><span data-stu-id="32413-176">For more information, see [Configuration](xref:fundamentals/configuration).</span></span>

## <a name="environments"></a><span data-ttu-id="32413-177">環境</span><span class="sxs-lookup"><span data-stu-id="32413-177">Environments</span></span>

<span data-ttu-id="32413-178">如「開發」和「生產」等環境是 ASP.NET Core 中的第一級概念，可以使用環境變數進行設定。</span><span class="sxs-lookup"><span data-stu-id="32413-178">Environments, like "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="32413-179">如需詳細資訊，請參閱[使用多個環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="32413-179">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="32413-180">.NET Core 與 .NET Framework 執行階段</span><span class="sxs-lookup"><span data-stu-id="32413-180">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="32413-181">ASP.NET Core 應用程式可將目標設為 .NET Core 或 .NET Framework 執行階段。</span><span class="sxs-lookup"><span data-stu-id="32413-181">An ASP.NET Core application can target the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="32413-182">如需詳細資訊，[選擇 .NET Core 或 .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)。</span><span class="sxs-lookup"><span data-stu-id="32413-182">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="additional-information"></a><span data-ttu-id="32413-183">其他資訊</span><span class="sxs-lookup"><span data-stu-id="32413-183">Additional information</span></span>

<span data-ttu-id="32413-184">另請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="32413-184">See also the following topics:</span></span>

- [<span data-ttu-id="32413-185">錯誤處理</span><span class="sxs-lookup"><span data-stu-id="32413-185">Error Handling</span></span>](xref:fundamentals/error-handling)
- [<span data-ttu-id="32413-186">檔案提供者</span><span class="sxs-lookup"><span data-stu-id="32413-186">File Providers</span></span>](xref:fundamentals/file-providers)
- [<span data-ttu-id="32413-187">全球化和當地語系化</span><span class="sxs-lookup"><span data-stu-id="32413-187">Globalization and localization</span></span>](xref:fundamentals/localization)
- [<span data-ttu-id="32413-188">記錄</span><span class="sxs-lookup"><span data-stu-id="32413-188">Logging</span></span>](xref:fundamentals/logging)
- [<span data-ttu-id="32413-189">管理應用程式狀態</span><span class="sxs-lookup"><span data-stu-id="32413-189">Managing Application State</span></span>](xref:fundamentals/app-state)