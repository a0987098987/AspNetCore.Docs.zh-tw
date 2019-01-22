---
title: ASP.NET Core 模組
author: guardrex
description: 了解如何設定 ASP.NET Core 模組以裝載 ASP.NET Core 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 192e4bf8e970083cc05babcd7fb3cf52985e35bf
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396320"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="88e60-103">ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="88e60-103">ASP.NET Core Module</span></span>

<span data-ttu-id="88e60-104">作者：[Tom Dykstra](https://github.com/tdykstra)、[Rick Strahl](https://github.com/RickStrahl)、[Chris Ross](https://github.com/Tratcher)、[Rick Anderson](https://twitter.com/RickAndMSFT)、[Sourabh Shirhatti](https://twitter.com/sshirhatti)、[Justin Kotalik](https://github.com/jkotalik) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="88e60-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="88e60-105">ASP.NET Core 模組是一種原生 IIS 模組，可外掛至 IIS 管線以便：</span><span class="sxs-lookup"><span data-stu-id="88e60-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="88e60-106">在 IIS 工作者處理序 (`w3wp.exe`) 中裝載 ASP.NET Core 應用程式 (稱為[同處理序裝載模型](#in-process-hosting-model))。</span><span class="sxs-lookup"><span data-stu-id="88e60-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="88e60-107">將 Web 要求轉送到執行 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的後端 ASP.NET Core 應用程式 (稱為[跨處理序裝載模型](#out-of-process-hosting-model))。</span><span class="sxs-lookup"><span data-stu-id="88e60-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="88e60-108">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="88e60-108">Supported Windows versions:</span></span>

* <span data-ttu-id="88e60-109">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="88e60-109">Windows 7 or later</span></span>
* <span data-ttu-id="88e60-110">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="88e60-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="88e60-111">同處理序裝載時，模組會使用 IIS 的同處理序伺服程式實作，稱為 IIS HTTP 伺服器 (`IISHttpServer`)。</span><span class="sxs-lookup"><span data-stu-id="88e60-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="88e60-112">跨處理序裝載時，該模組只適用於 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="88e60-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="88e60-113">該模組與 [HTTP.sys](xref:fundamentals/servers/httpsys) 不相容。</span><span class="sxs-lookup"><span data-stu-id="88e60-113">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="88e60-114">裝載模型</span><span class="sxs-lookup"><span data-stu-id="88e60-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="88e60-115">同處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="88e60-115">In-process hosting model</span></span>

<span data-ttu-id="88e60-116">若要設定同處理序裝載的應用程式，請將 `<AspNetCoreHostingModel>` 屬性新增至應用程式的專案檔，其值為 `InProcess` (跨處理序裝載是使用 `OutOfProcess` 設定)：</span><span class="sxs-lookup"><span data-stu-id="88e60-116">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="88e60-117">以 .NET Framework 為目標的 ASP.NET Core 應用程式不支援處理序內裝載模型。</span><span class="sxs-lookup"><span data-stu-id="88e60-117">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="88e60-118">如果檔案中沒有 `<AspNetCoreHostingModel>` 屬性，預設值為 `OutOfProcess`。</span><span class="sxs-lookup"><span data-stu-id="88e60-118">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="88e60-119">同處理序裝載時具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="88e60-119">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="88e60-120">使用 IIS HTTP 伺服器 (`IISHttpServer`) 而不是 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器。</span><span class="sxs-lookup"><span data-stu-id="88e60-120">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

* <span data-ttu-id="88e60-121">[requestTimeout 屬性](#attributes-of-the-aspnetcore-element)不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="88e60-121">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="88e60-122">不支援在應用程式之間共用應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="88e60-122">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="88e60-123">每個應用程式使用一個應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="88e60-123">Use one app pool per app.</span></span>

* <span data-ttu-id="88e60-124">使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 或以手動方式[將 app_offline.htm 檔案放入部署](xref:host-and-deploy/iis/index#locked-deployment-files)時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="88e60-124">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="88e60-125">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="88e60-125">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="88e60-126">應用程式的架構 (位元) 和已安裝的執行階段 (x64 或 x86) 必須符合應用程式集區的架構。</span><span class="sxs-lookup"><span data-stu-id="88e60-126">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="88e60-127">如果使用 `WebHostBuilder` (而不是使用 [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) 以手動方式設定應用程式的主機，而且曾在 Kestrel 伺服器上直接執行應用程式 (自我裝載)，請先呼叫 `UseKestrel`，再呼叫 `UseIISIntegration`。</span><span class="sxs-lookup"><span data-stu-id="88e60-127">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="88e60-128">如果順序相反，就無法啟動主機。</span><span class="sxs-lookup"><span data-stu-id="88e60-128">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="88e60-129">偵測到用戶端中斷連線。</span><span class="sxs-lookup"><span data-stu-id="88e60-129">Client disconnects are detected.</span></span> <span data-ttu-id="88e60-130">用戶端中斷連線時，會取消 [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) 取消權杖。</span><span class="sxs-lookup"><span data-stu-id="88e60-130">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="88e60-131"><xref:System.IO.Directory.GetCurrentDirectory*> 會傳回 IIS 所啟動處理序的背景工作目錄，而非應用程式目錄 (例如 *w3wp.exe* 為 *C:\Windows\System32\inetsrv*)。</span><span class="sxs-lookup"><span data-stu-id="88e60-131"><xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="88e60-132">如需設定應用程式目前所在目錄的範例程式碼，請參閱 [CurrentDirectoryHelpers 類別](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs)。</span><span class="sxs-lookup"><span data-stu-id="88e60-132">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="88e60-133">呼叫 `SetCurrentDirectory` 方法。</span><span class="sxs-lookup"><span data-stu-id="88e60-133">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="88e60-134">後續呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 會提供應用程式的目錄。</span><span class="sxs-lookup"><span data-stu-id="88e60-134">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="88e60-135">跨處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="88e60-135">Out-of-process hosting model</span></span>

<span data-ttu-id="88e60-136">若要設定跨處理序裝載的應用程式，請在專案檔中使用下列任一方法：</span><span class="sxs-lookup"><span data-stu-id="88e60-136">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="88e60-137">請勿指定 `<AspNetCoreHostingModel>` 屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-137">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="88e60-138">如果檔案中沒有 `<AspNetCoreHostingModel>` 屬性，預設值為 `OutOfProcess`。</span><span class="sxs-lookup"><span data-stu-id="88e60-138">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="88e60-139">將 `<AspNetCoreHostingModel>` 屬性的值設定為 `OutOfProcess` (同處理序裝載是使用 `InProcess` 設定)：</span><span class="sxs-lookup"><span data-stu-id="88e60-139">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="88e60-140">使用 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器而不是 IIS HTTP 伺服器 (`IISHttpServer`)。</span><span class="sxs-lookup"><span data-stu-id="88e60-140">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="88e60-141">裝載模型變更</span><span class="sxs-lookup"><span data-stu-id="88e60-141">Hosting model changes</span></span>

<span data-ttu-id="88e60-142">如果 `hostingModel` 設定在 *web.config* 檔案中已有所變更 (如[使用 web.config 進行設定](#configuration-with-webconfig)一節中所說明)，模組會回收 IIS 的工作者處理序。</span><span class="sxs-lookup"><span data-stu-id="88e60-142">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="88e60-143">針對 IIS Express，模組不會回收工作者處理序，但會改為觸發目前 IIS Express 處理序的正常關閉。</span><span class="sxs-lookup"><span data-stu-id="88e60-143">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="88e60-144">應用程式的下一個要求會繁衍新的 IIS Express 處理序。</span><span class="sxs-lookup"><span data-stu-id="88e60-144">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="88e60-145">處理序名稱</span><span class="sxs-lookup"><span data-stu-id="88e60-145">Process name</span></span>

<span data-ttu-id="88e60-146">`Process.GetCurrentProcess().ProcessName` 會報告 `w3wp`/`iisexpress` (同處理序) 或 `dotnet` (跨處理序)。</span><span class="sxs-lookup"><span data-stu-id="88e60-146">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="88e60-147">ASP.NET Core 模組是一種原生 IIS 模組，可外掛至 IIS 管線，將 Web 要求重新轉送到後端的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="88e60-147">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="88e60-148">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="88e60-148">Supported Windows versions:</span></span>

* <span data-ttu-id="88e60-149">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="88e60-149">Windows 7 or later</span></span>
* <span data-ttu-id="88e60-150">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="88e60-150">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="88e60-151">該模組只適用於 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="88e60-151">The module only works with Kestrel.</span></span> <span data-ttu-id="88e60-152">該模組與 [HTTP.sys](xref:fundamentals/servers/httpsys) 不相容。</span><span class="sxs-lookup"><span data-stu-id="88e60-152">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="88e60-153">因為處理序中執行的 ASP.NET Core 應用程式會與 IIS 背景工作處理序分開，所以此模組也會執行處理序管理。</span><span class="sxs-lookup"><span data-stu-id="88e60-153">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="88e60-154">此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="88e60-154">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="88e60-155">此行為基本上與在 IIS 中執行同處理序，並由 [Windows 處理器啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的 ASP.NET 4.x 應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="88e60-155">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="88e60-156">下圖說明 IIS、ASP.NET Core 模組和應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="88e60-156">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![ASP.NET Core 模組](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="88e60-158">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="88e60-158">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="88e60-159">驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="88e60-159">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="88e60-160">此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，而且不會是通訊埠 80 或 443。</span><span class="sxs-lookup"><span data-stu-id="88e60-160">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="88e60-161">此模組在啟動時透過環境變數指定通訊埠，而 IIS 整合中介軟體則會設定伺服器來接聽 `http://localhost:{port}`。</span><span class="sxs-lookup"><span data-stu-id="88e60-161">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="88e60-162">將會執行額外檢查，不是源自模組的要求都會遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="88e60-162">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="88e60-163">此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。</span><span class="sxs-lookup"><span data-stu-id="88e60-163">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="88e60-164">Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="88e60-164">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="88e60-165">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="88e60-165">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="88e60-166">IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="88e60-166">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="88e60-167">應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="88e60-167">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="88e60-168">許多如 Windows 驗證等原生模組仍在使用中。</span><span class="sxs-lookup"><span data-stu-id="88e60-168">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="88e60-169">若要深入了解搭配 ASP.NET Core 模組的使用中 IIS 模組，請參閱<xref:host-and-deploy/iis/modules>。</span><span class="sxs-lookup"><span data-stu-id="88e60-169">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="88e60-170">ASP.NET Core 模組也可以：</span><span class="sxs-lookup"><span data-stu-id="88e60-170">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="88e60-171">設定背景工作處理序的環境變數。</span><span class="sxs-lookup"><span data-stu-id="88e60-171">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="88e60-172">將 stdout 輸出記錄到檔案儲存區，以針對啟動問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="88e60-172">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="88e60-173">轉送 Windows 驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="88e60-173">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="88e60-174">如何安裝和使用 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="88e60-174">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="88e60-175">如需如何安裝和使用 ASP.NET Core 模組的指示，請參閱<xref:host-and-deploy/iis/index>。</span><span class="sxs-lookup"><span data-stu-id="88e60-175">For instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="88e60-176">使用 web.config 進行設定</span><span class="sxs-lookup"><span data-stu-id="88e60-176">Configuration with web.config</span></span>

<span data-ttu-id="88e60-177">設定 ASP.NET Core 模組時，是使用網站 *web.config* 檔案中 `system.webServer` 節點的 `aspNetCore` 區段來設定。</span><span class="sxs-lookup"><span data-stu-id="88e60-177">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="88e60-178">以下 *web.config* 檔案是針對[架構相依部署](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)發佈的檔案，會設定 ASP.NET Core 模組來處理網站要求：</span><span class="sxs-lookup"><span data-stu-id="88e60-178">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath="dotnet" 
                  arguments=".\MyApp.dll" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="88e60-179">以下 *web.config* 是針對[自封式部署](/dotnet/articles/core/deploying/#self-contained-deployments-scd)發佈的檔案：</span><span class="sxs-lookup"><span data-stu-id="88e60-179">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath=".\MyApp.exe" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="88e60-180">將 <xref:System.Configuration.SectionInformation.InheritInChildApplications*> 屬性設定為 `false`，以表示在 [\<位置>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) 內指定的設定，不是由位在應用程式子目錄中的應用程式所繼承。</span><span class="sxs-lookup"><span data-stu-id="88e60-180">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="88e60-181">將應用程式部署至 [Azure App Service](https://azure.microsoft.com/services/app-service/) 時，`stdoutLogFile` 路徑會設定為 `\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="88e60-181">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="88e60-182">此路徑會將 stdout 記錄檔儲存至 [LogFiles] 資料夾，這是服務自動建立的位置。</span><span class="sxs-lookup"><span data-stu-id="88e60-182">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="88e60-183">如需有關 IIS 子應用程式設定的詳細資訊，請參閱 <xref:host-and-deploy/iis/index#sub-applications>。</span><span class="sxs-lookup"><span data-stu-id="88e60-183">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="88e60-184">aspNetCore 元素的屬性</span><span class="sxs-lookup"><span data-stu-id="88e60-184">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="88e60-185">屬性</span><span class="sxs-lookup"><span data-stu-id="88e60-185">Attribute</span></span> | <span data-ttu-id="88e60-186">說明</span><span class="sxs-lookup"><span data-stu-id="88e60-186">Description</span></span> | <span data-ttu-id="88e60-187">預設</span><span class="sxs-lookup"><span data-stu-id="88e60-187">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="88e60-188">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-188">Optional string attribute.</span></span></p><p><span data-ttu-id="88e60-189">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="88e60-189">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="88e60-190">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-190">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="88e60-191">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="88e60-191">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="88e60-192">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-192">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="88e60-193">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="88e60-193">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="88e60-194">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="88e60-194">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="88e60-195">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-195">Optional string attribute.</span></span></p><p><span data-ttu-id="88e60-196">將裝載模型指定為同處理序 (`InProcess`) 或跨處理序 (`OutOfProcess`)。</span><span class="sxs-lookup"><span data-stu-id="88e60-196">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="88e60-197">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-197">Optional integer attribute.</span></span></p><p><span data-ttu-id="88e60-198">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="88e60-198">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="88e60-199">&dagger;針對同處理序裝載，此值會限制為 `1`。</span><span class="sxs-lookup"><span data-stu-id="88e60-199">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="88e60-200">預設值：`1`</span><span class="sxs-lookup"><span data-stu-id="88e60-200">Default: `1`</span></span><br><span data-ttu-id="88e60-201">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="88e60-201">Min: `1`</span></span><br><span data-ttu-id="88e60-202">最大值：`100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="88e60-202">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="88e60-203">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-203">Required string attribute.</span></span></p><p><span data-ttu-id="88e60-204">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="88e60-204">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="88e60-205">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="88e60-205">Relative paths are supported.</span></span> <span data-ttu-id="88e60-206">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="88e60-206">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="88e60-207">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-207">Optional integer attribute.</span></span></p><p><span data-ttu-id="88e60-208">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="88e60-208">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="88e60-209">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="88e60-209">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="88e60-210">不支援同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="88e60-210">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="88e60-211">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="88e60-211">Default: `10`</span></span><br><span data-ttu-id="88e60-212">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="88e60-212">Min: `0`</span></span><br><span data-ttu-id="88e60-213">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="88e60-213">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="88e60-214">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-214">Optional timespan attribute.</span></span></p><p><span data-ttu-id="88e60-215">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="88e60-215">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="88e60-216">在 ASP.NET Core 2.1 或更新版本隨附的 ASP.NET Core 模組版本中，是以小時、分鐘及秒為單位來指定 `requestTimeout`。</span><span class="sxs-lookup"><span data-stu-id="88e60-216">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="88e60-217">不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="88e60-217">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="88e60-218">針對同處理序裝載，該模組會等待應用程式處理要求。</span><span class="sxs-lookup"><span data-stu-id="88e60-218">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="88e60-219">預設值：`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="88e60-219">Default: `00:02:00`</span></span><br><span data-ttu-id="88e60-220">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="88e60-220">Min: `00:00:00`</span></span><br><span data-ttu-id="88e60-221">最大值︰`360:00:00`</span><span class="sxs-lookup"><span data-stu-id="88e60-221">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="88e60-222">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-222">Optional integer attribute.</span></span></p><p><span data-ttu-id="88e60-223">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="88e60-223">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="88e60-224">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="88e60-224">Default: `10`</span></span><br><span data-ttu-id="88e60-225">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="88e60-225">Min: `0`</span></span><br><span data-ttu-id="88e60-226">最大值︰`600`</span><span class="sxs-lookup"><span data-stu-id="88e60-226">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="88e60-227">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-227">Optional integer attribute.</span></span></p><p><span data-ttu-id="88e60-228">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="88e60-228">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="88e60-229">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="88e60-229">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="88e60-230">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="88e60-230">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="88e60-231">0 (零) 值**不會**視為無限逾時。</span><span class="sxs-lookup"><span data-stu-id="88e60-231">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="88e60-232">預設值：`120`</span><span class="sxs-lookup"><span data-stu-id="88e60-232">Default: `120`</span></span><br><span data-ttu-id="88e60-233">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="88e60-233">Min: `0`</span></span><br><span data-ttu-id="88e60-234">最大值︰`3600`</span><span class="sxs-lookup"><span data-stu-id="88e60-234">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="88e60-235">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-235">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="88e60-236">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="88e60-236">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="88e60-237">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-237">Optional string attribute.</span></span></p><p><span data-ttu-id="88e60-238">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="88e60-238">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="88e60-239">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="88e60-239">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="88e60-240">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="88e60-240">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="88e60-241">建立記錄檔後，模組會建立路徑中提供的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="88e60-241">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="88e60-242">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 (*.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="88e60-242">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="88e60-243">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="88e60-243">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="88e60-244">屬性</span><span class="sxs-lookup"><span data-stu-id="88e60-244">Attribute</span></span> | <span data-ttu-id="88e60-245">說明</span><span class="sxs-lookup"><span data-stu-id="88e60-245">Description</span></span> | <span data-ttu-id="88e60-246">預設</span><span class="sxs-lookup"><span data-stu-id="88e60-246">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="88e60-247">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-247">Optional string attribute.</span></span></p><p><span data-ttu-id="88e60-248">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="88e60-248">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="88e60-249">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-249">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="88e60-250">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="88e60-250">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="88e60-251">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-251">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="88e60-252">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="88e60-252">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="88e60-253">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="88e60-253">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="88e60-254">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-254">Optional integer attribute.</span></span></p><p><span data-ttu-id="88e60-255">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="88e60-255">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="88e60-256">預設值：`1`</span><span class="sxs-lookup"><span data-stu-id="88e60-256">Default: `1`</span></span><br><span data-ttu-id="88e60-257">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="88e60-257">Min: `1`</span></span><br><span data-ttu-id="88e60-258">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="88e60-258">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="88e60-259">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-259">Required string attribute.</span></span></p><p><span data-ttu-id="88e60-260">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="88e60-260">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="88e60-261">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="88e60-261">Relative paths are supported.</span></span> <span data-ttu-id="88e60-262">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="88e60-262">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="88e60-263">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-263">Optional integer attribute.</span></span></p><p><span data-ttu-id="88e60-264">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="88e60-264">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="88e60-265">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="88e60-265">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="88e60-266">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="88e60-266">Default: `10`</span></span><br><span data-ttu-id="88e60-267">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="88e60-267">Min: `0`</span></span><br><span data-ttu-id="88e60-268">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="88e60-268">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="88e60-269">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-269">Optional timespan attribute.</span></span></p><p><span data-ttu-id="88e60-270">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="88e60-270">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="88e60-271">在 ASP.NET Core 2.1 或更新版本隨附的 ASP.NET Core 模組版本中，是以小時、分鐘及秒為單位來指定 `requestTimeout`。</span><span class="sxs-lookup"><span data-stu-id="88e60-271">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="88e60-272">預設值：`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="88e60-272">Default: `00:02:00`</span></span><br><span data-ttu-id="88e60-273">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="88e60-273">Min: `00:00:00`</span></span><br><span data-ttu-id="88e60-274">最大值︰`360:00:00`</span><span class="sxs-lookup"><span data-stu-id="88e60-274">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="88e60-275">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-275">Optional integer attribute.</span></span></p><p><span data-ttu-id="88e60-276">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="88e60-276">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="88e60-277">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="88e60-277">Default: `10`</span></span><br><span data-ttu-id="88e60-278">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="88e60-278">Min: `0`</span></span><br><span data-ttu-id="88e60-279">最大值︰`600`</span><span class="sxs-lookup"><span data-stu-id="88e60-279">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="88e60-280">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-280">Optional integer attribute.</span></span></p><p><span data-ttu-id="88e60-281">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="88e60-281">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="88e60-282">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="88e60-282">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="88e60-283">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="88e60-283">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="88e60-284">0 (零) 值**不會**視為無限逾時。</span><span class="sxs-lookup"><span data-stu-id="88e60-284">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="88e60-285">預設值：`120`</span><span class="sxs-lookup"><span data-stu-id="88e60-285">Default: `120`</span></span><br><span data-ttu-id="88e60-286">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="88e60-286">Min: `0`</span></span><br><span data-ttu-id="88e60-287">最大值︰`3600`</span><span class="sxs-lookup"><span data-stu-id="88e60-287">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="88e60-288">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-288">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="88e60-289">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="88e60-289">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="88e60-290">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-290">Optional string attribute.</span></span></p><p><span data-ttu-id="88e60-291">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="88e60-291">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="88e60-292">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="88e60-292">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="88e60-293">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="88e60-293">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="88e60-294">路徑中提供的所有資料夾都必須存在，模組才能建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="88e60-294">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="88e60-295">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 (*.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="88e60-295">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="88e60-296">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="88e60-296">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="88e60-297">屬性</span><span class="sxs-lookup"><span data-stu-id="88e60-297">Attribute</span></span> | <span data-ttu-id="88e60-298">說明</span><span class="sxs-lookup"><span data-stu-id="88e60-298">Description</span></span> | <span data-ttu-id="88e60-299">預設</span><span class="sxs-lookup"><span data-stu-id="88e60-299">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="88e60-300">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-300">Optional string attribute.</span></span></p><p><span data-ttu-id="88e60-301">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="88e60-301">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="88e60-302">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-302">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="88e60-303">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="88e60-303">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="88e60-304">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-304">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="88e60-305">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="88e60-305">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="88e60-306">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="88e60-306">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="88e60-307">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-307">Optional integer attribute.</span></span></p><p><span data-ttu-id="88e60-308">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="88e60-308">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="88e60-309">預設值：`1`</span><span class="sxs-lookup"><span data-stu-id="88e60-309">Default: `1`</span></span><br><span data-ttu-id="88e60-310">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="88e60-310">Min: `1`</span></span><br><span data-ttu-id="88e60-311">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="88e60-311">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="88e60-312">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-312">Required string attribute.</span></span></p><p><span data-ttu-id="88e60-313">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="88e60-313">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="88e60-314">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="88e60-314">Relative paths are supported.</span></span> <span data-ttu-id="88e60-315">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="88e60-315">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="88e60-316">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-316">Optional integer attribute.</span></span></p><p><span data-ttu-id="88e60-317">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="88e60-317">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="88e60-318">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="88e60-318">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="88e60-319">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="88e60-319">Default: `10`</span></span><br><span data-ttu-id="88e60-320">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="88e60-320">Min: `0`</span></span><br><span data-ttu-id="88e60-321">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="88e60-321">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="88e60-322">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-322">Optional timespan attribute.</span></span></p><p><span data-ttu-id="88e60-323">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="88e60-323">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="88e60-324">在 ASP.NET Core 2.0 或更舊版本隨附的 ASP.NET Core 模組版本中，只能以整數分鐘為單位來指定 `requestTimeout`，否則會預設為 2 分鐘。</span><span class="sxs-lookup"><span data-stu-id="88e60-324">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="88e60-325">預設值：`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="88e60-325">Default: `00:02:00`</span></span><br><span data-ttu-id="88e60-326">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="88e60-326">Min: `00:00:00`</span></span><br><span data-ttu-id="88e60-327">最大值︰`360:00:00`</span><span class="sxs-lookup"><span data-stu-id="88e60-327">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="88e60-328">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-328">Optional integer attribute.</span></span></p><p><span data-ttu-id="88e60-329">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="88e60-329">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="88e60-330">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="88e60-330">Default: `10`</span></span><br><span data-ttu-id="88e60-331">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="88e60-331">Min: `0`</span></span><br><span data-ttu-id="88e60-332">最大值︰`600`</span><span class="sxs-lookup"><span data-stu-id="88e60-332">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="88e60-333">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-333">Optional integer attribute.</span></span></p><p><span data-ttu-id="88e60-334">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="88e60-334">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="88e60-335">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="88e60-335">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="88e60-336">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="88e60-336">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="88e60-337">預設值：`120`</span><span class="sxs-lookup"><span data-stu-id="88e60-337">Default: `120`</span></span><br><span data-ttu-id="88e60-338">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="88e60-338">Min: `0`</span></span><br><span data-ttu-id="88e60-339">最大值︰`3600`</span><span class="sxs-lookup"><span data-stu-id="88e60-339">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="88e60-340">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-340">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="88e60-341">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="88e60-341">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="88e60-342">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-342">Optional string attribute.</span></span></p><p><span data-ttu-id="88e60-343">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="88e60-343">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="88e60-344">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="88e60-344">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="88e60-345">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="88e60-345">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="88e60-346">路徑中提供的所有資料夾都必須存在，模組才能建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="88e60-346">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="88e60-347">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 (*.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="88e60-347">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="88e60-348">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="88e60-348">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="88e60-349">設定環境變數</span><span class="sxs-lookup"><span data-stu-id="88e60-349">Setting environment variables</span></span>

<span data-ttu-id="88e60-350">您可以在 `processPath` 屬性中為處理序指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="88e60-350">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="88e60-351">請使用 `environmentVariables` 集合元素的 `environmentVariable` 子元素來指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="88e60-351">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="88e60-352">本節中所設定環境變數的優先順序會高於系統環境變數。</span><span class="sxs-lookup"><span data-stu-id="88e60-352">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="88e60-353">下列範例會設定兩個環境變數。</span><span class="sxs-lookup"><span data-stu-id="88e60-353">The following example sets two environment variables.</span></span> <span data-ttu-id="88e60-354">`ASPNETCORE_ENVIRONMENT` 會將應用程式的環境設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="88e60-354">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="88e60-355">開發人員可以在 *web.config* 檔案中暫時設定這個值，以在進行應用程式例外狀況偵錯時，強制[開發人員例外狀況頁面](xref:fundamentals/error-handling)載入。</span><span class="sxs-lookup"><span data-stu-id="88e60-355">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="88e60-356">`CONFIG_DIR` 是一個使用者定義的環境變數範例，其中開發人員已撰寫程式碼，會在啟動時讀取值來構成用以載入應用程式設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="88e60-356">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="88e60-357">請只有在未受信任網路 (例如網際網路) 無法存取的暫存和測試伺服器上，才將 `ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="88e60-357">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="88e60-358">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="88e60-358">app_offline.htm</span></span>

<span data-ttu-id="88e60-359">如果在應用程式根目錄中偵測到名稱為 *app_offline.htm* 的檔案，ASP.NET Core Module 會嘗試正常關閉應用程式並停止處理連入的要求。</span><span class="sxs-lookup"><span data-stu-id="88e60-359">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="88e60-360">如果在經過 `shutdownTimeLimit` 中所定義的秒數之後，應用程式仍然在執行，ASP.NET Core Module 就會終止執行中的處理序。</span><span class="sxs-lookup"><span data-stu-id="88e60-360">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="88e60-361">當 *app_offline.htm* 檔案存在時，ASP.NET Core 模組會藉由傳回 *app_offline.htm* 檔案的內容來回應要求。</span><span class="sxs-lookup"><span data-stu-id="88e60-361">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="88e60-362">已移除 *app_offline.htm* 檔案時，下一個要求則會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="88e60-362">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="88e60-363">使用跨處理序裝載模型時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="88e60-363">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="88e60-364">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="88e60-364">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="88e60-365">啟動錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="88e60-365">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="88e60-366">同處理序及跨處理序裝載無法啟動應用程式時，兩者皆會產生自訂錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="88e60-366">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="88e60-367">若 ASP.NET Core 模組找不到同處理序或跨處理序要求處理常式時，就會顯示 [500.0 - 同處理序/跨處理序處理常式載入失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="88e60-367">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="88e60-368">對於同處理序裝載，若 ASP.NET Core 模組無法啟動應用程式，就會顯示 [500.30 - 啟動失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="88e60-368">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="88e60-369">對於跨處理序裝載，若 ASP.NET Core 模組無法啟動後端處理序，或後端處理序啟動但無法在所設定的連接埠上進行接聽，就會顯示 [502.5 - 處理序失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="88e60-369">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="88e60-370">若要避免此頁面產生並還原至預設的 IIS 5xx 狀態碼頁面，請使用 `disableStartUpErrorPage` 屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-370">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="88e60-371">如需設定自訂錯誤訊息的詳細資訊，請參閱 [HTTP 錯誤 \<httpErrors>](/iis/configuration/system.webServer/httpErrors/)。</span><span class="sxs-lookup"><span data-stu-id="88e60-371">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="88e60-372">如果 ASP.NET Core 模組無法啟動後端處理序，或後端處理序啟動但無法在所設定的連接埠上進行接聽，就會顯示 [502.5 - 處理序失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="88e60-372">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="88e60-373">若要抑制此頁面並還原至預設的 IIS 502 狀態碼頁面，請使用 `disableStartUpErrorPage` 屬性。</span><span class="sxs-lookup"><span data-stu-id="88e60-373">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="88e60-374">如需設定自訂錯誤訊息的詳細資訊，請參閱 [HTTP 錯誤 \<httpErrors>](/iis/configuration/system.webServer/httpErrors/)。</span><span class="sxs-lookup"><span data-stu-id="88e60-374">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 處理序失敗狀態碼頁面](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="88e60-376">記錄檔建立和重新導向</span><span class="sxs-lookup"><span data-stu-id="88e60-376">Log creation and redirection</span></span>

<span data-ttu-id="88e60-377">如果已設定 `aspNetCore` 元素的 `stdoutLogEnabled` 和 `stdoutLogFile` 屬性，ASP.NET Core 模組就會將 stdout 和 stderr 主控台輸出重新導向到磁碟。</span><span class="sxs-lookup"><span data-stu-id="88e60-377">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="88e60-378">建立記錄檔後，模組會建立 `stdoutLogFile` 路徑中的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="88e60-378">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="88e60-379">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="88e60-379">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="88e60-380">除非發生處理序回收/重新啟動，否則不會輪替記錄檔。</span><span class="sxs-lookup"><span data-stu-id="88e60-380">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="88e60-381">主機服務提供者必須負責限制記錄檔所使用的磁碟空間。</span><span class="sxs-lookup"><span data-stu-id="88e60-381">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="88e60-382">建議只有在進行應用程式啟動問題疑難排解時，才使用 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="88e60-382">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="88e60-383">請勿將 stdout 記錄檔用來進行一般應用程式記錄。</span><span class="sxs-lookup"><span data-stu-id="88e60-383">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="88e60-384">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="88e60-384">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="88e60-385">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="88e60-385">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="88e60-386">建立記錄檔時，系統會自動新增時間戳記和副檔名。</span><span class="sxs-lookup"><span data-stu-id="88e60-386">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="88e60-387">記錄檔名稱會藉由將時間戳記、處理序識別碼及副檔名 (*.log*) 以底線分隔並附加至 `stdoutLogFile` 路徑的最後一個區段 (通常是 *stdout*) 來組成。</span><span class="sxs-lookup"><span data-stu-id="88e60-387">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="88e60-388">如果 `stdoutLogFile` 路徑的結尾是 *stdout*，則在 2018 年 2 月 5 日 19:42:32 建立且 PID 為 1934 的應用程式記錄檔檔案名稱會是 *stdout_20180205194132_1934.log*。</span><span class="sxs-lookup"><span data-stu-id="88e60-388">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="88e60-389">若 `stdoutLogEnabled` 為 false，會擷取在應用程式啟動時發生的錯誤，並發出最大 30KB 的事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="88e60-389">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="88e60-390">啟動之後，就會捨棄其他的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="88e60-390">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="88e60-391">下列範例 `aspNetCore` 元素會設定 Azure App Service 中所裝載應用程式的 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="88e60-391">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="88e60-392">系統可接受使用本機路徑或網路共用路徑來進行本機記錄。</span><span class="sxs-lookup"><span data-stu-id="88e60-392">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="88e60-393">請確認 AppPool 使用者身分識別具備所提供路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="88e60-393">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="88e60-394">增強型診斷記錄</span><span class="sxs-lookup"><span data-stu-id="88e60-394">Enhanced diagnostic logs</span></span>

<span data-ttu-id="88e60-395">ASP.NET Core 模組提供者是可設定的，以提供增強型診斷記錄。</span><span class="sxs-lookup"><span data-stu-id="88e60-395">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="88e60-396">將 `<handlerSettings>` 項目新增至 *web.config* 中的 `<aspNetCore>` 項目。將 `debugLevel` 設定為 `TRACE` 會公開精確性更高的診斷資訊：</span><span class="sxs-lookup"><span data-stu-id="88e60-396">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="debugFile" value="aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="88e60-397">偵錯層級 (`debugLevel`) 值可以同時包含層級和位置。</span><span class="sxs-lookup"><span data-stu-id="88e60-397">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="88e60-398">層級 (順序從最不詳細到最詳細)：</span><span class="sxs-lookup"><span data-stu-id="88e60-398">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="88e60-399">ERROR</span><span class="sxs-lookup"><span data-stu-id="88e60-399">ERROR</span></span>
* <span data-ttu-id="88e60-400">WARNING</span><span class="sxs-lookup"><span data-stu-id="88e60-400">WARNING</span></span>
* <span data-ttu-id="88e60-401">INFO</span><span class="sxs-lookup"><span data-stu-id="88e60-401">INFO</span></span>
* <span data-ttu-id="88e60-402">TRACE</span><span class="sxs-lookup"><span data-stu-id="88e60-402">TRACE</span></span>

<span data-ttu-id="88e60-403">位置 (允許多個位置)：</span><span class="sxs-lookup"><span data-stu-id="88e60-403">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="88e60-404">主控台</span><span class="sxs-lookup"><span data-stu-id="88e60-404">CONSOLE</span></span>
* <span data-ttu-id="88e60-405">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="88e60-405">EVENTLOG</span></span>
* <span data-ttu-id="88e60-406">檔案</span><span class="sxs-lookup"><span data-stu-id="88e60-406">FILE</span></span>

<span data-ttu-id="88e60-407">也可以透過環境變數提供處理常式設定：</span><span class="sxs-lookup"><span data-stu-id="88e60-407">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="88e60-408">偵錯記錄檔的 `ASPNETCORE_MODULE_DEBUG_FILE` &ndash; 路徑。</span><span class="sxs-lookup"><span data-stu-id="88e60-408">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="88e60-409">(預設：*aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="88e60-409">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="88e60-410">`ASPNETCORE_MODULE_DEBUG` &ndash; 偵錯層級設定。</span><span class="sxs-lookup"><span data-stu-id="88e60-410">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="88e60-411">在部署中保持啟用偵錯記錄的時間，**不要**超過針對問題進行排解疑難所需的時間。</span><span class="sxs-lookup"><span data-stu-id="88e60-411">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="88e60-412">記錄的大小不受限制。</span><span class="sxs-lookup"><span data-stu-id="88e60-412">The size of the log isn't limited.</span></span> <span data-ttu-id="88e60-413">保持啟用偵錯記錄可能會耗盡可用磁碟空間，並讓伺服器或應用程式服務當機。</span><span class="sxs-lookup"><span data-stu-id="88e60-413">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="88e60-414">如需 *web.config* 檔案中 `aspNetCore` 元素的範例，請參閱[使用 web.config 進行設定](#configuration-with-webconfig)。</span><span class="sxs-lookup"><span data-stu-id="88e60-414">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="88e60-415">Proxy 組態使用 HTTP 通訊協定和配對權杖</span><span class="sxs-lookup"><span data-stu-id="88e60-415">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="88e60-416">僅適用於跨處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="88e60-416">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="88e60-417">在 ASP.NET Core 模組與 Kestrel 之間建立的 Proxy 會使用 HTTP 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="88e60-417">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="88e60-418">使用 HTTP 是一項效能最佳化作業，其中模組與 Kestrel 之間的流量會在網路介面外的回送位址進行。</span><span class="sxs-lookup"><span data-stu-id="88e60-418">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="88e60-419">沒有從伺服器外的位置竊聽模組與 Kestrel 之間流量的風險。</span><span class="sxs-lookup"><span data-stu-id="88e60-419">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="88e60-420">配對權杖用來保證 Kestrel 所接收的要求已由 IIS 代理，而且不是來自其他來源。</span><span class="sxs-lookup"><span data-stu-id="88e60-420">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="88e60-421">模組會建立配對權杖，並將其設定成環境變數 (`ASPNETCORE_TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="88e60-421">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="88e60-422">配對權杖也會設定成每個代理要求的標頭 (`MS-ASPNETCORE-TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="88e60-422">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="88e60-423">IIS 中介軟體會檢查其收到的每個要求，以確認配對權杖的標頭值符合環境變數值。</span><span class="sxs-lookup"><span data-stu-id="88e60-423">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="88e60-424">如果權杖值不相符，將記錄並拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="88e60-424">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="88e60-425">使用者無法從伺服器外的位置存取配對權杖環境變數，以及模組與 Kestrel 之間的流量。</span><span class="sxs-lookup"><span data-stu-id="88e60-425">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="88e60-426">在不知道配對權杖值的情況下，攻擊者無法略過 IIS 中介軟體的檢查送出要求。</span><span class="sxs-lookup"><span data-stu-id="88e60-426">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="88e60-427">具有 IIS 共用設定的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="88e60-427">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="88e60-428">ASP.NET Core 模組安裝程式會以 **SYSTEM** 帳戶的權限執行。</span><span class="sxs-lookup"><span data-stu-id="88e60-428">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="88e60-429">由於本機系統帳戶並不具備「IIS 共用設定」所使用共用路徑的修改權限，因此安裝程式在嘗試於共用上的 *applicationHost.config* 中進行模組設定時，會發生存取被拒錯誤。</span><span class="sxs-lookup"><span data-stu-id="88e60-429">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="88e60-430">使用「IIS 共用設定」時，請依照下列步驟進行操作：</span><span class="sxs-lookup"><span data-stu-id="88e60-430">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="88e60-431">停用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="88e60-431">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="88e60-432">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="88e60-432">Run the installer.</span></span>
1. <span data-ttu-id="88e60-433">將已更新的 *applicationHost.config* 檔案匯出到共用。</span><span class="sxs-lookup"><span data-stu-id="88e60-433">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="88e60-434">重新啟用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="88e60-434">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="88e60-435">模組版本和裝載套件組合安裝程式記錄檔</span><span class="sxs-lookup"><span data-stu-id="88e60-435">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="88e60-436">判斷已安裝的 ASP.NET Core 模組版本：</span><span class="sxs-lookup"><span data-stu-id="88e60-436">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="88e60-437">在主控系統上，瀏覽至 *%windir%\System32\inetsrv*。</span><span class="sxs-lookup"><span data-stu-id="88e60-437">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="88e60-438">找出 *aspnetcore.dll* 檔案。</span><span class="sxs-lookup"><span data-stu-id="88e60-438">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="88e60-439">在該檔案上按一下滑鼠右鍵，然後從關聯式功能表中選取 [內容]。</span><span class="sxs-lookup"><span data-stu-id="88e60-439">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="88e60-440">選取 [詳細資料] 索引標籤。[檔案版本] 和 [產品版本] 代表已安裝的模組版本。</span><span class="sxs-lookup"><span data-stu-id="88e60-440">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="88e60-441">模組的「裝載套件組合」安裝程式記錄檔位於 *C:\\Users\\%UserName%\\AppData\\Local\\Temp*。檔案的名稱為 *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*。</span><span class="sxs-lookup"><span data-stu-id="88e60-441">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="88e60-442">模組、結構描述及設定檔位置</span><span class="sxs-lookup"><span data-stu-id="88e60-442">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="88e60-443">Module</span><span class="sxs-lookup"><span data-stu-id="88e60-443">Module</span></span>

<span data-ttu-id="88e60-444">**IIS (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="88e60-444">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="88e60-445">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="88e60-445">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="88e60-446">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="88e60-446">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="88e60-447">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="88e60-447">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="88e60-448">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="88e60-448">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="88e60-449">**IIS Express (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="88e60-449">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="88e60-450">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="88e60-450">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="88e60-451">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="88e60-451">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="88e60-452">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="88e60-452">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="88e60-453">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="88e60-453">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="88e60-454">結構描述</span><span class="sxs-lookup"><span data-stu-id="88e60-454">Schema</span></span>

<span data-ttu-id="88e60-455">**IIS**</span><span class="sxs-lookup"><span data-stu-id="88e60-455">**IIS**</span></span>

   * <span data-ttu-id="88e60-456">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="88e60-456">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="88e60-457">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="88e60-457">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="88e60-458">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="88e60-458">**IIS Express**</span></span>

   * <span data-ttu-id="88e60-459">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="88e60-459">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="88e60-460">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="88e60-460">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="88e60-461">Configuration</span><span class="sxs-lookup"><span data-stu-id="88e60-461">Configuration</span></span>

<span data-ttu-id="88e60-462">**IIS**</span><span class="sxs-lookup"><span data-stu-id="88e60-462">**IIS**</span></span>

   * <span data-ttu-id="88e60-463">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="88e60-463">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="88e60-464">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="88e60-464">**IIS Express**</span></span>

   * <span data-ttu-id="88e60-465">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="88e60-465">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>
   
   * <span data-ttu-id="88e60-466">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="88e60-466">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="88e60-467">在 *applicationHost.config* 檔案中搜尋 *aspnetcore*，即可找到這些檔案。</span><span class="sxs-lookup"><span data-stu-id="88e60-467">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="88e60-468">其他資源</span><span class="sxs-lookup"><span data-stu-id="88e60-468">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="88e60-469">ASP.NET Core 模組 GitHub 存放庫 (參考來源)</span><span class="sxs-lookup"><span data-stu-id="88e60-469">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
