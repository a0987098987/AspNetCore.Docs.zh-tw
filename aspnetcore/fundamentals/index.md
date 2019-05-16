---
title: ASP.NET Core 基本概念
author: rick-anderson
description: 了解建置 ASP.NET Core 應用程式的基本概念。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: fundamentals/index
ms.openlocfilehash: 9c7bc25d813ad17825ef03f5176882993cc2dd63
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610323"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="64fce-103">ASP.NET Core 基本概念</span><span class="sxs-lookup"><span data-stu-id="64fce-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="64fce-104">本文是了解如何開發 ASP.NET Core 應用程式的關鍵主題概觀。</span><span class="sxs-lookup"><span data-stu-id="64fce-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="64fce-105">Startup 類別</span><span class="sxs-lookup"><span data-stu-id="64fce-105">The Startup class</span></span>

<span data-ttu-id="64fce-106">`Startup` 類別是：</span><span class="sxs-lookup"><span data-stu-id="64fce-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="64fce-107">設定任何應用程式所需服務的位置。</span><span class="sxs-lookup"><span data-stu-id="64fce-107">Any services required by the app are configured.</span></span>
* <span data-ttu-id="64fce-108">定義要求處理管線的位置。</span><span class="sxs-lookup"><span data-stu-id="64fce-108">The request handling pipeline is defined.</span></span>

* <span data-ttu-id="64fce-109">設定 (或「註冊」) 服務的程式碼會新增至 `Startup.ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="64fce-109">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="64fce-110">「服務」是應用程式所使用的元件。</span><span class="sxs-lookup"><span data-stu-id="64fce-110">*Services* are components that are used by the app.</span></span> <span data-ttu-id="64fce-111">例如，Entity Framework Core 內容物件便是一項服務。</span><span class="sxs-lookup"><span data-stu-id="64fce-111">For example, an Entity Framework Core context object is a service.</span></span>
* <span data-ttu-id="64fce-112">設定要求處理管線的程式碼會新增至 `Startup.Configure` 方法。</span><span class="sxs-lookup"><span data-stu-id="64fce-112">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span> <span data-ttu-id="64fce-113">管線是由一系列「中介軟體」元件所組成。</span><span class="sxs-lookup"><span data-stu-id="64fce-113">The pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="64fce-114">例如，中介軟體可能會處理靜態檔案的要求，或是將 HTTP 要求重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="64fce-114">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="64fce-115">每個中介軟體會在 `HttpContext` 上執行非同步操作，然後叫用管線中下一個中介軟體或終止要求。</span><span class="sxs-lookup"><span data-stu-id="64fce-115">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="64fce-116">以下是 `Startup` 類別範例：</span><span class="sxs-lookup"><span data-stu-id="64fce-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="64fce-117">如需詳細資訊，請參閱<xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="64fce-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="64fce-118">相依性插入 (服務)</span><span class="sxs-lookup"><span data-stu-id="64fce-118">Dependency injection (services)</span></span>

<span data-ttu-id="64fce-119">ASP.NET Core 具有內建的相依性插入 (DI) 架構，可讓應用程式的類別使用所設定服務。</span><span class="sxs-lookup"><span data-stu-id="64fce-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="64fce-120">取得類別中一項服務執行個體的其中一種方式，便是使用所需類型的參數來建立建構函式。</span><span class="sxs-lookup"><span data-stu-id="64fce-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="64fce-121">參數可以是服務類型或是介面。</span><span class="sxs-lookup"><span data-stu-id="64fce-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="64fce-122">DI 系統會在執行階段提供服務。</span><span class="sxs-lookup"><span data-stu-id="64fce-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="64fce-123">以下是使用 DI 取得 Entity Framework Core 內容物件的類別。</span><span class="sxs-lookup"><span data-stu-id="64fce-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="64fce-124">醒目提示的那一行便是建構函式插入的範例：</span><span class="sxs-lookup"><span data-stu-id="64fce-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="64fce-125">雖然 DI 為內建，但其設計用於讓您插入協力廠商的控制反轉 (IoC) 容器 (若您想要的話)。</span><span class="sxs-lookup"><span data-stu-id="64fce-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="64fce-126">如需詳細資訊，請參閱<xref:fundamentals/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="64fce-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="64fce-127">中介軟體</span><span class="sxs-lookup"><span data-stu-id="64fce-127">Middleware</span></span>

<span data-ttu-id="64fce-128">要求處理管線是以一系列中介軟體元件組成。</span><span class="sxs-lookup"><span data-stu-id="64fce-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="64fce-129">每個元件會在 `HttpContext` 上執行非同步操作，然後叫用管線中下一個中介軟體或終止要求。</span><span class="sxs-lookup"><span data-stu-id="64fce-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="64fce-130">依照慣例，中介軟體元件會透過叫用其 `Startup.Configure` 方法中的 `Use...` 延伸模組方法來新增至管線。</span><span class="sxs-lookup"><span data-stu-id="64fce-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="64fce-131">例如，若要啟用靜態檔案轉譯，請呼叫 `UseStaticFiles`。</span><span class="sxs-lookup"><span data-stu-id="64fce-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="64fce-132">下列範例中醒目提示的程式碼會設定要求處理管線：</span><span class="sxs-lookup"><span data-stu-id="64fce-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="64fce-133">ASP.NET Core 包含一組豐富的內建中介軟體，您也可以撰寫自訂中介軟體。</span><span class="sxs-lookup"><span data-stu-id="64fce-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="64fce-134">如需詳細資訊，請參閱<xref:fundamentals/middleware/index>。</span><span class="sxs-lookup"><span data-stu-id="64fce-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

<a id="host"/>

## <a name="the-host"></a><span data-ttu-id="64fce-135">主機</span><span class="sxs-lookup"><span data-stu-id="64fce-135">The host</span></span>

<span data-ttu-id="64fce-136">ASP.NET Core 應用程式會在啟動時建置一個「主機」。</span><span class="sxs-lookup"><span data-stu-id="64fce-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="64fce-137">主機是封裝所有應用程式資源的物件，例如：</span><span class="sxs-lookup"><span data-stu-id="64fce-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="64fce-138">HTTP 伺服器實作</span><span class="sxs-lookup"><span data-stu-id="64fce-138">An HTTP server implementation</span></span>
* <span data-ttu-id="64fce-139">中介軟體元件</span><span class="sxs-lookup"><span data-stu-id="64fce-139">Middleware components</span></span>
* <span data-ttu-id="64fce-140">記錄</span><span class="sxs-lookup"><span data-stu-id="64fce-140">Logging</span></span>
* <span data-ttu-id="64fce-141">DI</span><span class="sxs-lookup"><span data-stu-id="64fce-141">DI</span></span>
* <span data-ttu-id="64fce-142">Configuration</span><span class="sxs-lookup"><span data-stu-id="64fce-142">Configuration</span></span>

<span data-ttu-id="64fce-143">在單一物件中包含所有應用程式相互依存資源的主要理由便是生命週期管理：控制應用程式的啟動及順利關機。</span><span class="sxs-lookup"><span data-stu-id="64fce-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="64fce-144">建立主機的程式碼位於 `Program.Main` 中，並遵循 [builder pattern](https://wikipedia.org/wiki/Builder_pattern) (產生器模式)。</span><span class="sxs-lookup"><span data-stu-id="64fce-144">The code to create a host is in `Program.Main` and follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern).</span></span> <span data-ttu-id="64fce-145">會呼叫方法來設定每個作為主機一部分的資源。</span><span class="sxs-lookup"><span data-stu-id="64fce-145">Methods are called to configure each resource that is part of the host.</span></span> <span data-ttu-id="64fce-146">此外還會呼叫產生器方法，一起具現化主機物件。</span><span class="sxs-lookup"><span data-stu-id="64fce-146">A builder method is called to pull it all together and instantiate the host object.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="64fce-147">`CreateHostBuilder` 是特殊名稱，建立器方法可藉此向外部元件 (例如 [Entity Framework](/ef/core/)) 表明其身分。</span><span class="sxs-lookup"><span data-stu-id="64fce-147">`CreateHostBuilder` is special name that identifies the builder method to external components, such as [Entity Framework](/ef/core/).</span></span>

<span data-ttu-id="64fce-148">在 ASP.NET Core 3.0 或更新版本中，您可以在 Web 應用程式中使用一般 Web 主機 (`Host` 類別) 或 Web 主機 (`WebHost` 類別)。</span><span class="sxs-lookup"><span data-stu-id="64fce-148">In ASP.NET Core 3.0 or later, Generic Host (`Host` class) or Web Host (`WebHost` class) can be used in a web app.</span></span> <span data-ttu-id="64fce-149">建議您使用一般主機，Web 主機則適用於回溯相容性。</span><span class="sxs-lookup"><span data-stu-id="64fce-149">Generic Host is recommended, and Web Host is available for backwards compatibility.</span></span>

<span data-ttu-id="64fce-150">架構會提供 `CreateDefaultBuilder` 和 `ConfigureWebHostDefaults` 方法來設定具有常用選項的主機，例如下列項目：</span><span class="sxs-lookup"><span data-stu-id="64fce-150">The framework provides the `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods to set up a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="64fce-151">使用 [Kestrel](#servers) 作為網頁伺服器，並啟用 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="64fce-151">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="64fce-152">從 *appsettings.json*、*appsettings.{Environment Name}.json*、環境變數與命令列引數載入設定。</span><span class="sxs-lookup"><span data-stu-id="64fce-152">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="64fce-153">將記錄輸出傳送到主控台及偵錯提供者。</span><span class="sxs-lookup"><span data-stu-id="64fce-153">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="64fce-154">以下是建置主機的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="64fce-154">Here's sample code that builds a host.</span></span> <span data-ttu-id="64fce-155">使用常用選項設定主機的方法已在其中醒目提示：</span><span class="sxs-lookup"><span data-stu-id="64fce-155">The methods that set up the host with commonly used options are highlighted:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs?highlight=9-10)]

<span data-ttu-id="64fce-156">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host> 與 <xref:fundamentals/host/web-host>。</span><span class="sxs-lookup"><span data-stu-id="64fce-156">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="64fce-157">`CreateWebHostBuilder` 是特殊名稱，建立器方法可藉此向外部元件 (例如 [Entity Framework](/ef/core/)) 表明其身分。</span><span class="sxs-lookup"><span data-stu-id="64fce-157">`CreateWebHostBuilder` is special name that identifies the builder method to external components, such as [Entity Framework](/ef/core/).</span></span>

<span data-ttu-id="64fce-158">ASP.NET Core 2.x 會針對 Web 應用程式使用 Web 主機 (`WebHost` 類別)。</span><span class="sxs-lookup"><span data-stu-id="64fce-158">ASP.NET Core 2.x uses Web Host (`WebHost` class) for web apps.</span></span> <span data-ttu-id="64fce-159">架構會提供 `CreateDefaultBuilder` 來設定具有常用選項的主機，例如下列項目：</span><span class="sxs-lookup"><span data-stu-id="64fce-159">The framework provides `CreateDefaultBuilder` to set up a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="64fce-160">使用 [Kestrel](#servers) 作為網頁伺服器，並啟用 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="64fce-160">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="64fce-161">從 *appsettings.json*、*appsettings.{Environment Name}.json*、環境變數與命令列引數載入設定。</span><span class="sxs-lookup"><span data-stu-id="64fce-161">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="64fce-162">將記錄輸出傳送到主控台及偵錯提供者。</span><span class="sxs-lookup"><span data-stu-id="64fce-162">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="64fce-163">以下是建置主機的範例程式碼：</span><span class="sxs-lookup"><span data-stu-id="64fce-163">Here's sample code that builds a host:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs?highlight=9)]

<span data-ttu-id="64fce-164">如需詳細資訊，請參閱<xref:fundamentals/host/web-host>。</span><span class="sxs-lookup"><span data-stu-id="64fce-164">For more information, see <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

### <a name="advanced-host-scenarios"></a><span data-ttu-id="64fce-165">進階主機案例</span><span class="sxs-lookup"><span data-stu-id="64fce-165">Advanced host scenarios</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="64fce-166">一般主機可供任何 .NET Core 應用程式使用 &mdash; 而非僅限 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="64fce-166">Generic Host is available for any .NET Core app to use&mdash;not just ASP.NET Core apps.</span></span> <span data-ttu-id="64fce-167">一般主機 (`Host` 類別) 允許其他類型的應用程式使用交叉剪輯架構延伸模組，例如記錄、DI、設定與應用程式存留期管理。</span><span class="sxs-lookup"><span data-stu-id="64fce-167">Generic Host (`Host` class) allows other types of apps to use cross-cutting framework extensions, such as logging, DI, configuration, and app lifetime management.</span></span> <span data-ttu-id="64fce-168">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host>。</span><span class="sxs-lookup"><span data-stu-id="64fce-168">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="64fce-169">Web 主機設計為包含 HTTP 伺服器實作，但在其他類型的 .NET 應用程式中並不需要這類實作。</span><span class="sxs-lookup"><span data-stu-id="64fce-169">Web Host is designed to include an HTTP server implementation, which isn't required for other kinds of .NET apps.</span></span> <span data-ttu-id="64fce-170">從 ASP.NET Core 2.1 開始，一般主機 (`Host` 類別) 可供任何 .NET Core 應用程式使用 &mdash; 而非僅限 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="64fce-170">Starting in ASP.NET Core 2.1, the Generic Host (`Host` class) is available for any .NET Core app to use&mdash;not just ASP.NET Core apps.</span></span> <span data-ttu-id="64fce-171">一般主機允許其他類型的應用程式使用交叉剪輯架構延伸模組，例如記錄、DI、設定與應用程式存留期管理。</span><span class="sxs-lookup"><span data-stu-id="64fce-171">Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, DI, configuration, and app lifetime management.</span></span> <span data-ttu-id="64fce-172">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host>。</span><span class="sxs-lookup"><span data-stu-id="64fce-172">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

<span data-ttu-id="64fce-173">您也可以使用主機來執行背景工作。</span><span class="sxs-lookup"><span data-stu-id="64fce-173">You can also use the host to run background tasks.</span></span> <span data-ttu-id="64fce-174">如需詳細資訊，請參閱<xref:fundamentals/host/hosted-services>。</span><span class="sxs-lookup"><span data-stu-id="64fce-174">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="64fce-175">伺服器</span><span class="sxs-lookup"><span data-stu-id="64fce-175">Servers</span></span>

<span data-ttu-id="64fce-176">ASP.NET Core 應用程式使用 HTTP 伺服器實作來接聽 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="64fce-176">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="64fce-177">伺服器會把向應用程式發出的要求作為一組[要求功能](xref:fundamentals/request-features)，合併成一個 `HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="64fce-177">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="64fce-178">Windows</span><span class="sxs-lookup"><span data-stu-id="64fce-178">Windows</span></span>](#tab/windows)

<span data-ttu-id="64fce-179">ASP.NET Core 隨附下列伺服器實作：</span><span class="sxs-lookup"><span data-stu-id="64fce-179">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="64fce-180">*Kestrel* 是跨平台的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="64fce-180">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="64fce-181">Kestrel 通常會使用 [IIS](https://www.iis.net/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="64fce-181">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="64fce-182">在 ASP.NET Core 2.0 或更新版本中，Kestrel 可以作為直接向網際網路公開的公眾 Edge Server 執行。</span><span class="sxs-lookup"><span data-stu-id="64fce-182">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="64fce-183">*IIS HTTP 伺服器*則是適用於使用 IIS Windows 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="64fce-183">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="64fce-184">透過此伺服器，ASP.NET Core 應用程式及 IIS 便可以在相同處理序中執行。</span><span class="sxs-lookup"><span data-stu-id="64fce-184">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="64fce-185">*HTTP.sys* 則是適用於並未搭配 IIS 使用 Windows 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="64fce-185">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="64fce-186">macOS</span><span class="sxs-lookup"><span data-stu-id="64fce-186">macOS</span></span>](#tab/macos)

<span data-ttu-id="64fce-187">ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="64fce-187">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="64fce-188">在 ASP.NET Core 2.0 或更新版本中，Kestrel 可以作為直接向網際網路公開的公眾 Edge Server 執行。</span><span class="sxs-lookup"><span data-stu-id="64fce-188">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="64fce-189">Kestrel 通常會使用 [Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="64fce-189">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="64fce-190">Linux</span><span class="sxs-lookup"><span data-stu-id="64fce-190">Linux</span></span>](#tab/linux)

<span data-ttu-id="64fce-191">ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="64fce-191">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="64fce-192">在 ASP.NET Core 2.0 或更新版本中，Kestrel 可以作為直接向網際網路公開的公眾 Edge Server 執行。</span><span class="sxs-lookup"><span data-stu-id="64fce-192">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="64fce-193">Kestrel 通常會使用 [Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="64fce-193">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="64fce-194">Windows</span><span class="sxs-lookup"><span data-stu-id="64fce-194">Windows</span></span>](#tab/windows)

<span data-ttu-id="64fce-195">ASP.NET Core 隨附下列伺服器實作：</span><span class="sxs-lookup"><span data-stu-id="64fce-195">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="64fce-196">*Kestrel* 是跨平台的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="64fce-196">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="64fce-197">Kestrel 通常會使用 [IIS](https://www.iis.net/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="64fce-197">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="64fce-198">在 ASP.NET Core 2.0 或更新版本中，Kestrel 可以作為直接向網際網路公開的公眾 Edge Server 執行。</span><span class="sxs-lookup"><span data-stu-id="64fce-198">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="64fce-199">*HTTP.sys* 則是適用於並未搭配 IIS 使用 Windows 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="64fce-199">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="64fce-200">macOS</span><span class="sxs-lookup"><span data-stu-id="64fce-200">macOS</span></span>](#tab/macos)

<span data-ttu-id="64fce-201">ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="64fce-201">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="64fce-202">在 ASP.NET Core 2.0 或更新版本中，Kestrel 可以作為直接向網際網路公開的公眾 Edge Server 執行。</span><span class="sxs-lookup"><span data-stu-id="64fce-202">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="64fce-203">Kestrel 通常會使用 [Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="64fce-203">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="64fce-204">Linux</span><span class="sxs-lookup"><span data-stu-id="64fce-204">Linux</span></span>](#tab/linux)

<span data-ttu-id="64fce-205">ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="64fce-205">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="64fce-206">在 ASP.NET Core 2.0 或更新版本中，Kestrel 可以作為直接向網際網路公開的公眾 Edge Server 執行。</span><span class="sxs-lookup"><span data-stu-id="64fce-206">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="64fce-207">Kestrel 通常會使用 [Nginx](http://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。</span><span class="sxs-lookup"><span data-stu-id="64fce-207">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

<span data-ttu-id="64fce-208">如需詳細資訊，請參閱<xref:fundamentals/servers/index>。</span><span class="sxs-lookup"><span data-stu-id="64fce-208">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="64fce-209">Configuration</span><span class="sxs-lookup"><span data-stu-id="64fce-209">Configuration</span></span>

<span data-ttu-id="64fce-210">ASP.NET Core 提供組態架構，可從組態提供者的已排序集合中，以成對名稱和數值的形式取得設定。</span><span class="sxs-lookup"><span data-stu-id="64fce-210">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="64fce-211">您可以使用各種來源的內建組態提供者，例如 *.json* 檔案、*.xml* 檔案、環境變數及命令列引數。</span><span class="sxs-lookup"><span data-stu-id="64fce-211">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="64fce-212">您也可以撰寫自訂組態提供者。</span><span class="sxs-lookup"><span data-stu-id="64fce-212">You can also write custom configuration providers.</span></span>

<span data-ttu-id="64fce-213">例如，您可以指定組態來自 *appsettings.json* 和環境變數。</span><span class="sxs-lookup"><span data-stu-id="64fce-213">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="64fce-214">然後當要求 *ConnectionString* 的值時，架構便會先在 *appsettings.json* 檔案中尋找。</span><span class="sxs-lookup"><span data-stu-id="64fce-214">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="64fce-215">若有找到值，但在環境變數中也存在該值，則會優先使用來自環境變數的值。</span><span class="sxs-lookup"><span data-stu-id="64fce-215">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="64fce-216">針對管理保密組態資料 (例如密碼)，ASP.NET Core 提供[祕密管理員工具](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="64fce-216">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="64fce-217">針對生產祕密，我們建議使用 [Azure Key Vault](xref:security/key-vault-configuration)。</span><span class="sxs-lookup"><span data-stu-id="64fce-217">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="64fce-218">如需詳細資訊，請參閱<xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="64fce-218">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="64fce-219">選項</span><span class="sxs-lookup"><span data-stu-id="64fce-219">Options</span></span>

<span data-ttu-id="64fce-220">在可能的情況下，ASP.NET Core 會遵循「選項模式」來儲存及擷取組態值。</span><span class="sxs-lookup"><span data-stu-id="64fce-220">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="64fce-221">選項模式使用類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="64fce-221">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="64fce-222">例如，下列程式碼會設定 WebSocket 選項：</span><span class="sxs-lookup"><span data-stu-id="64fce-222">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="64fce-223">如需詳細資訊，請參閱<xref:fundamentals/configuration/options>。</span><span class="sxs-lookup"><span data-stu-id="64fce-223">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="64fce-224">環境</span><span class="sxs-lookup"><span data-stu-id="64fce-224">Environments</span></span>

<span data-ttu-id="64fce-225">執行環境 (例如「開發」、「預備」及「生產」) 是 ASP.NET Core 中的第一級概念。</span><span class="sxs-lookup"><span data-stu-id="64fce-225">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="64fce-226">您可以透過設定 `ASPNETCORE_ENVIRONMENT` 環境變數，指定應用程式執行的環境。</span><span class="sxs-lookup"><span data-stu-id="64fce-226">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="64fce-227">ASP.NET Core 會在應用程式啟動時讀取環境變數，然後將值儲存在 `IHostingEnvironment` 實作中。</span><span class="sxs-lookup"><span data-stu-id="64fce-227">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="64fce-228">環境物件可在應用程式中的任何位置，透過 DI 使用。</span><span class="sxs-lookup"><span data-stu-id="64fce-228">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="64fce-229">下列 `Startup` 類別的範例程式碼會設定應用程式，只在於「開發」環境中執行時提供詳細的錯誤資訊：</span><span class="sxs-lookup"><span data-stu-id="64fce-229">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="64fce-230">如需詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="64fce-230">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="64fce-231">記錄</span><span class="sxs-lookup"><span data-stu-id="64fce-231">Logging</span></span>

<span data-ttu-id="64fce-232">ASP.NET Core 支援記錄 API，此 API 能與各種內建和第三方記錄提供者搭配使用。</span><span class="sxs-lookup"><span data-stu-id="64fce-232">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="64fce-233">可用的提供者包括如下：</span><span class="sxs-lookup"><span data-stu-id="64fce-233">Available providers include the following:</span></span>

* <span data-ttu-id="64fce-234">主控台</span><span class="sxs-lookup"><span data-stu-id="64fce-234">Console</span></span>
* <span data-ttu-id="64fce-235">偵錯</span><span class="sxs-lookup"><span data-stu-id="64fce-235">Debug</span></span>
* <span data-ttu-id="64fce-236">Windows 上的事件追蹤</span><span class="sxs-lookup"><span data-stu-id="64fce-236">Event Tracing on Windows</span></span>
* <span data-ttu-id="64fce-237">Windows 事件記錄檔</span><span class="sxs-lookup"><span data-stu-id="64fce-237">Windows Event Log</span></span>
* <span data-ttu-id="64fce-238">TraceSource</span><span class="sxs-lookup"><span data-stu-id="64fce-238">TraceSource</span></span>
* <span data-ttu-id="64fce-239">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="64fce-239">Azure App Service</span></span>
* <span data-ttu-id="64fce-240">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="64fce-240">Azure Application Insights</span></span>

<span data-ttu-id="64fce-241">透過從 DI 取得 `ILogger` 物件並呼叫記錄方法，從應用程式程式碼中的任何位置寫入記錄。</span><span class="sxs-lookup"><span data-stu-id="64fce-241">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="64fce-242">以下是使用 `ILogger` 物件的範例程式碼，其中建構函式插入及記錄方法呼叫都已醒目提示。</span><span class="sxs-lookup"><span data-stu-id="64fce-242">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="64fce-243">`ILogger` 介面可讓您將任何數量的欄位傳遞給記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="64fce-243">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="64fce-244">欄位常用於建構訊息字串，但提供者也可以將它們作為個別欄位，傳送至資料存放區。</span><span class="sxs-lookup"><span data-stu-id="64fce-244">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="64fce-245">這項功能可讓記錄提供者實作 [semantic logging (語意記錄)，又稱為 structured logging (結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。</span><span class="sxs-lookup"><span data-stu-id="64fce-245">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="64fce-246">如需詳細資訊，請參閱<xref:fundamentals/logging/index>。</span><span class="sxs-lookup"><span data-stu-id="64fce-246">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="64fce-247">路由</span><span class="sxs-lookup"><span data-stu-id="64fce-247">Routing</span></span>

<span data-ttu-id="64fce-248">「路由」是一種對應到處理常式的 URL 模式。</span><span class="sxs-lookup"><span data-stu-id="64fce-248">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="64fce-249">處理常式通常是 Razor 頁面、MVC 控制器中的動作方法，或是中介軟體。</span><span class="sxs-lookup"><span data-stu-id="64fce-249">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="64fce-250">ASP.NET Core 路由可讓您控制您應用程式使用的 URL。</span><span class="sxs-lookup"><span data-stu-id="64fce-250">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="64fce-251">如需詳細資訊，請參閱<xref:fundamentals/routing>。</span><span class="sxs-lookup"><span data-stu-id="64fce-251">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="64fce-252">錯誤處理</span><span class="sxs-lookup"><span data-stu-id="64fce-252">Error handling</span></span>

<span data-ttu-id="64fce-253">ASP.NET Core 具有處理錯誤的內建功能，例如：</span><span class="sxs-lookup"><span data-stu-id="64fce-253">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="64fce-254">開發人員例外狀況頁面</span><span class="sxs-lookup"><span data-stu-id="64fce-254">A developer exception page</span></span>
* <span data-ttu-id="64fce-255">自訂錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="64fce-255">Custom error pages</span></span>
* <span data-ttu-id="64fce-256">靜態狀態碼頁面</span><span class="sxs-lookup"><span data-stu-id="64fce-256">Static status code pages</span></span>
* <span data-ttu-id="64fce-257">啟動例外狀況處理</span><span class="sxs-lookup"><span data-stu-id="64fce-257">Startup exception handling</span></span>

<span data-ttu-id="64fce-258">如需詳細資訊，請參閱<xref:fundamentals/error-handling>。</span><span class="sxs-lookup"><span data-stu-id="64fce-258">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="64fce-259">發出 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="64fce-259">Make HTTP requests</span></span>

<span data-ttu-id="64fce-260">`IHttpClientFactory` 的實作可用於建立 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="64fce-260">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="64fce-261">Factory：</span><span class="sxs-lookup"><span data-stu-id="64fce-261">The factory:</span></span>

* <span data-ttu-id="64fce-262">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="64fce-262">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="64fce-263">例如，您可以註冊 *github* 並設定用戶端以存取 GitHub。</span><span class="sxs-lookup"><span data-stu-id="64fce-263">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="64fce-264">預設用戶端可以註冊用於其他用途。</span><span class="sxs-lookup"><span data-stu-id="64fce-264">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="64fce-265">支援註冊及多個委派處理常式的鏈結，以用於建置傳出要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="64fce-265">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="64fce-266">此模式與 ASP.NET Core 中的輸入中介軟體管線相似。</span><span class="sxs-lookup"><span data-stu-id="64fce-266">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="64fce-267">模式提供一個機制來管理 HTTP 要求的跨領域關注，包括快取、錯誤處理、序列化和記錄。</span><span class="sxs-lookup"><span data-stu-id="64fce-267">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="64fce-268">與 *Polly* 整合，Polly 是一種熱門的協力廠商程式庫，用於進行暫時性的錯誤處理。</span><span class="sxs-lookup"><span data-stu-id="64fce-268">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="64fce-269">管理基礎 `HttpClientMessageHandler` 執行個體的共用和存留期，以避免在手動管理 `HttpClient` 存留期時，發生的常見 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="64fce-269">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="64fce-270">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="64fce-270">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="64fce-271">如需詳細資訊，請參閱<xref:fundamentals/http-requests>。</span><span class="sxs-lookup"><span data-stu-id="64fce-271">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="64fce-272">內容根目錄</span><span class="sxs-lookup"><span data-stu-id="64fce-272">Content root</span></span>

<span data-ttu-id="64fce-273">內容根是指向任何由應用程式所使用私人內容的基礎路徑，例如其 Razor 檔案。</span><span class="sxs-lookup"><span data-stu-id="64fce-273">The content root is the base path to any private content used by the app, such as its Razor files.</span></span> <span data-ttu-id="64fce-274">根據預設，內容根是裝載應用程式可執行檔的基礎路徑。</span><span class="sxs-lookup"><span data-stu-id="64fce-274">By default, the content root is the base path for the executable hosting the app.</span></span> <span data-ttu-id="64fce-275">您可以在[建置主機](#host)時指定替代位置。</span><span class="sxs-lookup"><span data-stu-id="64fce-275">An alternative location can be specified when [building the host](#host).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="64fce-276">如需詳細資訊，請參閱[內容根](xref:fundamentals/host/generic-host#content-root)。</span><span class="sxs-lookup"><span data-stu-id="64fce-276">For more information, see [Content root](xref:fundamentals/host/generic-host#content-root).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="64fce-277">如需詳細資訊，請參閱[內容根](xref:fundamentals/host/web-host#content-root)。</span><span class="sxs-lookup"><span data-stu-id="64fce-277">For more information, see [Content root](xref:fundamentals/host/web-host#content-root).</span></span>

::: moniker-end

## <a name="web-root"></a><span data-ttu-id="64fce-278">Web 根目錄</span><span class="sxs-lookup"><span data-stu-id="64fce-278">Web root</span></span>

<span data-ttu-id="64fce-279">Web 根目錄 (也稱為 *webroot*) 是公用、靜態資源 (例如 CSS、JavaScript 和影像檔) 的基礎路徑。</span><span class="sxs-lookup"><span data-stu-id="64fce-279">The web root (also known as *webroot*) is the base path to public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="64fce-280">根據預設，靜態檔案中介軟體只會提供來自 Web 根目錄 (及其子目錄) 的檔案。</span><span class="sxs-lookup"><span data-stu-id="64fce-280">The static files middleware will only serve files from the web root directory (and sub-directories) by default.</span></span> <span data-ttu-id="64fce-281">Web 根目錄路徑預設為 *{內容根}/wwwroot*，但您可以在[建置主機](#host)時指定不同的位置。</span><span class="sxs-lookup"><span data-stu-id="64fce-281">The web root path defaults to *{Content Root}/wwwroot*, but a different location can be specified when [building the host](#host).</span></span>

<span data-ttu-id="64fce-282">在 Razor (*.cshtml*) 檔案中，波狀符號與正斜線 `~/` 會指向 Web 根目錄。</span><span class="sxs-lookup"><span data-stu-id="64fce-282">In Razor (*.cshtml*) files, the tilde-slash `~/` points to the web root.</span></span> <span data-ttu-id="64fce-283">開頭為 `~/` 的路徑稱為虛擬路徑。</span><span class="sxs-lookup"><span data-stu-id="64fce-283">Paths beginning with `~/` are referred to as virtual paths.</span></span>

<span data-ttu-id="64fce-284">如需詳細資訊，請參閱<xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="64fce-284">For more information, see <xref:fundamentals/static-files>.</span></span>
