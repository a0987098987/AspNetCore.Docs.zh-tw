---
title: ASP.NET Core 中的網頁伺服器實作
author: guardrex
description: 探索 ASP.NET Core 的網頁伺服器 Kestrel 與 HTTP.sys。 了解如何選擇伺服器，以及何時使用反向 Proxy 伺服器。
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/14/2019
uid: fundamentals/servers/index
ms.openlocfilehash: da5be57fa728a4bc075a580cb9b57301046b4132
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64882493"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="a8ec1-104">ASP.NET Core 中的網頁伺服器實作</span><span class="sxs-lookup"><span data-stu-id="a8ec1-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="a8ec1-105">由 [Tom Dykstra](https://github.com/tdykstra)、[Steve Smith](https://ardalis.com/)、[Stephen Halter](https://twitter.com/halter73) 和 [Chris Ross](https://github.com/Tratcher) 提供</span><span class="sxs-lookup"><span data-stu-id="a8ec1-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="a8ec1-106">ASP.NET Core 應用程式執行時，需使用內含式 HTTP 伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="a8ec1-107">伺服器實作會接聽 HTTP 要求，並以組成 <xref:Microsoft.AspNetCore.Http.HttpContext> 的一組[要求功能](xref:fundamentals/request-features)形式向應用程式呈現。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-107">The server implementation listens for HTTP requests and surfaces them to the app as a set of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="a8ec1-108">Windows</span><span class="sxs-lookup"><span data-stu-id="a8ec1-108">Windows</span></span>](#tab/windows)

<span data-ttu-id="a8ec1-109">ASP.NET Core 隨附下列項目：</span><span class="sxs-lookup"><span data-stu-id="a8ec1-109">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="a8ec1-110">[Kestrel 伺服器](xref:fundamentals/servers/kestrel)是預設、跨平台的 HTTP 伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-110">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server implementation.</span></span>
* <span data-ttu-id="a8ec1-111">IIS HTTP 伺服器是 IIS 的[同處理序伺服器](#in-process-hosting-model)。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-111">IIS HTTP Server is an [in-process server](#in-process-hosting-model) for IIS.</span></span>
* <span data-ttu-id="a8ec1-112">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys)是以 [HTTP.sys 核心驅動程式與 HTTP 伺服器 API](/windows/desktop/Http/http-api-start-page) 為基礎的僅限 Windows HTTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](/windows/desktop/Http/http-api-start-page).</span></span>

<span data-ttu-id="a8ec1-113">使用 [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) 或 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 時，應用程式可能會執行於：</span><span class="sxs-lookup"><span data-stu-id="a8ec1-113">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the app either runs:</span></span>

* <span data-ttu-id="a8ec1-114">與 IIS 背景工作處理序相同的處理序 ([同處理序裝載模型](#in-process-hosting-model))，並搭配 [IIS HTTP 伺服器](#iis-http-server)。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-114">In the same process as the IIS worker process (the [in-process hosting model](#in-process-hosting-model)) with the [IIS HTTP Server](#iis-http-server).</span></span> <span data-ttu-id="a8ec1-115">「同處理序」是建議的設定。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-115">*In-process* is the recommended configuration.</span></span>
* <span data-ttu-id="a8ec1-116">從 IIS 背景工作處理序中分離出的處理序 ([跨處理序裝載模型](#out-of-process-hosting-model))，並搭配 [Kestrel 伺服器](#kestrel)。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-116">In a process separate from the IIS worker process (the [out-of-process hosting model](#out-of-process-hosting-model)) with the [Kestrel server](#kestrel).</span></span>

<span data-ttu-id="a8ec1-117">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)是一種原生 IIS 模組，可處理 IIS 與同處理序 IIS HTTP 伺服器或 Kestrel 之間的原生 IIS 要求。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-117">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is a native IIS module that handles native IIS requests between IIS and the in-process IIS HTTP Server or Kestrel.</span></span> <span data-ttu-id="a8ec1-118">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-118">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="hosting-models"></a><span data-ttu-id="a8ec1-119">裝載模型</span><span class="sxs-lookup"><span data-stu-id="a8ec1-119">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="a8ec1-120">同處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="a8ec1-120">In-process hosting model</span></span>

<span data-ttu-id="a8ec1-121">使用同處理序裝載，ASP.NET Core 應用程式會在與其 IIS 工作者處理序相同的處理序中執行。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-121">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="a8ec1-122">因為要求未透過回送介面卡 (將連出網路流量傳回同一部電腦的網路介面) 進行 proxy 處理，所以同處理序裝載會提供優於跨處理序裝載的效能。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-122">In-process hosting provides improved performance over out-of-process hosting because requests aren't proxied over the loopback adapter, a network interface that returns outgoing network traffic back to the same machine.</span></span> <span data-ttu-id="a8ec1-123">IIS 透過 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 來執行處理程序管理。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-123">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="a8ec1-124">ASP.NET Core 模組：</span><span class="sxs-lookup"><span data-stu-id="a8ec1-124">The ASP.NET Core Module:</span></span>

* <span data-ttu-id="a8ec1-125">執行應用程式初始化。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-125">Performs app initialization.</span></span>
  * <span data-ttu-id="a8ec1-126">載入 [CoreCLR](/dotnet/standard/glossary#coreclr)。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-126">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="a8ec1-127">呼叫 `Program.Main`。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-127">Calls `Program.Main`.</span></span>
* <span data-ttu-id="a8ec1-128">處理 IIS 原生要求的存留期。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-128">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="a8ec1-129">以 .NET Framework 為目標的 ASP.NET Core 應用程式不支援處理序內裝載模型。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-129">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="a8ec1-130">下圖說明 IIS、ASP.NET Core 模組和同處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="a8ec1-130">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![ASP.NET Core 模組](_static/ancm-inprocess.png)

<span data-ttu-id="a8ec1-132">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-132">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="a8ec1-133">驅動程式會在網站設定的連接埠上將原生要求路由至 IIS，此連接埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-133">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="a8ec1-134">模組會接收原生要求，並將它傳遞至 IIS HTTP 伺服器 (`IISHttpServer`)。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-134">The module receives the native request and passes it to IIS HTTP Server (`IISHttpServer`).</span></span> <span data-ttu-id="a8ec1-135">IIS HTTP 伺服器是 IIS 的同處理序伺服程式實作，可將要求從原生轉換為受控。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-135">IIS HTTP Server is an in-process server implementation for IIS that converts the request from native to managed.</span></span>

<span data-ttu-id="a8ec1-136">IIS HTTP 伺服器處理要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-136">After the IIS HTTP Server processes the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="a8ec1-137">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-137">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="a8ec1-138">應用程式的回應會透過 IIS HTTP 伺服器傳回 IIS。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-138">The app's response is passed back to IIS through IIS HTTP Server.</span></span> <span data-ttu-id="a8ec1-139">IIS 會將回應傳送到起始該要求的用戶端。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-139">IIS sends the response to the client that initiated the request.</span></span>

<span data-ttu-id="a8ec1-140">現有的應用程式可以選擇同處理序裝載，但 [dotnet new](/dotnet/core/tools/dotnet-new) 範本預設會對所有 IIS 和 IIS Express 案例使用同處理序裝載模型。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-140">In-process hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="a8ec1-141">跨處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="a8ec1-141">Out-of-process hosting model</span></span>

<span data-ttu-id="a8ec1-142">因為 ASP.NET Core 應用程式執行所在的處理序會與 IIS 工作者處理序分開，所以此模組會執行處理程序管理。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-142">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="a8ec1-143">此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式關閉或損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-143">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="a8ec1-144">此行為基本上與執行同處理序，並由 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-144">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="a8ec1-145">下圖說明 IIS、ASP.NET Core 模組和跨處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="a8ec1-145">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![ASP.NET Core 模組](_static/ancm-outofprocess.png)

<span data-ttu-id="a8ec1-147">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-147">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="a8ec1-148">驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-148">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="a8ec1-149">此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，而且不會是通訊埠 80 或 443。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-149">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="a8ec1-150">此模組在啟動時透過環境變數指定通訊埠，而 IIS 整合中介軟體則會設定伺服器來接聽 `http://localhost:{PORT}`。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-150">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{PORT}`.</span></span> <span data-ttu-id="a8ec1-151">將會執行額外檢查，不是源自模組的要求都會遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-151">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="a8ec1-152">此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-152">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="a8ec1-153">Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-153">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="a8ec1-154">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-154">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="a8ec1-155">IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-155">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="a8ec1-156">應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-156">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="a8ec1-157">如需 IIS 和 ASP.NET Core 模組的設定指南，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="a8ec1-157">For IIS and ASP.NET Core Module configuration guidance, see the following topics:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

# <a name="macostabmacos"></a>[<span data-ttu-id="a8ec1-158">macOS</span><span class="sxs-lookup"><span data-stu-id="a8ec1-158">macOS</span></span>](#tab/macos)

<span data-ttu-id="a8ec1-159">ASP.NET Core 隨附 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)，這是預設、跨平台的 HTTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-159">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="a8ec1-160">Linux</span><span class="sxs-lookup"><span data-stu-id="a8ec1-160">Linux</span></span>](#tab/linux)

<span data-ttu-id="a8ec1-161">ASP.NET Core 隨附 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)，這是預設、跨平台的 HTTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-161">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="a8ec1-162">Windows</span><span class="sxs-lookup"><span data-stu-id="a8ec1-162">Windows</span></span>](#tab/windows)

<span data-ttu-id="a8ec1-163">ASP.NET Core 隨附下列項目：</span><span class="sxs-lookup"><span data-stu-id="a8ec1-163">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="a8ec1-164">[Kestrel 伺服器](xref:fundamentals/servers/kestrel)是預設、跨平台的 HTTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-164">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server.</span></span>
* <span data-ttu-id="a8ec1-165">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys)是以 [HTTP.sys 核心驅動程式與 HTTP 伺服器 API](/windows/desktop/Http/http-api-start-page) 為基礎的僅限 Windows HTTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-165">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](/windows/desktop/Http/http-api-start-page).</span></span>

<span data-ttu-id="a8ec1-166">在使用 [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) 或 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 時，應用程式會執行於從 IIS 背景工作處理序中分離出的處理序 (跨處理序)，並搭配 [Kestrel 伺服器](#kestrel)。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-166">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the app runs in a process separate from the IIS worker process (*out-of-process*) with the [Kestrel server](#kestrel).</span></span>

<span data-ttu-id="a8ec1-167">因為 ASP.NET Core 應用程式執行所在的處理序會與 IIS 工作者處理序分開，所以此模組會執行處理程序管理。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-167">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="a8ec1-168">此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式關閉或損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-168">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="a8ec1-169">此行為基本上與執行同處理序，並由 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-169">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="a8ec1-170">下圖說明 IIS、ASP.NET Core 模組和跨處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="a8ec1-170">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![ASP.NET Core 模組](_static/ancm-outofprocess.png)

<span data-ttu-id="a8ec1-172">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-172">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="a8ec1-173">驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-173">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="a8ec1-174">此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，而且不會是通訊埠 80 或 443。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-174">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="a8ec1-175">此模組在啟動時透過環境變數指定通訊埠，而 IIS 整合中介軟體則會設定伺服器來接聽 `http://localhost:{port}`。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-175">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="a8ec1-176">將會執行額外檢查，不是源自模組的要求都會遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-176">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="a8ec1-177">此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-177">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="a8ec1-178">Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-178">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="a8ec1-179">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-179">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="a8ec1-180">IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-180">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="a8ec1-181">應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-181">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="a8ec1-182">如需 IIS 和 ASP.NET Core 模組的設定指南，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="a8ec1-182">For IIS and ASP.NET Core Module configuration guidance, see the following topics:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

# <a name="macostabmacos"></a>[<span data-ttu-id="a8ec1-183">macOS</span><span class="sxs-lookup"><span data-stu-id="a8ec1-183">macOS</span></span>](#tab/macos)

<span data-ttu-id="a8ec1-184">ASP.NET Core 隨附 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)，這是預設、跨平台的 HTTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-184">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="a8ec1-185">Linux</span><span class="sxs-lookup"><span data-stu-id="a8ec1-185">Linux</span></span>](#tab/linux)

<span data-ttu-id="a8ec1-186">ASP.NET Core 隨附 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)，這是預設、跨平台的 HTTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-186">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

## <a name="kestrel"></a><span data-ttu-id="a8ec1-187">Kestrel</span><span class="sxs-lookup"><span data-stu-id="a8ec1-187">Kestrel</span></span>

<span data-ttu-id="a8ec1-188">Kestrel 是內含於 ASP.NET Core 專案範本中的預設網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-188">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="a8ec1-189">Kestrel 的用法有：</span><span class="sxs-lookup"><span data-stu-id="a8ec1-189">Kestrel can be used:</span></span>

* <span data-ttu-id="a8ec1-190">供本身當作直接從網路 (包括網際網路) 處理要求的邊緣伺服器。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-190">By itself as an edge server processing requests directly from a network, including the Internet.</span></span>

  ![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

* <span data-ttu-id="a8ec1-192">搭配「反向 Proxy 伺服器」使用，例如 [Internet Information Services (IIS)](https://www.iis.net/)、[Nginx](http://nginx.org) 或 [Apache](https://httpd.apache.org/)。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-192">With a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="a8ec1-193">反向 Proxy 伺服器會從網際網路接收 HTTP 要求，然後轉送到 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-193">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

  ![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="a8ec1-195">不論裝載設定是否具有反向 Proxy 伺服器，ASP.NET Core 2.1 或更新版的應用程式均予以支援。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-195">Either hosting configuration&mdash;with or without a reverse proxy server&mdash;is supported for ASP.NET Core 2.1 or later apps.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="a8ec1-196">如果應用程式只接受來自內部網路的要求，就可單獨使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-196">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![Kestrel 直接與內部網路通訊](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="a8ec1-198">如果應用程式會向網際網路公開，Kestrel 就必須使用「反向 Proxy 伺服器」，例如 [Internet Information Services (IIS)](https://www.iis.net/)、[Nginx](http://nginx.org) 或 [Apache](https://httpd.apache.org/)。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-198">If the app is exposed to the Internet, Kestrel must use a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="a8ec1-199">反向 Proxy 伺服器會從網際網路接收 HTTP 要求，然後轉送到 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-199">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="a8ec1-201">使用反向 Proxy 進行公眾對應 Edge Server 部署 (直接公開到網際網路) 的最重要理由是安全性。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-201">The most important reason for using a reverse proxy for public-facing edge server deployments that are exposed directly the Internet is security.</span></span> <span data-ttu-id="a8ec1-202">1.x 版的 Kestrel 並不包含可防禦網際網路攻擊的重要安全性功能。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-202">The 1.x versions of Kestrel don't include important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="a8ec1-203">這包括但不限於適當的逾時、要求大小限制和同時連線限制。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-203">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

::: moniker-end

<span data-ttu-id="a8ec1-204">如需 Kestrel 設定指南及資訊，以了解在反向 Proxy 設定中使用 Kestrel 的時機，請參閱 <xref:fundamentals/servers/kestrel>。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-204">For Kestrel configuration guidance and information on when to use Kestrel in a reverse proxy configuration, see <xref:fundamentals/servers/kestrel>.</span></span>

### <a name="nginx-with-kestrel"></a><span data-ttu-id="a8ec1-205">Nginx 與 Kestrel</span><span class="sxs-lookup"><span data-stu-id="a8ec1-205">Nginx with Kestrel</span></span>

<span data-ttu-id="a8ec1-206">如需如何在 Linux 上使用 Nginx 作為 Kestrel 反向 Proxy 伺服器的資訊，請參閱 <xref:host-and-deploy/linux-nginx>。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-206">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-nginx>.</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="a8ec1-207">Apache 與 Kestrel</span><span class="sxs-lookup"><span data-stu-id="a8ec1-207">Apache with Kestrel</span></span>

<span data-ttu-id="a8ec1-208">如需如何在 Linux 上使用 Apache 作為 Kestrel 反向 Proxy 伺服器的資訊，請參閱 <xref:host-and-deploy/linux-apache>。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-208">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-apache>.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="iis-http-server"></a><span data-ttu-id="a8ec1-209">IIS HTTP 伺服器</span><span class="sxs-lookup"><span data-stu-id="a8ec1-209">IIS HTTP Server</span></span>

<span data-ttu-id="a8ec1-210">IIS HTTP 伺服器是 IIS 的[同處理序伺服器](#in-process-hosting-model)，對於同處理序部署不可或缺。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-210">IIS HTTP Server is an [in-process server](#in-process-hosting-model) for IIS and required for in-process deployments.</span></span> <span data-ttu-id="a8ec1-211">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)會處理 IIS 與 IIS HTTP 伺服器之間的原生 IIS 要求。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-211">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) handles native IIS requests between IIS and IIS HTTP Server.</span></span> <span data-ttu-id="a8ec1-212">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-212">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

## <a name="httpsys"></a><span data-ttu-id="a8ec1-213">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="a8ec1-213">HTTP.sys</span></span>

<span data-ttu-id="a8ec1-214">如果您在 Windows 上執行 ASP.NET Core 應用程式，則 HTTP.sys 是 Kestrel 的替代方案。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-214">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="a8ec1-215">通常建議使用 Kestrel 以達到最佳效能。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-215">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="a8ec1-216">HTTP.sys 可以用於下列情況：應用程式公開到網際網路，且必要功能是由 HTTP.sys 而非 Kestrel 支援。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-216">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="a8ec1-217">如需詳細資訊，請參閱<xref:fundamentals/servers/httpsys>。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-217">For more information, see <xref:fundamentals/servers/httpsys>.</span></span>

![HTTP.sys 直接與網際網路通訊](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="a8ec1-219">HTTP.sys 也可用於只公開到內部網路的應用程式。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-219">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![HTTP.sys 直接與內部網路通訊](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="a8ec1-221">如需 HTTP.sys 設定指南，請參閱 <xref:fundamentals/servers/httpsys>。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-221">For HTTP.sys configuration guidance, see <xref:fundamentals/servers/httpsys>.</span></span>

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="a8ec1-222">ASP.NET Core 伺服器基礎結構</span><span class="sxs-lookup"><span data-stu-id="a8ec1-222">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="a8ec1-223">`Startup.Configure` 方法提供的 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 會公開類型<xref:Microsoft.AspNetCore.Http.Features.IFeatureCollection> 的 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ServerFeatures> 屬性。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-223">The <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> available in the `Startup.Configure` method exposes the <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ServerFeatures> property of type <xref:Microsoft.AspNetCore.Http.Features.IFeatureCollection>.</span></span> <span data-ttu-id="a8ec1-224">Kestrel 與 HTTP.sys 只會公開 <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> 功能，但不同的伺服器實作則可能會公開更多的功能。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-224">Kestrel and HTTP.sys only expose a single feature each, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>, but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="a8ec1-225">`IServerAddressesFeature` 可用來找出伺服器實作在執行階段已繫結的連接埠。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-225">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="a8ec1-226">自訂伺服器</span><span class="sxs-lookup"><span data-stu-id="a8ec1-226">Custom servers</span></span>

<span data-ttu-id="a8ec1-227">如果內建伺服器不符合應用程式的需求，則可以建立自訂伺服器實作。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-227">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="a8ec1-228">[Open Web Interface for .NET (OWIN) 指南](xref:fundamentals/owin)示範如何撰寫採用 [Nowin](https://github.com/Bobris/Nowin) 的 <xref:Microsoft.AspNetCore.Hosting.Server.IServer> 實作。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-228">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation.</span></span> <span data-ttu-id="a8ec1-229">只有該應用程式使用的功能介面才需要實作，但至少須支援 <xref:Microsoft.AspNetCore.Http.Features.IHttpRequestFeature> 及 <xref:Microsoft.AspNetCore.Http.Features.IHttpResponseFeature>。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-229">Only the feature interfaces that the app uses require implementation, though at a minimum <xref:Microsoft.AspNetCore.Http.Features.IHttpRequestFeature> and <xref:Microsoft.AspNetCore.Http.Features.IHttpResponseFeature> must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="a8ec1-230">伺服器啟動</span><span class="sxs-lookup"><span data-stu-id="a8ec1-230">Server startup</span></span>

<span data-ttu-id="a8ec1-231">伺服器會在整合式開發環境 (IDE) 或編輯器啟動應用程式時啟動：</span><span class="sxs-lookup"><span data-stu-id="a8ec1-231">The server is launched when the Integrated Development Environment (IDE) or editor starts the app:</span></span>

* <span data-ttu-id="a8ec1-232">[Visual Studio](https://visualstudio.microsoft.com) &ndash; 您可以使用啟動設定檔搭配 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)或主控台來啟動應用程式和伺服器。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-232">[Visual Studio](https://visualstudio.microsoft.com) &ndash; Launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) or the console.</span></span>
* <span data-ttu-id="a8ec1-233">[Visual Studio Code](https://code.visualstudio.com/) &ndash; 應用程式和伺服器會由 [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode) 啟動，這也可啟動 CoreCLR 偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-233">[Visual Studio Code](https://code.visualstudio.com/) &ndash; The app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span>
* <span data-ttu-id="a8ec1-234">[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/) &ndash; 應用程式和伺服器會由 [Mono Soft-Mode 偵錯工具](https://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/)啟動。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-234">[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/) &ndash; The app and server are started by the [Mono Soft-Mode Debugger](https://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="a8ec1-235">當您在專案資料夾中使用命令提示字元啟動應用程式時，[dotnet run](/dotnet/core/tools/dotnet-run) 會啟動應用程式和伺服器 (僅限 Kestrel 和 HTTP.sys)。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-235">When launching the app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="a8ec1-236">組態是由 `-c|--configuration` 選項指定，會設為 `Debug` (預設值) 或 `Release`。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-236">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="a8ec1-237">如果 launchSettings.json 檔案中出現啟動設定檔，請使用 `--launch-profile <NAME>` 選項來設定啟動設定檔 (例如，`Development` 或 `Production`)。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-237">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="a8ec1-238">如需詳細資訊，請參閱 [dotnet run](/dotnet/core/tools/dotnet-run) 和 [.NET Core 發佈封裝](/dotnet/core/build/distribution-packaging)。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-238">For more information, see [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging).</span></span>

## <a name="http2-support"></a><span data-ttu-id="a8ec1-239">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="a8ec1-239">HTTP/2 support</span></span>

<span data-ttu-id="a8ec1-240">在下列部署案例中，ASP.NET Core 支援 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="a8ec1-240">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="a8ec1-241">Kestrel</span><span class="sxs-lookup"><span data-stu-id="a8ec1-241">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="a8ec1-242">作業系統</span><span class="sxs-lookup"><span data-stu-id="a8ec1-242">Operating system</span></span>
    * <span data-ttu-id="a8ec1-243">Windows Server 2016/Windows 10 或更新版本&dagger;</span><span class="sxs-lookup"><span data-stu-id="a8ec1-243">Windows Server 2016/Windows 10 or later&dagger;</span></span>
    * <span data-ttu-id="a8ec1-244">Linux 含 OpenSSL 1.0.2 或更新版本 (例如 Ubuntu 16.04 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="a8ec1-244">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="a8ec1-245">未來版本的 macOS 將會支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-245">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="a8ec1-246">目標 Framework：.NET Core 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a8ec1-246">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="a8ec1-247">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="a8ec1-247">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="a8ec1-248">Windows Server 2016/Windows 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a8ec1-248">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="a8ec1-249">目標 Framework：不適用於 HTTP.sys 部署。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-249">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="a8ec1-250">IIS (同處理序)</span><span class="sxs-lookup"><span data-stu-id="a8ec1-250">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="a8ec1-251">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a8ec1-251">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="a8ec1-252">目標 Framework：.NET Core 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a8ec1-252">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="a8ec1-253">IIS (跨處理序)</span><span class="sxs-lookup"><span data-stu-id="a8ec1-253">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="a8ec1-254">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a8ec1-254">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="a8ec1-255">公眾對應 Edge Server 連線使用 HTTP/2，但是對 Kestrel 的反向 Proxy 連線使用 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-255">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="a8ec1-256">目標 Framework：不適用於 IIS 跨處理序部署。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-256">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

<span data-ttu-id="a8ec1-257">&dagger;Kestrel 在 Windows Server 2012 R2 與 Windows 8.1 對 HTTP/2 的支援有限。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-257">&dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="a8ec1-258">支援有限的原因是這些作業系統上的支援 TLS 密碼編譯套件清單有限。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-258">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="a8ec1-259">可能需要使用橢圓曲線數位簽章演算法 (ECDSA) 產生的憑證來保護 TLS 連線。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-259">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="a8ec1-260">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="a8ec1-260">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="a8ec1-261">Windows Server 2016/Windows 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a8ec1-261">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="a8ec1-262">目標 Framework：不適用於 HTTP.sys 部署。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-262">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="a8ec1-263">IIS (跨處理序)</span><span class="sxs-lookup"><span data-stu-id="a8ec1-263">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="a8ec1-264">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a8ec1-264">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="a8ec1-265">公眾對應 Edge Server 連線使用 HTTP/2，但是對 Kestrel 的反向 Proxy 連線使用 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-265">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="a8ec1-266">目標 Framework：不適用於 IIS 跨處理序部署。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-266">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="a8ec1-267">HTTP/2 連線必須使用 [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 和 TLS 1.2 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-267">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="a8ec1-268">如需詳細資訊，請參閱與伺服器部署案例相關的主題。</span><span class="sxs-lookup"><span data-stu-id="a8ec1-268">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a8ec1-269">其他資源</span><span class="sxs-lookup"><span data-stu-id="a8ec1-269">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:fundamentals/servers/httpsys>
