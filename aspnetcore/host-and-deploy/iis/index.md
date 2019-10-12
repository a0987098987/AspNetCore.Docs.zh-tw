---
title: 在使用 IIS 的 Windows 上裝載 ASP.NET Core
author: guardrex
description: 了解如何在 Windows Server Internet Information Services (IIS) 上裝載 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2019
uid: host-and-deploy/iis/index
ms.openlocfilehash: c11a46220f0055f4d3d14c84065281f642a4cbe7
ms.sourcegitcommit: 020c3760492efed71b19e476f25392dda5dd7388
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/12/2019
ms.locfileid: "72289031"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="a8e3d-103">在使用 IIS 的 Windows 上裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a8e3d-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="a8e3d-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a8e3d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a8e3d-105">如需將 ASP.NET Core 應用程式發佈至 IIS 伺服器的教學課程體驗，請參閱<xref:tutorials/publish-to-iis>。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-105">For a tutorial experience on publishing an ASP.NET Core app to an IIS server, see <xref:tutorials/publish-to-iis>.</span></span>

[<span data-ttu-id="a8e3d-106">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="a8e3d-106">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="a8e3d-107">支援的作業系統</span><span class="sxs-lookup"><span data-stu-id="a8e3d-107">Supported operating systems</span></span>

<span data-ttu-id="a8e3d-108">支援下列作業系統：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-108">The following operating systems are supported:</span></span>

* <span data-ttu-id="a8e3d-109">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a8e3d-109">Windows 7 or later</span></span>
* <span data-ttu-id="a8e3d-110">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a8e3d-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="a8e3d-111">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys) (先前稱為 WebListener) 不適用搭配 IIS 的反向 Proxy 設定。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-111">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="a8e3d-112">請使用 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-112">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="a8e3d-113">如需在 Azure 中裝載的資訊，請參閱 <xref:host-and-deploy/azure-apps/index>。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-113">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

<span data-ttu-id="a8e3d-114">如需疑難排解指引，請參閱 <xref:test/troubleshoot>。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-114">For troubleshooting guidance, see <xref:test/troubleshoot>.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="a8e3d-115">支援的平台</span><span class="sxs-lookup"><span data-stu-id="a8e3d-115">Supported platforms</span></span>

<span data-ttu-id="a8e3d-116">支援針對 32 位元 (x86) 或 64 位元 (x64) 部署發行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-116">Apps published for 32-bit (x86) or 64-bit (x64) deployment are supported.</span></span> <span data-ttu-id="a8e3d-117">使用 32 位元 (x86) .NET Core SDK 部署 32 位元應用程式，除非應用程式：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-117">Deploy a 32-bit app with a 32-bit (x86) .NET Core SDK unless the app:</span></span>

* <span data-ttu-id="a8e3d-118">需要提供給 64 位元應用程式使用的較大虛擬記憶體位址空間。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-118">Requires the larger virtual memory address space available to a 64-bit app.</span></span>
* <span data-ttu-id="a8e3d-119">需要較大的 IIS 堆疊大小。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-119">Requires the larger IIS stack size.</span></span>
* <span data-ttu-id="a8e3d-120">有 64 位元原生相依性。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-120">Has 64-bit native dependencies.</span></span>

<span data-ttu-id="a8e3d-121">使用 64 位元 (x64) .NET Core SDK 來發行 64 位元應用程式。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-121">Use a 64-bit (x64) .NET Core SDK to publish a 64-bit app.</span></span> <span data-ttu-id="a8e3d-122">主機系統上必須有 64 位元執行階段存在。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-122">A 64-bit runtime must be present on the host system.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-models"></a><span data-ttu-id="a8e3d-123">裝載模型</span><span class="sxs-lookup"><span data-stu-id="a8e3d-123">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="a8e3d-124">同處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="a8e3d-124">In-process hosting model</span></span>

<span data-ttu-id="a8e3d-125">使用同處理序裝載，ASP.NET Core 應用程式會在與其 IIS 工作者處理序相同的處理序中執行。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-125">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="a8e3d-126">因為要求未透過回送介面卡 (將連出網路流量傳回同一部電腦的網路介面) 進行 proxy 處理，所以同處理序裝載會提供優於跨處理序裝載的效能。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-126">In-process hosting provides improved performance over out-of-process hosting because requests aren't proxied over the loopback adapter, a network interface that returns outgoing network traffic back to the same machine.</span></span> <span data-ttu-id="a8e3d-127">IIS 透過 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 來執行處理程序管理。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-127">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="a8e3d-128">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-128">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module):</span></span>

* <span data-ttu-id="a8e3d-129">執行應用程式初始化。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-129">Performs app initialization.</span></span>
  * <span data-ttu-id="a8e3d-130">載入 [CoreCLR](/dotnet/standard/glossary#coreclr)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-130">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="a8e3d-131">呼叫 `Program.Main`。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-131">Calls `Program.Main`.</span></span>
* <span data-ttu-id="a8e3d-132">處理 IIS 原生要求的存留期。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-132">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="a8e3d-133">以 .NET Framework 為目標的 ASP.NET Core 應用程式不支援處理序內裝載模型。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-133">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="a8e3d-134">下圖說明 IIS、ASP.NET Core 模組和同處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-134">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![同處理序代管內的 ASP.NET Core 模組案例](index/_static/ancm-inprocess.png)

<span data-ttu-id="a8e3d-136">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-136">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="a8e3d-137">驅動程式會在網站設定的連接埠上將原生要求路由至 IIS，此連接埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-137">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="a8e3d-138">模組會接收原生要求，並將它傳遞至 IIS HTTP 伺服器 (`IISHttpServer`)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-138">The module receives the native request and passes it to IIS HTTP Server (`IISHttpServer`).</span></span> <span data-ttu-id="a8e3d-139">IIS HTTP 伺服器是 IIS 的同處理序伺服程式實作，可將要求從原生轉換為受控。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-139">IIS HTTP Server is an in-process server implementation for IIS that converts the request from native to managed.</span></span>

<span data-ttu-id="a8e3d-140">IIS HTTP 伺服器處理要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-140">After the IIS HTTP Server processes the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="a8e3d-141">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-141">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="a8e3d-142">應用程式的回應會透過 IIS HTTP 伺服器傳回 IIS。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-142">The app's response is passed back to IIS through IIS HTTP Server.</span></span> <span data-ttu-id="a8e3d-143">IIS 會將回應傳送到起始該要求的用戶端。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-143">IIS sends the response to the client that initiated the request.</span></span>

<span data-ttu-id="a8e3d-144">現有的應用程式可以選擇同處理序裝載，但 [dotnet new](/dotnet/core/tools/dotnet-new) 範本預設會對所有 IIS 和 IIS Express 案例使用同處理序裝載模型。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-144">In-process hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="a8e3d-145">`CreateDefaultBuilder` 會透過呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> 方法來啟動 [CoreCLR](/dotnet/standard/glossary#coreclr) 以新增 <xref:Microsoft.AspNetCore.Hosting.Server.IServer>，並在 IIS 工作者處理序 (*w3wp.exe* 或 *iisexpress.exe*) 內裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-145">`CreateDefaultBuilder` adds an <xref:Microsoft.AspNetCore.Hosting.Server.IServer> instance by calling the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> method to boot the [CoreCLR](/dotnet/standard/glossary#coreclr) and host the app inside of the IIS worker process (*w3wp.exe* or *iisexpress.exe*).</span></span> <span data-ttu-id="a8e3d-146">效能測試指出，相較於跨處理序裝載 .NET Core 應用程式，並將要求 Proxy 處理至 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器，裝載於同處理序可提供明顯更高的要求輸送量。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-146">Performance tests indicate that hosting a .NET Core app in-process delivers significantly higher request throughput compared to hosting the app out-of-process and proxying requests to [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

> [!NOTE]
> <span data-ttu-id="a8e3d-147">發佈為單一檔案可執行檔的應用程式無法由同處理序裝載模型載入。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-147">Apps published as a single file executable can't be loaded by the in-process hosting model.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="a8e3d-148">跨處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="a8e3d-148">Out-of-process hosting model</span></span>

<span data-ttu-id="a8e3d-149">因為 ASP.NET Core 應用程式執行所在的處理序會與 IIS 工作者處理序分開，所以此模組會執行處理程序管理。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-149">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="a8e3d-150">此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式關閉或損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-150">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="a8e3d-151">此行為基本上與執行同處理序，並由 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-151">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="a8e3d-152">下圖說明 IIS、ASP.NET Core 模組和跨處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-152">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![非同處理序代管內的 ASP.NET Core 模組案例](index/_static/ancm-outofprocess.png)

<span data-ttu-id="a8e3d-154">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-154">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="a8e3d-155">驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-155">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="a8e3d-156">此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，而且不會是通訊埠 80 或 443。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-156">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="a8e3d-157">此模組在啟動時透過環境變數指定連接埠，而 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 延伸模組則會設定伺服器來接聽 `http://localhost:{PORT}`。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-157">The module specifies the port via an environment variable at startup, and the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> extension configures the server to listen on `http://localhost:{PORT}`.</span></span> <span data-ttu-id="a8e3d-158">將會執行額外檢查，不是源自模組的要求都會遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-158">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="a8e3d-159">此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-159">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="a8e3d-160">Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-160">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="a8e3d-161">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-161">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="a8e3d-162">IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-162">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="a8e3d-163">應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-163">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a8e3d-164">ASP.NET Core 隨附 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)，其為預設的跨平台 HTTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-164">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), a default, cross-platform HTTP server.</span></span>

<span data-ttu-id="a8e3d-165">在使用 [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) 或 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 時，應用程式會執行於從 IIS 背景工作處理序中分離出的處理序 (跨處理序)，並搭配 [Kestrel 伺服器](xref:fundamentals/servers/index#kestrel)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-165">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the app runs in a process separate from the IIS worker process (*out-of-process*) with the [Kestrel server](xref:fundamentals/servers/index#kestrel).</span></span>

<span data-ttu-id="a8e3d-166">因為 ASP.NET Core 應用程式執行所在的處理序會與 IIS 工作者處理序分開，所以此模組會執行處理程序管理。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-166">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="a8e3d-167">此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式關閉或損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-167">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="a8e3d-168">此行為基本上與執行同處理序，並由 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-168">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="a8e3d-169">下圖說明 IIS、ASP.NET Core 模組和跨處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-169">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![ASP.NET Core 模組](index/_static/ancm-outofprocess.png)

<span data-ttu-id="a8e3d-171">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-171">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="a8e3d-172">驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-172">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="a8e3d-173">此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，而且不會是通訊埠 80 或 443。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-173">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="a8e3d-174">此模組在啟動時透過環境變數指定連接埠，而 [IIS 整合中介軟體](xref:host-and-deploy/iis/index#enable-the-iisintegration-components)則會設定伺服器來接聽 `http://localhost:{port}`。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-174">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="a8e3d-175">將會執行額外檢查，不是源自模組的要求都會遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-175">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="a8e3d-176">此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-176">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="a8e3d-177">Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-177">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="a8e3d-178">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-178">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="a8e3d-179">IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-179">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="a8e3d-180">應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-180">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="a8e3d-181">`CreateDefaultBuilder` 會將 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器設為網頁伺服器，並設定 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)的基底路徑與連接埠來啟用 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-181">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and enables IIS Integration by configuring the base path and port for the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="a8e3d-182">ASP.NET Core 模組會產生要指派給後端處理序的動態連接埠。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-182">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="a8e3d-183">`CreateDefaultBuilder` 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 方法。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-183">`CreateDefaultBuilder` calls the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> method.</span></span> <span data-ttu-id="a8e3d-184">`UseIISIntegration` 會將 Kestrel 設定為在位於 localhost IP 位址 (`127.0.0.1`) 的動態連接埠上接聽。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-184">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="a8e3d-185">若動態連接埠是 1234，Kestrel 會在 `127.0.0.1:1234` 接聽。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-185">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="a8e3d-186">此設定會取代由下列項目提供的其他 URL 設定：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-186">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="a8e3d-187">Kestrel 的接聽 API</span><span class="sxs-lookup"><span data-stu-id="a8e3d-187">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="a8e3d-188">[設定](xref:fundamentals/configuration/index) (或[命令列 --urls 選項](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="a8e3d-188">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="a8e3d-189">在使用模組時不需要呼叫 `UseUrls` 或 Kestrel 的 `Listen` API。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-189">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="a8e3d-190">若呼叫 `UseUrls` 或 `Listen`，Kestrel 只會接聽未使用 IIS 執行應用程式時指定的連接埠。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-190">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

<span data-ttu-id="a8e3d-191">如需處理序內和處理序外裝載模型的詳細資訊，請參閱 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)及 [ASP.NET Core 模組設定參考](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-191">For more information on the in-process and out-of-process hosting models, see [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

::: moniker-end

<span data-ttu-id="a8e3d-192">如需 ASP.NET Core 模組組態指南，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-192">For ASP.NET Core Module configuration guidance, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="a8e3d-193">如需代管的詳細資訊，請參閱[在 ASP.NET Core 中代管](xref:fundamentals/index#host)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-193">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/index#host).</span></span>

## <a name="application-configuration"></a><span data-ttu-id="a8e3d-194">應用程式組態</span><span class="sxs-lookup"><span data-stu-id="a8e3d-194">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="a8e3d-195">啟用 IISIntegration 元件</span><span class="sxs-lookup"><span data-stu-id="a8e3d-195">Enable the IISIntegration components</span></span>

<span data-ttu-id="a8e3d-196">一般的 *Program.cs* 會呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>，開始設定會啟用與 IIS 整合的主機：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-196">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host that enables integration with IIS:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

### <a name="iis-options"></a><span data-ttu-id="a8e3d-197">IIS 選項</span><span class="sxs-lookup"><span data-stu-id="a8e3d-197">IIS options</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a8e3d-198">**同處理序主控模型**</span><span class="sxs-lookup"><span data-stu-id="a8e3d-198">**In-process hosting model**</span></span>

<span data-ttu-id="a8e3d-199">若要設定 IIS 伺服器選項，請在 <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 中加入 <xref:Microsoft.AspNetCore.Builder.IISServerOptions> 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-199">To configure IIS Server options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISServerOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="a8e3d-200">下列範例會停用 AutomaticAuthentication：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-200">The following example disables AutomaticAuthentication:</span></span>

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="a8e3d-201">選項</span><span class="sxs-lookup"><span data-stu-id="a8e3d-201">Option</span></span>                         | <span data-ttu-id="a8e3d-202">預設</span><span class="sxs-lookup"><span data-stu-id="a8e3d-202">Default</span></span> | <span data-ttu-id="a8e3d-203">設定</span><span class="sxs-lookup"><span data-stu-id="a8e3d-203">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="a8e3d-204">若為 `true`，IIS 伺服器會設定由 [Windows 驗證](xref:security/authentication/windowsauth)所驗證的 `HttpContext.User`。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-204">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="a8e3d-205">若為 `false`，則伺服器僅會對 `HttpContext.User` 提供身分識別，並在 `AuthenticationScheme` 明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-205">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="a8e3d-206">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-206">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="a8e3d-207">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-207">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="a8e3d-208">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-208">Sets the display name shown to users on login pages.</span></span> |
| `AllowSynchronousIO`           | `false` | <span data-ttu-id="a8e3d-209">是否要針對 `HttpContext.Request` 和 `HttpContext.Response` 允許同步 IO。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-209">Whether synchronous IO is allowed for the `HttpContext.Request` and the `HttpContext.Response`.</span></span> |
| `MaxRequestBodySize`           | `30000000`  | <span data-ttu-id="a8e3d-210">取得或設定 `HttpRequest` 的要求本文大小上限。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-210">Gets or sets the max request body size for the `HttpRequest`.</span></span> <span data-ttu-id="a8e3d-211">請注意，IIS 本身具有限制 `maxAllowedContentLength`，此限制將在 `IISServerOptions` 中設定 `MaxRequestBodySize` 時處理。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-211">Note that IIS itself has the limit `maxAllowedContentLength` which will be processed before the `MaxRequestBodySize` set in the `IISServerOptions`.</span></span> <span data-ttu-id="a8e3d-212">變更 `MaxRequestBodySize` 將不會影響 `maxAllowedContentLength`。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-212">Changing the `MaxRequestBodySize` won't affect the `maxAllowedContentLength`.</span></span> <span data-ttu-id="a8e3d-213">若要增加 `maxAllowedContentLength`，請在 *web.config* 中新增項目，以將 `maxAllowedContentLength` 設定為較高的值。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-213">To increase `maxAllowedContentLength`, add an entry in the *web.config* to set `maxAllowedContentLength` to a higher value.</span></span> <span data-ttu-id="a8e3d-214">如需更多詳細資料，請參閱[組態](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/#configuration)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-214">For more details, see [Configuration](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/#configuration).</span></span> |

<span data-ttu-id="a8e3d-215">**跨處理序裝載模型**</span><span class="sxs-lookup"><span data-stu-id="a8e3d-215">**Out-of-process hosting model**</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="a8e3d-216">選項</span><span class="sxs-lookup"><span data-stu-id="a8e3d-216">Option</span></span>                         | <span data-ttu-id="a8e3d-217">預設</span><span class="sxs-lookup"><span data-stu-id="a8e3d-217">Default</span></span> | <span data-ttu-id="a8e3d-218">設定</span><span class="sxs-lookup"><span data-stu-id="a8e3d-218">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="a8e3d-219">若為 `true`，IIS 伺服器會設定由 [Windows 驗證](xref:security/authentication/windowsauth)所驗證的 `HttpContext.User`。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-219">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="a8e3d-220">若為 `false`，則伺服器僅會對 `HttpContext.User` 提供身分識別，並在 `AuthenticationScheme` 明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-220">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="a8e3d-221">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-221">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="a8e3d-222">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-222">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="a8e3d-223">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-223">Sets the display name shown to users on login pages.</span></span> |

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a8e3d-224">**跨處理序裝載模型**</span><span class="sxs-lookup"><span data-stu-id="a8e3d-224">**Out-of-process hosting model**</span></span>

::: moniker-end

<span data-ttu-id="a8e3d-225">若要設定 IIS 選項，請在 <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 中加入 <xref:Microsoft.AspNetCore.Builder.IISOptions> 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-225">To configure IIS options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="a8e3d-226">下列範例會防止應用程式填入 `HttpContext.Connection.ClientCertificate`：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-226">The following example prevents the app from populating `HttpContext.Connection.ClientCertificate`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="a8e3d-227">選項</span><span class="sxs-lookup"><span data-stu-id="a8e3d-227">Option</span></span>                         | <span data-ttu-id="a8e3d-228">預設</span><span class="sxs-lookup"><span data-stu-id="a8e3d-228">Default</span></span> | <span data-ttu-id="a8e3d-229">設定</span><span class="sxs-lookup"><span data-stu-id="a8e3d-229">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="a8e3d-230">若為 `true`，[IIS 整合中介軟體](#enable-the-iisintegration-components)會設定由 [Windows 驗證](xref:security/authentication/windowsauth)所驗證的 `HttpContext.User`。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-230">If `true`, [IIS Integration Middleware](#enable-the-iisintegration-components) sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="a8e3d-231">如果為 `false`，則驗證中介軟體僅針對 `HttpContext.User` 提供身分識別，並在游 `AuthenticationScheme` 提出明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-231">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="a8e3d-232">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-232">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="a8e3d-233">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-233">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="a8e3d-234">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-234">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="a8e3d-235">如果為 `true` 且 `MS-ASPNETCORE-CLIENTCERT` 要求標頭已存在，則會填入 `HttpContext.Connection.ClientCertificate`。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-235">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="a8e3d-236">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="a8e3d-236">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="a8e3d-237">用來設定轉送標頭中介軟體及 ASP.NET Core 模組的 [IIS 整合中介軟體](#enable-the-iisintegration-components)會設定為轉送配置 (HTTP/HTTPS) 與發出要求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-237">The [IIS Integration Middleware](#enable-the-iisintegration-components), which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="a8e3d-238">其他 Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-238">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="a8e3d-239">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-239">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="a8e3d-240">web.config 檔案</span><span class="sxs-lookup"><span data-stu-id="a8e3d-240">web.config file</span></span>

<span data-ttu-id="a8e3d-241">*web.config* 檔案是用來設定 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-241">The *web.config* file configures the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="a8e3d-242">發佈專案時，由 MSBuild 目標 (`_TransformWebConfig`) 處理 *web.config* 檔案的建立、轉換及發佈。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-242">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="a8e3d-243">此目標存在於 Web SDK 目標 (`Microsoft.NET.Sdk.Web`)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-243">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="a8e3d-244">SDK 設定在專案檔的頂端：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-244">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="a8e3d-245">如果專案中沒有 *web.config* 檔案，則系統會使用正確的 *processPath* 和 *arguments* 建立該檔案以設定 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)，並將該檔案移至[已發行的輸出](xref:host-and-deploy/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-245">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="a8e3d-246">如果 *web.config* 檔案存在於專案中，則系統會使用正確的 *processPath* 和 *arguments* 來轉換該檔案以設定 ASP.NET Core 模組，然後將它移至已發行的輸出。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-246">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="a8e3d-247">轉換不會修改檔案中的 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-247">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="a8e3d-248">*web.config* 檔案可提供能控制作用中 IIS 模組的額外 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-248">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="a8e3d-249">如需能處理 ASP.NET Core 應用程式要求之 IIS 模組的相關資訊，請參閱 [IIS 模組](xref:host-and-deploy/iis/modules)主題。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-249">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="a8e3d-250">為防止 Web SDK 轉換 *web.config* 檔案，請使用專案檔中的 **\<IsTransformWebConfigDisabled>** 屬性：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-250">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="a8e3d-251">使 Web SDK 無法轉換檔案時，應該由開發人員手動設定 *processPath* 和 *arguments*。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-251">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="a8e3d-252">如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-252">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="a8e3d-253">web.config 檔案位置</span><span class="sxs-lookup"><span data-stu-id="a8e3d-253">web.config file location</span></span>

<span data-ttu-id="a8e3d-254">為了正確設定[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module) *，web.config 檔案*必須存在於已部署應用程式的[內容根](xref:fundamentals/index#content-root)路徑（通常是應用程式基底路徑）。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-254">In order to set up the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) correctly, the *web.config* file must be present at the [content root](xref:fundamentals/index#content-root) path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="a8e3d-255">這是與提供給 IIS 的網站實體路徑相同的位置。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-255">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="a8e3d-256">應用程式的根目錄需有 *web.config* 檔案，才能使用 Web Deploy 發行多個應用程式。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-256">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="a8e3d-257">機密檔案存在於應用程式的實體路徑上，例如 *\<組件>.runtimeconfig.json*、 *\<組件>.xml* (XML 文件註解)，以及 *\<組件>.deps.json*。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-257">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="a8e3d-258">當 *web.config* 檔案存在且網站正常啟動時，如果有人要求機密檔案，IIS 不會予以提供。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-258">When the *web.config* file is present and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="a8e3d-259">若 *web.config* 檔案遺失或沒有正確命名，或是無法設定網站以正常啟動，IIS 可能會公開提供機密檔案。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-259">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="a8e3d-260">***web.config* 檔案必須持續存在於部署之中、已正確命名，並能夠設定網站以正常啟動。無論在任何情況下，請都不要從生產環境部署移除 *web.config* 檔案。**</span><span class="sxs-lookup"><span data-stu-id="a8e3d-260">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

### <a name="transform-webconfig"></a><span data-ttu-id="a8e3d-261">轉換 web.config</span><span class="sxs-lookup"><span data-stu-id="a8e3d-261">Transform web.config</span></span>

<span data-ttu-id="a8e3d-262">如需在發佈時轉換 *web.config* (例如依據設定、設定檔或環境設定環境變數)，請參閱<xref:host-and-deploy/iis/transform-webconfig>。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-262">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="a8e3d-263">IIS 組態</span><span class="sxs-lookup"><span data-stu-id="a8e3d-263">IIS configuration</span></span>

<span data-ttu-id="a8e3d-264">**Windows Server 作業系統**</span><span class="sxs-lookup"><span data-stu-id="a8e3d-264">**Windows Server operating systems**</span></span>

<span data-ttu-id="a8e3d-265">啟用**網頁伺服器 (IIS)** 伺服器角色，並建立角色服務。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-265">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="a8e3d-266">使用來自 [管理] 功能表的 [新增角色及功能] 精靈，或是 [伺服器管理員] 中的連結。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-266">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="a8e3d-267">在**伺服器角色**步驟中，核取 [網頁伺服器 (IIS)] 方塊。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-267">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![在選取伺服器角色步驟中選取網頁伺服器 IIS 角色。](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="a8e3d-269">在 [功能] 步驟之後，[角色服務] 步驟會針對網頁伺服器 (IIS) 進行載入。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-269">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="a8e3d-270">選取所需的 IIS 角色服務或接受所提供的預設角色服務。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-270">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![在選取角色服務步驟中，選取預設的角色服務。](index/_static/role-services-ws2016.png)

   <span data-ttu-id="a8e3d-272">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="a8e3d-272">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="a8e3d-273">若要啟用 Windows 驗證，請展開下列節點：[網頁伺服器] > [安全性]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-273">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="a8e3d-274">選取 [Windows 驗證] 功能。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-274">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="a8e3d-275">如需詳細資訊，請參閱 [Windows 驗證 \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) 和[設定 Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-275">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="a8e3d-276">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="a8e3d-276">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="a8e3d-277">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-277">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="a8e3d-278">若要啟用 WebSockets，請展開下列節點：[網頁伺服器] > [應用程式開發]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-278">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="a8e3d-279">選取 [WebSocket 通訊協定] 功能。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-279">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="a8e3d-280">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-280">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="a8e3d-281">透過**確認**步驟繼續作業，安裝網頁伺服器角色和服務。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-281">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="a8e3d-282">安裝**網頁伺服器 (IIS)** 角色之後，不需要重新啟動伺服器/IIS。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-282">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="a8e3d-283">**Windows 桌面作業系統**</span><span class="sxs-lookup"><span data-stu-id="a8e3d-283">**Windows desktop operating systems**</span></span>

<span data-ttu-id="a8e3d-284">啟用 [IIS 管理主控台] 和 [World Wide Web 服務]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-284">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="a8e3d-285">瀏覽至控制台 > [程式] > [程式和功能] > [開啟或關閉 Windows 功能] (畫面左側)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-285">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="a8e3d-286">開啟 [Internet Information Services] 節點。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-286">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="a8e3d-287">開啟 [Web 管理工具] 節點。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-287">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="a8e3d-288">核取 [IIS 管理主控台] 方塊。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-288">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="a8e3d-289">[World Wide Web Services] (全球資訊網服務) 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-289">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="a8e3d-290">接受**全球資訊網服務**的預設功能，或自訂 IIS 功能。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-290">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="a8e3d-291">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="a8e3d-291">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="a8e3d-292">若要啟用 Windows 驗證，請展開下列節點：[World Wide Web 服務] > [安全性]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-292">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="a8e3d-293">選取 [Windows 驗證] 功能。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-293">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="a8e3d-294">如需詳細資訊，請參閱 [Windows 驗證 \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) 和[設定 Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-294">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="a8e3d-295">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="a8e3d-295">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="a8e3d-296">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-296">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="a8e3d-297">若要啟用 WebSockets，請展開下列節點：[World Wide Web 服務] > [應用程式開發功能]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-297">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="a8e3d-298">選取 [WebSocket 通訊協定] 功能。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-298">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="a8e3d-299">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-299">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="a8e3d-300">若 IIS 安裝需要重新啟動，請重新啟動系統。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-300">If the IIS installation requires a restart, restart the system.</span></span>

![選取 [Windows 功能] 中的 [IIS 管理主控台] 和 [World Wide Web Services] (全球資訊網服務)。](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="a8e3d-302">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="a8e3d-302">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="a8e3d-303">在主控系統上安裝 .NET Core 裝載套件組合。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-303">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="a8e3d-304">套件組合會安裝 .NET Core 執行階段、.NET Core 程式庫和 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-304">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="a8e3d-305">此模組可讓 ASP.NET Core 應用程式在 IIS 背後執行。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-305">The module allows ASP.NET Core apps to run behind IIS.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a8e3d-306">若裝載套件組合在 IIS 之前安裝，則必須對該套件組合安裝進行修復。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-306">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="a8e3d-307">請在安裝 IIS 之後，再次執行裝載套件組合安裝程式。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-307">Run the Hosting Bundle installer again after installing IIS.</span></span>
>
> <span data-ttu-id="a8e3d-308">如果在安裝 64 位元 (x64) 版本的 .NET Core 後才安裝裝載套件組合，那麼可能會遺漏 SDK ([未偵測到 .NET Core SDK](xref:test/troubleshoot#no-net-core-sdks-were-detected))。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-308">If the Hosting Bundle is installed after installing the 64-bit (x64) version of .NET Core, SDKs might appear to be missing ([No .NET Core SDKs were detected](xref:test/troubleshoot#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="a8e3d-309">若要解決此問題，請參閱 <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-309">To resolve the problem, see <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>.</span></span>

### <a name="direct-download-current-version"></a><span data-ttu-id="a8e3d-310">直接下載 (目前版本)</span><span class="sxs-lookup"><span data-stu-id="a8e3d-310">Direct download (current version)</span></span>

<span data-ttu-id="a8e3d-311">使用下列連結下載安裝程式：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-311">Download the installer using the following link:</span></span>

[<span data-ttu-id="a8e3d-312">目前的 .NET Core 裝載套件組合安裝程式 (直接下載)</span><span class="sxs-lookup"><span data-stu-id="a8e3d-312">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a><span data-ttu-id="a8e3d-313">安裝程式的先前版本</span><span class="sxs-lookup"><span data-stu-id="a8e3d-313">Earlier versions of the installer</span></span>

<span data-ttu-id="a8e3d-314">若要取得安裝程式的先前版本：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-314">To obtain an earlier version of the installer:</span></span>

1. <span data-ttu-id="a8e3d-315">瀏覽至 [.NET 下載封存](https://www.microsoft.com/net/download/archives)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-315">Navigate to the [.NET download archives](https://www.microsoft.com/net/download/archives).</span></span>
1. <span data-ttu-id="a8e3d-316">在 [.NET Core] 下，選取 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-316">Under **.NET Core**, select the .NET Core version.</span></span>
1. <span data-ttu-id="a8e3d-317">在 [執行應用程式 - 執行階段] 欄中，尋找想要的 .NET Core 執行階段版本列。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-317">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="a8e3d-318">使用**執行階段與裝載套件組合**連結。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-318">Download the installer using the **Runtime & Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="a8e3d-319">某些安裝程式包含已達到期生命週期結束 (EOL) 的發行版本，這些發行版本已不受 Microsoft 支援。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-319">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="a8e3d-320">如需詳細資訊，請參閱[支援原則](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) \(英文 \)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-320">For more information, see the [support policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="a8e3d-321">安裝裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="a8e3d-321">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="a8e3d-322">在伺服器上執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-322">Run the installer on the server.</span></span> <span data-ttu-id="a8e3d-323">從系統管理員命令殼層執行安裝程式時，有 下列參數可用：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-323">The following parameters are available when running the installer from an administrator command shell:</span></span>

   * <span data-ttu-id="a8e3d-324">`OPT_NO_ANCM=1` &ndash; 跳過安裝 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-324">`OPT_NO_ANCM=1` &ndash; Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="a8e3d-325">`OPT_NO_RUNTIME=1` &ndash; 跳過安裝 .NET Core 執行階段。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-325">`OPT_NO_RUNTIME=1` &ndash; Skip installing the .NET Core runtime.</span></span>
   * <span data-ttu-id="a8e3d-326">`OPT_NO_SHAREDFX=1` &ndash; 跳過安裝 ASP.NET 共用架構 (ASP.NET 執行階段)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-326">`OPT_NO_SHAREDFX=1` &ndash; Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span>
   * <span data-ttu-id="a8e3d-327">`OPT_NO_X86=1` &ndash; 跳過安裝 x86 執行階段。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-327">`OPT_NO_X86=1` &ndash; Skip installing x86 runtimes.</span></span> <span data-ttu-id="a8e3d-328">當您確定不會裝載 32 位元應用程式時，請使用此參數。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-328">Use this parameter when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="a8e3d-329">如果將來有可能同時裝載 32 位元和 64 位元應用程式，請不要使用此參數並安裝這兩個執行階段。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-329">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this parameter and install both runtimes.</span></span>
   * <span data-ttu-id="a8e3d-330">`OPT_NO_SHARED_CONFIG_CHECK=1` &ndash; 停用使用 IIS 共用設定 (當共用設定 (*applicationHost.config*) 位於與 IIS 安裝相同的機器上時) 進行檢查。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-330">`OPT_NO_SHARED_CONFIG_CHECK=1` &ndash; Disable the check for using an IIS Shared Configuration when the shared configuration (*applicationHost.config*) is on the same machine as the IIS installation.</span></span> <span data-ttu-id="a8e3d-331">*只在 ASP.NET Core 2.2 或更新版本的裝載套件組合安裝程式上可用。*</span><span class="sxs-lookup"><span data-stu-id="a8e3d-331">*Only available for ASP.NET Core 2.2 or later Hosting Bundler installers.*</span></span> <span data-ttu-id="a8e3d-332">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-332">For more information, see <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>.</span></span>
1. <span data-ttu-id="a8e3d-333">請重新啟動系統，或是從命令殼層依序執行 **net stop was /y** 和 **net start w3svc**。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-333">Restart the system or execute **net stop was /y**, followed by **net start w3svc** from a command shell.</span></span> <span data-ttu-id="a8e3d-334">重新啟動 IIS 將能偵測到由安裝程式對系統路徑 (此為環境變數) 所做出的變更。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-334">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

> [!NOTE]
> <span data-ttu-id="a8e3d-335">如需 IIS 共用組態的資訊，請參閱[使用 IIS 共用組態的 ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-335">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="a8e3d-336">使用 Visual Studio 發佈時安裝 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="a8e3d-336">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="a8e3d-337">將應用程式部署到具有 [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later) 的伺服器時，請在伺服器上安裝最新版的 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-337">When deploying apps to servers with [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="a8e3d-338">若要安裝 Web Deploy，請使用 [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) 或從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=43717)直接取得安裝程式。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-338">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="a8e3d-339">慣用的方法是使用 WebPI。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-339">The preferred method is to use WebPI.</span></span> <span data-ttu-id="a8e3d-340">WebPI 提供獨立的安裝程式和組態以裝載提供者。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-340">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="a8e3d-341">建立 IIS 網站</span><span class="sxs-lookup"><span data-stu-id="a8e3d-341">Create the IIS site</span></span>

1. <span data-ttu-id="a8e3d-342">在主控系統上，請建立資料夾以容納應用程式的已發行資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-342">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="a8e3d-343">在下列步驟中，您提供資料夾路徑給 IIS，作為應用程式的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-343">In a following step, the folder's path is provided to IIS as the physical path to the app.</span></span> <span data-ttu-id="a8e3d-344">如需應用程式之部署資料夾和檔案配置的詳細資訊，請參閱 <xref:host-and-deploy/directory-structure>。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-344">For more information on an app's deployment folder and file layout, see <xref:host-and-deploy/directory-structure>.</span></span>

1. <span data-ttu-id="a8e3d-345">在 [IIS 管理員] 中，於 [連線] 面板中開啟伺服器的節點。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-345">In IIS Manager, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="a8e3d-346">以滑鼠右鍵按一下 [網站] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-346">Right-click the **Sites** folder.</span></span> <span data-ttu-id="a8e3d-347">從操作功能表選取 [新增網站]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-347">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="a8e3d-348">提供**網站名稱**，並將**實體路徑**設定為應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-348">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="a8e3d-349">透過選取 [確定] 來提供**繫結**設定並建立網站：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-349">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![在新增網站步驟中提供站台名稱、實體路徑和主機名稱。](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="a8e3d-351">請**勿**使用最上層萬用字元繫結 (`http://*:80/`與 `http://+:80`)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-351">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="a8e3d-352">最上層萬用字元繫結可能暴露您的應用程式安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-352">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="a8e3d-353">這對強式與弱式萬用字元皆適用。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-353">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="a8e3d-354">請使用明確主機名稱，而非萬用字元。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-354">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="a8e3d-355">若您擁有整個父網域 (與具弱點的 `*.com` 相對) 的控制權，則子網域萬用字元繫結 (例如 `*.mysub.com`) 就沒有此安全性風險。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-355">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="a8e3d-356">如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-356">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="a8e3d-357">在伺服器的節點之下，選取 [應用程式集區]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-357">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="a8e3d-358">以滑鼠右鍵按一下網站的應用程式集區，然後從操作功能表選取 [基本設定]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-358">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="a8e3d-359">在 [編輯應用程式集區] 視窗中，將 [.NET CLR 版本] 設定為 [沒有受控碼]：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-359">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![將 .NET CLR 版本設為 [沒有受控碼]。](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="a8e3d-361">ASP.NET Core 會在不同的處理序中執行，並管理執行階段。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-361">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="a8e3d-362">ASP.NET Core 不仰賴載入桌面 CLR (.NET CLR)&mdash;會使用 .NET Core 的核心通用語言執行平台 (CoreCLR) 來開機以在背景工作處理序中裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-362">ASP.NET Core doesn't rely on loading the desktop CLR (.NET CLR)&mdash;the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process.</span></span> <span data-ttu-id="a8e3d-363">將 [.NET CLR 版本] 設定為 [沒有受控碼] 是選擇性的，但建議這樣做。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-363">Setting the **.NET CLR version** to **No Managed Code** is optional but recommended.</span></span>

1. <span data-ttu-id="a8e3d-364">*ASP.NET Core 2.2 或更新版本*：對於使用[同處理序主控模型](#in-process-hosting-model)的 64 位元 (x64) [自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)，會停用 32 位元 (x86) 處理序的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-364">*ASP.NET Core 2.2 or later*: For a 64-bit (x64) [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) that uses the [in-process hosting model](#in-process-hosting-model), disable the app pool for 32-bit (x86) processes.</span></span>

   <span data-ttu-id="a8e3d-365">在 IIS 管理員的 [動作] 資訊看板 > [應用程式集區] 中，選取 [設定應用程式集區預設值] 或 [進階設定]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-365">In the **Actions** sidebar of IIS Manager > **Application Pools**, select **Set Application Pool Defaults** or **Advanced Settings**.</span></span> <span data-ttu-id="a8e3d-366">找到 [啟用 32 位元應用程式]，然後將其值設定為 `False`。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-366">Locate **Enable 32-Bit Applications** and set the value to `False`.</span></span> <span data-ttu-id="a8e3d-367">此設定不會影響為[處理程序外裝載](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model)部署的應用程式。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-367">This setting doesn't affect apps deployed for [out-of-process hosting](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).</span></span>

1. <span data-ttu-id="a8e3d-368">確認處理序模型身分識別具有適當的權限。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-368">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="a8e3d-369">如果您將應用程式集區的預設身分識別 ([處理序模型] > [身分識別]) 從 **ApplicationPoolIdentity** 變更為其他身分識別，請確認新的身分識別具有必要權限，可存取應用程式的資料夾、資料庫和其他必要的資源。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-369">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="a8e3d-370">例如，應用程式集區需要針對應用程式讀取和寫入檔案的資料夾取得讀取和寫入權限。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-370">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="a8e3d-371">**Windows 驗證設定 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="a8e3d-371">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="a8e3d-372">如需詳細資訊，請參閱[設定 Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-372">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="a8e3d-373">部署應用程式</span><span class="sxs-lookup"><span data-stu-id="a8e3d-373">Deploy the app</span></span>

<span data-ttu-id="a8e3d-374">將應用程式部署至在[建立 IIS 網站](#create-the-iis-site)一節中所建立的 IIS **實體路徑**資料夾。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-374">Deploy the app to the IIS **Physical path** folder that was established in the [Create the IIS site](#create-the-iis-site) section.</span></span> <span data-ttu-id="a8e3d-375">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 是建議的部署機制，但有數個選項可將應用程式從專案的 [publish] 資料夾移動至主機系統的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-375">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment, but several options exist for moving the app from the project's *publish* folder to the hosting system's deployment folder.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="a8e3d-376">使用 Visual Studio 的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="a8e3d-376">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="a8e3d-377">請參閱[適用於 ASP.NET Core 應用程式部署的 Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)主題，了解如何建立搭配 Web Deploy 使用的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-377">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="a8e3d-378">如果主機服務提供者提供或支援建立發行設定檔，請下載其設定檔，並使用 Visual Studio 的 [發行] 對話方塊匯入其設定檔：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-378">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog:</span></span>

![[發佈] 對話方塊頁](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="a8e3d-380">Visual Studio 外部的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="a8e3d-380">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="a8e3d-381">透過命令列也可以在 Visual Studio 外部使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-381">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="a8e3d-382">如需詳細資訊，請參閱 [Web 部署工具](/iis/publish/using-web-deploy/use-the-web-deployment-tool)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-382">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="a8e3d-383">Web Deploy 的替代項目</span><span class="sxs-lookup"><span data-stu-id="a8e3d-383">Alternatives to Web Deploy</span></span>

<span data-ttu-id="a8e3d-384">使用數種方法的其中一種將應用程式移到主機系統，例如手動複製、[Xcopy](/windows-server/administration/windows-commands/xcopy)、[Robocopy](/windows-server/administration/windows-commands/robocopy) 或 [PowerShell](/powershell/)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-384">Use any of several methods to move the app to the hosting system, such as manual copy, [Xcopy](/windows-server/administration/windows-commands/xcopy), [Robocopy](/windows-server/administration/windows-commands/robocopy), or [PowerShell](/powershell/).</span></span>

<span data-ttu-id="a8e3d-385">如需 IIS 的 ASP.NET Core 部署詳細資訊，請參閱 [IIS 系統管理員的部署資源](#deployment-resources-for-iis-administrators)一節。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-385">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="a8e3d-386">瀏覽網站</span><span class="sxs-lookup"><span data-stu-id="a8e3d-386">Browse the website</span></span>

<span data-ttu-id="a8e3d-387">應用程式部署到主機系統之後，請向其中一個應用程式的公用端點發出要求。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-387">After the app is deployed to the hosting system, make a request to one of the app's public endpoints.</span></span>

<span data-ttu-id="a8e3d-388">在下列範例中，網站繫結至 IIS 上的**連接埠** `80`，其**主機名稱**為 `www.mysite.com`。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-388">In the following example, the site is bound to an IIS **Host name** of `www.mysite.com` on **Port** `80`.</span></span> <span data-ttu-id="a8e3d-389">對 `http://www.mysite.com` 發出要求：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-389">A request is made to `http://www.mysite.com`:</span></span>

![Microsoft Edge 瀏覽器已載入 IIS 啟動頁面。](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="a8e3d-391">已鎖定的部署檔案</span><span class="sxs-lookup"><span data-stu-id="a8e3d-391">Locked deployment files</span></span>

<span data-ttu-id="a8e3d-392">當應用程式執行時，會鎖定部署資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-392">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="a8e3d-393">無法於部署期間覆寫已鎖定的檔案。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-393">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="a8e3d-394">若要釋放部署中的已鎖定檔案，請使用下列其中**一種**方法停止應用程式集區：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-394">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="a8e3d-395">使用 Web Deploy 並參考專案檔中的 `Microsoft.NET.Sdk.Web`。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-395">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="a8e3d-396">*app_offline.htm* 檔案是放在 Web 應用程式目錄的根目錄中。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-396">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="a8e3d-397">當檔案存在時，ASP.NET Core 模組會正常關閉應用程式，並在部署期間提供 *app_offline.htm* 檔案。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-397">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="a8e3d-398">如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-398">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="a8e3d-399">在伺服器上的 IIS 管理員中手動停止應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-399">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="a8e3d-400">使用 PowerShell 卸載*app_offline* （需要 PowerShell 5 或更新版本）：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-400">Use PowerShell to drop *app_offline.htm* (requires PowerShell 5 or later):</span></span>

  ```PowerShell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a><span data-ttu-id="a8e3d-401">資料保護</span><span class="sxs-lookup"><span data-stu-id="a8e3d-401">Data protection</span></span>

<span data-ttu-id="a8e3d-402">[ASP.NET Core 資料保護堆疊](xref:security/data-protection/introduction)是由數個 ASP.NET Core [中介軟體](xref:fundamentals/middleware/index)所使用，包括用於驗證的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-402">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="a8e3d-403">即使資料保護 API 不是由使用者程式碼呼叫，仍應使用部署指令碼或是在使用者程式碼中設定資料保護，以建立持續性的密碼編譯[金鑰存放區](xref:security/data-protection/implementation/key-management)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-403">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="a8e3d-404">如不設定資料保護，金鑰會保留在記憶體中，並於應用程式重新啟動時捨棄。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-404">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="a8e3d-405">如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-405">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="a8e3d-406">所有以 Cookie 為基礎的驗證權杖都會失效。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-406">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="a8e3d-407">當使用者提出下一個要求時，需要再次登入。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-407">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="a8e3d-408">所有以 Keyring 保護的資料都無法再解密。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-408">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="a8e3d-409">這可能會包含 [CSRF 權杖](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)和 [ASP.NET Core MVC TempData cookie](xref:fundamentals/app-state#tempdata)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-409">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="a8e3d-410">若要在 IIS 下設定資料保護以保存 Keyring，請使用下列其中**一種**方法：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-410">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="a8e3d-411">**建立資料保護登錄機碼**</span><span class="sxs-lookup"><span data-stu-id="a8e3d-411">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="a8e3d-412">ASP.NET Core 應用程式所使用的資料保護金鑰會儲存在應用程式外部的登錄中。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-412">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="a8e3d-413">若要保存指定應用程式的金鑰，請為應用程式集區建立登錄機碼。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-413">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="a8e3d-414">若為獨立的非Web 伺服陣列 IIS 安裝，請針對搭配使用 ASP.NET Core 應用程式的每個應用程式集區，使用[資料保護 Provision-AutoGenKeys.ps1 PowerShell 指令碼](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-414">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="a8e3d-415">此指令碼會在 HKLM 登錄中建立登錄機碼，只有應用程式之應用程式集區的背景工作處理序帳戶可以存取它。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-415">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="a8e3d-416">在待用期間使用 DPAPI 和全電腦金鑰加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-416">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="a8e3d-417">在 Web 伺服陣列案例中，應用程式可以設定成使用 UNC 路徑來儲存其資料保護 Keyring。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-417">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="a8e3d-418">根據預設，資料保護金鑰不予加密。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-418">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="a8e3d-419">請確保網路共用的檔案權限僅限於執行應用程式的 Windows 帳戶。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-419">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="a8e3d-420">可以使用 X509 憑證來保護待用的金鑰。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-420">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="a8e3d-421">請考慮下列讓使用者上傳憑證的機制：將憑證放入使用者的受信任憑證存放區，並確保在執行使用者應用程式的所有電腦上都能使用這些憑證。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-421">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="a8e3d-422">如需詳細資訊，請參閱[設定 ASP.NET Core 資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-422">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="a8e3d-423">**設定 IIS 應用程式集區載入使用者設定檔**</span><span class="sxs-lookup"><span data-stu-id="a8e3d-423">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="a8e3d-424">此設定位在應用程式集區 [進階設定] 下的 [處理序模型] 區段中。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-424">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="a8e3d-425">將 [載入使用者設定檔] 設為 `True`。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-425">Set **Load User Profile** to `True`.</span></span> <span data-ttu-id="a8e3d-426">當設定為 `True` 時，金鑰會儲存在使用者設定檔目錄中，且使用具有使用者帳戶專屬金鑰的 DPAPI 保護。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-426">When set to `True`, keys are stored in the user profile directory and protected using DPAPI with a key specific to the user account.</span></span> <span data-ttu-id="a8e3d-427">金鑰會保存到 *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-427">Keys are persisted to the *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* folder.</span></span>

  <span data-ttu-id="a8e3d-428">應用程式集區的 [setProfileEnvironment 屬性](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration)也必須啟用。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-428">The app pool's [setProfileEnvironment attribute](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) must also be enabled.</span></span> <span data-ttu-id="a8e3d-429">`setProfileEnvironment` 的預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-429">The default value of `setProfileEnvironment` is `true`.</span></span> <span data-ttu-id="a8e3d-430">在某些情況下 (例如 Windows OS)，`setProfileEnvironment` 會設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-430">In some scenarios (for example, Windows OS), `setProfileEnvironment` is set to `false`.</span></span> <span data-ttu-id="a8e3d-431">如果金鑰並未如預期地儲存在使用者設定檔目錄中：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-431">If keys aren't stored in the user profile directory as expected:</span></span>

  1. <span data-ttu-id="a8e3d-432">瀏覽至 *%windir%/system32/inetsrv/config* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-432">Navigate to the *%windir%/system32/inetsrv/config* folder.</span></span>
  1. <span data-ttu-id="a8e3d-433">開啟 *applicationHost.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-433">Open the *applicationHost.config* file.</span></span>
  1. <span data-ttu-id="a8e3d-434">找到 `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` 項目。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-434">Locate the `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` element.</span></span>
  1. <span data-ttu-id="a8e3d-435">確認 `setProfileEnvironment` 屬性不存在 (其預設值為 `true`)，或明確地將屬性值設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-435">Confirm that the `setProfileEnvironment` attribute isn't present, which defaults the value to `true`, or explicitly set the attribute's value to `true`.</span></span>

* <span data-ttu-id="a8e3d-436">**將檔案系統當作 Keyring 存放區使用**</span><span class="sxs-lookup"><span data-stu-id="a8e3d-436">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="a8e3d-437">調整應用程式程式碼，以[將檔案系統當作 Keyring 存放區使用](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-437">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="a8e3d-438">使用 X509 憑證來保護 Keyring，並確保憑證是受信任的憑證。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-438">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="a8e3d-439">如果憑證為自我簽署，請將憑證放在受信任的根存放區。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-439">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="a8e3d-440">在 Web 伺服陣列中使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-440">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="a8e3d-441">請使用所有電腦都可以存取的檔案共用。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-441">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="a8e3d-442">將 X509 憑證部署到每一部電腦。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-442">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="a8e3d-443">設定[程式碼中的資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-443">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="a8e3d-444">**設定資料保護的全電腦原則**</span><span class="sxs-lookup"><span data-stu-id="a8e3d-444">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="a8e3d-445">針對取用資料保護 API 的所有應用程式，資料保護系統僅支援有限的預設[全電腦原則](xref:security/data-protection/configuration/machine-wide-policy)設定。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-445">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="a8e3d-446">如需詳細資訊，請參閱<xref:security/data-protection/introduction>。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-446">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="virtual-directories"></a><span data-ttu-id="a8e3d-447">虛擬目錄</span><span class="sxs-lookup"><span data-stu-id="a8e3d-447">Virtual Directories</span></span>

<span data-ttu-id="a8e3d-448">ASP.NET Core 應用程式不支援 [IIS 虛擬目錄](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-448">[IIS Virtual Directories](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) aren't supported with ASP.NET Core apps.</span></span> <span data-ttu-id="a8e3d-449">應用程式能以[子應用程式](#sub-applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-449">An app can be hosted as a [sub-application](#sub-applications).</span></span>

## <a name="sub-applications"></a><span data-ttu-id="a8e3d-450">子應用程式</span><span class="sxs-lookup"><span data-stu-id="a8e3d-450">Sub-applications</span></span>

<span data-ttu-id="a8e3d-451">ASP.NET Core 應用程式能以 [IIS 子應用程式](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-451">An ASP.NET Core app can be hosted as an [IIS sub-application (sub-app)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications).</span></span> <span data-ttu-id="a8e3d-452">子應用程式的路徑會成為根應用程式 URL 的一部分。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-452">The sub-app's path becomes part of the root app's URL.</span></span>

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a8e3d-453">子應用程式不應該包括 ASP.NET Core 模組做為處理常式。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-453">A sub-app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="a8e3d-454">如果您在子應用程式的 *web.config* 檔案中將模組新增為處理常式，則嘗試瀏覽子應用程式時，會收到參考錯誤設定檔的 *500.19 內部伺服器錯誤*。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-454">If the module is added as a handler in a sub-app's *web.config* file, a *500.19 Internal Server Error* referencing the faulty config file is received when attempting to browse the sub-app.</span></span>

<span data-ttu-id="a8e3d-455">下列範例顯示 ASP.NET Core 子應用程式的已發佈 *web.config* 檔案：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-455">The following example shows a published *web.config* file for an ASP.NET Core sub-app:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="a8e3d-456">在 ASP.NET Core 應用程式下裝載非 ASP.NET Core 子應用程式時，請明確移除子應用程式 *web.config* 檔案中已繼承的處理常式：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-456">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app's *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore" />
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="a8e3d-457">子應用程式內的靜態資產連結應該使用波狀符號與斜線 (`~/`) 標記法。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-457">Static asset links within the sub-app should use tilde-slash (`~/`) notation.</span></span> <span data-ttu-id="a8e3d-458">波狀符號與斜線標記法會觸發[標記協助程式](xref:mvc/views/tag-helpers/intro)以將子應用程式的路徑基底附加到轉譯的相對連結前面。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-458">Tilde-slash notation triggers a [Tag Helper](xref:mvc/views/tag-helpers/intro) to prepend the sub-app's pathbase to the rendered relative link.</span></span> <span data-ttu-id="a8e3d-459">針對位於 `/subapp_path` 的子應用程式，使用 `src="~/image.png"` 連結的影像會轉譯為 `src="/subapp_path/image.png"`。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-459">For a sub-app at `/subapp_path`, an image linked with `src="~/image.png"` is rendered as `src="/subapp_path/image.png"`.</span></span> <span data-ttu-id="a8e3d-460">根應用程式的靜態檔案中介軟體不會處理靜態檔案要求。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-460">The root app's Static File Middleware doesn't process the static file request.</span></span> <span data-ttu-id="a8e3d-461">要求會由子應用程式的靜態檔案中介軟體處理。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-461">The request is processed by the sub-app's Static File Middleware.</span></span>

<span data-ttu-id="a8e3d-462">若靜態資產的 `src` 屬性是設定為絕對路徑 (例如，`src="/image.png"`)，會以不使用子應用程式路徑基底的方式轉譯連結。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-462">If a static asset's `src` attribute is set to an absolute path (for example, `src="/image.png"`), the link is rendered without the sub-app's pathbase.</span></span> <span data-ttu-id="a8e3d-463">根應用程式的靜態檔案中介軟體會嘗試從根應用程式的 [webroot](xref:fundamentals/index#web-root) 提供資產，這會導致「404 - 找不到」回應 (除非根應用程式可存取靜態資產)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-463">The root app's Static File Middleware attempts to serve the asset from the root app's [web root](xref:fundamentals/index#web-root), which results in a *404 - Not Found* response unless the static asset is available from the root app.</span></span>

<span data-ttu-id="a8e3d-464">裝載 ASP.NET Core 應用程式做為另一個 ASP.NET Core 應用程式下的子應用程式：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-464">To host an ASP.NET Core app as a sub-app under another ASP.NET Core app:</span></span>

1. <span data-ttu-id="a8e3d-465">為子應用程式建立應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-465">Establish an app pool for the sub-app.</span></span> <span data-ttu-id="a8e3d-466">將 [.NET CLR 版本] 設定為 [沒有受控碼]，因為會將核心通用語言執行平台 (CoreCLR) 開機以在背景工作處理序中裝載應用程式，而非在桌面 CLR (.NET CLR) 中裝載。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-466">Set the **.NET CLR Version** to **No Managed Code** because the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process, not the desktop CLR (.NET CLR).</span></span>

1. <span data-ttu-id="a8e3d-467">使用根網站下資料夾中的子應用程式在 IIS 管理員中新增根網站。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-467">Add the root site in IIS Manager with the sub-app in a folder under the root site.</span></span>

1. <span data-ttu-id="a8e3d-468">以滑鼠右鍵按一下 IIS 管理員中的子應用程式資料夾，然後選取 [轉換成應用程式]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-468">Right-click the sub-app folder in IIS Manager and select **Convert to Application**.</span></span>

1. <span data-ttu-id="a8e3d-469">在 [新增應用程式] 對話方塊中，使用 [應用程式集區] 的[選取] 按鈕來指派您為子應用程式建立的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-469">In the **Add Application** dialog, use the **Select** button for the **Application Pool** to assign the app pool that you created for the sub-app.</span></span> <span data-ttu-id="a8e3d-470">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-470">Select **OK**.</span></span>

<span data-ttu-id="a8e3d-471">將不同的應用程式集區指派給子應用程式是使用同處理序裝載模型。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-471">The assignment of a separate app pool to the sub-app is a requirement when using the in-process hosting model.</span></span>

<span data-ttu-id="a8e3d-472">如需有關同處理序裝載模型與如何設定 ASP.NET Core 模組的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module> 與 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-472">For more information on the in-process hosting model and configuring the ASP.NET Core Module, see <xref:host-and-deploy/aspnet-core-module> and <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="a8e3d-473">使用 web.config 的 IIS 組態</span><span class="sxs-lookup"><span data-stu-id="a8e3d-473">Configuration of IIS with web.config</span></span>

<span data-ttu-id="a8e3d-474">在對使用了 ASP.NET Core 模組的 ASP.NET Core 有作用的 IIS 情境下，設定會受 *web.config* 的 `<system.webServer>` 區段影響。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-474">IIS configuration is influenced by the `<system.webServer>` section of *web.config* for IIS scenarios that are functional for ASP.NET Core apps with the ASP.NET Core Module.</span></span> <span data-ttu-id="a8e3d-475">舉例來說，IIS 設定對動態壓縮有作用。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-475">For example, IIS configuration is functional for dynamic compression.</span></span> <span data-ttu-id="a8e3d-476">如果在伺服器層級將 IIS 設為使用動態壓縮，應用程式 *web.config* 檔案中的 `<urlCompression>` 元素則可為 ASP.NET Core 應用程式予以停用。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-476">If IIS is configured at the server level to use dynamic compression, the `<urlCompression>` element in the app's *web.config* file can disable it for an ASP.NET Core app.</span></span>

<span data-ttu-id="a8e3d-477">如需詳細資訊，請參閱 [\<system.webServer> 的設定參考](/iis/configuration/system.webServer/)、[ASP.NET Core 模組設定參考](xref:host-and-deploy/aspnet-core-module)以及 [IIS 模組與 ASP.NET Core](xref:host-and-deploy/iis/modules)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-477">For more information, see the [configuration reference for \<system.webServer>](/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module), and [IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="a8e3d-478">若要設定在隔離的應用程式集區中執行之個別應用程式的環境變數 (支援 IIS 10.0 或更新版本)，請參閱 IIS 參考文件之[環境變數 \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的 *AppCmd.exe 命令*一節。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-478">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="a8e3d-479">web.config 的組態區段</span><span class="sxs-lookup"><span data-stu-id="a8e3d-479">Configuration sections of web.config</span></span>

<span data-ttu-id="a8e3d-480">ASP.NET Core 應用程式的設定不使用 *web.config* 中 ASP.NET 4.x 應用程式的設定區段：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-480">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

<span data-ttu-id="a8e3d-481">使用其他組態提供者設定的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-481">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="a8e3d-482">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-482">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="a8e3d-483">應用程式集區</span><span class="sxs-lookup"><span data-stu-id="a8e3d-483">Application Pools</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a8e3d-484">應用程式集區隔離取決於裝載模型：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-484">App pool isolation is determined by the hosting model:</span></span>

* <span data-ttu-id="a8e3d-485">處理序內裝載 &ndash; 應用程式必須在分開的應用程式集區中執行。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-485">In-process hosting &ndash; Apps are required to run in separate app pools.</span></span>
* <span data-ttu-id="a8e3d-486">處理序外裝載 &ndash; 建議藉由在各自的應用程式集區中執行每個應用程式，以將應用程式互相隔離。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-486">Out-of-process hosting &ndash; We recommend isolating the apps from each other by running each app in its own app pool.</span></span>

<span data-ttu-id="a8e3d-487">IIS [新增網站] 對話方塊預設每個應用程式皆為單一應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-487">The IIS **Add Website** dialog defaults to a single app pool per app.</span></span> <span data-ttu-id="a8e3d-488">當提供 [網站名稱] 時，文字會自動轉移至 [應用程式集區] 文字方塊。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-488">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="a8e3d-489">新增網站時，會使用該網站名稱建立新的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-489">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a8e3d-490">在伺服器上裝載多個網站時，建議您在其各自的應用程式集區中執行各個應用程式，讓應用程式彼此隔離。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-490">When hosting multiple websites on a server, we recommend isolating the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="a8e3d-491">IIS [新增網站] 對話方塊預設成此組態。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-491">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="a8e3d-492">當提供 [網站名稱] 時，文字會自動轉移至 [應用程式集區] 文字方塊。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-492">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="a8e3d-493">新增網站時，會使用該網站名稱建立新的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-493">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

## <a name="application-pool-identity"></a><span data-ttu-id="a8e3d-494">應用程式集區身分識別</span><span class="sxs-lookup"><span data-stu-id="a8e3d-494">Application Pool Identity</span></span>

<span data-ttu-id="a8e3d-495">應用程式集區身分識別帳戶可讓應用程式在唯一的帳戶下執行，不必建立及管理網域或本機帳戶。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-495">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="a8e3d-496">在 IIS 8.0 或更新版本中，IIS 管理背景工作處理序 (WAS) 會使用新的應用程式集區名稱建立虛擬帳戶，並預設在此帳戶下執行應用程式集區的背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-496">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="a8e3d-497">在 IIS 管理主控台中，於應用程式集區的 [進階設定] 下，確定 [身分識別] 設定為使用 **ApplicationPoolIdentity**：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-497">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![應用程式集區進階設定對話方塊](index/_static/apppool-identity.png)

<span data-ttu-id="a8e3d-499">IIS 管理程序會在 Windows 安全系統中，以應用程式集區的名稱建立安全識別碼。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-499">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="a8e3d-500">可使用此身分識別來保護資源。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-500">Resources can be secured using this identity.</span></span> <span data-ttu-id="a8e3d-501">不過，此身分識別不是真正的使用者帳戶，也不會顯示在 Windows 使用者管理主控台中。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-501">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="a8e3d-502">如果 IIS 背景工作處理序需要提升應用程式的存取權限，請修改包含應用程式的目錄存取控制清單 (ACL)：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-502">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="a8e3d-503">開啟 Windows 檔案總管，巡覽至目錄。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-503">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="a8e3d-504">以滑鼠右鍵按一下目錄並選取 [屬性]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-504">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="a8e3d-505">依序選取 [安全性] 索引標籤下的 [編輯] 按鈕和 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-505">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="a8e3d-506">選取 [位置] 按鈕，並確定選取系統。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-506">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="a8e3d-507">在 [輸入要選取的物件名稱] 區域中，輸入 **IIS AppPool\\<app_pool_name>** 。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-507">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="a8e3d-508">選取 [檢查名稱] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-508">Select the **Check Names** button.</span></span> <span data-ttu-id="a8e3d-509">針對 [DefaultAppPool]，請使用 **IIS AppPool\DefaultAppPool** 檢查名稱。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-509">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="a8e3d-510">選取 [檢查名稱] 按鈕時，[DefaultAppPool] 的值便會顯示於物件名稱區域中。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-510">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="a8e3d-511">您無法直接將應用程式集區名稱輸入至物件名稱區域。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-511">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="a8e3d-512">檢查物件名稱時，請使用 **IIS AppPool\\<app_pool_name>** 的格式。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-512">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：在選取 [檢查名稱] 之前，"DefaultAppPool" 這個應用程式集區名稱在物件名稱區域中會附加至 "IIS AppPool\"。](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="a8e3d-514">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-514">Select **OK**.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：選取 [檢查名稱] 之後，物件名稱 "DefaultAppPool" 會顯示在物件名稱區域中。](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="a8e3d-516">預設應該會授與讀取 &amp; 執行權限。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-516">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="a8e3d-517">請視需要提供其他權限。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-517">Provide additional permissions as needed.</span></span>

<span data-ttu-id="a8e3d-518">也可使用 **ICACLS** 工具透過命令提示字元授與存取權限。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-518">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="a8e3d-519">使用 *DefaultAppPool*作為範例，將會使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-519">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="a8e3d-520">如需詳細資訊，請參閱 [icacls](/windows-server/administration/windows-commands/icacls) 主題。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-520">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="http2-support"></a><span data-ttu-id="a8e3d-521">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="a8e3d-521">HTTP/2 support</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a8e3d-522">在下列 IIS 部署案例中，ASP.NET Core 支援 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-522">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following IIS deployment scenarios:</span></span>

* <span data-ttu-id="a8e3d-523">同處理序</span><span class="sxs-lookup"><span data-stu-id="a8e3d-523">In-process</span></span>
  * <span data-ttu-id="a8e3d-524">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a8e3d-524">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="a8e3d-525">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="a8e3d-525">TLS 1.2 or later connection</span></span>
* <span data-ttu-id="a8e3d-526">跨處理序</span><span class="sxs-lookup"><span data-stu-id="a8e3d-526">Out-of-process</span></span>
  * <span data-ttu-id="a8e3d-527">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a8e3d-527">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="a8e3d-528">公開 Edge Server 連線使用 HTTP/2，但是對 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的反向 Proxy 連線使用 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-528">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
  * <span data-ttu-id="a8e3d-529">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="a8e3d-529">TLS 1.2 or later connection</span></span>

<span data-ttu-id="a8e3d-530">針對建立 HTTP/2 連線時的同處理序部署，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會回報 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-530">For an in-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span> <span data-ttu-id="a8e3d-531">針對建立 HTTP/2 連線時的跨處理序部署，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會回報 `HTTP/1.1`。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-531">For an out-of-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="a8e3d-532">如需有關同處理序和跨處理序主控模型的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-532">For more information on the in-process and out-of-process hosting models, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a8e3d-533">[HTTP/2](https://httpwg.org/specs/rfc7540.html) 支援符合下列基本需求的跨處理序部署：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-533">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported for out-of-process deployments that meet the following base requirements:</span></span>

* <span data-ttu-id="a8e3d-534">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a8e3d-534">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
* <span data-ttu-id="a8e3d-535">公開 Edge Server 連線使用 HTTP/2，但是對 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的反向 Proxy 連線使用 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-535">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
* <span data-ttu-id="a8e3d-536">目標 Framework：不適用於跨處理序部署，因為 HTTP/2 連線完全由 IIS 處理。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-536">Target framework: Not applicable to out-of-process deployments, since the HTTP/2 connection is handled entirely by IIS.</span></span>
* <span data-ttu-id="a8e3d-537">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="a8e3d-537">TLS 1.2 or later connection</span></span>

<span data-ttu-id="a8e3d-538">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/1.1`。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-538">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="a8e3d-539">HTTP/2 預設為啟用。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-539">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="a8e3d-540">如果 HTTP/2 連線尚未建立，連線會退為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-540">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="a8e3d-541">如需使用 IIS 部署之 HTTP/2 設定的詳細資訊，請參閱 [IIS 上的 HTTP/2](/iis/get-started/whats-new-in-iis-10/http2-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-541">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="cors-preflight-requests"></a><span data-ttu-id="a8e3d-542">CORS 預檢要求</span><span class="sxs-lookup"><span data-stu-id="a8e3d-542">CORS preflight requests</span></span>

<span data-ttu-id="a8e3d-543">*此節只適用於以 .NET Framework 為目標的 ASP.NET Core 應用程式。*</span><span class="sxs-lookup"><span data-stu-id="a8e3d-543">*This section only applies to ASP.NET Core apps that target the .NET Framework.*</span></span>

<span data-ttu-id="a8e3d-544">針對以 .NET Framework 為目標的 ASP.NET Core 應用程式，在 IIS 中OPTIONS 要求預設不會傳遞到應用程式。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-544">For an ASP.NET Core app that targets the .NET Framework, OPTIONS requests aren't passed to the app by default in IIS.</span></span> <span data-ttu-id="a8e3d-545">若要了解如何在 *web.config* 中設定應用程式的 IIS 處理常式以傳遞 OPTIONS 要求，請參閱[在 ASP.NET Web API 2 中啟用跨原始來源要求：CORS 如何運作](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-545">To learn how to configure the app's IIS handlers in *web.config* to pass OPTIONS requests, see [Enable cross-origin requests in ASP.NET Web API 2: How CORS Works](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="application-initialization-module-and-idle-timeout"></a><span data-ttu-id="a8e3d-546">應用程式初始化模組與閒置逾時</span><span class="sxs-lookup"><span data-stu-id="a8e3d-546">Application Initialization Module and Idle Timeout</span></span>

<span data-ttu-id="a8e3d-547">在 IIS 中由 ASP.NET Core 模組版本 2 裝載時：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-547">When hosted in IIS by the ASP.NET Core Module version 2:</span></span>

* <span data-ttu-id="a8e3d-548">[應用程式初始化模組](#application-initialization-module) &ndash; 應用程式的代管[同處理序](#in-process-hosting-model)或[非同處理序](#out-of-process-hosting-model)，可設定為在背景工作處理序重新啟動或伺服器重新啟動時自動啟動。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-548">[Application Initialization Module](#application-initialization-module) &ndash; App's hosted [in-process](#in-process-hosting-model) or [out-of-process](#out-of-process-hosting-model) can be configured to start automatically on a worker process restart or server restart.</span></span>
* <span data-ttu-id="a8e3d-549">[閒置逾時](#idle-timeout) &ndash; 應用程式的裝載 [同處理序](#in-process-hosting-model)可設定為在無活動期間不逾時。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-549">[Idle Timeout](#idle-timeout) &ndash; App's hosted [in-process](#in-process-hosting-model) can be configured not to timeout during periods of inactivity.</span></span>

### <a name="application-initialization-module"></a><span data-ttu-id="a8e3d-550">應用程式初始化模組</span><span class="sxs-lookup"><span data-stu-id="a8e3d-550">Application Initialization Module</span></span>

<span data-ttu-id="a8e3d-551">*套用到應用程式裝載同處理序與非同處理序。*</span><span class="sxs-lookup"><span data-stu-id="a8e3d-551">*Applies to apps hosted in-process and out-of-process.*</span></span>

<span data-ttu-id="a8e3d-552">[IIS 應用程式初始化](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)是一項 IIS 功能，可在應用程式集區啟動或回收時，將 HTTP 要求傳送給應用程式。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-552">[IIS Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) is an IIS feature that sends an HTTP request to the app when the app pool starts or is recycled.</span></span> <span data-ttu-id="a8e3d-553">要求會觸發應用程式啟動。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-553">The request triggers the app to start.</span></span> <span data-ttu-id="a8e3d-554">根據預設，IIS 會發出應用程式根 URL (`/`) 的要求以初始化應用程式 (如需有關設定的詳細資訊，請參閱[額外資源](#application-initialization-module-and-idle-timeout-additional-resources))。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-554">By default, IIS issues a request to the app's root URL (`/`) to initialize the app (see the [additional resources](#application-initialization-module-and-idle-timeout-additional-resources) for more details on configuration).</span></span>

<span data-ttu-id="a8e3d-555">確認已啟用「IIS 應用程式初始化」角色功能：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-555">Confirm that the IIS Application Initialization role feature in enabled:</span></span>

<span data-ttu-id="a8e3d-556">在 Windows 7 或更新的電腦系統上，當在本機使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-556">On Windows 7 or later desktop systems when using IIS locally:</span></span>

1. <span data-ttu-id="a8e3d-557">瀏覽到 [控制台] > [程式] > [程式和功能] > **[開啟或關閉 Windows 功能]** \(畫面左側\)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-557">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="a8e3d-558">開啟 [網際網路資訊服務]> [World Wide Web 服務]> [應用程式開發功能]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-558">Open **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="a8e3d-559">選取 [應用程式初始化]的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-559">Select the check box for **Application Initialization**.</span></span>

<span data-ttu-id="a8e3d-560">在 Windows Server 2008 R2 或更新版本上：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-560">On Windows Server 2008 R2 or later:</span></span>

1. <span data-ttu-id="a8e3d-561">開啟「新增角色與功能精靈」。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-561">Open the **Add Roles and Features Wizard**.</span></span>
1. <span data-ttu-id="a8e3d-562">在 [選取角色服務] 面板中，開啟 [應用程式開發] 節點。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-562">In the **Select role services** panel, open the **Application Development** node.</span></span>
1. <span data-ttu-id="a8e3d-563">選取 [應用程式初始化]的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-563">Select the check box for **Application Initialization**.</span></span>

<span data-ttu-id="a8e3d-564">使用下列任一方式為網站啟用應用程式初始化模組：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-564">Use either of the following approaches to enable the Application Initialization Module for the site:</span></span>

* <span data-ttu-id="a8e3d-565">使用 IIS 管理員：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-565">Using IIS Manager:</span></span>

  1. <span data-ttu-id="a8e3d-566">選取 [連線] 面板中的 [應用程式集區]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-566">Select **Application Pools** in the **Connections** panel.</span></span>
  1. <span data-ttu-id="a8e3d-567">以滑鼠右鍵按一下清單中應用程式的應用程式集區，然後選取 [進階設定]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-567">Right-click the app's app pool in the list and select **Advanced Settings**.</span></span>
  1. <span data-ttu-id="a8e3d-568">預設的 [啟動模式] 是 [OnDemand]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-568">The default **Start Mode** is **OnDemand**.</span></span> <span data-ttu-id="a8e3d-569">將 [啟動模式] 設定為 [AlwaysRunning]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-569">Set the **Start Mode** to **AlwaysRunning**.</span></span> <span data-ttu-id="a8e3d-570">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-570">Select **OK**.</span></span>
  1. <span data-ttu-id="a8e3d-571">開啟 [連線] 面板中的 [站台] 節點。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-571">Open the **Sites** node in the **Connections** panel.</span></span>
  1. <span data-ttu-id="a8e3d-572">以滑鼠右鍵按一下應用程式，然後選取 [管理網站] > [進階設定]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-572">Right-click the app and select **Manage Website** > **Advanced Settings**.</span></span>
  1. <span data-ttu-id="a8e3d-573">預設 [預先載入已啟用] 設定是 [False]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-573">The default **Preload Enabled** setting is **False**.</span></span> <span data-ttu-id="a8e3d-574">將 [預先載入已啟用] 設定為 [True]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-574">Set **Preload Enabled** to **True**.</span></span> <span data-ttu-id="a8e3d-575">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-575">Select **OK**.</span></span>

* <span data-ttu-id="a8e3d-576">使用 *web.config*，新增 `<applicationInitialization>` 元素並將 `doAppInitAfterRestart` 設定為 `true` 至應用程式 *web.config* 檔案中的 `<system.webServer>` 元素：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-576">Using *web.config*, add the `<applicationInitialization>` element with `doAppInitAfterRestart` set to `true` to the `<system.webServer>` elements in the app's *web.config* file:</span></span>

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <configuration>
    <location path="." inheritInChildApplications="false">
      <system.webServer>
        <applicationInitialization doAppInitAfterRestart="true" />
      </system.webServer>
    </location>
  </configuration>
  ```

### <a name="idle-timeout"></a><span data-ttu-id="a8e3d-577">閒置逾時</span><span class="sxs-lookup"><span data-stu-id="a8e3d-577">Idle Timeout</span></span>

<span data-ttu-id="a8e3d-578">*僅適用於應用程式裝載同處理序。*</span><span class="sxs-lookup"><span data-stu-id="a8e3d-578">*Only applies to apps hosted in-process.*</span></span>

<span data-ttu-id="a8e3d-579">若要防止應用程式閒置，請使用 IIS 管理員設定應用程式集區的閒置逾時：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-579">To prevent the app from idling, set the app pool's idle timeout using IIS Manager:</span></span>

1. <span data-ttu-id="a8e3d-580">選取 [連線] 面板中的 [應用程式集區]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-580">Select **Application Pools** in the **Connections** panel.</span></span>
1. <span data-ttu-id="a8e3d-581">以滑鼠右鍵按一下清單中應用程式的應用程式集區，然後選取 [進階設定]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-581">Right-click the app's app pool in the list and select **Advanced Settings**.</span></span>
1. <span data-ttu-id="a8e3d-582">預設 [閒置逾時 (分鐘)] 是 **20** 分鐘。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-582">The default **Idle Time-out (minutes)** is **20** minutes.</span></span> <span data-ttu-id="a8e3d-583">將 [閒置逾時 (分鐘)] 設定為 **0** (零)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-583">Set the **Idle Time-out (minutes)** to **0** (zero).</span></span> <span data-ttu-id="a8e3d-584">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-584">Select **OK**.</span></span>
1. <span data-ttu-id="a8e3d-585">回收背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-585">Recycle the worker process.</span></span>

<span data-ttu-id="a8e3d-586">若要防止應用程式裝載[非同處理序](#out-of-process-hosting-model)逾時，請使用下列任一方式：</span><span class="sxs-lookup"><span data-stu-id="a8e3d-586">To prevent apps hosted [out-of-process](#out-of-process-hosting-model) from timing out, use either of the following approaches:</span></span>

* <span data-ttu-id="a8e3d-587">從外部服務對應用程式執行 Ping 以讓它繼續執行。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-587">Ping the app from an external service in order to keep it running.</span></span>
* <span data-ttu-id="a8e3d-588">若應用程式只裝載背景服務，避免 IIS 裝載並使用 [Windows 服務來裝載 ASP.NET Core 應用程式](xref:host-and-deploy/windows-service)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-588">If the app only hosts background services, avoid IIS hosting and use a [Windows Service to host the ASP.NET Core app](xref:host-and-deploy/windows-service).</span></span>

### <a name="application-initialization-module-and-idle-timeout-additional-resources"></a><span data-ttu-id="a8e3d-589">應用程式初始化模組與閒置逾時額外資源</span><span class="sxs-lookup"><span data-stu-id="a8e3d-589">Application Initialization Module and Idle Timeout additional resources</span></span>

* [<span data-ttu-id="a8e3d-590">IIS 8.0 應用程式初始化</span><span class="sxs-lookup"><span data-stu-id="a8e3d-590">IIS 8.0 Application Initialization</span></span>](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)
* <span data-ttu-id="a8e3d-591">[應用程式初始化 \<applicationInitialization>](/iis/configuration/system.webserver/applicationinitialization/)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-591">[Application Initialization \<applicationInitialization>](/iis/configuration/system.webserver/applicationinitialization/).</span></span>
* <span data-ttu-id="a8e3d-592">[應用程式集區的處理序模組設定 \<processModel>](/iis/configuration/system.applicationhost/applicationpools/add/processmodel)。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-592">[Process Model Settings for an Application Pool \<processModel>](/iis/configuration/system.applicationhost/applicationpools/add/processmodel).</span></span>

::: moniker-end

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="a8e3d-593">IIS 系統管理員的部署資源</span><span class="sxs-lookup"><span data-stu-id="a8e3d-593">Deployment resources for IIS administrators</span></span>

<span data-ttu-id="a8e3d-594">請參閱 IIS 文件以深入了解 IIS。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-594">Learn about IIS in-depth in the IIS documentation.</span></span>  
[<span data-ttu-id="a8e3d-595">IIS 文件</span><span class="sxs-lookup"><span data-stu-id="a8e3d-595">IIS documentation</span></span>](/iis)

<span data-ttu-id="a8e3d-596">了解 .NET Core 應用程式部署模型。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-596">Learn about .NET Core app deployment models.</span></span>  
[<span data-ttu-id="a8e3d-597">.NET Core 應用程式部署</span><span class="sxs-lookup"><span data-stu-id="a8e3d-597">.NET Core application deployment</span></span>](/dotnet/core/deploying/)

<span data-ttu-id="a8e3d-598">了解 ASP.NET Core 模組如何讓 Kestrel Web 伺服器將 IIS 或 IIS Express 作為反向 Proxy 伺服器使用。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-598">Learn how the ASP.NET Core Module allows the Kestrel web server to use IIS or IIS Express as a reverse proxy server.</span></span>  
[<span data-ttu-id="a8e3d-599">ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="a8e3d-599">ASP.NET Core Module</span></span>](xref:host-and-deploy/aspnet-core-module)

<span data-ttu-id="a8e3d-600">了解如何設定 ASP.NET Core 模組以裝載 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-600">Learn how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span>  
[<span data-ttu-id="a8e3d-601">ASP.NET Core 模組組態參考</span><span class="sxs-lookup"><span data-stu-id="a8e3d-601">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)

<span data-ttu-id="a8e3d-602">了解已發行之 ASP.NET Core 應用程式的目錄結構。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-602">Learn about the directory structure of published ASP.NET Core apps.</span></span>  
[<span data-ttu-id="a8e3d-603">目錄結構</span><span class="sxs-lookup"><span data-stu-id="a8e3d-603">Directory structure</span></span>](xref:host-and-deploy/directory-structure)

<span data-ttu-id="a8e3d-604">探索 ASP.NET Core 應用程式的使用中和非使用中 IIS 模組，管理 IIS 模組的方式。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-604">Discover active and inactive IIS modules for ASP.NET Core apps and how to manage IIS modules.</span></span>  
[<span data-ttu-id="a8e3d-605">IIS 模組</span><span class="sxs-lookup"><span data-stu-id="a8e3d-605">IIS modules</span></span>](xref:host-and-deploy/iis/modules)

<span data-ttu-id="a8e3d-606">了解如何診斷 ASP.NET Core 應用程式的 IIS 部署問題。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-606">Learn how to diagnose problems with IIS deployments of ASP.NET Core apps.</span></span>  
[<span data-ttu-id="a8e3d-607">疑難排解</span><span class="sxs-lookup"><span data-stu-id="a8e3d-607">Troubleshoot</span></span>](xref:test/troubleshoot-azure-iis)

<span data-ttu-id="a8e3d-608">區分在 IIS 上裝載 ASP.NET Core 應用程式時的常見錯誤。</span><span class="sxs-lookup"><span data-stu-id="a8e3d-608">Distinguish common errors when hosting ASP.NET Core apps on IIS.</span></span>  
[<span data-ttu-id="a8e3d-609">Azure App Service 和 IIS 常見的錯誤參考</span><span class="sxs-lookup"><span data-stu-id="a8e3d-609">Common errors reference for Azure App Service and IIS</span></span>](xref:host-and-deploy/azure-iis-errors-reference)

## <a name="additional-resources"></a><span data-ttu-id="a8e3d-610">其他資源</span><span class="sxs-lookup"><span data-stu-id="a8e3d-610">Additional resources</span></span>

* <xref:test/troubleshoot>
* [<span data-ttu-id="a8e3d-611">ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="a8e3d-611">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="a8e3d-612">Microsoft IIS 官方網站</span><span class="sxs-lookup"><span data-stu-id="a8e3d-612">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="a8e3d-613">Windows Server 技術內容庫</span><span class="sxs-lookup"><span data-stu-id="a8e3d-613">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="a8e3d-614">ISS 上的 HTTP/2</span><span class="sxs-lookup"><span data-stu-id="a8e3d-614">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
* <xref:host-and-deploy/iis/transform-webconfig>
