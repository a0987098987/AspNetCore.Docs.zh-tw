---
title: "裝載於 ASP.NET Core |Microsoft 文件"
author: ardalis
description: "在 ASP.NET Core web 主機簡介。"
keywords: "ASP.NET Core web 主機，IWebHost"
ms.author: riande
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 4e45311d-8d56-46e2-b99d-6f65b648a277
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0725f3d2c130068094792e5ba9e17481155e4294
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/25/2017
---
# <a name="introduction-to-hosting-in-aspnet-core"></a><span data-ttu-id="e30e6-104">在 ASP.NET Core 中裝載的簡介</span><span class="sxs-lookup"><span data-stu-id="e30e6-104">Introduction to hosting in ASP.NET Core</span></span>

<span data-ttu-id="e30e6-105">由[Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="e30e6-105">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="e30e6-106">若要執行 ASP.NET Core 應用程式，您必須設定並啟動使用主機`WebHostBuilder`。</span><span class="sxs-lookup"><span data-stu-id="e30e6-106">To run an ASP.NET Core app, you need to configure and launch a host using `WebHostBuilder`.</span></span>

## <a name="what-is-a-host"></a><span data-ttu-id="e30e6-107">什麼是主應用程式？</span><span class="sxs-lookup"><span data-stu-id="e30e6-107">What is a Host?</span></span>

<span data-ttu-id="e30e6-108">ASP.NET Core 應用程式需要*主機*要在其中執行。</span><span class="sxs-lookup"><span data-stu-id="e30e6-108">ASP.NET Core apps require a *host* in which to execute.</span></span> <span data-ttu-id="e30e6-109">主機必須實作`IWebHost`介面，以便公開的功能和服務的集合，而`Start`方法。</span><span class="sxs-lookup"><span data-stu-id="e30e6-109">A host must implement the `IWebHost` interface, which exposes collections of features and services, and a `Start` method.</span></span> <span data-ttu-id="e30e6-110">主機通常會建立使用的執行個體`WebHostBuilder`，會建立並傳回`WebHost`執行個體。</span><span class="sxs-lookup"><span data-stu-id="e30e6-110">The host is typically created using an instance of a `WebHostBuilder`, which builds and returns a  `WebHost` instance.</span></span> <span data-ttu-id="e30e6-111">`WebHost`參照的伺服器將會處理要求。</span><span class="sxs-lookup"><span data-stu-id="e30e6-111">The `WebHost` references the server that will handle requests.</span></span> <span data-ttu-id="e30e6-112">深入了解[伺服器](servers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="e30e6-112">Learn more about [servers](servers/index.md).</span></span>

### <a name="what-is-the-difference-between-a-host-and-a-server"></a><span data-ttu-id="e30e6-113">在主機與伺服器之間的差異為何？</span><span class="sxs-lookup"><span data-stu-id="e30e6-113">What is the difference between a host and a server?</span></span>

<span data-ttu-id="e30e6-114">負責應用程式啟動和生命週期管理的主機。</span><span class="sxs-lookup"><span data-stu-id="e30e6-114">The host is responsible for application startup and lifetime management.</span></span> <span data-ttu-id="e30e6-115">伺服器是負責接受 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="e30e6-115">The server is responsible for accepting HTTP requests.</span></span> <span data-ttu-id="e30e6-116">主機負責的部分包括確保應用程式的服務和伺服器可供使用且已正確設定。</span><span class="sxs-lookup"><span data-stu-id="e30e6-116">Part of the host's responsibility includes ensuring the application's services and the server are available and properly configured.</span></span> <span data-ttu-id="e30e6-117">您可以將主機視為伺服器的包裝函式。</span><span class="sxs-lookup"><span data-stu-id="e30e6-117">You can think of the host as being a wrapper around the server.</span></span> <span data-ttu-id="e30e6-118">主機已設定為使用特定的伺服器。伺服器不會知道其主機。</span><span class="sxs-lookup"><span data-stu-id="e30e6-118">The host is configured to use a particular server; the server is unaware of its host.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="e30e6-119">設定主機</span><span class="sxs-lookup"><span data-stu-id="e30e6-119">Setting up a Host</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e30e6-120">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e30e6-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e30e6-121">建立使用的執行個體的主機`WebHostBuilder`。</span><span class="sxs-lookup"><span data-stu-id="e30e6-121">You create a host using an instance of `WebHostBuilder`.</span></span> <span data-ttu-id="e30e6-122">這通常是在您的應用程式進入點： `public static void Main` (在專案範本位於*Program.cs*檔案)。</span><span class="sxs-lookup"><span data-stu-id="e30e6-122">This is typically done in your app's entry point: `public static void Main` (which in the project templates is located in a *Program.cs* file).</span></span> <span data-ttu-id="e30e6-123">一般*Program.cs*，所示，將示範如何使用`WebHostBuilder`建置主應用程式。</span><span class="sxs-lookup"><span data-stu-id="e30e6-123">A typical *Program.cs*, shown below, demonstrates how to use a `WebHostBuilder` to build a host.</span></span>

<span data-ttu-id="e30e6-124">[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=14,15,16,17,18,19,20,21)]</span><span class="sxs-lookup"><span data-stu-id="e30e6-124">[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=14,15,16,17,18,19,20,21)]</span></span>

<span data-ttu-id="e30e6-125">`WebHostBuilder`負責建立主機將啟動載入應用程式伺服器。</span><span class="sxs-lookup"><span data-stu-id="e30e6-125">The `WebHostBuilder` is responsible for creating the host that will bootstrap the server for the app.</span></span> <span data-ttu-id="e30e6-126">`WebHostBuilder`需要您提供伺服器實作`IServer`(`UseKestrel`上述程式碼中)。</span><span class="sxs-lookup"><span data-stu-id="e30e6-126">`WebHostBuilder` requires you provide a server that implements `IServer` (`UseKestrel` in the code above).</span></span> <span data-ttu-id="e30e6-127">`UseKestrel`指定應用程式會使用 Kestrel 伺服器。</span><span class="sxs-lookup"><span data-stu-id="e30e6-127">`UseKestrel` specifies the Kestrel server will be used by the app.</span></span>

<span data-ttu-id="e30e6-128">伺服器的*內容的根*決定搜尋內容的檔案，就像 MVC 檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="e30e6-128">The server's *content root* determines where it searches for content files, like MVC View files.</span></span> <span data-ttu-id="e30e6-129">預設內容的根是從中執行應用程式的資料夾。</span><span class="sxs-lookup"><span data-stu-id="e30e6-129">The default content root is the folder from which the application is run.</span></span>

> [!NOTE]
> <span data-ttu-id="e30e6-130">指定`Directory.GetCurrentDirectory`為內容的根會用於 web 專案的根資料夾為應用程式的內容的根應用程式啟動時從這個資料夾 (例如，呼叫`dotnet run`web 專案資料夾中)。</span><span class="sxs-lookup"><span data-stu-id="e30e6-130">Specifying `Directory.GetCurrentDirectory` as the content root will use the web project's root folder as the app's content root when the app is started from this folder (for example, calling `dotnet run` from the web project folder).</span></span> <span data-ttu-id="e30e6-131">這是使用 Visual Studio 中的預設值和`dotnet new`範本。</span><span class="sxs-lookup"><span data-stu-id="e30e6-131">This is the default used in Visual Studio and `dotnet new` templates.</span></span>

<span data-ttu-id="e30e6-132">若要使用 IIS 做為反向 proxy，呼叫`UseIISIntegration`建置主應用程式的一部分。</span><span class="sxs-lookup"><span data-stu-id="e30e6-132">To use IIS as a reverse proxy, call `UseIISIntegration` as part of building the host.</span></span> 

<span data-ttu-id="e30e6-133">請注意，`UseIISIntegration`不設定*伺服器*、 like`UseKestrel`沒有。</span><span class="sxs-lookup"><span data-stu-id="e30e6-133">Note that `UseIISIntegration` doesn't configure a *server*, like `UseKestrel` does.</span></span> <span data-ttu-id="e30e6-134">若要使用 IIS 與 ASP.NET Core，您必須同時指定`UseKestrel`和`UseIISIntegration`。</span><span class="sxs-lookup"><span data-stu-id="e30e6-134">To use IIS with ASP.NET Core, you must specify both `UseKestrel` and `UseIISIntegration`.</span></span> <span data-ttu-id="e30e6-135">`UseKestrel`建立網頁伺服器及裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="e30e6-135">`UseKestrel` creates the web server and hosts the app.</span></span> <span data-ttu-id="e30e6-136">`UseIISIntegration`檢查 IIS/iis Express 所使用的環境變數，並設定要接聽的連接埠等要使用的標頭的設定。</span><span class="sxs-lookup"><span data-stu-id="e30e6-136">`UseIISIntegration` examines environment variables used by IIS/IISExpress and configures settings such as the port to listen on and the headers to use.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e30e6-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e30e6-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e30e6-138">建立使用的執行個體的主機`WebHostBuilder`。</span><span class="sxs-lookup"><span data-stu-id="e30e6-138">You create a host using an instance of `WebHostBuilder`.</span></span> <span data-ttu-id="e30e6-139">這通常是在您的應用程式進入點： `public static void Main` (在專案範本位於*Program.cs*檔案)。</span><span class="sxs-lookup"><span data-stu-id="e30e6-139">This is typically done in your app's entry point: `public static void Main` (which in the project templates is located in a *Program.cs* file).</span></span> <span data-ttu-id="e30e6-140">一般*Program.cs*，如下所示，呼叫`CreateDefaultbuilder`建置主應用程式：</span><span class="sxs-lookup"><span data-stu-id="e30e6-140">A typical *Program.cs*, shown below, calls `CreateDefaultbuilder` to build a host:</span></span>

<span data-ttu-id="e30e6-141">[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="e30e6-141">[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]</span></span>

<span data-ttu-id="e30e6-142">`CreateDefaultbuilder`建立的執行個體`WebHostBuilder`建置 bootstraps 應用程式伺服器的主機。</span><span class="sxs-lookup"><span data-stu-id="e30e6-142">`CreateDefaultbuilder` creates an instance of `WebHostBuilder` to build the host that bootstraps the server for the app.</span></span> <span data-ttu-id="e30e6-143">主應用程式需要[伺服器實作 IServer](servers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="e30e6-143">The host requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="e30e6-144">內建的伺服器是[Kestrel](servers/kestrel.md)和[HTTP.sys](servers/httpsys.md);`CreateDefaultbuilder`預設都會使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="e30e6-144">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md); `CreateDefaultbuilder` use Kestrel by default.</span></span>

<span data-ttu-id="e30e6-145">`CreateDefaultbuilder`會執行除了與網頁伺服器設定 Kestrel 安裝工作：</span><span class="sxs-lookup"><span data-stu-id="e30e6-145">`CreateDefaultbuilder` performs set-up tasks in addition to configuring Kestrel as the web server:</span></span>

* <span data-ttu-id="e30e6-146">若要設定內容的根`Directory.GetCurrentDirectory`。</span><span class="sxs-lookup"><span data-stu-id="e30e6-146">Sets the content root to `Directory.GetCurrentDirectory`.</span></span>
* <span data-ttu-id="e30e6-147">載入組態：</span><span class="sxs-lookup"><span data-stu-id="e30e6-147">Loads configuration from:</span></span>
  * <span data-ttu-id="e30e6-148">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="e30e6-148">*appsettings.json*</span></span>
  * <span data-ttu-id="e30e6-149">*appsettings。\<EnvironmentName >.json*。</span><span class="sxs-lookup"><span data-stu-id="e30e6-149">*appsettings.\<EnvironmentName>.json*.</span></span>
  * <span data-ttu-id="e30e6-150">在開發環境中執行的應用程式時的使用者密碼</span><span class="sxs-lookup"><span data-stu-id="e30e6-150">user secrets when the app runs in the Development environment</span></span>
  * <span data-ttu-id="e30e6-151">環境變數</span><span class="sxs-lookup"><span data-stu-id="e30e6-151">environment variables</span></span>
  * <span data-ttu-id="e30e6-152">提供的命令列引數</span><span class="sxs-lookup"><span data-stu-id="e30e6-152">supplied command line args</span></span>
* <span data-ttu-id="e30e6-153">使用篩選的記錄組態區段中指定的規則來設定主控台和偵錯輸出的記錄。</span><span class="sxs-lookup"><span data-stu-id="e30e6-153">Configures logging for console and debug output, with filtering rules specified in a Logging configuration section.</span></span>
* <span data-ttu-id="e30e6-154">可讓 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="e30e6-154">Enables IIS integration.</span></span>
* <span data-ttu-id="e30e6-155">在開發環境中執行的應用程式時，請新增 [開發人員的例外狀況] 頁面。</span><span class="sxs-lookup"><span data-stu-id="e30e6-155">Adds the developer exception page when the app runs in the Development environment.</span></span>

<span data-ttu-id="e30e6-156">伺服器的*內容的根*決定搜尋內容的檔案，就像 MVC 檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="e30e6-156">The server's *content root* determines where it searches for content files, like MVC View files.</span></span> <span data-ttu-id="e30e6-157">預設內容的根是從中執行應用程式的資料夾。</span><span class="sxs-lookup"><span data-stu-id="e30e6-157">The default content root is the folder from which the application is run.</span></span>

> [!NOTE]
> <span data-ttu-id="e30e6-158">指定`Directory.GetCurrentDirectory`為內容的根會用於 web 專案的根資料夾為應用程式的內容的根應用程式啟動時從這個資料夾 (例如，呼叫`dotnet run`web 專案資料夾中)。</span><span class="sxs-lookup"><span data-stu-id="e30e6-158">Specifying `Directory.GetCurrentDirectory` as the content root will use the web project's root folder as the app's content root when the app is started from this folder (for example, calling `dotnet run` from the web project folder).</span></span> <span data-ttu-id="e30e6-159">這是使用 Visual Studio 中的預設值和`dotnet new`範本。</span><span class="sxs-lookup"><span data-stu-id="e30e6-159">This is the default used in Visual Studio and `dotnet new` templates.</span></span>

<span data-ttu-id="e30e6-160">當您使用 IIS 的反向 proxy 時，ASP.NET Core 自動呼叫`UseIISIntegration`建置主應用程式的一部分。</span><span class="sxs-lookup"><span data-stu-id="e30e6-160">When you use IIS as a reverse proxy, ASP.NET Core automatically calls `UseIISIntegration` as part of building the host.</span></span> <span data-ttu-id="e30e6-161">如需詳細資訊，請參閱[ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="e30e6-161">For more information, see [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>

<span data-ttu-id="e30e6-162">請注意，`UseIISIntegration`不設定*伺服器*、 like`UseKestrel`沒有。</span><span class="sxs-lookup"><span data-stu-id="e30e6-162">Note that `UseIISIntegration` doesn't configure a *server*, like `UseKestrel` does.</span></span> <span data-ttu-id="e30e6-163">`UseKestrel`建立網頁伺服器及裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="e30e6-163">`UseKestrel` creates the web server and hosts the app.</span></span> <span data-ttu-id="e30e6-164">`UseIISIntegration`檢查 IIS/iis Express 所使用的環境變數，並設定要接聽的連接埠等要使用的標頭的設定。</span><span class="sxs-lookup"><span data-stu-id="e30e6-164">`UseIISIntegration` examines environment variables used by IIS/IISExpress and configures settings such as the port to listen on and the headers to use.</span></span>

---

<span data-ttu-id="e30e6-165">設定主機 （和 ASP.NET Core 應用程式） 的最簡單的實作會包含只伺服器和應用程式的要求管線的組態：</span><span class="sxs-lookup"><span data-stu-id="e30e6-165">A minimal implementation of configuring a host (and an ASP.NET Core app) would include just a server and configuration of the app's request pipeline:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
    })
    .Build();

host.Run();
```

> [!NOTE]
> <span data-ttu-id="e30e6-166">當設定主機，您可以提供`Configure`和`ConfigureServices`方法，而不是，或除了指定`Startup`類別 (其中也必須定義這些方法中-請參閱[應用程式啟動](startup.md))。</span><span class="sxs-lookup"><span data-stu-id="e30e6-166">When setting up a host, you can provide `Configure` and `ConfigureServices` methods, instead of or in addition to specifying a `Startup` class (which must also define these methods - see [Application Startup](startup.md)).</span></span> <span data-ttu-id="e30e6-167">多個呼叫`ConfigureServices`會將附加至另一個，則為呼叫`Configure`或`UseStartup`將會取代先前的設定。</span><span class="sxs-lookup"><span data-stu-id="e30e6-167">Multiple calls to `ConfigureServices` will append to one another; calls to `Configure` or `UseStartup` will replace previous settings.</span></span>

## <a name="configuring-a-host"></a><span data-ttu-id="e30e6-168">主控件設定</span><span class="sxs-lookup"><span data-stu-id="e30e6-168">Configuring a Host</span></span>

<span data-ttu-id="e30e6-169">`WebHostBuilder`提供方法來設定大部分可用的設定值，這也可以設定直接使用主機`UseSetting`和相關聯的金鑰。</span><span class="sxs-lookup"><span data-stu-id="e30e6-169">The `WebHostBuilder` provides methods for setting most of the available configuration values for the host, which can also be set directly using `UseSetting` and associated key.</span></span>

### <a name="host-configuration-values"></a><span data-ttu-id="e30e6-170">主機組態值</span><span class="sxs-lookup"><span data-stu-id="e30e6-170">Host Configuration Values</span></span>

<span data-ttu-id="e30e6-171">**擷取啟動錯誤**`bool`</span><span class="sxs-lookup"><span data-stu-id="e30e6-171">**Capture Startup Errors** `bool`</span></span>

<span data-ttu-id="e30e6-172">索引鍵： `captureStartupErrors`。</span><span class="sxs-lookup"><span data-stu-id="e30e6-172">Key: `captureStartupErrors`.</span></span> <span data-ttu-id="e30e6-173">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="e30e6-173">Defaults to `false`.</span></span> <span data-ttu-id="e30e6-174">當`false`，啟動導致主機結束期間發生的錯誤。</span><span class="sxs-lookup"><span data-stu-id="e30e6-174">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="e30e6-175">當`true`，主機會擷取任何例外狀況，從`Startup`類別，並嘗試啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="e30e6-175">When `true`, the host will capture any exceptions from the `Startup` class and attempt to start the server.</span></span> <span data-ttu-id="e30e6-176">它會顯示錯誤頁面 （泛型，或詳細，根據詳細的錯誤設定下, 面） 為每個要求。</span><span class="sxs-lookup"><span data-stu-id="e30e6-176">It will display an error page (generic, or detailed, based on the Detailed Errors setting, below) for every request.</span></span> <span data-ttu-id="e30e6-177">使用設定`CaptureStartupErrors`方法。</span><span class="sxs-lookup"><span data-stu-id="e30e6-177">Set using the `CaptureStartupErrors` method.</span></span>

<span data-ttu-id="e30e6-178">注意： 您的應用程式執行時的 Kestrel 和 IIS，預設行為是以擷取啟動錯誤。</span><span class="sxs-lookup"><span data-stu-id="e30e6-178">Note: When your app runs with Kestrel and IIS, the default behavior is to capture startup errors.</span></span> 

```csharp
new WebHostBuilder()
    .CaptureStartupErrors(true)
   ```

<span data-ttu-id="e30e6-179">**內容根**`string`</span><span class="sxs-lookup"><span data-stu-id="e30e6-179">**Content Root** `string`</span></span>

<span data-ttu-id="e30e6-180">索引鍵： `contentRoot`。</span><span class="sxs-lookup"><span data-stu-id="e30e6-180">Key: `contentRoot`.</span></span> <span data-ttu-id="e30e6-181">預設應用程式組件 （適用於 Kestrel; 所在的資料夾IIS 預設會使用 web 專案根目錄）。</span><span class="sxs-lookup"><span data-stu-id="e30e6-181">Defaults to the folder where the application assembly resides (for Kestrel; IIS will use the web project root by default).</span></span> <span data-ttu-id="e30e6-182">此設定可決定 ASP.NET Core 會開始搜尋的內容檔案，例如 MVC 檢視。</span><span class="sxs-lookup"><span data-stu-id="e30e6-182">This setting determines where ASP.NET Core will begin searching for content files, such as MVC Views.</span></span> <span data-ttu-id="e30e6-183">也做為基底路徑[Web 根目錄設定](#web-root-setting)。</span><span class="sxs-lookup"><span data-stu-id="e30e6-183">Also used as the base path for the [Web Root Setting](#web-root-setting).</span></span> <span data-ttu-id="e30e6-184">使用設定`UseContentRoot`方法。</span><span class="sxs-lookup"><span data-stu-id="e30e6-184">Set using the `UseContentRoot` method.</span></span> <span data-ttu-id="e30e6-185">路徑必須存在，或主機將無法啟動。</span><span class="sxs-lookup"><span data-stu-id="e30e6-185">Path must exist, or host will fail to start.</span></span>

```csharp
new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
   ```

<span data-ttu-id="e30e6-186">**詳細錯誤**`bool`</span><span class="sxs-lookup"><span data-stu-id="e30e6-186">**Detailed Errors** `bool`</span></span>

<span data-ttu-id="e30e6-187">索引鍵： `detailedErrors`。</span><span class="sxs-lookup"><span data-stu-id="e30e6-187">Key: `detailedErrors`.</span></span> <span data-ttu-id="e30e6-188">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="e30e6-188">Defaults to `false`.</span></span> <span data-ttu-id="e30e6-189">當`true`（或當環境設定為 「 開發 」），應用程式會顯示啟動例外狀況，而不是一般錯誤網頁的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="e30e6-189">When `true` (or when Environment is set to "Development"), the app will display details of startup exceptions, instead of just a generic error page.</span></span> <span data-ttu-id="e30e6-190">使用設定`UseSetting`。</span><span class="sxs-lookup"><span data-stu-id="e30e6-190">Set using `UseSetting`.</span></span>

```csharp
new WebHostBuilder()
    .UseSetting("detailedErrors", "true")
```

<span data-ttu-id="e30e6-191">當設定詳細的錯誤為`false`擷取啟動錯誤，且`true`，以伺服器的每個要求的回應會顯示一般錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="e30e6-191">When Detailed Errors is set to `false` and Capture Startup Errors is `true`, a generic error page is displayed in response to every request to the server.</span></span>

![一般錯誤頁面](hosting/_static/generic-error-page.png)

<span data-ttu-id="e30e6-193">當設定詳細的錯誤為`true`擷取啟動錯誤，且`true`，以伺服器的每個要求的回應會顯示詳細的錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="e30e6-193">When Detailed Errors is set to `true` and Capture Startup Errors is `true`, a detailed error page is displayed in response to every request to the server.</span></span>

![詳細的錯誤頁面](hosting/_static/detailed-error-page.png)

<span data-ttu-id="e30e6-195">**環境**`string`</span><span class="sxs-lookup"><span data-stu-id="e30e6-195">**Environment** `string`</span></span>

<span data-ttu-id="e30e6-196">索引鍵： `environment`。</span><span class="sxs-lookup"><span data-stu-id="e30e6-196">Key: `environment`.</span></span> <span data-ttu-id="e30e6-197">預設為 「 生產 」。</span><span class="sxs-lookup"><span data-stu-id="e30e6-197">Defaults to "Production".</span></span> <span data-ttu-id="e30e6-198">可能會設定為任何值。</span><span class="sxs-lookup"><span data-stu-id="e30e6-198">May be set to any value.</span></span> <span data-ttu-id="e30e6-199">架構定義的值包括 「 開發 」、 「 預備 」 及 「 生產 」。</span><span class="sxs-lookup"><span data-stu-id="e30e6-199">Framework-defined values include "Development", "Staging", and "Production".</span></span> <span data-ttu-id="e30e6-200">值不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="e30e6-200">Values are not case sensitive.</span></span> <span data-ttu-id="e30e6-201">請參閱[使用多個環境](environments.md)。</span><span class="sxs-lookup"><span data-stu-id="e30e6-201">See [Working with Multiple Environments](environments.md).</span></span> <span data-ttu-id="e30e6-202">使用設定`UseEnvironment`方法。</span><span class="sxs-lookup"><span data-stu-id="e30e6-202">Set using the `UseEnvironment` method.</span></span>

```csharp
new WebHostBuilder()
    .UseEnvironment("Development")
```

> [!NOTE]
> <span data-ttu-id="e30e6-203">根據預設，環境會從讀取`ASPNETCORE_ENVIRONMENT`環境變數。</span><span class="sxs-lookup"><span data-stu-id="e30e6-203">By default, the environment is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="e30e6-204">當使用 Visual Studio，可能會設定環境變數*launchSettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="e30e6-204">When using Visual Studio, environment variables may be set in the *launchSettings.json* file.</span></span>

<a id="server-urls"></a>

<span data-ttu-id="e30e6-205">**伺服器 Url**`string`</span><span class="sxs-lookup"><span data-stu-id="e30e6-205">**Server URLs** `string`</span></span>

<span data-ttu-id="e30e6-206">索引鍵： `urls`。</span><span class="sxs-lookup"><span data-stu-id="e30e6-206">Key: `urls`.</span></span> <span data-ttu-id="e30e6-207">設定為分號 （;） 分隔清單的 URL 前置詞應該回應伺服器。</span><span class="sxs-lookup"><span data-stu-id="e30e6-207">Set to a semicolon (;) separated list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="e30e6-208">例如，`http://localhost:123`。</span><span class="sxs-lookup"><span data-stu-id="e30e6-208">For example, `http://localhost:123`.</span></span> <span data-ttu-id="e30e6-209">網域/主機名稱可以取代"\*"，表示伺服器應接聽任何 IP 位址的要求，或裝載使用指定的連接埠和通訊協定 (例如，`http://*:5000`或`https://*:5001`)。</span><span class="sxs-lookup"><span data-stu-id="e30e6-209">The domain/host name can be replaced with "\*" to indicate the server should listen to requests on any IP address or host using the specified port and protocol (for example, `http://*:5000` or `https://*:5001`).</span></span> <span data-ttu-id="e30e6-210">通訊協定 (`http://`或`https://`) 必須包含以每個 URL。</span><span class="sxs-lookup"><span data-stu-id="e30e6-210">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="e30e6-211">前置詞解譯所設定的伺服器。支援的格式而異的伺服器之間。</span><span class="sxs-lookup"><span data-stu-id="e30e6-211">The prefixes are interpreted by the configured server; supported formats will vary between servers.</span></span>

```csharp
new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="e30e6-212">在 ASP.NET Core 2.0 中，Kestrel 有它自己的端點設定應用程式開發介面，而且不支援`https://`中`urls`字串。</span><span class="sxs-lookup"><span data-stu-id="e30e6-212">In ASP.NET Core 2.0, Kestrel has its own endpoint configuration API and does not support `https://` in the `urls` string.</span></span> <span data-ttu-id="e30e6-213">如需詳細資訊，請參閱[簡介 Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)。</span><span class="sxs-lookup"><span data-stu-id="e30e6-213">For more information, see [Introduction to Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

<span data-ttu-id="e30e6-214">**啟動組件**`string`</span><span class="sxs-lookup"><span data-stu-id="e30e6-214">**Startup Assembly** `string`</span></span>

<span data-ttu-id="e30e6-215">索引鍵： `startupAssembly`。</span><span class="sxs-lookup"><span data-stu-id="e30e6-215">Key: `startupAssembly`.</span></span> <span data-ttu-id="e30e6-216">決定要搜尋的組件`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="e30e6-216">Determines the assembly to search for the `Startup` class.</span></span> <span data-ttu-id="e30e6-217">使用設定`UseStartup`方法。</span><span class="sxs-lookup"><span data-stu-id="e30e6-217">Set using the `UseStartup` method.</span></span> <span data-ttu-id="e30e6-218">可能會改為參照特定的型別使用`WebHostBuilder.UseStartup<StartupType>`。</span><span class="sxs-lookup"><span data-stu-id="e30e6-218">May instead reference specific type using `WebHostBuilder.UseStartup<StartupType>`.</span></span> <span data-ttu-id="e30e6-219">若為多個`UseStartup`呼叫的方法，最後一個的優先順序。</span><span class="sxs-lookup"><span data-stu-id="e30e6-219">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

<a name=web-root-setting></a>

<span data-ttu-id="e30e6-220">**Web 根目錄**`string`</span><span class="sxs-lookup"><span data-stu-id="e30e6-220">**Web Root** `string`</span></span>

<span data-ttu-id="e30e6-221">索引鍵： `webroot`。</span><span class="sxs-lookup"><span data-stu-id="e30e6-221">Key: `webroot`.</span></span> <span data-ttu-id="e30e6-222">如果未指定預設值是`(Content Root Path)\wwwroot`，如果它存在。</span><span class="sxs-lookup"><span data-stu-id="e30e6-222">If not specified the default is `(Content Root Path)\wwwroot`, if it exists.</span></span> <span data-ttu-id="e30e6-223">如果此路徑不存在，則會使用任何作業檔案提供者。</span><span class="sxs-lookup"><span data-stu-id="e30e6-223">If this path doesn't exist, then a no-op file provider is used.</span></span> <span data-ttu-id="e30e6-224">使用設定`UseWebRoot`。</span><span class="sxs-lookup"><span data-stu-id="e30e6-224">Set using `UseWebRoot`.</span></span>

```csharp
new WebHostBuilder()
    .UseWebRoot("public")
```

### <a name="overriding-configuration"></a><span data-ttu-id="e30e6-225">覆寫設定</span><span class="sxs-lookup"><span data-stu-id="e30e6-225">Overriding Configuration</span></span>

<span data-ttu-id="e30e6-226">使用[組態](configuration.md)設定主應用程式所使用的組態值。</span><span class="sxs-lookup"><span data-stu-id="e30e6-226">Use [Configuration](configuration.md) to set configuration values to be used by the host.</span></span> <span data-ttu-id="e30e6-227">這些值可能會隨後覆寫。</span><span class="sxs-lookup"><span data-stu-id="e30e6-227">These values may be subsequently overridden.</span></span> <span data-ttu-id="e30e6-228">這指定使用`UseConfiguration`。</span><span class="sxs-lookup"><span data-stu-id="e30e6-228">This is specified using `UseConfiguration`.</span></span>

```csharp
public static void Main(string[] args)
{
    var config = new ConfigurationBuilder()
        .AddJsonFile("hosting.json", optional: true)
        .AddCommandLine(args)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .Configure(app =>
        {
            app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
        })
        .Build();

    host.Run();
}
```

<span data-ttu-id="e30e6-229">在上述範例中，命令列引數可能會傳入設定主控件，或設定選擇性地指定在*hosting.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="e30e6-229">In the example above, command-line arguments may be passed in to configure the host, or configuration settings may optionally be specified in a *hosting.json* file.</span></span> <span data-ttu-id="e30e6-230">若要指定特定 URL 上所執行的主機，您可以傳入所要的值從命令提示字元：</span><span class="sxs-lookup"><span data-stu-id="e30e6-230">To specify the host run on a particular URL, you could pass in the desired value from a command prompt:</span></span>

```console
dotnet run --urls "http://*:5000"
```

<span data-ttu-id="e30e6-231">`Run`方法會啟動 web 應用程式，並且封鎖呼叫執行緒，直到主機已關閉。</span><span class="sxs-lookup"><span data-stu-id="e30e6-231">The `Run` method starts the web app and blocks the calling thread until the host is shutdown.</span></span>

```csharp
host.Run();
```

<span data-ttu-id="e30e6-232">您可以藉由呼叫非封鎖方式執行主機其`Start`方法：</span><span class="sxs-lookup"><span data-stu-id="e30e6-232">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="e30e6-233">將 Url 的清單`Start`方法，它會接聽指定的 Url:</span><span class="sxs-lookup"><span data-stu-id="e30e6-233">Pass a list of URLs to the `Start` method and it will listen on the URLs specified:</span></span>

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

<span data-ttu-id="e30e6-234">以下是有效的 URL 格式取決於您要使用的伺服器。</span><span class="sxs-lookup"><span data-stu-id="e30e6-234">The URL formats that are valid here depend on the server you're using.</span></span> <span data-ttu-id="e30e6-235">如需詳細資訊，請參閱[伺服器 Url](#server-urls)稍早在本文章。</span><span class="sxs-lookup"><span data-stu-id="e30e6-235">For more information, see [Server URLs](#server-urls) earlier in this article.</span></span>

> [!NOTE]
> <span data-ttu-id="e30e6-236">`UseConfiguration`擴充方法不是目前可以剖析所傳回的組態區段`GetSection`(例如， `.UseConfiguration(Configuration.GetSection("section"))`。</span><span class="sxs-lookup"><span data-stu-id="e30e6-236">The `UseConfiguration` extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="e30e6-237">`GetSection`方法篩選到要求的區段之組態機碼，但會保留的區段名稱索引鍵 (例如， `section:urls`， `section:environment`)。</span><span class="sxs-lookup"><span data-stu-id="e30e6-237">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="e30e6-238">`UseConfiguration`方法預期要比對的索引鍵`WebHostBuilder`索引鍵 (例如， `urls`， `environment`)。</span><span class="sxs-lookup"><span data-stu-id="e30e6-238">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="e30e6-239">索引鍵的區段名稱存在主控件設定防止區段的值。</span><span class="sxs-lookup"><span data-stu-id="e30e6-239">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="e30e6-240">這個問題將在近期的版本中解決。</span><span class="sxs-lookup"><span data-stu-id="e30e6-240">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="e30e6-241">如需詳細資訊和因應措施，請參閱[傳遞到 WebHostBuilder.UseConfiguration 組態區段會使用完整金鑰](https://github.com/aspnet/Hosting/issues/839)。</span><span class="sxs-lookup"><span data-stu-id="e30e6-241">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="ordering-importance"></a><span data-ttu-id="e30e6-242">排序重要性</span><span class="sxs-lookup"><span data-stu-id="e30e6-242">Ordering Importance</span></span>

<span data-ttu-id="e30e6-243">`WebHostBuilder`如果設定第一次從某些環境變數，讀取設定。</span><span class="sxs-lookup"><span data-stu-id="e30e6-243">`WebHostBuilder` settings are first read from certain environment variables, if set.</span></span> <span data-ttu-id="e30e6-244">這些環境變數都必須使用格式`ASPNETCORE_{configurationKey}`，所以若要設定 Url 的範例則伺服器會接聽預設，會設定`ASPNETCORE_URLS`。</span><span class="sxs-lookup"><span data-stu-id="e30e6-244">These environment variables must use the format `ASPNETCORE_{configurationKey}`, so for example to set the URLs the server will listen on by default, you would set `ASPNETCORE_URLS`.</span></span>

<span data-ttu-id="e30e6-245">您可以指定覆寫這些環境變數值的任何組態 (使用`UseConfiguration`) 或明確地將此值設定 (使用`UseUrls`執行個體)。</span><span class="sxs-lookup"><span data-stu-id="e30e6-245">You can override any of these environment variable values by specifying configuration (using `UseConfiguration`) or by setting the value explicitly (using `UseUrls` for instance).</span></span> <span data-ttu-id="e30e6-246">主機會使用任何選項設定的值上一次。</span><span class="sxs-lookup"><span data-stu-id="e30e6-246">The host will use whichever option sets the value last.</span></span> <span data-ttu-id="e30e6-247">基於這個理由，`UseIISIntegration`必須出現在之後`UseUrls`，因為它會取代 URL 動態 IIS 所提供的其中一個。</span><span class="sxs-lookup"><span data-stu-id="e30e6-247">For this reason, `UseIISIntegration` must appear after `UseUrls`, because it replaces the URL with one dynamically provided by IIS.</span></span> <span data-ttu-id="e30e6-248">如果您想要以程式設計方式設定一個值，預設 URL，但允許它會覆寫的組態，您可以設定主應用程式，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e30e6-248">If you want to programmatically set the default URL to one value, but allow it to be overridden with configuration, you could configure the host as follows:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseUrls("http://*:1000") // default URL
    .UseConfiguration(config) // override from command line
    .UseKestrel()
    .Build();
```

## <a name="additional-resources"></a><span data-ttu-id="e30e6-249">其他資源</span><span class="sxs-lookup"><span data-stu-id="e30e6-249">Additional resources</span></span>

* [<span data-ttu-id="e30e6-250">發行到使用 IIS 的 Windows</span><span class="sxs-lookup"><span data-stu-id="e30e6-250">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="e30e6-251">發行至使用 Nginx Linux</span><span class="sxs-lookup"><span data-stu-id="e30e6-251">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="e30e6-252">若要使用 Apache 的 Linux 發行</span><span class="sxs-lookup"><span data-stu-id="e30e6-252">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="e30e6-253">在 Windows 服務的主機</span><span class="sxs-lookup"><span data-stu-id="e30e6-253">Host in a Windows Service</span></span>](xref:hosting/windows-service)

