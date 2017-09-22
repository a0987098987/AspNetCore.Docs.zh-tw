---
title: "ASP.NET 核心模組"
author: tdykstra
description: "導入了 ASP.NET 核心模組 (ANCM)，可讓 Kestrel 網頁伺服器 IIS 或 IIS Express 做為反向 proxy 伺服器的 IIS 模組。"
keywords: "ASP.NET Core，IIS、 IIS Express,ASP.NET 核心模組 UseIISIntegration"
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.assetid: 4661af33-34c5-4d71-93a0-8c7632f43580
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/aspnet-core-module
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 479fc3a389168bb08e278aa3d9da3bf7df1b49f4
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-aspnet-core-module"></a><span data-ttu-id="227c5-104">ASP.NET Core 模組簡介</span><span class="sxs-lookup"><span data-stu-id="227c5-104">Introduction to ASP.NET Core Module</span></span>

<span data-ttu-id="227c5-105">由[Tom Dykstra](https://github.com/tdykstra)， [Rick Strahl](https://github.com/RickStrahl)，和[Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="227c5-105">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span> 

<span data-ttu-id="227c5-106">ASP.NET 核心模組 (ANCM) 可讓您執行 ASP.NET Core 應用程式背後 IIS，它很適合 （安全性、 管理及許多更多） 中使用 IIS，並使用[Kestrel](kestrel.md)如它很適合 （要快速學會），以及取得從一次這兩種技術的優點。</span><span class="sxs-lookup"><span data-stu-id="227c5-106">ASP.NET Core Module (ANCM) lets you run ASP.NET Core applications behind IIS, using IIS for what it's good at (security, manageability, and lots more) and using [Kestrel](kestrel.md) for what it's good at (being really fast), and getting the benefits from both technologies at once.</span></span> <span data-ttu-id="227c5-107">**ANCM 只適用於 Kestrel;它與不相容 WebListener (在 ASP.NET Core 1.x) 或 （在 2.x) HTTP.sys。**</span><span class="sxs-lookup"><span data-stu-id="227c5-107">**ANCM works only with Kestrel; it isn't compatible with WebListener (in ASP.NET Core 1.x) or HTTP.sys (in 2.x).**</span></span> 

<span data-ttu-id="227c5-108">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="227c5-108">Supported Windows versions:</span></span>

* <span data-ttu-id="227c5-109">Windows 7 和 Windows Server 2008 R2 和更新版本</span><span class="sxs-lookup"><span data-stu-id="227c5-109">Windows 7 and Windows Server 2008 R2 and later</span></span>

[<span data-ttu-id="227c5-110">檢視或下載範例程式碼</span><span class="sxs-lookup"><span data-stu-id="227c5-110">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)

## <a name="what-aspnet-core-module-does"></a><span data-ttu-id="227c5-111">ASP.NET 核心模組的功能</span><span class="sxs-lookup"><span data-stu-id="227c5-111">What ASP.NET Core Module does</span></span>

<span data-ttu-id="227c5-112">ANCM 是連結到管線的 IIS，並將流量重新導向到後端 ASP.NET Core 應用程式的原生 IIS 模組。</span><span class="sxs-lookup"><span data-stu-id="227c5-112">ANCM is a native IIS module that hooks into the IIS pipeline and redirects traffic to the backend ASP.NET Core application.</span></span> <span data-ttu-id="227c5-113">大部分其他模組，例如 windows 驗證時，仍有機會執行。</span><span class="sxs-lookup"><span data-stu-id="227c5-113">Most other modules, such as windows authentication, still get a chance to run.</span></span> <span data-ttu-id="227c5-114">ANCM 才會控制當處理常式已選取的要求，且應用程式中定義的處理常式對應*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="227c5-114">ANCM only takes control when a handler is selected for the request, and handler mapping is defined in the application *web.config* file.</span></span>

<span data-ttu-id="227c5-115">因為處理序中執行的 ASP.NET 核心應用程式是由 IIS 工作者處理序分開，ANCM 也沒有處理序管理。</span><span class="sxs-lookup"><span data-stu-id="227c5-115">Because ASP.NET Core applications run in a process separate from the IIS worker process, ANCM also does process management.</span></span> <span data-ttu-id="227c5-116">ANCM 啟動 ASP.NET Core 應用程式的程序，當第一個要求送入，然後損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="227c5-116">ANCM starts the process for the ASP.NET Core application when the first request comes in and restarts it when it crashes.</span></span> <span data-ttu-id="227c5-117">這是傳統的 ASP.NET 應用程式基本上相同的行為執行同處理序在 IIS 和 WAS （Windows 啟用服務） 所管理。</span><span class="sxs-lookup"><span data-stu-id="227c5-117">This is essentially the same behavior as classic ASP.NET applications that run in-process in IIS and are managed by WAS (Windows Activation Service).</span></span>

<span data-ttu-id="227c5-118">以下是說明 IIS、 ANCM 和 ASP.NET Core 應用程式之間的關聯性圖表。</span><span class="sxs-lookup"><span data-stu-id="227c5-118">Here's a diagram that illustrates the relationship between IIS, ANCM, and ASP.NET Core applications.</span></span>

![ASP.NET 核心模組](aspnet-core-module/_static/ancm.png)

<span data-ttu-id="227c5-120">要求來自網站及叫用核心模式 Http.Sys 驅動程式的路由傳送到 IIS 上的主要連接埠 (80) 或 SSL 連接埠 (443)。</span><span class="sxs-lookup"><span data-stu-id="227c5-120">Requests come in from the Web and hit the kernel mode Http.Sys driver which routes them into IIS on the primary port (80) or SSL port (443).</span></span> <span data-ttu-id="227c5-121">ANCM 將要求轉送至 ASP.NET Core 上的應用程式設定應用程式，不是通訊埠 80/443 的 HTTP 連接埠。</span><span class="sxs-lookup"><span data-stu-id="227c5-121">ANCM forwards the requests to the ASP.NET Core application on the HTTP port configured for the application, which is not port 80/443.</span></span>

<span data-ttu-id="227c5-122">Kestrel 會接聽來自 ANCM 流量。</span><span class="sxs-lookup"><span data-stu-id="227c5-122">Kestrel listens for traffic coming from ANCM.</span></span>  <span data-ttu-id="227c5-123">ANCM 指定環境變數在啟動時，透過的連接埠和[UseIISIntegration](#call-useiisintegration)方法會設定伺服器接聽`http://localhost:{port}`。</span><span class="sxs-lookup"><span data-stu-id="227c5-123">ANCM specifies the port via environment variable at startup, and the [UseIISIntegration](#call-useiisintegration) method configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="227c5-124">沒有額外的檢查拒絕不是從 ANCM 的要求。</span><span class="sxs-lookup"><span data-stu-id="227c5-124">There are additional checks to reject requests not from ANCM.</span></span> <span data-ttu-id="227c5-125">（ANCM 不支援 HTTPS 轉送，因此即使透過 HTTPS 接收 IIS 要求都會轉送 over HTTP。）</span><span class="sxs-lookup"><span data-stu-id="227c5-125">(ANCM does not support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.)</span></span>

<span data-ttu-id="227c5-126">Kestrel 拾取 ANCM 要求，並將 ASP.NET Core 中介軟體管線，然後處理它們，並將它們當做傳遞`HttpContext`應用程式邏輯的執行個體。</span><span class="sxs-lookup"><span data-stu-id="227c5-126">Kestrel picks up requests from ANCM and pushes them into the ASP.NET Core middleware pipeline, which then handles them and passes them on as `HttpContext` instances to application logic.</span></span> <span data-ttu-id="227c5-127">應用程式的回應，接著會傳遞至 IIS，它們回起始要求的 HTTP 用戶端的推播通知。</span><span class="sxs-lookup"><span data-stu-id="227c5-127">The application's responses are then passed back to IIS, which pushes them back out to the HTTP client that initiated the requests.</span></span>

<span data-ttu-id="227c5-128">ANCM 有幾個其他函式：</span><span class="sxs-lookup"><span data-stu-id="227c5-128">ANCM has a few other functions as well:</span></span>

* <span data-ttu-id="227c5-129">設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="227c5-129">Sets environment variables.</span></span>
* <span data-ttu-id="227c5-130">記錄檔`stdout`輸出到檔案存放裝置。</span><span class="sxs-lookup"><span data-stu-id="227c5-130">Logs `stdout` output to file storage.</span></span>
* <span data-ttu-id="227c5-131">將轉送 Windows 驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="227c5-131">Forwards Windows authentication tokens.</span></span>

## <a name="how-to-use-ancm-in-aspnet-core-apps"></a><span data-ttu-id="227c5-132">如何在 ASP.NET Core 應用程式中使用 ANCM</span><span class="sxs-lookup"><span data-stu-id="227c5-132">How to use ANCM in ASP.NET Core apps</span></span>

<span data-ttu-id="227c5-133">本節提供設定 IIS 伺服器和 ASP.NET Core 應用程式的程序概觀。</span><span class="sxs-lookup"><span data-stu-id="227c5-133">This section provides an overview of the process for setting up an IIS server and ASP.NET Core application.</span></span> <span data-ttu-id="227c5-134">如需詳細指示，請參閱[發行至 IIS](../../publishing/iis.md)。</span><span class="sxs-lookup"><span data-stu-id="227c5-134">For detailed instructions, see [Publishing to IIS](../../publishing/iis.md).</span></span>

### <a name="install-ancm"></a><span data-ttu-id="227c5-135">安裝 ANCM</span><span class="sxs-lookup"><span data-stu-id="227c5-135">Install ANCM</span></span>

<span data-ttu-id="227c5-136">ASP.NET 核心模組必須安裝在 IIS 伺服器上，並在 IIS Express 開發電腦上。</span><span class="sxs-lookup"><span data-stu-id="227c5-136">The ASP.NET Core Module has to be installed in IIS on your servers and in IIS Express on your development machines.</span></span> <span data-ttu-id="227c5-137">針對伺服器，ANCM 隨附於[.NET 核心 Windows Server 裝載配套](https://aka.ms/dotnetcore.2.0.0-windowshosting)。</span><span class="sxs-lookup"><span data-stu-id="227c5-137">For servers, ANCM is included in the [.NET Core Windows Server Hosting bundle](https://aka.ms/dotnetcore.2.0.0-windowshosting).</span></span> <span data-ttu-id="227c5-138">開發電腦的 Visual Studio 會自動安裝 ANCM 在 IIS Express 中，並在 IIS 中如果已安裝在電腦上。</span><span class="sxs-lookup"><span data-stu-id="227c5-138">For development machines, Visual Studio automatically installs ANCM in IIS Express, and in IIS if it is already installed on the machine.</span></span>

### <a name="install-the-iisintegration-nuget-package"></a><span data-ttu-id="227c5-139">安裝 IISIntegration NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="227c5-139">Install the IISIntegration NuGet package</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="227c5-140">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="227c5-140">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="227c5-141">[Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/)封裝包含在 ASP.NET Core metapackages ([Microsoft.AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore/)和[Microsoft.AspNetCore.All](xref:fundamentals/metapackage)).</span><span class="sxs-lookup"><span data-stu-id="227c5-141">The [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package is included in the ASP.NET Core metapackages ([Microsoft.AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore/) and [Microsoft.AspNetCore.All](xref:fundamentals/metapackage)).</span></span> <span data-ttu-id="227c5-142">如果您不使用其中一個 metapackages，安裝`Microsoft.AspNetCore.Server.IISIntegration`分開。</span><span class="sxs-lookup"><span data-stu-id="227c5-142">If you don't use one of the metapackages, install `Microsoft.AspNetCore.Server.IISIntegration` separately.</span></span> <span data-ttu-id="227c5-143">`IISIntegration`封裝會讀取廣播 ANCM 設定您的應用程式的環境變數的互通性組件。</span><span class="sxs-lookup"><span data-stu-id="227c5-143">The `IISIntegration` package is an interoperability pack that reads environment variables broadcast by ANCM to set up your app.</span></span> <span data-ttu-id="227c5-144">環境變數提供組態資訊，例如要接聽的通訊埠。</span><span class="sxs-lookup"><span data-stu-id="227c5-144">The environment variables provide configuration information, such as the port to listen on.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="227c5-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="227c5-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="227c5-146">在您的應用程式安裝[Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/)。</span><span class="sxs-lookup"><span data-stu-id="227c5-146">In your application, install [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/).</span></span> <span data-ttu-id="227c5-147">`IISIntegration`封裝會讀取廣播 ANCM 設定您的應用程式的環境變數的互通性組件。</span><span class="sxs-lookup"><span data-stu-id="227c5-147">The `IISIntegration` package  is an interoperability pack that reads environment variables broadcast by ANCM to set up your app.</span></span> <span data-ttu-id="227c5-148">環境變數提供組態資訊，例如要接聽的通訊埠。</span><span class="sxs-lookup"><span data-stu-id="227c5-148">The environment variables provide configuration information, such as the port to listen on.</span></span> 

---

### <a name="call-useiisintegration"></a><span data-ttu-id="227c5-149">呼叫 UseIISIntegration</span><span class="sxs-lookup"><span data-stu-id="227c5-149">Call UseIISIntegration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="227c5-150">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="227c5-150">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="227c5-151">`UseIISIntegration`上的擴充方法[ `WebHostBuilder` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder)使用 IIS 執行時自動呼叫。</span><span class="sxs-lookup"><span data-stu-id="227c5-151">The `UseIISIntegration` extension method on [`WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) is called automatically when you run with IIS.</span></span>

<span data-ttu-id="227c5-152">如果您不使用其中一種 ASP.NET Core metapackages 並且尚未安裝`Microsoft.AspNetCore.Server.IISIntegration`封裝時，取得執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="227c5-152">If you aren't using one of the ASP.NET Core metapackages and haven't installed the  `Microsoft.AspNetCore.Server.IISIntegration` package, you get a runtime error.</span></span> <span data-ttu-id="227c5-153">如果您呼叫`UseIISIntegration`明確地便會產生編譯時間錯誤如果封裝未安裝。</span><span class="sxs-lookup"><span data-stu-id="227c5-153">If you call `UseIISIntegration` explicitly, you get a compile time error if the package isn't installed.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="227c5-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="227c5-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="227c5-155">在您的應用程式中`Main`方法，請呼叫`UseIISIntegration`上的擴充方法[ `WebHostBuilder` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder)。</span><span class="sxs-lookup"><span data-stu-id="227c5-155">In your application's `Main` method, call the `UseIISIntegration` extension method on [`WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> 

[!code-csharp[](aspnet-core-module/sample/Program.cs?name=snippet_Main&highlight=12)]

---

<span data-ttu-id="227c5-156">`UseIISIntegration`方法會尋找 ANCM 設定時，環境變數，並且它沒有 ops 如果找不到它們。</span><span class="sxs-lookup"><span data-stu-id="227c5-156">The `UseIISIntegration` method looks for environment variables that ANCM sets, and it no-ops if they aren't found.</span></span> <span data-ttu-id="227c5-157">此行為可加速開發與 macOS 或 Linux 上測試並部署到執行 IIS 的伺服器等案例。</span><span class="sxs-lookup"><span data-stu-id="227c5-157">This behavior facilitates scenarios like developing and testing on macOS or Linux and deploying to a server that runs IIS.</span></span> <span data-ttu-id="227c5-158">MacOS 或 Linux 上執行，同時 Kestrel 做為網頁伺服器;但是，當應用程式部署至 IIS 環境時，它會自動使用 ANCM 和 IIS。</span><span class="sxs-lookup"><span data-stu-id="227c5-158">While running on macOS or Linux, Kestrel acts as the web server; but when the app is deployed to the IIS environment, it automatically uses ANCM and IIS.</span></span>

### <a name="ancm-port-binding-overrides-other-port-bindings"></a><span data-ttu-id="227c5-159">ANCM 連接埠繫結會覆寫其他連接埠繫結</span><span class="sxs-lookup"><span data-stu-id="227c5-159">ANCM port binding overrides other port bindings</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="227c5-160">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="227c5-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="227c5-161">ANCM 會產生要指派給後端程序的動態連接埠。</span><span class="sxs-lookup"><span data-stu-id="227c5-161">ANCM generates a dynamic port to assign to the back-end process.</span></span> <span data-ttu-id="227c5-162">`UseIISIntegration`方法會拾取此動態連接埠，並會設定為接聽 Kestrel `http://locahost:{dynamicPort}/`。</span><span class="sxs-lookup"><span data-stu-id="227c5-162">The `UseIISIntegration` method picks up this dynamic port and configures Kestrel to listen on `http://locahost:{dynamicPort}/`.</span></span> <span data-ttu-id="227c5-163">這會覆寫其他的 URL 設定，例如呼叫`UseUrls`或[Kestrel 的接聽 API](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)。</span><span class="sxs-lookup"><span data-stu-id="227c5-163">This overrides other URL configurations, such as calls to `UseUrls` or [Kestrel's Listen API](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span> <span data-ttu-id="227c5-164">因此，您不需要呼叫`UseUrls`或 Kestrel 的`Listen`API，當您使用 ANCM。</span><span class="sxs-lookup"><span data-stu-id="227c5-164">Therefore, you don't need to call `UseUrls` or Kestrel's `Listen` API when you use ANCM.</span></span> <span data-ttu-id="227c5-165">如果您呼叫`UseUrls`或`Listen`，Kestrel 執行未使用 IIS 應用程式時指定通訊埠上接聽。</span><span class="sxs-lookup"><span data-stu-id="227c5-165">If you do call `UseUrls` or `Listen`, Kestrel listens on the port you specify when you run the app without IIS.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="227c5-166">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="227c5-166">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="227c5-167">ANCM 會產生要指派給後端程序的動態連接埠。</span><span class="sxs-lookup"><span data-stu-id="227c5-167">ANCM generates a dynamic port to assign to the back-end process.</span></span> <span data-ttu-id="227c5-168">`UseIISIntegration`方法會拾取此動態連接埠，並會設定為接聽 Kestrel `http://locahost:{dynamicPort}/`。</span><span class="sxs-lookup"><span data-stu-id="227c5-168">The `UseIISIntegration` method picks up this dynamic port and configures Kestrel to listen on `http://locahost:{dynamicPort}/`.</span></span> <span data-ttu-id="227c5-169">這會覆寫其他的 URL 設定，例如呼叫`UseUrls`。</span><span class="sxs-lookup"><span data-stu-id="227c5-169">This overrides other URL configurations, such as calls to `UseUrls`.</span></span> <span data-ttu-id="227c5-170">因此，您不需要呼叫`UseUrls`當您使用 ANCM。</span><span class="sxs-lookup"><span data-stu-id="227c5-170">Therefore, you don't need to call `UseUrls` when you use ANCM.</span></span> <span data-ttu-id="227c5-171">如果您呼叫`UseUrls`，Kestrel 執行未使用 IIS 應用程式時指定通訊埠上接聽。</span><span class="sxs-lookup"><span data-stu-id="227c5-171">If you do call `UseUrls`, Kestrel listens on the port you specify when you run the app without IIS.</span></span>

<span data-ttu-id="227c5-172">在 ASP.NET Core 1.0 中，如果您呼叫`UseUrls`，稱之為 「**之前**您呼叫`UseIISIntegration`以便 ANCM 設定連接埠不會覆寫。</span><span class="sxs-lookup"><span data-stu-id="227c5-172">In ASP.NET Core 1.0, if you call `UseUrls`, call it **before** you call `UseIISIntegration` so that the ANCM-configured port doesn't get overwritten.</span></span> <span data-ttu-id="227c5-173">在 ASP.NET Core 1.1 中，您不需要此呼叫的順序，因為 ANCM 設定會覆寫`UseUrls`。</span><span class="sxs-lookup"><span data-stu-id="227c5-173">This calling order isn't required in ASP.NET Core 1.1, because the ANCM setting overrides `UseUrls`.</span></span>

---

### <a name="configure-ancm-options-in-webconfig"></a><span data-ttu-id="227c5-174">在 Web.config 中設定 ANCM 選項</span><span class="sxs-lookup"><span data-stu-id="227c5-174">Configure ANCM options in Web.config</span></span>

<span data-ttu-id="227c5-175">ASP.NET Core 模組的設定會儲存在*Web.config*位於應用程式的根資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="227c5-175">Configuration for the ASP.NET Core Module is stored in the *Web.config* file that is located in the application's root folder.</span></span> <span data-ttu-id="227c5-176">此檔案中的設定會指向的啟動命令和啟動 ASP.NET Core 應用程式的引數。</span><span class="sxs-lookup"><span data-stu-id="227c5-176">Settings in this file point to the startup command and arguments that start your ASP.NET Core app.</span></span> <span data-ttu-id="227c5-177">範例 Web.config 程式碼和組態選項的指引，請參閱[ASP.NET 核心模組的組態參考](../../hosting/aspnet-core-module.md)。</span><span class="sxs-lookup"><span data-stu-id="227c5-177">For sample Web.config code and guidance on configuration options, see [ASP.NET Core Module Configuration Reference](../../hosting/aspnet-core-module.md).</span></span>

### <a name="run-with-iis-express-in-development"></a><span data-ttu-id="227c5-178">在開發中執行的 IIS Express</span><span class="sxs-lookup"><span data-stu-id="227c5-178">Run with IIS Express in development</span></span>

<span data-ttu-id="227c5-179">IIS Express 可以由 Visual Studio 中使用 ASP.NET Core 範本所定義的預設設定檔啟動。</span><span class="sxs-lookup"><span data-stu-id="227c5-179">IIS Express can be launched by Visual Studio using the default profile defined by the ASP.NET Core templates.</span></span>

## <a name="next-steps"></a><span data-ttu-id="227c5-180">後續步驟</span><span class="sxs-lookup"><span data-stu-id="227c5-180">Next steps</span></span>

<span data-ttu-id="227c5-181">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="227c5-181">For more information, see the following resources:</span></span>

* [<span data-ttu-id="227c5-182">這篇文章的範例應用程式</span><span class="sxs-lookup"><span data-stu-id="227c5-182">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)
* [<span data-ttu-id="227c5-183">ASP.NET 核心模組的原始程式碼</span><span class="sxs-lookup"><span data-stu-id="227c5-183">ASP.NET Core Module source code</span></span>](https://github.com/aspnet/AspNetCoreModule)
* [<span data-ttu-id="227c5-184">ASP.NET Core 模組的組態參考</span><span class="sxs-lookup"><span data-stu-id="227c5-184">ASP.NET Core Module Configuration Reference</span></span>](../../hosting/aspnet-core-module.md)
* [<span data-ttu-id="227c5-185">發行至 IIS</span><span class="sxs-lookup"><span data-stu-id="227c5-185">Publishing to IIS</span></span>](../../publishing/iis.md)
