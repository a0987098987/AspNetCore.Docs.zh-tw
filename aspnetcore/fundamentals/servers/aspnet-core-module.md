---
title: ASP.NET Core 模組
author: guardrex
description: 了解 ASP.NET Core 模組如何讓 Kestrel Web 伺服器將 IIS 或 IIS Express 作為反向 Proxy 伺服器使用。
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 39c1b364f9dab635c79e00561d212c858c0c4395
ms.sourcegitcommit: 09affee3d234cb27ea6fe33bc113b79e68900d22
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/06/2018
ms.locfileid: "51191252"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="e524a-103">ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="e524a-103">ASP.NET Core Module</span></span>

<span data-ttu-id="e524a-104">作者：[Tom Dykstra](https://github.com/tdykstra)、[Rick Strahl](https://github.com/RickStrahl) 和 [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="e524a-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="e524a-105">ASP.NET Core 模組可讓 ASP.NET Core 應用程式在 IIS 工作者處理序 (「同處理序」) 或反向 Proxy 設定中的 IIS 後方 (「跨處理序」) 執行。</span><span class="sxs-lookup"><span data-stu-id="e524a-105">The ASP.NET Core Module allows ASP.NET Core apps to run in an IIS worker process (*in-process*) or behind IIS in a reverse proxy configuration (*out-of-process*).</span></span> <span data-ttu-id="e524a-106">IIS 提供進階的 Web 應用程式安全性和管理能力功能。</span><span class="sxs-lookup"><span data-stu-id="e524a-106">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="e524a-107">ASP.NET Core 模組可讓 ASP.NET Core 應用程式在反向 Proxy 組態中的 IIS 後方 執行。</span><span class="sxs-lookup"><span data-stu-id="e524a-107">The ASP.NET Core Module allows ASP.NET Core apps to run behind IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="e524a-108">IIS 提供進階的 Web 應用程式安全性和管理能力功能。</span><span class="sxs-lookup"><span data-stu-id="e524a-108">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

<span data-ttu-id="e524a-109">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="e524a-109">Supported Windows versions:</span></span>

* <span data-ttu-id="e524a-110">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="e524a-110">Windows 7 or later</span></span>
* <span data-ttu-id="e524a-111">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="e524a-111">Windows Server 2008 R2 or later</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="e524a-112">同處理序裝載時，該模組會有自己的伺服器實作 `IISHttpServer`。</span><span class="sxs-lookup"><span data-stu-id="e524a-112">When hosting in-process, the module has its own server implementation, `IISHttpServer`.</span></span>

<span data-ttu-id="e524a-113">跨處理序裝載時，該模組只適用於 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="e524a-113">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="e524a-114">該模組與 [HTTP.sys](xref:fundamentals/servers/httpsys) (先前稱為 [WebListener](xref:fundamentals/servers/weblistener)) 不相容。</span><span class="sxs-lookup"><span data-stu-id="e524a-114">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="e524a-115">該模組只適用於 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="e524a-115">The module only works with Kestrel.</span></span> <span data-ttu-id="e524a-116">該模組與 [HTTP.sys](xref:fundamentals/servers/httpsys) (先前稱為 [WebListener](xref:fundamentals/servers/weblistener)) 不相容。</span><span class="sxs-lookup"><span data-stu-id="e524a-116">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

## <a name="aspnet-core-module-description"></a><span data-ttu-id="e524a-117">ASP.NET Core 模組說明</span><span class="sxs-lookup"><span data-stu-id="e524a-117">ASP.NET Core Module description</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="e524a-118">ASP.NET Core 模組是一種原生 IIS 模組，可外掛至 IIS 管線以便：</span><span class="sxs-lookup"><span data-stu-id="e524a-118">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="e524a-119">在 IIS 工作者處理序 (`w3wp.exe`) 中裝載 ASP.NET Core 應用程式 (稱為[同處理序裝載模型](#in-process-hosting-model))。</span><span class="sxs-lookup"><span data-stu-id="e524a-119">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>

* <span data-ttu-id="e524a-120">將 Web 要求轉送到執行 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的後端 ASP.NET Core 應用程式 (稱為[跨處理序裝載模型](#out-of-process-hosting-model))。</span><span class="sxs-lookup"><span data-stu-id="e524a-120">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="e524a-121">同處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="e524a-121">In-process hosting model</span></span>

<span data-ttu-id="e524a-122">使用同處理序裝載，ASP.NET Core 應用程式會在與其 IIS 工作者處理序相同的處理序中執行。</span><span class="sxs-lookup"><span data-stu-id="e524a-122">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="e524a-123">這可避免使用跨處理序裝載模型時，透過回送介面卡 Proxy 處理要求對效能的負面影響。</span><span class="sxs-lookup"><span data-stu-id="e524a-123">This removes the performance penalty of proxying requests over the loopback adapter when using the out-of-process hosting model.</span></span> <span data-ttu-id="e524a-124">IIS 透過 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 來執行處理程序管理。</span><span class="sxs-lookup"><span data-stu-id="e524a-124">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="e524a-125">ASP.NET Core 模組：</span><span class="sxs-lookup"><span data-stu-id="e524a-125">The ASP.NET Core Module:</span></span>

* <span data-ttu-id="e524a-126">執行應用程式初始化。</span><span class="sxs-lookup"><span data-stu-id="e524a-126">Performs app initialization.</span></span>
  * <span data-ttu-id="e524a-127">載入 [CoreCLR](/dotnet/standard/glossary#coreclr)。</span><span class="sxs-lookup"><span data-stu-id="e524a-127">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="e524a-128">呼叫 `Program.Main`。</span><span class="sxs-lookup"><span data-stu-id="e524a-128">Calls `Program.Main`.</span></span>
* <span data-ttu-id="e524a-129">處理 IIS 原生要求的存留期。</span><span class="sxs-lookup"><span data-stu-id="e524a-129">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="e524a-130">下圖說明 IIS、ASP.NET Core 模組和同處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="e524a-130">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![ASP.NET Core 模組](aspnet-core-module/_static/ancm-inprocess.png)

<span data-ttu-id="e524a-132">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="e524a-132">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="e524a-133">驅動程式會在網站設定的連接埠上將原生要求路由至 IIS，此連接埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="e524a-133">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="e524a-134">模組會接收原生要求，並將控制項傳遞至 `IISHttpServer`，再由其將要求從原生轉換成受控。</span><span class="sxs-lookup"><span data-stu-id="e524a-134">The module receives the native request and passes control to `IISHttpServer`, which is what converts the request from native to managed.</span></span>

<span data-ttu-id="e524a-135">`IISHttpServer` 收取要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="e524a-135">After `IISHttpServer` picks up the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="e524a-136">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="e524a-136">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="e524a-137">應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="e524a-137">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="e524a-138">跨處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="e524a-138">Out-of-process hosting model</span></span>

<span data-ttu-id="e524a-139">因為 ASP.NET Core 應用程式執行所在的處理序會與 IIS 工作者處理序分開，所以此模組會執行處理程序管理。</span><span class="sxs-lookup"><span data-stu-id="e524a-139">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="e524a-140">此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式關閉或損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="e524a-140">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="e524a-141">此行為基本上與執行同處理序，並由 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="e524a-141">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="e524a-142">下圖說明 IIS、ASP.NET Core 模組和跨處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="e524a-142">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![ASP.NET Core 模組](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="e524a-144">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="e524a-144">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="e524a-145">驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="e524a-145">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="e524a-146">此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，這並不是通訊埠 80/443。</span><span class="sxs-lookup"><span data-stu-id="e524a-146">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="e524a-147">此模組在啟動時透過環境變數指定通訊埠，而 IIS 整合中介軟體則會設定伺服器來接聽 `http://localhost:{port}`。</span><span class="sxs-lookup"><span data-stu-id="e524a-147">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="e524a-148">將會執行額外檢查，不是源自模組的要求都會遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="e524a-148">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="e524a-149">此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。</span><span class="sxs-lookup"><span data-stu-id="e524a-149">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="e524a-150">Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="e524a-150">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="e524a-151">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="e524a-151">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="e524a-152">IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="e524a-152">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="e524a-153">應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="e524a-153">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="e524a-154">ASP.NET Core 模組是一種原生 IIS 模組，可外掛至 IIS 管線，將 Web 要求重新轉送到後端的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e524a-154">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="e524a-155">因為處理序中執行的 ASP.NET Core 應用程式會與 IIS 背景工作處理序分開，所以此模組也會執行處理序管理。</span><span class="sxs-lookup"><span data-stu-id="e524a-155">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="e524a-156">此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="e524a-156">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="e524a-157">此行為基本上與在 IIS 中執行同處理序，並由 [Windows 處理器啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的 ASP.NET 4.x 應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="e524a-157">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="e524a-158">下圖說明 IIS、ASP.NET Core 模組和應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="e524a-158">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![ASP.NET Core 模組](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="e524a-160">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="e524a-160">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="e524a-161">驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="e524a-161">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="e524a-162">此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，這並不是通訊埠 80/443。</span><span class="sxs-lookup"><span data-stu-id="e524a-162">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="e524a-163">此模組在啟動時透過環境變數指定通訊埠，而 IIS 整合中介軟體則會設定伺服器來接聽 `http://localhost:{port}`。</span><span class="sxs-lookup"><span data-stu-id="e524a-163">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="e524a-164">將會執行額外檢查，不是源自模組的要求都會遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="e524a-164">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="e524a-165">此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。</span><span class="sxs-lookup"><span data-stu-id="e524a-165">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="e524a-166">Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="e524a-166">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="e524a-167">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="e524a-167">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="e524a-168">IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="e524a-168">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="e524a-169">應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="e524a-169">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="e524a-170">許多如 Windows 驗證等原生模組仍在使用中。</span><span class="sxs-lookup"><span data-stu-id="e524a-170">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="e524a-171">若要深入了解搭配此模組的使用中 IIS 模組，請參閱<xref:host-and-deploy/iis/modules>。</span><span class="sxs-lookup"><span data-stu-id="e524a-171">To learn more about IIS modules active with the module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="e524a-172">ASP.NET Core 模組具有一些其他功能。</span><span class="sxs-lookup"><span data-stu-id="e524a-172">The ASP.NET Core Module has a few other functions.</span></span> <span data-ttu-id="e524a-173">此模組可以：</span><span class="sxs-lookup"><span data-stu-id="e524a-173">The module can:</span></span>

* <span data-ttu-id="e524a-174">設定背景工作處理序的環境變數。</span><span class="sxs-lookup"><span data-stu-id="e524a-174">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="e524a-175">將 stdout 輸出記錄到檔案儲存區，以針對啟動問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="e524a-175">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="e524a-176">轉送 Windows 驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="e524a-176">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="e524a-177">如何安裝和使用 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="e524a-177">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="e524a-178">如需如何安裝和使用 ASP.NET Core 模組的詳細指示，請參閱<xref:host-and-deploy/iis/index>。</span><span class="sxs-lookup"><span data-stu-id="e524a-178">For detailed instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span> <span data-ttu-id="e524a-179">如需設定模組的資訊，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="e524a-179">For information on configuring the module, see the <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e524a-180">其他資源</span><span class="sxs-lookup"><span data-stu-id="e524a-180">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* [<span data-ttu-id="e524a-181">ASP.NET Core 模組 GitHub 存放庫 (原始程式碼)</span><span class="sxs-lookup"><span data-stu-id="e524a-181">ASP.NET Core Module GitHub repository (source code)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
