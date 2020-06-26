---
title: 在使用 IIS 的 Windows 上裝載 ASP.NET Core
author: rick-anderson
description: 了解如何在 Windows Server Internet Information Services (IIS) 上裝載 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 5/7/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: host-and-deploy/iis/index
ms.openlocfilehash: 951ae53876edf345af1a3eb32cb9be1b9668fa53
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85404167"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="ff8d0-103">在使用 IIS 的 Windows 上裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ff8d0-103">Host ASP.NET Core on Windows with IIS</span></span>

<!-- 

    NOTE FOR 5.0
    
    When making the 5.0 version of this topic, remove the Hosting Bundle
    direct download section from the (new) <5.0 & >2.2 version and modify 
    the text and heading for the *Earlier versions of the installer* 
    section. See the 2.2 version for an example.
    
-->

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ff8d0-104">如需將 ASP.NET Core 應用程式發佈至 IIS 伺服器的教學課程體驗，請參閱<xref:tutorials/publish-to-iis>。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-104">For a tutorial experience on publishing an ASP.NET Core app to an IIS server, see <xref:tutorials/publish-to-iis>.</span></span>

[<span data-ttu-id="ff8d0-105">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="ff8d0-105">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="ff8d0-106">支援的作業系統</span><span class="sxs-lookup"><span data-stu-id="ff8d0-106">Supported operating systems</span></span>

<span data-ttu-id="ff8d0-107">以下為支援的作業系統：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-107">The following operating systems are supported:</span></span>

* <span data-ttu-id="ff8d0-108">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="ff8d0-108">Windows 7 or later</span></span>
* <span data-ttu-id="ff8d0-109">Windows Server 2012 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="ff8d0-109">Windows Server 2012 R2 or later</span></span>

<span data-ttu-id="ff8d0-110">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys) (先前稱為 WebListener) 不適用搭配 IIS 的反向 Proxy 設定。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-110">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="ff8d0-111">請使用 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-111">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="ff8d0-112">如需在 Azure 中裝載的資訊，請參閱 <xref:host-and-deploy/azure-apps/index>。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-112">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

<span data-ttu-id="ff8d0-113">如需疑難排解指引，請參閱 <xref:test/troubleshoot>。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-113">For troubleshooting guidance, see <xref:test/troubleshoot>.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="ff8d0-114">支援的平台</span><span class="sxs-lookup"><span data-stu-id="ff8d0-114">Supported platforms</span></span>

<span data-ttu-id="ff8d0-115">支援針對 32 位元 (x86) 或 64 位元 (x64) 部署發行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-115">Apps published for 32-bit (x86) or 64-bit (x64) deployment are supported.</span></span> <span data-ttu-id="ff8d0-116">使用 32 位元 (x86) .NET Core SDK 部署 32 位元應用程式，除非應用程式：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-116">Deploy a 32-bit app with a 32-bit (x86) .NET Core SDK unless the app:</span></span>

* <span data-ttu-id="ff8d0-117">需要提供給 64 位元應用程式使用的較大虛擬記憶體位址空間。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-117">Requires the larger virtual memory address space available to a 64-bit app.</span></span>
* <span data-ttu-id="ff8d0-118">需要較大的 IIS 堆疊大小。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-118">Requires the larger IIS stack size.</span></span>
* <span data-ttu-id="ff8d0-119">有 64 位元原生相依性。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-119">Has 64-bit native dependencies.</span></span>

<span data-ttu-id="ff8d0-120">針對32位（x86）發行的應用程式必須為其 IIS 應用程式集區啟用32位。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-120">Apps published for 32-bit (x86) must have 32-bit enabled for their IIS Application Pools.</span></span> <span data-ttu-id="ff8d0-121">如需詳細資訊，請參閱[建立 IIS 網站](#create-the-iis-site)一節。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-121">For more information, see the [Create the IIS site](#create-the-iis-site) section.</span></span>

<span data-ttu-id="ff8d0-122">使用 64 位元 (x64) .NET Core SDK 來發行 64 位元應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-122">Use a 64-bit (x64) .NET Core SDK to publish a 64-bit app.</span></span> <span data-ttu-id="ff8d0-123">主機系統上必須有 64 位元執行階段存在。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-123">A 64-bit runtime must be present on the host system.</span></span>

## <a name="hosting-models"></a><span data-ttu-id="ff8d0-124">裝載模型</span><span class="sxs-lookup"><span data-stu-id="ff8d0-124">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="ff8d0-125">同處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="ff8d0-125">In-process hosting model</span></span>

<span data-ttu-id="ff8d0-126">使用同處理序裝載，ASP.NET Core 應用程式會在與其 IIS 工作者處理序相同的處理序中執行。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-126">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="ff8d0-127">因為要求未透過回送介面卡 (將連出網路流量傳回同一部電腦的網路介面) 進行 proxy 處理，所以同處理序裝載會提供優於跨處理序裝載的效能。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-127">In-process hosting provides improved performance over out-of-process hosting because requests aren't proxied over the loopback adapter, a network interface that returns outgoing network traffic back to the same machine.</span></span> <span data-ttu-id="ff8d0-128">IIS 透過 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 來執行處理程序管理。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-128">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="ff8d0-129">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-129">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module):</span></span>

* <span data-ttu-id="ff8d0-130">執行應用程式初始化。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-130">Performs app initialization.</span></span>
  * <span data-ttu-id="ff8d0-131">載入 [CoreCLR](/dotnet/standard/glossary#coreclr)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-131">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="ff8d0-132">呼叫 `Program.Main`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-132">Calls `Program.Main`.</span></span>
* <span data-ttu-id="ff8d0-133">處理 IIS 原生要求的存留期。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-133">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="ff8d0-134">下圖說明 IIS、ASP.NET Core 模組和同處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-134">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![同處理序代管內的 ASP.NET Core 模組案例](index/_static/ancm-inprocess.png)

1. <span data-ttu-id="ff8d0-136">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-136">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span>
1. <span data-ttu-id="ff8d0-137">驅動程式會在網站設定的連接埠上將原生要求路由至 IIS，此連接埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-137">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span>
1. <span data-ttu-id="ff8d0-138">ASP.NET Core 模組會接收原生要求，並將它傳遞至 IIS HTTP 伺服器（ `IISHttpServer` ）。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-138">The ASP.NET Core Module receives the native request and passes it to IIS HTTP Server (`IISHttpServer`).</span></span> <span data-ttu-id="ff8d0-139">IIS HTTP 伺服器是 IIS 的同處理序伺服程式實作，可將要求從原生轉換為受控。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-139">IIS HTTP Server is an in-process server implementation for IIS that converts the request from native to managed.</span></span>

<span data-ttu-id="ff8d0-140">在 IIS HTTP 伺服器處理要求之後：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-140">After the IIS HTTP Server processes the request:</span></span>

1. <span data-ttu-id="ff8d0-141">要求會傳送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-141">The request is sent to the ASP.NET Core middleware pipeline.</span></span>
1. <span data-ttu-id="ff8d0-142">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-142">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span>
1. <span data-ttu-id="ff8d0-143">應用程式的回應會透過 IIS HTTP 伺服器傳回 IIS。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-143">The app's response is passed back to IIS through IIS HTTP Server.</span></span>
1. <span data-ttu-id="ff8d0-144">IIS 會將回應傳送到起始該要求的用戶端。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-144">IIS sends the response to the client that initiated the request.</span></span>

<span data-ttu-id="ff8d0-145">同進程裝載是加入宣告現有的應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-145">In-process hosting is opt-in for existing apps.</span></span> <span data-ttu-id="ff8d0-146">ASP.NET Core 的 web 範本會使用同進程裝載模型。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-146">The ASP.NET Core web templates use the in-process hosting model.</span></span>

<span data-ttu-id="ff8d0-147">`CreateDefaultBuilder` 會透過呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> 方法來啟動 [CoreCLR](/dotnet/standard/glossary#coreclr) 以新增 <xref:Microsoft.AspNetCore.Hosting.Server.IServer>，並在 IIS 工作者處理序 (*w3wp.exe* 或 *iisexpress.exe*) 內裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-147">`CreateDefaultBuilder` adds an <xref:Microsoft.AspNetCore.Hosting.Server.IServer> instance by calling the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> method to boot the [CoreCLR](/dotnet/standard/glossary#coreclr) and host the app inside of the IIS worker process (*w3wp.exe* or *iisexpress.exe*).</span></span> <span data-ttu-id="ff8d0-148">效能測試指出，相較於將 .NET Core 應用程式裝載於處理序外，並將要求 Proxy 處理至 [Kestrel](xref:fundamentals/servers/kestrel)，裝載於處理序內可提供明顯更高的要求輸送量。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-148">Performance tests indicate that hosting a .NET Core app in-process delivers significantly higher request throughput compared to hosting the app out-of-process and proxying requests to [Kestrel](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="ff8d0-149">發佈為單一檔案可執行檔的應用程式無法由同處理序裝載模型載入。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-149">Apps published as a single file executable can't be loaded by the in-process hosting model.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="ff8d0-150">跨處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="ff8d0-150">Out-of-process hosting model</span></span>

<span data-ttu-id="ff8d0-151">因為 ASP.NET Core 應用程式會在與 IIS 背景工作進程不同的進程中執行，所以 ASP.NET Core 模組會處理進程管理。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-151">Because ASP.NET Core apps run in a process separate from the IIS worker process, the ASP.NET Core Module handles process management.</span></span> <span data-ttu-id="ff8d0-152">此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式關閉或損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-152">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="ff8d0-153">此行為基本上與執行同處理序，並由 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-153">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="ff8d0-154">下圖說明 IIS、ASP.NET Core 模組和跨處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-154">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![非同處理序代管內的 ASP.NET Core 模組案例](index/_static/ancm-outofprocess.png)

1. <span data-ttu-id="ff8d0-156">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-156">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span>
1. <span data-ttu-id="ff8d0-157">驅動程式會將要求路由至網站設定之埠上的 IIS。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-157">The driver routes the requests to IIS on the website's configured port.</span></span> <span data-ttu-id="ff8d0-158">設定的埠通常是80（HTTP）或443（HTTPS）。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-158">The configured port is usually 80 (HTTP) or 443 (HTTPS).</span></span>
1. <span data-ttu-id="ff8d0-159">模組會在應用程式的隨機埠上將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-159">The module forwards the requests to Kestrel on a random port for the app.</span></span> <span data-ttu-id="ff8d0-160">隨機埠不是80或443。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-160">The random port isn't 80 or 443.</span></span>

<!-- make this a bullet list -->
<span data-ttu-id="ff8d0-161">ASP.NET Core 模組會在啟動時透過環境變數指定埠。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-161">The ASP.NET Core Module specifies the port via an environment variable at startup.</span></span> <span data-ttu-id="ff8d0-162">延伸模組會設定 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 伺服器來接聽 `http://localhost:{PORT}` 。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-162">The <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> extension configures the server to listen on `http://localhost:{PORT}`.</span></span> <span data-ttu-id="ff8d0-163">將會執行額外檢查，不是源自模組的要求都會遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-163">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="ff8d0-164">模組不支援 HTTPS 轉送。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-164">The module doesn't support HTTPS forwarding.</span></span> <span data-ttu-id="ff8d0-165">即使 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-165">Requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="ff8d0-166">在 Kestrel 拾取來自模組的要求之後，會將要求轉送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-166">After Kestrel picks up the request from the module, the request is forwarded into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="ff8d0-167">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-167">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="ff8d0-168">IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-168">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="ff8d0-169">應用程式的回應會傳回 IIS，將它轉送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-169">The app's response is passed back to IIS, which forwards it back to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="ff8d0-170">如需 ASP.NET Core 模組組態指南，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-170">For ASP.NET Core Module configuration guidance, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="ff8d0-171">如需代管的詳細資訊，請參閱[在 ASP.NET Core 中代管](xref:fundamentals/index#host)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-171">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/index#host).</span></span>

## <a name="application-configuration"></a><span data-ttu-id="ff8d0-172">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="ff8d0-172">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="ff8d0-173">啟用 IISIntegration 元件</span><span class="sxs-lookup"><span data-stu-id="ff8d0-173">Enable the IISIntegration components</span></span>

<span data-ttu-id="ff8d0-174">在 `CreateHostBuilder` （*Program.cs*）中建立主機時，請呼叫 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> 以啟用 IIS 整合：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-174">When building a host in `CreateHostBuilder` (*Program.cs*), call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> to enable IIS integration:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        ...
```

<span data-ttu-id="ff8d0-175">如需 `CreateDefaultBuilder` 的詳細資訊，請參閱 <xref:fundamentals/host/generic-host#default-builder-settings>。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-175">For more information on `CreateDefaultBuilder`, see <xref:fundamentals/host/generic-host#default-builder-settings>.</span></span>

### <a name="iis-options"></a><span data-ttu-id="ff8d0-176">IIS 選項</span><span class="sxs-lookup"><span data-stu-id="ff8d0-176">IIS options</span></span>

<span data-ttu-id="ff8d0-177">**同處理序主控模型**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-177">**In-process hosting model**</span></span>

<span data-ttu-id="ff8d0-178">若要設定 IIS 伺服器選項，請在 <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 中加入 <xref:Microsoft.AspNetCore.Builder.IISServerOptions> 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-178">To configure IIS Server options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISServerOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="ff8d0-179">下列範例會停用 AutomaticAuthentication：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-179">The following example disables AutomaticAuthentication:</span></span>

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

| <span data-ttu-id="ff8d0-180">選項</span><span class="sxs-lookup"><span data-stu-id="ff8d0-180">Option</span></span>                         | <span data-ttu-id="ff8d0-181">預設</span><span class="sxs-lookup"><span data-stu-id="ff8d0-181">Default</span></span> | <span data-ttu-id="ff8d0-182">設定</span><span class="sxs-lookup"><span data-stu-id="ff8d0-182">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="ff8d0-183">若為 `true`，IIS 伺服器會設定由 [Windows 驗證](xref:security/authentication/windowsauth)所驗證的 `HttpContext.User`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-183">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="ff8d0-184">若為 `false`，則伺服器僅會對 `HttpContext.User` 提供身分識別，並在 `AuthenticationScheme` 明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-184">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="ff8d0-185">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-185">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="ff8d0-186">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-186">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="ff8d0-187">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-187">Sets the display name shown to users on login pages.</span></span> |
| `AllowSynchronousIO`           | `false` | <span data-ttu-id="ff8d0-188">是否允許和的同步 i/o `HttpContext.Request` `HttpContext.Response` 。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-188">Whether synchronous I/O is allowed for the `HttpContext.Request` and the `HttpContext.Response`.</span></span> |
| `MaxRequestBodySize`           | `30000000`  | <span data-ttu-id="ff8d0-189">取得或設定 `HttpRequest` 的要求本文大小上限。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-189">Gets or sets the max request body size for the `HttpRequest`.</span></span> <span data-ttu-id="ff8d0-190">請注意，IIS 本身具有限制 `maxAllowedContentLength`，此限制將在 `IISServerOptions` 中設定 `MaxRequestBodySize` 時處理。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-190">Note that IIS itself has the limit `maxAllowedContentLength` which will be processed before the `MaxRequestBodySize` set in the `IISServerOptions`.</span></span> <span data-ttu-id="ff8d0-191">變更 `MaxRequestBodySize` 將不會影響 `maxAllowedContentLength`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-191">Changing the `MaxRequestBodySize` won't affect the `maxAllowedContentLength`.</span></span> <span data-ttu-id="ff8d0-192">若要增加 `maxAllowedContentLength`，請在 *web.config* 中新增項目，以將 `maxAllowedContentLength` 設定為較高的值。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-192">To increase `maxAllowedContentLength`, add an entry in the *web.config* to set `maxAllowedContentLength` to a higher value.</span></span> <span data-ttu-id="ff8d0-193">如需更多詳細資料，請參閱[組態](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/#configuration)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-193">For more details, see [Configuration](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/#configuration).</span></span> |

<span data-ttu-id="ff8d0-194">**跨處理序裝載模型**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-194">**Out-of-process hosting model**</span></span>

<span data-ttu-id="ff8d0-195">若要設定 IIS 選項，請在 <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 中加入 <xref:Microsoft.AspNetCore.Builder.IISOptions> 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-195">To configure IIS options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="ff8d0-196">下列範例會防止應用程式填入 `HttpContext.Connection.ClientCertificate`：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-196">The following example prevents the app from populating `HttpContext.Connection.ClientCertificate`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="ff8d0-197">選項</span><span class="sxs-lookup"><span data-stu-id="ff8d0-197">Option</span></span>                         | <span data-ttu-id="ff8d0-198">預設</span><span class="sxs-lookup"><span data-stu-id="ff8d0-198">Default</span></span> | <span data-ttu-id="ff8d0-199">設定</span><span class="sxs-lookup"><span data-stu-id="ff8d0-199">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="ff8d0-200">若為 `true`，[IIS 整合中介軟體](#enable-the-iisintegration-components)會設定由 [Windows 驗證](xref:security/authentication/windowsauth)所驗證的 `HttpContext.User`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-200">If `true`, [IIS Integration Middleware](#enable-the-iisintegration-components) sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="ff8d0-201">如果為 `false`，則驗證中介軟體僅針對 `HttpContext.User` 提供身分識別，並在游 `AuthenticationScheme` 提出明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-201">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="ff8d0-202">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-202">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="ff8d0-203">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-203">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="ff8d0-204">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-204">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="ff8d0-205">如果為 `true` 且 `MS-ASPNETCORE-CLIENTCERT` 要求標頭已存在，則會填入 `HttpContext.Connection.ClientCertificate`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-205">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="ff8d0-206">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="ff8d0-206">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="ff8d0-207">[IIS 整合中介軟體](#enable-the-iisintegration-components)和 ASP.NET Core 模組已設定為轉送：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-207">The [IIS Integration Middleware](#enable-the-iisintegration-components) and the ASP.NET Core Module are configured to forward the:</span></span>

* <span data-ttu-id="ff8d0-208">配置（HTTP/HTTPS）。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-208">Scheme (HTTP/HTTPS).</span></span>
* <span data-ttu-id="ff8d0-209">發出要求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-209">Remote IP address where the request originated.</span></span>

<span data-ttu-id="ff8d0-210">[IIS 整合中介軟體](#enable-the-iisintegration-components)會設定轉送的標頭中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-210">The [IIS Integration Middleware](#enable-the-iisintegration-components) configures Forwarded Headers Middleware.</span></span>

<span data-ttu-id="ff8d0-211">其他 Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-211">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="ff8d0-212">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-212">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="ff8d0-213">web.config 檔案</span><span class="sxs-lookup"><span data-stu-id="ff8d0-213">web.config file</span></span>

<span data-ttu-id="ff8d0-214">*web.config* 檔案是用來設定 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-214">The *web.config* file configures the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="ff8d0-215">發佈專案時，由 MSBuild 目標 (`_TransformWebConfig`) 處理 *web.config* 檔案的建立、轉換及發佈。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-215">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="ff8d0-216">此目標存在於 Web SDK 目標 (`Microsoft.NET.Sdk.Web`)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-216">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="ff8d0-217">SDK 設定在專案檔的頂端：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-217">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="ff8d0-218">如果專案中沒有 *web.config* 檔案，則系統會使用正確的 *processPath* 和 *arguments* 建立該檔案以設定 ASP.NET Core 模組，並將該檔案移至[已發行的輸出](xref:host-and-deploy/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-218">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="ff8d0-219">如果 *web.config* 檔案存在於專案中，則系統會使用正確的 *processPath* 和 *arguments* 來轉換該檔案以設定 ASP.NET Core 模組，然後將它移至已發行的輸出。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-219">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="ff8d0-220">轉換不會修改檔案中的 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-220">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="ff8d0-221">*web.config* 檔案可提供能控制作用中 IIS 模組的額外 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-221">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="ff8d0-222">如需能處理 ASP.NET Core 應用程式要求之 IIS 模組的相關資訊，請參閱 [IIS 模組](xref:host-and-deploy/iis/modules)主題。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-222">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="ff8d0-223">若要防止 Web SDK 轉換*web.config*檔案，請使用專案檔 **\<IsTransformWebConfigDisabled>** 中的屬性：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-223">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="ff8d0-224">使 Web SDK 無法轉換檔案時，應該由開發人員手動設定 *processPath* 和 *arguments*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-224">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="ff8d0-225">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module> 。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-225">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="ff8d0-226">web.config 檔案位置</span><span class="sxs-lookup"><span data-stu-id="ff8d0-226">web.config file location</span></span>

<span data-ttu-id="ff8d0-227">為了正確設定[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)， *web.config*檔案必須存在於已部署應用程式的[內容根](xref:fundamentals/index#content-root)路徑（通常是應用程式基底路徑）。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-227">In order to set up the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) correctly, the *web.config* file must be present at the [content root](xref:fundamentals/index#content-root) path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="ff8d0-228">這是與提供給 IIS 的網站實體路徑相同的位置。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-228">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="ff8d0-229">應用程式的根目錄需有 *web.config* 檔案，才能使用 Web Deploy 發行多個應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-229">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="ff8d0-230">機密檔案存在於應用程式的實體路徑上，例如\* \<assembly>.runtimeconfig.json*、 \* \<assembly> .xml* （xml 檔批註）和\* \<assembly>.deps.js\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-230">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="ff8d0-231">當 *web.config* 檔案存在且網站正常啟動時，如果有人要求機密檔案，IIS 不會予以提供。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-231">When the *web.config* file is present and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="ff8d0-232">若 *web.config* 檔案遺失或沒有正確命名，或是無法設定網站以正常啟動，IIS 可能會公開提供機密檔案。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-232">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="ff8d0-233">\***web.config*檔案必須隨時存在於部署中、正確命名，而且能夠將網站設定為正常啟動。絕對不要從生產環境部署移除*web.config\*檔案。**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-233">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

### <a name="transform-webconfig"></a><span data-ttu-id="ff8d0-234">轉換 web.config</span><span class="sxs-lookup"><span data-stu-id="ff8d0-234">Transform web.config</span></span>

<span data-ttu-id="ff8d0-235">如果您需要在發行時轉換*web.config* ，請參閱 <xref:host-and-deploy/iis/transform-webconfig> 。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-235">If you need to transform *web.config* on publish, see <xref:host-and-deploy/iis/transform-webconfig>.</span></span> <span data-ttu-id="ff8d0-236">您可能需要在 [發行] 上轉換*web.config* ，以根據設定、設定檔或環境設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-236">You might need to transform *web.config* on publish to set environment variables based on the configuration, profile, or environment.</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="ff8d0-237">IIS 組態</span><span class="sxs-lookup"><span data-stu-id="ff8d0-237">IIS configuration</span></span>

<span data-ttu-id="ff8d0-238">**Windows Server 作業系統**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-238">**Windows Server operating systems**</span></span>

<span data-ttu-id="ff8d0-239">啟用**網頁伺服器 (IIS)** 伺服器角色，並建立角色服務。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-239">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="ff8d0-240">使用來自 [管理]\*\*\*\* 功能表的 [新增角色及功能]\*\*\*\* 精靈，或是 [伺服器管理員]\*\*\*\* 中的連結。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-240">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="ff8d0-241">在**伺服器角色**步驟中，核取 [網頁伺服器 (IIS)]\*\*\*\* 方塊。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-241">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![在選取伺服器角色步驟中選取網頁伺服器 IIS 角色。](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="ff8d0-243">在 [功能]\*\*\*\* 步驟之後，[角色服務]\*\*\*\* 步驟會針對網頁伺服器 (IIS) 進行載入。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-243">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="ff8d0-244">選取所需的 IIS 角色服務或接受所提供的預設角色服務。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-244">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![在選取角色服務步驟中，選取預設的角色服務。](index/_static/role-services-ws2016.png)

   <span data-ttu-id="ff8d0-246">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-246">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="ff8d0-247">若要啟用 Windows 驗證，請展開下列節點： [**網頁伺服器**  >  **安全性**]。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-247">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="ff8d0-248">選取 [Windows 驗證]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-248">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="ff8d0-249">如需詳細資訊，請參閱[Windows 驗證 \<windowsAuthentication> ](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)和[設定 windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-249">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="ff8d0-250">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-250">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="ff8d0-251">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-251">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="ff8d0-252">若要啟用 websocket，請展開下列節點： [**網頁伺服器**  >  **應用程式開發**]。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-252">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="ff8d0-253">選取 [WebSocket 通訊協定]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-253">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="ff8d0-254">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-254">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="ff8d0-255">透過**確認**步驟繼續作業，安裝網頁伺服器角色和服務。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-255">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="ff8d0-256">安裝**網頁伺服器（iis）** 角色之後，不需要重新開機伺服器/iis。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-256">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="ff8d0-257">**Windows 桌面作業系統**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-257">**Windows desktop operating systems**</span></span>

<span data-ttu-id="ff8d0-258">啟用 [IIS 管理主控台]\*\*\*\* 和 [World Wide Web 服務]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-258">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="ff8d0-259">瀏覽到 [控制台]\*\* [程式]\*\* > \*\* [程式和功能]\*\* > \*\*\*\* > **[開啟或關閉 Windows 功能]** \(畫面左側\)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-259">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="ff8d0-260">開啟 [Internet Information Services]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-260">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="ff8d0-261">開啟 [Web 管理工具]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-261">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="ff8d0-262">核取 [IIS 管理主控台]\*\*\*\* 方塊。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-262">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="ff8d0-263">[World Wide Web Services] (全球資訊網服務)\*\*\*\* 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-263">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="ff8d0-264">接受**全球資訊網服務**的預設功能，或自訂 IIS 功能。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-264">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="ff8d0-265">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-265">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="ff8d0-266">若要啟用 Windows 驗證，請展開下列節點： **World Wide Web 服務**  >  **安全性**。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-266">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="ff8d0-267">選取 [Windows 驗證]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-267">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="ff8d0-268">如需詳細資訊，請參閱[Windows 驗證 \<windowsAuthentication> ](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)和[設定 windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-268">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="ff8d0-269">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-269">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="ff8d0-270">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-270">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="ff8d0-271">若要啟用 websocket，請展開下列節點： [ **World Wide Web 服務**] [  >  **應用程式開發] 功能**。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-271">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="ff8d0-272">選取 [WebSocket 通訊協定]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-272">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="ff8d0-273">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-273">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="ff8d0-274">若 IIS 安裝需要重新啟動，請重新啟動系統。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-274">If the IIS installation requires a restart, restart the system.</span></span>

![選取 [Windows 功能] 中的 [IIS 管理主控台] 和 [World Wide Web Services] (全球資訊網服務)。](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="ff8d0-276">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="ff8d0-276">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="ff8d0-277">在主控系統上安裝 .NET Core 裝載套件組合\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-277">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="ff8d0-278">套件組合會安裝 .NET Core 執行時間、.NET Core 程式庫和[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-278">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="ff8d0-279">此模組可讓 ASP.NET Core 應用程式在 IIS 背後執行。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-279">The module allows ASP.NET Core apps to run behind IIS.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ff8d0-280">若裝載套件組合在 IIS 之前安裝，則必須對該套件組合安裝進行修復。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-280">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="ff8d0-281">請在安裝 IIS 之後，再次執行裝載套件組合安裝程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-281">Run the Hosting Bundle installer again after installing IIS.</span></span>
>
> <span data-ttu-id="ff8d0-282">如果在安裝 64 位元 (x64) 版本的 .NET Core 後才安裝裝載套件組合，那麼可能會遺漏 SDK ([未偵測到 .NET Core SDK](xref:test/troubleshoot#no-net-core-sdks-were-detected))。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-282">If the Hosting Bundle is installed after installing the 64-bit (x64) version of .NET Core, SDKs might appear to be missing ([No .NET Core SDKs were detected](xref:test/troubleshoot#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="ff8d0-283">若要解決此問題，請參閱 <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-283">To resolve the problem, see <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>.</span></span>

### <a name="direct-download-current-version"></a><span data-ttu-id="ff8d0-284">直接下載 (目前版本)</span><span class="sxs-lookup"><span data-stu-id="ff8d0-284">Direct download (current version)</span></span>

<span data-ttu-id="ff8d0-285">使用下列連結下載安裝程式：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-285">Download the installer using the following link:</span></span>

[<span data-ttu-id="ff8d0-286">目前的 .NET Core 裝載套件組合安裝程式 (直接下載)</span><span class="sxs-lookup"><span data-stu-id="ff8d0-286">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a><span data-ttu-id="ff8d0-287">安裝程式的先前版本</span><span class="sxs-lookup"><span data-stu-id="ff8d0-287">Earlier versions of the installer</span></span>

<span data-ttu-id="ff8d0-288">若要取得安裝程式的先前版本：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-288">To obtain an earlier version of the installer:</span></span>

1. <span data-ttu-id="ff8d0-289">流覽至 [[下載 .Net Core](https://dotnet.microsoft.com/download/dotnet-core) ] 頁面。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-289">Navigate to the [Download .NET Core](https://dotnet.microsoft.com/download/dotnet-core) page.</span></span>
1. <span data-ttu-id="ff8d0-290">選取所需的 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-290">Select the desired .NET Core version.</span></span>
1. <span data-ttu-id="ff8d0-291">在 [執行應用程式 - 執行階段]\*\*\*\* 欄中，尋找想要的 .NET Core 執行階段版本列。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-291">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="ff8d0-292">使用**裝載**套件組合連結來下載安裝程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-292">Download the installer using the **Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="ff8d0-293">某些安裝程式包含已達到期生命週期結束 (EOL) 的發行版本，這些發行版本已不受 Microsoft 支援。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-293">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="ff8d0-294">如需詳細資訊，請參閱[支援原則](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) \(英文 \)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-294">For more information, see the [support policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="ff8d0-295">安裝裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="ff8d0-295">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="ff8d0-296">在伺服器上執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-296">Run the installer on the server.</span></span> <span data-ttu-id="ff8d0-297">從系統管理員命令殼層執行安裝程式時，有 下列參數可用：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-297">The following parameters are available when running the installer from an administrator command shell:</span></span>

   * <span data-ttu-id="ff8d0-298">`OPT_NO_ANCM=1`：略過安裝 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-298">`OPT_NO_ANCM=1`: Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="ff8d0-299">`OPT_NO_RUNTIME=1`：略過安裝 .NET Core 執行時間。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-299">`OPT_NO_RUNTIME=1`: Skip installing the .NET Core runtime.</span></span> <span data-ttu-id="ff8d0-300">當伺服器只裝載[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)時使用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-300">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="ff8d0-301">`OPT_NO_SHAREDFX=1`：略過安裝 ASP.NET 共用架構（ASP.NET 執行時間）。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-301">`OPT_NO_SHAREDFX=1`: Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span> <span data-ttu-id="ff8d0-302">當伺服器只裝載[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)時使用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-302">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="ff8d0-303">`OPT_NO_X86=1`：略過安裝 x86 執行時間。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-303">`OPT_NO_X86=1`: Skip installing x86 runtimes.</span></span> <span data-ttu-id="ff8d0-304">當您確定不會裝載 32 位元應用程式時，請使用此參數。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-304">Use this parameter when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="ff8d0-305">如果將來有可能同時裝載 32 位元和 64 位元應用程式，請不要使用此參數並安裝這兩個執行階段。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-305">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this parameter and install both runtimes.</span></span>
   * <span data-ttu-id="ff8d0-306">`OPT_NO_SHARED_CONFIG_CHECK=1`：當共用設定（*applicationHost.config*）位於與 IIS 安裝相同的電腦上時，請停用使用 iis 共用設定的檢查。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-306">`OPT_NO_SHARED_CONFIG_CHECK=1`: Disable the check for using an IIS Shared Configuration when the shared configuration (*applicationHost.config*) is on the same machine as the IIS installation.</span></span> <span data-ttu-id="ff8d0-307">*只在 ASP.NET Core 2.2 或更新版本的裝載套件組合安裝程式上可用。*</span><span class="sxs-lookup"><span data-stu-id="ff8d0-307">*Only available for ASP.NET Core 2.2 or later Hosting Bundler installers.*</span></span> <span data-ttu-id="ff8d0-308">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration> 。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-308">For more information, see <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>.</span></span>
1. <span data-ttu-id="ff8d0-309">重新開機系統，或在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-309">Restart the system or execute the following commands in a command shell:</span></span>

   ```console
   net stop was /y
   net start w3svc
   ```
   <span data-ttu-id="ff8d0-310">重新啟動 IIS 將能偵測到由安裝程式對系統路徑 (此為環境變數) 所做出的變更。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-310">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

<span data-ttu-id="ff8d0-311">ASP.NET Core 不採用共用架構封裝修補程式版本的向前復原行為。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-311">ASP.NET Core doesn't adopt roll-forward behavior for patch releases of shared framework packages.</span></span> <span data-ttu-id="ff8d0-312">藉由安裝新的裝載套件組合來升級共用架構之後，請重新開機系統，或在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-312">After upgrading the shared framework by installing a new hosting bundle, restart the system or execute the following commands in a command shell:</span></span>

```console
net stop was /y
net start w3svc
```

> [!NOTE]
> <span data-ttu-id="ff8d0-313">如需 IIS 共用組態的資訊，請參閱[使用 IIS 共用組態的 ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-313">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="ff8d0-314">使用 Visual Studio 發佈時安裝 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="ff8d0-314">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="ff8d0-315">將應用程式部署到具有 [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later) 的伺服器時，請在伺服器上安裝最新版的 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-315">When deploying apps to servers with [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="ff8d0-316">若要安裝 Web Deploy，請使用 [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) 或從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=43717)直接取得安裝程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-316">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="ff8d0-317">慣用的方法是使用 WebPI。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-317">The preferred method is to use WebPI.</span></span> <span data-ttu-id="ff8d0-318">WebPI 提供獨立的安裝程式和組態以裝載提供者。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-318">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="ff8d0-319">建立 IIS 網站</span><span class="sxs-lookup"><span data-stu-id="ff8d0-319">Create the IIS site</span></span>

1. <span data-ttu-id="ff8d0-320">在主控系統上，請建立資料夾以容納應用程式的已發行資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-320">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="ff8d0-321">在下列步驟中，您提供資料夾路徑給 IIS，作為應用程式的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-321">In a following step, the folder's path is provided to IIS as the physical path to the app.</span></span> <span data-ttu-id="ff8d0-322">如需應用程式之部署資料夾和檔案配置的詳細資訊，請參閱 <xref:host-and-deploy/directory-structure>。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-322">For more information on an app's deployment folder and file layout, see <xref:host-and-deploy/directory-structure>.</span></span>

1. <span data-ttu-id="ff8d0-323">在 [IIS 管理員] 中 **，在 [** 連線] 面板中開啟伺服器的節點。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-323">In IIS Manager, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="ff8d0-324">以滑鼠右鍵按一下 [網站]\*\*\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-324">Right-click the **Sites** folder.</span></span> <span data-ttu-id="ff8d0-325">從操作功能表選取 [新增網站]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-325">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="ff8d0-326">提供**網站名稱**，並將**實體路徑**設定為應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-326">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="ff8d0-327">藉由**Binding**選取 **[確定]** 來提供系結設定並建立網站：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-327">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![在新增網站步驟中提供站台名稱、實體路徑和主機名稱。](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="ff8d0-329">請**勿**使用最上層萬用字元繫結 (`http://*:80/`與 `http://+:80`)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-329">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="ff8d0-330">最上層萬用字元繫結可能暴露您的應用程式安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-330">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="ff8d0-331">這對強式與弱式萬用字元皆適用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-331">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="ff8d0-332">請使用明確主機名稱，而非萬用字元。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-332">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="ff8d0-333">若您擁有整個父網域 (與具弱點的 `*.com` 相對) 的控制權，則子網域萬用字元繫結 (例如 `*.mysub.com`) 就沒有此安全性風險。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-333">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="ff8d0-334">如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-334">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="ff8d0-335">在伺服器的節點之下，選取 [應用程式集區]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-335">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="ff8d0-336">以滑鼠右鍵按一下網站的應用程式集區，然後從操作功能表選取 [基本設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-336">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="ff8d0-337">在 [編輯應用程式集區]\*\*\*\* 視窗中，將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\*：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-337">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![將 .NET CLR 版本設為 [沒有受控碼]。](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="ff8d0-339">ASP.NET Core 會在不同的處理序中執行，並管理執行階段。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-339">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="ff8d0-340">ASP.NET Core 不依賴載入桌面 CLR （.NET CLR）。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-340">ASP.NET Core doesn't rely on loading the desktop CLR (.NET CLR).</span></span> <span data-ttu-id="ff8d0-341">.NET Core 的核心通用語言執行時間（CoreCLR）會啟動以在背景工作進程中裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-341">The Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process.</span></span> <span data-ttu-id="ff8d0-342">將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\* 是選擇性的，但建議這樣做。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-342">Setting the **.NET CLR version** to **No Managed Code** is optional but recommended.</span></span>

1. <span data-ttu-id="ff8d0-343">*ASP.NET Core 2.2 或更新版本*：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-343">*ASP.NET Core 2.2 or later*:</span></span>

   * <span data-ttu-id="ff8d0-344">針對以使用同[進程裝載模型](#in-process-hosting-model)的32位 SDK 發行的32位（x86）獨立式[部署](/dotnet/core/deploying/#self-contained-deployments-scd)，請啟用32位的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-344">For a 32-bit (x86) [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) published with a 32-bit SDK that uses the [in-process hosting model](#in-process-hosting-model), enable the Application Pool for 32-bit.</span></span> <span data-ttu-id="ff8d0-345">在 IIS 管理員中，流覽至 [**連接**] 提要欄位中的 [**應用程式**集區]。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-345">In IIS Manager, navigate to **Application Pools** in the **Connections** sidebar.</span></span> <span data-ttu-id="ff8d0-346">選取應用程式的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-346">Select the app's Application Pool.</span></span> <span data-ttu-id="ff8d0-347">在 [**動作**] 提要欄位中，選取 [ **Advanced Settings**]。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-347">In the **Actions** sidebar, select **Advanced Settings**.</span></span> <span data-ttu-id="ff8d0-348">將 [**啟用32位應用程式**] 設定為 `True` 。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-348">Set **Enable 32-Bit Applications** to `True`.</span></span> 

   * <span data-ttu-id="ff8d0-349">對於使用[同處理序主控模型](#in-process-hosting-model)的 64 位元 (x64) [自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)，會停用 32 位元 (x86) 處理序的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-349">For a 64-bit (x64) [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) that uses the [in-process hosting model](#in-process-hosting-model), disable the app pool for 32-bit (x86) processes.</span></span> <span data-ttu-id="ff8d0-350">在 IIS 管理員中，流覽至 [**連接**] 提要欄位中的 [**應用程式**集區]。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-350">In IIS Manager, navigate to **Application Pools** in the **Connections** sidebar.</span></span> <span data-ttu-id="ff8d0-351">選取應用程式的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-351">Select the app's Application Pool.</span></span> <span data-ttu-id="ff8d0-352">在 [**動作**] 提要欄位中，選取 [ **Advanced Settings**]。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-352">In the **Actions** sidebar, select **Advanced Settings**.</span></span> <span data-ttu-id="ff8d0-353">將 [**啟用32位應用程式**] 設定為 `False` 。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-353">Set **Enable 32-Bit Applications** to `False`.</span></span> 

1. <span data-ttu-id="ff8d0-354">確認處理序模型身分識別具有適當的權限。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-354">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="ff8d0-355">如果應用程式集區的預設識別（**進程模型**  >  **Identity** ）從**ApplicationPoolIdentity**變更為另一個身分識別，請確認新的身分識別具有存取應用程式資料夾、資料庫和其他必要資源的必要許可權。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-355">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="ff8d0-356">例如，應用程式集區需要針對應用程式讀取和寫入檔案的資料夾取得讀取和寫入權限。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-356">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="ff8d0-357">**Windows 驗證設定 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-357">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="ff8d0-358">如需詳細資訊，請參閱[設定 Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-358">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="ff8d0-359">部署應用程式</span><span class="sxs-lookup"><span data-stu-id="ff8d0-359">Deploy the app</span></span>

<span data-ttu-id="ff8d0-360">將應用程式部署至在[建立 IIS 網站](#create-the-iis-site)一節中所建立的 IIS **實體路徑**資料夾。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-360">Deploy the app to the IIS **Physical path** folder that was established in the [Create the IIS site](#create-the-iis-site) section.</span></span> <span data-ttu-id="ff8d0-361">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 是建議的部署機制，但有數個選項可將應用程式從專案的 [publish]\*\* 資料夾移動至主機系統的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-361">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment, but several options exist for moving the app from the project's *publish* folder to the hosting system's deployment folder.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="ff8d0-362">使用 Visual Studio 的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="ff8d0-362">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="ff8d0-363">請參閱[適用於 ASP.NET Core 應用程式部署的 Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)主題，了解如何建立搭配 Web Deploy 使用的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-363">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="ff8d0-364">如果主機服務提供者提供或支援建立發行設定檔，請下載其設定檔，並使用 Visual Studio 的 [發行]\*\*\*\* 對話方塊匯入其設定檔：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-364">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog:</span></span>

![[發佈] 對話方塊頁](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="ff8d0-366">Visual Studio 外部的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="ff8d0-366">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="ff8d0-367">透過命令列也可以在 Visual Studio 外部使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-367">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="ff8d0-368">如需詳細資訊，請參閱 [Web 部署工具](/iis/publish/using-web-deploy/use-the-web-deployment-tool)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-368">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="ff8d0-369">Web Deploy 的替代項目</span><span class="sxs-lookup"><span data-stu-id="ff8d0-369">Alternatives to Web Deploy</span></span>

<span data-ttu-id="ff8d0-370">使用數種方法的其中一種將應用程式移到主機系統，例如手動複製、[Xcopy](/windows-server/administration/windows-commands/xcopy)、[Robocopy](/windows-server/administration/windows-commands/robocopy) 或 [PowerShell](/powershell/)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-370">Use any of several methods to move the app to the hosting system, such as manual copy, [Xcopy](/windows-server/administration/windows-commands/xcopy), [Robocopy](/windows-server/administration/windows-commands/robocopy), or [PowerShell](/powershell/).</span></span>

<span data-ttu-id="ff8d0-371">如需 IIS 的 ASP.NET Core 部署詳細資訊，請參閱 [IIS 系統管理員的部署資源](#deployment-resources-for-iis-administrators)一節。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-371">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="ff8d0-372">瀏覽網站</span><span class="sxs-lookup"><span data-stu-id="ff8d0-372">Browse the website</span></span>

<span data-ttu-id="ff8d0-373">應用程式部署到主機系統之後，請向其中一個應用程式的公用端點發出要求。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-373">After the app is deployed to the hosting system, make a request to one of the app's public endpoints.</span></span>

<span data-ttu-id="ff8d0-374">在下列範例中，網站繫結至 IIS 上的**連接埠** `80`，其**主機名稱**為 `www.mysite.com`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-374">In the following example, the site is bound to an IIS **Host name** of `www.mysite.com` on **Port** `80`.</span></span> <span data-ttu-id="ff8d0-375">對 `http://www.mysite.com` 發出要求：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-375">A request is made to `http://www.mysite.com`:</span></span>

![Microsoft Edge 瀏覽器已載入 IIS 啟動頁面。](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="ff8d0-377">已鎖定的部署檔案</span><span class="sxs-lookup"><span data-stu-id="ff8d0-377">Locked deployment files</span></span>

<span data-ttu-id="ff8d0-378">當應用程式執行時，會鎖定部署資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-378">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="ff8d0-379">無法於部署期間覆寫已鎖定的檔案。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-379">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="ff8d0-380">若要釋放部署中的已鎖定檔案，請使用下列其中**一種**方法停止應用程式集區：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-380">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="ff8d0-381">使用 Web Deploy 並參考專案檔中的 `Microsoft.NET.Sdk.Web`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-381">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="ff8d0-382">*app_offline.htm* 檔案是放在 Web 應用程式目錄的根目錄中。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-382">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="ff8d0-383">當檔案存在時，ASP.NET Core 模組會正常關閉應用程式，並在部署期間提供 *app_offline.htm* 檔案。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-383">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="ff8d0-384">如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-384">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="ff8d0-385">在伺服器上的 IIS 管理員中手動停止應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-385">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="ff8d0-386">使用 PowerShell 卸載*app_offline.htm* （需要 PowerShell 5 或更新版本）：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-386">Use PowerShell to drop *app_offline.htm* (requires PowerShell 5 or later):</span></span>

  ```powershell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a><span data-ttu-id="ff8d0-387">資料保護</span><span class="sxs-lookup"><span data-stu-id="ff8d0-387">Data protection</span></span>

<span data-ttu-id="ff8d0-388">[ASP.NET Core 資料保護堆疊](xref:security/data-protection/introduction)是由數個 ASP.NET Core [中介軟體](xref:fundamentals/middleware/index)所使用，包括用於驗證的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-388">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="ff8d0-389">即使資料保護 API 不是由使用者程式碼呼叫，仍應使用部署指令碼或是在使用者程式碼中設定資料保護，以建立持續性的密碼編譯[金鑰存放區](xref:security/data-protection/implementation/key-management)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-389">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="ff8d0-390">如不設定資料保護，金鑰會保留在記憶體中，並於應用程式重新啟動時捨棄。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-390">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="ff8d0-391">如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-391">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="ff8d0-392">所有以 Cookie 為基礎的驗證權杖都會失效。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-392">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="ff8d0-393">當使用者提出下一個要求時，需要再次登入。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-393">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="ff8d0-394">所有以 Keyring 保護的資料都無法再解密。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-394">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="ff8d0-395">這可能會包含 [CSRF 權杖](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)和 [ASP.NET Core MVC TempData cookie](xref:fundamentals/app-state#tempdata)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-395">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="ff8d0-396">若要在 IIS 下設定資料保護以保存 Keyring，請使用下列其中**一種**方法：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-396">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="ff8d0-397">**建立資料保護登錄機碼**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-397">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="ff8d0-398">ASP.NET Core 應用程式所使用的資料保護金鑰會儲存在應用程式外部的登錄中。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-398">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="ff8d0-399">若要保存指定應用程式的金鑰，請為應用程式集區建立登錄機碼。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-399">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="ff8d0-400">若為獨立的非Web 伺服陣列 IIS 安裝，請針對搭配使用 ASP.NET Core 應用程式的每個應用程式集區，使用[資料保護 Provision-AutoGenKeys.ps1 PowerShell 指令碼](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-400">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="ff8d0-401">此指令碼會在 HKLM 登錄中建立登錄機碼，只有應用程式之應用程式集區的背景工作處理序帳戶可以存取它。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-401">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="ff8d0-402">在待用期間使用 DPAPI 和全電腦金鑰加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-402">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="ff8d0-403">在 Web 伺服陣列案例中，應用程式可以設定成使用 UNC 路徑來儲存其資料保護 Keyring。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-403">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="ff8d0-404">根據預設，資料保護金鑰不予加密。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-404">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="ff8d0-405">請確保網路共用的檔案權限僅限於執行應用程式的 Windows 帳戶。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-405">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="ff8d0-406">可以使用 X509 憑證來保護待用的金鑰。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-406">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="ff8d0-407">請考慮下列讓使用者上傳憑證的機制：將憑證放入使用者的受信任憑證存放區，並確保在執行使用者應用程式的所有電腦上都能使用這些憑證。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-407">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="ff8d0-408">如需詳細資訊，請參閱[設定 ASP.NET Core 資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-408">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="ff8d0-409">**設定 IIS 應用程式集區載入使用者設定檔**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-409">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="ff8d0-410">此設定位在應用程式集區 [進階設定]\*\*\*\* 下的 [處理序模型]\*\*\*\* 區段中。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-410">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="ff8d0-411">將 [載入使用者設定檔]\*\*\*\* 設為 `True`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-411">Set **Load User Profile** to `True`.</span></span> <span data-ttu-id="ff8d0-412">當設定為 `True` 時，金鑰會儲存在使用者設定檔目錄中，且使用具有使用者帳戶專屬金鑰的 DPAPI 保護。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-412">When set to `True`, keys are stored in the user profile directory and protected using DPAPI with a key specific to the user account.</span></span> <span data-ttu-id="ff8d0-413">金鑰會保存到 *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-413">Keys are persisted to the *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* folder.</span></span>

  <span data-ttu-id="ff8d0-414">應用程式集區的 [setProfileEnvironment 屬性](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration)也必須啟用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-414">The app pool's [setProfileEnvironment attribute](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) must also be enabled.</span></span> <span data-ttu-id="ff8d0-415">`setProfileEnvironment` 的預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-415">The default value of `setProfileEnvironment` is `true`.</span></span> <span data-ttu-id="ff8d0-416">在某些情況下 (例如 Windows OS)，`setProfileEnvironment` 會設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-416">In some scenarios (for example, Windows OS), `setProfileEnvironment` is set to `false`.</span></span> <span data-ttu-id="ff8d0-417">如果金鑰並未如預期地儲存在使用者設定檔目錄中：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-417">If keys aren't stored in the user profile directory as expected:</span></span>

  1. <span data-ttu-id="ff8d0-418">瀏覽至 *%windir%/system32/inetsrv/config* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-418">Navigate to the *%windir%/system32/inetsrv/config* folder.</span></span>
  1. <span data-ttu-id="ff8d0-419">開啟 *applicationHost.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-419">Open the *applicationHost.config* file.</span></span>
  1. <span data-ttu-id="ff8d0-420">找出 `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` 元素。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-420">Locate the `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` element.</span></span>
  1. <span data-ttu-id="ff8d0-421">確認 `setProfileEnvironment` 屬性不存在 (其預設值為 `true`)，或明確地將屬性值設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-421">Confirm that the `setProfileEnvironment` attribute isn't present, which defaults the value to `true`, or explicitly set the attribute's value to `true`.</span></span>

* <span data-ttu-id="ff8d0-422">**將檔案系統當作 Keyring 存放區使用**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-422">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="ff8d0-423">調整應用程式程式碼，以[將檔案系統當作 Keyring 存放區使用](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-423">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="ff8d0-424">使用 X509 憑證來保護 Keyring，並確保憑證是受信任的憑證。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-424">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="ff8d0-425">如果憑證為自我簽署，請將憑證放在受信任的根存放區。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-425">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="ff8d0-426">在 Web 伺服陣列中使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-426">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="ff8d0-427">請使用所有電腦都可以存取的檔案共用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-427">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="ff8d0-428">將 X509 憑證部署到每一部電腦。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-428">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="ff8d0-429">設定[程式碼中的資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-429">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="ff8d0-430">**設定資料保護的全電腦原則**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-430">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="ff8d0-431">針對取用資料保護 API 的所有應用程式，資料保護系統僅支援有限的預設[全電腦原則](xref:security/data-protection/configuration/machine-wide-policy)設定。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-431">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="ff8d0-432">如需詳細資訊，請參閱 <xref:security/data-protection/introduction> 。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-432">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="virtual-directories"></a><span data-ttu-id="ff8d0-433">虛擬目錄</span><span class="sxs-lookup"><span data-stu-id="ff8d0-433">Virtual Directories</span></span>

<span data-ttu-id="ff8d0-434">ASP.NET Core 應用程式不支援 [IIS 虛擬目錄](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-434">[IIS Virtual Directories](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) aren't supported with ASP.NET Core apps.</span></span> <span data-ttu-id="ff8d0-435">應用程式能以[子應用程式](#sub-applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-435">An app can be hosted as a [sub-application](#sub-applications).</span></span>

## <a name="sub-applications"></a><span data-ttu-id="ff8d0-436">子應用程式</span><span class="sxs-lookup"><span data-stu-id="ff8d0-436">Sub-applications</span></span>

<span data-ttu-id="ff8d0-437">ASP.NET Core 應用程式能以 [IIS 子應用程式](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-437">An ASP.NET Core app can be hosted as an [IIS sub-application (sub-app)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications).</span></span> <span data-ttu-id="ff8d0-438">子應用程式的路徑會成為根應用程式 URL 的一部分。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-438">The sub-app's path becomes part of the root app's URL.</span></span>

<span data-ttu-id="ff8d0-439">子應用程式內的靜態資產連結應該使用波狀符號與斜線 (`~/`) 標記法。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-439">Static asset links within the sub-app should use tilde-slash (`~/`) notation.</span></span> <span data-ttu-id="ff8d0-440">波狀符號與斜線標記法會觸發[標記協助程式](xref:mvc/views/tag-helpers/intro)以將子應用程式的路徑基底附加到轉譯的相對連結前面。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-440">Tilde-slash notation triggers a [Tag Helper](xref:mvc/views/tag-helpers/intro) to prepend the sub-app's pathbase to the rendered relative link.</span></span> <span data-ttu-id="ff8d0-441">針對位於 `/subapp_path` 的子應用程式，使用 `src="~/image.png"` 連結的影像會轉譯為 `src="/subapp_path/image.png"`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-441">For a sub-app at `/subapp_path`, an image linked with `src="~/image.png"` is rendered as `src="/subapp_path/image.png"`.</span></span> <span data-ttu-id="ff8d0-442">根應用程式的靜態檔案中介軟體不會處理靜態檔案要求。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-442">The root app's Static File Middleware doesn't process the static file request.</span></span> <span data-ttu-id="ff8d0-443">要求會由子應用程式的靜態檔案中介軟體處理。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-443">The request is processed by the sub-app's Static File Middleware.</span></span>

<span data-ttu-id="ff8d0-444">若靜態資產的 `src` 屬性是設定為絕對路徑 (例如，`src="/image.png"`)，會以不使用子應用程式路徑基底的方式轉譯連結。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-444">If a static asset's `src` attribute is set to an absolute path (for example, `src="/image.png"`), the link is rendered without the sub-app's pathbase.</span></span> <span data-ttu-id="ff8d0-445">根應用程式的靜態檔案中介軟體會嘗試從根應用程式的 [webroot](xref:fundamentals/index#web-root) 提供資產，這會導致「404 - 找不到」\*\* 回應 (除非根應用程式可存取靜態資產)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-445">The root app's Static File Middleware attempts to serve the asset from the root app's [web root](xref:fundamentals/index#web-root), which results in a *404 - Not Found* response unless the static asset is available from the root app.</span></span>

<span data-ttu-id="ff8d0-446">裝載 ASP.NET Core 應用程式做為另一個 ASP.NET Core 應用程式下的子應用程式：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-446">To host an ASP.NET Core app as a sub-app under another ASP.NET Core app:</span></span>

1. <span data-ttu-id="ff8d0-447">為子應用程式建立應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-447">Establish an app pool for the sub-app.</span></span> <span data-ttu-id="ff8d0-448">將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\*，因為會將核心通用語言執行平台 (CoreCLR) 開機以在背景工作處理序中裝載應用程式，而非在桌面 CLR (.NET CLR) 中裝載。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-448">Set the **.NET CLR Version** to **No Managed Code** because the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process, not the desktop CLR (.NET CLR).</span></span>

1. <span data-ttu-id="ff8d0-449">使用根網站下資料夾中的子應用程式在 IIS 管理員中新增根網站。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-449">Add the root site in IIS Manager with the sub-app in a folder under the root site.</span></span>

1. <span data-ttu-id="ff8d0-450">以滑鼠右鍵按一下 IIS 管理員中的子應用程式資料夾，然後選取 [轉換成應用程式]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-450">Right-click the sub-app folder in IIS Manager and select **Convert to Application**.</span></span>

1. <span data-ttu-id="ff8d0-451">在 [新增應用程式]\*\*\*\* 對話方塊中，使用 [應用程式集區]\*\*\*\* 的[選取]\*\*\*\* 按鈕來指派您為子應用程式建立的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-451">In the **Add Application** dialog, use the **Select** button for the **Application Pool** to assign the app pool that you created for the sub-app.</span></span> <span data-ttu-id="ff8d0-452">選取 [確定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-452">Select **OK**.</span></span>

<span data-ttu-id="ff8d0-453">將不同的應用程式集區指派給子應用程式是使用同處理序裝載模型。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-453">The assignment of a separate app pool to the sub-app is a requirement when using the in-process hosting model.</span></span>

<span data-ttu-id="ff8d0-454">如需有關同進程裝載模型和設定 ASP.NET Core 模組的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module> 。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-454">For more information on the in-process hosting model and configuring the ASP.NET Core Module, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="ff8d0-455">使用 web.config 的 IIS 組態</span><span class="sxs-lookup"><span data-stu-id="ff8d0-455">Configuration of IIS with web.config</span></span>

<span data-ttu-id="ff8d0-456">在對使用了 ASP.NET Core 模組的 ASP.NET Core 有作用的 IIS 情境下，設定會受 *web.config* 的 `<system.webServer>` 區段影響。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-456">IIS configuration is influenced by the `<system.webServer>` section of *web.config* for IIS scenarios that are functional for ASP.NET Core apps with the ASP.NET Core Module.</span></span> <span data-ttu-id="ff8d0-457">舉例來說，IIS 設定對動態壓縮有作用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-457">For example, IIS configuration is functional for dynamic compression.</span></span> <span data-ttu-id="ff8d0-458">如果在伺服器層級將 IIS 設為使用動態壓縮，應用程式 *web.config* 檔案中的 `<urlCompression>` 元素則可為 ASP.NET Core 應用程式予以停用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-458">If IIS is configured at the server level to use dynamic compression, the `<urlCompression>` element in the app's *web.config* file can disable it for an ASP.NET Core app.</span></span>

<span data-ttu-id="ff8d0-459">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-459">For more information, see the following topics:</span></span>

* [<span data-ttu-id="ff8d0-460">的設定參考\<system.webServer></span><span class="sxs-lookup"><span data-stu-id="ff8d0-460">Configuration reference for \<system.webServer></span></span>](/iis/configuration/system.webServer/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>

<span data-ttu-id="ff8d0-461">若要設定在隔離的應用程式集區中執行之個別應用程式的環境變數（支援 IIS 10.0 或更新版本），請參閱 IIS 參考檔中[環境變數 \<environmentVariables> ](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe)主題的*AppCmd.exe 命令*一節。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-461">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="ff8d0-462">web.config 的組態區段</span><span class="sxs-lookup"><span data-stu-id="ff8d0-462">Configuration sections of web.config</span></span>

<span data-ttu-id="ff8d0-463">ASP.NET Core 應用程式的設定不使用 *web.config* 中 ASP.NET 4.x 應用程式的設定區段：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-463">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

<span data-ttu-id="ff8d0-464">使用其他組態提供者設定的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-464">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="ff8d0-465">如需詳細資訊，請參閱[Configuration](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-465">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="ff8d0-466">應用程式集區</span><span class="sxs-lookup"><span data-stu-id="ff8d0-466">Application Pools</span></span>

<span data-ttu-id="ff8d0-467">應用程式集區隔離取決於裝載模型：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-467">App pool isolation is determined by the hosting model:</span></span>

* <span data-ttu-id="ff8d0-468">同進程裝載：應用程式必須在不同的應用程式集區中執行。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-468">In-process hosting: Apps are required to run in separate app pools.</span></span>
* <span data-ttu-id="ff8d0-469">跨進程裝載：我們建議您在自己的應用程式集區中執行每個應用程式，以將應用程式彼此隔離。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-469">Out-of-process hosting: We recommend isolating the apps from each other by running each app in its own app pool.</span></span>

<span data-ttu-id="ff8d0-470">IIS [新增網站]\*\*\*\* 對話方塊預設每個應用程式皆為單一應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-470">The IIS **Add Website** dialog defaults to a single app pool per app.</span></span> <span data-ttu-id="ff8d0-471">當提供**網站名稱**時，文字會自動轉移至 [應用程式集區]\*\*\*\* 文字方塊。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-471">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="ff8d0-472">新增網站時，會使用該網站名稱建立新的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-472">A new app pool is created using the site name when the site is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="ff8d0-473">應用程式集區Identity</span><span class="sxs-lookup"><span data-stu-id="ff8d0-473">Application Pool Identity</span></span>

<span data-ttu-id="ff8d0-474">應用程式集區身分識別帳戶可讓應用程式在唯一的帳戶下執行，不必建立及管理網域或本機帳戶。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-474">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="ff8d0-475">在 IIS 8.0 或更新版本中，IIS 管理背景工作處理序 (WAS) 會使用新的應用程式集區名稱建立虛擬帳戶，並預設在此帳戶下執行應用程式集區的背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-475">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="ff8d0-476">在 IIS 管理主控台中，于應用程式集區的 [**高級設定**] 底下，確定 **Identity** 已設定為使用**ApplicationPoolIdentity**：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-476">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![應用程式集區進階設定對話方塊](index/_static/apppool-identity.png)

<span data-ttu-id="ff8d0-478">IIS 管理程序會在 Windows 安全系統中，以應用程式集區的名稱建立安全識別碼。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-478">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="ff8d0-479">可使用此身分識別來保護資源。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-479">Resources can be secured using this identity.</span></span> <span data-ttu-id="ff8d0-480">不過，此身分識別不是真正的使用者帳戶，也不會顯示在 Windows 使用者管理主控台中。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-480">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="ff8d0-481">如果 IIS 背景工作處理序需要提升應用程式的存取權限，請修改包含應用程式的目錄存取控制清單 (ACL)：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-481">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="ff8d0-482">開啟 Windows 檔案總管，巡覽至目錄。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-482">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="ff8d0-483">以滑鼠右鍵按一下目錄並選取 [屬性]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-483">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="ff8d0-484">依序選取 [安全性]\*\*\*\* 索引標籤下的 [編輯]\*\*\*\* 按鈕和 [新增]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-484">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="ff8d0-485">選取 [位置]\*\*\*\* 按鈕，並確定選取系統。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-485">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="ff8d0-486">在 [輸入要選取的物件名稱]\*\*\*\* 區域中，輸入 **IIS AppPool\\<app_pool_name>**。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-486">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="ff8d0-487">選取 [檢查名稱]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-487">Select the **Check Names** button.</span></span> <span data-ttu-id="ff8d0-488">針對 [DefaultAppPool]\*\*，請使用 **IIS AppPool\DefaultAppPool** 檢查名稱。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-488">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="ff8d0-489">選取 [檢查名稱]\*\*\*\* 按鈕時，[DefaultAppPool]\*\*\*\* 的值便會顯示於物件名稱區域中。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-489">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="ff8d0-490">您無法直接將應用程式集區名稱輸入至物件名稱區域。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-490">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="ff8d0-491">檢查物件名稱時，請使用 **IIS AppPool\\<app_pool_name>** 的格式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-491">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：在選取 [檢查名稱] 之前，"DefaultAppPool" 這個應用程式集區名稱在物件名稱區域中會附加至 "IIS AppPool\"。](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="ff8d0-493">選取 [確定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-493">Select **OK**.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：選取 [檢查名稱] 之後，物件名稱 "DefaultAppPool" 會顯示在物件名稱區域中。](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="ff8d0-495">預設應該會授與讀取 &amp; 執行權限。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-495">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="ff8d0-496">請視需要提供其他權限。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-496">Provide additional permissions as needed.</span></span>

<span data-ttu-id="ff8d0-497">也可使用 **ICACLS** 工具透過命令提示字元授與存取權限。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-497">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="ff8d0-498">使用 *DefaultAppPool*作為範例，將會使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-498">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="ff8d0-499">如需詳細資訊，請參閱 [icacls](/windows-server/administration/windows-commands/icacls) 主題。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-499">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="http2-support"></a><span data-ttu-id="ff8d0-500">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="ff8d0-500">HTTP/2 support</span></span>

<span data-ttu-id="ff8d0-501">在下列 IIS 部署案例中，ASP.NET Core 支援 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-501">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following IIS deployment scenarios:</span></span>

* <span data-ttu-id="ff8d0-502">內含式</span><span class="sxs-lookup"><span data-stu-id="ff8d0-502">In-process</span></span>
  * <span data-ttu-id="ff8d0-503">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="ff8d0-503">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="ff8d0-504">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="ff8d0-504">TLS 1.2 or later connection</span></span>
* <span data-ttu-id="ff8d0-505">跨處理序</span><span class="sxs-lookup"><span data-stu-id="ff8d0-505">Out-of-process</span></span>
  * <span data-ttu-id="ff8d0-506">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="ff8d0-506">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="ff8d0-507">公開 Edge Server 連線使用 HTTP/2，但是對 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的反向 Proxy 連線使用 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-507">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
  * <span data-ttu-id="ff8d0-508">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="ff8d0-508">TLS 1.2 or later connection</span></span>

<span data-ttu-id="ff8d0-509">針對建立 HTTP/2 連線時的同處理序部署，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會回報 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-509">For an in-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span> <span data-ttu-id="ff8d0-510">針對建立 HTTP/2 連線時的跨處理序部署，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會回報 `HTTP/1.1`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-510">For an out-of-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="ff8d0-511">如需有關同處理序和跨處理序主控模型的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-511">For more information on the in-process and out-of-process hosting models, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="ff8d0-512">HTTP/2 預設為啟用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-512">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="ff8d0-513">如果 HTTP/2 連線尚未建立，連線會退為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-513">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="ff8d0-514">如需使用 IIS 部署之 HTTP/2 設定的詳細資訊，請參閱 [IIS 上的 HTTP/2](/iis/get-started/whats-new-in-iis-10/http2-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-514">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="cors-preflight-requests"></a><span data-ttu-id="ff8d0-515">CORS 預檢要求</span><span class="sxs-lookup"><span data-stu-id="ff8d0-515">CORS preflight requests</span></span>

<span data-ttu-id="ff8d0-516">*此節只適用於以 .NET Framework 為目標的 ASP.NET Core 應用程式。*</span><span class="sxs-lookup"><span data-stu-id="ff8d0-516">*This section only applies to ASP.NET Core apps that target the .NET Framework.*</span></span>

<span data-ttu-id="ff8d0-517">針對以 .NET Framework 為目標的 ASP.NET Core 應用程式，在 IIS 中OPTIONS 要求預設不會傳遞到應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-517">For an ASP.NET Core app that targets the .NET Framework, OPTIONS requests aren't passed to the app by default in IIS.</span></span> <span data-ttu-id="ff8d0-518">若要瞭解如何在*web.config*中設定應用程式的 IIS 處理常式以傳遞選項要求，請參閱[在 ASP.NET Web API 2 中啟用跨原始來源要求： CORS 的運作方式](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-518">To learn how to configure the app's IIS handlers in *web.config* to pass OPTIONS requests, see [Enable cross-origin requests in ASP.NET Web API 2: How CORS Works](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).</span></span>

## <a name="application-initialization-module-and-idle-timeout"></a><span data-ttu-id="ff8d0-519">應用程式初始化模組與閒置逾時</span><span class="sxs-lookup"><span data-stu-id="ff8d0-519">Application Initialization Module and Idle Timeout</span></span>

<span data-ttu-id="ff8d0-520">在 IIS 中由 ASP.NET Core 模組版本 2 裝載時：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-520">When hosted in IIS by the ASP.NET Core Module version 2:</span></span>

* <span data-ttu-id="ff8d0-521">[應用程式初始化模組](#application-initialization-module)：應用程式裝載的同[進程](#in-process-hosting-model)或跨[進程](#out-of-process-hosting-model)可以設定為在背景工作進程重新開機或伺服器重新開機時自動啟動。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-521">[Application Initialization Module](#application-initialization-module): App's hosted [in-process](#in-process-hosting-model) or [out-of-process](#out-of-process-hosting-model) can be configured to start automatically on a worker process restart or server restart.</span></span>
* <span data-ttu-id="ff8d0-522">[閒置超時](#idle-timeout)：應用程式的裝載同[進程](#in-process-hosting-model)可以設定為不在非活動期間的時間超時。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-522">[Idle Timeout](#idle-timeout): App's hosted [in-process](#in-process-hosting-model) can be configured not to timeout during periods of inactivity.</span></span>

### <a name="application-initialization-module"></a><span data-ttu-id="ff8d0-523">應用程式初始化模組</span><span class="sxs-lookup"><span data-stu-id="ff8d0-523">Application Initialization Module</span></span>

<span data-ttu-id="ff8d0-524">*套用到應用程式裝載同處理序與非同處理序。*</span><span class="sxs-lookup"><span data-stu-id="ff8d0-524">*Applies to apps hosted in-process and out-of-process.*</span></span>

<span data-ttu-id="ff8d0-525">[IIS 應用程式初始化](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)是一項 IIS 功能，可在應用程式集區啟動或回收時，將 HTTP 要求傳送給應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-525">[IIS Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) is an IIS feature that sends an HTTP request to the app when the app pool starts or is recycled.</span></span> <span data-ttu-id="ff8d0-526">要求會觸發應用程式啟動。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-526">The request triggers the app to start.</span></span> <span data-ttu-id="ff8d0-527">根據預設，IIS 會發出應用程式根 URL (`/`) 的要求以初始化應用程式 (如需有關設定的詳細資訊，請參閱[額外資源](#application-initialization-module-and-idle-timeout-additional-resources))。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-527">By default, IIS issues a request to the app's root URL (`/`) to initialize the app (see the [additional resources](#application-initialization-module-and-idle-timeout-additional-resources) for more details on configuration).</span></span>

<span data-ttu-id="ff8d0-528">確認已啟用「IIS 應用程式初始化」角色功能：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-528">Confirm that the IIS Application Initialization role feature in enabled:</span></span>

<span data-ttu-id="ff8d0-529">在 Windows 7 或更新的電腦系統上，當在本機使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-529">On Windows 7 or later desktop systems when using IIS locally:</span></span>

1. <span data-ttu-id="ff8d0-530">瀏覽到 [控制台]\*\* [程式]\*\* > \*\* [程式和功能]\*\* > \*\*\*\* > **[開啟或關閉 Windows 功能]** \(畫面左側\)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-530">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="ff8d0-531">開啟 [網際網路資訊服務]\*\* [World Wide Web 服務]\*\* > \*\* [應用程式開發功能]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-531">Open **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="ff8d0-532">選取 [應用程式初始化]\*\*\*\* 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-532">Select the check box for **Application Initialization**.</span></span>

<span data-ttu-id="ff8d0-533">在 Windows Server 2008 R2 或更新版本上：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-533">On Windows Server 2008 R2 or later:</span></span>

1. <span data-ttu-id="ff8d0-534">開啟「新增角色與功能精靈」\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-534">Open the **Add Roles and Features Wizard**.</span></span>
1. <span data-ttu-id="ff8d0-535">在 [選取角色服務]\*\*\*\* 面板中，開啟 [應用程式開發]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-535">In the **Select role services** panel, open the **Application Development** node.</span></span>
1. <span data-ttu-id="ff8d0-536">選取 [應用程式初始化]\*\*\*\* 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-536">Select the check box for **Application Initialization**.</span></span>

<span data-ttu-id="ff8d0-537">使用下列任一方式為網站啟用應用程式初始化模組：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-537">Use either of the following approaches to enable the Application Initialization Module for the site:</span></span>

* <span data-ttu-id="ff8d0-538">使用 IIS 管理員：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-538">Using IIS Manager:</span></span>

  1. <span data-ttu-id="ff8d0-539">選取 [連線]\*\*\*\* 面板中的 [應用程式集區]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-539">Select **Application Pools** in the **Connections** panel.</span></span>
  1. <span data-ttu-id="ff8d0-540">以滑鼠右鍵按一下清單中應用程式的應用程式集區，然後選取 [進階設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-540">Right-click the app's app pool in the list and select **Advanced Settings**.</span></span>
  1. <span data-ttu-id="ff8d0-541">預設的 [啟動模式]\*\*\*\* 是 [OnDemand]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-541">The default **Start Mode** is **OnDemand**.</span></span> <span data-ttu-id="ff8d0-542">將 [啟動模式]\*\*\*\* 設定為 [AlwaysRunning]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-542">Set the **Start Mode** to **AlwaysRunning**.</span></span> <span data-ttu-id="ff8d0-543">選取 [確定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-543">Select **OK**.</span></span>
  1. <span data-ttu-id="ff8d0-544">開啟 [連線]\*\*\*\* 面板中的 [站台]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-544">Open the **Sites** node in the **Connections** panel.</span></span>
  1. <span data-ttu-id="ff8d0-545">以滑鼠右鍵按一下應用程式，然後選取 [管理網站]\*\* [進階設定]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-545">Right-click the app and select **Manage Website** > **Advanced Settings**.</span></span>
  1. <span data-ttu-id="ff8d0-546">預設 [預先載入已啟用]\*\*\*\* 設定是 [False]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-546">The default **Preload Enabled** setting is **False**.</span></span> <span data-ttu-id="ff8d0-547">將 [預先載入已啟用]\*\*\*\* 設定為 [True]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-547">Set **Preload Enabled** to **True**.</span></span> <span data-ttu-id="ff8d0-548">選取 [確定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-548">Select **OK**.</span></span>

* <span data-ttu-id="ff8d0-549">使用 *web.config*，新增 `<applicationInitialization>` 元素並將 `doAppInitAfterRestart` 設定為 `true` 至應用程式 *web.config* 檔案中的 `<system.webServer>` 元素：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-549">Using *web.config*, add the `<applicationInitialization>` element with `doAppInitAfterRestart` set to `true` to the `<system.webServer>` elements in the app's *web.config* file:</span></span>

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

### <a name="idle-timeout"></a><span data-ttu-id="ff8d0-550">閒置逾時</span><span class="sxs-lookup"><span data-stu-id="ff8d0-550">Idle Timeout</span></span>

<span data-ttu-id="ff8d0-551">*僅適用於應用程式裝載同處理序。*</span><span class="sxs-lookup"><span data-stu-id="ff8d0-551">*Only applies to apps hosted in-process.*</span></span>

<span data-ttu-id="ff8d0-552">若要防止應用程式閒置，請使用 IIS 管理員設定應用程式集區的閒置逾時：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-552">To prevent the app from idling, set the app pool's idle timeout using IIS Manager:</span></span>

1. <span data-ttu-id="ff8d0-553">選取 [連線]\*\*\*\* 面板中的 [應用程式集區]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-553">Select **Application Pools** in the **Connections** panel.</span></span>
1. <span data-ttu-id="ff8d0-554">以滑鼠右鍵按一下清單中應用程式的應用程式集區，然後選取 [進階設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-554">Right-click the app's app pool in the list and select **Advanced Settings**.</span></span>
1. <span data-ttu-id="ff8d0-555">預設 [閒置逾時 (分鐘)]\*\*\*\* 是 **20** 分鐘。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-555">The default **Idle Time-out (minutes)** is **20** minutes.</span></span> <span data-ttu-id="ff8d0-556">將 [閒置逾時 (分鐘)]\*\*\*\* 設定為 **0** (零)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-556">Set the **Idle Time-out (minutes)** to **0** (zero).</span></span> <span data-ttu-id="ff8d0-557">選取 [確定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-557">Select **OK**.</span></span>
1. <span data-ttu-id="ff8d0-558">回收背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-558">Recycle the worker process.</span></span>

<span data-ttu-id="ff8d0-559">若要防止應用程式裝載[非同處理序](#out-of-process-hosting-model)逾時，請使用下列任一方式：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-559">To prevent apps hosted [out-of-process](#out-of-process-hosting-model) from timing out, use either of the following approaches:</span></span>

* <span data-ttu-id="ff8d0-560">從外部服務對應用程式執行 Ping 以讓它繼續執行。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-560">Ping the app from an external service in order to keep it running.</span></span>
* <span data-ttu-id="ff8d0-561">若應用程式只裝載背景服務，避免 IIS 裝載並使用 [Windows 服務來裝載 ASP.NET Core 應用程式](xref:host-and-deploy/windows-service)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-561">If the app only hosts background services, avoid IIS hosting and use a [Windows Service to host the ASP.NET Core app](xref:host-and-deploy/windows-service).</span></span>

### <a name="application-initialization-module-and-idle-timeout-additional-resources"></a><span data-ttu-id="ff8d0-562">應用程式初始化模組與閒置逾時額外資源</span><span class="sxs-lookup"><span data-stu-id="ff8d0-562">Application Initialization Module and Idle Timeout additional resources</span></span>

* [<span data-ttu-id="ff8d0-563">IIS 8.0 應用程式初始化</span><span class="sxs-lookup"><span data-stu-id="ff8d0-563">IIS 8.0 Application Initialization</span></span>](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)
* <span data-ttu-id="ff8d0-564">[應用程式 \<applicationInitialization> 初始化](/iis/configuration/system.webserver/applicationinitialization/)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-564">[Application Initialization \<applicationInitialization>](/iis/configuration/system.webserver/applicationinitialization/).</span></span>
* <span data-ttu-id="ff8d0-565">[應用程式集 \<processModel> 區的進程模型設定](/iis/configuration/system.applicationhost/applicationpools/add/processmodel)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-565">[Process Model Settings for an Application Pool \<processModel>](/iis/configuration/system.applicationhost/applicationpools/add/processmodel).</span></span>

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="ff8d0-566">IIS 系統管理員的部署資源</span><span class="sxs-lookup"><span data-stu-id="ff8d0-566">Deployment resources for IIS administrators</span></span>

* [<span data-ttu-id="ff8d0-567">IIS 文件</span><span class="sxs-lookup"><span data-stu-id="ff8d0-567">IIS documentation</span></span>](/iis)
* [<span data-ttu-id="ff8d0-568">IIS 中的 IIS 管理員使用者入門</span><span class="sxs-lookup"><span data-stu-id="ff8d0-568">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* [<span data-ttu-id="ff8d0-569">.NET Core 應用程式部署</span><span class="sxs-lookup"><span data-stu-id="ff8d0-569">.NET Core application deployment</span></span>](/dotnet/core/deploying/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/directory-structure>
* <xref:host-and-deploy/iis/modules>
* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>

## <a name="additional-resources"></a><span data-ttu-id="ff8d0-570">其他資源</span><span class="sxs-lookup"><span data-stu-id="ff8d0-570">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:index>
* [<span data-ttu-id="ff8d0-571">Microsoft IIS 官方網站</span><span class="sxs-lookup"><span data-stu-id="ff8d0-571">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="ff8d0-572">Windows Server 技術內容庫</span><span class="sxs-lookup"><span data-stu-id="ff8d0-572">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="ff8d0-573">IIS 上的 HTTP/2</span><span class="sxs-lookup"><span data-stu-id="ff8d0-573">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
* <xref:host-and-deploy/iis/transform-webconfig>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="ff8d0-574">如需將 ASP.NET Core 應用程式發佈至 IIS 伺服器的教學課程體驗，請參閱<xref:tutorials/publish-to-iis>。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-574">For a tutorial experience on publishing an ASP.NET Core app to an IIS server, see <xref:tutorials/publish-to-iis>.</span></span>

[<span data-ttu-id="ff8d0-575">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="ff8d0-575">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="ff8d0-576">支援的作業系統</span><span class="sxs-lookup"><span data-stu-id="ff8d0-576">Supported operating systems</span></span>

<span data-ttu-id="ff8d0-577">以下為支援的作業系統：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-577">The following operating systems are supported:</span></span>

* <span data-ttu-id="ff8d0-578">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="ff8d0-578">Windows 7 or later</span></span>
* <span data-ttu-id="ff8d0-579">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="ff8d0-579">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="ff8d0-580">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys) (先前稱為 WebListener) 不適用搭配 IIS 的反向 Proxy 設定。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-580">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="ff8d0-581">請使用 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-581">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="ff8d0-582">如需在 Azure 中裝載的資訊，請參閱 <xref:host-and-deploy/azure-apps/index>。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-582">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

<span data-ttu-id="ff8d0-583">如需疑難排解指引，請參閱 <xref:test/troubleshoot>。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-583">For troubleshooting guidance, see <xref:test/troubleshoot>.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="ff8d0-584">支援的平台</span><span class="sxs-lookup"><span data-stu-id="ff8d0-584">Supported platforms</span></span>

<span data-ttu-id="ff8d0-585">支援針對 32 位元 (x86) 或 64 位元 (x64) 部署發行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-585">Apps published for 32-bit (x86) or 64-bit (x64) deployment are supported.</span></span> <span data-ttu-id="ff8d0-586">使用 32 位元 (x86) .NET Core SDK 部署 32 位元應用程式，除非應用程式：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-586">Deploy a 32-bit app with a 32-bit (x86) .NET Core SDK unless the app:</span></span>

* <span data-ttu-id="ff8d0-587">需要提供給 64 位元應用程式使用的較大虛擬記憶體位址空間。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-587">Requires the larger virtual memory address space available to a 64-bit app.</span></span>
* <span data-ttu-id="ff8d0-588">需要較大的 IIS 堆疊大小。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-588">Requires the larger IIS stack size.</span></span>
* <span data-ttu-id="ff8d0-589">有 64 位元原生相依性。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-589">Has 64-bit native dependencies.</span></span>

<span data-ttu-id="ff8d0-590">使用 64 位元 (x64) .NET Core SDK 來發行 64 位元應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-590">Use a 64-bit (x64) .NET Core SDK to publish a 64-bit app.</span></span> <span data-ttu-id="ff8d0-591">主機系統上必須有 64 位元執行階段存在。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-591">A 64-bit runtime must be present on the host system.</span></span>

## <a name="hosting-models"></a><span data-ttu-id="ff8d0-592">裝載模型</span><span class="sxs-lookup"><span data-stu-id="ff8d0-592">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="ff8d0-593">同處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="ff8d0-593">In-process hosting model</span></span>

<span data-ttu-id="ff8d0-594">使用同處理序裝載，ASP.NET Core 應用程式會在與其 IIS 工作者處理序相同的處理序中執行。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-594">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="ff8d0-595">同進程裝載可提供跨進程裝載的效能提升，原因如下：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-595">In-process hosting provides improved performance over out-of-process hosting because:</span></span>

* <span data-ttu-id="ff8d0-596">要求不會透過回送介面卡進行 proxy 處理。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-596">Requests aren't proxied over the loopback adapter.</span></span> <span data-ttu-id="ff8d0-597">回送介面卡是一種網路介面，可將連出的網路流量傳回給同一部電腦。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-597">A loopback adapter is a network interface that returns outgoing network traffic back to the same machine.</span></span>

<span data-ttu-id="ff8d0-598">IIS 透過 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 來執行處理程序管理。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-598">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="ff8d0-599">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-599">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module):</span></span>

* <span data-ttu-id="ff8d0-600">執行應用程式初始化。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-600">Performs app initialization.</span></span>
  * <span data-ttu-id="ff8d0-601">載入 [CoreCLR](/dotnet/standard/glossary#coreclr)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-601">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="ff8d0-602">呼叫 `Program.Main`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-602">Calls `Program.Main`.</span></span>
* <span data-ttu-id="ff8d0-603">處理 IIS 原生要求的存留期。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-603">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="ff8d0-604">以 .NET Framework 為目標的 ASP.NET Core 應用程式不支援處理序內裝載模型。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-604">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="ff8d0-605">下圖說明 IIS、ASP.NET Core 模組和同處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-605">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![同處理序代管內的 ASP.NET Core 模組案例](index/_static/ancm-inprocess.png)

<span data-ttu-id="ff8d0-607">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-607">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="ff8d0-608">驅動程式會在網站設定的連接埠上將原生要求路由至 IIS，此連接埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-608">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="ff8d0-609">ASP.NET Core 模組會接收原生要求，並將它傳遞至 IIS HTTP 伺服器（ `IISHttpServer` ）。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-609">The ASP.NET Core Module receives the native request and passes it to IIS HTTP Server (`IISHttpServer`).</span></span> <span data-ttu-id="ff8d0-610">IIS HTTP 伺服器是 IIS 的同處理序伺服程式實作，可將要求從原生轉換為受控。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-610">IIS HTTP Server is an in-process server implementation for IIS that converts the request from native to managed.</span></span>

<span data-ttu-id="ff8d0-611">IIS HTTP 伺服器處理要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-611">After the IIS HTTP Server processes the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="ff8d0-612">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-612">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="ff8d0-613">應用程式的回應會透過 IIS HTTP 伺服器傳回 IIS。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-613">The app's response is passed back to IIS through IIS HTTP Server.</span></span> <span data-ttu-id="ff8d0-614">IIS 會將回應傳送到起始該要求的用戶端。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-614">IIS sends the response to the client that initiated the request.</span></span>

<span data-ttu-id="ff8d0-615">現有的應用程式可以選擇同處理序裝載，但 [dotnet new](/dotnet/core/tools/dotnet-new) 範本預設會對所有 IIS 和 IIS Express 案例使用同處理序裝載模型。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-615">In-process hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="ff8d0-616">`CreateDefaultBuilder` 會透過呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> 方法來啟動 [CoreCLR](/dotnet/standard/glossary#coreclr) 以新增 <xref:Microsoft.AspNetCore.Hosting.Server.IServer>，並在 IIS 工作者處理序 (*w3wp.exe* 或 *iisexpress.exe*) 內裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-616">`CreateDefaultBuilder` adds an <xref:Microsoft.AspNetCore.Hosting.Server.IServer> instance by calling the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> method to boot the [CoreCLR](/dotnet/standard/glossary#coreclr) and host the app inside of the IIS worker process (*w3wp.exe* or *iisexpress.exe*).</span></span> <span data-ttu-id="ff8d0-617">效能測試指出，相較於跨處理序裝載 .NET Core 應用程式，並將要求 Proxy 處理至 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器，裝載於同處理序可提供明顯更高的要求輸送量。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-617">Performance tests indicate that hosting a .NET Core app in-process delivers significantly higher request throughput compared to hosting the app out-of-process and proxying requests to [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="ff8d0-618">跨處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="ff8d0-618">Out-of-process hosting model</span></span>

<span data-ttu-id="ff8d0-619">因為 ASP.NET Core 應用程式會在與 IIS 背景工作進程不同的進程中執行，所以 ASP.NET Core 模組會處理進程管理。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-619">Because ASP.NET Core apps run in a process separate from the IIS worker process, the ASP.NET Core Module handles process management.</span></span> <span data-ttu-id="ff8d0-620">此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式關閉或損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-620">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="ff8d0-621">此行為基本上與執行同處理序，並由 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-621">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="ff8d0-622">下圖說明 IIS、ASP.NET Core 模組和跨處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-622">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![非同處理序代管內的 ASP.NET Core 模組案例](index/_static/ancm-outofprocess.png)

<span data-ttu-id="ff8d0-624">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-624">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="ff8d0-625">驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-625">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="ff8d0-626">此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，而且不會是通訊埠 80 或 443。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-626">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="ff8d0-627">此模組在啟動時透過環境變數指定連接埠，而 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 延伸模組則會設定伺服器來接聽 `http://localhost:{PORT}`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-627">The module specifies the port via an environment variable at startup, and the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> extension configures the server to listen on `http://localhost:{PORT}`.</span></span> <span data-ttu-id="ff8d0-628">將會執行額外檢查，不是源自模組的要求都會遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-628">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="ff8d0-629">此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-629">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="ff8d0-630">Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-630">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="ff8d0-631">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-631">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="ff8d0-632">IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-632">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="ff8d0-633">應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-633">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="ff8d0-634">如需 ASP.NET Core 模組組態指南，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-634">For ASP.NET Core Module configuration guidance, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="ff8d0-635">如需代管的詳細資訊，請參閱[在 ASP.NET Core 中代管](xref:fundamentals/index#host)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-635">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/index#host).</span></span>

## <a name="application-configuration"></a><span data-ttu-id="ff8d0-636">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="ff8d0-636">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="ff8d0-637">啟用 IISIntegration 元件</span><span class="sxs-lookup"><span data-stu-id="ff8d0-637">Enable the IISIntegration components</span></span>

<span data-ttu-id="ff8d0-638">在 `CreateWebHostBuilder` （*Program.cs*）中建立主機時，請呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 以啟用 IIS 整合：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-638">When building a host in `CreateWebHostBuilder` (*Program.cs*), call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to enable IIS integration:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

<span data-ttu-id="ff8d0-639">如需 `CreateDefaultBuilder` 的詳細資訊，請參閱 <xref:fundamentals/host/web-host#set-up-a-host>。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-639">For more information on `CreateDefaultBuilder`, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

### <a name="iis-options"></a><span data-ttu-id="ff8d0-640">IIS 選項</span><span class="sxs-lookup"><span data-stu-id="ff8d0-640">IIS options</span></span>

<span data-ttu-id="ff8d0-641">**同處理序主控模型**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-641">**In-process hosting model**</span></span>

<span data-ttu-id="ff8d0-642">若要設定 IIS 伺服器選項，請在 <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 中加入 <xref:Microsoft.AspNetCore.Builder.IISServerOptions> 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-642">To configure IIS Server options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISServerOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="ff8d0-643">下列範例會停用 AutomaticAuthentication：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-643">The following example disables AutomaticAuthentication:</span></span>

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

| <span data-ttu-id="ff8d0-644">選項</span><span class="sxs-lookup"><span data-stu-id="ff8d0-644">Option</span></span>                         | <span data-ttu-id="ff8d0-645">預設</span><span class="sxs-lookup"><span data-stu-id="ff8d0-645">Default</span></span> | <span data-ttu-id="ff8d0-646">設定</span><span class="sxs-lookup"><span data-stu-id="ff8d0-646">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="ff8d0-647">若為 `true`，IIS 伺服器會設定由 [Windows 驗證](xref:security/authentication/windowsauth)所驗證的 `HttpContext.User`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-647">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="ff8d0-648">若為 `false`，則伺服器僅會對 `HttpContext.User` 提供身分識別，並在 `AuthenticationScheme` 明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-648">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="ff8d0-649">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-649">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="ff8d0-650">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-650">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="ff8d0-651">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-651">Sets the display name shown to users on login pages.</span></span> |

<span data-ttu-id="ff8d0-652">**跨處理序裝載模型**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-652">**Out-of-process hosting model**</span></span>

<span data-ttu-id="ff8d0-653">若要設定 IIS 選項，請在 <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 中加入 <xref:Microsoft.AspNetCore.Builder.IISOptions> 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-653">To configure IIS options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="ff8d0-654">下列範例會防止應用程式填入 `HttpContext.Connection.ClientCertificate`：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-654">The following example prevents the app from populating `HttpContext.Connection.ClientCertificate`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="ff8d0-655">選項</span><span class="sxs-lookup"><span data-stu-id="ff8d0-655">Option</span></span>                         | <span data-ttu-id="ff8d0-656">預設</span><span class="sxs-lookup"><span data-stu-id="ff8d0-656">Default</span></span> | <span data-ttu-id="ff8d0-657">設定</span><span class="sxs-lookup"><span data-stu-id="ff8d0-657">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="ff8d0-658">若為 `true`，[IIS 整合中介軟體](#enable-the-iisintegration-components)會設定由 [Windows 驗證](xref:security/authentication/windowsauth)所驗證的 `HttpContext.User`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-658">If `true`, [IIS Integration Middleware](#enable-the-iisintegration-components) sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="ff8d0-659">如果為 `false`，則驗證中介軟體僅針對 `HttpContext.User` 提供身分識別，並在游 `AuthenticationScheme` 提出明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-659">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="ff8d0-660">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-660">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="ff8d0-661">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-661">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="ff8d0-662">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-662">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="ff8d0-663">如果為 `true` 且 `MS-ASPNETCORE-CLIENTCERT` 要求標頭已存在，則會填入 `HttpContext.Connection.ClientCertificate`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-663">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="ff8d0-664">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="ff8d0-664">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="ff8d0-665">用來設定轉送標頭中介軟體及 ASP.NET Core 模組的 [IIS 整合中介軟體](#enable-the-iisintegration-components)會設定為轉送配置 (HTTP/HTTPS) 與發出要求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-665">The [IIS Integration Middleware](#enable-the-iisintegration-components), which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="ff8d0-666">其他 Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-666">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="ff8d0-667">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-667">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="ff8d0-668">web.config 檔案</span><span class="sxs-lookup"><span data-stu-id="ff8d0-668">web.config file</span></span>

<span data-ttu-id="ff8d0-669">*web.config* 檔案是用來設定 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-669">The *web.config* file configures the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="ff8d0-670">發佈專案時，由 MSBuild 目標 (`_TransformWebConfig`) 處理 *web.config* 檔案的建立、轉換及發佈。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-670">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="ff8d0-671">此目標存在於 Web SDK 目標 (`Microsoft.NET.Sdk.Web`)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-671">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="ff8d0-672">SDK 設定在專案檔的頂端：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-672">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="ff8d0-673">如果專案中沒有 *web.config* 檔案，則系統會使用正確的 *processPath* 和 *arguments* 建立該檔案以設定 ASP.NET Core 模組，並將該檔案移至[已發行的輸出](xref:host-and-deploy/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-673">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="ff8d0-674">如果 *web.config* 檔案存在於專案中，則系統會使用正確的 *processPath* 和 *arguments* 來轉換該檔案以設定 ASP.NET Core 模組，然後將它移至已發行的輸出。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-674">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="ff8d0-675">轉換不會修改檔案中的 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-675">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="ff8d0-676">*web.config* 檔案可提供能控制作用中 IIS 模組的額外 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-676">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="ff8d0-677">如需能處理 ASP.NET Core 應用程式要求之 IIS 模組的相關資訊，請參閱 [IIS 模組](xref:host-and-deploy/iis/modules)主題。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-677">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="ff8d0-678">若要防止 Web SDK 轉換*web.config*檔案，請使用專案檔 **\<IsTransformWebConfigDisabled>** 中的屬性：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-678">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="ff8d0-679">使 Web SDK 無法轉換檔案時，應該由開發人員手動設定 *processPath* 和 *arguments*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-679">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="ff8d0-680">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module> 。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-680">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="ff8d0-681">web.config 檔案位置</span><span class="sxs-lookup"><span data-stu-id="ff8d0-681">web.config file location</span></span>

<span data-ttu-id="ff8d0-682">為了正確設定[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)， *web.config*檔案必須存在於已部署應用程式的[內容根](xref:fundamentals/index#content-root)路徑（通常是應用程式基底路徑）。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-682">In order to set up the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) correctly, the *web.config* file must be present at the [content root](xref:fundamentals/index#content-root) path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="ff8d0-683">這是與提供給 IIS 的網站實體路徑相同的位置。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-683">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="ff8d0-684">應用程式的根目錄需有 *web.config* 檔案，才能使用 Web Deploy 發行多個應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-684">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="ff8d0-685">機密檔案存在於應用程式的實體路徑上，例如\* \<assembly>.runtimeconfig.json*、 \* \<assembly> .xml* （xml 檔批註）和\* \<assembly>.deps.js\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-685">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="ff8d0-686">當 *web.config* 檔案存在且網站正常啟動時，如果有人要求機密檔案，IIS 不會予以提供。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-686">When the *web.config* file is present and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="ff8d0-687">若 *web.config* 檔案遺失或沒有正確命名，或是無法設定網站以正常啟動，IIS 可能會公開提供機密檔案。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-687">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="ff8d0-688">\***web.config*檔案必須隨時存在於部署中、正確命名，而且能夠將網站設定為正常啟動。絕對不要從生產環境部署移除*web.config\*檔案。**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-688">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

### <a name="transform-webconfig"></a><span data-ttu-id="ff8d0-689">轉換 web.config</span><span class="sxs-lookup"><span data-stu-id="ff8d0-689">Transform web.config</span></span>

<span data-ttu-id="ff8d0-690">如需在發佈時轉換 *web.config* (例如依據設定、設定檔或環境設定環境變數)，請參閱<xref:host-and-deploy/iis/transform-webconfig>。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-690">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="ff8d0-691">IIS 組態</span><span class="sxs-lookup"><span data-stu-id="ff8d0-691">IIS configuration</span></span>

<span data-ttu-id="ff8d0-692">**Windows Server 作業系統**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-692">**Windows Server operating systems**</span></span>

<span data-ttu-id="ff8d0-693">啟用**網頁伺服器 (IIS)** 伺服器角色，並建立角色服務。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-693">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="ff8d0-694">使用來自 [管理]\*\*\*\* 功能表的 [新增角色及功能]\*\*\*\* 精靈，或是 [伺服器管理員]\*\*\*\* 中的連結。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-694">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="ff8d0-695">在**伺服器角色**步驟中，核取 [網頁伺服器 (IIS)]\*\*\*\* 方塊。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-695">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![在選取伺服器角色步驟中選取網頁伺服器 IIS 角色。](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="ff8d0-697">在 [功能]\*\*\*\* 步驟之後，[角色服務]\*\*\*\* 步驟會針對網頁伺服器 (IIS) 進行載入。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-697">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="ff8d0-698">選取所需的 IIS 角色服務或接受所提供的預設角色服務。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-698">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![在選取角色服務步驟中，選取預設的角色服務。](index/_static/role-services-ws2016.png)

   <span data-ttu-id="ff8d0-700">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-700">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="ff8d0-701">若要啟用 Windows 驗證，請展開下列節點： [**網頁伺服器**  >  **安全性**]。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-701">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="ff8d0-702">選取 [Windows 驗證]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-702">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="ff8d0-703">如需詳細資訊，請參閱[Windows 驗證 \<windowsAuthentication> ](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)和[設定 windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-703">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="ff8d0-704">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-704">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="ff8d0-705">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-705">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="ff8d0-706">若要啟用 websocket，請展開下列節點： [**網頁伺服器**  >  **應用程式開發**]。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-706">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="ff8d0-707">選取 [WebSocket 通訊協定]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-707">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="ff8d0-708">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-708">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="ff8d0-709">透過**確認**步驟繼續作業，安裝網頁伺服器角色和服務。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-709">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="ff8d0-710">安裝**網頁伺服器（iis）** 角色之後，不需要重新開機伺服器/iis。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-710">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="ff8d0-711">**Windows 桌面作業系統**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-711">**Windows desktop operating systems**</span></span>

<span data-ttu-id="ff8d0-712">啟用 [IIS 管理主控台]\*\*\*\* 和 [World Wide Web 服務]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-712">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="ff8d0-713">瀏覽到 [控制台]\*\* [程式]\*\* > \*\* [程式和功能]\*\* > \*\*\*\* > **[開啟或關閉 Windows 功能]** \(畫面左側\)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-713">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="ff8d0-714">開啟 [Internet Information Services]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-714">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="ff8d0-715">開啟 [Web 管理工具]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-715">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="ff8d0-716">核取 [IIS 管理主控台]\*\*\*\* 方塊。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-716">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="ff8d0-717">[World Wide Web Services] (全球資訊網服務)\*\*\*\* 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-717">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="ff8d0-718">接受**全球資訊網服務**的預設功能，或自訂 IIS 功能。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-718">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="ff8d0-719">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-719">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="ff8d0-720">若要啟用 Windows 驗證，請展開下列節點： **World Wide Web 服務**  >  **安全性**。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-720">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="ff8d0-721">選取 [Windows 驗證]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-721">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="ff8d0-722">如需詳細資訊，請參閱[Windows 驗證 \<windowsAuthentication> ](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)和[設定 windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-722">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="ff8d0-723">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-723">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="ff8d0-724">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-724">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="ff8d0-725">若要啟用 websocket，請展開下列節點： [ **World Wide Web 服務**] [  >  **應用程式開發] 功能**。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-725">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="ff8d0-726">選取 [WebSocket 通訊協定]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-726">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="ff8d0-727">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-727">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="ff8d0-728">若 IIS 安裝需要重新啟動，請重新啟動系統。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-728">If the IIS installation requires a restart, restart the system.</span></span>

![選取 [Windows 功能] 中的 [IIS 管理主控台] 和 [World Wide Web Services] (全球資訊網服務)。](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="ff8d0-730">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="ff8d0-730">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="ff8d0-731">在主控系統上安裝 .NET Core 裝載套件組合\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-731">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="ff8d0-732">套件組合會安裝 .NET Core 執行時間、.NET Core 程式庫和[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-732">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="ff8d0-733">此模組可讓 ASP.NET Core 應用程式在 IIS 背後執行。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-733">The module allows ASP.NET Core apps to run behind IIS.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ff8d0-734">若裝載套件組合在 IIS 之前安裝，則必須對該套件組合安裝進行修復。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-734">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="ff8d0-735">請在安裝 IIS 之後，再次執行裝載套件組合安裝程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-735">Run the Hosting Bundle installer again after installing IIS.</span></span>
>
> <span data-ttu-id="ff8d0-736">如果在安裝 64 位元 (x64) 版本的 .NET Core 後才安裝裝載套件組合，那麼可能會遺漏 SDK ([未偵測到 .NET Core SDK](xref:test/troubleshoot#no-net-core-sdks-were-detected))。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-736">If the Hosting Bundle is installed after installing the 64-bit (x64) version of .NET Core, SDKs might appear to be missing ([No .NET Core SDKs were detected](xref:test/troubleshoot#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="ff8d0-737">若要解決此問題，請參閱 <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-737">To resolve the problem, see <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>.</span></span>

### <a name="download"></a><span data-ttu-id="ff8d0-738">下載</span><span class="sxs-lookup"><span data-stu-id="ff8d0-738">Download</span></span>

1. <span data-ttu-id="ff8d0-739">流覽至 [[下載 .Net Core](https://dotnet.microsoft.com/download/dotnet-core) ] 頁面。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-739">Navigate to the [Download .NET Core](https://dotnet.microsoft.com/download/dotnet-core) page.</span></span>
1. <span data-ttu-id="ff8d0-740">選取所需的 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-740">Select the desired .NET Core version.</span></span>
1. <span data-ttu-id="ff8d0-741">在 [執行應用程式 - 執行階段]\*\*\*\* 欄中，尋找想要的 .NET Core 執行階段版本列。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-741">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="ff8d0-742">使用**裝載**套件組合連結來下載安裝程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-742">Download the installer using the **Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="ff8d0-743">某些安裝程式包含已達到期生命週期結束 (EOL) 的發行版本，這些發行版本已不受 Microsoft 支援。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-743">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="ff8d0-744">如需詳細資訊，請參閱[支援原則](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) \(英文 \)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-744">For more information, see the [support policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="ff8d0-745">安裝裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="ff8d0-745">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="ff8d0-746">在伺服器上執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-746">Run the installer on the server.</span></span> <span data-ttu-id="ff8d0-747">從系統管理員命令殼層執行安裝程式時，有 下列參數可用：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-747">The following parameters are available when running the installer from an administrator command shell:</span></span>

   * <span data-ttu-id="ff8d0-748">`OPT_NO_ANCM=1`：略過安裝 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-748">`OPT_NO_ANCM=1`: Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="ff8d0-749">`OPT_NO_RUNTIME=1`：略過安裝 .NET Core 執行時間。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-749">`OPT_NO_RUNTIME=1`: Skip installing the .NET Core runtime.</span></span> <span data-ttu-id="ff8d0-750">當伺服器只裝載[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)時使用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-750">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="ff8d0-751">`OPT_NO_SHAREDFX=1`：略過安裝 ASP.NET 共用架構（ASP.NET 執行時間）。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-751">`OPT_NO_SHAREDFX=1`: Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span> <span data-ttu-id="ff8d0-752">當伺服器只裝載[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)時使用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-752">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="ff8d0-753">`OPT_NO_X86=1`：略過安裝 x86 執行時間。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-753">`OPT_NO_X86=1`: Skip installing x86 runtimes.</span></span> <span data-ttu-id="ff8d0-754">當您確定不會裝載 32 位元應用程式時，請使用此參數。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-754">Use this parameter when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="ff8d0-755">如果將來有可能同時裝載 32 位元和 64 位元應用程式，請不要使用此參數並安裝這兩個執行階段。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-755">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this parameter and install both runtimes.</span></span>
   * <span data-ttu-id="ff8d0-756">`OPT_NO_SHARED_CONFIG_CHECK=1`：當共用設定（*applicationHost.config*）位於與 IIS 安裝相同的電腦上時，請停用使用 iis 共用設定的檢查。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-756">`OPT_NO_SHARED_CONFIG_CHECK=1`: Disable the check for using an IIS Shared Configuration when the shared configuration (*applicationHost.config*) is on the same machine as the IIS installation.</span></span> <span data-ttu-id="ff8d0-757">*只在 ASP.NET Core 2.2 或更新版本的裝載套件組合安裝程式上可用。*</span><span class="sxs-lookup"><span data-stu-id="ff8d0-757">*Only available for ASP.NET Core 2.2 or later Hosting Bundler installers.*</span></span> <span data-ttu-id="ff8d0-758">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration> 。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-758">For more information, see <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>.</span></span>
1. <span data-ttu-id="ff8d0-759">重新開機系統，或在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-759">Restart the system or execute the following commands in a command shell:</span></span>

   ```console
   net stop was /y
   net start w3svc
   ```
   <span data-ttu-id="ff8d0-760">重新啟動 IIS 將能偵測到由安裝程式對系統路徑 (此為環境變數) 所做出的變更。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-760">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

<span data-ttu-id="ff8d0-761">安裝裝載套件組合時，不需要手動停止 IIS 中的個別網站。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-761">It isn't necessary to manually stop individual sites in IIS when installing the Hosting Bundle.</span></span> <span data-ttu-id="ff8d0-762">裝載的應用程式（IIS 網站）會在 IIS 重新開機時重新開機。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-762">Hosted apps (IIS sites) restart when IIS restarts.</span></span> <span data-ttu-id="ff8d0-763">應用程式會在收到第一個要求時重新開機，包括從[應用程式初始化模組](#application-initialization-module-and-idle-timeout)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-763">Apps start up again when they receive their first request, including from the [Application Initialization Module](#application-initialization-module-and-idle-timeout).</span></span>

<span data-ttu-id="ff8d0-764">ASP.NET Core 採用共用架構封裝修補程式版本的向前復原行為。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-764">ASP.NET Core adopts roll-forward behavior for patch releases of shared framework packages.</span></span> <span data-ttu-id="ff8d0-765">當 IIS 所裝載的應用程式使用 IIS 重新開機時，應用程式會在收到第一個要求時，以其所參考套件的最新修補程式版本來載入。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-765">When apps hosted by IIS restart with IIS, the apps load with the latest patch releases of their referenced packages when they receive their first request.</span></span> <span data-ttu-id="ff8d0-766">如果未重新開機 IIS，應用程式會在其工作者進程回收時重新開機並展示向前復原行為，並接收其第一個要求。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-766">If IIS isn't restarted, apps restart and exhibit roll-forward behavior when their worker processes are recycled and they receive their first request.</span></span>

> [!NOTE]
> <span data-ttu-id="ff8d0-767">如需 IIS 共用組態的資訊，請參閱[使用 IIS 共用組態的 ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-767">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="ff8d0-768">使用 Visual Studio 發佈時安裝 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="ff8d0-768">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="ff8d0-769">將應用程式部署到具有 [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later) 的伺服器時，請在伺服器上安裝最新版的 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-769">When deploying apps to servers with [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="ff8d0-770">若要安裝 Web Deploy，請使用 [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) 或從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=43717)直接取得安裝程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-770">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="ff8d0-771">慣用的方法是使用 WebPI。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-771">The preferred method is to use WebPI.</span></span> <span data-ttu-id="ff8d0-772">WebPI 提供獨立的安裝程式和組態以裝載提供者。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-772">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="ff8d0-773">建立 IIS 網站</span><span class="sxs-lookup"><span data-stu-id="ff8d0-773">Create the IIS site</span></span>

1. <span data-ttu-id="ff8d0-774">在主控系統上，請建立資料夾以容納應用程式的已發行資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-774">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="ff8d0-775">在下列步驟中，您提供資料夾路徑給 IIS，作為應用程式的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-775">In a following step, the folder's path is provided to IIS as the physical path to the app.</span></span> <span data-ttu-id="ff8d0-776">如需應用程式之部署資料夾和檔案配置的詳細資訊，請參閱 <xref:host-and-deploy/directory-structure>。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-776">For more information on an app's deployment folder and file layout, see <xref:host-and-deploy/directory-structure>.</span></span>

1. <span data-ttu-id="ff8d0-777">在 [IIS 管理員] 中 **，在 [** 連線] 面板中開啟伺服器的節點。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-777">In IIS Manager, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="ff8d0-778">以滑鼠右鍵按一下 [網站]\*\*\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-778">Right-click the **Sites** folder.</span></span> <span data-ttu-id="ff8d0-779">從操作功能表選取 [新增網站]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-779">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="ff8d0-780">提供**網站名稱**，並將**實體路徑**設定為應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-780">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="ff8d0-781">藉由**Binding**選取 **[確定]** 來提供系結設定並建立網站：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-781">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![在新增網站步驟中提供站台名稱、實體路徑和主機名稱。](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="ff8d0-783">請**勿**使用最上層萬用字元繫結 (`http://*:80/`與 `http://+:80`)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-783">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="ff8d0-784">最上層萬用字元繫結可能暴露您的應用程式安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-784">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="ff8d0-785">這對強式與弱式萬用字元皆適用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-785">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="ff8d0-786">請使用明確主機名稱，而非萬用字元。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-786">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="ff8d0-787">若您擁有整個父網域 (與具弱點的 `*.com` 相對) 的控制權，則子網域萬用字元繫結 (例如 `*.mysub.com`) 就沒有此安全性風險。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-787">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="ff8d0-788">如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-788">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="ff8d0-789">在伺服器的節點之下，選取 [應用程式集區]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-789">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="ff8d0-790">以滑鼠右鍵按一下網站的應用程式集區，然後從操作功能表選取 [基本設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-790">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="ff8d0-791">在 [編輯應用程式集區]\*\*\*\* 視窗中，將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\*：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-791">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![將 .NET CLR 版本設為 [沒有受控碼]。](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="ff8d0-793">ASP.NET Core 會在不同的處理序中執行，並管理執行階段。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-793">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="ff8d0-794">ASP.NET Core 不仰賴載入桌面 CLR (.NET CLR)&mdash;會使用 .NET Core 的核心通用語言執行平台 (CoreCLR) 來開機以在背景工作處理序中裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-794">ASP.NET Core doesn't rely on loading the desktop CLR (.NET CLR)&mdash;the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process.</span></span> <span data-ttu-id="ff8d0-795">將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\* 是選擇性的，但建議這樣做。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-795">Setting the **.NET CLR version** to **No Managed Code** is optional but recommended.</span></span>

1. <span data-ttu-id="ff8d0-796">*ASP.NET Core 2.2 或更新版本*：對於使用[同處理序主控模型](#in-process-hosting-model)的 64 位元 (x64) [獨立式部署](/dotnet/core/deploying/#self-contained-deployments-scd)，會停用 32 位元 (x86) 處理序的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-796">*ASP.NET Core 2.2 or later*: For a 64-bit (x64) [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) that uses the [in-process hosting model](#in-process-hosting-model), disable the app pool for 32-bit (x86) processes.</span></span>

   <span data-ttu-id="ff8d0-797">在 IIS 管理員的 [動作]\*\*\*\* 資訊看板 > [應用程式集區]\*\*\*\* 中，選取 [設定應用程式集區預設值]\*\*\*\* 或 [進階設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-797">In the **Actions** sidebar of IIS Manager > **Application Pools**, select **Set Application Pool Defaults** or **Advanced Settings**.</span></span> <span data-ttu-id="ff8d0-798">找到 [啟用 32 位元應用程式]\*\*\*\*，然後將其值設定為 `False`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-798">Locate **Enable 32-Bit Applications** and set the value to `False`.</span></span> <span data-ttu-id="ff8d0-799">此設定不會影響為[處理程序外裝載](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model)部署的應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-799">This setting doesn't affect apps deployed for [out-of-process hosting](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).</span></span>

1. <span data-ttu-id="ff8d0-800">確認處理序模型身分識別具有適當的權限。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-800">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="ff8d0-801">如果應用程式集區的預設識別（**進程模型**  >  **Identity** ）從**ApplicationPoolIdentity**變更為另一個身分識別，請確認新的身分識別具有存取應用程式資料夾、資料庫和其他必要資源的必要許可權。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-801">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="ff8d0-802">例如，應用程式集區需要針對應用程式讀取和寫入檔案的資料夾取得讀取和寫入權限。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-802">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="ff8d0-803">**Windows 驗證設定 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-803">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="ff8d0-804">如需詳細資訊，請參閱[設定 Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-804">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="ff8d0-805">部署應用程式</span><span class="sxs-lookup"><span data-stu-id="ff8d0-805">Deploy the app</span></span>

<span data-ttu-id="ff8d0-806">將應用程式部署至在[建立 IIS 網站](#create-the-iis-site)一節中所建立的 IIS **實體路徑**資料夾。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-806">Deploy the app to the IIS **Physical path** folder that was established in the [Create the IIS site](#create-the-iis-site) section.</span></span> <span data-ttu-id="ff8d0-807">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 是建議的部署機制，但有數個選項可將應用程式從專案的 [publish]\*\* 資料夾移動至主機系統的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-807">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment, but several options exist for moving the app from the project's *publish* folder to the hosting system's deployment folder.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="ff8d0-808">使用 Visual Studio 的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="ff8d0-808">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="ff8d0-809">請參閱[適用於 ASP.NET Core 應用程式部署的 Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)主題，了解如何建立搭配 Web Deploy 使用的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-809">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="ff8d0-810">如果主機服務提供者提供或支援建立發行設定檔，請下載其設定檔，並使用 Visual Studio 的 [發行]\*\*\*\* 對話方塊匯入其設定檔：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-810">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog:</span></span>

![[發佈] 對話方塊頁](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="ff8d0-812">Visual Studio 外部的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="ff8d0-812">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="ff8d0-813">透過命令列也可以在 Visual Studio 外部使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-813">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="ff8d0-814">如需詳細資訊，請參閱 [Web 部署工具](/iis/publish/using-web-deploy/use-the-web-deployment-tool)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-814">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="ff8d0-815">Web Deploy 的替代項目</span><span class="sxs-lookup"><span data-stu-id="ff8d0-815">Alternatives to Web Deploy</span></span>

<span data-ttu-id="ff8d0-816">使用數種方法的其中一種將應用程式移到主機系統，例如手動複製、[Xcopy](/windows-server/administration/windows-commands/xcopy)、[Robocopy](/windows-server/administration/windows-commands/robocopy) 或 [PowerShell](/powershell/)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-816">Use any of several methods to move the app to the hosting system, such as manual copy, [Xcopy](/windows-server/administration/windows-commands/xcopy), [Robocopy](/windows-server/administration/windows-commands/robocopy), or [PowerShell](/powershell/).</span></span>

<span data-ttu-id="ff8d0-817">如需 IIS 的 ASP.NET Core 部署詳細資訊，請參閱 [IIS 系統管理員的部署資源](#deployment-resources-for-iis-administrators)一節。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-817">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="ff8d0-818">瀏覽網站</span><span class="sxs-lookup"><span data-stu-id="ff8d0-818">Browse the website</span></span>

<span data-ttu-id="ff8d0-819">應用程式部署到主機系統之後，請向其中一個應用程式的公用端點發出要求。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-819">After the app is deployed to the hosting system, make a request to one of the app's public endpoints.</span></span>

<span data-ttu-id="ff8d0-820">在下列範例中，網站繫結至 IIS 上的**連接埠** `80`，其**主機名稱**為 `www.mysite.com`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-820">In the following example, the site is bound to an IIS **Host name** of `www.mysite.com` on **Port** `80`.</span></span> <span data-ttu-id="ff8d0-821">對 `http://www.mysite.com` 發出要求：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-821">A request is made to `http://www.mysite.com`:</span></span>

![Microsoft Edge 瀏覽器已載入 IIS 啟動頁面。](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="ff8d0-823">已鎖定的部署檔案</span><span class="sxs-lookup"><span data-stu-id="ff8d0-823">Locked deployment files</span></span>

<span data-ttu-id="ff8d0-824">當應用程式執行時，會鎖定部署資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-824">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="ff8d0-825">無法於部署期間覆寫已鎖定的檔案。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-825">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="ff8d0-826">若要釋放部署中的已鎖定檔案，請使用下列其中**一種**方法停止應用程式集區：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-826">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="ff8d0-827">使用 Web Deploy 並參考專案檔中的 `Microsoft.NET.Sdk.Web`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-827">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="ff8d0-828">*app_offline.htm* 檔案是放在 Web 應用程式目錄的根目錄中。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-828">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="ff8d0-829">當檔案存在時，ASP.NET Core 模組會正常關閉應用程式，並在部署期間提供 *app_offline.htm* 檔案。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-829">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="ff8d0-830">如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-830">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="ff8d0-831">在伺服器上的 IIS 管理員中手動停止應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-831">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="ff8d0-832">使用 PowerShell 卸載*app_offline.htm* （需要 PowerShell 5 或更新版本）：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-832">Use PowerShell to drop *app_offline.htm* (requires PowerShell 5 or later):</span></span>

  ```powershell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a><span data-ttu-id="ff8d0-833">資料保護</span><span class="sxs-lookup"><span data-stu-id="ff8d0-833">Data protection</span></span>

<span data-ttu-id="ff8d0-834">[ASP.NET Core 資料保護堆疊](xref:security/data-protection/introduction)是由數個 ASP.NET Core [中介軟體](xref:fundamentals/middleware/index)所使用，包括用於驗證的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-834">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="ff8d0-835">即使資料保護 API 不是由使用者程式碼呼叫，仍應使用部署指令碼或是在使用者程式碼中設定資料保護，以建立持續性的密碼編譯[金鑰存放區](xref:security/data-protection/implementation/key-management)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-835">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="ff8d0-836">如不設定資料保護，金鑰會保留在記憶體中，並於應用程式重新啟動時捨棄。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-836">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="ff8d0-837">如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-837">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="ff8d0-838">所有以 Cookie 為基礎的驗證權杖都會失效。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-838">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="ff8d0-839">當使用者提出下一個要求時，需要再次登入。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-839">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="ff8d0-840">所有以 Keyring 保護的資料都無法再解密。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-840">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="ff8d0-841">這可能會包含 [CSRF 權杖](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)和 [ASP.NET Core MVC TempData cookie](xref:fundamentals/app-state#tempdata)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-841">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="ff8d0-842">若要在 IIS 下設定資料保護以保存 Keyring，請使用下列其中**一種**方法：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-842">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="ff8d0-843">**建立資料保護登錄機碼**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-843">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="ff8d0-844">ASP.NET Core 應用程式所使用的資料保護金鑰會儲存在應用程式外部的登錄中。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-844">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="ff8d0-845">若要保存指定應用程式的金鑰，請為應用程式集區建立登錄機碼。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-845">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="ff8d0-846">若為獨立的非Web 伺服陣列 IIS 安裝，請針對搭配使用 ASP.NET Core 應用程式的每個應用程式集區，使用[資料保護 Provision-AutoGenKeys.ps1 PowerShell 指令碼](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-846">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="ff8d0-847">此指令碼會在 HKLM 登錄中建立登錄機碼，只有應用程式之應用程式集區的背景工作處理序帳戶可以存取它。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-847">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="ff8d0-848">在待用期間使用 DPAPI 和全電腦金鑰加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-848">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="ff8d0-849">在 Web 伺服陣列案例中，應用程式可以設定成使用 UNC 路徑來儲存其資料保護 Keyring。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-849">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="ff8d0-850">根據預設，資料保護金鑰不予加密。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-850">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="ff8d0-851">請確保網路共用的檔案權限僅限於執行應用程式的 Windows 帳戶。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-851">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="ff8d0-852">可以使用 X509 憑證來保護待用的金鑰。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-852">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="ff8d0-853">請考慮下列讓使用者上傳憑證的機制：將憑證放入使用者的受信任憑證存放區，並確保在執行使用者應用程式的所有電腦上都能使用這些憑證。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-853">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="ff8d0-854">如需詳細資訊，請參閱[設定 ASP.NET Core 資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-854">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="ff8d0-855">**設定 IIS 應用程式集區載入使用者設定檔**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-855">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="ff8d0-856">此設定位在應用程式集區 [進階設定]\*\*\*\* 下的 [處理序模型]\*\*\*\* 區段中。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-856">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="ff8d0-857">將 [載入使用者設定檔]\*\*\*\* 設為 `True`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-857">Set **Load User Profile** to `True`.</span></span> <span data-ttu-id="ff8d0-858">當設定為 `True` 時，金鑰會儲存在使用者設定檔目錄中，且使用具有使用者帳戶專屬金鑰的 DPAPI 保護。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-858">When set to `True`, keys are stored in the user profile directory and protected using DPAPI with a key specific to the user account.</span></span> <span data-ttu-id="ff8d0-859">金鑰會保存到 *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-859">Keys are persisted to the *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* folder.</span></span>

  <span data-ttu-id="ff8d0-860">應用程式集區的 [setProfileEnvironment 屬性](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration)也必須啟用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-860">The app pool's [setProfileEnvironment attribute](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) must also be enabled.</span></span> <span data-ttu-id="ff8d0-861">`setProfileEnvironment` 的預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-861">The default value of `setProfileEnvironment` is `true`.</span></span> <span data-ttu-id="ff8d0-862">在某些情況下 (例如 Windows OS)，`setProfileEnvironment` 會設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-862">In some scenarios (for example, Windows OS), `setProfileEnvironment` is set to `false`.</span></span> <span data-ttu-id="ff8d0-863">如果金鑰並未如預期地儲存在使用者設定檔目錄中：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-863">If keys aren't stored in the user profile directory as expected:</span></span>

  1. <span data-ttu-id="ff8d0-864">瀏覽至 *%windir%/system32/inetsrv/config* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-864">Navigate to the *%windir%/system32/inetsrv/config* folder.</span></span>
  1. <span data-ttu-id="ff8d0-865">開啟 *applicationHost.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-865">Open the *applicationHost.config* file.</span></span>
  1. <span data-ttu-id="ff8d0-866">找出 `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` 元素。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-866">Locate the `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` element.</span></span>
  1. <span data-ttu-id="ff8d0-867">確認 `setProfileEnvironment` 屬性不存在 (其預設值為 `true`)，或明確地將屬性值設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-867">Confirm that the `setProfileEnvironment` attribute isn't present, which defaults the value to `true`, or explicitly set the attribute's value to `true`.</span></span>

* <span data-ttu-id="ff8d0-868">**將檔案系統當作 Keyring 存放區使用**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-868">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="ff8d0-869">調整應用程式程式碼，以[將檔案系統當作 Keyring 存放區使用](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-869">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="ff8d0-870">使用 X509 憑證來保護 Keyring，並確保憑證是受信任的憑證。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-870">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="ff8d0-871">如果憑證為自我簽署，請將憑證放在受信任的根存放區。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-871">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="ff8d0-872">在 Web 伺服陣列中使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-872">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="ff8d0-873">請使用所有電腦都可以存取的檔案共用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-873">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="ff8d0-874">將 X509 憑證部署到每一部電腦。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-874">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="ff8d0-875">設定[程式碼中的資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-875">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="ff8d0-876">**設定資料保護的全電腦原則**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-876">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="ff8d0-877">針對取用資料保護 API 的所有應用程式，資料保護系統僅支援有限的預設[全電腦原則](xref:security/data-protection/configuration/machine-wide-policy)設定。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-877">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="ff8d0-878">如需詳細資訊，請參閱 <xref:security/data-protection/introduction> 。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-878">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="virtual-directories"></a><span data-ttu-id="ff8d0-879">虛擬目錄</span><span class="sxs-lookup"><span data-stu-id="ff8d0-879">Virtual Directories</span></span>

<span data-ttu-id="ff8d0-880">ASP.NET Core 應用程式不支援 [IIS 虛擬目錄](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-880">[IIS Virtual Directories](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) aren't supported with ASP.NET Core apps.</span></span> <span data-ttu-id="ff8d0-881">應用程式能以[子應用程式](#sub-applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-881">An app can be hosted as a [sub-application](#sub-applications).</span></span>

## <a name="sub-applications"></a><span data-ttu-id="ff8d0-882">子應用程式</span><span class="sxs-lookup"><span data-stu-id="ff8d0-882">Sub-applications</span></span>

<span data-ttu-id="ff8d0-883">ASP.NET Core 應用程式能以 [IIS 子應用程式](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-883">An ASP.NET Core app can be hosted as an [IIS sub-application (sub-app)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications).</span></span> <span data-ttu-id="ff8d0-884">子應用程式的路徑會成為根應用程式 URL 的一部分。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-884">The sub-app's path becomes part of the root app's URL.</span></span>

<span data-ttu-id="ff8d0-885">子應用程式內的靜態資產連結應該使用波狀符號與斜線 (`~/`) 標記法。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-885">Static asset links within the sub-app should use tilde-slash (`~/`) notation.</span></span> <span data-ttu-id="ff8d0-886">波狀符號與斜線標記法會觸發[標記協助程式](xref:mvc/views/tag-helpers/intro)以將子應用程式的路徑基底附加到轉譯的相對連結前面。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-886">Tilde-slash notation triggers a [Tag Helper](xref:mvc/views/tag-helpers/intro) to prepend the sub-app's pathbase to the rendered relative link.</span></span> <span data-ttu-id="ff8d0-887">針對位於 `/subapp_path` 的子應用程式，使用 `src="~/image.png"` 連結的影像會轉譯為 `src="/subapp_path/image.png"`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-887">For a sub-app at `/subapp_path`, an image linked with `src="~/image.png"` is rendered as `src="/subapp_path/image.png"`.</span></span> <span data-ttu-id="ff8d0-888">根應用程式的靜態檔案中介軟體不會處理靜態檔案要求。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-888">The root app's Static File Middleware doesn't process the static file request.</span></span> <span data-ttu-id="ff8d0-889">要求會由子應用程式的靜態檔案中介軟體處理。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-889">The request is processed by the sub-app's Static File Middleware.</span></span>

<span data-ttu-id="ff8d0-890">若靜態資產的 `src` 屬性是設定為絕對路徑 (例如，`src="/image.png"`)，會以不使用子應用程式路徑基底的方式轉譯連結。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-890">If a static asset's `src` attribute is set to an absolute path (for example, `src="/image.png"`), the link is rendered without the sub-app's pathbase.</span></span> <span data-ttu-id="ff8d0-891">根應用程式的靜態檔案中介軟體會嘗試從根應用程式的 [webroot](xref:fundamentals/index#web-root) 提供資產，這會導致「404 - 找不到」\*\* 回應 (除非根應用程式可存取靜態資產)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-891">The root app's Static File Middleware attempts to serve the asset from the root app's [web root](xref:fundamentals/index#web-root), which results in a *404 - Not Found* response unless the static asset is available from the root app.</span></span>

<span data-ttu-id="ff8d0-892">裝載 ASP.NET Core 應用程式做為另一個 ASP.NET Core 應用程式下的子應用程式：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-892">To host an ASP.NET Core app as a sub-app under another ASP.NET Core app:</span></span>

1. <span data-ttu-id="ff8d0-893">為子應用程式建立應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-893">Establish an app pool for the sub-app.</span></span> <span data-ttu-id="ff8d0-894">將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\*，因為會將核心通用語言執行平台 (CoreCLR) 開機以在背景工作處理序中裝載應用程式，而非在桌面 CLR (.NET CLR) 中裝載。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-894">Set the **.NET CLR Version** to **No Managed Code** because the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process, not the desktop CLR (.NET CLR).</span></span>

1. <span data-ttu-id="ff8d0-895">使用根網站下資料夾中的子應用程式在 IIS 管理員中新增根網站。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-895">Add the root site in IIS Manager with the sub-app in a folder under the root site.</span></span>

1. <span data-ttu-id="ff8d0-896">以滑鼠右鍵按一下 IIS 管理員中的子應用程式資料夾，然後選取 [轉換成應用程式]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-896">Right-click the sub-app folder in IIS Manager and select **Convert to Application**.</span></span>

1. <span data-ttu-id="ff8d0-897">在 [新增應用程式]\*\*\*\* 對話方塊中，使用 [應用程式集區]\*\*\*\* 的[選取]\*\*\*\* 按鈕來指派您為子應用程式建立的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-897">In the **Add Application** dialog, use the **Select** button for the **Application Pool** to assign the app pool that you created for the sub-app.</span></span> <span data-ttu-id="ff8d0-898">選取 [確定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-898">Select **OK**.</span></span>

<span data-ttu-id="ff8d0-899">將不同的應用程式集區指派給子應用程式是使用同處理序裝載模型。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-899">The assignment of a separate app pool to the sub-app is a requirement when using the in-process hosting model.</span></span>

<span data-ttu-id="ff8d0-900">如需有關同進程裝載模型和設定 ASP.NET Core 模組的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module> 。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-900">For more information on the in-process hosting model and configuring the ASP.NET Core Module, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="ff8d0-901">使用 web.config 的 IIS 組態</span><span class="sxs-lookup"><span data-stu-id="ff8d0-901">Configuration of IIS with web.config</span></span>

<span data-ttu-id="ff8d0-902">在對使用了 ASP.NET Core 模組的 ASP.NET Core 有作用的 IIS 情境下，設定會受 *web.config* 的 `<system.webServer>` 區段影響。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-902">IIS configuration is influenced by the `<system.webServer>` section of *web.config* for IIS scenarios that are functional for ASP.NET Core apps with the ASP.NET Core Module.</span></span> <span data-ttu-id="ff8d0-903">舉例來說，IIS 設定對動態壓縮有作用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-903">For example, IIS configuration is functional for dynamic compression.</span></span> <span data-ttu-id="ff8d0-904">如果在伺服器層級將 IIS 設為使用動態壓縮，應用程式 *web.config* 檔案中的 `<urlCompression>` 元素則可為 ASP.NET Core 應用程式予以停用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-904">If IIS is configured at the server level to use dynamic compression, the `<urlCompression>` element in the app's *web.config* file can disable it for an ASP.NET Core app.</span></span>

<span data-ttu-id="ff8d0-905">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-905">For more information, see the following topics:</span></span>

* [<span data-ttu-id="ff8d0-906">的設定參考\<system.webServer></span><span class="sxs-lookup"><span data-stu-id="ff8d0-906">Configuration reference for \<system.webServer></span></span>](/iis/configuration/system.webServer/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>

<span data-ttu-id="ff8d0-907">若要設定在隔離的應用程式集區中執行之個別應用程式的環境變數（支援 IIS 10.0 或更新版本），請參閱 IIS 參考檔中[環境變數 \<environmentVariables> ](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe)主題的*AppCmd.exe 命令*一節。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-907">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="ff8d0-908">web.config 的組態區段</span><span class="sxs-lookup"><span data-stu-id="ff8d0-908">Configuration sections of web.config</span></span>

<span data-ttu-id="ff8d0-909">ASP.NET Core 應用程式的設定不使用 *web.config* 中 ASP.NET 4.x 應用程式的設定區段：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-909">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

<span data-ttu-id="ff8d0-910">使用其他組態提供者設定的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-910">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="ff8d0-911">如需詳細資訊，請參閱[Configuration](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-911">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="ff8d0-912">應用程式集區</span><span class="sxs-lookup"><span data-stu-id="ff8d0-912">Application Pools</span></span>

<span data-ttu-id="ff8d0-913">應用程式集區隔離取決於裝載模型：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-913">App pool isolation is determined by the hosting model:</span></span>

* <span data-ttu-id="ff8d0-914">同進程裝載：應用程式必須在不同的應用程式集區中執行。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-914">In-process hosting: Apps are required to run in separate app pools.</span></span>
* <span data-ttu-id="ff8d0-915">跨進程裝載：我們建議您在自己的應用程式集區中執行每個應用程式，以將應用程式彼此隔離。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-915">Out-of-process hosting: We recommend isolating the apps from each other by running each app in its own app pool.</span></span>

<span data-ttu-id="ff8d0-916">IIS [新增網站]\*\*\*\* 對話方塊預設每個應用程式皆為單一應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-916">The IIS **Add Website** dialog defaults to a single app pool per app.</span></span> <span data-ttu-id="ff8d0-917">當提供**網站名稱**時，文字會自動轉移至 [應用程式集區]\*\*\*\* 文字方塊。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-917">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="ff8d0-918">新增網站時，會使用該網站名稱建立新的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-918">A new app pool is created using the site name when the site is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="ff8d0-919">應用程式集區Identity</span><span class="sxs-lookup"><span data-stu-id="ff8d0-919">Application Pool Identity</span></span>

<span data-ttu-id="ff8d0-920">應用程式集區身分識別帳戶可讓應用程式在唯一的帳戶下執行，不必建立及管理網域或本機帳戶。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-920">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="ff8d0-921">在 IIS 8.0 或更新版本中，IIS 管理背景工作處理序 (WAS) 會使用新的應用程式集區名稱建立虛擬帳戶，並預設在此帳戶下執行應用程式集區的背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-921">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="ff8d0-922">在 IIS 管理主控台中，于應用程式集區的 [**高級設定**] 底下，確定 **Identity** 已設定為使用**ApplicationPoolIdentity**：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-922">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![應用程式集區進階設定對話方塊](index/_static/apppool-identity.png)

<span data-ttu-id="ff8d0-924">IIS 管理程序會在 Windows 安全系統中，以應用程式集區的名稱建立安全識別碼。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-924">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="ff8d0-925">可使用此身分識別來保護資源。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-925">Resources can be secured using this identity.</span></span> <span data-ttu-id="ff8d0-926">不過，此身分識別不是真正的使用者帳戶，也不會顯示在 Windows 使用者管理主控台中。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-926">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="ff8d0-927">如果 IIS 背景工作處理序需要提升應用程式的存取權限，請修改包含應用程式的目錄存取控制清單 (ACL)：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-927">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="ff8d0-928">開啟 Windows 檔案總管，巡覽至目錄。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-928">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="ff8d0-929">以滑鼠右鍵按一下目錄並選取 [屬性]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-929">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="ff8d0-930">依序選取 [安全性]\*\*\*\* 索引標籤下的 [編輯]\*\*\*\* 按鈕和 [新增]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-930">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="ff8d0-931">選取 [位置]\*\*\*\* 按鈕，並確定選取系統。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-931">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="ff8d0-932">在 [輸入要選取的物件名稱]\*\*\*\* 區域中，輸入 **IIS AppPool\\<app_pool_name>**。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-932">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="ff8d0-933">選取 [檢查名稱]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-933">Select the **Check Names** button.</span></span> <span data-ttu-id="ff8d0-934">針對 [DefaultAppPool]\*\*，請使用 **IIS AppPool\DefaultAppPool** 檢查名稱。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-934">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="ff8d0-935">選取 [檢查名稱]\*\*\*\* 按鈕時，[DefaultAppPool]\*\*\*\* 的值便會顯示於物件名稱區域中。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-935">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="ff8d0-936">您無法直接將應用程式集區名稱輸入至物件名稱區域。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-936">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="ff8d0-937">檢查物件名稱時，請使用 **IIS AppPool\\<app_pool_name>** 的格式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-937">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：在選取 [檢查名稱] 之前，"DefaultAppPool" 這個應用程式集區名稱在物件名稱區域中會附加至 "IIS AppPool\"。](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="ff8d0-939">選取 [確定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-939">Select **OK**.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：選取 [檢查名稱] 之後，物件名稱 "DefaultAppPool" 會顯示在物件名稱區域中。](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="ff8d0-941">預設應該會授與讀取 &amp; 執行權限。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-941">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="ff8d0-942">請視需要提供其他權限。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-942">Provide additional permissions as needed.</span></span>

<span data-ttu-id="ff8d0-943">也可使用 **ICACLS** 工具透過命令提示字元授與存取權限。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-943">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="ff8d0-944">使用 *DefaultAppPool*作為範例，將會使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-944">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="ff8d0-945">如需詳細資訊，請參閱 [icacls](/windows-server/administration/windows-commands/icacls) 主題。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-945">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="http2-support"></a><span data-ttu-id="ff8d0-946">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="ff8d0-946">HTTP/2 support</span></span>

<span data-ttu-id="ff8d0-947">在下列 IIS 部署案例中，ASP.NET Core 支援 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-947">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following IIS deployment scenarios:</span></span>

* <span data-ttu-id="ff8d0-948">內含式</span><span class="sxs-lookup"><span data-stu-id="ff8d0-948">In-process</span></span>
  * <span data-ttu-id="ff8d0-949">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="ff8d0-949">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="ff8d0-950">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="ff8d0-950">TLS 1.2 or later connection</span></span>
* <span data-ttu-id="ff8d0-951">跨處理序</span><span class="sxs-lookup"><span data-stu-id="ff8d0-951">Out-of-process</span></span>
  * <span data-ttu-id="ff8d0-952">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="ff8d0-952">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="ff8d0-953">公開 Edge Server 連線使用 HTTP/2，但是對 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的反向 Proxy 連線使用 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-953">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
  * <span data-ttu-id="ff8d0-954">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="ff8d0-954">TLS 1.2 or later connection</span></span>

<span data-ttu-id="ff8d0-955">針對建立 HTTP/2 連線時的同處理序部署，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會回報 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-955">For an in-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span> <span data-ttu-id="ff8d0-956">針對建立 HTTP/2 連線時的跨處理序部署，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會回報 `HTTP/1.1`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-956">For an out-of-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="ff8d0-957">如需有關同處理序和跨處理序主控模型的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-957">For more information on the in-process and out-of-process hosting models, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="ff8d0-958">HTTP/2 預設為啟用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-958">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="ff8d0-959">如果 HTTP/2 連線尚未建立，連線會退為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-959">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="ff8d0-960">如需使用 IIS 部署之 HTTP/2 設定的詳細資訊，請參閱 [IIS 上的 HTTP/2](/iis/get-started/whats-new-in-iis-10/http2-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-960">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="cors-preflight-requests"></a><span data-ttu-id="ff8d0-961">CORS 預檢要求</span><span class="sxs-lookup"><span data-stu-id="ff8d0-961">CORS preflight requests</span></span>

<span data-ttu-id="ff8d0-962">*此節只適用於以 .NET Framework 為目標的 ASP.NET Core 應用程式。*</span><span class="sxs-lookup"><span data-stu-id="ff8d0-962">*This section only applies to ASP.NET Core apps that target the .NET Framework.*</span></span>

<span data-ttu-id="ff8d0-963">針對以 .NET Framework 為目標的 ASP.NET Core 應用程式，在 IIS 中OPTIONS 要求預設不會傳遞到應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-963">For an ASP.NET Core app that targets the .NET Framework, OPTIONS requests aren't passed to the app by default in IIS.</span></span> <span data-ttu-id="ff8d0-964">若要瞭解如何在*web.config*中設定應用程式的 IIS 處理常式以傳遞選項要求，請參閱[在 ASP.NET Web API 2 中啟用跨原始來源要求： CORS 的運作方式](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-964">To learn how to configure the app's IIS handlers in *web.config* to pass OPTIONS requests, see [Enable cross-origin requests in ASP.NET Web API 2: How CORS Works](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).</span></span>

## <a name="application-initialization-module-and-idle-timeout"></a><span data-ttu-id="ff8d0-965">應用程式初始化模組與閒置逾時</span><span class="sxs-lookup"><span data-stu-id="ff8d0-965">Application Initialization Module and Idle Timeout</span></span>

<span data-ttu-id="ff8d0-966">在 IIS 中由 ASP.NET Core 模組版本 2 裝載時：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-966">When hosted in IIS by the ASP.NET Core Module version 2:</span></span>

* <span data-ttu-id="ff8d0-967">[應用程式初始化模組](#application-initialization-module)：應用程式裝載的同[進程](#in-process-hosting-model)或跨[進程](#out-of-process-hosting-model)可以設定為在背景工作進程重新開機或伺服器重新開機時自動啟動。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-967">[Application Initialization Module](#application-initialization-module): App's hosted [in-process](#in-process-hosting-model) or [out-of-process](#out-of-process-hosting-model) can be configured to start automatically on a worker process restart or server restart.</span></span>
* <span data-ttu-id="ff8d0-968">[閒置超時](#idle-timeout)：應用程式的裝載同[進程](#in-process-hosting-model)可以設定為不在非活動期間的時間超時。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-968">[Idle Timeout](#idle-timeout): App's hosted [in-process](#in-process-hosting-model) can be configured not to timeout during periods of inactivity.</span></span>

### <a name="application-initialization-module"></a><span data-ttu-id="ff8d0-969">應用程式初始化模組</span><span class="sxs-lookup"><span data-stu-id="ff8d0-969">Application Initialization Module</span></span>

<span data-ttu-id="ff8d0-970">*套用到應用程式裝載同處理序與非同處理序。*</span><span class="sxs-lookup"><span data-stu-id="ff8d0-970">*Applies to apps hosted in-process and out-of-process.*</span></span>

<span data-ttu-id="ff8d0-971">[IIS 應用程式初始化](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)是一項 IIS 功能，可在應用程式集區啟動或回收時，將 HTTP 要求傳送給應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-971">[IIS Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) is an IIS feature that sends an HTTP request to the app when the app pool starts or is recycled.</span></span> <span data-ttu-id="ff8d0-972">要求會觸發應用程式啟動。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-972">The request triggers the app to start.</span></span> <span data-ttu-id="ff8d0-973">根據預設，IIS 會發出應用程式根 URL (`/`) 的要求以初始化應用程式 (如需有關設定的詳細資訊，請參閱[額外資源](#application-initialization-module-and-idle-timeout-additional-resources))。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-973">By default, IIS issues a request to the app's root URL (`/`) to initialize the app (see the [additional resources](#application-initialization-module-and-idle-timeout-additional-resources) for more details on configuration).</span></span>

<span data-ttu-id="ff8d0-974">確認已啟用「IIS 應用程式初始化」角色功能：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-974">Confirm that the IIS Application Initialization role feature in enabled:</span></span>

<span data-ttu-id="ff8d0-975">在 Windows 7 或更新的電腦系統上，當在本機使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-975">On Windows 7 or later desktop systems when using IIS locally:</span></span>

1. <span data-ttu-id="ff8d0-976">瀏覽到 [控制台]\*\* [程式]\*\* > \*\* [程式和功能]\*\* > \*\*\*\* > **[開啟或關閉 Windows 功能]** \(畫面左側\)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-976">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="ff8d0-977">開啟 [網際網路資訊服務]\*\* [World Wide Web 服務]\*\* > \*\* [應用程式開發功能]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-977">Open **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="ff8d0-978">選取 [應用程式初始化]\*\*\*\* 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-978">Select the check box for **Application Initialization**.</span></span>

<span data-ttu-id="ff8d0-979">在 Windows Server 2008 R2 或更新版本上：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-979">On Windows Server 2008 R2 or later:</span></span>

1. <span data-ttu-id="ff8d0-980">開啟「新增角色與功能精靈」\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-980">Open the **Add Roles and Features Wizard**.</span></span>
1. <span data-ttu-id="ff8d0-981">在 [選取角色服務]\*\*\*\* 面板中，開啟 [應用程式開發]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-981">In the **Select role services** panel, open the **Application Development** node.</span></span>
1. <span data-ttu-id="ff8d0-982">選取 [應用程式初始化]\*\*\*\* 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-982">Select the check box for **Application Initialization**.</span></span>

<span data-ttu-id="ff8d0-983">使用下列任一方式為網站啟用應用程式初始化模組：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-983">Use either of the following approaches to enable the Application Initialization Module for the site:</span></span>

* <span data-ttu-id="ff8d0-984">使用 IIS 管理員：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-984">Using IIS Manager:</span></span>

  1. <span data-ttu-id="ff8d0-985">選取 [連線]\*\*\*\* 面板中的 [應用程式集區]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-985">Select **Application Pools** in the **Connections** panel.</span></span>
  1. <span data-ttu-id="ff8d0-986">以滑鼠右鍵按一下清單中應用程式的應用程式集區，然後選取 [進階設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-986">Right-click the app's app pool in the list and select **Advanced Settings**.</span></span>
  1. <span data-ttu-id="ff8d0-987">預設的 [啟動模式]\*\*\*\* 是 [OnDemand]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-987">The default **Start Mode** is **OnDemand**.</span></span> <span data-ttu-id="ff8d0-988">將 [啟動模式]\*\*\*\* 設定為 [AlwaysRunning]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-988">Set the **Start Mode** to **AlwaysRunning**.</span></span> <span data-ttu-id="ff8d0-989">選取 [確定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-989">Select **OK**.</span></span>
  1. <span data-ttu-id="ff8d0-990">開啟 [連線]\*\*\*\* 面板中的 [站台]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-990">Open the **Sites** node in the **Connections** panel.</span></span>
  1. <span data-ttu-id="ff8d0-991">以滑鼠右鍵按一下應用程式，然後選取 [管理網站]\*\* [進階設定]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-991">Right-click the app and select **Manage Website** > **Advanced Settings**.</span></span>
  1. <span data-ttu-id="ff8d0-992">預設 [預先載入已啟用]\*\*\*\* 設定是 [False]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-992">The default **Preload Enabled** setting is **False**.</span></span> <span data-ttu-id="ff8d0-993">將 [預先載入已啟用]\*\*\*\* 設定為 [True]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-993">Set **Preload Enabled** to **True**.</span></span> <span data-ttu-id="ff8d0-994">選取 [確定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-994">Select **OK**.</span></span>

* <span data-ttu-id="ff8d0-995">使用 *web.config*，新增 `<applicationInitialization>` 元素並將 `doAppInitAfterRestart` 設定為 `true` 至應用程式 *web.config* 檔案中的 `<system.webServer>` 元素：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-995">Using *web.config*, add the `<applicationInitialization>` element with `doAppInitAfterRestart` set to `true` to the `<system.webServer>` elements in the app's *web.config* file:</span></span>

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

### <a name="idle-timeout"></a><span data-ttu-id="ff8d0-996">閒置逾時</span><span class="sxs-lookup"><span data-stu-id="ff8d0-996">Idle Timeout</span></span>

<span data-ttu-id="ff8d0-997">*僅適用於應用程式裝載同處理序。*</span><span class="sxs-lookup"><span data-stu-id="ff8d0-997">*Only applies to apps hosted in-process.*</span></span>

<span data-ttu-id="ff8d0-998">若要防止應用程式閒置，請使用 IIS 管理員設定應用程式集區的閒置逾時：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-998">To prevent the app from idling, set the app pool's idle timeout using IIS Manager:</span></span>

1. <span data-ttu-id="ff8d0-999">選取 [連線]\*\*\*\* 面板中的 [應用程式集區]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-999">Select **Application Pools** in the **Connections** panel.</span></span>
1. <span data-ttu-id="ff8d0-1000">以滑鼠右鍵按一下清單中應用程式的應用程式集區，然後選取 [進階設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1000">Right-click the app's app pool in the list and select **Advanced Settings**.</span></span>
1. <span data-ttu-id="ff8d0-1001">預設 [閒置逾時 (分鐘)]\*\*\*\* 是 **20** 分鐘。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1001">The default **Idle Time-out (minutes)** is **20** minutes.</span></span> <span data-ttu-id="ff8d0-1002">將 [閒置逾時 (分鐘)]\*\*\*\* 設定為 **0** (零)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1002">Set the **Idle Time-out (minutes)** to **0** (zero).</span></span> <span data-ttu-id="ff8d0-1003">選取 [確定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1003">Select **OK**.</span></span>
1. <span data-ttu-id="ff8d0-1004">回收背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1004">Recycle the worker process.</span></span>

<span data-ttu-id="ff8d0-1005">若要防止應用程式裝載[非同處理序](#out-of-process-hosting-model)逾時，請使用下列任一方式：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1005">To prevent apps hosted [out-of-process](#out-of-process-hosting-model) from timing out, use either of the following approaches:</span></span>

* <span data-ttu-id="ff8d0-1006">從外部服務對應用程式執行 Ping 以讓它繼續執行。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1006">Ping the app from an external service in order to keep it running.</span></span>
* <span data-ttu-id="ff8d0-1007">若應用程式只裝載背景服務，避免 IIS 裝載並使用 [Windows 服務來裝載 ASP.NET Core 應用程式](xref:host-and-deploy/windows-service)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1007">If the app only hosts background services, avoid IIS hosting and use a [Windows Service to host the ASP.NET Core app](xref:host-and-deploy/windows-service).</span></span>

### <a name="application-initialization-module-and-idle-timeout-additional-resources"></a><span data-ttu-id="ff8d0-1008">應用程式初始化模組與閒置逾時額外資源</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1008">Application Initialization Module and Idle Timeout additional resources</span></span>

* [<span data-ttu-id="ff8d0-1009">IIS 8.0 應用程式初始化</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1009">IIS 8.0 Application Initialization</span></span>](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)
* <span data-ttu-id="ff8d0-1010">[應用程式 \<applicationInitialization> 初始化](/iis/configuration/system.webserver/applicationinitialization/)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1010">[Application Initialization \<applicationInitialization>](/iis/configuration/system.webserver/applicationinitialization/).</span></span>
* <span data-ttu-id="ff8d0-1011">[應用程式集 \<processModel> 區的進程模型設定](/iis/configuration/system.applicationhost/applicationpools/add/processmodel)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1011">[Process Model Settings for an Application Pool \<processModel>](/iis/configuration/system.applicationhost/applicationpools/add/processmodel).</span></span>

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="ff8d0-1012">IIS 系統管理員的部署資源</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1012">Deployment resources for IIS administrators</span></span>

* [<span data-ttu-id="ff8d0-1013">IIS 文件</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1013">IIS documentation</span></span>](/iis)
* [<span data-ttu-id="ff8d0-1014">IIS 中的 IIS 管理員使用者入門</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1014">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* [<span data-ttu-id="ff8d0-1015">.NET Core 應用程式部署</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1015">.NET Core application deployment</span></span>](/dotnet/core/deploying/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/directory-structure>
* <xref:host-and-deploy/iis/modules>
* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>

## <a name="additional-resources"></a><span data-ttu-id="ff8d0-1016">其他資源</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1016">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:index>
* [<span data-ttu-id="ff8d0-1017">Microsoft IIS 官方網站</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1017">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="ff8d0-1018">Windows Server 技術內容庫</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1018">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="ff8d0-1019">IIS 上的 HTTP/2</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1019">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
* <xref:host-and-deploy/iis/transform-webconfig>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ff8d0-1020">如需將 ASP.NET Core 應用程式發佈至 IIS 伺服器的教學課程體驗，請參閱<xref:tutorials/publish-to-iis>。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1020">For a tutorial experience on publishing an ASP.NET Core app to an IIS server, see <xref:tutorials/publish-to-iis>.</span></span>

[<span data-ttu-id="ff8d0-1021">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1021">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="ff8d0-1022">支援的作業系統</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1022">Supported operating systems</span></span>

<span data-ttu-id="ff8d0-1023">以下為支援的作業系統：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1023">The following operating systems are supported:</span></span>

* <span data-ttu-id="ff8d0-1024">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1024">Windows 7 or later</span></span>
* <span data-ttu-id="ff8d0-1025">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1025">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="ff8d0-1026">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys) (先前稱為 WebListener) 不適用搭配 IIS 的反向 Proxy 設定。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1026">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="ff8d0-1027">請使用 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1027">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="ff8d0-1028">如需在 Azure 中裝載的資訊，請參閱 <xref:host-and-deploy/azure-apps/index>。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1028">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

<span data-ttu-id="ff8d0-1029">如需疑難排解指引，請參閱 <xref:test/troubleshoot>。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1029">For troubleshooting guidance, see <xref:test/troubleshoot>.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="ff8d0-1030">支援的平台</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1030">Supported platforms</span></span>

<span data-ttu-id="ff8d0-1031">支援針對 32 位元 (x86) 或 64 位元 (x64) 部署發行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1031">Apps published for 32-bit (x86) or 64-bit (x64) deployment are supported.</span></span> <span data-ttu-id="ff8d0-1032">使用 32 位元 (x86) .NET Core SDK 部署 32 位元應用程式，除非應用程式：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1032">Deploy a 32-bit app with a 32-bit (x86) .NET Core SDK unless the app:</span></span>

* <span data-ttu-id="ff8d0-1033">需要提供給 64 位元應用程式使用的較大虛擬記憶體位址空間。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1033">Requires the larger virtual memory address space available to a 64-bit app.</span></span>
* <span data-ttu-id="ff8d0-1034">需要較大的 IIS 堆疊大小。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1034">Requires the larger IIS stack size.</span></span>
* <span data-ttu-id="ff8d0-1035">有 64 位元原生相依性。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1035">Has 64-bit native dependencies.</span></span>

<span data-ttu-id="ff8d0-1036">使用 64 位元 (x64) .NET Core SDK 來發行 64 位元應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1036">Use a 64-bit (x64) .NET Core SDK to publish a 64-bit app.</span></span> <span data-ttu-id="ff8d0-1037">主機系統上必須有 64 位元執行階段存在。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1037">A 64-bit runtime must be present on the host system.</span></span>

<span data-ttu-id="ff8d0-1038">ASP.NET Core 隨附 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)，其為預設的跨平台 HTTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1038">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), a default, cross-platform HTTP server.</span></span>

<span data-ttu-id="ff8d0-1039">在使用 [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) 或 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 時，應用程式會執行於從 IIS 背景工作處理序中分離出的處理序 (跨處理序\*\*)，並搭配 [Kestrel 伺服器](xref:fundamentals/servers/index#kestrel)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1039">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the app runs in a process separate from the IIS worker process (*out-of-process*) with the [Kestrel server](xref:fundamentals/servers/index#kestrel).</span></span>

<span data-ttu-id="ff8d0-1040">因為 ASP.NET Core 應用程式執行所在的處理序會與 IIS 工作者處理序分開，所以此模組會執行處理程序管理。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1040">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="ff8d0-1041">此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式關閉或損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1041">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="ff8d0-1042">此行為基本上與執行同處理序，並由 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1042">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="ff8d0-1043">下圖說明 IIS、ASP.NET Core 模組和跨處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1043">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![ASP.NET Core 模組](index/_static/ancm-outofprocess.png)

<span data-ttu-id="ff8d0-1045">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1045">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="ff8d0-1046">驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1046">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="ff8d0-1047">此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，而且不會是通訊埠 80 或 443。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1047">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="ff8d0-1048">此模組會在啟動時透過環境變數指定埠，而[IIS 整合中介軟體](xref:host-and-deploy/iis/index#enable-the-iisintegration-components)會設定伺服器來接聽 `http://localhost:{port}` 。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1048">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="ff8d0-1049">將會執行額外檢查，不是源自模組的要求都會遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1049">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="ff8d0-1050">此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1050">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="ff8d0-1051">Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1051">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="ff8d0-1052">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1052">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="ff8d0-1053">IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1053">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="ff8d0-1054">應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1054">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="ff8d0-1055">`CreateDefaultBuilder` 會將 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器設為網頁伺服器，並設定 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)的基底路徑與連接埠來啟用 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1055">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and enables IIS Integration by configuring the base path and port for the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="ff8d0-1056">ASP.NET Core 模組會產生要指派給後端處理序的動態連接埠。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1056">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="ff8d0-1057">`CreateDefaultBuilder` 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 方法。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1057">`CreateDefaultBuilder` calls the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> method.</span></span> <span data-ttu-id="ff8d0-1058">`UseIISIntegration` 會將 Kestrel 設定為在位於 localhost IP 位址 (`127.0.0.1`) 的動態連接埠上接聽。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1058">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="ff8d0-1059">若動態連接埠是 1234，Kestrel 會在 `127.0.0.1:1234` 接聽。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1059">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="ff8d0-1060">此設定會取代由下列項目提供的其他 URL 設定：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1060">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="ff8d0-1061">Kestrel 的接聽 API</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1061">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="ff8d0-1062">[設定](xref:fundamentals/configuration/index) (或[命令列 --urls 選項](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1062">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="ff8d0-1063">在使用模組時不需要呼叫 `UseUrls` 或 Kestrel 的 `Listen` API。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1063">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="ff8d0-1064">若呼叫 `UseUrls` 或 `Listen`，Kestrel 只會接聽未使用 IIS 執行應用程式時指定的連接埠。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1064">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

<span data-ttu-id="ff8d0-1065">如需 ASP.NET Core 模組組態指南，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1065">For ASP.NET Core Module configuration guidance, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="ff8d0-1066">如需代管的詳細資訊，請參閱[在 ASP.NET Core 中代管](xref:fundamentals/index#host)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1066">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/index#host).</span></span>

## <a name="application-configuration"></a><span data-ttu-id="ff8d0-1067">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1067">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="ff8d0-1068">啟用 IISIntegration 元件</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1068">Enable the IISIntegration components</span></span>

<span data-ttu-id="ff8d0-1069">在 `CreateWebHostBuilder` （*Program.cs*）中建立主機時，請呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 以啟用 IIS 整合：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1069">When building a host in `CreateWebHostBuilder` (*Program.cs*), call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to enable IIS integration:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

<span data-ttu-id="ff8d0-1070">如需 `CreateDefaultBuilder` 的詳細資訊，請參閱 <xref:fundamentals/host/web-host#set-up-a-host>。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1070">For more information on `CreateDefaultBuilder`, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

### <a name="iis-options"></a><span data-ttu-id="ff8d0-1071">IIS 選項</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1071">IIS options</span></span>

| <span data-ttu-id="ff8d0-1072">選項</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1072">Option</span></span>                         | <span data-ttu-id="ff8d0-1073">預設</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1073">Default</span></span> | <span data-ttu-id="ff8d0-1074">設定</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1074">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="ff8d0-1075">若為 `true`，IIS 伺服器會設定由 [Windows 驗證](xref:security/authentication/windowsauth)所驗證的 `HttpContext.User`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1075">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="ff8d0-1076">若為 `false`，則伺服器僅會對 `HttpContext.User` 提供身分識別，並在 `AuthenticationScheme` 明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1076">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="ff8d0-1077">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1077">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="ff8d0-1078">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1078">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="ff8d0-1079">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1079">Sets the display name shown to users on login pages.</span></span> |

<span data-ttu-id="ff8d0-1080">若要設定 IIS 選項，請在 <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 中加入 <xref:Microsoft.AspNetCore.Builder.IISOptions> 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1080">To configure IIS options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="ff8d0-1081">下列範例會防止應用程式填入 `HttpContext.Connection.ClientCertificate`：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1081">The following example prevents the app from populating `HttpContext.Connection.ClientCertificate`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="ff8d0-1082">選項</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1082">Option</span></span>                         | <span data-ttu-id="ff8d0-1083">預設</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1083">Default</span></span> | <span data-ttu-id="ff8d0-1084">設定</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1084">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="ff8d0-1085">若為 `true`，[IIS 整合中介軟體](#enable-the-iisintegration-components)會設定由 [Windows 驗證](xref:security/authentication/windowsauth)所驗證的 `HttpContext.User`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1085">If `true`, [IIS Integration Middleware](#enable-the-iisintegration-components) sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="ff8d0-1086">如果為 `false`，則驗證中介軟體僅針對 `HttpContext.User` 提供身分識別，並在游 `AuthenticationScheme` 提出明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1086">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="ff8d0-1087">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1087">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="ff8d0-1088">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1088">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="ff8d0-1089">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1089">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="ff8d0-1090">如果為 `true` 且 `MS-ASPNETCORE-CLIENTCERT` 要求標頭已存在，則會填入 `HttpContext.Connection.ClientCertificate`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1090">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="ff8d0-1091">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1091">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="ff8d0-1092">用來設定轉送標頭中介軟體及 ASP.NET Core 模組的 [IIS 整合中介軟體](#enable-the-iisintegration-components)會設定為轉送配置 (HTTP/HTTPS) 與發出要求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1092">The [IIS Integration Middleware](#enable-the-iisintegration-components), which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="ff8d0-1093">其他 Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1093">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="ff8d0-1094">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1094">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="ff8d0-1095">web.config 檔案</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1095">web.config file</span></span>

<span data-ttu-id="ff8d0-1096">*web.config* 檔案是用來設定 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1096">The *web.config* file configures the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="ff8d0-1097">發佈專案時，由 MSBuild 目標 (`_TransformWebConfig`) 處理 *web.config* 檔案的建立、轉換及發佈。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1097">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="ff8d0-1098">此目標存在於 Web SDK 目標 (`Microsoft.NET.Sdk.Web`)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1098">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="ff8d0-1099">SDK 設定在專案檔的頂端：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1099">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="ff8d0-1100">如果專案中沒有 *web.config* 檔案，則系統會使用正確的 *processPath* 和 *arguments* 建立該檔案以設定 ASP.NET Core 模組，並將該檔案移至[已發行的輸出](xref:host-and-deploy/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1100">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="ff8d0-1101">如果 *web.config* 檔案存在於專案中，則系統會使用正確的 *processPath* 和 *arguments* 來轉換該檔案以設定 ASP.NET Core 模組，然後將它移至已發行的輸出。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1101">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="ff8d0-1102">轉換不會修改檔案中的 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1102">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="ff8d0-1103">*web.config* 檔案可提供能控制作用中 IIS 模組的額外 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1103">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="ff8d0-1104">如需能處理 ASP.NET Core 應用程式要求之 IIS 模組的相關資訊，請參閱 [IIS 模組](xref:host-and-deploy/iis/modules)主題。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1104">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="ff8d0-1105">若要防止 Web SDK 轉換*web.config*檔案，請使用專案檔 **\<IsTransformWebConfigDisabled>** 中的屬性：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1105">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="ff8d0-1106">使 Web SDK 無法轉換檔案時，應該由開發人員手動設定 *processPath* 和 *arguments*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1106">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="ff8d0-1107">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module> 。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1107">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="ff8d0-1108">web.config 檔案位置</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1108">web.config file location</span></span>

<span data-ttu-id="ff8d0-1109">為了正確設定[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)， *web.config*檔案必須存在於已部署應用程式的[內容根](xref:fundamentals/index#content-root)路徑（通常是應用程式基底路徑）。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1109">In order to set up the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) correctly, the *web.config* file must be present at the [content root](xref:fundamentals/index#content-root) path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="ff8d0-1110">這是與提供給 IIS 的網站實體路徑相同的位置。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1110">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="ff8d0-1111">應用程式的根目錄需有 *web.config* 檔案，才能使用 Web Deploy 發行多個應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1111">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="ff8d0-1112">機密檔案存在於應用程式的實體路徑上，例如\* \<assembly>.runtimeconfig.json*、 \* \<assembly> .xml* （xml 檔批註）和\* \<assembly>.deps.js\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1112">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="ff8d0-1113">當 *web.config* 檔案存在且網站正常啟動時，如果有人要求機密檔案，IIS 不會予以提供。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1113">When the *web.config* file is present and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="ff8d0-1114">若 *web.config* 檔案遺失或沒有正確命名，或是無法設定網站以正常啟動，IIS 可能會公開提供機密檔案。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1114">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="ff8d0-1115">\***web.config*檔案必須隨時存在於部署中、正確命名，而且能夠將網站設定為正常啟動。絕對不要從生產環境部署移除*web.config\*檔案。**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1115">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

### <a name="transform-webconfig"></a><span data-ttu-id="ff8d0-1116">轉換 web.config</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1116">Transform web.config</span></span>

<span data-ttu-id="ff8d0-1117">如需在發佈時轉換 *web.config* (例如依據設定、設定檔或環境設定環境變數)，請參閱<xref:host-and-deploy/iis/transform-webconfig>。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1117">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="ff8d0-1118">IIS 組態</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1118">IIS configuration</span></span>

<span data-ttu-id="ff8d0-1119">**Windows Server 作業系統**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1119">**Windows Server operating systems**</span></span>

<span data-ttu-id="ff8d0-1120">啟用**網頁伺服器 (IIS)** 伺服器角色，並建立角色服務。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1120">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="ff8d0-1121">使用來自 [管理]\*\*\*\* 功能表的 [新增角色及功能]\*\*\*\* 精靈，或是 [伺服器管理員]\*\*\*\* 中的連結。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1121">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="ff8d0-1122">在**伺服器角色**步驟中，核取 [網頁伺服器 (IIS)]\*\*\*\* 方塊。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1122">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![在選取伺服器角色步驟中選取網頁伺服器 IIS 角色。](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="ff8d0-1124">在 [功能]\*\*\*\* 步驟之後，[角色服務]\*\*\*\* 步驟會針對網頁伺服器 (IIS) 進行載入。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1124">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="ff8d0-1125">選取所需的 IIS 角色服務或接受所提供的預設角色服務。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1125">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![在選取角色服務步驟中，選取預設的角色服務。](index/_static/role-services-ws2016.png)

   <span data-ttu-id="ff8d0-1127">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1127">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="ff8d0-1128">若要啟用 Windows 驗證，請展開下列節點： [**網頁伺服器**  >  **安全性**]。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1128">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="ff8d0-1129">選取 [Windows 驗證]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1129">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="ff8d0-1130">如需詳細資訊，請參閱[Windows 驗證 \<windowsAuthentication> ](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)和[設定 windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1130">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="ff8d0-1131">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1131">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="ff8d0-1132">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1132">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="ff8d0-1133">若要啟用 websocket，請展開下列節點： [**網頁伺服器**  >  **應用程式開發**]。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1133">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="ff8d0-1134">選取 [WebSocket 通訊協定]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1134">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="ff8d0-1135">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1135">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="ff8d0-1136">透過**確認**步驟繼續作業，安裝網頁伺服器角色和服務。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1136">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="ff8d0-1137">安裝**網頁伺服器（iis）** 角色之後，不需要重新開機伺服器/iis。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1137">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="ff8d0-1138">**Windows 桌面作業系統**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1138">**Windows desktop operating systems**</span></span>

<span data-ttu-id="ff8d0-1139">啟用 [IIS 管理主控台]\*\*\*\* 和 [World Wide Web 服務]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1139">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="ff8d0-1140">瀏覽到 [控制台]\*\* [程式]\*\* > \*\* [程式和功能]\*\* > \*\*\*\* > **[開啟或關閉 Windows 功能]** \(畫面左側\)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1140">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="ff8d0-1141">開啟 [Internet Information Services]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1141">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="ff8d0-1142">開啟 [Web 管理工具]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1142">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="ff8d0-1143">核取 [IIS 管理主控台]\*\*\*\* 方塊。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1143">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="ff8d0-1144">[World Wide Web Services] (全球資訊網服務)\*\*\*\* 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1144">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="ff8d0-1145">接受**全球資訊網服務**的預設功能，或自訂 IIS 功能。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1145">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="ff8d0-1146">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1146">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="ff8d0-1147">若要啟用 Windows 驗證，請展開下列節點： **World Wide Web 服務**  >  **安全性**。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1147">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="ff8d0-1148">選取 [Windows 驗證]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1148">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="ff8d0-1149">如需詳細資訊，請參閱[Windows 驗證 \<windowsAuthentication> ](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)和[設定 windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1149">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="ff8d0-1150">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1150">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="ff8d0-1151">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1151">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="ff8d0-1152">若要啟用 websocket，請展開下列節點： [ **World Wide Web 服務**] [  >  **應用程式開發] 功能**。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1152">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="ff8d0-1153">選取 [WebSocket 通訊協定]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1153">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="ff8d0-1154">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1154">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="ff8d0-1155">若 IIS 安裝需要重新啟動，請重新啟動系統。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1155">If the IIS installation requires a restart, restart the system.</span></span>

![選取 [Windows 功能] 中的 [IIS 管理主控台] 和 [World Wide Web Services] (全球資訊網服務)。](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="ff8d0-1157">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1157">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="ff8d0-1158">在主控系統上安裝 .NET Core 裝載套件組合\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1158">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="ff8d0-1159">套件組合會安裝 .NET Core 執行時間、.NET Core 程式庫和[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1159">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="ff8d0-1160">此模組可讓 ASP.NET Core 應用程式在 IIS 背後執行。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1160">The module allows ASP.NET Core apps to run behind IIS.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ff8d0-1161">若裝載套件組合在 IIS 之前安裝，則必須對該套件組合安裝進行修復。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1161">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="ff8d0-1162">請在安裝 IIS 之後，再次執行裝載套件組合安裝程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1162">Run the Hosting Bundle installer again after installing IIS.</span></span>
>
> <span data-ttu-id="ff8d0-1163">如果在安裝 64 位元 (x64) 版本的 .NET Core 後才安裝裝載套件組合，那麼可能會遺漏 SDK ([未偵測到 .NET Core SDK](xref:test/troubleshoot#no-net-core-sdks-were-detected))。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1163">If the Hosting Bundle is installed after installing the 64-bit (x64) version of .NET Core, SDKs might appear to be missing ([No .NET Core SDKs were detected](xref:test/troubleshoot#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="ff8d0-1164">若要解決此問題，請參閱 <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1164">To resolve the problem, see <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>.</span></span>

### <a name="download"></a><span data-ttu-id="ff8d0-1165">下載</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1165">Download</span></span>

1. <span data-ttu-id="ff8d0-1166">流覽至 [[下載 .Net Core](https://dotnet.microsoft.com/download/dotnet-core) ] 頁面。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1166">Navigate to the [Download .NET Core](https://dotnet.microsoft.com/download/dotnet-core) page.</span></span>
1. <span data-ttu-id="ff8d0-1167">選取所需的 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1167">Select the desired .NET Core version.</span></span>
1. <span data-ttu-id="ff8d0-1168">在 [執行應用程式 - 執行階段]\*\*\*\* 欄中，尋找想要的 .NET Core 執行階段版本列。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1168">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="ff8d0-1169">使用**裝載**套件組合連結來下載安裝程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1169">Download the installer using the **Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="ff8d0-1170">某些安裝程式包含已達到期生命週期結束 (EOL) 的發行版本，這些發行版本已不受 Microsoft 支援。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1170">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="ff8d0-1171">如需詳細資訊，請參閱[支援原則](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) \(英文 \)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1171">For more information, see the [support policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="ff8d0-1172">安裝裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1172">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="ff8d0-1173">在伺服器上執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1173">Run the installer on the server.</span></span> <span data-ttu-id="ff8d0-1174">從系統管理員命令殼層執行安裝程式時，有 下列參數可用：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1174">The following parameters are available when running the installer from an administrator command shell:</span></span>

   * <span data-ttu-id="ff8d0-1175">`OPT_NO_ANCM=1`：略過安裝 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1175">`OPT_NO_ANCM=1`: Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="ff8d0-1176">`OPT_NO_RUNTIME=1`：略過安裝 .NET Core 執行時間。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1176">`OPT_NO_RUNTIME=1`: Skip installing the .NET Core runtime.</span></span> <span data-ttu-id="ff8d0-1177">當伺服器只裝載[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)時使用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1177">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="ff8d0-1178">`OPT_NO_SHAREDFX=1`：略過安裝 ASP.NET 共用架構（ASP.NET 執行時間）。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1178">`OPT_NO_SHAREDFX=1`: Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span> <span data-ttu-id="ff8d0-1179">當伺服器只裝載[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)時使用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1179">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="ff8d0-1180">`OPT_NO_X86=1`：略過安裝 x86 執行時間。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1180">`OPT_NO_X86=1`: Skip installing x86 runtimes.</span></span> <span data-ttu-id="ff8d0-1181">當您確定不會裝載 32 位元應用程式時，請使用此參數。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1181">Use this parameter when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="ff8d0-1182">如果將來有可能同時裝載 32 位元和 64 位元應用程式，請不要使用此參數並安裝這兩個執行階段。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1182">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this parameter and install both runtimes.</span></span>
   * <span data-ttu-id="ff8d0-1183">`OPT_NO_SHARED_CONFIG_CHECK=1`：當共用設定（*applicationHost.config*）位於與 IIS 安裝相同的電腦上時，請停用使用 iis 共用設定的檢查。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1183">`OPT_NO_SHARED_CONFIG_CHECK=1`: Disable the check for using an IIS Shared Configuration when the shared configuration (*applicationHost.config*) is on the same machine as the IIS installation.</span></span> <span data-ttu-id="ff8d0-1184">*只在 ASP.NET Core 2.2 或更新版本的裝載套件組合安裝程式上可用。*</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1184">*Only available for ASP.NET Core 2.2 or later Hosting Bundler installers.*</span></span> <span data-ttu-id="ff8d0-1185">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration> 。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1185">For more information, see <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>.</span></span>
1. <span data-ttu-id="ff8d0-1186">重新開機系統，或在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1186">Restart the system or execute the following commands in a command shell:</span></span>

   ```console
   net stop was /y
   net start w3svc
   ```
   <span data-ttu-id="ff8d0-1187">重新啟動 IIS 將能偵測到由安裝程式對系統路徑 (此為環境變數) 所做出的變更。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1187">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

<span data-ttu-id="ff8d0-1188">安裝裝載套件組合時，不需要手動停止 IIS 中的個別網站。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1188">It isn't necessary to manually stop individual sites in IIS when installing the Hosting Bundle.</span></span> <span data-ttu-id="ff8d0-1189">裝載的應用程式（IIS 網站）會在 IIS 重新開機時重新開機。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1189">Hosted apps (IIS sites) restart when IIS restarts.</span></span> <span data-ttu-id="ff8d0-1190">應用程式會在收到第一個要求時重新開機，包括從[應用程式初始化模組](#application-initialization-module-and-idle-timeout)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1190">Apps start up again when they receive their first request, including from the [Application Initialization Module](#application-initialization-module-and-idle-timeout).</span></span>

<span data-ttu-id="ff8d0-1191">ASP.NET Core 採用共用架構封裝修補程式版本的向前復原行為。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1191">ASP.NET Core adopts roll-forward behavior for patch releases of shared framework packages.</span></span> <span data-ttu-id="ff8d0-1192">當 IIS 所裝載的應用程式使用 IIS 重新開機時，應用程式會在收到第一個要求時，以其所參考套件的最新修補程式版本來載入。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1192">When apps hosted by IIS restart with IIS, the apps load with the latest patch releases of their referenced packages when they receive their first request.</span></span> <span data-ttu-id="ff8d0-1193">如果未重新開機 IIS，應用程式會在其工作者進程回收時重新開機並展示向前復原行為，並接收其第一個要求。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1193">If IIS isn't restarted, apps restart and exhibit roll-forward behavior when their worker processes are recycled and they receive their first request.</span></span>

> [!NOTE]
> <span data-ttu-id="ff8d0-1194">如需 IIS 共用組態的資訊，請參閱[使用 IIS 共用組態的 ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1194">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="ff8d0-1195">使用 Visual Studio 發佈時安裝 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1195">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="ff8d0-1196">將應用程式部署到具有 [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later) 的伺服器時，請在伺服器上安裝最新版的 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1196">When deploying apps to servers with [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="ff8d0-1197">若要安裝 Web Deploy，請使用 [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) 或從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=43717)直接取得安裝程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1197">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="ff8d0-1198">慣用的方法是使用 WebPI。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1198">The preferred method is to use WebPI.</span></span> <span data-ttu-id="ff8d0-1199">WebPI 提供獨立的安裝程式和組態以裝載提供者。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1199">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="ff8d0-1200">建立 IIS 網站</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1200">Create the IIS site</span></span>

1. <span data-ttu-id="ff8d0-1201">在主控系統上，請建立資料夾以容納應用程式的已發行資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1201">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="ff8d0-1202">在下列步驟中，您提供資料夾路徑給 IIS，作為應用程式的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1202">In a following step, the folder's path is provided to IIS as the physical path to the app.</span></span> <span data-ttu-id="ff8d0-1203">如需應用程式之部署資料夾和檔案配置的詳細資訊，請參閱 <xref:host-and-deploy/directory-structure>。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1203">For more information on an app's deployment folder and file layout, see <xref:host-and-deploy/directory-structure>.</span></span>

1. <span data-ttu-id="ff8d0-1204">在 [IIS 管理員] 中 **，在 [** 連線] 面板中開啟伺服器的節點。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1204">In IIS Manager, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="ff8d0-1205">以滑鼠右鍵按一下 [網站]\*\*\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1205">Right-click the **Sites** folder.</span></span> <span data-ttu-id="ff8d0-1206">從操作功能表選取 [新增網站]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1206">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="ff8d0-1207">提供**網站名稱**，並將**實體路徑**設定為應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1207">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="ff8d0-1208">藉由**Binding**選取 **[確定]** 來提供系結設定並建立網站：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1208">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![在新增網站步驟中提供站台名稱、實體路徑和主機名稱。](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="ff8d0-1210">請**勿**使用最上層萬用字元繫結 (`http://*:80/`與 `http://+:80`)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1210">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="ff8d0-1211">最上層萬用字元繫結可能暴露您的應用程式安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1211">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="ff8d0-1212">這對強式與弱式萬用字元皆適用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1212">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="ff8d0-1213">請使用明確主機名稱，而非萬用字元。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1213">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="ff8d0-1214">若您擁有整個父網域 (與具弱點的 `*.com` 相對) 的控制權，則子網域萬用字元繫結 (例如 `*.mysub.com`) 就沒有此安全性風險。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1214">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="ff8d0-1215">如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1215">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="ff8d0-1216">在伺服器的節點之下，選取 [應用程式集區]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1216">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="ff8d0-1217">以滑鼠右鍵按一下網站的應用程式集區，然後從操作功能表選取 [基本設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1217">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="ff8d0-1218">在 [編輯應用程式集區]\*\*\*\* 視窗中，將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\*：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1218">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![將 .NET CLR 版本設為 [沒有受控碼]。](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="ff8d0-1220">ASP.NET Core 會在不同的處理序中執行，並管理執行階段。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1220">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="ff8d0-1221">ASP.NET Core 不仰賴載入桌面 CLR (.NET CLR)&mdash;會使用 .NET Core 的核心通用語言執行平台 (CoreCLR) 來開機以在背景工作處理序中裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1221">ASP.NET Core doesn't rely on loading the desktop CLR (.NET CLR)&mdash;the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process.</span></span> <span data-ttu-id="ff8d0-1222">將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\* 是選擇性的，但建議這樣做。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1222">Setting the **.NET CLR version** to **No Managed Code** is optional but recommended.</span></span>

1. <span data-ttu-id="ff8d0-1223">*ASP.NET Core 2.2 或更新版本*：對於使用[同處理序主控模型](#in-process-hosting-model)的 64 位元 (x64) [獨立式部署](/dotnet/core/deploying/#self-contained-deployments-scd)，會停用 32 位元 (x86) 處理序的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1223">*ASP.NET Core 2.2 or later*: For a 64-bit (x64) [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) that uses the [in-process hosting model](#in-process-hosting-model), disable the app pool for 32-bit (x86) processes.</span></span>

   <span data-ttu-id="ff8d0-1224">在 IIS 管理員的 [動作]\*\*\*\* 資訊看板 > [應用程式集區]\*\*\*\* 中，選取 [設定應用程式集區預設值]\*\*\*\* 或 [進階設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1224">In the **Actions** sidebar of IIS Manager > **Application Pools**, select **Set Application Pool Defaults** or **Advanced Settings**.</span></span> <span data-ttu-id="ff8d0-1225">找到 [啟用 32 位元應用程式]\*\*\*\*，然後將其值設定為 `False`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1225">Locate **Enable 32-Bit Applications** and set the value to `False`.</span></span> <span data-ttu-id="ff8d0-1226">此設定不會影響為[處理程序外裝載](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model)部署的應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1226">This setting doesn't affect apps deployed for [out-of-process hosting](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).</span></span>

1. <span data-ttu-id="ff8d0-1227">確認處理序模型身分識別具有適當的權限。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1227">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="ff8d0-1228">如果應用程式集區的預設識別（**進程模型**  >  **Identity** ）從**ApplicationPoolIdentity**變更為另一個身分識別，請確認新的身分識別具有存取應用程式資料夾、資料庫和其他必要資源的必要許可權。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1228">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="ff8d0-1229">例如，應用程式集區需要針對應用程式讀取和寫入檔案的資料夾取得讀取和寫入權限。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1229">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="ff8d0-1230">**Windows 驗證設定 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1230">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="ff8d0-1231">如需詳細資訊，請參閱[設定 Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1231">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="ff8d0-1232">部署應用程式</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1232">Deploy the app</span></span>

<span data-ttu-id="ff8d0-1233">將應用程式部署至在[建立 IIS 網站](#create-the-iis-site)一節中所建立的 IIS **實體路徑**資料夾。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1233">Deploy the app to the IIS **Physical path** folder that was established in the [Create the IIS site](#create-the-iis-site) section.</span></span> <span data-ttu-id="ff8d0-1234">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 是建議的部署機制，但有數個選項可將應用程式從專案的 [publish]\*\* 資料夾移動至主機系統的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1234">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment, but several options exist for moving the app from the project's *publish* folder to the hosting system's deployment folder.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="ff8d0-1235">使用 Visual Studio 的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1235">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="ff8d0-1236">請參閱[適用於 ASP.NET Core 應用程式部署的 Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)主題，了解如何建立搭配 Web Deploy 使用的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1236">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="ff8d0-1237">如果主機服務提供者提供或支援建立發行設定檔，請下載其設定檔，並使用 Visual Studio 的 [發行]\*\*\*\* 對話方塊匯入其設定檔：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1237">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog:</span></span>

![[發佈] 對話方塊頁](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="ff8d0-1239">Visual Studio 外部的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1239">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="ff8d0-1240">透過命令列也可以在 Visual Studio 外部使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1240">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="ff8d0-1241">如需詳細資訊，請參閱 [Web 部署工具](/iis/publish/using-web-deploy/use-the-web-deployment-tool)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1241">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="ff8d0-1242">Web Deploy 的替代項目</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1242">Alternatives to Web Deploy</span></span>

<span data-ttu-id="ff8d0-1243">使用數種方法的其中一種將應用程式移到主機系統，例如手動複製、[Xcopy](/windows-server/administration/windows-commands/xcopy)、[Robocopy](/windows-server/administration/windows-commands/robocopy) 或 [PowerShell](/powershell/)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1243">Use any of several methods to move the app to the hosting system, such as manual copy, [Xcopy](/windows-server/administration/windows-commands/xcopy), [Robocopy](/windows-server/administration/windows-commands/robocopy), or [PowerShell](/powershell/).</span></span>

<span data-ttu-id="ff8d0-1244">如需 IIS 的 ASP.NET Core 部署詳細資訊，請參閱 [IIS 系統管理員的部署資源](#deployment-resources-for-iis-administrators)一節。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1244">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="ff8d0-1245">瀏覽網站</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1245">Browse the website</span></span>

<span data-ttu-id="ff8d0-1246">應用程式部署到主機系統之後，請向其中一個應用程式的公用端點發出要求。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1246">After the app is deployed to the hosting system, make a request to one of the app's public endpoints.</span></span>

<span data-ttu-id="ff8d0-1247">在下列範例中，網站繫結至 IIS 上的**連接埠** `80`，其**主機名稱**為 `www.mysite.com`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1247">In the following example, the site is bound to an IIS **Host name** of `www.mysite.com` on **Port** `80`.</span></span> <span data-ttu-id="ff8d0-1248">對 `http://www.mysite.com` 發出要求：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1248">A request is made to `http://www.mysite.com`:</span></span>

![Microsoft Edge 瀏覽器已載入 IIS 啟動頁面。](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="ff8d0-1250">已鎖定的部署檔案</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1250">Locked deployment files</span></span>

<span data-ttu-id="ff8d0-1251">當應用程式執行時，會鎖定部署資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1251">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="ff8d0-1252">無法於部署期間覆寫已鎖定的檔案。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1252">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="ff8d0-1253">若要釋放部署中的已鎖定檔案，請使用下列其中**一種**方法停止應用程式集區：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1253">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="ff8d0-1254">使用 Web Deploy 並參考專案檔中的 `Microsoft.NET.Sdk.Web`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1254">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="ff8d0-1255">*app_offline.htm* 檔案是放在 Web 應用程式目錄的根目錄中。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1255">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="ff8d0-1256">當檔案存在時，ASP.NET Core 模組會正常關閉應用程式，並在部署期間提供 *app_offline.htm* 檔案。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1256">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="ff8d0-1257">如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1257">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="ff8d0-1258">在伺服器上的 IIS 管理員中手動停止應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1258">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="ff8d0-1259">使用 PowerShell 卸載*app_offline.htm* （需要 PowerShell 5 或更新版本）：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1259">Use PowerShell to drop *app_offline.htm* (requires PowerShell 5 or later):</span></span>

  ```powershell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a><span data-ttu-id="ff8d0-1260">資料保護</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1260">Data protection</span></span>

<span data-ttu-id="ff8d0-1261">[ASP.NET Core 資料保護堆疊](xref:security/data-protection/introduction)是由數個 ASP.NET Core [中介軟體](xref:fundamentals/middleware/index)所使用，包括用於驗證的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1261">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="ff8d0-1262">即使資料保護 API 不是由使用者程式碼呼叫，仍應使用部署指令碼或是在使用者程式碼中設定資料保護，以建立持續性的密碼編譯[金鑰存放區](xref:security/data-protection/implementation/key-management)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1262">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="ff8d0-1263">如不設定資料保護，金鑰會保留在記憶體中，並於應用程式重新啟動時捨棄。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1263">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="ff8d0-1264">如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1264">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="ff8d0-1265">所有以 Cookie 為基礎的驗證權杖都會失效。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1265">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="ff8d0-1266">當使用者提出下一個要求時，需要再次登入。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1266">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="ff8d0-1267">所有以 Keyring 保護的資料都無法再解密。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1267">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="ff8d0-1268">這可能會包含 [CSRF 權杖](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)和 [ASP.NET Core MVC TempData cookie](xref:fundamentals/app-state#tempdata)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1268">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="ff8d0-1269">若要在 IIS 下設定資料保護以保存 Keyring，請使用下列其中**一種**方法：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1269">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="ff8d0-1270">**建立資料保護登錄機碼**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1270">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="ff8d0-1271">ASP.NET Core 應用程式所使用的資料保護金鑰會儲存在應用程式外部的登錄中。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1271">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="ff8d0-1272">若要保存指定應用程式的金鑰，請為應用程式集區建立登錄機碼。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1272">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="ff8d0-1273">若為獨立的非Web 伺服陣列 IIS 安裝，請針對搭配使用 ASP.NET Core 應用程式的每個應用程式集區，使用[資料保護 Provision-AutoGenKeys.ps1 PowerShell 指令碼](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1273">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="ff8d0-1274">此指令碼會在 HKLM 登錄中建立登錄機碼，只有應用程式之應用程式集區的背景工作處理序帳戶可以存取它。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1274">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="ff8d0-1275">在待用期間使用 DPAPI 和全電腦金鑰加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1275">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="ff8d0-1276">在 Web 伺服陣列案例中，應用程式可以設定成使用 UNC 路徑來儲存其資料保護 Keyring。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1276">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="ff8d0-1277">根據預設，資料保護金鑰不予加密。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1277">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="ff8d0-1278">請確保網路共用的檔案權限僅限於執行應用程式的 Windows 帳戶。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1278">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="ff8d0-1279">可以使用 X509 憑證來保護待用的金鑰。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1279">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="ff8d0-1280">請考慮下列讓使用者上傳憑證的機制：將憑證放入使用者的受信任憑證存放區，並確保在執行使用者應用程式的所有電腦上都能使用這些憑證。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1280">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="ff8d0-1281">如需詳細資訊，請參閱[設定 ASP.NET Core 資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1281">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="ff8d0-1282">**設定 IIS 應用程式集區載入使用者設定檔**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1282">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="ff8d0-1283">此設定位在應用程式集區 [進階設定]\*\*\*\* 下的 [處理序模型]\*\*\*\* 區段中。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1283">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="ff8d0-1284">將 [載入使用者設定檔]\*\*\*\* 設為 `True`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1284">Set **Load User Profile** to `True`.</span></span> <span data-ttu-id="ff8d0-1285">當設定為 `True` 時，金鑰會儲存在使用者設定檔目錄中，且使用具有使用者帳戶專屬金鑰的 DPAPI 保護。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1285">When set to `True`, keys are stored in the user profile directory and protected using DPAPI with a key specific to the user account.</span></span> <span data-ttu-id="ff8d0-1286">金鑰會保存到 *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1286">Keys are persisted to the *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* folder.</span></span>

  <span data-ttu-id="ff8d0-1287">應用程式集區的 [setProfileEnvironment 屬性](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration)也必須啟用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1287">The app pool's [setProfileEnvironment attribute](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) must also be enabled.</span></span> <span data-ttu-id="ff8d0-1288">`setProfileEnvironment` 的預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1288">The default value of `setProfileEnvironment` is `true`.</span></span> <span data-ttu-id="ff8d0-1289">在某些情況下 (例如 Windows OS)，`setProfileEnvironment` 會設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1289">In some scenarios (for example, Windows OS), `setProfileEnvironment` is set to `false`.</span></span> <span data-ttu-id="ff8d0-1290">如果金鑰並未如預期地儲存在使用者設定檔目錄中：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1290">If keys aren't stored in the user profile directory as expected:</span></span>

  1. <span data-ttu-id="ff8d0-1291">瀏覽至 *%windir%/system32/inetsrv/config* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1291">Navigate to the *%windir%/system32/inetsrv/config* folder.</span></span>
  1. <span data-ttu-id="ff8d0-1292">開啟 *applicationHost.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1292">Open the *applicationHost.config* file.</span></span>
  1. <span data-ttu-id="ff8d0-1293">找出 `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` 元素。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1293">Locate the `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` element.</span></span>
  1. <span data-ttu-id="ff8d0-1294">確認 `setProfileEnvironment` 屬性不存在 (其預設值為 `true`)，或明確地將屬性值設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1294">Confirm that the `setProfileEnvironment` attribute isn't present, which defaults the value to `true`, or explicitly set the attribute's value to `true`.</span></span>

* <span data-ttu-id="ff8d0-1295">**將檔案系統當作 Keyring 存放區使用**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1295">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="ff8d0-1296">調整應用程式程式碼，以[將檔案系統當作 Keyring 存放區使用](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1296">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="ff8d0-1297">使用 X509 憑證來保護 Keyring，並確保憑證是受信任的憑證。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1297">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="ff8d0-1298">如果憑證為自我簽署，請將憑證放在受信任的根存放區。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1298">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="ff8d0-1299">在 Web 伺服陣列中使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1299">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="ff8d0-1300">請使用所有電腦都可以存取的檔案共用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1300">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="ff8d0-1301">將 X509 憑證部署到每一部電腦。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1301">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="ff8d0-1302">設定[程式碼中的資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1302">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="ff8d0-1303">**設定資料保護的全電腦原則**</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1303">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="ff8d0-1304">針對取用資料保護 API 的所有應用程式，資料保護系統僅支援有限的預設[全電腦原則](xref:security/data-protection/configuration/machine-wide-policy)設定。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1304">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="ff8d0-1305">如需詳細資訊，請參閱 <xref:security/data-protection/introduction> 。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1305">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="virtual-directories"></a><span data-ttu-id="ff8d0-1306">虛擬目錄</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1306">Virtual Directories</span></span>

<span data-ttu-id="ff8d0-1307">ASP.NET Core 應用程式不支援 [IIS 虛擬目錄](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1307">[IIS Virtual Directories](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) aren't supported with ASP.NET Core apps.</span></span> <span data-ttu-id="ff8d0-1308">應用程式能以[子應用程式](#sub-applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1308">An app can be hosted as a [sub-application](#sub-applications).</span></span>

## <a name="sub-applications"></a><span data-ttu-id="ff8d0-1309">子應用程式</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1309">Sub-applications</span></span>

<span data-ttu-id="ff8d0-1310">ASP.NET Core 應用程式能以 [IIS 子應用程式](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1310">An ASP.NET Core app can be hosted as an [IIS sub-application (sub-app)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications).</span></span> <span data-ttu-id="ff8d0-1311">子應用程式的路徑會成為根應用程式 URL 的一部分。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1311">The sub-app's path becomes part of the root app's URL.</span></span>

<span data-ttu-id="ff8d0-1312">子應用程式不應該包括 ASP.NET Core 模組做為處理常式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1312">A sub-app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="ff8d0-1313">如果您在子應用程式的 *web.config* 檔案中將模組新增為處理常式，則嘗試瀏覽子應用程式時，會收到參考錯誤設定檔的 *500.19 內部伺服器錯誤*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1313">If the module is added as a handler in a sub-app's *web.config* file, a *500.19 Internal Server Error* referencing the faulty config file is received when attempting to browse the sub-app.</span></span>

<span data-ttu-id="ff8d0-1314">下列範例顯示 ASP.NET Core 子應用程式的已發佈 *web.config* 檔案：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1314">The following example shows a published *web.config* file for an ASP.NET Core sub-app:</span></span>

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

<span data-ttu-id="ff8d0-1315">在 ASP.NET Core 應用程式下裝載非 ASP.NET Core 子應用程式時，請明確移除子應用程式 *web.config* 檔案中已繼承的處理常式：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1315">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app's *web.config* file:</span></span>

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

<span data-ttu-id="ff8d0-1316">子應用程式內的靜態資產連結應該使用波狀符號與斜線 (`~/`) 標記法。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1316">Static asset links within the sub-app should use tilde-slash (`~/`) notation.</span></span> <span data-ttu-id="ff8d0-1317">波狀符號與斜線標記法會觸發[標記協助程式](xref:mvc/views/tag-helpers/intro)以將子應用程式的路徑基底附加到轉譯的相對連結前面。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1317">Tilde-slash notation triggers a [Tag Helper](xref:mvc/views/tag-helpers/intro) to prepend the sub-app's pathbase to the rendered relative link.</span></span> <span data-ttu-id="ff8d0-1318">針對位於 `/subapp_path` 的子應用程式，使用 `src="~/image.png"` 連結的影像會轉譯為 `src="/subapp_path/image.png"`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1318">For a sub-app at `/subapp_path`, an image linked with `src="~/image.png"` is rendered as `src="/subapp_path/image.png"`.</span></span> <span data-ttu-id="ff8d0-1319">根應用程式的靜態檔案中介軟體不會處理靜態檔案要求。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1319">The root app's Static File Middleware doesn't process the static file request.</span></span> <span data-ttu-id="ff8d0-1320">要求會由子應用程式的靜態檔案中介軟體處理。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1320">The request is processed by the sub-app's Static File Middleware.</span></span>

<span data-ttu-id="ff8d0-1321">若靜態資產的 `src` 屬性是設定為絕對路徑 (例如，`src="/image.png"`)，會以不使用子應用程式路徑基底的方式轉譯連結。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1321">If a static asset's `src` attribute is set to an absolute path (for example, `src="/image.png"`), the link is rendered without the sub-app's pathbase.</span></span> <span data-ttu-id="ff8d0-1322">根應用程式的靜態檔案中介軟體會嘗試從根應用程式的 [webroot](xref:fundamentals/index#web-root) 提供資產，這會導致「404 - 找不到」\*\* 回應 (除非根應用程式可存取靜態資產)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1322">The root app's Static File Middleware attempts to serve the asset from the root app's [web root](xref:fundamentals/index#web-root), which results in a *404 - Not Found* response unless the static asset is available from the root app.</span></span>

<span data-ttu-id="ff8d0-1323">裝載 ASP.NET Core 應用程式做為另一個 ASP.NET Core 應用程式下的子應用程式：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1323">To host an ASP.NET Core app as a sub-app under another ASP.NET Core app:</span></span>

1. <span data-ttu-id="ff8d0-1324">為子應用程式建立應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1324">Establish an app pool for the sub-app.</span></span> <span data-ttu-id="ff8d0-1325">將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\*，因為會將核心通用語言執行平台 (CoreCLR) 開機以在背景工作處理序中裝載應用程式，而非在桌面 CLR (.NET CLR) 中裝載。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1325">Set the **.NET CLR Version** to **No Managed Code** because the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process, not the desktop CLR (.NET CLR).</span></span>

1. <span data-ttu-id="ff8d0-1326">使用根網站下資料夾中的子應用程式在 IIS 管理員中新增根網站。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1326">Add the root site in IIS Manager with the sub-app in a folder under the root site.</span></span>

1. <span data-ttu-id="ff8d0-1327">以滑鼠右鍵按一下 IIS 管理員中的子應用程式資料夾，然後選取 [轉換成應用程式]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1327">Right-click the sub-app folder in IIS Manager and select **Convert to Application**.</span></span>

1. <span data-ttu-id="ff8d0-1328">在 [新增應用程式]\*\*\*\* 對話方塊中，使用 [應用程式集區]\*\*\*\* 的[選取]\*\*\*\* 按鈕來指派您為子應用程式建立的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1328">In the **Add Application** dialog, use the **Select** button for the **Application Pool** to assign the app pool that you created for the sub-app.</span></span> <span data-ttu-id="ff8d0-1329">選取 [確定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1329">Select **OK**.</span></span>

<span data-ttu-id="ff8d0-1330">將不同的應用程式集區指派給子應用程式是使用同處理序裝載模型。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1330">The assignment of a separate app pool to the sub-app is a requirement when using the in-process hosting model.</span></span>

<span data-ttu-id="ff8d0-1331">如需有關同進程裝載模型和設定 ASP.NET Core 模組的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module> 。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1331">For more information on the in-process hosting model and configuring the ASP.NET Core Module, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="ff8d0-1332">使用 web.config 的 IIS 組態</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1332">Configuration of IIS with web.config</span></span>

<span data-ttu-id="ff8d0-1333">在對使用了 ASP.NET Core 模組的 ASP.NET Core 有作用的 IIS 情境下，設定會受 *web.config* 的 `<system.webServer>` 區段影響。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1333">IIS configuration is influenced by the `<system.webServer>` section of *web.config* for IIS scenarios that are functional for ASP.NET Core apps with the ASP.NET Core Module.</span></span> <span data-ttu-id="ff8d0-1334">舉例來說，IIS 設定對動態壓縮有作用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1334">For example, IIS configuration is functional for dynamic compression.</span></span> <span data-ttu-id="ff8d0-1335">如果在伺服器層級將 IIS 設為使用動態壓縮，應用程式 *web.config* 檔案中的 `<urlCompression>` 元素則可為 ASP.NET Core 應用程式予以停用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1335">If IIS is configured at the server level to use dynamic compression, the `<urlCompression>` element in the app's *web.config* file can disable it for an ASP.NET Core app.</span></span>

<span data-ttu-id="ff8d0-1336">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1336">For more information, see the following topics:</span></span>

* [<span data-ttu-id="ff8d0-1337">的設定參考\<system.webServer></span><span class="sxs-lookup"><span data-stu-id="ff8d0-1337">Configuration reference for \<system.webServer></span></span>](/iis/configuration/system.webServer/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>

<span data-ttu-id="ff8d0-1338">若要設定在隔離的應用程式集區中執行之個別應用程式的環境變數（支援 IIS 10.0 或更新版本），請參閱 IIS 參考檔中[環境變數 \<environmentVariables> ](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe)主題的*AppCmd.exe 命令*一節。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1338">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="ff8d0-1339">web.config 的組態區段</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1339">Configuration sections of web.config</span></span>

<span data-ttu-id="ff8d0-1340">ASP.NET Core 應用程式的設定不使用 *web.config* 中 ASP.NET 4.x 應用程式的設定區段：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1340">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

<span data-ttu-id="ff8d0-1341">使用其他組態提供者設定的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1341">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="ff8d0-1342">如需詳細資訊，請參閱[Configuration](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1342">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="ff8d0-1343">應用程式集區</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1343">Application Pools</span></span>

<span data-ttu-id="ff8d0-1344">在伺服器上裝載多個網站時，建議您在其各自的應用程式集區中執行各個應用程式，讓應用程式彼此隔離。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1344">When hosting multiple websites on a server, we recommend isolating the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="ff8d0-1345">IIS [新增網站]\*\*\*\* 對話方塊預設成此組態。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1345">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="ff8d0-1346">當提供**網站名稱**時，文字會自動轉移至 [應用程式集區]\*\*\*\* 文字方塊。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1346">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="ff8d0-1347">新增網站時，會使用該網站名稱建立新的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1347">A new app pool is created using the site name when the site is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="ff8d0-1348">應用程式集區Identity</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1348">Application Pool Identity</span></span>

<span data-ttu-id="ff8d0-1349">應用程式集區身分識別帳戶可讓應用程式在唯一的帳戶下執行，不必建立及管理網域或本機帳戶。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1349">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="ff8d0-1350">在 IIS 8.0 或更新版本中，IIS 管理背景工作處理序 (WAS) 會使用新的應用程式集區名稱建立虛擬帳戶，並預設在此帳戶下執行應用程式集區的背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1350">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="ff8d0-1351">在 IIS 管理主控台中，于應用程式集區的 [**高級設定**] 底下，確定 **Identity** 已設定為使用**ApplicationPoolIdentity**：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1351">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![應用程式集區進階設定對話方塊](index/_static/apppool-identity.png)

<span data-ttu-id="ff8d0-1353">IIS 管理程序會在 Windows 安全系統中，以應用程式集區的名稱建立安全識別碼。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1353">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="ff8d0-1354">可使用此身分識別來保護資源。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1354">Resources can be secured using this identity.</span></span> <span data-ttu-id="ff8d0-1355">不過，此身分識別不是真正的使用者帳戶，也不會顯示在 Windows 使用者管理主控台中。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1355">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="ff8d0-1356">如果 IIS 背景工作處理序需要提升應用程式的存取權限，請修改包含應用程式的目錄存取控制清單 (ACL)：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1356">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="ff8d0-1357">開啟 Windows 檔案總管，巡覽至目錄。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1357">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="ff8d0-1358">以滑鼠右鍵按一下目錄並選取 [屬性]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1358">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="ff8d0-1359">依序選取 [安全性]\*\*\*\* 索引標籤下的 [編輯]\*\*\*\* 按鈕和 [新增]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1359">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="ff8d0-1360">選取 [位置]\*\*\*\* 按鈕，並確定選取系統。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1360">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="ff8d0-1361">在 [輸入要選取的物件名稱]\*\*\*\* 區域中，輸入 **IIS AppPool\\<app_pool_name>**。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1361">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="ff8d0-1362">選取 [檢查名稱]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1362">Select the **Check Names** button.</span></span> <span data-ttu-id="ff8d0-1363">針對 [DefaultAppPool]\*\*，請使用 **IIS AppPool\DefaultAppPool** 檢查名稱。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1363">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="ff8d0-1364">選取 [檢查名稱]\*\*\*\* 按鈕時，[DefaultAppPool]\*\*\*\* 的值便會顯示於物件名稱區域中。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1364">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="ff8d0-1365">您無法直接將應用程式集區名稱輸入至物件名稱區域。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1365">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="ff8d0-1366">檢查物件名稱時，請使用 **IIS AppPool\\<app_pool_name>** 的格式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1366">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：在選取 [檢查名稱] 之前，"DefaultAppPool" 這個應用程式集區名稱在物件名稱區域中會附加至 "IIS AppPool\"。](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="ff8d0-1368">選取 [確定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1368">Select **OK**.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：選取 [檢查名稱] 之後，物件名稱 "DefaultAppPool" 會顯示在物件名稱區域中。](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="ff8d0-1370">預設應該會授與讀取 &amp; 執行權限。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1370">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="ff8d0-1371">請視需要提供其他權限。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1371">Provide additional permissions as needed.</span></span>

<span data-ttu-id="ff8d0-1372">也可使用 **ICACLS** 工具透過命令提示字元授與存取權限。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1372">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="ff8d0-1373">使用 *DefaultAppPool*作為範例，將會使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1373">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="ff8d0-1374">如需詳細資訊，請參閱 [icacls](/windows-server/administration/windows-commands/icacls) 主題。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1374">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="http2-support"></a><span data-ttu-id="ff8d0-1375">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1375">HTTP/2 support</span></span>

<span data-ttu-id="ff8d0-1376">[HTTP/2](https://httpwg.org/specs/rfc7540.html) 支援符合下列基本需求的跨處理序部署：</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1376">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported for out-of-process deployments that meet the following base requirements:</span></span>

* <span data-ttu-id="ff8d0-1377">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1377">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
* <span data-ttu-id="ff8d0-1378">公開 Edge Server 連線使用 HTTP/2，但是對 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的反向 Proxy 連線使用 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1378">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
* <span data-ttu-id="ff8d0-1379">目標 Framework：不適用於跨處理序部署，因為 HTTP/2 連線完全由 IIS 處理。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1379">Target framework: Not applicable to out-of-process deployments, since the HTTP/2 connection is handled entirely by IIS.</span></span>
* <span data-ttu-id="ff8d0-1380">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1380">TLS 1.2 or later connection</span></span>

<span data-ttu-id="ff8d0-1381">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/1.1`。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1381">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="ff8d0-1382">HTTP/2 預設為啟用。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1382">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="ff8d0-1383">如果 HTTP/2 連線尚未建立，連線會退為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1383">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="ff8d0-1384">如需使用 IIS 部署之 HTTP/2 設定的詳細資訊，請參閱 [IIS 上的 HTTP/2](/iis/get-started/whats-new-in-iis-10/http2-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1384">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="cors-preflight-requests"></a><span data-ttu-id="ff8d0-1385">CORS 預檢要求</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1385">CORS preflight requests</span></span>

<span data-ttu-id="ff8d0-1386">*此節只適用於以 .NET Framework 為目標的 ASP.NET Core 應用程式。*</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1386">*This section only applies to ASP.NET Core apps that target the .NET Framework.*</span></span>

<span data-ttu-id="ff8d0-1387">針對以 .NET Framework 為目標的 ASP.NET Core 應用程式，在 IIS 中OPTIONS 要求預設不會傳遞到應用程式。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1387">For an ASP.NET Core app that targets the .NET Framework, OPTIONS requests aren't passed to the app by default in IIS.</span></span> <span data-ttu-id="ff8d0-1388">若要瞭解如何在*web.config*中設定應用程式的 IIS 處理常式以傳遞選項要求，請參閱[在 ASP.NET Web API 2 中啟用跨原始來源要求： CORS 的運作方式](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works)。</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1388">To learn how to configure the app's IIS handlers in *web.config* to pass OPTIONS requests, see [Enable cross-origin requests in ASP.NET Web API 2: How CORS Works](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).</span></span>

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="ff8d0-1389">IIS 系統管理員的部署資源</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1389">Deployment resources for IIS administrators</span></span>

* [<span data-ttu-id="ff8d0-1390">IIS 文件</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1390">IIS documentation</span></span>](/iis)
* [<span data-ttu-id="ff8d0-1391">IIS 中的 IIS 管理員使用者入門</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1391">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* [<span data-ttu-id="ff8d0-1392">.NET Core 應用程式部署</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1392">.NET Core application deployment</span></span>](/dotnet/core/deploying/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/directory-structure>
* <xref:host-and-deploy/iis/modules>
* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>

## <a name="additional-resources"></a><span data-ttu-id="ff8d0-1393">其他資源</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1393">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:index>
* [<span data-ttu-id="ff8d0-1394">Microsoft IIS 官方網站</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1394">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="ff8d0-1395">Windows Server 技術內容庫</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1395">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="ff8d0-1396">IIS 上的 HTTP/2</span><span class="sxs-lookup"><span data-stu-id="ff8d0-1396">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
* <xref:host-and-deploy/iis/transform-webconfig>

::: moniker-end
