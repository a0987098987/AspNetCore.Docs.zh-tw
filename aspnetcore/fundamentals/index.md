---
title: ASP.NET Core 基本概念
author: rick-anderson
description: 探索 ASP.NET Core 應用程式的基本建置概念。
ms.author: riande
ms.custom: mvc
ms.date: 01/06/2019
uid: fundamentals/index
ms.openlocfilehash: a56beebd796448705c7b84f47699e9739f451419
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099230"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="71646-103">ASP.NET Core 基本概念</span><span class="sxs-lookup"><span data-stu-id="71646-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="71646-104">ASP.NET Core 應用程式是主控台應用程式，可使用其 `Program.Main` 方法建立網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="71646-104">An ASP.NET Core app is a console app that creates a web server in its `Program.Main` method.</span></span> <span data-ttu-id="71646-105">`Main` 方法是應用程式的「受控進入點」：</span><span class="sxs-lookup"><span data-stu-id="71646-105">The `Main` method is the app's *managed entry point*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

<span data-ttu-id="71646-106">.NET Core 主機：</span><span class="sxs-lookup"><span data-stu-id="71646-106">The .NET Core Host:</span></span>

* <span data-ttu-id="71646-107">載入 [.NET Core 執行階段](https://github.com/dotnet/coreclr)。</span><span class="sxs-lookup"><span data-stu-id="71646-107">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="71646-108">使用第一個命令列引數做為包含進入點 (`Main`) 之受控二進位檔案的路徑，並開始執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="71646-108">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="71646-109">`Main` 方法會叫用 [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*)，這會遵循[建立器模式](https://wikipedia.org/wiki/Builder_pattern)來建立 Web 主機。</span><span class="sxs-lookup"><span data-stu-id="71646-109">The `Main` method invokes [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web host.</span></span> <span data-ttu-id="71646-110">建立器具有定義網頁伺服器 (例如 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) 和啟動類別 (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>) 的方法。</span><span class="sxs-lookup"><span data-stu-id="71646-110">The builder has methods that define a web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="71646-111">以前述範例而言，會自動配置 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="71646-111">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="71646-112">若可行，ASP.NET Core 的 Web 主機會嘗試在 [Internet Information Services (IIS)](https://www.iis.net/) 執行。</span><span class="sxs-lookup"><span data-stu-id="71646-112">ASP.NET Core's web host attempts to run on [Internet Information Services (IIS)](https://www.iis.net/), if available.</span></span> <span data-ttu-id="71646-113">其他網頁伺服器 (例如 [HTTP.sys](xref:fundamentals/servers/httpsys)) 則可透過叫用適當的擴充方法來使用。</span><span class="sxs-lookup"><span data-stu-id="71646-113">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="71646-114">`UseStartup`啟動[章節會進一步說明 ](#startup)。</span><span class="sxs-lookup"><span data-stu-id="71646-114">`UseStartup` is explained further in the [Startup](#startup) section.</span></span>

<span data-ttu-id="71646-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> 是 `WebHost.CreateDefaultBuilder` 叫用的傳回型別，提供了許多選擇性方法。</span><span class="sxs-lookup"><span data-stu-id="71646-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="71646-116">其中某些方法包括用來在 HTTP.sys 中裝載應用程式的 `UseHttpSys`，以及用於指定根內容目錄的 <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*>。</span><span class="sxs-lookup"><span data-stu-id="71646-116">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="71646-117"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> 與 <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> 方法則會建置 <xref:Microsoft.AspNetCore.Hosting.IWebHost> 物件，裝載應用程式並開始接聽 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="71646-117">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

<span data-ttu-id="71646-118">.NET Core 主機：</span><span class="sxs-lookup"><span data-stu-id="71646-118">The .NET Core Host:</span></span>

* <span data-ttu-id="71646-119">載入 [.NET Core 執行階段](https://github.com/dotnet/coreclr)。</span><span class="sxs-lookup"><span data-stu-id="71646-119">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="71646-120">使用第一個命令列引數做為包含進入點 (`Main`) 之受控二進位檔案的路徑，並開始執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="71646-120">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="71646-121">`Main` 方法會使用 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>，這會遵循[建立器模式](https://wikipedia.org/wiki/Builder_pattern)來建立 Web 應用程式主機。</span><span class="sxs-lookup"><span data-stu-id="71646-121">The `Main` method uses <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web app host.</span></span> <span data-ttu-id="71646-122">產生器具有定義網頁伺服器 (例如，<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) 和啟動類別 (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>) 的方法。</span><span class="sxs-lookup"><span data-stu-id="71646-122">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="71646-123">在上述範例中，會使用 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="71646-123">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="71646-124">其他網頁伺服器 (例如 [HTTP.sys](xref:fundamentals/servers/httpsys)) 則可透過叫用適當的擴充方法來使用。</span><span class="sxs-lookup"><span data-stu-id="71646-124">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="71646-125">`UseStartup`啟動[章節會進一步說明 ](#startup)。</span><span class="sxs-lookup"><span data-stu-id="71646-125">`UseStartup` is explained further in the [Startup](#startup) section.</span></span>

<span data-ttu-id="71646-126">`WebHostBuilder` 提供許多選擇性方法，包括用來裝載於 IIS 和 IIS Express 中的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*>，以及用於指定根內容目錄的 <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*>。</span><span class="sxs-lookup"><span data-stu-id="71646-126">`WebHostBuilder` provides many optional methods, including <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> for hosting in IIS and IIS Express and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="71646-127"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> 與 <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> 方法則會建置 <xref:Microsoft.AspNetCore.Hosting.IWebHost> 物件，裝載應用程式並開始接聽 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="71646-127">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

## <a name="startup"></a><span data-ttu-id="71646-128">啟動</span><span class="sxs-lookup"><span data-stu-id="71646-128">Startup</span></span>

<span data-ttu-id="71646-129">`WebHostBuilder` 上的 `UseStartup` 方法可為應用程式指定 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="71646-129">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

<span data-ttu-id="71646-130">`Startup` 類別是設定應用程式所需之任何服務及定義要求處理管線的位置。</span><span class="sxs-lookup"><span data-stu-id="71646-130">The `Startup` class is where any services required by the app are configured and the request handling pipeline is defined.</span></span> <span data-ttu-id="71646-131">`Startup` 必須是公用類別，而且通常包含下列方法。</span><span class="sxs-lookup"><span data-stu-id="71646-131">The `Startup` class must be public and usually contains the following methods.</span></span> <span data-ttu-id="71646-132">`Startup.ConfigureServices` 是選擇項。</span><span class="sxs-lookup"><span data-stu-id="71646-132">`Startup.ConfigureServices` is optional.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<span data-ttu-id="71646-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 會定義應用程式所使用的[服務](#dependency-injection-services) (例如 ASP.NET Core MVC、Entity Framework Core、Identity 等)。</span><span class="sxs-lookup"><span data-stu-id="71646-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="71646-134"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> 定義在要求管線中呼叫的[中介軟體](xref:fundamentals/middleware/index)。</span><span class="sxs-lookup"><span data-stu-id="71646-134"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> defines the [middleware](xref:fundamentals/middleware/index) called in the request pipeline.</span></span>

<span data-ttu-id="71646-135">如需詳細資訊，請參閱<xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="71646-135">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="content-root"></a><span data-ttu-id="71646-136">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="71646-136">Content root</span></span>

<span data-ttu-id="71646-137">內容根目錄是應用程式所使用任何內容的基底路徑，例如 [Razor 頁面](xref:razor-pages/index)、MVC 檢視和靜態資產。</span><span class="sxs-lookup"><span data-stu-id="71646-137">The content root is the base path to any content used by the app, such as [Razor Pages](xref:razor-pages/index), MVC views, and static assets.</span></span> <span data-ttu-id="71646-138">根據預設，內容根目錄與裝載應用程式之可執行檔的應用程式基底路徑相同。</span><span class="sxs-lookup"><span data-stu-id="71646-138">By default, the content root is the same location as the app base path for the executable hosting the app.</span></span>

## <a name="web-root-webroot"></a><span data-ttu-id="71646-139">Web 根目錄 (webroot)</span><span class="sxs-lookup"><span data-stu-id="71646-139">Web root (webroot)</span></span>

<span data-ttu-id="71646-140">應用程式的 Web 根目錄是專案中包含公用、靜態資源 (例如 CSS、JavaScript 與影像檔) 的目錄。</span><span class="sxs-lookup"><span data-stu-id="71646-140">The webroot of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="71646-141">根據預設，*wwwroot* 是 webroot。</span><span class="sxs-lookup"><span data-stu-id="71646-141">By default, *wwwroot* is the webroot.</span></span>

<span data-ttu-id="71646-142">針對 Razor (*.cshtml*) 檔案，波狀符號斜線 `~/` 指向 webroot。</span><span class="sxs-lookup"><span data-stu-id="71646-142">For Razor (*.cshtml*) files, the tilde-slash  `~/` points to the webroot.</span></span> <span data-ttu-id="71646-143">開頭為 `~/` 的路徑稱為虛擬路徑。</span><span class="sxs-lookup"><span data-stu-id="71646-143">Paths beginning with `~/` are referred to as virtual paths.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="71646-144">相依性插入 (服務)</span><span class="sxs-lookup"><span data-stu-id="71646-144">Dependency injection (services)</span></span>

<span data-ttu-id="71646-145">「服務」是一種在應用程式中常用的元件。</span><span class="sxs-lookup"><span data-stu-id="71646-145">A *service* is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="71646-146">服務可透過[相依性插入 (DI)](xref:fundamentals/dependency-injection) 提供。</span><span class="sxs-lookup"><span data-stu-id="71646-146">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="71646-147">ASP.NET Core 包含原生逆轉控制 (IoC) 容器，其根據預設支援[建構函式插入](xref:mvc/controllers/dependency-injection#constructor-injection)。</span><span class="sxs-lookup"><span data-stu-id="71646-147">ASP.NET Core includes a native Inversion of Control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="71646-148">您可視需要取代預設容器。</span><span class="sxs-lookup"><span data-stu-id="71646-148">You can replace the default container if you wish.</span></span> <span data-ttu-id="71646-149">DI 除了具有[鬆散結合的優點](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation)之外，還能夠讓整個應用程式皆可使用服務，例如[記錄](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="71646-149">In addition to its [loose coupling benefit](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI makes services, such as [logging](xref:fundamentals/logging/index), available throughout your app.</span></span>

<span data-ttu-id="71646-150">如需詳細資訊，請參閱<xref:fundamentals/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="71646-150">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="71646-151">中介軟體</span><span class="sxs-lookup"><span data-stu-id="71646-151">Middleware</span></span>

<span data-ttu-id="71646-152">在 ASP.NET Core 中，您可以使用[中介軟體](xref:fundamentals/middleware/index)來撰寫要求管線。</span><span class="sxs-lookup"><span data-stu-id="71646-152">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="71646-153">ASP.NET Core 中介軟體會在 `HttpContext` 上執行非同步作業，然後叫用管線中下一個中介軟體或終止要求。</span><span class="sxs-lookup"><span data-stu-id="71646-153">ASP.NET Core middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="71646-154">依照慣例，將稱為 "XYZ" 之中介軟體元件新增至管線的方法是，在 `Configure` 方法中叫用 `UseXYZ` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="71646-154">By convention, a middleware component called "XYZ" is added to the pipeline by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="71646-155">ASP.NET Core 包含一組豐富的內建中介軟體，您也可以自行撰寫自訂中介軟體。</span><span class="sxs-lookup"><span data-stu-id="71646-155">ASP.NET Core includes a rich set of built-in middleware, and you can write your own custom middleware.</span></span> <span data-ttu-id="71646-156">ASP.NET Core 應用程式支援 [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin)，這可讓 Web 應用程式與網頁伺服器分離。</span><span class="sxs-lookup"><span data-stu-id="71646-156">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), which allows web apps to be decoupled from web servers, is supported in ASP.NET Core apps.</span></span>

<span data-ttu-id="71646-157">如需詳細資訊，請參閱 <xref:fundamentals/middleware/index> 與 <xref:fundamentals/owin>。</span><span class="sxs-lookup"><span data-stu-id="71646-157">For more information, see <xref:fundamentals/middleware/index> and <xref:fundamentals/owin>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="71646-158">初始化 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="71646-158">Initiate HTTP requests</span></span>

<span data-ttu-id="71646-159"><xref:System.Net.Http.IHttpClientFactory> 可用來存取 <xref:System.Net.Http.HttpClient> 執行個體以發出 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="71646-159"><xref:System.Net.Http.IHttpClientFactory> is available to access <xref:System.Net.Http.HttpClient> instances to make HTTP requests.</span></span>

<span data-ttu-id="71646-160">如需詳細資訊，請參閱<xref:fundamentals/http-requests>。</span><span class="sxs-lookup"><span data-stu-id="71646-160">For more information, see <xref:fundamentals/http-requests>.</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="71646-161">環境</span><span class="sxs-lookup"><span data-stu-id="71646-161">Environments</span></span>

<span data-ttu-id="71646-162">「開發」與「生產」等環境是 ASP.NET Core 中的第一級概念，並可使用環境變數、設定檔案和命令列引數加以設定。</span><span class="sxs-lookup"><span data-stu-id="71646-162">Environments, such as *Development* and *Production*, are a first-class notion in ASP.NET Core and can be set using an environment variable, settings file, and command-line argument.</span></span>

<span data-ttu-id="71646-163">如需詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="71646-163">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="hosting"></a><span data-ttu-id="71646-164">裝載</span><span class="sxs-lookup"><span data-stu-id="71646-164">Hosting</span></span>

<span data-ttu-id="71646-165">ASP.NET Core 應用程式會設定並啟動*主機*，其負責啟動應用程式以及管理存留期。</span><span class="sxs-lookup"><span data-stu-id="71646-165">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="71646-166">如需詳細資訊，請參閱<xref:fundamentals/host/index>。</span><span class="sxs-lookup"><span data-stu-id="71646-166">For more information, see <xref:fundamentals/host/index>.</span></span>

## <a name="servers"></a><span data-ttu-id="71646-167">伺服器</span><span class="sxs-lookup"><span data-stu-id="71646-167">Servers</span></span>

<span data-ttu-id="71646-168">ASP.NET Core 裝載模型不會直接接聽要求。</span><span class="sxs-lookup"><span data-stu-id="71646-168">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="71646-169">裝載模型需透過 HTTP 伺服器實作，才可將要求轉寄至應用程式。</span><span class="sxs-lookup"><span data-stu-id="71646-169">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="71646-170">Windows</span><span class="sxs-lookup"><span data-stu-id="71646-170">Windows</span></span>](#tab/windows)

<span data-ttu-id="71646-171">ASP.NET Core 隨附下列伺服器實作：</span><span class="sxs-lookup"><span data-stu-id="71646-171">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="71646-172">[Kestrel](xref:fundamentals/servers/kestrel) 伺服器是受控、跨平台的網路伺服器。</span><span class="sxs-lookup"><span data-stu-id="71646-172">[Kestrel](xref:fundamentals/servers/kestrel) server is a managed, cross-platform web server.</span></span> <span data-ttu-id="71646-173">Kestrel 通常會使用 [IIS](https://www.iis.net/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="71646-173">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="71646-174">Kestrel 也可在 ASP.NET Core 2.0 或更新版本中作為直接向網際網路公開的公眾 Edge Server 執行。</span><span class="sxs-lookup"><span data-stu-id="71646-174">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="71646-175">IIS HTTP 伺服器 (`IISHttpServer`) 是 IIS 的[同處理序伺服器](xref:fundamentals/servers/index#in-process-hosting-model)。</span><span class="sxs-lookup"><span data-stu-id="71646-175">IIS HTTP Server (`IISHttpServer`) is an [in-process server](xref:fundamentals/servers/index#in-process-hosting-model) for IIS.</span></span>
* <span data-ttu-id="71646-176">[HTTP.sys](xref:fundamentals/servers/httpsys) 伺服器是 Windows 上的 ASP.NET Core 網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="71646-176">[HTTP.sys](xref:fundamentals/servers/httpsys) server is a web server for ASP.NET Core on Windows.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="71646-177">macOS</span><span class="sxs-lookup"><span data-stu-id="71646-177">macOS</span></span>](#tab/macos)

<span data-ttu-id="71646-178">ASP.NET Core 會使用 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="71646-178">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="71646-179">Kestrel 是受控、跨平台的網路伺服器。</span><span class="sxs-lookup"><span data-stu-id="71646-179">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="71646-180">Kestrel 也可在 ASP.NET Core 2.0 或更新版本中作為直接向網際網路公開的公眾 Edge Server 執行。</span><span class="sxs-lookup"><span data-stu-id="71646-180">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="71646-181">Linux</span><span class="sxs-lookup"><span data-stu-id="71646-181">Linux</span></span>](#tab/linux)

<span data-ttu-id="71646-182">ASP.NET Core 會使用 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="71646-182">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="71646-183">Kestrel 是受控、跨平台的網路伺服器。</span><span class="sxs-lookup"><span data-stu-id="71646-183">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="71646-184">Kestrel 通常會使用 [Nginx](http://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="71646-184">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="71646-185">Kestrel 也可在 ASP.NET Core 2.0 或更新版本中作為直接向網際網路公開的公眾 Edge Server 執行。</span><span class="sxs-lookup"><span data-stu-id="71646-185">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="71646-186">Windows</span><span class="sxs-lookup"><span data-stu-id="71646-186">Windows</span></span>](#tab/windows)

<span data-ttu-id="71646-187">ASP.NET Core 隨附下列伺服器實作：</span><span class="sxs-lookup"><span data-stu-id="71646-187">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="71646-188">[Kestrel](xref:fundamentals/servers/kestrel) 伺服器是受控、跨平台的網路伺服器。</span><span class="sxs-lookup"><span data-stu-id="71646-188">[Kestrel](xref:fundamentals/servers/kestrel) server is a managed, cross-platform web server.</span></span> <span data-ttu-id="71646-189">Kestrel 通常會使用 [IIS](https://www.iis.net/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="71646-189">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="71646-190">Kestrel 也可在 ASP.NET Core 2.0 或更新版本中作為直接向網際網路公開的公眾 Edge Server 執行。</span><span class="sxs-lookup"><span data-stu-id="71646-190">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="71646-191">[HTTP.sys](xref:fundamentals/servers/httpsys) 伺服器是 Windows 上的 ASP.NET Core 網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="71646-191">[HTTP.sys](xref:fundamentals/servers/httpsys) server is a web server for ASP.NET Core on Windows.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="71646-192">macOS</span><span class="sxs-lookup"><span data-stu-id="71646-192">macOS</span></span>](#tab/macos)

<span data-ttu-id="71646-193">ASP.NET Core 會使用 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="71646-193">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="71646-194">Kestrel 是受控、跨平台的網路伺服器。</span><span class="sxs-lookup"><span data-stu-id="71646-194">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="71646-195">Kestrel 也可在 ASP.NET Core 2.0 或更新版本中作為直接向網際網路公開的公眾 Edge Server 執行。</span><span class="sxs-lookup"><span data-stu-id="71646-195">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="71646-196">Linux</span><span class="sxs-lookup"><span data-stu-id="71646-196">Linux</span></span>](#tab/linux)

<span data-ttu-id="71646-197">ASP.NET Core 會使用 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="71646-197">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="71646-198">Kestrel 是受控、跨平台的網路伺服器。</span><span class="sxs-lookup"><span data-stu-id="71646-198">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="71646-199">Kestrel 通常會使用 [Nginx](http://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="71646-199">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="71646-200">Kestrel 也可在 ASP.NET Core 2.0 或更新版本中作為直接向網際網路公開的公眾 Edge Server 執行。</span><span class="sxs-lookup"><span data-stu-id="71646-200">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

---

::: moniker-end

<span data-ttu-id="71646-201">如需詳細資訊，請參閱<xref:fundamentals/servers/index>。</span><span class="sxs-lookup"><span data-stu-id="71646-201">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="71646-202">Configuration</span><span class="sxs-lookup"><span data-stu-id="71646-202">Configuration</span></span>

<span data-ttu-id="71646-203">ASP.NET Core 會使用以成對的名稱/值為基礎的組態模型。</span><span class="sxs-lookup"><span data-stu-id="71646-203">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="71646-204">而非以 <xref:System.Configuration> 或 *web.config* 為基礎的組態模型。組態會從組態提供者經排序的集合中取得設定。</span><span class="sxs-lookup"><span data-stu-id="71646-204">The configuration model isn't based on <xref:System.Configuration> or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="71646-205">內建的組態提供者支援各種檔案格式 (XML、JSON、INI)、環境變數和命令列引數。</span><span class="sxs-lookup"><span data-stu-id="71646-205">The built-in configuration providers support a variety of file formats (XML, JSON, INI), environment variables, and command-line arguments.</span></span> <span data-ttu-id="71646-206">您也可以撰寫您自己的自訂組態提供者。</span><span class="sxs-lookup"><span data-stu-id="71646-206">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="71646-207">如需詳細資訊，請參閱<xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="71646-207">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="logging"></a><span data-ttu-id="71646-208">記錄</span><span class="sxs-lookup"><span data-stu-id="71646-208">Logging</span></span>

<span data-ttu-id="71646-209">ASP.NET Core 支援可搭配各種記錄提供者的記錄 API。</span><span class="sxs-lookup"><span data-stu-id="71646-209">ASP.NET Core supports a Logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="71646-210">內建提供者支援將記錄檔傳送至一或多個目的地。</span><span class="sxs-lookup"><span data-stu-id="71646-210">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="71646-211">可以使用協力廠商記錄架構。</span><span class="sxs-lookup"><span data-stu-id="71646-211">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="71646-212">如需詳細資訊，請參閱<xref:fundamentals/logging/index>。</span><span class="sxs-lookup"><span data-stu-id="71646-212">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="71646-213">錯誤處理</span><span class="sxs-lookup"><span data-stu-id="71646-213">Error handling</span></span>

<span data-ttu-id="71646-214">ASP.NET Core 具有內建案例，可處理應用程式中的錯誤，包括開發人員例外狀況頁面、自訂錯誤頁面、靜態狀態碼頁面，以及啟動例外狀況處理。</span><span class="sxs-lookup"><span data-stu-id="71646-214">ASP.NET Core has built-in scenarios for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="71646-215">如需詳細資訊，請參閱<xref:fundamentals/error-handling>。</span><span class="sxs-lookup"><span data-stu-id="71646-215">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="routing"></a><span data-ttu-id="71646-216">路由</span><span class="sxs-lookup"><span data-stu-id="71646-216">Routing</span></span>

<span data-ttu-id="71646-217">ASP.NET Core 提供用來將應用程式要求路由至路由處理常式的案例。</span><span class="sxs-lookup"><span data-stu-id="71646-217">ASP.NET Core offers scenarios for routing of app requests to route handlers.</span></span>

<span data-ttu-id="71646-218">如需詳細資訊，請參閱<xref:fundamentals/routing>。</span><span class="sxs-lookup"><span data-stu-id="71646-218">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="background-tasks"></a><span data-ttu-id="71646-219">背景工作</span><span class="sxs-lookup"><span data-stu-id="71646-219">Background tasks</span></span>

<span data-ttu-id="71646-220">背景工作會實作為*託管服務*。</span><span class="sxs-lookup"><span data-stu-id="71646-220">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="71646-221">託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="71646-221">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span>

<span data-ttu-id="71646-222">如需詳細資訊，請參閱<xref:fundamentals/host/hosted-services>。</span><span class="sxs-lookup"><span data-stu-id="71646-222">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="71646-223">存取 HttpContext</span><span class="sxs-lookup"><span data-stu-id="71646-223">Access HttpContext</span></span>

<span data-ttu-id="71646-224">在使用 Razor 頁面和 MVC 處理要求時，會自動提供 `HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="71646-224">`HttpContext` is automatically available when processing requests with Razor Pages and MVC.</span></span> <span data-ttu-id="71646-225">在 `HttpContext` 尚無法使用的情況下，您可以透過 <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> 介面及其預設實作 <xref:Microsoft.AspNetCore.Http.HttpContextAccessor> 存取 `HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="71646-225">In circumstances where `HttpContext` isn't readily available, you can access the `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span>

<span data-ttu-id="71646-226">如需詳細資訊，請參閱<xref:fundamentals/httpcontext>。</span><span class="sxs-lookup"><span data-stu-id="71646-226">For more information, see <xref:fundamentals/httpcontext>.</span></span>
