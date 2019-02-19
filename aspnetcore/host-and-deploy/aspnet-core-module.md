---
title: ASP.NET Core 模組
author: guardrex
description: 了解如何設定 ASP.NET Core 模組以裝載 ASP.NET Core 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 02/08/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 9270d7b462bbac1ae0ad896c0937ea6dd909b2cd
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159551"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="93561-103">ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="93561-103">ASP.NET Core Module</span></span>

<span data-ttu-id="93561-104">作者：[Tom Dykstra](https://github.com/tdykstra)、[Rick Strahl](https://github.com/RickStrahl)、[Chris Ross](https://github.com/Tratcher)、[Rick Anderson](https://twitter.com/RickAndMSFT)、[Sourabh Shirhatti](https://twitter.com/sshirhatti)、[Justin Kotalik](https://github.com/jkotalik) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="93561-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="93561-105">ASP.NET Core 模組是一種原生 IIS 模組，可外掛至 IIS 管線以便：</span><span class="sxs-lookup"><span data-stu-id="93561-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="93561-106">在 IIS 工作者處理序 (`w3wp.exe`) 中裝載 ASP.NET Core 應用程式 (稱為[同處理序裝載模型](#in-process-hosting-model))。</span><span class="sxs-lookup"><span data-stu-id="93561-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="93561-107">將 Web 要求轉送到執行 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的後端 ASP.NET Core 應用程式 (稱為[跨處理序裝載模型](#out-of-process-hosting-model))。</span><span class="sxs-lookup"><span data-stu-id="93561-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="93561-108">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="93561-108">Supported Windows versions:</span></span>

* <span data-ttu-id="93561-109">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="93561-109">Windows 7 or later</span></span>
* <span data-ttu-id="93561-110">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="93561-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="93561-111">同處理序裝載時，模組會使用 IIS 的同處理序伺服程式實作，稱為 IIS HTTP 伺服器 (`IISHttpServer`)。</span><span class="sxs-lookup"><span data-stu-id="93561-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="93561-112">跨處理序裝載時，該模組只適用於 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="93561-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="93561-113">該模組與 [HTTP.sys](xref:fundamentals/servers/httpsys) 不相容。</span><span class="sxs-lookup"><span data-stu-id="93561-113">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="93561-114">裝載模型</span><span class="sxs-lookup"><span data-stu-id="93561-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="93561-115">同處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="93561-115">In-process hosting model</span></span>

<span data-ttu-id="93561-116">若要設定同處理序裝載的應用程式，請將 `<AspNetCoreHostingModel>` 屬性新增至應用程式的專案檔，其值為 `InProcess` (跨處理序裝載是使用 `OutOfProcess` 設定)：</span><span class="sxs-lookup"><span data-stu-id="93561-116">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="93561-117">以 .NET Framework 為目標的 ASP.NET Core 應用程式不支援處理序內裝載模型。</span><span class="sxs-lookup"><span data-stu-id="93561-117">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="93561-118">如果檔案中沒有 `<AspNetCoreHostingModel>` 屬性，預設值為 `OutOfProcess`。</span><span class="sxs-lookup"><span data-stu-id="93561-118">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="93561-119">同處理序裝載時具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="93561-119">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="93561-120">使用 IIS HTTP 伺服器 (`IISHttpServer`) 而不是 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器。</span><span class="sxs-lookup"><span data-stu-id="93561-120">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="93561-121">針對同處理序，[CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> 來：</span><span class="sxs-lookup"><span data-stu-id="93561-121">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="93561-122">註冊 `IISHttpServer`。</span><span class="sxs-lookup"><span data-stu-id="93561-122">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="93561-123">設定伺服器在 ASP.NET Core 模組後方執行時應該接聽的連接埠和基底路徑。</span><span class="sxs-lookup"><span data-stu-id="93561-123">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="93561-124">設定主機以擷取啟動錯誤。</span><span class="sxs-lookup"><span data-stu-id="93561-124">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="93561-125">[requestTimeout 屬性](#attributes-of-the-aspnetcore-element)不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="93561-125">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="93561-126">不支援在應用程式之間共用應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="93561-126">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="93561-127">每個應用程式使用一個應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="93561-127">Use one app pool per app.</span></span>

* <span data-ttu-id="93561-128">使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 或以手動方式[將 app_offline.htm 檔案放入部署](xref:host-and-deploy/iis/index#locked-deployment-files)時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="93561-128">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="93561-129">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="93561-129">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="93561-130">應用程式的架構 (位元) 和已安裝的執行階段 (x64 或 x86) 必須符合應用程式集區的架構。</span><span class="sxs-lookup"><span data-stu-id="93561-130">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="93561-131">如果使用 `WebHostBuilder` (而不是使用 [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) 以手動方式設定應用程式的主機，而且曾在 Kestrel 伺服器上直接執行應用程式 (自我裝載)，請先呼叫 `UseKestrel`，再呼叫 `UseIISIntegration`。</span><span class="sxs-lookup"><span data-stu-id="93561-131">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="93561-132">如果順序相反，就無法啟動主機。</span><span class="sxs-lookup"><span data-stu-id="93561-132">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="93561-133">偵測到用戶端中斷連線。</span><span class="sxs-lookup"><span data-stu-id="93561-133">Client disconnects are detected.</span></span> <span data-ttu-id="93561-134">用戶端中斷連線時，會取消 [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) 取消權杖。</span><span class="sxs-lookup"><span data-stu-id="93561-134">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="93561-135"><xref:System.IO.Directory.GetCurrentDirectory*> 會傳回 IIS 所啟動處理序的背景工作目錄，而非應用程式目錄 (例如 *w3wp.exe* 為 *C:\Windows\System32\inetsrv*)。</span><span class="sxs-lookup"><span data-stu-id="93561-135"><xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="93561-136">如需設定應用程式目前所在目錄的範例程式碼，請參閱 [CurrentDirectoryHelpers 類別](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs)。</span><span class="sxs-lookup"><span data-stu-id="93561-136">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="93561-137">呼叫 `SetCurrentDirectory` 方法。</span><span class="sxs-lookup"><span data-stu-id="93561-137">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="93561-138">後續呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 會提供應用程式的目錄。</span><span class="sxs-lookup"><span data-stu-id="93561-138">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="93561-139">跨處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="93561-139">Out-of-process hosting model</span></span>

<span data-ttu-id="93561-140">若要設定跨處理序裝載的應用程式，請在專案檔中使用下列任一方法：</span><span class="sxs-lookup"><span data-stu-id="93561-140">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="93561-141">請勿指定 `<AspNetCoreHostingModel>` 屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-141">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="93561-142">如果檔案中沒有 `<AspNetCoreHostingModel>` 屬性，預設值為 `OutOfProcess`。</span><span class="sxs-lookup"><span data-stu-id="93561-142">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="93561-143">將 `<AspNetCoreHostingModel>` 屬性的值設定為 `OutOfProcess` (同處理序裝載是使用 `InProcess` 設定)：</span><span class="sxs-lookup"><span data-stu-id="93561-143">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="93561-144">使用 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器而不是 IIS HTTP 伺服器 (`IISHttpServer`)。</span><span class="sxs-lookup"><span data-stu-id="93561-144">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="93561-145">針對跨處理序，[CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 來：</span><span class="sxs-lookup"><span data-stu-id="93561-145">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="93561-146">設定伺服器在 ASP.NET Core 模組後方執行時應該接聽的連接埠和基底路徑。</span><span class="sxs-lookup"><span data-stu-id="93561-146">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="93561-147">設定主機以擷取啟動錯誤。</span><span class="sxs-lookup"><span data-stu-id="93561-147">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="93561-148">裝載模型變更</span><span class="sxs-lookup"><span data-stu-id="93561-148">Hosting model changes</span></span>

<span data-ttu-id="93561-149">如果 `hostingModel` 設定在 *web.config* 檔案中已有所變更 (如[使用 web.config 進行設定](#configuration-with-webconfig)一節中所說明)，模組會回收 IIS 的工作者處理序。</span><span class="sxs-lookup"><span data-stu-id="93561-149">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="93561-150">針對 IIS Express，模組不會回收工作者處理序，但會改為觸發目前 IIS Express 處理序的正常關閉。</span><span class="sxs-lookup"><span data-stu-id="93561-150">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="93561-151">應用程式的下一個要求會繁衍新的 IIS Express 處理序。</span><span class="sxs-lookup"><span data-stu-id="93561-151">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="93561-152">處理序名稱</span><span class="sxs-lookup"><span data-stu-id="93561-152">Process name</span></span>

<span data-ttu-id="93561-153">`Process.GetCurrentProcess().ProcessName` 會報告 `w3wp`/`iisexpress` (同處理序) 或 `dotnet` (跨處理序)。</span><span class="sxs-lookup"><span data-stu-id="93561-153">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="93561-154">ASP.NET Core 模組是一種原生 IIS 模組，可外掛至 IIS 管線，將 Web 要求重新轉送到後端的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="93561-154">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="93561-155">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="93561-155">Supported Windows versions:</span></span>

* <span data-ttu-id="93561-156">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="93561-156">Windows 7 or later</span></span>
* <span data-ttu-id="93561-157">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="93561-157">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="93561-158">該模組只適用於 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="93561-158">The module only works with Kestrel.</span></span> <span data-ttu-id="93561-159">該模組與 [HTTP.sys](xref:fundamentals/servers/httpsys) 不相容。</span><span class="sxs-lookup"><span data-stu-id="93561-159">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="93561-160">因為處理序中執行的 ASP.NET Core 應用程式會與 IIS 背景工作處理序分開，所以此模組也會執行處理序管理。</span><span class="sxs-lookup"><span data-stu-id="93561-160">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="93561-161">此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="93561-161">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="93561-162">此行為基本上與在 IIS 中執行同處理序，並由 [Windows 處理器啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的 ASP.NET 4.x 應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="93561-162">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="93561-163">下圖說明 IIS、ASP.NET Core 模組和應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="93561-163">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![ASP.NET Core 模組](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="93561-165">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="93561-165">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="93561-166">驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="93561-166">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="93561-167">此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，而且不會是通訊埠 80 或 443。</span><span class="sxs-lookup"><span data-stu-id="93561-167">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="93561-168">此模組在啟動時透過環境變數指定通訊埠，而 IIS 整合中介軟體則會設定伺服器來接聽 `http://localhost:{port}`。</span><span class="sxs-lookup"><span data-stu-id="93561-168">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="93561-169">將會執行額外檢查，不是源自模組的要求都會遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="93561-169">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="93561-170">此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。</span><span class="sxs-lookup"><span data-stu-id="93561-170">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="93561-171">Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="93561-171">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="93561-172">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="93561-172">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="93561-173">IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="93561-173">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="93561-174">應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="93561-174">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="93561-175">許多如 Windows 驗證等原生模組仍在使用中。</span><span class="sxs-lookup"><span data-stu-id="93561-175">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="93561-176">若要深入了解搭配 ASP.NET Core 模組的使用中 IIS 模組，請參閱<xref:host-and-deploy/iis/modules>。</span><span class="sxs-lookup"><span data-stu-id="93561-176">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="93561-177">ASP.NET Core 模組也可以：</span><span class="sxs-lookup"><span data-stu-id="93561-177">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="93561-178">設定背景工作處理序的環境變數。</span><span class="sxs-lookup"><span data-stu-id="93561-178">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="93561-179">將 stdout 輸出記錄到檔案儲存區，以針對啟動問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="93561-179">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="93561-180">轉送 Windows 驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="93561-180">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="93561-181">如何安裝和使用 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="93561-181">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="93561-182">如需如何安裝和使用 ASP.NET Core 模組的指示，請參閱<xref:host-and-deploy/iis/index>。</span><span class="sxs-lookup"><span data-stu-id="93561-182">For instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="93561-183">使用 web.config 進行設定</span><span class="sxs-lookup"><span data-stu-id="93561-183">Configuration with web.config</span></span>

<span data-ttu-id="93561-184">設定 ASP.NET Core 模組時，是使用網站 *web.config* 檔案中 `system.webServer` 節點的 `aspNetCore` 區段來設定。</span><span class="sxs-lookup"><span data-stu-id="93561-184">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="93561-185">以下 *web.config* 檔案是針對[架構相依部署](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)發佈的檔案，會設定 ASP.NET Core 模組來處理網站要求：</span><span class="sxs-lookup"><span data-stu-id="93561-185">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="93561-186">以下 *web.config* 是針對[自封式部署](/dotnet/articles/core/deploying/#self-contained-deployments-scd)發佈的檔案：</span><span class="sxs-lookup"><span data-stu-id="93561-186">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="93561-187">將 <xref:System.Configuration.SectionInformation.InheritInChildApplications*> 屬性設定為 `false`，以表示在 [\<位置>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) 內指定的設定，不是由位在應用程式子目錄中的應用程式所繼承。</span><span class="sxs-lookup"><span data-stu-id="93561-187">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

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

<span data-ttu-id="93561-188">將應用程式部署至 [Azure App Service](https://azure.microsoft.com/services/app-service/) 時，`stdoutLogFile` 路徑會設定為 `\\?\%home%\LogFiles\stdout`。</span><span class="sxs-lookup"><span data-stu-id="93561-188">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="93561-189">此路徑會將 stdout 記錄檔儲存至 [LogFiles] 資料夾，這是服務自動建立的位置。</span><span class="sxs-lookup"><span data-stu-id="93561-189">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="93561-190">如需有關 IIS 子應用程式設定的詳細資訊，請參閱 <xref:host-and-deploy/iis/index#sub-applications>。</span><span class="sxs-lookup"><span data-stu-id="93561-190">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="93561-191">aspNetCore 元素的屬性</span><span class="sxs-lookup"><span data-stu-id="93561-191">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="93561-192">屬性</span><span class="sxs-lookup"><span data-stu-id="93561-192">Attribute</span></span> | <span data-ttu-id="93561-193">描述</span><span class="sxs-lookup"><span data-stu-id="93561-193">Description</span></span> | <span data-ttu-id="93561-194">預設</span><span class="sxs-lookup"><span data-stu-id="93561-194">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="93561-195">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-195">Optional string attribute.</span></span></p><p><span data-ttu-id="93561-196">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="93561-196">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="93561-197">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-197">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="93561-198">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="93561-198">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="93561-199">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-199">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="93561-200">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="93561-200">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="93561-201">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="93561-201">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="93561-202">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-202">Optional string attribute.</span></span></p><p><span data-ttu-id="93561-203">將裝載模型指定為同處理序 (`InProcess`) 或跨處理序 (`OutOfProcess`)。</span><span class="sxs-lookup"><span data-stu-id="93561-203">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="93561-204">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-204">Optional integer attribute.</span></span></p><p><span data-ttu-id="93561-205">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="93561-205">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="93561-206">&dagger;針對同處理序裝載，此值會限制為 `1`。</span><span class="sxs-lookup"><span data-stu-id="93561-206">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="93561-207">預設值：`1`</span><span class="sxs-lookup"><span data-stu-id="93561-207">Default: `1`</span></span><br><span data-ttu-id="93561-208">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="93561-208">Min: `1`</span></span><br><span data-ttu-id="93561-209">最大值：`100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="93561-209">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="93561-210">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-210">Required string attribute.</span></span></p><p><span data-ttu-id="93561-211">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="93561-211">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="93561-212">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="93561-212">Relative paths are supported.</span></span> <span data-ttu-id="93561-213">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="93561-213">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="93561-214">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-214">Optional integer attribute.</span></span></p><p><span data-ttu-id="93561-215">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="93561-215">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="93561-216">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="93561-216">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="93561-217">不支援同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="93561-217">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="93561-218">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="93561-218">Default: `10`</span></span><br><span data-ttu-id="93561-219">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="93561-219">Min: `0`</span></span><br><span data-ttu-id="93561-220">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="93561-220">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="93561-221">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-221">Optional timespan attribute.</span></span></p><p><span data-ttu-id="93561-222">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="93561-222">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="93561-223">在 ASP.NET Core 2.1 或更新版本隨附的 ASP.NET Core 模組版本中，是以小時、分鐘及秒為單位來指定 `requestTimeout`。</span><span class="sxs-lookup"><span data-stu-id="93561-223">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="93561-224">不適用於同處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="93561-224">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="93561-225">針對同處理序裝載，該模組會等待應用程式處理要求。</span><span class="sxs-lookup"><span data-stu-id="93561-225">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="93561-226">預設值：`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="93561-226">Default: `00:02:00`</span></span><br><span data-ttu-id="93561-227">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="93561-227">Min: `00:00:00`</span></span><br><span data-ttu-id="93561-228">最大值︰`360:00:00`</span><span class="sxs-lookup"><span data-stu-id="93561-228">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="93561-229">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-229">Optional integer attribute.</span></span></p><p><span data-ttu-id="93561-230">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="93561-230">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="93561-231">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="93561-231">Default: `10`</span></span><br><span data-ttu-id="93561-232">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="93561-232">Min: `0`</span></span><br><span data-ttu-id="93561-233">最大值︰`600`</span><span class="sxs-lookup"><span data-stu-id="93561-233">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="93561-234">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-234">Optional integer attribute.</span></span></p><p><span data-ttu-id="93561-235">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="93561-235">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="93561-236">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="93561-236">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="93561-237">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="93561-237">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="93561-238">0 (零) 值**不會**視為無限逾時。</span><span class="sxs-lookup"><span data-stu-id="93561-238">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="93561-239">預設值：`120`</span><span class="sxs-lookup"><span data-stu-id="93561-239">Default: `120`</span></span><br><span data-ttu-id="93561-240">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="93561-240">Min: `0`</span></span><br><span data-ttu-id="93561-241">最大值︰`3600`</span><span class="sxs-lookup"><span data-stu-id="93561-241">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="93561-242">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-242">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="93561-243">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="93561-243">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="93561-244">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-244">Optional string attribute.</span></span></p><p><span data-ttu-id="93561-245">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="93561-245">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="93561-246">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="93561-246">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="93561-247">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="93561-247">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="93561-248">建立記錄檔後，模組會建立路徑中提供的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="93561-248">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="93561-249">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 (*.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="93561-249">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="93561-250">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="93561-250">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="93561-251">屬性</span><span class="sxs-lookup"><span data-stu-id="93561-251">Attribute</span></span> | <span data-ttu-id="93561-252">描述</span><span class="sxs-lookup"><span data-stu-id="93561-252">Description</span></span> | <span data-ttu-id="93561-253">預設</span><span class="sxs-lookup"><span data-stu-id="93561-253">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="93561-254">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-254">Optional string attribute.</span></span></p><p><span data-ttu-id="93561-255">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="93561-255">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="93561-256">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-256">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="93561-257">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="93561-257">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="93561-258">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-258">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="93561-259">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="93561-259">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="93561-260">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="93561-260">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="93561-261">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-261">Optional integer attribute.</span></span></p><p><span data-ttu-id="93561-262">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="93561-262">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="93561-263">預設值：`1`</span><span class="sxs-lookup"><span data-stu-id="93561-263">Default: `1`</span></span><br><span data-ttu-id="93561-264">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="93561-264">Min: `1`</span></span><br><span data-ttu-id="93561-265">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="93561-265">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="93561-266">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-266">Required string attribute.</span></span></p><p><span data-ttu-id="93561-267">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="93561-267">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="93561-268">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="93561-268">Relative paths are supported.</span></span> <span data-ttu-id="93561-269">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="93561-269">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="93561-270">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-270">Optional integer attribute.</span></span></p><p><span data-ttu-id="93561-271">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="93561-271">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="93561-272">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="93561-272">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="93561-273">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="93561-273">Default: `10`</span></span><br><span data-ttu-id="93561-274">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="93561-274">Min: `0`</span></span><br><span data-ttu-id="93561-275">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="93561-275">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="93561-276">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-276">Optional timespan attribute.</span></span></p><p><span data-ttu-id="93561-277">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="93561-277">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="93561-278">在 ASP.NET Core 2.1 或更新版本隨附的 ASP.NET Core 模組版本中，是以小時、分鐘及秒為單位來指定 `requestTimeout`。</span><span class="sxs-lookup"><span data-stu-id="93561-278">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="93561-279">預設值：`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="93561-279">Default: `00:02:00`</span></span><br><span data-ttu-id="93561-280">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="93561-280">Min: `00:00:00`</span></span><br><span data-ttu-id="93561-281">最大值︰`360:00:00`</span><span class="sxs-lookup"><span data-stu-id="93561-281">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="93561-282">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-282">Optional integer attribute.</span></span></p><p><span data-ttu-id="93561-283">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="93561-283">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="93561-284">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="93561-284">Default: `10`</span></span><br><span data-ttu-id="93561-285">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="93561-285">Min: `0`</span></span><br><span data-ttu-id="93561-286">最大值︰`600`</span><span class="sxs-lookup"><span data-stu-id="93561-286">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="93561-287">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-287">Optional integer attribute.</span></span></p><p><span data-ttu-id="93561-288">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="93561-288">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="93561-289">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="93561-289">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="93561-290">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="93561-290">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="93561-291">0 (零) 值**不會**視為無限逾時。</span><span class="sxs-lookup"><span data-stu-id="93561-291">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="93561-292">預設值：`120`</span><span class="sxs-lookup"><span data-stu-id="93561-292">Default: `120`</span></span><br><span data-ttu-id="93561-293">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="93561-293">Min: `0`</span></span><br><span data-ttu-id="93561-294">最大值︰`3600`</span><span class="sxs-lookup"><span data-stu-id="93561-294">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="93561-295">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-295">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="93561-296">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="93561-296">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="93561-297">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-297">Optional string attribute.</span></span></p><p><span data-ttu-id="93561-298">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="93561-298">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="93561-299">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="93561-299">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="93561-300">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="93561-300">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="93561-301">路徑中提供的所有資料夾都必須存在，模組才能建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="93561-301">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="93561-302">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 (*.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="93561-302">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="93561-303">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="93561-303">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="93561-304">屬性</span><span class="sxs-lookup"><span data-stu-id="93561-304">Attribute</span></span> | <span data-ttu-id="93561-305">描述</span><span class="sxs-lookup"><span data-stu-id="93561-305">Description</span></span> | <span data-ttu-id="93561-306">預設</span><span class="sxs-lookup"><span data-stu-id="93561-306">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="93561-307">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-307">Optional string attribute.</span></span></p><p><span data-ttu-id="93561-308">**processPath** 中所指定可執行檔的引數。</span><span class="sxs-lookup"><span data-stu-id="93561-308">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="93561-309">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-309">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="93561-310">如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="93561-310">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="93561-311">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-311">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="93561-312">如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。</span><span class="sxs-lookup"><span data-stu-id="93561-312">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="93561-313">該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</span><span class="sxs-lookup"><span data-stu-id="93561-313">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="93561-314">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-314">Optional integer attribute.</span></span></p><p><span data-ttu-id="93561-315">指定 **processPath** 設定中所指定處理序執行個體每個應用程式可上調的數目。</span><span class="sxs-lookup"><span data-stu-id="93561-315">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="93561-316">預設值：`1`</span><span class="sxs-lookup"><span data-stu-id="93561-316">Default: `1`</span></span><br><span data-ttu-id="93561-317">最小值：`1`</span><span class="sxs-lookup"><span data-stu-id="93561-317">Min: `1`</span></span><br><span data-ttu-id="93561-318">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="93561-318">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="93561-319">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-319">Required string attribute.</span></span></p><p><span data-ttu-id="93561-320">啟動接聽 HTTP 要求之處理序的可執行檔路徑。</span><span class="sxs-lookup"><span data-stu-id="93561-320">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="93561-321">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="93561-321">Relative paths are supported.</span></span> <span data-ttu-id="93561-322">如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="93561-322">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="93561-323">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-323">Optional integer attribute.</span></span></p><p><span data-ttu-id="93561-324">指定允許 **processPath** 中所指定處理序每分鐘當機的次數。</span><span class="sxs-lookup"><span data-stu-id="93561-324">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="93561-325">如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</span><span class="sxs-lookup"><span data-stu-id="93561-325">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="93561-326">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="93561-326">Default: `10`</span></span><br><span data-ttu-id="93561-327">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="93561-327">Min: `0`</span></span><br><span data-ttu-id="93561-328">最大值︰`100`</span><span class="sxs-lookup"><span data-stu-id="93561-328">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="93561-329">選擇性的時間範圍屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-329">Optional timespan attribute.</span></span></p><p><span data-ttu-id="93561-330">針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</span><span class="sxs-lookup"><span data-stu-id="93561-330">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="93561-331">在 ASP.NET Core 2.0 或更舊版本隨附的 ASP.NET Core 模組版本中，只能以整數分鐘為單位來指定 `requestTimeout`，否則會預設為 2 分鐘。</span><span class="sxs-lookup"><span data-stu-id="93561-331">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="93561-332">預設值：`00:02:00`</span><span class="sxs-lookup"><span data-stu-id="93561-332">Default: `00:02:00`</span></span><br><span data-ttu-id="93561-333">最小值：`00:00:00`</span><span class="sxs-lookup"><span data-stu-id="93561-333">Min: `00:00:00`</span></span><br><span data-ttu-id="93561-334">最大值︰`360:00:00`</span><span class="sxs-lookup"><span data-stu-id="93561-334">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="93561-335">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-335">Optional integer attribute.</span></span></p><p><span data-ttu-id="93561-336">當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="93561-336">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="93561-337">預設值：`10`</span><span class="sxs-lookup"><span data-stu-id="93561-337">Default: `10`</span></span><br><span data-ttu-id="93561-338">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="93561-338">Min: `0`</span></span><br><span data-ttu-id="93561-339">最大值︰`600`</span><span class="sxs-lookup"><span data-stu-id="93561-339">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="93561-340">選擇性的整數屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-340">Optional integer attribute.</span></span></p><p><span data-ttu-id="93561-341">針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。</span><span class="sxs-lookup"><span data-stu-id="93561-341">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="93561-342">如果超出此時間限制，模組就會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="93561-342">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="93561-343">模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</span><span class="sxs-lookup"><span data-stu-id="93561-343">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="93561-344">預設值：`120`</span><span class="sxs-lookup"><span data-stu-id="93561-344">Default: `120`</span></span><br><span data-ttu-id="93561-345">最小值：`0`</span><span class="sxs-lookup"><span data-stu-id="93561-345">Min: `0`</span></span><br><span data-ttu-id="93561-346">最大值︰`3600`</span><span class="sxs-lookup"><span data-stu-id="93561-346">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="93561-347">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-347">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="93561-348">如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="93561-348">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="93561-349">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-349">Optional string attribute.</span></span></p><p><span data-ttu-id="93561-350">指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="93561-350">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="93561-351">相對路徑是相對於網站的根目錄。</span><span class="sxs-lookup"><span data-stu-id="93561-351">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="93561-352">所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="93561-352">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="93561-353">路徑中提供的所有資料夾都必須存在，模組才能建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="93561-353">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="93561-354">使用底線分隔符號，時間戳記、處理序識別碼及副檔名 (*.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。</span><span class="sxs-lookup"><span data-stu-id="93561-354">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="93561-355">如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="93561-355">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="93561-356">設定環境變數</span><span class="sxs-lookup"><span data-stu-id="93561-356">Setting environment variables</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="93561-357">您可以在 `processPath` 屬性中為處理序指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="93561-357">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="93561-358">請使用 `<environmentVariables>` 集合元素的 `<environmentVariable>` 子元素來指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="93561-358">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="93561-359">本節中所設定環境變數的優先順序會高於系統環境變數。</span><span class="sxs-lookup"><span data-stu-id="93561-359">Environment variables set in this section take precedence over system environment variables.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="93561-360">您可以在 `processPath` 屬性中為處理序指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="93561-360">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="93561-361">請使用 `<environmentVariables>` 集合元素的 `<environmentVariable>` 子元素來指定環境變數。</span><span class="sxs-lookup"><span data-stu-id="93561-361">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="93561-362">此節中所設定的環境變數與使用相同名稱設定的系統環境變數相衝突。</span><span class="sxs-lookup"><span data-stu-id="93561-362">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="93561-363">若同時在 *web.config* 檔案與 Windows 中的系統層級設定環境變數，來自 *web.config* 檔案的值會成為附加到系統環境變數值 (例如，`ASPNETCORE_ENVIRONMENT: Development;Development`)，這會造成應用程式無法啟動。</span><span class="sxs-lookup"><span data-stu-id="93561-363">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

::: moniker-end

<span data-ttu-id="93561-364">下列範例會設定兩個環境變數。</span><span class="sxs-lookup"><span data-stu-id="93561-364">The following example sets two environment variables.</span></span> <span data-ttu-id="93561-365">`ASPNETCORE_ENVIRONMENT` 會將應用程式的環境設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="93561-365">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="93561-366">開發人員可以在 *web.config* 檔案中暫時設定這個值，以在進行應用程式例外狀況偵錯時，強制[開發人員例外狀況頁面](xref:fundamentals/error-handling)載入。</span><span class="sxs-lookup"><span data-stu-id="93561-366">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="93561-367">`CONFIG_DIR` 是一個使用者定義的環境變數範例，其中開發人員已撰寫程式碼，會在啟動時讀取值來構成用以載入應用程式設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="93561-367">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="93561-368">在 *web.config* 中直接設定環境的替代方式是在發行設定檔 (*.pubxml*) 或專案檔中包括 `<EnvironmentName>` 屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-368">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="93561-369">此方法會在專案發行時於 *web.config* 中設定環境：</span><span class="sxs-lookup"><span data-stu-id="93561-369">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="93561-370">請只有在未受信任網路 (例如網際網路) 無法存取的暫存和測試伺服器上，才將 `ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`。</span><span class="sxs-lookup"><span data-stu-id="93561-370">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="93561-371">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="93561-371">app_offline.htm</span></span>

<span data-ttu-id="93561-372">如果在應用程式根目錄中偵測到名稱為 *app_offline.htm* 的檔案，ASP.NET Core Module 會嘗試正常關閉應用程式並停止處理連入的要求。</span><span class="sxs-lookup"><span data-stu-id="93561-372">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="93561-373">如果在經過 `shutdownTimeLimit` 中所定義的秒數之後，應用程式仍然在執行，ASP.NET Core Module 就會終止執行中的處理序。</span><span class="sxs-lookup"><span data-stu-id="93561-373">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="93561-374">當 *app_offline.htm* 檔案存在時，ASP.NET Core 模組會藉由傳回 *app_offline.htm* 檔案的內容來回應要求。</span><span class="sxs-lookup"><span data-stu-id="93561-374">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="93561-375">已移除 *app_offline.htm* 檔案時，下一個要求則會啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="93561-375">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="93561-376">使用跨處理序裝載模型時，若未開啟連線，應用程式可能無法立即關閉。</span><span class="sxs-lookup"><span data-stu-id="93561-376">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="93561-377">例如，WebSocket 連線可能會延遲應用程式關閉。</span><span class="sxs-lookup"><span data-stu-id="93561-377">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="93561-378">啟動錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="93561-378">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="93561-379">同處理序及跨處理序裝載無法啟動應用程式時，兩者皆會產生自訂錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="93561-379">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="93561-380">若 ASP.NET Core 模組找不到同處理序或跨處理序要求處理常式時，就會顯示 [500.0 - 同處理序/跨處理序處理常式載入失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="93561-380">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="93561-381">對於同處理序裝載，若 ASP.NET Core 模組無法啟動應用程式，就會顯示 [500.30 - 啟動失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="93561-381">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="93561-382">對於跨處理序裝載，若 ASP.NET Core 模組無法啟動後端處理序，或後端處理序啟動但無法在所設定的連接埠上進行接聽，就會顯示 [502.5 - 處理序失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="93561-382">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="93561-383">若要避免此頁面產生並還原至預設的 IIS 5xx 狀態碼頁面，請使用 `disableStartUpErrorPage` 屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-383">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="93561-384">如需設定自訂錯誤訊息的詳細資訊，請參閱 [HTTP 錯誤 \<httpErrors>](/iis/configuration/system.webServer/httpErrors/)。</span><span class="sxs-lookup"><span data-stu-id="93561-384">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="93561-385">如果 ASP.NET Core 模組無法啟動後端處理序，或後端處理序啟動但無法在所設定的連接埠上進行接聽，就會顯示 [502.5 - 處理序失敗] 狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="93561-385">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="93561-386">若要抑制此頁面並還原至預設的 IIS 502 狀態碼頁面，請使用 `disableStartUpErrorPage` 屬性。</span><span class="sxs-lookup"><span data-stu-id="93561-386">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="93561-387">如需設定自訂錯誤訊息的詳細資訊，請參閱 [HTTP 錯誤 \<httpErrors>](/iis/configuration/system.webServer/httpErrors/)。</span><span class="sxs-lookup"><span data-stu-id="93561-387">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 處理序失敗狀態碼頁面](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="93561-389">記錄檔建立和重新導向</span><span class="sxs-lookup"><span data-stu-id="93561-389">Log creation and redirection</span></span>

<span data-ttu-id="93561-390">如果已設定 `aspNetCore` 元素的 `stdoutLogEnabled` 和 `stdoutLogFile` 屬性，ASP.NET Core 模組就會將 stdout 和 stderr 主控台輸出重新導向到磁碟。</span><span class="sxs-lookup"><span data-stu-id="93561-390">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="93561-391">建立記錄檔後，模組會建立 `stdoutLogFile` 路徑中的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="93561-391">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="93561-392">應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。</span><span class="sxs-lookup"><span data-stu-id="93561-392">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="93561-393">除非發生處理序回收/重新啟動，否則不會輪替記錄檔。</span><span class="sxs-lookup"><span data-stu-id="93561-393">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="93561-394">主機服務提供者必須負責限制記錄檔所使用的磁碟空間。</span><span class="sxs-lookup"><span data-stu-id="93561-394">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="93561-395">建議只有在進行應用程式啟動問題疑難排解時，才使用 stdout 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="93561-395">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="93561-396">請勿將 stdout 記錄檔用來進行一般應用程式記錄。</span><span class="sxs-lookup"><span data-stu-id="93561-396">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="93561-397">針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。</span><span class="sxs-lookup"><span data-stu-id="93561-397">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="93561-398">如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="93561-398">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="93561-399">建立記錄檔時，系統會自動新增時間戳記和副檔名。</span><span class="sxs-lookup"><span data-stu-id="93561-399">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="93561-400">記錄檔名稱會藉由將時間戳記、處理序識別碼及副檔名 (*.log*) 以底線分隔並附加至 `stdoutLogFile` 路徑的最後一個區段 (通常是 *stdout*) 來組成。</span><span class="sxs-lookup"><span data-stu-id="93561-400">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="93561-401">如果 `stdoutLogFile` 路徑的結尾是 *stdout*，則在 2018 年 2 月 5 日 19:42:32 建立且 PID 為 1934 的應用程式記錄檔檔案名稱會是 *stdout_20180205194132_1934.log*。</span><span class="sxs-lookup"><span data-stu-id="93561-401">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="93561-402">若 `stdoutLogEnabled` 為 false，會擷取在應用程式啟動時發生的錯誤，並發出最大 30KB 的事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="93561-402">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="93561-403">啟動之後，就會捨棄其他的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="93561-403">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="93561-404">下列範例 `aspNetCore` 元素會設定 Azure App Service 中所裝載應用程式的 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="93561-404">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="93561-405">系統可接受使用本機路徑或網路共用路徑來進行本機記錄。</span><span class="sxs-lookup"><span data-stu-id="93561-405">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="93561-406">請確認 AppPool 使用者身分識別具備所提供路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="93561-406">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="93561-407">增強型診斷記錄</span><span class="sxs-lookup"><span data-stu-id="93561-407">Enhanced diagnostic logs</span></span>

<span data-ttu-id="93561-408">ASP.NET Core 模組是可設定的，以提供增強型診斷記錄。</span><span class="sxs-lookup"><span data-stu-id="93561-408">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="93561-409">將 `<handlerSettings>` 項目新增至 *web.config* 中的 `<aspNetCore>` 項目。將 `debugLevel` 設定為 `TRACE` 會公開精確性更高的診斷資訊：</span><span class="sxs-lookup"><span data-stu-id="93561-409">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="93561-410">偵錯層級 (`debugLevel`) 值可以同時包含層級和位置。</span><span class="sxs-lookup"><span data-stu-id="93561-410">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="93561-411">層級 (順序從最不詳細到最詳細)：</span><span class="sxs-lookup"><span data-stu-id="93561-411">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="93561-412">ERROR</span><span class="sxs-lookup"><span data-stu-id="93561-412">ERROR</span></span>
* <span data-ttu-id="93561-413">WARNING</span><span class="sxs-lookup"><span data-stu-id="93561-413">WARNING</span></span>
* <span data-ttu-id="93561-414">INFO</span><span class="sxs-lookup"><span data-stu-id="93561-414">INFO</span></span>
* <span data-ttu-id="93561-415">TRACE</span><span class="sxs-lookup"><span data-stu-id="93561-415">TRACE</span></span>

<span data-ttu-id="93561-416">位置 (允許多個位置)：</span><span class="sxs-lookup"><span data-stu-id="93561-416">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="93561-417">主控台</span><span class="sxs-lookup"><span data-stu-id="93561-417">CONSOLE</span></span>
* <span data-ttu-id="93561-418">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="93561-418">EVENTLOG</span></span>
* <span data-ttu-id="93561-419">檔案</span><span class="sxs-lookup"><span data-stu-id="93561-419">FILE</span></span>

<span data-ttu-id="93561-420">也可以透過環境變數提供處理常式設定：</span><span class="sxs-lookup"><span data-stu-id="93561-420">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="93561-421">偵錯記錄檔的 `ASPNETCORE_MODULE_DEBUG_FILE` &ndash; 路徑。</span><span class="sxs-lookup"><span data-stu-id="93561-421">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="93561-422">(預設：*aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="93561-422">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="93561-423">`ASPNETCORE_MODULE_DEBUG` &ndash; 偵錯層級設定。</span><span class="sxs-lookup"><span data-stu-id="93561-423">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="93561-424">在部署中保持啟用偵錯記錄的時間，**不要**超過針對問題進行排解疑難所需的時間。</span><span class="sxs-lookup"><span data-stu-id="93561-424">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="93561-425">記錄的大小不受限制。</span><span class="sxs-lookup"><span data-stu-id="93561-425">The size of the log isn't limited.</span></span> <span data-ttu-id="93561-426">保持啟用偵錯記錄可能會耗盡可用磁碟空間，並讓伺服器或應用程式服務當機。</span><span class="sxs-lookup"><span data-stu-id="93561-426">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="93561-427">如需 *web.config* 檔案中 `aspNetCore` 元素的範例，請參閱[使用 web.config 進行設定](#configuration-with-webconfig)。</span><span class="sxs-lookup"><span data-stu-id="93561-427">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="93561-428">Proxy 組態使用 HTTP 通訊協定和配對權杖</span><span class="sxs-lookup"><span data-stu-id="93561-428">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="93561-429">僅適用於跨處理序裝載。</span><span class="sxs-lookup"><span data-stu-id="93561-429">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="93561-430">在 ASP.NET Core 模組與 Kestrel 之間建立的 Proxy 會使用 HTTP 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="93561-430">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="93561-431">使用 HTTP 是一項效能最佳化作業，其中模組與 Kestrel 之間的流量會在網路介面外的回送位址進行。</span><span class="sxs-lookup"><span data-stu-id="93561-431">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="93561-432">沒有從伺服器外的位置竊聽模組與 Kestrel 之間流量的風險。</span><span class="sxs-lookup"><span data-stu-id="93561-432">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="93561-433">配對權杖用來保證 Kestrel 所接收的要求已由 IIS 代理，而且不是來自其他來源。</span><span class="sxs-lookup"><span data-stu-id="93561-433">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="93561-434">模組會建立配對權杖，並將其設定成環境變數 (`ASPNETCORE_TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="93561-434">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="93561-435">配對權杖也會設定成每個代理要求的標頭 (`MS-ASPNETCORE-TOKEN`)。</span><span class="sxs-lookup"><span data-stu-id="93561-435">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="93561-436">IIS 中介軟體會檢查其收到的每個要求，以確認配對權杖的標頭值符合環境變數值。</span><span class="sxs-lookup"><span data-stu-id="93561-436">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="93561-437">如果權杖值不相符，將記錄並拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="93561-437">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="93561-438">使用者無法從伺服器外的位置存取配對權杖環境變數，以及模組與 Kestrel 之間的流量。</span><span class="sxs-lookup"><span data-stu-id="93561-438">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="93561-439">在不知道配對權杖值的情況下，攻擊者無法略過 IIS 中介軟體的檢查送出要求。</span><span class="sxs-lookup"><span data-stu-id="93561-439">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="93561-440">具有 IIS 共用設定的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="93561-440">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="93561-441">ASP.NET Core 模組安裝程式會以 **SYSTEM** 帳戶的權限執行。</span><span class="sxs-lookup"><span data-stu-id="93561-441">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="93561-442">由於本機系統帳戶並不具備「IIS 共用設定」所使用共用路徑的修改權限，因此安裝程式在嘗試於共用上的 *applicationHost.config* 中進行模組設定時，會發生存取被拒錯誤。</span><span class="sxs-lookup"><span data-stu-id="93561-442">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="93561-443">使用「IIS 共用設定」時，請依照下列步驟進行操作：</span><span class="sxs-lookup"><span data-stu-id="93561-443">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="93561-444">停用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="93561-444">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="93561-445">執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="93561-445">Run the installer.</span></span>
1. <span data-ttu-id="93561-446">將已更新的 *applicationHost.config* 檔案匯出到共用。</span><span class="sxs-lookup"><span data-stu-id="93561-446">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="93561-447">重新啟用「IIS 共用設定」。</span><span class="sxs-lookup"><span data-stu-id="93561-447">Re-enable the IIS Shared Configuration.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="application-initialization"></a><span data-ttu-id="93561-448">應用程式初始化</span><span class="sxs-lookup"><span data-stu-id="93561-448">Application Initialization</span></span>

<span data-ttu-id="93561-449">[IIS 應用程式初始化](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)是一項 IIS 功能，可在應用程式集區啟動或回收時，將 HTTP 要求傳送給應用程式。</span><span class="sxs-lookup"><span data-stu-id="93561-449">[IIS Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) is an IIS feature that sends an HTTP request to the app when the app pool starts or is recycled.</span></span> <span data-ttu-id="93561-450">要求會觸發應用程式啟動。</span><span class="sxs-lookup"><span data-stu-id="93561-450">The request triggers the app to start.</span></span> <span data-ttu-id="93561-451">[同處理序裝載模型](xref:fundamentals/servers/index#in-process-hosting-model)和[跨處理序裝載模型](xref:fundamentals/servers/index#out-of-process-hosting-model)都可藉由 ASP.NET Core 模組第 2 版使用「應用程式初始化」。</span><span class="sxs-lookup"><span data-stu-id="93561-451">Application Initialization can be used by both the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model) and [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model) with the ASP.NET Core Module version 2.</span></span>

<span data-ttu-id="93561-452">啟用「應用程式初始化」：</span><span class="sxs-lookup"><span data-stu-id="93561-452">To enable Application Initialization:</span></span>

1. <span data-ttu-id="93561-453">確認已啟用「IIS 應用程式初始化」角色功能：</span><span class="sxs-lookup"><span data-stu-id="93561-453">Confirm that the IIS Application Initialization role feature in enabled:</span></span>
   * <span data-ttu-id="93561-454">在 Windows 7 或更新版本上：瀏覽至控制台 > [程式] > [程式和功能] > [開啟或關閉 Windows 功能] (畫面左側)。</span><span class="sxs-lookup"><span data-stu-id="93561-454">On Windows 7 or later: Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="93561-455">開啟 [網際網路資訊服務] > [World Wide Web 服務] > [應用程式開發功能]。</span><span class="sxs-lookup"><span data-stu-id="93561-455">Open **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="93561-456">選取 [應用程式初始化]的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="93561-456">Select the check box for **Application Initialization**.</span></span>
   * <span data-ttu-id="93561-457">在 Windows Server 2008 R2 或更新版本上，開啟 [新增角色及功能精靈]。</span><span class="sxs-lookup"><span data-stu-id="93561-457">On Windows Server 2008 R2 or later, open the **Add Roles and Features Wizard**.</span></span> <span data-ttu-id="93561-458">當您到達 [選取角色服務] 面板時，開啟 [應用程式開發] 節點，然後選取 [應用程式初始化] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="93561-458">When you reach the **Select role services** panel, open the **Application Development** node and select the **Application Initialization** check box.</span></span>
1. <span data-ttu-id="93561-459">在 [IIS 管理員] 中，選取 [連線] 面板中的 [應用程式集區]。</span><span class="sxs-lookup"><span data-stu-id="93561-459">In IIS Manager, select **Application Pools** in the **Connections** panel.</span></span>
1. <span data-ttu-id="93561-460">從清單中選取應用程式的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="93561-460">Select the app's app pool in the list.</span></span>
1. <span data-ttu-id="93561-461">在 [動作] 面板中，選取 [編輯應用程式集區] 下的 [進階設定]。</span><span class="sxs-lookup"><span data-stu-id="93561-461">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="93561-462">將 [啟動模式] 設定為 [AlwaysRunning]。</span><span class="sxs-lookup"><span data-stu-id="93561-462">Set **Start Mode** to **AlwaysRunning**.</span></span>
1. <span data-ttu-id="93561-463">開啟 [連線] 面板中的 [站台] 節點。</span><span class="sxs-lookup"><span data-stu-id="93561-463">Open the **Sites** node in the **Connections** panel.</span></span>
1. <span data-ttu-id="93561-464">選取應用程式。</span><span class="sxs-lookup"><span data-stu-id="93561-464">Select the app.</span></span>
1. <span data-ttu-id="93561-465">選取 [動作] 面板中 [管理網站] 底下的 [進階設定]。</span><span class="sxs-lookup"><span data-stu-id="93561-465">Select **Advanced Settings** under **Manage Website** in the **Actions** panel.</span></span>
1. <span data-ttu-id="93561-466">將 [預先載入已啟用] 設定為 [True]。</span><span class="sxs-lookup"><span data-stu-id="93561-466">Set **Preload Enabled** to **True**.</span></span>

<span data-ttu-id="93561-467">如需詳細資訊，請參閱 [IIS 8.0 應用程式初始化](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)。</span><span class="sxs-lookup"><span data-stu-id="93561-467">For more information, see [IIS 8.0 Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization).</span></span>

<span data-ttu-id="93561-468">應用程式如果使用[跨處理序裝載模型](xref:fundamentals/servers/index#out-of-process-hosting-model)，就必須使用外部服務來定期偵測應用程式，以讓它保持執行。</span><span class="sxs-lookup"><span data-stu-id="93561-468">Apps that use the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model) must use an external service to periodically ping the app in order to keep it running.</span></span>

::: moniker-end

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="93561-469">模組版本和裝載套件組合安裝程式記錄檔</span><span class="sxs-lookup"><span data-stu-id="93561-469">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="93561-470">判斷已安裝的 ASP.NET Core 模組版本：</span><span class="sxs-lookup"><span data-stu-id="93561-470">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="93561-471">在主控系統上，瀏覽至 *%windir%\System32\inetsrv*。</span><span class="sxs-lookup"><span data-stu-id="93561-471">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="93561-472">找出 *aspnetcore.dll* 檔案。</span><span class="sxs-lookup"><span data-stu-id="93561-472">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="93561-473">在該檔案上按一下滑鼠右鍵，然後從關聯式功能表中選取 [內容]。</span><span class="sxs-lookup"><span data-stu-id="93561-473">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="93561-474">選取 [詳細資料] 索引標籤。[檔案版本] 和 [產品版本] 代表已安裝的模組版本。</span><span class="sxs-lookup"><span data-stu-id="93561-474">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="93561-475">模組的「裝載套件組合」安裝程式記錄檔位於 *C:\\Users\\%UserName%\\AppData\\Local\\Temp*。檔案的名稱為 *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*。</span><span class="sxs-lookup"><span data-stu-id="93561-475">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="93561-476">模組、結構描述及設定檔位置</span><span class="sxs-lookup"><span data-stu-id="93561-476">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="93561-477">Module</span><span class="sxs-lookup"><span data-stu-id="93561-477">Module</span></span>

<span data-ttu-id="93561-478">**IIS (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="93561-478">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="93561-479">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="93561-479">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="93561-480">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="93561-480">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="93561-481">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="93561-481">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="93561-482">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="93561-482">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="93561-483">**IIS Express (x86/amd64)：**</span><span class="sxs-lookup"><span data-stu-id="93561-483">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="93561-484">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="93561-484">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="93561-485">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="93561-485">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="93561-486">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="93561-486">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="93561-487">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="93561-487">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="93561-488">結構描述</span><span class="sxs-lookup"><span data-stu-id="93561-488">Schema</span></span>

<span data-ttu-id="93561-489">**IIS**</span><span class="sxs-lookup"><span data-stu-id="93561-489">**IIS**</span></span>

   * <span data-ttu-id="93561-490">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="93561-490">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="93561-491">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="93561-491">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="93561-492">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="93561-492">**IIS Express**</span></span>

   * <span data-ttu-id="93561-493">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="93561-493">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="93561-494">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="93561-494">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="93561-495">Configuration</span><span class="sxs-lookup"><span data-stu-id="93561-495">Configuration</span></span>

<span data-ttu-id="93561-496">**IIS**</span><span class="sxs-lookup"><span data-stu-id="93561-496">**IIS**</span></span>

   * <span data-ttu-id="93561-497">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="93561-497">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="93561-498">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="93561-498">**IIS Express**</span></span>

   * <span data-ttu-id="93561-499">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="93561-499">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>
   
   * <span data-ttu-id="93561-500">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="93561-500">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="93561-501">在 *applicationHost.config* 檔案中搜尋 *aspnetcore*，即可找到這些檔案。</span><span class="sxs-lookup"><span data-stu-id="93561-501">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="93561-502">其他資源</span><span class="sxs-lookup"><span data-stu-id="93561-502">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="93561-503">ASP.NET Core 模組 GitHub 存放庫 (參考來源)</span><span class="sxs-lookup"><span data-stu-id="93561-503">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
