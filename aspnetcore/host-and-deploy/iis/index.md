---
title: 在使用 IIS 的 Windows 上裝載 ASP.NET Core
author: guardrex
description: 了解如何在 Windows Server Internet Information Services (IIS) 上裝載 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: host-and-deploy/iis/index
ms.openlocfilehash: ee7918783c0189a63d17678cda02f54dc40bdc24
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172487"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="375e5-103">在使用 IIS 的 Windows 上裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="375e5-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="375e5-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="375e5-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="375e5-105">如需將 ASP.NET Core 應用程式發佈至 IIS 伺服器的教學課程體驗，請參閱<xref:tutorials/publish-to-iis>。</span><span class="sxs-lookup"><span data-stu-id="375e5-105">For a tutorial experience on publishing an ASP.NET Core app to an IIS server, see <xref:tutorials/publish-to-iis>.</span></span>

[<span data-ttu-id="375e5-106">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="375e5-106">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="375e5-107">支援的作業系統</span><span class="sxs-lookup"><span data-stu-id="375e5-107">Supported operating systems</span></span>

<span data-ttu-id="375e5-108">以下為支援的作業系統：</span><span class="sxs-lookup"><span data-stu-id="375e5-108">The following operating systems are supported:</span></span>

* <span data-ttu-id="375e5-109">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="375e5-109">Windows 7 or later</span></span>
* <span data-ttu-id="375e5-110">Windows Server 2012 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="375e5-110">Windows Server 2012 R2 or later</span></span>

<span data-ttu-id="375e5-111">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys) (先前稱為 WebListener) 不適用搭配 IIS 的反向 Proxy 設定。</span><span class="sxs-lookup"><span data-stu-id="375e5-111">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="375e5-112">請使用 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="375e5-112">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="375e5-113">如需在 Azure 中裝載的資訊，請參閱 <xref:host-and-deploy/azure-apps/index>。</span><span class="sxs-lookup"><span data-stu-id="375e5-113">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

<span data-ttu-id="375e5-114">如需疑難排解指引，請參閱 <xref:test/troubleshoot>。</span><span class="sxs-lookup"><span data-stu-id="375e5-114">For troubleshooting guidance, see <xref:test/troubleshoot>.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="375e5-115">支援的平台</span><span class="sxs-lookup"><span data-stu-id="375e5-115">Supported platforms</span></span>

<span data-ttu-id="375e5-116">支援針對 32 位元 (x86) 或 64 位元 (x64) 部署發行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-116">Apps published for 32-bit (x86) or 64-bit (x64) deployment are supported.</span></span> <span data-ttu-id="375e5-117">使用 32 位元 (x86) .NET Core SDK 部署 32 位元應用程式，除非應用程式：</span><span class="sxs-lookup"><span data-stu-id="375e5-117">Deploy a 32-bit app with a 32-bit (x86) .NET Core SDK unless the app:</span></span>

* <span data-ttu-id="375e5-118">需要提供給 64 位元應用程式使用的較大虛擬記憶體位址空間。</span><span class="sxs-lookup"><span data-stu-id="375e5-118">Requires the larger virtual memory address space available to a 64-bit app.</span></span>
* <span data-ttu-id="375e5-119">需要較大的 IIS 堆疊大小。</span><span class="sxs-lookup"><span data-stu-id="375e5-119">Requires the larger IIS stack size.</span></span>
* <span data-ttu-id="375e5-120">有 64 位元原生相依性。</span><span class="sxs-lookup"><span data-stu-id="375e5-120">Has 64-bit native dependencies.</span></span>

<span data-ttu-id="375e5-121">使用 64 位元 (x64) .NET Core SDK 來發行 64 位元應用程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-121">Use a 64-bit (x64) .NET Core SDK to publish a 64-bit app.</span></span> <span data-ttu-id="375e5-122">主機系統上必須有 64 位元執行階段存在。</span><span class="sxs-lookup"><span data-stu-id="375e5-122">A 64-bit runtime must be present on the host system.</span></span>

## <a name="hosting-models"></a><span data-ttu-id="375e5-123">裝載模型</span><span class="sxs-lookup"><span data-stu-id="375e5-123">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="375e5-124">同處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="375e5-124">In-process hosting model</span></span>

<span data-ttu-id="375e5-125">使用同處理序裝載，ASP.NET Core 應用程式會在與其 IIS 工作者處理序相同的處理序中執行。</span><span class="sxs-lookup"><span data-stu-id="375e5-125">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="375e5-126">因為要求未透過回送介面卡 (將連出網路流量傳回同一部電腦的網路介面) 進行 proxy 處理，所以同處理序裝載會提供優於跨處理序裝載的效能。</span><span class="sxs-lookup"><span data-stu-id="375e5-126">In-process hosting provides improved performance over out-of-process hosting because requests aren't proxied over the loopback adapter, a network interface that returns outgoing network traffic back to the same machine.</span></span> <span data-ttu-id="375e5-127">IIS 透過 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 來執行處理程序管理。</span><span class="sxs-lookup"><span data-stu-id="375e5-127">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="375e5-128">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)：</span><span class="sxs-lookup"><span data-stu-id="375e5-128">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module):</span></span>

* <span data-ttu-id="375e5-129">執行應用程式初始化。</span><span class="sxs-lookup"><span data-stu-id="375e5-129">Performs app initialization.</span></span>
  * <span data-ttu-id="375e5-130">載入 [CoreCLR](/dotnet/standard/glossary#coreclr)。</span><span class="sxs-lookup"><span data-stu-id="375e5-130">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="375e5-131">呼叫 `Program.Main`。</span><span class="sxs-lookup"><span data-stu-id="375e5-131">Calls `Program.Main`.</span></span>
* <span data-ttu-id="375e5-132">處理 IIS 原生要求的存留期。</span><span class="sxs-lookup"><span data-stu-id="375e5-132">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="375e5-133">以 .NET Framework 為目標的 ASP.NET Core 應用程式不支援處理序內裝載模型。</span><span class="sxs-lookup"><span data-stu-id="375e5-133">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="375e5-134">下圖說明 IIS、ASP.NET Core 模組和同處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="375e5-134">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![同處理序代管內的 ASP.NET Core 模組案例](index/_static/ancm-inprocess.png)

<span data-ttu-id="375e5-136">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-136">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="375e5-137">驅動程式會在網站設定的連接埠上將原生要求路由至 IIS，此連接埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="375e5-137">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="375e5-138">ASP.NET Core 模組會接收原生要求，並將它傳遞給 IIS HTTP 伺服器（`IISHttpServer`）。</span><span class="sxs-lookup"><span data-stu-id="375e5-138">The ASP.NET Core Module receives the native request and passes it to IIS HTTP Server (`IISHttpServer`).</span></span> <span data-ttu-id="375e5-139">IIS HTTP 伺服器是 IIS 的同處理序伺服程式實作，可將要求從原生轉換為受控。</span><span class="sxs-lookup"><span data-stu-id="375e5-139">IIS HTTP Server is an in-process server implementation for IIS that converts the request from native to managed.</span></span>

<span data-ttu-id="375e5-140">IIS HTTP 伺服器處理要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="375e5-140">After the IIS HTTP Server processes the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="375e5-141">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="375e5-141">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="375e5-142">應用程式的回應會透過 IIS HTTP 伺服器傳回 IIS。</span><span class="sxs-lookup"><span data-stu-id="375e5-142">The app's response is passed back to IIS through IIS HTTP Server.</span></span> <span data-ttu-id="375e5-143">IIS 會將回應傳送到起始該要求的用戶端。</span><span class="sxs-lookup"><span data-stu-id="375e5-143">IIS sends the response to the client that initiated the request.</span></span>

<span data-ttu-id="375e5-144">現有的應用程式可以選擇同處理序裝載，但 [dotnet new](/dotnet/core/tools/dotnet-new) 範本預設會對所有 IIS 和 IIS Express 案例使用同處理序裝載模型。</span><span class="sxs-lookup"><span data-stu-id="375e5-144">In-process hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="375e5-145">`CreateDefaultBuilder` 會透過呼叫 <xref:Microsoft.AspNetCore.Hosting.Server.IServer> 方法來啟動 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*>CoreCLR[ 以新增 ](/dotnet/standard/glossary#coreclr)，並在 IIS 工作者處理序 (*w3wp.exe* 或 *iisexpress.exe*) 內裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-145">`CreateDefaultBuilder` adds an <xref:Microsoft.AspNetCore.Hosting.Server.IServer> instance by calling the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> method to boot the [CoreCLR](/dotnet/standard/glossary#coreclr) and host the app inside of the IIS worker process (*w3wp.exe* or *iisexpress.exe*).</span></span> <span data-ttu-id="375e5-146">效能測試指出，相較於跨處理序裝載 .NET Core 應用程式，並將要求 Proxy 處理至 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器，裝載於同處理序可提供明顯更高的要求輸送量。</span><span class="sxs-lookup"><span data-stu-id="375e5-146">Performance tests indicate that hosting a .NET Core app in-process delivers significantly higher request throughput compared to hosting the app out-of-process and proxying requests to [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

> [!NOTE]
> <span data-ttu-id="375e5-147">發佈為單一檔案可執行檔的應用程式無法由同處理序裝載模型載入。</span><span class="sxs-lookup"><span data-stu-id="375e5-147">Apps published as a single file executable can't be loaded by the in-process hosting model.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="375e5-148">跨處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="375e5-148">Out-of-process hosting model</span></span>

<span data-ttu-id="375e5-149">因為 ASP.NET Core 應用程式會在與 IIS 背景工作進程不同的進程中執行，所以 ASP.NET Core 模組會處理進程管理。</span><span class="sxs-lookup"><span data-stu-id="375e5-149">Because ASP.NET Core apps run in a process separate from the IIS worker process, the ASP.NET Core Module handles process management.</span></span> <span data-ttu-id="375e5-150">此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式關閉或損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="375e5-150">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="375e5-151">此行為基本上與執行同處理序，並由 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="375e5-151">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="375e5-152">下圖說明 IIS、ASP.NET Core 模組和跨處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="375e5-152">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![非同處理序代管內的 ASP.NET Core 模組案例](index/_static/ancm-outofprocess.png)

<span data-ttu-id="375e5-154">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-154">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="375e5-155">驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="375e5-155">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="375e5-156">此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，而且不會是通訊埠 80 或 443。</span><span class="sxs-lookup"><span data-stu-id="375e5-156">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="375e5-157">此模組在啟動時透過環境變數指定連接埠，而 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 延伸模組則會設定伺服器來接聽 `http://localhost:{PORT}`。</span><span class="sxs-lookup"><span data-stu-id="375e5-157">The module specifies the port via an environment variable at startup, and the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> extension configures the server to listen on `http://localhost:{PORT}`.</span></span> <span data-ttu-id="375e5-158">將會執行額外檢查，不是源自模組的要求都會遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="375e5-158">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="375e5-159">此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。</span><span class="sxs-lookup"><span data-stu-id="375e5-159">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="375e5-160">Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="375e5-160">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="375e5-161">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="375e5-161">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="375e5-162">IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="375e5-162">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="375e5-163">應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="375e5-163">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="375e5-164">如需 ASP.NET Core 模組組態指南，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="375e5-164">For ASP.NET Core Module configuration guidance, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="375e5-165">如需代管的詳細資訊，請參閱[在 ASP.NET Core 中代管](xref:fundamentals/index#host)。</span><span class="sxs-lookup"><span data-stu-id="375e5-165">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/index#host).</span></span>

## <a name="application-configuration"></a><span data-ttu-id="375e5-166">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="375e5-166">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="375e5-167">啟用 IISIntegration 元件</span><span class="sxs-lookup"><span data-stu-id="375e5-167">Enable the IISIntegration components</span></span>

<span data-ttu-id="375e5-168">在 `CreateHostBuilder` （*Program.cs*）中建立主機時，請呼叫 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> 以啟用 IIS 整合：</span><span class="sxs-lookup"><span data-stu-id="375e5-168">When building a host in `CreateHostBuilder` (*Program.cs*), call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> to enable IIS integration:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        ...
```

<span data-ttu-id="375e5-169">如需 `CreateDefaultBuilder` 的詳細資訊，請參閱 <xref:fundamentals/host/generic-host#default-builder-settings>。</span><span class="sxs-lookup"><span data-stu-id="375e5-169">For more information on `CreateDefaultBuilder`, see <xref:fundamentals/host/generic-host#default-builder-settings>.</span></span>

### <a name="iis-options"></a><span data-ttu-id="375e5-170">IIS 選項</span><span class="sxs-lookup"><span data-stu-id="375e5-170">IIS options</span></span>

<span data-ttu-id="375e5-171">**同處理序主控模型**</span><span class="sxs-lookup"><span data-stu-id="375e5-171">**In-process hosting model**</span></span>

<span data-ttu-id="375e5-172">若要設定 IIS 伺服器選項，請在 <xref:Microsoft.AspNetCore.Builder.IISServerOptions> 中加入 <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="375e5-172">To configure IIS Server options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISServerOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="375e5-173">下列範例會停用 AutomaticAuthentication：</span><span class="sxs-lookup"><span data-stu-id="375e5-173">The following example disables AutomaticAuthentication:</span></span>

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

| <span data-ttu-id="375e5-174">選項</span><span class="sxs-lookup"><span data-stu-id="375e5-174">Option</span></span>                         | <span data-ttu-id="375e5-175">預設</span><span class="sxs-lookup"><span data-stu-id="375e5-175">Default</span></span> | <span data-ttu-id="375e5-176">設定</span><span class="sxs-lookup"><span data-stu-id="375e5-176">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="375e5-177">若為 `true`，IIS 伺服器會設定由 `HttpContext.User`Windows 驗證[所驗證的 ](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="375e5-177">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="375e5-178">若為 `false`，則伺服器僅會對 `HttpContext.User` 提供身分識別，並在 `AuthenticationScheme` 明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="375e5-178">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="375e5-179">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="375e5-179">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="375e5-180">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="375e5-180">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="375e5-181">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="375e5-181">Sets the display name shown to users on login pages.</span></span> |
| `AllowSynchronousIO`           | `false` | <span data-ttu-id="375e5-182">是否要針對 `HttpContext.Request` 和 `HttpContext.Response` 允許同步 IO。</span><span class="sxs-lookup"><span data-stu-id="375e5-182">Whether synchronous IO is allowed for the `HttpContext.Request` and the `HttpContext.Response`.</span></span> |
| `MaxRequestBodySize`           | `30000000`  | <span data-ttu-id="375e5-183">取得或設定 `HttpRequest` 的要求本文大小上限。</span><span class="sxs-lookup"><span data-stu-id="375e5-183">Gets or sets the max request body size for the `HttpRequest`.</span></span> <span data-ttu-id="375e5-184">請注意，IIS 本身具有限制 `maxAllowedContentLength`，此限制將在 `MaxRequestBodySize` 中設定 `IISServerOptions` 時處理。</span><span class="sxs-lookup"><span data-stu-id="375e5-184">Note that IIS itself has the limit `maxAllowedContentLength` which will be processed before the `MaxRequestBodySize` set in the `IISServerOptions`.</span></span> <span data-ttu-id="375e5-185">變更 `MaxRequestBodySize` 將不會影響 `maxAllowedContentLength`。</span><span class="sxs-lookup"><span data-stu-id="375e5-185">Changing the `MaxRequestBodySize` won't affect the `maxAllowedContentLength`.</span></span> <span data-ttu-id="375e5-186">若要增加 `maxAllowedContentLength`，請在 *web.config* 中新增項目，以將 `maxAllowedContentLength` 設定為較高的值。</span><span class="sxs-lookup"><span data-stu-id="375e5-186">To increase `maxAllowedContentLength`, add an entry in the *web.config* to set `maxAllowedContentLength` to a higher value.</span></span> <span data-ttu-id="375e5-187">如需更多詳細資料，請參閱[組態](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/#configuration)。</span><span class="sxs-lookup"><span data-stu-id="375e5-187">For more details, see [Configuration](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/#configuration).</span></span> |

<span data-ttu-id="375e5-188">**跨處理序裝載模型**</span><span class="sxs-lookup"><span data-stu-id="375e5-188">**Out-of-process hosting model**</span></span>

<span data-ttu-id="375e5-189">若要設定 IIS 選項，請在 <xref:Microsoft.AspNetCore.Builder.IISOptions> 中加入 <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="375e5-189">To configure IIS options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="375e5-190">下列範例會防止應用程式填入 `HttpContext.Connection.ClientCertificate`：</span><span class="sxs-lookup"><span data-stu-id="375e5-190">The following example prevents the app from populating `HttpContext.Connection.ClientCertificate`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="375e5-191">選項</span><span class="sxs-lookup"><span data-stu-id="375e5-191">Option</span></span>                         | <span data-ttu-id="375e5-192">預設</span><span class="sxs-lookup"><span data-stu-id="375e5-192">Default</span></span> | <span data-ttu-id="375e5-193">設定</span><span class="sxs-lookup"><span data-stu-id="375e5-193">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="375e5-194">若為 `true`，[IIS 整合中介軟體](#enable-the-iisintegration-components)會設定由 `HttpContext.User`Windows 驗證[所驗證的 ](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="375e5-194">If `true`, [IIS Integration Middleware](#enable-the-iisintegration-components) sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="375e5-195">如果為 `false`，則驗證中介軟體僅針對 `HttpContext.User` 提供身分識別，並在游 `AuthenticationScheme` 提出明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="375e5-195">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="375e5-196">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="375e5-196">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="375e5-197">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="375e5-197">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="375e5-198">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="375e5-198">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="375e5-199">如果為 `true` 且 `MS-ASPNETCORE-CLIENTCERT` 要求標頭已存在，則會填入 `HttpContext.Connection.ClientCertificate`。</span><span class="sxs-lookup"><span data-stu-id="375e5-199">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="375e5-200">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="375e5-200">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="375e5-201">用來設定轉送標頭中介軟體及 ASP.NET Core 模組的 [IIS 整合中介軟體](#enable-the-iisintegration-components)會設定為轉送配置 (HTTP/HTTPS) 與發出要求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="375e5-201">The [IIS Integration Middleware](#enable-the-iisintegration-components), which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="375e5-202">其他 Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。</span><span class="sxs-lookup"><span data-stu-id="375e5-202">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="375e5-203">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="375e5-203">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="375e5-204">web.config 檔案</span><span class="sxs-lookup"><span data-stu-id="375e5-204">web.config file</span></span>

<span data-ttu-id="375e5-205">*web.config* 檔案是用來設定 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="375e5-205">The *web.config* file configures the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="375e5-206">發佈專案時，由 MSBuild 目標 ( *) 處理* web.config`_TransformWebConfig` 檔案的建立、轉換及發佈。</span><span class="sxs-lookup"><span data-stu-id="375e5-206">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="375e5-207">此目標存在於 Web SDK 目標 (`Microsoft.NET.Sdk.Web`)。</span><span class="sxs-lookup"><span data-stu-id="375e5-207">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="375e5-208">SDK 設定在專案檔的頂端：</span><span class="sxs-lookup"><span data-stu-id="375e5-208">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="375e5-209">如果*web.config*檔案不存在於專案中，則會使用正確的*processPath*和*引數*來建立檔案，以設定 ASP.NET Core 模組並移至[已發行的輸出](xref:host-and-deploy/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="375e5-209">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="375e5-210">如果 *web.config* 檔案存在於專案中，則系統會使用正確的 *processPath* 和 *arguments* 來轉換該檔案以設定 ASP.NET Core 模組，然後將它移至已發行的輸出。</span><span class="sxs-lookup"><span data-stu-id="375e5-210">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="375e5-211">轉換不會修改檔案中的 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="375e5-211">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="375e5-212">*web.config* 檔案可提供能控制作用中 IIS 模組的額外 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="375e5-212">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="375e5-213">如需能處理 ASP.NET Core 應用程式要求之 IIS 模組的相關資訊，請參閱 [IIS 模組](xref:host-and-deploy/iis/modules)主題。</span><span class="sxs-lookup"><span data-stu-id="375e5-213">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="375e5-214">為防止 Web SDK 轉換 *web.config* 檔案，請使用專案檔中的 **\<IsTransformWebConfigDisabled>** 屬性：</span><span class="sxs-lookup"><span data-stu-id="375e5-214">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="375e5-215">使 Web SDK 無法轉換檔案時，應該由開發人員手動設定 *processPath* 和 *arguments*。</span><span class="sxs-lookup"><span data-stu-id="375e5-215">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="375e5-216">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="375e5-216">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="375e5-217">web.config 檔案位置</span><span class="sxs-lookup"><span data-stu-id="375e5-217">web.config file location</span></span>

<span data-ttu-id="375e5-218">為了正確設定[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module) *，web.config 檔案*必須存在於已部署應用程式的[內容根](xref:fundamentals/index#content-root)路徑（通常是應用程式基底路徑）。</span><span class="sxs-lookup"><span data-stu-id="375e5-218">In order to set up the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) correctly, the *web.config* file must be present at the [content root](xref:fundamentals/index#content-root) path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="375e5-219">這是與提供給 IIS 的網站實體路徑相同的位置。</span><span class="sxs-lookup"><span data-stu-id="375e5-219">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="375e5-220">應用程式的根目錄需有 *web.config* 檔案，才能使用 Web Deploy 發行多個應用程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-220">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="375e5-221">機密檔案存在於應用程式的實體路徑上，例如 *\<組件>.runtimeconfig.json*、 *\<組件>.xml* (XML 文件註解)，以及 *\<組件>.deps.json*。</span><span class="sxs-lookup"><span data-stu-id="375e5-221">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="375e5-222">當 *web.config* 檔案存在且網站正常啟動時，如果有人要求機密檔案，IIS 不會予以提供。</span><span class="sxs-lookup"><span data-stu-id="375e5-222">When the *web.config* file is present and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="375e5-223">若 *web.config* 檔案遺失或沒有正確命名，或是無法設定網站以正常啟動，IIS 可能會公開提供機密檔案。</span><span class="sxs-lookup"><span data-stu-id="375e5-223">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="375e5-224">\***Web.config*檔案必須隨時存在於部署中、正確命名，而且能夠將網站設定為正常啟動。絕對不要從生產環境部署*移除 web.config 檔案\*。**</span><span class="sxs-lookup"><span data-stu-id="375e5-224">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

### <a name="transform-webconfig"></a><span data-ttu-id="375e5-225">轉換 web.config</span><span class="sxs-lookup"><span data-stu-id="375e5-225">Transform web.config</span></span>

<span data-ttu-id="375e5-226">如需在發佈時轉換 *web.config* (例如依據設定、設定檔或環境設定環境變數)，請參閱<xref:host-and-deploy/iis/transform-webconfig>。</span><span class="sxs-lookup"><span data-stu-id="375e5-226">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="375e5-227">IIS 組態</span><span class="sxs-lookup"><span data-stu-id="375e5-227">IIS configuration</span></span>

<span data-ttu-id="375e5-228">**Windows Server 作業系統**</span><span class="sxs-lookup"><span data-stu-id="375e5-228">**Windows Server operating systems**</span></span>

<span data-ttu-id="375e5-229">啟用**網頁伺服器 (IIS)** 伺服器角色，並建立角色服務。</span><span class="sxs-lookup"><span data-stu-id="375e5-229">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="375e5-230">使用來自 [管理] 功能表的 [新增角色及功能] 精靈，或是 [伺服器管理員] 中的連結。</span><span class="sxs-lookup"><span data-stu-id="375e5-230">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="375e5-231">在**伺服器角色**步驟中，核取 [網頁伺服器 (IIS)] 方塊。</span><span class="sxs-lookup"><span data-stu-id="375e5-231">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![在選取伺服器角色步驟中選取網頁伺服器 IIS 角色。](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="375e5-233">在 [功能] 步驟之後，[角色服務] 步驟會針對網頁伺服器 (IIS) 進行載入。</span><span class="sxs-lookup"><span data-stu-id="375e5-233">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="375e5-234">選取所需的 IIS 角色服務或接受所提供的預設角色服務。</span><span class="sxs-lookup"><span data-stu-id="375e5-234">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![在選取角色服務步驟中，選取預設的角色服務。](index/_static/role-services-ws2016.png)

   <span data-ttu-id="375e5-236">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="375e5-236">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="375e5-237">若要啟用 Windows 驗證，請展開下列節點：[網頁伺服器] > [安全性]。</span><span class="sxs-lookup"><span data-stu-id="375e5-237">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="375e5-238">選取 [Windows 驗證] 功能。</span><span class="sxs-lookup"><span data-stu-id="375e5-238">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="375e5-239">如需詳細資訊，請參閱 [Windows 驗證 \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) 和[設定 Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="375e5-239">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="375e5-240">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="375e5-240">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="375e5-241">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="375e5-241">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="375e5-242">若要啟用 WebSocket，請展開下列節點：[網頁伺服器] > [應用程式開發]。</span><span class="sxs-lookup"><span data-stu-id="375e5-242">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="375e5-243">選取 [WebSocket 通訊協定] 功能。</span><span class="sxs-lookup"><span data-stu-id="375e5-243">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="375e5-244">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="375e5-244">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="375e5-245">透過**確認**步驟繼續作業，安裝網頁伺服器角色和服務。</span><span class="sxs-lookup"><span data-stu-id="375e5-245">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="375e5-246">安裝**網頁伺服器 (IIS)** 角色之後，不需要重新啟動伺服器/IIS。</span><span class="sxs-lookup"><span data-stu-id="375e5-246">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="375e5-247">**Windows 桌面作業系統**</span><span class="sxs-lookup"><span data-stu-id="375e5-247">**Windows desktop operating systems**</span></span>

<span data-ttu-id="375e5-248">啟用 [IIS 管理主控台] 和 [World Wide Web 服務]。</span><span class="sxs-lookup"><span data-stu-id="375e5-248">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="375e5-249">流覽至 [**控制台**] >**程式**> [**程式和功能**] > [**開啟或關閉 Windows 功能**] （畫面左側）。</span><span class="sxs-lookup"><span data-stu-id="375e5-249">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="375e5-250">開啟 [Internet Information Services] 節點。</span><span class="sxs-lookup"><span data-stu-id="375e5-250">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="375e5-251">開啟 [Web 管理工具] 節點。</span><span class="sxs-lookup"><span data-stu-id="375e5-251">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="375e5-252">核取 [IIS 管理主控台] 方塊。</span><span class="sxs-lookup"><span data-stu-id="375e5-252">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="375e5-253">[World Wide Web Services] (全球資訊網服務) 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="375e5-253">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="375e5-254">接受**全球資訊網服務**的預設功能，或自訂 IIS 功能。</span><span class="sxs-lookup"><span data-stu-id="375e5-254">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="375e5-255">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="375e5-255">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="375e5-256">若要啟用 Windows 驗證，請展開下列節點：[World Wide Web 服務] > [安全性]。</span><span class="sxs-lookup"><span data-stu-id="375e5-256">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="375e5-257">選取 [Windows 驗證] 功能。</span><span class="sxs-lookup"><span data-stu-id="375e5-257">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="375e5-258">如需詳細資訊，請參閱 [Windows 驗證 \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) 和[設定 Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="375e5-258">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="375e5-259">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="375e5-259">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="375e5-260">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="375e5-260">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="375e5-261">若要啟用 WebSocket，請展開下列節點：[World Wide Web 服務] > [應用程式開發功能]。</span><span class="sxs-lookup"><span data-stu-id="375e5-261">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="375e5-262">選取 [WebSocket 通訊協定] 功能。</span><span class="sxs-lookup"><span data-stu-id="375e5-262">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="375e5-263">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="375e5-263">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="375e5-264">若 IIS 安裝需要重新啟動，請重新啟動系統。</span><span class="sxs-lookup"><span data-stu-id="375e5-264">If the IIS installation requires a restart, restart the system.</span></span>

![選取 [Windows 功能] 中的 [IIS 管理主控台] 和 [World Wide Web Services] (全球資訊網服務)。](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="375e5-266">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="375e5-266">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="375e5-267">在主控系統上安裝 .NET Core 裝載套件組合。</span><span class="sxs-lookup"><span data-stu-id="375e5-267">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="375e5-268">套件組合會安裝 .NET Core 執行階段、.NET Core 程式庫和 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="375e5-268">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="375e5-269">此模組可讓 ASP.NET Core 應用程式在 IIS 背後執行。</span><span class="sxs-lookup"><span data-stu-id="375e5-269">The module allows ASP.NET Core apps to run behind IIS.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="375e5-270">若裝載套件組合在 IIS 之前安裝，則必須對該套件組合安裝進行修復。</span><span class="sxs-lookup"><span data-stu-id="375e5-270">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="375e5-271">請在安裝 IIS 之後，再次執行裝載套件組合安裝程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-271">Run the Hosting Bundle installer again after installing IIS.</span></span>
>
> <span data-ttu-id="375e5-272">如果在安裝 64 位元 (x64) 版本的 .NET Core 後才安裝裝載套件組合，那麼可能會遺漏 SDK ([未偵測到 .NET Core SDK](xref:test/troubleshoot#no-net-core-sdks-were-detected))。</span><span class="sxs-lookup"><span data-stu-id="375e5-272">If the Hosting Bundle is installed after installing the 64-bit (x64) version of .NET Core, SDKs might appear to be missing ([No .NET Core SDKs were detected](xref:test/troubleshoot#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="375e5-273">若要解決此問題，請參閱 <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>。</span><span class="sxs-lookup"><span data-stu-id="375e5-273">To resolve the problem, see <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>.</span></span>

### <a name="direct-download-current-version"></a><span data-ttu-id="375e5-274">直接下載 (目前版本)</span><span class="sxs-lookup"><span data-stu-id="375e5-274">Direct download (current version)</span></span>

<span data-ttu-id="375e5-275">使用下列連結下載安裝程式：</span><span class="sxs-lookup"><span data-stu-id="375e5-275">Download the installer using the following link:</span></span>

[<span data-ttu-id="375e5-276">目前的 .NET Core 裝載套件組合安裝程式 (直接下載)</span><span class="sxs-lookup"><span data-stu-id="375e5-276">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a><span data-ttu-id="375e5-277">安裝程式的先前版本</span><span class="sxs-lookup"><span data-stu-id="375e5-277">Earlier versions of the installer</span></span>

<span data-ttu-id="375e5-278">若要取得安裝程式的先前版本：</span><span class="sxs-lookup"><span data-stu-id="375e5-278">To obtain an earlier version of the installer:</span></span>

1. <span data-ttu-id="375e5-279">瀏覽至 [.NET 下載封存](https://www.microsoft.com/net/download/archives)。</span><span class="sxs-lookup"><span data-stu-id="375e5-279">Navigate to the [.NET download archives](https://www.microsoft.com/net/download/archives).</span></span>
1. <span data-ttu-id="375e5-280">在 [.NET Core] 下，選取 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="375e5-280">Under **.NET Core**, select the .NET Core version.</span></span>
1. <span data-ttu-id="375e5-281">在 [執行應用程式 - 執行階段] 欄中，尋找想要的 .NET Core 執行階段版本列。</span><span class="sxs-lookup"><span data-stu-id="375e5-281">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="375e5-282">使用**執行階段與裝載套件組合**連結。</span><span class="sxs-lookup"><span data-stu-id="375e5-282">Download the installer using the **Runtime & Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="375e5-283">某些安裝程式包含已達到期生命週期結束 (EOL) 的發行版本，這些發行版本已不受 Microsoft 支援。</span><span class="sxs-lookup"><span data-stu-id="375e5-283">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="375e5-284">如需詳細資訊，請參閱[支援原則](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) \(英文 \)。</span><span class="sxs-lookup"><span data-stu-id="375e5-284">For more information, see the [support policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="375e5-285">安裝裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="375e5-285">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="375e5-286">在伺服器上執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-286">Run the installer on the server.</span></span> <span data-ttu-id="375e5-287">從系統管理員命令殼層執行安裝程式時，有 下列參數可用：</span><span class="sxs-lookup"><span data-stu-id="375e5-287">The following parameters are available when running the installer from an administrator command shell:</span></span>

   * <span data-ttu-id="375e5-288">`OPT_NO_ANCM=1` &ndash; 略過安裝 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="375e5-288">`OPT_NO_ANCM=1` &ndash; Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="375e5-289">`OPT_NO_RUNTIME=1` &ndash; 略過安裝 .NET Core 執行時間。</span><span class="sxs-lookup"><span data-stu-id="375e5-289">`OPT_NO_RUNTIME=1` &ndash; Skip installing the .NET Core runtime.</span></span> <span data-ttu-id="375e5-290">當伺服器只裝載[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)時使用。</span><span class="sxs-lookup"><span data-stu-id="375e5-290">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="375e5-291">`OPT_NO_SHAREDFX=1` &ndash; 略過安裝 ASP.NET 共用架構（ASP.NET 執行時間）。</span><span class="sxs-lookup"><span data-stu-id="375e5-291">`OPT_NO_SHAREDFX=1` &ndash; Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span> <span data-ttu-id="375e5-292">當伺服器只裝載[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)時使用。</span><span class="sxs-lookup"><span data-stu-id="375e5-292">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="375e5-293">`OPT_NO_X86=1` &ndash; 略過安裝 x86 執行時間。</span><span class="sxs-lookup"><span data-stu-id="375e5-293">`OPT_NO_X86=1` &ndash; Skip installing x86 runtimes.</span></span> <span data-ttu-id="375e5-294">當您確定不會裝載 32 位元應用程式時，請使用此參數。</span><span class="sxs-lookup"><span data-stu-id="375e5-294">Use this parameter when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="375e5-295">如果將來有可能同時裝載 32 位元和 64 位元應用程式，請不要使用此參數並安裝這兩個執行階段。</span><span class="sxs-lookup"><span data-stu-id="375e5-295">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this parameter and install both runtimes.</span></span>
   * <span data-ttu-id="375e5-296">`OPT_NO_SHARED_CONFIG_CHECK=1` &ndash; 當共用設定（*applicationhost.config*）位於與 iis 安裝相同的電腦上時，請停用 [使用 iis 共用設定檢查]。</span><span class="sxs-lookup"><span data-stu-id="375e5-296">`OPT_NO_SHARED_CONFIG_CHECK=1` &ndash; Disable the check for using an IIS Shared Configuration when the shared configuration (*applicationHost.config*) is on the same machine as the IIS installation.</span></span> <span data-ttu-id="375e5-297">*只在 ASP.NET Core 2.2 或更新版本的裝載套件組合安裝程式上可用。*</span><span class="sxs-lookup"><span data-stu-id="375e5-297">*Only available for ASP.NET Core 2.2 or later Hosting Bundler installers.*</span></span> <span data-ttu-id="375e5-298">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>。</span><span class="sxs-lookup"><span data-stu-id="375e5-298">For more information, see <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>.</span></span>
1. <span data-ttu-id="375e5-299">重新開機系統，或在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="375e5-299">Restart the system or execute the following commands in a command shell:</span></span>

   ```console
   net stop was /y
   net start w3svc
   ```
   <span data-ttu-id="375e5-300">重新啟動 IIS 將能偵測到由安裝程式對系統路徑 (此為環境變數) 所做出的變更。</span><span class="sxs-lookup"><span data-stu-id="375e5-300">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

<span data-ttu-id="375e5-301">ASP.NET Core 不採用共用架構封裝修補程式版本的向前復原行為。</span><span class="sxs-lookup"><span data-stu-id="375e5-301">ASP.NET Core doesn't adopt roll-forward behavior for patch releases of shared framework packages.</span></span> <span data-ttu-id="375e5-302">藉由安裝新的裝載套件組合來升級共用架構之後，請重新開機系統，或在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="375e5-302">After upgrading the shared framework by installing a new hosting bundle, restart the system or execute the following commands in a command shell:</span></span>

```console
net stop was /y
net start w3svc
```

> [!NOTE]
> <span data-ttu-id="375e5-303">如需 IIS 共用組態的資訊，請參閱[使用 IIS 共用組態的 ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)。</span><span class="sxs-lookup"><span data-stu-id="375e5-303">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="375e5-304">使用 Visual Studio 發佈時安裝 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="375e5-304">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="375e5-305">將應用程式部署到具有 [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later) 的伺服器時，請在伺服器上安裝最新版的 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="375e5-305">When deploying apps to servers with [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="375e5-306">若要安裝 Web Deploy，請使用 [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) 或從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=43717)直接取得安裝程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-306">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="375e5-307">慣用的方法是使用 WebPI。</span><span class="sxs-lookup"><span data-stu-id="375e5-307">The preferred method is to use WebPI.</span></span> <span data-ttu-id="375e5-308">WebPI 提供獨立的安裝程式和組態以裝載提供者。</span><span class="sxs-lookup"><span data-stu-id="375e5-308">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="375e5-309">建立 IIS 網站</span><span class="sxs-lookup"><span data-stu-id="375e5-309">Create the IIS site</span></span>

1. <span data-ttu-id="375e5-310">在主控系統上，請建立資料夾以容納應用程式的已發行資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="375e5-310">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="375e5-311">在下列步驟中，您提供資料夾路徑給 IIS，作為應用程式的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="375e5-311">In a following step, the folder's path is provided to IIS as the physical path to the app.</span></span> <span data-ttu-id="375e5-312">如需應用程式之部署資料夾和檔案配置的詳細資訊，請參閱 <xref:host-and-deploy/directory-structure>。</span><span class="sxs-lookup"><span data-stu-id="375e5-312">For more information on an app's deployment folder and file layout, see <xref:host-and-deploy/directory-structure>.</span></span>

1. <span data-ttu-id="375e5-313">在 [IIS 管理員] 中，於 [連線] 面板中開啟伺服器的節點。</span><span class="sxs-lookup"><span data-stu-id="375e5-313">In IIS Manager, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="375e5-314">以滑鼠右鍵按一下 [網站] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="375e5-314">Right-click the **Sites** folder.</span></span> <span data-ttu-id="375e5-315">從操作功能表選取 [新增網站]。</span><span class="sxs-lookup"><span data-stu-id="375e5-315">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="375e5-316">提供**網站名稱**，並將**實體路徑**設定為應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="375e5-316">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="375e5-317">透過選取 [確定] 來提供**繫結**設定並建立網站：</span><span class="sxs-lookup"><span data-stu-id="375e5-317">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![在新增網站步驟中提供站台名稱、實體路徑和主機名稱。](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="375e5-319">請`http://*:80/`勿`http://+:80`使用最上層萬用字元繫結 (**與** )。</span><span class="sxs-lookup"><span data-stu-id="375e5-319">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="375e5-320">最上層萬用字元繫結可能暴露您的應用程式安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="375e5-320">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="375e5-321">這對強式與弱式萬用字元皆適用。</span><span class="sxs-lookup"><span data-stu-id="375e5-321">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="375e5-322">請使用明確主機名稱，而非萬用字元。</span><span class="sxs-lookup"><span data-stu-id="375e5-322">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="375e5-323">若您擁有整個父網域 (與具弱點的 `*.mysub.com` 相對) 的控制權，則子網域萬用字元繫結 (例如 `*.com`) 就沒有此安全性風險。</span><span class="sxs-lookup"><span data-stu-id="375e5-323">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="375e5-324">如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="375e5-324">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="375e5-325">在伺服器的節點之下，選取 [應用程式集區]。</span><span class="sxs-lookup"><span data-stu-id="375e5-325">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="375e5-326">以滑鼠右鍵按一下網站的應用程式集區，然後從操作功能表選取 [基本設定]。</span><span class="sxs-lookup"><span data-stu-id="375e5-326">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="375e5-327">在 [編輯應用程式集區] 視窗中，將 [.NET CLR 版本] 設定為 [沒有受控碼]：</span><span class="sxs-lookup"><span data-stu-id="375e5-327">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![將 .NET CLR 版本設為 [沒有受控碼]。](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="375e5-329">ASP.NET Core 會在不同的處理序中執行，並管理執行階段。</span><span class="sxs-lookup"><span data-stu-id="375e5-329">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="375e5-330">ASP.NET Core 不仰賴載入桌面 CLR (.NET CLR)&mdash;會使用 .NET Core 的核心通用語言執行平台 (CoreCLR) 來開機以在背景工作處理序中裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-330">ASP.NET Core doesn't rely on loading the desktop CLR (.NET CLR)&mdash;the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process.</span></span> <span data-ttu-id="375e5-331">將 [.NET CLR 版本] 設定為 [沒有受控碼] 是選擇性的，但建議這樣做。</span><span class="sxs-lookup"><span data-stu-id="375e5-331">Setting the **.NET CLR version** to **No Managed Code** is optional but recommended.</span></span>

1. <span data-ttu-id="375e5-332">*ASP.NET Core 2.2 或更新版本*：對於使用[同處理序主控模型](/dotnet/core/deploying/#self-contained-deployments-scd)的 64 位元 (x64) [獨立式部署](#in-process-hosting-model)，會停用 32 位元 (x86) 處理序的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="375e5-332">*ASP.NET Core 2.2 or later*: For a 64-bit (x64) [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) that uses the [in-process hosting model](#in-process-hosting-model), disable the app pool for 32-bit (x86) processes.</span></span>

   <span data-ttu-id="375e5-333">在 IIS 管理員的 [動作] 資訊看板 > [應用程式集區] 中，選取 [設定應用程式集區預設值] 或 [進階設定]。</span><span class="sxs-lookup"><span data-stu-id="375e5-333">In the **Actions** sidebar of IIS Manager > **Application Pools**, select **Set Application Pool Defaults** or **Advanced Settings**.</span></span> <span data-ttu-id="375e5-334">找到 [啟用 32 位元應用程式]，然後將其值設定為 `False`。</span><span class="sxs-lookup"><span data-stu-id="375e5-334">Locate **Enable 32-Bit Applications** and set the value to `False`.</span></span> <span data-ttu-id="375e5-335">此設定不會影響為[處理程序外裝載](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model)部署的應用程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-335">This setting doesn't affect apps deployed for [out-of-process hosting](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).</span></span>

1. <span data-ttu-id="375e5-336">確認處理序模型身分識別具有適當的權限。</span><span class="sxs-lookup"><span data-stu-id="375e5-336">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="375e5-337">如果您將應用程式集區的預設身分識別 ([處理序模型] > [身分識別]) 從 **ApplicationPoolIdentity** 變更為其他身分識別，請確認新的身分識別具有必要權限，可存取應用程式的資料夾、資料庫和其他必要的資源。</span><span class="sxs-lookup"><span data-stu-id="375e5-337">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="375e5-338">例如，應用程式集區需要針對應用程式讀取和寫入檔案的資料夾取得讀取和寫入權限。</span><span class="sxs-lookup"><span data-stu-id="375e5-338">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="375e5-339">**Windows 驗證設定 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="375e5-339">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="375e5-340">如需詳細資訊，請參閱[設定 Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="375e5-340">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="375e5-341">部署應用程式</span><span class="sxs-lookup"><span data-stu-id="375e5-341">Deploy the app</span></span>

<span data-ttu-id="375e5-342">將應用程式部署至在**建立 IIS 網站**一節中所建立的 IIS [實體路徑](#create-the-iis-site)資料夾。</span><span class="sxs-lookup"><span data-stu-id="375e5-342">Deploy the app to the IIS **Physical path** folder that was established in the [Create the IIS site](#create-the-iis-site) section.</span></span> <span data-ttu-id="375e5-343">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 是建議的部署機制，但有數個選項可將應用程式從專案的 [publish] 資料夾移動至主機系統的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="375e5-343">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment, but several options exist for moving the app from the project's *publish* folder to the hosting system's deployment folder.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="375e5-344">使用 Visual Studio 的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="375e5-344">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="375e5-345">請參閱[適用於 ASP.NET Core 應用程式部署的 Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)主題，了解如何建立搭配 Web Deploy 使用的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="375e5-345">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="375e5-346">如果主機服務提供者提供或支援建立發行設定檔，請下載其設定檔，並使用 Visual Studio 的 [發行] 對話方塊匯入其設定檔：</span><span class="sxs-lookup"><span data-stu-id="375e5-346">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog:</span></span>

![[發佈] 對話方塊頁](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="375e5-348">Visual Studio 外部的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="375e5-348">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="375e5-349">透過命令列也可以在 Visual Studio 外部使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy)。</span><span class="sxs-lookup"><span data-stu-id="375e5-349">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="375e5-350">如需詳細資訊，請參閱 [Web 部署工具](/iis/publish/using-web-deploy/use-the-web-deployment-tool)。</span><span class="sxs-lookup"><span data-stu-id="375e5-350">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="375e5-351">Web Deploy 的替代項目</span><span class="sxs-lookup"><span data-stu-id="375e5-351">Alternatives to Web Deploy</span></span>

<span data-ttu-id="375e5-352">使用數種方法的其中一種將應用程式移到主機系統，例如手動複製、[Xcopy](/windows-server/administration/windows-commands/xcopy)、[Robocopy](/windows-server/administration/windows-commands/robocopy) 或 [PowerShell](/powershell/)。</span><span class="sxs-lookup"><span data-stu-id="375e5-352">Use any of several methods to move the app to the hosting system, such as manual copy, [Xcopy](/windows-server/administration/windows-commands/xcopy), [Robocopy](/windows-server/administration/windows-commands/robocopy), or [PowerShell](/powershell/).</span></span>

<span data-ttu-id="375e5-353">如需 IIS 的 ASP.NET Core 部署詳細資訊，請參閱 [IIS 系統管理員的部署資源](#deployment-resources-for-iis-administrators)一節。</span><span class="sxs-lookup"><span data-stu-id="375e5-353">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="375e5-354">瀏覽網站</span><span class="sxs-lookup"><span data-stu-id="375e5-354">Browse the website</span></span>

<span data-ttu-id="375e5-355">應用程式部署到主機系統之後，請向其中一個應用程式的公用端點發出要求。</span><span class="sxs-lookup"><span data-stu-id="375e5-355">After the app is deployed to the hosting system, make a request to one of the app's public endpoints.</span></span>

<span data-ttu-id="375e5-356">在下列範例中，網站系結至**埠**`80`上 `www.mysite.com` 的 IIS**主機名稱**。</span><span class="sxs-lookup"><span data-stu-id="375e5-356">In the following example, the site is bound to an IIS **Host name** of `www.mysite.com` on **Port** `80`.</span></span> <span data-ttu-id="375e5-357">對 `http://www.mysite.com` 發出要求：</span><span class="sxs-lookup"><span data-stu-id="375e5-357">A request is made to `http://www.mysite.com`:</span></span>

![Microsoft Edge 瀏覽器已載入 IIS 啟動頁面。](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="375e5-359">已鎖定的部署檔案</span><span class="sxs-lookup"><span data-stu-id="375e5-359">Locked deployment files</span></span>

<span data-ttu-id="375e5-360">當應用程式執行時，會鎖定部署資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="375e5-360">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="375e5-361">無法於部署期間覆寫已鎖定的檔案。</span><span class="sxs-lookup"><span data-stu-id="375e5-361">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="375e5-362">若要釋放部署中的已鎖定檔案，請使用下列其中**一種**方法停止應用程式集區：</span><span class="sxs-lookup"><span data-stu-id="375e5-362">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="375e5-363">使用 Web Deploy 並參考專案檔中的 `Microsoft.NET.Sdk.Web`。</span><span class="sxs-lookup"><span data-stu-id="375e5-363">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="375e5-364">*app_offline.htm* 檔案是放在 Web 應用程式目錄的根目錄中。</span><span class="sxs-lookup"><span data-stu-id="375e5-364">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="375e5-365">當檔案存在時，ASP.NET Core 模組會正常關閉應用程式，並在部署期間提供 *app_offline.htm* 檔案。</span><span class="sxs-lookup"><span data-stu-id="375e5-365">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="375e5-366">如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)。</span><span class="sxs-lookup"><span data-stu-id="375e5-366">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="375e5-367">在伺服器上的 IIS 管理員中手動停止應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="375e5-367">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="375e5-368">使用 PowerShell 卸載*app_offline .htm* （需要 PowerShell 5 或更新版本）：</span><span class="sxs-lookup"><span data-stu-id="375e5-368">Use PowerShell to drop *app_offline.htm* (requires PowerShell 5 or later):</span></span>

  ```powershell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a><span data-ttu-id="375e5-369">資料保護</span><span class="sxs-lookup"><span data-stu-id="375e5-369">Data protection</span></span>

<span data-ttu-id="375e5-370">[ASP.NET Core 資料保護堆疊](xref:security/data-protection/introduction)是由數個 ASP.NET Core [中介軟體](xref:fundamentals/middleware/index)所使用，包括用於驗證的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="375e5-370">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="375e5-371">即使資料保護 API 不是由使用者程式碼呼叫，仍應使用部署指令碼或是在使用者程式碼中設定資料保護，以建立持續性的密碼編譯[金鑰存放區](xref:security/data-protection/implementation/key-management)。</span><span class="sxs-lookup"><span data-stu-id="375e5-371">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="375e5-372">如不設定資料保護，金鑰會保留在記憶體中，並於應用程式重新啟動時捨棄。</span><span class="sxs-lookup"><span data-stu-id="375e5-372">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="375e5-373">如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：</span><span class="sxs-lookup"><span data-stu-id="375e5-373">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="375e5-374">所有以 Cookie 為基礎的驗證權杖都會失效。</span><span class="sxs-lookup"><span data-stu-id="375e5-374">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="375e5-375">當使用者提出下一個要求時，需要再次登入。</span><span class="sxs-lookup"><span data-stu-id="375e5-375">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="375e5-376">所有以 Keyring 保護的資料都無法再解密。</span><span class="sxs-lookup"><span data-stu-id="375e5-376">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="375e5-377">這可能會包含 [CSRF 權杖](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)和 [ASP.NET Core MVC TempData cookie](xref:fundamentals/app-state#tempdata)。</span><span class="sxs-lookup"><span data-stu-id="375e5-377">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="375e5-378">若要在 IIS 下設定資料保護以保存 Keyring，請使用下列其中**一種**方法：</span><span class="sxs-lookup"><span data-stu-id="375e5-378">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="375e5-379">**建立資料保護登錄機碼**</span><span class="sxs-lookup"><span data-stu-id="375e5-379">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="375e5-380">ASP.NET Core 應用程式所使用的資料保護金鑰會儲存在應用程式外部的登錄中。</span><span class="sxs-lookup"><span data-stu-id="375e5-380">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="375e5-381">若要保存指定應用程式的金鑰，請為應用程式集區建立登錄機碼。</span><span class="sxs-lookup"><span data-stu-id="375e5-381">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="375e5-382">若為獨立的非Web 伺服陣列 IIS 安裝，請針對搭配使用 ASP.NET Core 應用程式的每個應用程式集區，使用[資料保護 Provision-AutoGenKeys.ps1 PowerShell 指令碼](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1)。</span><span class="sxs-lookup"><span data-stu-id="375e5-382">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="375e5-383">此指令碼會在 HKLM 登錄中建立登錄機碼，只有應用程式之應用程式集區的背景工作處理序帳戶可以存取它。</span><span class="sxs-lookup"><span data-stu-id="375e5-383">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="375e5-384">在待用期間使用 DPAPI 和全電腦金鑰加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="375e5-384">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="375e5-385">在 Web 伺服陣列案例中，應用程式可以設定成使用 UNC 路徑來儲存其資料保護 Keyring。</span><span class="sxs-lookup"><span data-stu-id="375e5-385">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="375e5-386">根據預設，資料保護金鑰不予加密。</span><span class="sxs-lookup"><span data-stu-id="375e5-386">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="375e5-387">請確保網路共用的檔案權限僅限於執行應用程式的 Windows 帳戶。</span><span class="sxs-lookup"><span data-stu-id="375e5-387">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="375e5-388">可以使用 X509 憑證來保護待用的金鑰。</span><span class="sxs-lookup"><span data-stu-id="375e5-388">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="375e5-389">請考慮下列讓使用者上傳憑證的機制：將憑證放入使用者的受信任憑證存放區，並確保在執行使用者應用程式的所有電腦上都能使用這些憑證。</span><span class="sxs-lookup"><span data-stu-id="375e5-389">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="375e5-390">如需詳細資訊，請參閱[設定 ASP.NET Core 資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="375e5-390">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="375e5-391">**設定 IIS 應用程式集區載入使用者設定檔**</span><span class="sxs-lookup"><span data-stu-id="375e5-391">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="375e5-392">此設定位在應用程式集區 [進階設定] 下的 [處理序模型] 區段中。</span><span class="sxs-lookup"><span data-stu-id="375e5-392">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="375e5-393">將 [載入使用者設定檔] 設為 `True`。</span><span class="sxs-lookup"><span data-stu-id="375e5-393">Set **Load User Profile** to `True`.</span></span> <span data-ttu-id="375e5-394">當設定為 `True` 時，金鑰會儲存在使用者設定檔目錄中，且使用具有使用者帳戶專屬金鑰的 DPAPI 保護。</span><span class="sxs-lookup"><span data-stu-id="375e5-394">When set to `True`, keys are stored in the user profile directory and protected using DPAPI with a key specific to the user account.</span></span> <span data-ttu-id="375e5-395">金鑰會保存到 *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="375e5-395">Keys are persisted to the *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* folder.</span></span>

  <span data-ttu-id="375e5-396">應用程式集區的 [setProfileEnvironment 屬性](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration)也必須啟用。</span><span class="sxs-lookup"><span data-stu-id="375e5-396">The app pool's [setProfileEnvironment attribute](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) must also be enabled.</span></span> <span data-ttu-id="375e5-397">`setProfileEnvironment` 的預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="375e5-397">The default value of `setProfileEnvironment` is `true`.</span></span> <span data-ttu-id="375e5-398">在某些情況下 (例如 Windows OS)，`setProfileEnvironment` 會設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="375e5-398">In some scenarios (for example, Windows OS), `setProfileEnvironment` is set to `false`.</span></span> <span data-ttu-id="375e5-399">如果金鑰並未如預期地儲存在使用者設定檔目錄中：</span><span class="sxs-lookup"><span data-stu-id="375e5-399">If keys aren't stored in the user profile directory as expected:</span></span>

  1. <span data-ttu-id="375e5-400">瀏覽至 *%windir%/system32/inetsrv/config* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="375e5-400">Navigate to the *%windir%/system32/inetsrv/config* folder.</span></span>
  1. <span data-ttu-id="375e5-401">開啟 *applicationHost.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="375e5-401">Open the *applicationHost.config* file.</span></span>
  1. <span data-ttu-id="375e5-402">找出 `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` 元素。</span><span class="sxs-lookup"><span data-stu-id="375e5-402">Locate the `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` element.</span></span>
  1. <span data-ttu-id="375e5-403">確認 `setProfileEnvironment` 屬性不存在 (其預設值為 `true`)，或明確地將屬性值設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="375e5-403">Confirm that the `setProfileEnvironment` attribute isn't present, which defaults the value to `true`, or explicitly set the attribute's value to `true`.</span></span>

* <span data-ttu-id="375e5-404">**將檔案系統當作 Keyring 存放區使用**</span><span class="sxs-lookup"><span data-stu-id="375e5-404">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="375e5-405">調整應用程式程式碼，以[將檔案系統當作 Keyring 存放區使用](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="375e5-405">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="375e5-406">使用 X509 憑證來保護 Keyring，並確保憑證是受信任的憑證。</span><span class="sxs-lookup"><span data-stu-id="375e5-406">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="375e5-407">如果憑證為自我簽署，請將憑證放在受信任的根存放區。</span><span class="sxs-lookup"><span data-stu-id="375e5-407">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="375e5-408">在 Web 伺服陣列中使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="375e5-408">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="375e5-409">請使用所有電腦都可以存取的檔案共用。</span><span class="sxs-lookup"><span data-stu-id="375e5-409">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="375e5-410">將 X509 憑證部署到每一部電腦。</span><span class="sxs-lookup"><span data-stu-id="375e5-410">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="375e5-411">設定[程式碼中的資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="375e5-411">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="375e5-412">**設定資料保護的全電腦原則**</span><span class="sxs-lookup"><span data-stu-id="375e5-412">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="375e5-413">針對取用資料保護 API 的所有應用程式，資料保護系統僅支援有限的預設[全電腦原則](xref:security/data-protection/configuration/machine-wide-policy)設定。</span><span class="sxs-lookup"><span data-stu-id="375e5-413">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="375e5-414">如需詳細資訊，請參閱 <xref:security/data-protection/introduction>。</span><span class="sxs-lookup"><span data-stu-id="375e5-414">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="virtual-directories"></a><span data-ttu-id="375e5-415">虛擬目錄</span><span class="sxs-lookup"><span data-stu-id="375e5-415">Virtual Directories</span></span>

<span data-ttu-id="375e5-416">ASP.NET Core 應用程式不支援 [IIS 虛擬目錄](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories)。</span><span class="sxs-lookup"><span data-stu-id="375e5-416">[IIS Virtual Directories](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) aren't supported with ASP.NET Core apps.</span></span> <span data-ttu-id="375e5-417">應用程式能以[子應用程式](#sub-applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="375e5-417">An app can be hosted as a [sub-application](#sub-applications).</span></span>

## <a name="sub-applications"></a><span data-ttu-id="375e5-418">子應用程式</span><span class="sxs-lookup"><span data-stu-id="375e5-418">Sub-applications</span></span>

<span data-ttu-id="375e5-419">ASP.NET Core 應用程式能以 [IIS 子應用程式](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="375e5-419">An ASP.NET Core app can be hosted as an [IIS sub-application (sub-app)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications).</span></span> <span data-ttu-id="375e5-420">子應用程式的路徑會成為根應用程式 URL 的一部分。</span><span class="sxs-lookup"><span data-stu-id="375e5-420">The sub-app's path becomes part of the root app's URL.</span></span>

<span data-ttu-id="375e5-421">子應用程式內的靜態資產連結應該使用波狀符號與斜線 (`~/`) 標記法。</span><span class="sxs-lookup"><span data-stu-id="375e5-421">Static asset links within the sub-app should use tilde-slash (`~/`) notation.</span></span> <span data-ttu-id="375e5-422">波狀符號與斜線標記法會觸發[標記協助程式](xref:mvc/views/tag-helpers/intro)以將子應用程式的路徑基底附加到轉譯的相對連結前面。</span><span class="sxs-lookup"><span data-stu-id="375e5-422">Tilde-slash notation triggers a [Tag Helper](xref:mvc/views/tag-helpers/intro) to prepend the sub-app's pathbase to the rendered relative link.</span></span> <span data-ttu-id="375e5-423">針對位於 `/subapp_path` 的子應用程式，使用 `src="~/image.png"` 連結的影像會轉譯為 `src="/subapp_path/image.png"`。</span><span class="sxs-lookup"><span data-stu-id="375e5-423">For a sub-app at `/subapp_path`, an image linked with `src="~/image.png"` is rendered as `src="/subapp_path/image.png"`.</span></span> <span data-ttu-id="375e5-424">根應用程式的靜態檔案中介軟體不會處理靜態檔案要求。</span><span class="sxs-lookup"><span data-stu-id="375e5-424">The root app's Static File Middleware doesn't process the static file request.</span></span> <span data-ttu-id="375e5-425">要求會由子應用程式的靜態檔案中介軟體處理。</span><span class="sxs-lookup"><span data-stu-id="375e5-425">The request is processed by the sub-app's Static File Middleware.</span></span>

<span data-ttu-id="375e5-426">若靜態資產的 `src` 屬性是設定為絕對路徑 (例如，`src="/image.png"`)，會以不使用子應用程式路徑基底的方式轉譯連結。</span><span class="sxs-lookup"><span data-stu-id="375e5-426">If a static asset's `src` attribute is set to an absolute path (for example, `src="/image.png"`), the link is rendered without the sub-app's pathbase.</span></span> <span data-ttu-id="375e5-427">根應用程式的靜態檔案中介軟體會嘗試從根應用程式的 [webroot](xref:fundamentals/index#web-root) 提供資產，這會導致「404 - 找不到」回應 (除非根應用程式可存取靜態資產)。</span><span class="sxs-lookup"><span data-stu-id="375e5-427">The root app's Static File Middleware attempts to serve the asset from the root app's [web root](xref:fundamentals/index#web-root), which results in a *404 - Not Found* response unless the static asset is available from the root app.</span></span>

<span data-ttu-id="375e5-428">裝載 ASP.NET Core 應用程式做為另一個 ASP.NET Core 應用程式下的子應用程式：</span><span class="sxs-lookup"><span data-stu-id="375e5-428">To host an ASP.NET Core app as a sub-app under another ASP.NET Core app:</span></span>

1. <span data-ttu-id="375e5-429">為子應用程式建立應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="375e5-429">Establish an app pool for the sub-app.</span></span> <span data-ttu-id="375e5-430">將 [.NET CLR 版本] 設定為 [沒有受控碼]，因為會將核心通用語言執行平台 (CoreCLR) 開機以在背景工作處理序中裝載應用程式，而非在桌面 CLR (.NET CLR) 中裝載。</span><span class="sxs-lookup"><span data-stu-id="375e5-430">Set the **.NET CLR Version** to **No Managed Code** because the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process, not the desktop CLR (.NET CLR).</span></span>

1. <span data-ttu-id="375e5-431">使用根網站下資料夾中的子應用程式在 IIS 管理員中新增根網站。</span><span class="sxs-lookup"><span data-stu-id="375e5-431">Add the root site in IIS Manager with the sub-app in a folder under the root site.</span></span>

1. <span data-ttu-id="375e5-432">以滑鼠右鍵按一下 IIS 管理員中的子應用程式資料夾，然後選取 [轉換成應用程式]。</span><span class="sxs-lookup"><span data-stu-id="375e5-432">Right-click the sub-app folder in IIS Manager and select **Convert to Application**.</span></span>

1. <span data-ttu-id="375e5-433">在 [新增應用程式] 對話方塊中，使用 [應用程式集區] 的[選取] 按鈕來指派您為子應用程式建立的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="375e5-433">In the **Add Application** dialog, use the **Select** button for the **Application Pool** to assign the app pool that you created for the sub-app.</span></span> <span data-ttu-id="375e5-434">選取 **[確定]** 。</span><span class="sxs-lookup"><span data-stu-id="375e5-434">Select **OK**.</span></span>

<span data-ttu-id="375e5-435">將不同的應用程式集區指派給子應用程式是使用同處理序裝載模型。</span><span class="sxs-lookup"><span data-stu-id="375e5-435">The assignment of a separate app pool to the sub-app is a requirement when using the in-process hosting model.</span></span>

<span data-ttu-id="375e5-436">如需有關同進程裝載模型和設定 ASP.NET Core 模組的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="375e5-436">For more information on the in-process hosting model and configuring the ASP.NET Core Module, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="375e5-437">使用 web.config 的 IIS 組態</span><span class="sxs-lookup"><span data-stu-id="375e5-437">Configuration of IIS with web.config</span></span>

<span data-ttu-id="375e5-438">在對使用了 ASP.NET Core 模組的 ASP.NET Core 有作用的 IIS 情境下，設定會受 `<system.webServer>`web.config*的* 區段影響。</span><span class="sxs-lookup"><span data-stu-id="375e5-438">IIS configuration is influenced by the `<system.webServer>` section of *web.config* for IIS scenarios that are functional for ASP.NET Core apps with the ASP.NET Core Module.</span></span> <span data-ttu-id="375e5-439">舉例來說，IIS 設定對動態壓縮有作用。</span><span class="sxs-lookup"><span data-stu-id="375e5-439">For example, IIS configuration is functional for dynamic compression.</span></span> <span data-ttu-id="375e5-440">如果在伺服器層級將 IIS 設為使用動態壓縮，應用程式 `<urlCompression>`web.config*檔案中的* 元素則可為 ASP.NET Core 應用程式予以停用。</span><span class="sxs-lookup"><span data-stu-id="375e5-440">If IIS is configured at the server level to use dynamic compression, the `<urlCompression>` element in the app's *web.config* file can disable it for an ASP.NET Core app.</span></span>

<span data-ttu-id="375e5-441">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="375e5-441">For more information, see the following topics:</span></span>

* [<span data-ttu-id="375e5-442">\<System.webserver > 的設定參考</span><span class="sxs-lookup"><span data-stu-id="375e5-442">Configuration reference for \<system.webServer></span></span>](/iis/configuration/system.webServer/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>

<span data-ttu-id="375e5-443">若要設定在隔離的應用程式集區中執行之個別應用程式的環境變數 (支援 IIS 10.0 或更新版本)，請參閱 IIS 參考文件之*環境變數* environmentVariables>[ 主題的 \<AppCmd.exe 命令](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe)一節。</span><span class="sxs-lookup"><span data-stu-id="375e5-443">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="375e5-444">web.config 的組態區段</span><span class="sxs-lookup"><span data-stu-id="375e5-444">Configuration sections of web.config</span></span>

<span data-ttu-id="375e5-445">ASP.NET Core 應用程式的設定不使用 *web.config* 中 ASP.NET 4.x 應用程式的設定區段：</span><span class="sxs-lookup"><span data-stu-id="375e5-445">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

<span data-ttu-id="375e5-446">使用其他組態提供者設定的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-446">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="375e5-447">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="375e5-447">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="375e5-448">應用程式集區</span><span class="sxs-lookup"><span data-stu-id="375e5-448">Application Pools</span></span>

<span data-ttu-id="375e5-449">應用程式集區隔離取決於裝載模型：</span><span class="sxs-lookup"><span data-stu-id="375e5-449">App pool isolation is determined by the hosting model:</span></span>

* <span data-ttu-id="375e5-450">處理序內裝載 &ndash; 應用程式必須在分開的應用程式集區中執行。</span><span class="sxs-lookup"><span data-stu-id="375e5-450">In-process hosting &ndash; Apps are required to run in separate app pools.</span></span>
* <span data-ttu-id="375e5-451">處理序外裝載 &ndash; 建議藉由在各自的應用程式集區中執行每個應用程式，以將應用程式互相隔離。</span><span class="sxs-lookup"><span data-stu-id="375e5-451">Out-of-process hosting &ndash; We recommend isolating the apps from each other by running each app in its own app pool.</span></span>

<span data-ttu-id="375e5-452">IIS [新增網站] 對話方塊預設每個應用程式皆為單一應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="375e5-452">The IIS **Add Website** dialog defaults to a single app pool per app.</span></span> <span data-ttu-id="375e5-453">當提供 [網站名稱] 時，文字會自動轉移至 [應用程式集區] 文字方塊。</span><span class="sxs-lookup"><span data-stu-id="375e5-453">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="375e5-454">新增網站時，會使用該網站名稱建立新的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="375e5-454">A new app pool is created using the site name when the site is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="375e5-455">應用程式集區識別碼</span><span class="sxs-lookup"><span data-stu-id="375e5-455">Application Pool Identity</span></span>

<span data-ttu-id="375e5-456">應用程式集區身分識別帳戶可讓應用程式在唯一的帳戶下執行，不必建立及管理網域或本機帳戶。</span><span class="sxs-lookup"><span data-stu-id="375e5-456">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="375e5-457">在 IIS 8.0 或更新版本中，IIS 管理背景工作處理序 (WAS) 會使用新的應用程式集區名稱建立虛擬帳戶，並預設在此帳戶下執行應用程式集區的背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="375e5-457">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="375e5-458">在 IIS 管理主控台中，於應用程式集區的 [進階設定] 下，確定 [身分識別] 設定為使用 **ApplicationPoolIdentity**：</span><span class="sxs-lookup"><span data-stu-id="375e5-458">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![應用程式集區進階設定對話方塊](index/_static/apppool-identity.png)

<span data-ttu-id="375e5-460">IIS 管理程序會在 Windows 安全系統中，以應用程式集區的名稱建立安全識別碼。</span><span class="sxs-lookup"><span data-stu-id="375e5-460">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="375e5-461">可使用此身分識別來保護資源。</span><span class="sxs-lookup"><span data-stu-id="375e5-461">Resources can be secured using this identity.</span></span> <span data-ttu-id="375e5-462">不過，此身分識別不是真正的使用者帳戶，也不會顯示在 Windows 使用者管理主控台中。</span><span class="sxs-lookup"><span data-stu-id="375e5-462">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="375e5-463">如果 IIS 背景工作處理序需要提升應用程式的存取權限，請修改包含應用程式的目錄存取控制清單 (ACL)：</span><span class="sxs-lookup"><span data-stu-id="375e5-463">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="375e5-464">開啟 Windows 檔案總管，巡覽至目錄。</span><span class="sxs-lookup"><span data-stu-id="375e5-464">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="375e5-465">以滑鼠右鍵按一下目錄並選取 [屬性]。</span><span class="sxs-lookup"><span data-stu-id="375e5-465">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="375e5-466">依序選取 [安全性] 索引標籤下的 [編輯] 按鈕和 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="375e5-466">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="375e5-467">選取 [位置] 按鈕，並確定選取系統。</span><span class="sxs-lookup"><span data-stu-id="375e5-467">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="375e5-468">在 [輸入要選取的物件名稱] **\\ 區域中，輸入** IIS AppPool **<app_pool_name>** 。</span><span class="sxs-lookup"><span data-stu-id="375e5-468">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="375e5-469">選取 [檢查名稱] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="375e5-469">Select the **Check Names** button.</span></span> <span data-ttu-id="375e5-470">針對 [DefaultAppPool]，請使用 **IIS AppPool\DefaultAppPool** 檢查名稱。</span><span class="sxs-lookup"><span data-stu-id="375e5-470">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="375e5-471">選取 [檢查名稱] 按鈕時，[DefaultAppPool] 的值便會顯示於物件名稱區域中。</span><span class="sxs-lookup"><span data-stu-id="375e5-471">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="375e5-472">您無法直接將應用程式集區名稱輸入至物件名稱區域。</span><span class="sxs-lookup"><span data-stu-id="375e5-472">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="375e5-473">檢查物件名稱時，請使用 **IIS AppPool\\<app_pool_name>** 的格式。</span><span class="sxs-lookup"><span data-stu-id="375e5-473">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：在選取 [檢查名稱] 之前，"DefaultAppPool" 這個應用程式集區名稱在物件名稱區域中會附加至 "IIS AppPool\"。](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="375e5-475">選取 **[確定]** 。</span><span class="sxs-lookup"><span data-stu-id="375e5-475">Select **OK**.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：選取 [檢查名稱] 之後，物件名稱 "DefaultAppPool" 會顯示在物件名稱區域中。](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="375e5-477">預設應該會授與讀取 &amp; 執行權限。</span><span class="sxs-lookup"><span data-stu-id="375e5-477">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="375e5-478">請視需要提供其他權限。</span><span class="sxs-lookup"><span data-stu-id="375e5-478">Provide additional permissions as needed.</span></span>

<span data-ttu-id="375e5-479">也可使用 **ICACLS** 工具透過命令提示字元授與存取權限。</span><span class="sxs-lookup"><span data-stu-id="375e5-479">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="375e5-480">使用 *DefaultAppPool*作為範例，將會使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="375e5-480">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="375e5-481">如需詳細資訊，請參閱 [icacls](/windows-server/administration/windows-commands/icacls) 主題。</span><span class="sxs-lookup"><span data-stu-id="375e5-481">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="http2-support"></a><span data-ttu-id="375e5-482">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="375e5-482">HTTP/2 support</span></span>

<span data-ttu-id="375e5-483">在下列 IIS 部署案例中，ASP.NET Core 支援 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="375e5-483">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following IIS deployment scenarios:</span></span>

* <span data-ttu-id="375e5-484">內含式</span><span class="sxs-lookup"><span data-stu-id="375e5-484">In-process</span></span>
  * <span data-ttu-id="375e5-485">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="375e5-485">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="375e5-486">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="375e5-486">TLS 1.2 or later connection</span></span>
* <span data-ttu-id="375e5-487">跨處理序</span><span class="sxs-lookup"><span data-stu-id="375e5-487">Out-of-process</span></span>
  * <span data-ttu-id="375e5-488">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="375e5-488">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="375e5-489">公開 Edge Server 連線使用 HTTP/2，但是對 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的反向 Proxy 連線使用 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="375e5-489">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
  * <span data-ttu-id="375e5-490">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="375e5-490">TLS 1.2 or later connection</span></span>

<span data-ttu-id="375e5-491">針對建立 HTTP/2 連線時的同處理序部署，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會回報 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="375e5-491">For an in-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span> <span data-ttu-id="375e5-492">針對建立 HTTP/2 連線時的跨處理序部署，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會回報 `HTTP/1.1`。</span><span class="sxs-lookup"><span data-stu-id="375e5-492">For an out-of-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="375e5-493">如需有關同處理序和跨處理序主控模型的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="375e5-493">For more information on the in-process and out-of-process hosting models, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="375e5-494">HTTP/2 預設為啟用。</span><span class="sxs-lookup"><span data-stu-id="375e5-494">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="375e5-495">如果 HTTP/2 連線尚未建立，連線會退為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="375e5-495">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="375e5-496">如需使用 IIS 部署之 HTTP/2 設定的詳細資訊，請參閱 [IIS 上的 HTTP/2](/iis/get-started/whats-new-in-iis-10/http2-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="375e5-496">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="cors-preflight-requests"></a><span data-ttu-id="375e5-497">CORS 預檢要求</span><span class="sxs-lookup"><span data-stu-id="375e5-497">CORS preflight requests</span></span>

<span data-ttu-id="375e5-498">*此節只適用於以 .NET Framework 為目標的 ASP.NET Core 應用程式。*</span><span class="sxs-lookup"><span data-stu-id="375e5-498">*This section only applies to ASP.NET Core apps that target the .NET Framework.*</span></span>

<span data-ttu-id="375e5-499">針對以 .NET Framework 為目標的 ASP.NET Core 應用程式，在 IIS 中OPTIONS 要求預設不會傳遞到應用程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-499">For an ASP.NET Core app that targets the .NET Framework, OPTIONS requests aren't passed to the app by default in IIS.</span></span> <span data-ttu-id="375e5-500">若要瞭解如何在 web.config 中設定應用程式的 IIS 處理*程式來傳遞*選項要求，請參閱[在 ASP.NET Web API 2 中啟用跨原始來源要求： CORS 的運作方式](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works)。</span><span class="sxs-lookup"><span data-stu-id="375e5-500">To learn how to configure the app's IIS handlers in *web.config* to pass OPTIONS requests, see [Enable cross-origin requests in ASP.NET Web API 2: How CORS Works](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).</span></span>

## <a name="application-initialization-module-and-idle-timeout"></a><span data-ttu-id="375e5-501">應用程式初始化模組與閒置逾時</span><span class="sxs-lookup"><span data-stu-id="375e5-501">Application Initialization Module and Idle Timeout</span></span>

<span data-ttu-id="375e5-502">在 IIS 中由 ASP.NET Core 模組版本 2 裝載時：</span><span class="sxs-lookup"><span data-stu-id="375e5-502">When hosted in IIS by the ASP.NET Core Module version 2:</span></span>

* <span data-ttu-id="375e5-503">[應用程式初始化模組](#application-initialization-module)&ndash; 應用程式的裝載同[進程](#in-process-hosting-model)或跨[進程](#out-of-process-hosting-model)，可以設定為在背景工作進程重新開機或伺服器重新開機時自動啟動。</span><span class="sxs-lookup"><span data-stu-id="375e5-503">[Application Initialization Module](#application-initialization-module) &ndash; App's hosted [in-process](#in-process-hosting-model) or [out-of-process](#out-of-process-hosting-model) can be configured to start automatically on a worker process restart or server restart.</span></span>
* <span data-ttu-id="375e5-504">[閒置時間](#idle-timeout)&ndash; 應用程式的裝載同[進程](#in-process-hosting-model)可設定為不會在非活動期間超時。</span><span class="sxs-lookup"><span data-stu-id="375e5-504">[Idle Timeout](#idle-timeout) &ndash; App's hosted [in-process](#in-process-hosting-model) can be configured not to timeout during periods of inactivity.</span></span>

### <a name="application-initialization-module"></a><span data-ttu-id="375e5-505">應用程式初始化模組</span><span class="sxs-lookup"><span data-stu-id="375e5-505">Application Initialization Module</span></span>

<span data-ttu-id="375e5-506">*套用到應用程式裝載同處理序與非同處理序。*</span><span class="sxs-lookup"><span data-stu-id="375e5-506">*Applies to apps hosted in-process and out-of-process.*</span></span>

<span data-ttu-id="375e5-507">[IIS 應用程式初始化](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)是一項 IIS 功能，可在應用程式集區啟動或回收時，將 HTTP 要求傳送給應用程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-507">[IIS Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) is an IIS feature that sends an HTTP request to the app when the app pool starts or is recycled.</span></span> <span data-ttu-id="375e5-508">要求會觸發應用程式啟動。</span><span class="sxs-lookup"><span data-stu-id="375e5-508">The request triggers the app to start.</span></span> <span data-ttu-id="375e5-509">根據預設，IIS 會發出應用程式根 URL (`/`) 的要求以初始化應用程式 (如需有關設定的詳細資訊，請參閱[額外資源](#application-initialization-module-and-idle-timeout-additional-resources))。</span><span class="sxs-lookup"><span data-stu-id="375e5-509">By default, IIS issues a request to the app's root URL (`/`) to initialize the app (see the [additional resources](#application-initialization-module-and-idle-timeout-additional-resources) for more details on configuration).</span></span>

<span data-ttu-id="375e5-510">確認已啟用「IIS 應用程式初始化」角色功能：</span><span class="sxs-lookup"><span data-stu-id="375e5-510">Confirm that the IIS Application Initialization role feature in enabled:</span></span>

<span data-ttu-id="375e5-511">在 Windows 7 或更新的電腦系統上，當在本機使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="375e5-511">On Windows 7 or later desktop systems when using IIS locally:</span></span>

1. <span data-ttu-id="375e5-512">流覽至 [**控制台**] >**程式**> [**程式和功能**] > [**開啟或關閉 Windows 功能**] （畫面左側）。</span><span class="sxs-lookup"><span data-stu-id="375e5-512">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="375e5-513">開啟**Internet Information Services** > **World Wide Web 服務**>**應用程式開發功能**。</span><span class="sxs-lookup"><span data-stu-id="375e5-513">Open **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="375e5-514">選取 [應用程式初始化]的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="375e5-514">Select the check box for **Application Initialization**.</span></span>

<span data-ttu-id="375e5-515">在 Windows Server 2008 R2 或更新版本上：</span><span class="sxs-lookup"><span data-stu-id="375e5-515">On Windows Server 2008 R2 or later:</span></span>

1. <span data-ttu-id="375e5-516">開啟「新增角色與功能精靈」。</span><span class="sxs-lookup"><span data-stu-id="375e5-516">Open the **Add Roles and Features Wizard**.</span></span>
1. <span data-ttu-id="375e5-517">在 [選取角色服務] 面板中，開啟 [應用程式開發] 節點。</span><span class="sxs-lookup"><span data-stu-id="375e5-517">In the **Select role services** panel, open the **Application Development** node.</span></span>
1. <span data-ttu-id="375e5-518">選取 [應用程式初始化]的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="375e5-518">Select the check box for **Application Initialization**.</span></span>

<span data-ttu-id="375e5-519">使用下列任一方式為網站啟用應用程式初始化模組：</span><span class="sxs-lookup"><span data-stu-id="375e5-519">Use either of the following approaches to enable the Application Initialization Module for the site:</span></span>

* <span data-ttu-id="375e5-520">使用 IIS 管理員：</span><span class="sxs-lookup"><span data-stu-id="375e5-520">Using IIS Manager:</span></span>

  1. <span data-ttu-id="375e5-521">選取 [連線] 面板中的 [應用程式集區]。</span><span class="sxs-lookup"><span data-stu-id="375e5-521">Select **Application Pools** in the **Connections** panel.</span></span>
  1. <span data-ttu-id="375e5-522">以滑鼠右鍵按一下清單中應用程式的應用程式集區，然後選取 [進階設定]。</span><span class="sxs-lookup"><span data-stu-id="375e5-522">Right-click the app's app pool in the list and select **Advanced Settings**.</span></span>
  1. <span data-ttu-id="375e5-523">預設的 [啟動模式] 是 [OnDemand]。</span><span class="sxs-lookup"><span data-stu-id="375e5-523">The default **Start Mode** is **OnDemand**.</span></span> <span data-ttu-id="375e5-524">將 [啟動模式] 設定為 [AlwaysRunning]。</span><span class="sxs-lookup"><span data-stu-id="375e5-524">Set the **Start Mode** to **AlwaysRunning**.</span></span> <span data-ttu-id="375e5-525">選取 **[確定]** 。</span><span class="sxs-lookup"><span data-stu-id="375e5-525">Select **OK**.</span></span>
  1. <span data-ttu-id="375e5-526">開啟 [連線] 面板中的 [站台] 節點。</span><span class="sxs-lookup"><span data-stu-id="375e5-526">Open the **Sites** node in the **Connections** panel.</span></span>
  1. <span data-ttu-id="375e5-527">以滑鼠右鍵按一下應用程式，然後選取 [**管理網站**] > [**高級設定**]。</span><span class="sxs-lookup"><span data-stu-id="375e5-527">Right-click the app and select **Manage Website** > **Advanced Settings**.</span></span>
  1. <span data-ttu-id="375e5-528">預設 [預先載入已啟用] 設定是 [False]。</span><span class="sxs-lookup"><span data-stu-id="375e5-528">The default **Preload Enabled** setting is **False**.</span></span> <span data-ttu-id="375e5-529">將 [預先載入已啟用] 設定為 [True]。</span><span class="sxs-lookup"><span data-stu-id="375e5-529">Set **Preload Enabled** to **True**.</span></span> <span data-ttu-id="375e5-530">選取 **[確定]** 。</span><span class="sxs-lookup"><span data-stu-id="375e5-530">Select **OK**.</span></span>

* <span data-ttu-id="375e5-531">使用 *web.config*，新增 `<applicationInitialization>` 元素並將 `doAppInitAfterRestart` 設定為 `true` 至應用程式 `<system.webServer>`web.config*檔案中的* 元素：</span><span class="sxs-lookup"><span data-stu-id="375e5-531">Using *web.config*, add the `<applicationInitialization>` element with `doAppInitAfterRestart` set to `true` to the `<system.webServer>` elements in the app's *web.config* file:</span></span>

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

### <a name="idle-timeout"></a><span data-ttu-id="375e5-532">閒置逾時</span><span class="sxs-lookup"><span data-stu-id="375e5-532">Idle Timeout</span></span>

<span data-ttu-id="375e5-533">*僅適用於應用程式裝載同處理序。*</span><span class="sxs-lookup"><span data-stu-id="375e5-533">*Only applies to apps hosted in-process.*</span></span>

<span data-ttu-id="375e5-534">若要防止應用程式閒置，請使用 IIS 管理員設定應用程式集區的閒置逾時：</span><span class="sxs-lookup"><span data-stu-id="375e5-534">To prevent the app from idling, set the app pool's idle timeout using IIS Manager:</span></span>

1. <span data-ttu-id="375e5-535">選取 [連線] 面板中的 [應用程式集區]。</span><span class="sxs-lookup"><span data-stu-id="375e5-535">Select **Application Pools** in the **Connections** panel.</span></span>
1. <span data-ttu-id="375e5-536">以滑鼠右鍵按一下清單中應用程式的應用程式集區，然後選取 [進階設定]。</span><span class="sxs-lookup"><span data-stu-id="375e5-536">Right-click the app's app pool in the list and select **Advanced Settings**.</span></span>
1. <span data-ttu-id="375e5-537">預設 [閒置逾時 (分鐘)] 是 **20** 分鐘。</span><span class="sxs-lookup"><span data-stu-id="375e5-537">The default **Idle Time-out (minutes)** is **20** minutes.</span></span> <span data-ttu-id="375e5-538">將 [閒置逾時 (分鐘)] 設定為 **0** (零)。</span><span class="sxs-lookup"><span data-stu-id="375e5-538">Set the **Idle Time-out (minutes)** to **0** (zero).</span></span> <span data-ttu-id="375e5-539">選取 **[確定]** 。</span><span class="sxs-lookup"><span data-stu-id="375e5-539">Select **OK**.</span></span>
1. <span data-ttu-id="375e5-540">回收背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="375e5-540">Recycle the worker process.</span></span>

<span data-ttu-id="375e5-541">若要防止應用程式裝載[非同處理序](#out-of-process-hosting-model)逾時，請使用下列任一方式：</span><span class="sxs-lookup"><span data-stu-id="375e5-541">To prevent apps hosted [out-of-process](#out-of-process-hosting-model) from timing out, use either of the following approaches:</span></span>

* <span data-ttu-id="375e5-542">從外部服務對應用程式執行 Ping 以讓它繼續執行。</span><span class="sxs-lookup"><span data-stu-id="375e5-542">Ping the app from an external service in order to keep it running.</span></span>
* <span data-ttu-id="375e5-543">若應用程式只裝載背景服務，避免 IIS 裝載並使用 [Windows 服務來裝載 ASP.NET Core 應用程式](xref:host-and-deploy/windows-service)。</span><span class="sxs-lookup"><span data-stu-id="375e5-543">If the app only hosts background services, avoid IIS hosting and use a [Windows Service to host the ASP.NET Core app](xref:host-and-deploy/windows-service).</span></span>

### <a name="application-initialization-module-and-idle-timeout-additional-resources"></a><span data-ttu-id="375e5-544">應用程式初始化模組與閒置逾時額外資源</span><span class="sxs-lookup"><span data-stu-id="375e5-544">Application Initialization Module and Idle Timeout additional resources</span></span>

* [<span data-ttu-id="375e5-545">IIS 8.0 應用程式初始化</span><span class="sxs-lookup"><span data-stu-id="375e5-545">IIS 8.0 Application Initialization</span></span>](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)
* <span data-ttu-id="375e5-546">[應用程式初始化 \<applicationInitialization>](/iis/configuration/system.webserver/applicationinitialization/)。</span><span class="sxs-lookup"><span data-stu-id="375e5-546">[Application Initialization \<applicationInitialization>](/iis/configuration/system.webserver/applicationinitialization/).</span></span>
* <span data-ttu-id="375e5-547">[應用程式集區的處理序模組設定 \<processModel>](/iis/configuration/system.applicationhost/applicationpools/add/processmodel)。</span><span class="sxs-lookup"><span data-stu-id="375e5-547">[Process Model Settings for an Application Pool \<processModel>](/iis/configuration/system.applicationhost/applicationpools/add/processmodel).</span></span>

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="375e5-548">IIS 系統管理員的部署資源</span><span class="sxs-lookup"><span data-stu-id="375e5-548">Deployment resources for IIS administrators</span></span>

* [<span data-ttu-id="375e5-549">IIS 文件</span><span class="sxs-lookup"><span data-stu-id="375e5-549">IIS documentation</span></span>](/iis)
* [<span data-ttu-id="375e5-550">IIS 中的 IIS 管理員使用者入門</span><span class="sxs-lookup"><span data-stu-id="375e5-550">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* [<span data-ttu-id="375e5-551">.NET Core 應用程式部署</span><span class="sxs-lookup"><span data-stu-id="375e5-551">.NET Core application deployment</span></span>](/dotnet/core/deploying/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/directory-structure>
* <xref:host-and-deploy/iis/modules>
* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>

## <a name="additional-resources"></a><span data-ttu-id="375e5-552">其他資源</span><span class="sxs-lookup"><span data-stu-id="375e5-552">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:index>
* [<span data-ttu-id="375e5-553">Microsoft IIS 官方網站</span><span class="sxs-lookup"><span data-stu-id="375e5-553">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="375e5-554">Windows Server 技術內容庫</span><span class="sxs-lookup"><span data-stu-id="375e5-554">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="375e5-555">IIS 上的 HTTP/2</span><span class="sxs-lookup"><span data-stu-id="375e5-555">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
* <xref:host-and-deploy/iis/transform-webconfig>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="375e5-556">如需將 ASP.NET Core 應用程式發佈至 IIS 伺服器的教學課程體驗，請參閱<xref:tutorials/publish-to-iis>。</span><span class="sxs-lookup"><span data-stu-id="375e5-556">For a tutorial experience on publishing an ASP.NET Core app to an IIS server, see <xref:tutorials/publish-to-iis>.</span></span>

[<span data-ttu-id="375e5-557">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="375e5-557">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="375e5-558">支援的作業系統</span><span class="sxs-lookup"><span data-stu-id="375e5-558">Supported operating systems</span></span>

<span data-ttu-id="375e5-559">以下為支援的作業系統：</span><span class="sxs-lookup"><span data-stu-id="375e5-559">The following operating systems are supported:</span></span>

* <span data-ttu-id="375e5-560">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="375e5-560">Windows 7 or later</span></span>
* <span data-ttu-id="375e5-561">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="375e5-561">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="375e5-562">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys) (先前稱為 WebListener) 不適用搭配 IIS 的反向 Proxy 設定。</span><span class="sxs-lookup"><span data-stu-id="375e5-562">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="375e5-563">請使用 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="375e5-563">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="375e5-564">如需在 Azure 中裝載的資訊，請參閱 <xref:host-and-deploy/azure-apps/index>。</span><span class="sxs-lookup"><span data-stu-id="375e5-564">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

<span data-ttu-id="375e5-565">如需疑難排解指引，請參閱 <xref:test/troubleshoot>。</span><span class="sxs-lookup"><span data-stu-id="375e5-565">For troubleshooting guidance, see <xref:test/troubleshoot>.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="375e5-566">支援的平台</span><span class="sxs-lookup"><span data-stu-id="375e5-566">Supported platforms</span></span>

<span data-ttu-id="375e5-567">支援針對 32 位元 (x86) 或 64 位元 (x64) 部署發行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-567">Apps published for 32-bit (x86) or 64-bit (x64) deployment are supported.</span></span> <span data-ttu-id="375e5-568">使用 32 位元 (x86) .NET Core SDK 部署 32 位元應用程式，除非應用程式：</span><span class="sxs-lookup"><span data-stu-id="375e5-568">Deploy a 32-bit app with a 32-bit (x86) .NET Core SDK unless the app:</span></span>

* <span data-ttu-id="375e5-569">需要提供給 64 位元應用程式使用的較大虛擬記憶體位址空間。</span><span class="sxs-lookup"><span data-stu-id="375e5-569">Requires the larger virtual memory address space available to a 64-bit app.</span></span>
* <span data-ttu-id="375e5-570">需要較大的 IIS 堆疊大小。</span><span class="sxs-lookup"><span data-stu-id="375e5-570">Requires the larger IIS stack size.</span></span>
* <span data-ttu-id="375e5-571">有 64 位元原生相依性。</span><span class="sxs-lookup"><span data-stu-id="375e5-571">Has 64-bit native dependencies.</span></span>

<span data-ttu-id="375e5-572">使用 64 位元 (x64) .NET Core SDK 來發行 64 位元應用程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-572">Use a 64-bit (x64) .NET Core SDK to publish a 64-bit app.</span></span> <span data-ttu-id="375e5-573">主機系統上必須有 64 位元執行階段存在。</span><span class="sxs-lookup"><span data-stu-id="375e5-573">A 64-bit runtime must be present on the host system.</span></span>

## <a name="hosting-models"></a><span data-ttu-id="375e5-574">裝載模型</span><span class="sxs-lookup"><span data-stu-id="375e5-574">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="375e5-575">同處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="375e5-575">In-process hosting model</span></span>

<span data-ttu-id="375e5-576">使用同處理序裝載，ASP.NET Core 應用程式會在與其 IIS 工作者處理序相同的處理序中執行。</span><span class="sxs-lookup"><span data-stu-id="375e5-576">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="375e5-577">因為要求未透過回送介面卡 (將連出網路流量傳回同一部電腦的網路介面) 進行 proxy 處理，所以同處理序裝載會提供優於跨處理序裝載的效能。</span><span class="sxs-lookup"><span data-stu-id="375e5-577">In-process hosting provides improved performance over out-of-process hosting because requests aren't proxied over the loopback adapter, a network interface that returns outgoing network traffic back to the same machine.</span></span> <span data-ttu-id="375e5-578">IIS 透過 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 來執行處理程序管理。</span><span class="sxs-lookup"><span data-stu-id="375e5-578">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="375e5-579">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)：</span><span class="sxs-lookup"><span data-stu-id="375e5-579">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module):</span></span>

* <span data-ttu-id="375e5-580">執行應用程式初始化。</span><span class="sxs-lookup"><span data-stu-id="375e5-580">Performs app initialization.</span></span>
  * <span data-ttu-id="375e5-581">載入 [CoreCLR](/dotnet/standard/glossary#coreclr)。</span><span class="sxs-lookup"><span data-stu-id="375e5-581">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="375e5-582">呼叫 `Program.Main`。</span><span class="sxs-lookup"><span data-stu-id="375e5-582">Calls `Program.Main`.</span></span>
* <span data-ttu-id="375e5-583">處理 IIS 原生要求的存留期。</span><span class="sxs-lookup"><span data-stu-id="375e5-583">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="375e5-584">以 .NET Framework 為目標的 ASP.NET Core 應用程式不支援處理序內裝載模型。</span><span class="sxs-lookup"><span data-stu-id="375e5-584">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="375e5-585">下圖說明 IIS、ASP.NET Core 模組和同處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="375e5-585">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![同處理序代管內的 ASP.NET Core 模組案例](index/_static/ancm-inprocess.png)

<span data-ttu-id="375e5-587">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-587">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="375e5-588">驅動程式會在網站設定的連接埠上將原生要求路由至 IIS，此連接埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="375e5-588">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="375e5-589">ASP.NET Core 模組會接收原生要求，並將它傳遞給 IIS HTTP 伺服器（`IISHttpServer`）。</span><span class="sxs-lookup"><span data-stu-id="375e5-589">The ASP.NET Core Module receives the native request and passes it to IIS HTTP Server (`IISHttpServer`).</span></span> <span data-ttu-id="375e5-590">IIS HTTP 伺服器是 IIS 的同處理序伺服程式實作，可將要求從原生轉換為受控。</span><span class="sxs-lookup"><span data-stu-id="375e5-590">IIS HTTP Server is an in-process server implementation for IIS that converts the request from native to managed.</span></span>

<span data-ttu-id="375e5-591">IIS HTTP 伺服器處理要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="375e5-591">After the IIS HTTP Server processes the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="375e5-592">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="375e5-592">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="375e5-593">應用程式的回應會透過 IIS HTTP 伺服器傳回 IIS。</span><span class="sxs-lookup"><span data-stu-id="375e5-593">The app's response is passed back to IIS through IIS HTTP Server.</span></span> <span data-ttu-id="375e5-594">IIS 會將回應傳送到起始該要求的用戶端。</span><span class="sxs-lookup"><span data-stu-id="375e5-594">IIS sends the response to the client that initiated the request.</span></span>

<span data-ttu-id="375e5-595">現有的應用程式可以選擇同處理序裝載，但 [dotnet new](/dotnet/core/tools/dotnet-new) 範本預設會對所有 IIS 和 IIS Express 案例使用同處理序裝載模型。</span><span class="sxs-lookup"><span data-stu-id="375e5-595">In-process hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="375e5-596">`CreateDefaultBuilder` 會透過呼叫 <xref:Microsoft.AspNetCore.Hosting.Server.IServer> 方法來啟動 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*>CoreCLR[ 以新增 ](/dotnet/standard/glossary#coreclr)，並在 IIS 工作者處理序 (*w3wp.exe* 或 *iisexpress.exe*) 內裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-596">`CreateDefaultBuilder` adds an <xref:Microsoft.AspNetCore.Hosting.Server.IServer> instance by calling the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> method to boot the [CoreCLR](/dotnet/standard/glossary#coreclr) and host the app inside of the IIS worker process (*w3wp.exe* or *iisexpress.exe*).</span></span> <span data-ttu-id="375e5-597">效能測試指出，相較於跨處理序裝載 .NET Core 應用程式，並將要求 Proxy 處理至 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器，裝載於同處理序可提供明顯更高的要求輸送量。</span><span class="sxs-lookup"><span data-stu-id="375e5-597">Performance tests indicate that hosting a .NET Core app in-process delivers significantly higher request throughput compared to hosting the app out-of-process and proxying requests to [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="375e5-598">跨處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="375e5-598">Out-of-process hosting model</span></span>

<span data-ttu-id="375e5-599">因為 ASP.NET Core 應用程式會在與 IIS 背景工作進程不同的進程中執行，所以 ASP.NET Core 模組會處理進程管理。</span><span class="sxs-lookup"><span data-stu-id="375e5-599">Because ASP.NET Core apps run in a process separate from the IIS worker process, the ASP.NET Core Module handles process management.</span></span> <span data-ttu-id="375e5-600">此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式關閉或損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="375e5-600">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="375e5-601">此行為基本上與執行同處理序，並由 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="375e5-601">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="375e5-602">下圖說明 IIS、ASP.NET Core 模組和跨處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="375e5-602">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![非同處理序代管內的 ASP.NET Core 模組案例](index/_static/ancm-outofprocess.png)

<span data-ttu-id="375e5-604">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-604">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="375e5-605">驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="375e5-605">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="375e5-606">此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，而且不會是通訊埠 80 或 443。</span><span class="sxs-lookup"><span data-stu-id="375e5-606">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="375e5-607">此模組在啟動時透過環境變數指定連接埠，而 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 延伸模組則會設定伺服器來接聽 `http://localhost:{PORT}`。</span><span class="sxs-lookup"><span data-stu-id="375e5-607">The module specifies the port via an environment variable at startup, and the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> extension configures the server to listen on `http://localhost:{PORT}`.</span></span> <span data-ttu-id="375e5-608">將會執行額外檢查，不是源自模組的要求都會遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="375e5-608">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="375e5-609">此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。</span><span class="sxs-lookup"><span data-stu-id="375e5-609">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="375e5-610">Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="375e5-610">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="375e5-611">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="375e5-611">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="375e5-612">IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="375e5-612">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="375e5-613">應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="375e5-613">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="375e5-614">如需 ASP.NET Core 模組組態指南，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="375e5-614">For ASP.NET Core Module configuration guidance, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="375e5-615">如需代管的詳細資訊，請參閱[在 ASP.NET Core 中代管](xref:fundamentals/index#host)。</span><span class="sxs-lookup"><span data-stu-id="375e5-615">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/index#host).</span></span>

## <a name="application-configuration"></a><span data-ttu-id="375e5-616">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="375e5-616">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="375e5-617">啟用 IISIntegration 元件</span><span class="sxs-lookup"><span data-stu-id="375e5-617">Enable the IISIntegration components</span></span>

<span data-ttu-id="375e5-618">在 `CreateWebHostBuilder` （*Program.cs*）中建立主機時，請呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 以啟用 IIS 整合：</span><span class="sxs-lookup"><span data-stu-id="375e5-618">When building a host in `CreateWebHostBuilder` (*Program.cs*), call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to enable IIS integration:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

<span data-ttu-id="375e5-619">如需 `CreateDefaultBuilder` 的詳細資訊，請參閱 <xref:fundamentals/host/web-host#set-up-a-host>。</span><span class="sxs-lookup"><span data-stu-id="375e5-619">For more information on `CreateDefaultBuilder`, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

### <a name="iis-options"></a><span data-ttu-id="375e5-620">IIS 選項</span><span class="sxs-lookup"><span data-stu-id="375e5-620">IIS options</span></span>

<span data-ttu-id="375e5-621">**同處理序主控模型**</span><span class="sxs-lookup"><span data-stu-id="375e5-621">**In-process hosting model**</span></span>

<span data-ttu-id="375e5-622">若要設定 IIS 伺服器選項，請在 <xref:Microsoft.AspNetCore.Builder.IISServerOptions> 中加入 <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="375e5-622">To configure IIS Server options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISServerOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="375e5-623">下列範例會停用 AutomaticAuthentication：</span><span class="sxs-lookup"><span data-stu-id="375e5-623">The following example disables AutomaticAuthentication:</span></span>

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

| <span data-ttu-id="375e5-624">選項</span><span class="sxs-lookup"><span data-stu-id="375e5-624">Option</span></span>                         | <span data-ttu-id="375e5-625">預設</span><span class="sxs-lookup"><span data-stu-id="375e5-625">Default</span></span> | <span data-ttu-id="375e5-626">設定</span><span class="sxs-lookup"><span data-stu-id="375e5-626">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="375e5-627">若為 `true`，IIS 伺服器會設定由 `HttpContext.User`Windows 驗證[所驗證的 ](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="375e5-627">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="375e5-628">若為 `false`，則伺服器僅會對 `HttpContext.User` 提供身分識別，並在 `AuthenticationScheme` 明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="375e5-628">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="375e5-629">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="375e5-629">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="375e5-630">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="375e5-630">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="375e5-631">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="375e5-631">Sets the display name shown to users on login pages.</span></span> |

<span data-ttu-id="375e5-632">**跨處理序裝載模型**</span><span class="sxs-lookup"><span data-stu-id="375e5-632">**Out-of-process hosting model**</span></span>

<span data-ttu-id="375e5-633">若要設定 IIS 選項，請在 <xref:Microsoft.AspNetCore.Builder.IISOptions> 中加入 <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="375e5-633">To configure IIS options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="375e5-634">下列範例會防止應用程式填入 `HttpContext.Connection.ClientCertificate`：</span><span class="sxs-lookup"><span data-stu-id="375e5-634">The following example prevents the app from populating `HttpContext.Connection.ClientCertificate`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="375e5-635">選項</span><span class="sxs-lookup"><span data-stu-id="375e5-635">Option</span></span>                         | <span data-ttu-id="375e5-636">預設</span><span class="sxs-lookup"><span data-stu-id="375e5-636">Default</span></span> | <span data-ttu-id="375e5-637">設定</span><span class="sxs-lookup"><span data-stu-id="375e5-637">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="375e5-638">若為 `true`，[IIS 整合中介軟體](#enable-the-iisintegration-components)會設定由 `HttpContext.User`Windows 驗證[所驗證的 ](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="375e5-638">If `true`, [IIS Integration Middleware](#enable-the-iisintegration-components) sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="375e5-639">如果為 `false`，則驗證中介軟體僅針對 `HttpContext.User` 提供身分識別，並在游 `AuthenticationScheme` 提出明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="375e5-639">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="375e5-640">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="375e5-640">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="375e5-641">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="375e5-641">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="375e5-642">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="375e5-642">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="375e5-643">如果為 `true` 且 `MS-ASPNETCORE-CLIENTCERT` 要求標頭已存在，則會填入 `HttpContext.Connection.ClientCertificate`。</span><span class="sxs-lookup"><span data-stu-id="375e5-643">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="375e5-644">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="375e5-644">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="375e5-645">用來設定轉送標頭中介軟體及 ASP.NET Core 模組的 [IIS 整合中介軟體](#enable-the-iisintegration-components)會設定為轉送配置 (HTTP/HTTPS) 與發出要求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="375e5-645">The [IIS Integration Middleware](#enable-the-iisintegration-components), which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="375e5-646">其他 Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。</span><span class="sxs-lookup"><span data-stu-id="375e5-646">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="375e5-647">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="375e5-647">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="375e5-648">web.config 檔案</span><span class="sxs-lookup"><span data-stu-id="375e5-648">web.config file</span></span>

<span data-ttu-id="375e5-649">*web.config* 檔案是用來設定 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="375e5-649">The *web.config* file configures the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="375e5-650">發佈專案時，由 MSBuild 目標 ( *) 處理* web.config`_TransformWebConfig` 檔案的建立、轉換及發佈。</span><span class="sxs-lookup"><span data-stu-id="375e5-650">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="375e5-651">此目標存在於 Web SDK 目標 (`Microsoft.NET.Sdk.Web`)。</span><span class="sxs-lookup"><span data-stu-id="375e5-651">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="375e5-652">SDK 設定在專案檔的頂端：</span><span class="sxs-lookup"><span data-stu-id="375e5-652">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="375e5-653">如果*web.config*檔案不存在於專案中，則會使用正確的*processPath*和*引數*來建立檔案，以設定 ASP.NET Core 模組並移至[已發行的輸出](xref:host-and-deploy/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="375e5-653">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="375e5-654">如果 *web.config* 檔案存在於專案中，則系統會使用正確的 *processPath* 和 *arguments* 來轉換該檔案以設定 ASP.NET Core 模組，然後將它移至已發行的輸出。</span><span class="sxs-lookup"><span data-stu-id="375e5-654">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="375e5-655">轉換不會修改檔案中的 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="375e5-655">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="375e5-656">*web.config* 檔案可提供能控制作用中 IIS 模組的額外 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="375e5-656">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="375e5-657">如需能處理 ASP.NET Core 應用程式要求之 IIS 模組的相關資訊，請參閱 [IIS 模組](xref:host-and-deploy/iis/modules)主題。</span><span class="sxs-lookup"><span data-stu-id="375e5-657">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="375e5-658">為防止 Web SDK 轉換 *web.config* 檔案，請使用專案檔中的 **\<IsTransformWebConfigDisabled>** 屬性：</span><span class="sxs-lookup"><span data-stu-id="375e5-658">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="375e5-659">使 Web SDK 無法轉換檔案時，應該由開發人員手動設定 *processPath* 和 *arguments*。</span><span class="sxs-lookup"><span data-stu-id="375e5-659">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="375e5-660">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="375e5-660">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="375e5-661">web.config 檔案位置</span><span class="sxs-lookup"><span data-stu-id="375e5-661">web.config file location</span></span>

<span data-ttu-id="375e5-662">為了正確設定[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module) *，web.config 檔案*必須存在於已部署應用程式的[內容根](xref:fundamentals/index#content-root)路徑（通常是應用程式基底路徑）。</span><span class="sxs-lookup"><span data-stu-id="375e5-662">In order to set up the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) correctly, the *web.config* file must be present at the [content root](xref:fundamentals/index#content-root) path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="375e5-663">這是與提供給 IIS 的網站實體路徑相同的位置。</span><span class="sxs-lookup"><span data-stu-id="375e5-663">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="375e5-664">應用程式的根目錄需有 *web.config* 檔案，才能使用 Web Deploy 發行多個應用程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-664">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="375e5-665">機密檔案存在於應用程式的實體路徑上，例如 *\<組件>.runtimeconfig.json*、 *\<組件>.xml* (XML 文件註解)，以及 *\<組件>.deps.json*。</span><span class="sxs-lookup"><span data-stu-id="375e5-665">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="375e5-666">當 *web.config* 檔案存在且網站正常啟動時，如果有人要求機密檔案，IIS 不會予以提供。</span><span class="sxs-lookup"><span data-stu-id="375e5-666">When the *web.config* file is present and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="375e5-667">若 *web.config* 檔案遺失或沒有正確命名，或是無法設定網站以正常啟動，IIS 可能會公開提供機密檔案。</span><span class="sxs-lookup"><span data-stu-id="375e5-667">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="375e5-668">\***Web.config*檔案必須隨時存在於部署中、正確命名，而且能夠將網站設定為正常啟動。絕對不要從生產環境部署*移除 web.config 檔案\*。**</span><span class="sxs-lookup"><span data-stu-id="375e5-668">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

### <a name="transform-webconfig"></a><span data-ttu-id="375e5-669">轉換 web.config</span><span class="sxs-lookup"><span data-stu-id="375e5-669">Transform web.config</span></span>

<span data-ttu-id="375e5-670">如需在發佈時轉換 *web.config* (例如依據設定、設定檔或環境設定環境變數)，請參閱<xref:host-and-deploy/iis/transform-webconfig>。</span><span class="sxs-lookup"><span data-stu-id="375e5-670">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="375e5-671">IIS 組態</span><span class="sxs-lookup"><span data-stu-id="375e5-671">IIS configuration</span></span>

<span data-ttu-id="375e5-672">**Windows Server 作業系統**</span><span class="sxs-lookup"><span data-stu-id="375e5-672">**Windows Server operating systems**</span></span>

<span data-ttu-id="375e5-673">啟用**網頁伺服器 (IIS)** 伺服器角色，並建立角色服務。</span><span class="sxs-lookup"><span data-stu-id="375e5-673">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="375e5-674">使用來自 [管理] 功能表的 [新增角色及功能] 精靈，或是 [伺服器管理員] 中的連結。</span><span class="sxs-lookup"><span data-stu-id="375e5-674">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="375e5-675">在**伺服器角色**步驟中，核取 [網頁伺服器 (IIS)] 方塊。</span><span class="sxs-lookup"><span data-stu-id="375e5-675">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![在選取伺服器角色步驟中選取網頁伺服器 IIS 角色。](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="375e5-677">在 [功能] 步驟之後，[角色服務] 步驟會針對網頁伺服器 (IIS) 進行載入。</span><span class="sxs-lookup"><span data-stu-id="375e5-677">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="375e5-678">選取所需的 IIS 角色服務或接受所提供的預設角色服務。</span><span class="sxs-lookup"><span data-stu-id="375e5-678">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![在選取角色服務步驟中，選取預設的角色服務。](index/_static/role-services-ws2016.png)

   <span data-ttu-id="375e5-680">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="375e5-680">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="375e5-681">若要啟用 Windows 驗證，請展開下列節點：[網頁伺服器] > [安全性]。</span><span class="sxs-lookup"><span data-stu-id="375e5-681">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="375e5-682">選取 [Windows 驗證] 功能。</span><span class="sxs-lookup"><span data-stu-id="375e5-682">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="375e5-683">如需詳細資訊，請參閱 [Windows 驗證 \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) 和[設定 Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="375e5-683">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="375e5-684">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="375e5-684">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="375e5-685">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="375e5-685">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="375e5-686">若要啟用 WebSocket，請展開下列節點：[網頁伺服器] > [應用程式開發]。</span><span class="sxs-lookup"><span data-stu-id="375e5-686">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="375e5-687">選取 [WebSocket 通訊協定] 功能。</span><span class="sxs-lookup"><span data-stu-id="375e5-687">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="375e5-688">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="375e5-688">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="375e5-689">透過**確認**步驟繼續作業，安裝網頁伺服器角色和服務。</span><span class="sxs-lookup"><span data-stu-id="375e5-689">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="375e5-690">安裝**網頁伺服器 (IIS)** 角色之後，不需要重新啟動伺服器/IIS。</span><span class="sxs-lookup"><span data-stu-id="375e5-690">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="375e5-691">**Windows 桌面作業系統**</span><span class="sxs-lookup"><span data-stu-id="375e5-691">**Windows desktop operating systems**</span></span>

<span data-ttu-id="375e5-692">啟用 [IIS 管理主控台] 和 [World Wide Web 服務]。</span><span class="sxs-lookup"><span data-stu-id="375e5-692">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="375e5-693">流覽至 [**控制台**] >**程式**> [**程式和功能**] > [**開啟或關閉 Windows 功能**] （畫面左側）。</span><span class="sxs-lookup"><span data-stu-id="375e5-693">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="375e5-694">開啟 [Internet Information Services] 節點。</span><span class="sxs-lookup"><span data-stu-id="375e5-694">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="375e5-695">開啟 [Web 管理工具] 節點。</span><span class="sxs-lookup"><span data-stu-id="375e5-695">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="375e5-696">核取 [IIS 管理主控台] 方塊。</span><span class="sxs-lookup"><span data-stu-id="375e5-696">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="375e5-697">[World Wide Web Services] (全球資訊網服務) 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="375e5-697">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="375e5-698">接受**全球資訊網服務**的預設功能，或自訂 IIS 功能。</span><span class="sxs-lookup"><span data-stu-id="375e5-698">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="375e5-699">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="375e5-699">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="375e5-700">若要啟用 Windows 驗證，請展開下列節點：[World Wide Web 服務] > [安全性]。</span><span class="sxs-lookup"><span data-stu-id="375e5-700">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="375e5-701">選取 [Windows 驗證] 功能。</span><span class="sxs-lookup"><span data-stu-id="375e5-701">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="375e5-702">如需詳細資訊，請參閱 [Windows 驗證 \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) 和[設定 Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="375e5-702">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="375e5-703">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="375e5-703">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="375e5-704">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="375e5-704">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="375e5-705">若要啟用 WebSocket，請展開下列節點：[World Wide Web 服務] > [應用程式開發功能]。</span><span class="sxs-lookup"><span data-stu-id="375e5-705">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="375e5-706">選取 [WebSocket 通訊協定] 功能。</span><span class="sxs-lookup"><span data-stu-id="375e5-706">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="375e5-707">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="375e5-707">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="375e5-708">若 IIS 安裝需要重新啟動，請重新啟動系統。</span><span class="sxs-lookup"><span data-stu-id="375e5-708">If the IIS installation requires a restart, restart the system.</span></span>

![選取 [Windows 功能] 中的 [IIS 管理主控台] 和 [World Wide Web Services] (全球資訊網服務)。](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="375e5-710">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="375e5-710">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="375e5-711">在主控系統上安裝 .NET Core 裝載套件組合。</span><span class="sxs-lookup"><span data-stu-id="375e5-711">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="375e5-712">套件組合會安裝 .NET Core 執行階段、.NET Core 程式庫和 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="375e5-712">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="375e5-713">此模組可讓 ASP.NET Core 應用程式在 IIS 背後執行。</span><span class="sxs-lookup"><span data-stu-id="375e5-713">The module allows ASP.NET Core apps to run behind IIS.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="375e5-714">若裝載套件組合在 IIS 之前安裝，則必須對該套件組合安裝進行修復。</span><span class="sxs-lookup"><span data-stu-id="375e5-714">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="375e5-715">請在安裝 IIS 之後，再次執行裝載套件組合安裝程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-715">Run the Hosting Bundle installer again after installing IIS.</span></span>
>
> <span data-ttu-id="375e5-716">如果在安裝 64 位元 (x64) 版本的 .NET Core 後才安裝裝載套件組合，那麼可能會遺漏 SDK ([未偵測到 .NET Core SDK](xref:test/troubleshoot#no-net-core-sdks-were-detected))。</span><span class="sxs-lookup"><span data-stu-id="375e5-716">If the Hosting Bundle is installed after installing the 64-bit (x64) version of .NET Core, SDKs might appear to be missing ([No .NET Core SDKs were detected](xref:test/troubleshoot#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="375e5-717">若要解決此問題，請參閱 <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>。</span><span class="sxs-lookup"><span data-stu-id="375e5-717">To resolve the problem, see <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>.</span></span>

### <a name="direct-download-current-version"></a><span data-ttu-id="375e5-718">直接下載 (目前版本)</span><span class="sxs-lookup"><span data-stu-id="375e5-718">Direct download (current version)</span></span>

<span data-ttu-id="375e5-719">使用下列連結下載安裝程式：</span><span class="sxs-lookup"><span data-stu-id="375e5-719">Download the installer using the following link:</span></span>

[<span data-ttu-id="375e5-720">目前的 .NET Core 裝載套件組合安裝程式 (直接下載)</span><span class="sxs-lookup"><span data-stu-id="375e5-720">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a><span data-ttu-id="375e5-721">安裝程式的先前版本</span><span class="sxs-lookup"><span data-stu-id="375e5-721">Earlier versions of the installer</span></span>

<span data-ttu-id="375e5-722">若要取得安裝程式的先前版本：</span><span class="sxs-lookup"><span data-stu-id="375e5-722">To obtain an earlier version of the installer:</span></span>

1. <span data-ttu-id="375e5-723">瀏覽至 [.NET 下載封存](https://www.microsoft.com/net/download/archives)。</span><span class="sxs-lookup"><span data-stu-id="375e5-723">Navigate to the [.NET download archives](https://www.microsoft.com/net/download/archives).</span></span>
1. <span data-ttu-id="375e5-724">在 [.NET Core] 下，選取 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="375e5-724">Under **.NET Core**, select the .NET Core version.</span></span>
1. <span data-ttu-id="375e5-725">在 [執行應用程式 - 執行階段] 欄中，尋找想要的 .NET Core 執行階段版本列。</span><span class="sxs-lookup"><span data-stu-id="375e5-725">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="375e5-726">使用**執行階段與裝載套件組合**連結。</span><span class="sxs-lookup"><span data-stu-id="375e5-726">Download the installer using the **Runtime & Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="375e5-727">某些安裝程式包含已達到期生命週期結束 (EOL) 的發行版本，這些發行版本已不受 Microsoft 支援。</span><span class="sxs-lookup"><span data-stu-id="375e5-727">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="375e5-728">如需詳細資訊，請參閱[支援原則](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) \(英文 \)。</span><span class="sxs-lookup"><span data-stu-id="375e5-728">For more information, see the [support policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="375e5-729">安裝裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="375e5-729">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="375e5-730">在伺服器上執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-730">Run the installer on the server.</span></span> <span data-ttu-id="375e5-731">從系統管理員命令殼層執行安裝程式時，有 下列參數可用：</span><span class="sxs-lookup"><span data-stu-id="375e5-731">The following parameters are available when running the installer from an administrator command shell:</span></span>

   * <span data-ttu-id="375e5-732">`OPT_NO_ANCM=1` &ndash; 略過安裝 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="375e5-732">`OPT_NO_ANCM=1` &ndash; Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="375e5-733">`OPT_NO_RUNTIME=1` &ndash; 略過安裝 .NET Core 執行時間。</span><span class="sxs-lookup"><span data-stu-id="375e5-733">`OPT_NO_RUNTIME=1` &ndash; Skip installing the .NET Core runtime.</span></span> <span data-ttu-id="375e5-734">當伺服器只裝載[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)時使用。</span><span class="sxs-lookup"><span data-stu-id="375e5-734">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="375e5-735">`OPT_NO_SHAREDFX=1` &ndash; 略過安裝 ASP.NET 共用架構（ASP.NET 執行時間）。</span><span class="sxs-lookup"><span data-stu-id="375e5-735">`OPT_NO_SHAREDFX=1` &ndash; Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span> <span data-ttu-id="375e5-736">當伺服器只裝載[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)時使用。</span><span class="sxs-lookup"><span data-stu-id="375e5-736">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="375e5-737">`OPT_NO_X86=1` &ndash; 略過安裝 x86 執行時間。</span><span class="sxs-lookup"><span data-stu-id="375e5-737">`OPT_NO_X86=1` &ndash; Skip installing x86 runtimes.</span></span> <span data-ttu-id="375e5-738">當您確定不會裝載 32 位元應用程式時，請使用此參數。</span><span class="sxs-lookup"><span data-stu-id="375e5-738">Use this parameter when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="375e5-739">如果將來有可能同時裝載 32 位元和 64 位元應用程式，請不要使用此參數並安裝這兩個執行階段。</span><span class="sxs-lookup"><span data-stu-id="375e5-739">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this parameter and install both runtimes.</span></span>
   * <span data-ttu-id="375e5-740">`OPT_NO_SHARED_CONFIG_CHECK=1` &ndash; 當共用設定（*applicationhost.config*）位於與 iis 安裝相同的電腦上時，請停用 [使用 iis 共用設定檢查]。</span><span class="sxs-lookup"><span data-stu-id="375e5-740">`OPT_NO_SHARED_CONFIG_CHECK=1` &ndash; Disable the check for using an IIS Shared Configuration when the shared configuration (*applicationHost.config*) is on the same machine as the IIS installation.</span></span> <span data-ttu-id="375e5-741">*只在 ASP.NET Core 2.2 或更新版本的裝載套件組合安裝程式上可用。*</span><span class="sxs-lookup"><span data-stu-id="375e5-741">*Only available for ASP.NET Core 2.2 or later Hosting Bundler installers.*</span></span> <span data-ttu-id="375e5-742">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>。</span><span class="sxs-lookup"><span data-stu-id="375e5-742">For more information, see <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>.</span></span>
1. <span data-ttu-id="375e5-743">重新開機系統，或在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="375e5-743">Restart the system or execute the following commands in a command shell:</span></span>

   ```console
   net stop was /y
   net start w3svc
   ```
   <span data-ttu-id="375e5-744">重新啟動 IIS 將能偵測到由安裝程式對系統路徑 (此為環境變數) 所做出的變更。</span><span class="sxs-lookup"><span data-stu-id="375e5-744">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

<span data-ttu-id="375e5-745">安裝裝載套件組合時，不需要手動停止 IIS 中的個別網站。</span><span class="sxs-lookup"><span data-stu-id="375e5-745">It isn't necessary to manually stop individual sites in IIS when installing the Hosting Bundle.</span></span> <span data-ttu-id="375e5-746">裝載的應用程式（IIS 網站）會在 IIS 重新開機時重新開機。</span><span class="sxs-lookup"><span data-stu-id="375e5-746">Hosted apps (IIS sites) restart when IIS restarts.</span></span> <span data-ttu-id="375e5-747">應用程式會在收到第一個要求時重新開機，包括從[應用程式初始化模組](#application-initialization-module-and-idle-timeout)。</span><span class="sxs-lookup"><span data-stu-id="375e5-747">Apps start up again when they receive their first request, including from the [Application Initialization Module](#application-initialization-module-and-idle-timeout).</span></span>

<span data-ttu-id="375e5-748">ASP.NET Core 採用共用架構封裝修補程式版本的向前復原行為。</span><span class="sxs-lookup"><span data-stu-id="375e5-748">ASP.NET Core adopts roll-forward behavior for patch releases of shared framework packages.</span></span> <span data-ttu-id="375e5-749">當 IIS 所裝載的應用程式使用 IIS 重新開機時，應用程式會在收到第一個要求時，以其所參考套件的最新修補程式版本來載入。</span><span class="sxs-lookup"><span data-stu-id="375e5-749">When apps hosted by IIS restart with IIS, the apps load with the latest patch releases of their referenced packages when they receive their first request.</span></span> <span data-ttu-id="375e5-750">如果未重新開機 IIS，應用程式會在其工作者進程回收時重新開機並展示向前復原行為，並接收其第一個要求。</span><span class="sxs-lookup"><span data-stu-id="375e5-750">If IIS isn't restarted, apps restart and exhibit roll-forward behavior when their worker processes are recycled and they receive their first request.</span></span>

> [!NOTE]
> <span data-ttu-id="375e5-751">如需 IIS 共用組態的資訊，請參閱[使用 IIS 共用組態的 ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)。</span><span class="sxs-lookup"><span data-stu-id="375e5-751">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="375e5-752">使用 Visual Studio 發佈時安裝 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="375e5-752">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="375e5-753">將應用程式部署到具有 [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later) 的伺服器時，請在伺服器上安裝最新版的 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="375e5-753">When deploying apps to servers with [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="375e5-754">若要安裝 Web Deploy，請使用 [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) 或從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=43717)直接取得安裝程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-754">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="375e5-755">慣用的方法是使用 WebPI。</span><span class="sxs-lookup"><span data-stu-id="375e5-755">The preferred method is to use WebPI.</span></span> <span data-ttu-id="375e5-756">WebPI 提供獨立的安裝程式和組態以裝載提供者。</span><span class="sxs-lookup"><span data-stu-id="375e5-756">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="375e5-757">建立 IIS 網站</span><span class="sxs-lookup"><span data-stu-id="375e5-757">Create the IIS site</span></span>

1. <span data-ttu-id="375e5-758">在主控系統上，請建立資料夾以容納應用程式的已發行資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="375e5-758">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="375e5-759">在下列步驟中，您提供資料夾路徑給 IIS，作為應用程式的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="375e5-759">In a following step, the folder's path is provided to IIS as the physical path to the app.</span></span> <span data-ttu-id="375e5-760">如需應用程式之部署資料夾和檔案配置的詳細資訊，請參閱 <xref:host-and-deploy/directory-structure>。</span><span class="sxs-lookup"><span data-stu-id="375e5-760">For more information on an app's deployment folder and file layout, see <xref:host-and-deploy/directory-structure>.</span></span>

1. <span data-ttu-id="375e5-761">在 [IIS 管理員] 中，於 [連線] 面板中開啟伺服器的節點。</span><span class="sxs-lookup"><span data-stu-id="375e5-761">In IIS Manager, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="375e5-762">以滑鼠右鍵按一下 [網站] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="375e5-762">Right-click the **Sites** folder.</span></span> <span data-ttu-id="375e5-763">從操作功能表選取 [新增網站]。</span><span class="sxs-lookup"><span data-stu-id="375e5-763">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="375e5-764">提供**網站名稱**，並將**實體路徑**設定為應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="375e5-764">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="375e5-765">透過選取 [確定] 來提供**繫結**設定並建立網站：</span><span class="sxs-lookup"><span data-stu-id="375e5-765">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![在新增網站步驟中提供站台名稱、實體路徑和主機名稱。](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="375e5-767">請`http://*:80/`勿`http://+:80`使用最上層萬用字元繫結 (**與** )。</span><span class="sxs-lookup"><span data-stu-id="375e5-767">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="375e5-768">最上層萬用字元繫結可能暴露您的應用程式安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="375e5-768">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="375e5-769">這對強式與弱式萬用字元皆適用。</span><span class="sxs-lookup"><span data-stu-id="375e5-769">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="375e5-770">請使用明確主機名稱，而非萬用字元。</span><span class="sxs-lookup"><span data-stu-id="375e5-770">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="375e5-771">若您擁有整個父網域 (與具弱點的 `*.mysub.com` 相對) 的控制權，則子網域萬用字元繫結 (例如 `*.com`) 就沒有此安全性風險。</span><span class="sxs-lookup"><span data-stu-id="375e5-771">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="375e5-772">如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="375e5-772">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="375e5-773">在伺服器的節點之下，選取 [應用程式集區]。</span><span class="sxs-lookup"><span data-stu-id="375e5-773">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="375e5-774">以滑鼠右鍵按一下網站的應用程式集區，然後從操作功能表選取 [基本設定]。</span><span class="sxs-lookup"><span data-stu-id="375e5-774">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="375e5-775">在 [編輯應用程式集區] 視窗中，將 [.NET CLR 版本] 設定為 [沒有受控碼]：</span><span class="sxs-lookup"><span data-stu-id="375e5-775">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![將 .NET CLR 版本設為 [沒有受控碼]。](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="375e5-777">ASP.NET Core 會在不同的處理序中執行，並管理執行階段。</span><span class="sxs-lookup"><span data-stu-id="375e5-777">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="375e5-778">ASP.NET Core 不仰賴載入桌面 CLR (.NET CLR)&mdash;會使用 .NET Core 的核心通用語言執行平台 (CoreCLR) 來開機以在背景工作處理序中裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-778">ASP.NET Core doesn't rely on loading the desktop CLR (.NET CLR)&mdash;the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process.</span></span> <span data-ttu-id="375e5-779">將 [.NET CLR 版本] 設定為 [沒有受控碼] 是選擇性的，但建議這樣做。</span><span class="sxs-lookup"><span data-stu-id="375e5-779">Setting the **.NET CLR version** to **No Managed Code** is optional but recommended.</span></span>

1. <span data-ttu-id="375e5-780">*ASP.NET Core 2.2 或更新版本*：對於使用[同處理序主控模型](/dotnet/core/deploying/#self-contained-deployments-scd)的 64 位元 (x64) [獨立式部署](#in-process-hosting-model)，會停用 32 位元 (x86) 處理序的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="375e5-780">*ASP.NET Core 2.2 or later*: For a 64-bit (x64) [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) that uses the [in-process hosting model](#in-process-hosting-model), disable the app pool for 32-bit (x86) processes.</span></span>

   <span data-ttu-id="375e5-781">在 IIS 管理員的 [動作] 資訊看板 > [應用程式集區] 中，選取 [設定應用程式集區預設值] 或 [進階設定]。</span><span class="sxs-lookup"><span data-stu-id="375e5-781">In the **Actions** sidebar of IIS Manager > **Application Pools**, select **Set Application Pool Defaults** or **Advanced Settings**.</span></span> <span data-ttu-id="375e5-782">找到 [啟用 32 位元應用程式]，然後將其值設定為 `False`。</span><span class="sxs-lookup"><span data-stu-id="375e5-782">Locate **Enable 32-Bit Applications** and set the value to `False`.</span></span> <span data-ttu-id="375e5-783">此設定不會影響為[處理程序外裝載](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model)部署的應用程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-783">This setting doesn't affect apps deployed for [out-of-process hosting](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).</span></span>

1. <span data-ttu-id="375e5-784">確認處理序模型身分識別具有適當的權限。</span><span class="sxs-lookup"><span data-stu-id="375e5-784">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="375e5-785">如果您將應用程式集區的預設身分識別 ([處理序模型] > [身分識別]) 從 **ApplicationPoolIdentity** 變更為其他身分識別，請確認新的身分識別具有必要權限，可存取應用程式的資料夾、資料庫和其他必要的資源。</span><span class="sxs-lookup"><span data-stu-id="375e5-785">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="375e5-786">例如，應用程式集區需要針對應用程式讀取和寫入檔案的資料夾取得讀取和寫入權限。</span><span class="sxs-lookup"><span data-stu-id="375e5-786">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="375e5-787">**Windows 驗證設定 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="375e5-787">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="375e5-788">如需詳細資訊，請參閱[設定 Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="375e5-788">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="375e5-789">部署應用程式</span><span class="sxs-lookup"><span data-stu-id="375e5-789">Deploy the app</span></span>

<span data-ttu-id="375e5-790">將應用程式部署至在**建立 IIS 網站**一節中所建立的 IIS [實體路徑](#create-the-iis-site)資料夾。</span><span class="sxs-lookup"><span data-stu-id="375e5-790">Deploy the app to the IIS **Physical path** folder that was established in the [Create the IIS site](#create-the-iis-site) section.</span></span> <span data-ttu-id="375e5-791">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 是建議的部署機制，但有數個選項可將應用程式從專案的 [publish] 資料夾移動至主機系統的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="375e5-791">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment, but several options exist for moving the app from the project's *publish* folder to the hosting system's deployment folder.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="375e5-792">使用 Visual Studio 的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="375e5-792">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="375e5-793">請參閱[適用於 ASP.NET Core 應用程式部署的 Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)主題，了解如何建立搭配 Web Deploy 使用的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="375e5-793">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="375e5-794">如果主機服務提供者提供或支援建立發行設定檔，請下載其設定檔，並使用 Visual Studio 的 [發行] 對話方塊匯入其設定檔：</span><span class="sxs-lookup"><span data-stu-id="375e5-794">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog:</span></span>

![[發佈] 對話方塊頁](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="375e5-796">Visual Studio 外部的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="375e5-796">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="375e5-797">透過命令列也可以在 Visual Studio 外部使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy)。</span><span class="sxs-lookup"><span data-stu-id="375e5-797">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="375e5-798">如需詳細資訊，請參閱 [Web 部署工具](/iis/publish/using-web-deploy/use-the-web-deployment-tool)。</span><span class="sxs-lookup"><span data-stu-id="375e5-798">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="375e5-799">Web Deploy 的替代項目</span><span class="sxs-lookup"><span data-stu-id="375e5-799">Alternatives to Web Deploy</span></span>

<span data-ttu-id="375e5-800">使用數種方法的其中一種將應用程式移到主機系統，例如手動複製、[Xcopy](/windows-server/administration/windows-commands/xcopy)、[Robocopy](/windows-server/administration/windows-commands/robocopy) 或 [PowerShell](/powershell/)。</span><span class="sxs-lookup"><span data-stu-id="375e5-800">Use any of several methods to move the app to the hosting system, such as manual copy, [Xcopy](/windows-server/administration/windows-commands/xcopy), [Robocopy](/windows-server/administration/windows-commands/robocopy), or [PowerShell](/powershell/).</span></span>

<span data-ttu-id="375e5-801">如需 IIS 的 ASP.NET Core 部署詳細資訊，請參閱 [IIS 系統管理員的部署資源](#deployment-resources-for-iis-administrators)一節。</span><span class="sxs-lookup"><span data-stu-id="375e5-801">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="375e5-802">瀏覽網站</span><span class="sxs-lookup"><span data-stu-id="375e5-802">Browse the website</span></span>

<span data-ttu-id="375e5-803">應用程式部署到主機系統之後，請向其中一個應用程式的公用端點發出要求。</span><span class="sxs-lookup"><span data-stu-id="375e5-803">After the app is deployed to the hosting system, make a request to one of the app's public endpoints.</span></span>

<span data-ttu-id="375e5-804">在下列範例中，網站系結至**埠**`80`上 `www.mysite.com` 的 IIS**主機名稱**。</span><span class="sxs-lookup"><span data-stu-id="375e5-804">In the following example, the site is bound to an IIS **Host name** of `www.mysite.com` on **Port** `80`.</span></span> <span data-ttu-id="375e5-805">對 `http://www.mysite.com` 發出要求：</span><span class="sxs-lookup"><span data-stu-id="375e5-805">A request is made to `http://www.mysite.com`:</span></span>

![Microsoft Edge 瀏覽器已載入 IIS 啟動頁面。](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="375e5-807">已鎖定的部署檔案</span><span class="sxs-lookup"><span data-stu-id="375e5-807">Locked deployment files</span></span>

<span data-ttu-id="375e5-808">當應用程式執行時，會鎖定部署資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="375e5-808">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="375e5-809">無法於部署期間覆寫已鎖定的檔案。</span><span class="sxs-lookup"><span data-stu-id="375e5-809">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="375e5-810">若要釋放部署中的已鎖定檔案，請使用下列其中**一種**方法停止應用程式集區：</span><span class="sxs-lookup"><span data-stu-id="375e5-810">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="375e5-811">使用 Web Deploy 並參考專案檔中的 `Microsoft.NET.Sdk.Web`。</span><span class="sxs-lookup"><span data-stu-id="375e5-811">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="375e5-812">*app_offline.htm* 檔案是放在 Web 應用程式目錄的根目錄中。</span><span class="sxs-lookup"><span data-stu-id="375e5-812">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="375e5-813">當檔案存在時，ASP.NET Core 模組會正常關閉應用程式，並在部署期間提供 *app_offline.htm* 檔案。</span><span class="sxs-lookup"><span data-stu-id="375e5-813">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="375e5-814">如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)。</span><span class="sxs-lookup"><span data-stu-id="375e5-814">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="375e5-815">在伺服器上的 IIS 管理員中手動停止應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="375e5-815">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="375e5-816">使用 PowerShell 卸載*app_offline .htm* （需要 PowerShell 5 或更新版本）：</span><span class="sxs-lookup"><span data-stu-id="375e5-816">Use PowerShell to drop *app_offline.htm* (requires PowerShell 5 or later):</span></span>

  ```powershell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a><span data-ttu-id="375e5-817">資料保護</span><span class="sxs-lookup"><span data-stu-id="375e5-817">Data protection</span></span>

<span data-ttu-id="375e5-818">[ASP.NET Core 資料保護堆疊](xref:security/data-protection/introduction)是由數個 ASP.NET Core [中介軟體](xref:fundamentals/middleware/index)所使用，包括用於驗證的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="375e5-818">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="375e5-819">即使資料保護 API 不是由使用者程式碼呼叫，仍應使用部署指令碼或是在使用者程式碼中設定資料保護，以建立持續性的密碼編譯[金鑰存放區](xref:security/data-protection/implementation/key-management)。</span><span class="sxs-lookup"><span data-stu-id="375e5-819">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="375e5-820">如不設定資料保護，金鑰會保留在記憶體中，並於應用程式重新啟動時捨棄。</span><span class="sxs-lookup"><span data-stu-id="375e5-820">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="375e5-821">如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：</span><span class="sxs-lookup"><span data-stu-id="375e5-821">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="375e5-822">所有以 Cookie 為基礎的驗證權杖都會失效。</span><span class="sxs-lookup"><span data-stu-id="375e5-822">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="375e5-823">當使用者提出下一個要求時，需要再次登入。</span><span class="sxs-lookup"><span data-stu-id="375e5-823">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="375e5-824">所有以 Keyring 保護的資料都無法再解密。</span><span class="sxs-lookup"><span data-stu-id="375e5-824">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="375e5-825">這可能會包含 [CSRF 權杖](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)和 [ASP.NET Core MVC TempData cookie](xref:fundamentals/app-state#tempdata)。</span><span class="sxs-lookup"><span data-stu-id="375e5-825">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="375e5-826">若要在 IIS 下設定資料保護以保存 Keyring，請使用下列其中**一種**方法：</span><span class="sxs-lookup"><span data-stu-id="375e5-826">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="375e5-827">**建立資料保護登錄機碼**</span><span class="sxs-lookup"><span data-stu-id="375e5-827">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="375e5-828">ASP.NET Core 應用程式所使用的資料保護金鑰會儲存在應用程式外部的登錄中。</span><span class="sxs-lookup"><span data-stu-id="375e5-828">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="375e5-829">若要保存指定應用程式的金鑰，請為應用程式集區建立登錄機碼。</span><span class="sxs-lookup"><span data-stu-id="375e5-829">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="375e5-830">若為獨立的非Web 伺服陣列 IIS 安裝，請針對搭配使用 ASP.NET Core 應用程式的每個應用程式集區，使用[資料保護 Provision-AutoGenKeys.ps1 PowerShell 指令碼](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1)。</span><span class="sxs-lookup"><span data-stu-id="375e5-830">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="375e5-831">此指令碼會在 HKLM 登錄中建立登錄機碼，只有應用程式之應用程式集區的背景工作處理序帳戶可以存取它。</span><span class="sxs-lookup"><span data-stu-id="375e5-831">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="375e5-832">在待用期間使用 DPAPI 和全電腦金鑰加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="375e5-832">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="375e5-833">在 Web 伺服陣列案例中，應用程式可以設定成使用 UNC 路徑來儲存其資料保護 Keyring。</span><span class="sxs-lookup"><span data-stu-id="375e5-833">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="375e5-834">根據預設，資料保護金鑰不予加密。</span><span class="sxs-lookup"><span data-stu-id="375e5-834">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="375e5-835">請確保網路共用的檔案權限僅限於執行應用程式的 Windows 帳戶。</span><span class="sxs-lookup"><span data-stu-id="375e5-835">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="375e5-836">可以使用 X509 憑證來保護待用的金鑰。</span><span class="sxs-lookup"><span data-stu-id="375e5-836">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="375e5-837">請考慮下列讓使用者上傳憑證的機制：將憑證放入使用者的受信任憑證存放區，並確保在執行使用者應用程式的所有電腦上都能使用這些憑證。</span><span class="sxs-lookup"><span data-stu-id="375e5-837">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="375e5-838">如需詳細資訊，請參閱[設定 ASP.NET Core 資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="375e5-838">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="375e5-839">**設定 IIS 應用程式集區載入使用者設定檔**</span><span class="sxs-lookup"><span data-stu-id="375e5-839">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="375e5-840">此設定位在應用程式集區 [進階設定] 下的 [處理序模型] 區段中。</span><span class="sxs-lookup"><span data-stu-id="375e5-840">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="375e5-841">將 [載入使用者設定檔] 設為 `True`。</span><span class="sxs-lookup"><span data-stu-id="375e5-841">Set **Load User Profile** to `True`.</span></span> <span data-ttu-id="375e5-842">當設定為 `True` 時，金鑰會儲存在使用者設定檔目錄中，且使用具有使用者帳戶專屬金鑰的 DPAPI 保護。</span><span class="sxs-lookup"><span data-stu-id="375e5-842">When set to `True`, keys are stored in the user profile directory and protected using DPAPI with a key specific to the user account.</span></span> <span data-ttu-id="375e5-843">金鑰會保存到 *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="375e5-843">Keys are persisted to the *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* folder.</span></span>

  <span data-ttu-id="375e5-844">應用程式集區的 [setProfileEnvironment 屬性](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration)也必須啟用。</span><span class="sxs-lookup"><span data-stu-id="375e5-844">The app pool's [setProfileEnvironment attribute](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) must also be enabled.</span></span> <span data-ttu-id="375e5-845">`setProfileEnvironment` 的預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="375e5-845">The default value of `setProfileEnvironment` is `true`.</span></span> <span data-ttu-id="375e5-846">在某些情況下 (例如 Windows OS)，`setProfileEnvironment` 會設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="375e5-846">In some scenarios (for example, Windows OS), `setProfileEnvironment` is set to `false`.</span></span> <span data-ttu-id="375e5-847">如果金鑰並未如預期地儲存在使用者設定檔目錄中：</span><span class="sxs-lookup"><span data-stu-id="375e5-847">If keys aren't stored in the user profile directory as expected:</span></span>

  1. <span data-ttu-id="375e5-848">瀏覽至 *%windir%/system32/inetsrv/config* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="375e5-848">Navigate to the *%windir%/system32/inetsrv/config* folder.</span></span>
  1. <span data-ttu-id="375e5-849">開啟 *applicationHost.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="375e5-849">Open the *applicationHost.config* file.</span></span>
  1. <span data-ttu-id="375e5-850">找出 `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` 元素。</span><span class="sxs-lookup"><span data-stu-id="375e5-850">Locate the `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` element.</span></span>
  1. <span data-ttu-id="375e5-851">確認 `setProfileEnvironment` 屬性不存在 (其預設值為 `true`)，或明確地將屬性值設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="375e5-851">Confirm that the `setProfileEnvironment` attribute isn't present, which defaults the value to `true`, or explicitly set the attribute's value to `true`.</span></span>

* <span data-ttu-id="375e5-852">**將檔案系統當作 Keyring 存放區使用**</span><span class="sxs-lookup"><span data-stu-id="375e5-852">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="375e5-853">調整應用程式程式碼，以[將檔案系統當作 Keyring 存放區使用](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="375e5-853">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="375e5-854">使用 X509 憑證來保護 Keyring，並確保憑證是受信任的憑證。</span><span class="sxs-lookup"><span data-stu-id="375e5-854">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="375e5-855">如果憑證為自我簽署，請將憑證放在受信任的根存放區。</span><span class="sxs-lookup"><span data-stu-id="375e5-855">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="375e5-856">在 Web 伺服陣列中使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="375e5-856">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="375e5-857">請使用所有電腦都可以存取的檔案共用。</span><span class="sxs-lookup"><span data-stu-id="375e5-857">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="375e5-858">將 X509 憑證部署到每一部電腦。</span><span class="sxs-lookup"><span data-stu-id="375e5-858">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="375e5-859">設定[程式碼中的資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="375e5-859">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="375e5-860">**設定資料保護的全電腦原則**</span><span class="sxs-lookup"><span data-stu-id="375e5-860">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="375e5-861">針對取用資料保護 API 的所有應用程式，資料保護系統僅支援有限的預設[全電腦原則](xref:security/data-protection/configuration/machine-wide-policy)設定。</span><span class="sxs-lookup"><span data-stu-id="375e5-861">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="375e5-862">如需詳細資訊，請參閱 <xref:security/data-protection/introduction>。</span><span class="sxs-lookup"><span data-stu-id="375e5-862">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="virtual-directories"></a><span data-ttu-id="375e5-863">虛擬目錄</span><span class="sxs-lookup"><span data-stu-id="375e5-863">Virtual Directories</span></span>

<span data-ttu-id="375e5-864">ASP.NET Core 應用程式不支援 [IIS 虛擬目錄](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories)。</span><span class="sxs-lookup"><span data-stu-id="375e5-864">[IIS Virtual Directories](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) aren't supported with ASP.NET Core apps.</span></span> <span data-ttu-id="375e5-865">應用程式能以[子應用程式](#sub-applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="375e5-865">An app can be hosted as a [sub-application](#sub-applications).</span></span>

## <a name="sub-applications"></a><span data-ttu-id="375e5-866">子應用程式</span><span class="sxs-lookup"><span data-stu-id="375e5-866">Sub-applications</span></span>

<span data-ttu-id="375e5-867">ASP.NET Core 應用程式能以 [IIS 子應用程式](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="375e5-867">An ASP.NET Core app can be hosted as an [IIS sub-application (sub-app)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications).</span></span> <span data-ttu-id="375e5-868">子應用程式的路徑會成為根應用程式 URL 的一部分。</span><span class="sxs-lookup"><span data-stu-id="375e5-868">The sub-app's path becomes part of the root app's URL.</span></span>

<span data-ttu-id="375e5-869">子應用程式內的靜態資產連結應該使用波狀符號與斜線 (`~/`) 標記法。</span><span class="sxs-lookup"><span data-stu-id="375e5-869">Static asset links within the sub-app should use tilde-slash (`~/`) notation.</span></span> <span data-ttu-id="375e5-870">波狀符號與斜線標記法會觸發[標記協助程式](xref:mvc/views/tag-helpers/intro)以將子應用程式的路徑基底附加到轉譯的相對連結前面。</span><span class="sxs-lookup"><span data-stu-id="375e5-870">Tilde-slash notation triggers a [Tag Helper](xref:mvc/views/tag-helpers/intro) to prepend the sub-app's pathbase to the rendered relative link.</span></span> <span data-ttu-id="375e5-871">針對位於 `/subapp_path` 的子應用程式，使用 `src="~/image.png"` 連結的影像會轉譯為 `src="/subapp_path/image.png"`。</span><span class="sxs-lookup"><span data-stu-id="375e5-871">For a sub-app at `/subapp_path`, an image linked with `src="~/image.png"` is rendered as `src="/subapp_path/image.png"`.</span></span> <span data-ttu-id="375e5-872">根應用程式的靜態檔案中介軟體不會處理靜態檔案要求。</span><span class="sxs-lookup"><span data-stu-id="375e5-872">The root app's Static File Middleware doesn't process the static file request.</span></span> <span data-ttu-id="375e5-873">要求會由子應用程式的靜態檔案中介軟體處理。</span><span class="sxs-lookup"><span data-stu-id="375e5-873">The request is processed by the sub-app's Static File Middleware.</span></span>

<span data-ttu-id="375e5-874">若靜態資產的 `src` 屬性是設定為絕對路徑 (例如，`src="/image.png"`)，會以不使用子應用程式路徑基底的方式轉譯連結。</span><span class="sxs-lookup"><span data-stu-id="375e5-874">If a static asset's `src` attribute is set to an absolute path (for example, `src="/image.png"`), the link is rendered without the sub-app's pathbase.</span></span> <span data-ttu-id="375e5-875">根應用程式的靜態檔案中介軟體會嘗試從根應用程式的 [webroot](xref:fundamentals/index#web-root) 提供資產，這會導致「404 - 找不到」回應 (除非根應用程式可存取靜態資產)。</span><span class="sxs-lookup"><span data-stu-id="375e5-875">The root app's Static File Middleware attempts to serve the asset from the root app's [web root](xref:fundamentals/index#web-root), which results in a *404 - Not Found* response unless the static asset is available from the root app.</span></span>

<span data-ttu-id="375e5-876">裝載 ASP.NET Core 應用程式做為另一個 ASP.NET Core 應用程式下的子應用程式：</span><span class="sxs-lookup"><span data-stu-id="375e5-876">To host an ASP.NET Core app as a sub-app under another ASP.NET Core app:</span></span>

1. <span data-ttu-id="375e5-877">為子應用程式建立應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="375e5-877">Establish an app pool for the sub-app.</span></span> <span data-ttu-id="375e5-878">將 [.NET CLR 版本] 設定為 [沒有受控碼]，因為會將核心通用語言執行平台 (CoreCLR) 開機以在背景工作處理序中裝載應用程式，而非在桌面 CLR (.NET CLR) 中裝載。</span><span class="sxs-lookup"><span data-stu-id="375e5-878">Set the **.NET CLR Version** to **No Managed Code** because the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process, not the desktop CLR (.NET CLR).</span></span>

1. <span data-ttu-id="375e5-879">使用根網站下資料夾中的子應用程式在 IIS 管理員中新增根網站。</span><span class="sxs-lookup"><span data-stu-id="375e5-879">Add the root site in IIS Manager with the sub-app in a folder under the root site.</span></span>

1. <span data-ttu-id="375e5-880">以滑鼠右鍵按一下 IIS 管理員中的子應用程式資料夾，然後選取 [轉換成應用程式]。</span><span class="sxs-lookup"><span data-stu-id="375e5-880">Right-click the sub-app folder in IIS Manager and select **Convert to Application**.</span></span>

1. <span data-ttu-id="375e5-881">在 [新增應用程式] 對話方塊中，使用 [應用程式集區] 的[選取] 按鈕來指派您為子應用程式建立的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="375e5-881">In the **Add Application** dialog, use the **Select** button for the **Application Pool** to assign the app pool that you created for the sub-app.</span></span> <span data-ttu-id="375e5-882">選取 **[確定]** 。</span><span class="sxs-lookup"><span data-stu-id="375e5-882">Select **OK**.</span></span>

<span data-ttu-id="375e5-883">將不同的應用程式集區指派給子應用程式是使用同處理序裝載模型。</span><span class="sxs-lookup"><span data-stu-id="375e5-883">The assignment of a separate app pool to the sub-app is a requirement when using the in-process hosting model.</span></span>

<span data-ttu-id="375e5-884">如需有關同進程裝載模型和設定 ASP.NET Core 模組的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="375e5-884">For more information on the in-process hosting model and configuring the ASP.NET Core Module, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="375e5-885">使用 web.config 的 IIS 組態</span><span class="sxs-lookup"><span data-stu-id="375e5-885">Configuration of IIS with web.config</span></span>

<span data-ttu-id="375e5-886">在對使用了 ASP.NET Core 模組的 ASP.NET Core 有作用的 IIS 情境下，設定會受 `<system.webServer>`web.config*的* 區段影響。</span><span class="sxs-lookup"><span data-stu-id="375e5-886">IIS configuration is influenced by the `<system.webServer>` section of *web.config* for IIS scenarios that are functional for ASP.NET Core apps with the ASP.NET Core Module.</span></span> <span data-ttu-id="375e5-887">舉例來說，IIS 設定對動態壓縮有作用。</span><span class="sxs-lookup"><span data-stu-id="375e5-887">For example, IIS configuration is functional for dynamic compression.</span></span> <span data-ttu-id="375e5-888">如果在伺服器層級將 IIS 設為使用動態壓縮，應用程式 `<urlCompression>`web.config*檔案中的* 元素則可為 ASP.NET Core 應用程式予以停用。</span><span class="sxs-lookup"><span data-stu-id="375e5-888">If IIS is configured at the server level to use dynamic compression, the `<urlCompression>` element in the app's *web.config* file can disable it for an ASP.NET Core app.</span></span>

<span data-ttu-id="375e5-889">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="375e5-889">For more information, see the following topics:</span></span>

* [<span data-ttu-id="375e5-890">\<System.webserver > 的設定參考</span><span class="sxs-lookup"><span data-stu-id="375e5-890">Configuration reference for \<system.webServer></span></span>](/iis/configuration/system.webServer/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>

<span data-ttu-id="375e5-891">若要設定在隔離的應用程式集區中執行之個別應用程式的環境變數 (支援 IIS 10.0 或更新版本)，請參閱 IIS 參考文件之*環境變數* environmentVariables>[ 主題的 \<AppCmd.exe 命令](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe)一節。</span><span class="sxs-lookup"><span data-stu-id="375e5-891">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="375e5-892">web.config 的組態區段</span><span class="sxs-lookup"><span data-stu-id="375e5-892">Configuration sections of web.config</span></span>

<span data-ttu-id="375e5-893">ASP.NET Core 應用程式的設定不使用 *web.config* 中 ASP.NET 4.x 應用程式的設定區段：</span><span class="sxs-lookup"><span data-stu-id="375e5-893">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

<span data-ttu-id="375e5-894">使用其他組態提供者設定的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-894">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="375e5-895">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="375e5-895">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="375e5-896">應用程式集區</span><span class="sxs-lookup"><span data-stu-id="375e5-896">Application Pools</span></span>

<span data-ttu-id="375e5-897">應用程式集區隔離取決於裝載模型：</span><span class="sxs-lookup"><span data-stu-id="375e5-897">App pool isolation is determined by the hosting model:</span></span>

* <span data-ttu-id="375e5-898">處理序內裝載 &ndash; 應用程式必須在分開的應用程式集區中執行。</span><span class="sxs-lookup"><span data-stu-id="375e5-898">In-process hosting &ndash; Apps are required to run in separate app pools.</span></span>
* <span data-ttu-id="375e5-899">處理序外裝載 &ndash; 建議藉由在各自的應用程式集區中執行每個應用程式，以將應用程式互相隔離。</span><span class="sxs-lookup"><span data-stu-id="375e5-899">Out-of-process hosting &ndash; We recommend isolating the apps from each other by running each app in its own app pool.</span></span>

<span data-ttu-id="375e5-900">IIS [新增網站] 對話方塊預設每個應用程式皆為單一應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="375e5-900">The IIS **Add Website** dialog defaults to a single app pool per app.</span></span> <span data-ttu-id="375e5-901">當提供 [網站名稱] 時，文字會自動轉移至 [應用程式集區] 文字方塊。</span><span class="sxs-lookup"><span data-stu-id="375e5-901">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="375e5-902">新增網站時，會使用該網站名稱建立新的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="375e5-902">A new app pool is created using the site name when the site is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="375e5-903">應用程式集區識別碼</span><span class="sxs-lookup"><span data-stu-id="375e5-903">Application Pool Identity</span></span>

<span data-ttu-id="375e5-904">應用程式集區身分識別帳戶可讓應用程式在唯一的帳戶下執行，不必建立及管理網域或本機帳戶。</span><span class="sxs-lookup"><span data-stu-id="375e5-904">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="375e5-905">在 IIS 8.0 或更新版本中，IIS 管理背景工作處理序 (WAS) 會使用新的應用程式集區名稱建立虛擬帳戶，並預設在此帳戶下執行應用程式集區的背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="375e5-905">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="375e5-906">在 IIS 管理主控台中，於應用程式集區的 [進階設定] 下，確定 [身分識別] 設定為使用 **ApplicationPoolIdentity**：</span><span class="sxs-lookup"><span data-stu-id="375e5-906">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![應用程式集區進階設定對話方塊](index/_static/apppool-identity.png)

<span data-ttu-id="375e5-908">IIS 管理程序會在 Windows 安全系統中，以應用程式集區的名稱建立安全識別碼。</span><span class="sxs-lookup"><span data-stu-id="375e5-908">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="375e5-909">可使用此身分識別來保護資源。</span><span class="sxs-lookup"><span data-stu-id="375e5-909">Resources can be secured using this identity.</span></span> <span data-ttu-id="375e5-910">不過，此身分識別不是真正的使用者帳戶，也不會顯示在 Windows 使用者管理主控台中。</span><span class="sxs-lookup"><span data-stu-id="375e5-910">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="375e5-911">如果 IIS 背景工作處理序需要提升應用程式的存取權限，請修改包含應用程式的目錄存取控制清單 (ACL)：</span><span class="sxs-lookup"><span data-stu-id="375e5-911">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="375e5-912">開啟 Windows 檔案總管，巡覽至目錄。</span><span class="sxs-lookup"><span data-stu-id="375e5-912">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="375e5-913">以滑鼠右鍵按一下目錄並選取 [屬性]。</span><span class="sxs-lookup"><span data-stu-id="375e5-913">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="375e5-914">依序選取 [安全性] 索引標籤下的 [編輯] 按鈕和 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="375e5-914">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="375e5-915">選取 [位置] 按鈕，並確定選取系統。</span><span class="sxs-lookup"><span data-stu-id="375e5-915">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="375e5-916">在 [輸入要選取的物件名稱] **\\ 區域中，輸入** IIS AppPool **<app_pool_name>** 。</span><span class="sxs-lookup"><span data-stu-id="375e5-916">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="375e5-917">選取 [檢查名稱] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="375e5-917">Select the **Check Names** button.</span></span> <span data-ttu-id="375e5-918">針對 [DefaultAppPool]，請使用 **IIS AppPool\DefaultAppPool** 檢查名稱。</span><span class="sxs-lookup"><span data-stu-id="375e5-918">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="375e5-919">選取 [檢查名稱] 按鈕時，[DefaultAppPool] 的值便會顯示於物件名稱區域中。</span><span class="sxs-lookup"><span data-stu-id="375e5-919">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="375e5-920">您無法直接將應用程式集區名稱輸入至物件名稱區域。</span><span class="sxs-lookup"><span data-stu-id="375e5-920">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="375e5-921">檢查物件名稱時，請使用 **IIS AppPool\\<app_pool_name>** 的格式。</span><span class="sxs-lookup"><span data-stu-id="375e5-921">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：在選取 [檢查名稱] 之前，"DefaultAppPool" 這個應用程式集區名稱在物件名稱區域中會附加至 "IIS AppPool\"。](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="375e5-923">選取 **[確定]** 。</span><span class="sxs-lookup"><span data-stu-id="375e5-923">Select **OK**.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：選取 [檢查名稱] 之後，物件名稱 "DefaultAppPool" 會顯示在物件名稱區域中。](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="375e5-925">預設應該會授與讀取 &amp; 執行權限。</span><span class="sxs-lookup"><span data-stu-id="375e5-925">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="375e5-926">請視需要提供其他權限。</span><span class="sxs-lookup"><span data-stu-id="375e5-926">Provide additional permissions as needed.</span></span>

<span data-ttu-id="375e5-927">也可使用 **ICACLS** 工具透過命令提示字元授與存取權限。</span><span class="sxs-lookup"><span data-stu-id="375e5-927">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="375e5-928">使用 *DefaultAppPool*作為範例，將會使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="375e5-928">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="375e5-929">如需詳細資訊，請參閱 [icacls](/windows-server/administration/windows-commands/icacls) 主題。</span><span class="sxs-lookup"><span data-stu-id="375e5-929">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="http2-support"></a><span data-ttu-id="375e5-930">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="375e5-930">HTTP/2 support</span></span>

<span data-ttu-id="375e5-931">在下列 IIS 部署案例中，ASP.NET Core 支援 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="375e5-931">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following IIS deployment scenarios:</span></span>

* <span data-ttu-id="375e5-932">內含式</span><span class="sxs-lookup"><span data-stu-id="375e5-932">In-process</span></span>
  * <span data-ttu-id="375e5-933">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="375e5-933">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="375e5-934">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="375e5-934">TLS 1.2 or later connection</span></span>
* <span data-ttu-id="375e5-935">跨處理序</span><span class="sxs-lookup"><span data-stu-id="375e5-935">Out-of-process</span></span>
  * <span data-ttu-id="375e5-936">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="375e5-936">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="375e5-937">公開 Edge Server 連線使用 HTTP/2，但是對 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的反向 Proxy 連線使用 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="375e5-937">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
  * <span data-ttu-id="375e5-938">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="375e5-938">TLS 1.2 or later connection</span></span>

<span data-ttu-id="375e5-939">針對建立 HTTP/2 連線時的同處理序部署，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會回報 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="375e5-939">For an in-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span> <span data-ttu-id="375e5-940">針對建立 HTTP/2 連線時的跨處理序部署，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會回報 `HTTP/1.1`。</span><span class="sxs-lookup"><span data-stu-id="375e5-940">For an out-of-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="375e5-941">如需有關同處理序和跨處理序主控模型的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="375e5-941">For more information on the in-process and out-of-process hosting models, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="375e5-942">HTTP/2 預設為啟用。</span><span class="sxs-lookup"><span data-stu-id="375e5-942">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="375e5-943">如果 HTTP/2 連線尚未建立，連線會退為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="375e5-943">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="375e5-944">如需使用 IIS 部署之 HTTP/2 設定的詳細資訊，請參閱 [IIS 上的 HTTP/2](/iis/get-started/whats-new-in-iis-10/http2-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="375e5-944">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="cors-preflight-requests"></a><span data-ttu-id="375e5-945">CORS 預檢要求</span><span class="sxs-lookup"><span data-stu-id="375e5-945">CORS preflight requests</span></span>

<span data-ttu-id="375e5-946">*此節只適用於以 .NET Framework 為目標的 ASP.NET Core 應用程式。*</span><span class="sxs-lookup"><span data-stu-id="375e5-946">*This section only applies to ASP.NET Core apps that target the .NET Framework.*</span></span>

<span data-ttu-id="375e5-947">針對以 .NET Framework 為目標的 ASP.NET Core 應用程式，在 IIS 中OPTIONS 要求預設不會傳遞到應用程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-947">For an ASP.NET Core app that targets the .NET Framework, OPTIONS requests aren't passed to the app by default in IIS.</span></span> <span data-ttu-id="375e5-948">若要瞭解如何在 web.config 中設定應用程式的 IIS 處理*程式來傳遞*選項要求，請參閱[在 ASP.NET Web API 2 中啟用跨原始來源要求： CORS 的運作方式](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works)。</span><span class="sxs-lookup"><span data-stu-id="375e5-948">To learn how to configure the app's IIS handlers in *web.config* to pass OPTIONS requests, see [Enable cross-origin requests in ASP.NET Web API 2: How CORS Works](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).</span></span>

## <a name="application-initialization-module-and-idle-timeout"></a><span data-ttu-id="375e5-949">應用程式初始化模組與閒置逾時</span><span class="sxs-lookup"><span data-stu-id="375e5-949">Application Initialization Module and Idle Timeout</span></span>

<span data-ttu-id="375e5-950">在 IIS 中由 ASP.NET Core 模組版本 2 裝載時：</span><span class="sxs-lookup"><span data-stu-id="375e5-950">When hosted in IIS by the ASP.NET Core Module version 2:</span></span>

* <span data-ttu-id="375e5-951">[應用程式初始化模組](#application-initialization-module)&ndash; 應用程式的裝載同[進程](#in-process-hosting-model)或跨[進程](#out-of-process-hosting-model)，可以設定為在背景工作進程重新開機或伺服器重新開機時自動啟動。</span><span class="sxs-lookup"><span data-stu-id="375e5-951">[Application Initialization Module](#application-initialization-module) &ndash; App's hosted [in-process](#in-process-hosting-model) or [out-of-process](#out-of-process-hosting-model) can be configured to start automatically on a worker process restart or server restart.</span></span>
* <span data-ttu-id="375e5-952">[閒置時間](#idle-timeout)&ndash; 應用程式的裝載同[進程](#in-process-hosting-model)可設定為不會在非活動期間超時。</span><span class="sxs-lookup"><span data-stu-id="375e5-952">[Idle Timeout](#idle-timeout) &ndash; App's hosted [in-process](#in-process-hosting-model) can be configured not to timeout during periods of inactivity.</span></span>

### <a name="application-initialization-module"></a><span data-ttu-id="375e5-953">應用程式初始化模組</span><span class="sxs-lookup"><span data-stu-id="375e5-953">Application Initialization Module</span></span>

<span data-ttu-id="375e5-954">*套用到應用程式裝載同處理序與非同處理序。*</span><span class="sxs-lookup"><span data-stu-id="375e5-954">*Applies to apps hosted in-process and out-of-process.*</span></span>

<span data-ttu-id="375e5-955">[IIS 應用程式初始化](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)是一項 IIS 功能，可在應用程式集區啟動或回收時，將 HTTP 要求傳送給應用程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-955">[IIS Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) is an IIS feature that sends an HTTP request to the app when the app pool starts or is recycled.</span></span> <span data-ttu-id="375e5-956">要求會觸發應用程式啟動。</span><span class="sxs-lookup"><span data-stu-id="375e5-956">The request triggers the app to start.</span></span> <span data-ttu-id="375e5-957">根據預設，IIS 會發出應用程式根 URL (`/`) 的要求以初始化應用程式 (如需有關設定的詳細資訊，請參閱[額外資源](#application-initialization-module-and-idle-timeout-additional-resources))。</span><span class="sxs-lookup"><span data-stu-id="375e5-957">By default, IIS issues a request to the app's root URL (`/`) to initialize the app (see the [additional resources](#application-initialization-module-and-idle-timeout-additional-resources) for more details on configuration).</span></span>

<span data-ttu-id="375e5-958">確認已啟用「IIS 應用程式初始化」角色功能：</span><span class="sxs-lookup"><span data-stu-id="375e5-958">Confirm that the IIS Application Initialization role feature in enabled:</span></span>

<span data-ttu-id="375e5-959">在 Windows 7 或更新的電腦系統上，當在本機使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="375e5-959">On Windows 7 or later desktop systems when using IIS locally:</span></span>

1. <span data-ttu-id="375e5-960">流覽至 [**控制台**] >**程式**> [**程式和功能**] > [**開啟或關閉 Windows 功能**] （畫面左側）。</span><span class="sxs-lookup"><span data-stu-id="375e5-960">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="375e5-961">開啟**Internet Information Services** > **World Wide Web 服務**>**應用程式開發功能**。</span><span class="sxs-lookup"><span data-stu-id="375e5-961">Open **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="375e5-962">選取 [應用程式初始化]的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="375e5-962">Select the check box for **Application Initialization**.</span></span>

<span data-ttu-id="375e5-963">在 Windows Server 2008 R2 或更新版本上：</span><span class="sxs-lookup"><span data-stu-id="375e5-963">On Windows Server 2008 R2 or later:</span></span>

1. <span data-ttu-id="375e5-964">開啟「新增角色與功能精靈」。</span><span class="sxs-lookup"><span data-stu-id="375e5-964">Open the **Add Roles and Features Wizard**.</span></span>
1. <span data-ttu-id="375e5-965">在 [選取角色服務] 面板中，開啟 [應用程式開發] 節點。</span><span class="sxs-lookup"><span data-stu-id="375e5-965">In the **Select role services** panel, open the **Application Development** node.</span></span>
1. <span data-ttu-id="375e5-966">選取 [應用程式初始化]的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="375e5-966">Select the check box for **Application Initialization**.</span></span>

<span data-ttu-id="375e5-967">使用下列任一方式為網站啟用應用程式初始化模組：</span><span class="sxs-lookup"><span data-stu-id="375e5-967">Use either of the following approaches to enable the Application Initialization Module for the site:</span></span>

* <span data-ttu-id="375e5-968">使用 IIS 管理員：</span><span class="sxs-lookup"><span data-stu-id="375e5-968">Using IIS Manager:</span></span>

  1. <span data-ttu-id="375e5-969">選取 [連線] 面板中的 [應用程式集區]。</span><span class="sxs-lookup"><span data-stu-id="375e5-969">Select **Application Pools** in the **Connections** panel.</span></span>
  1. <span data-ttu-id="375e5-970">以滑鼠右鍵按一下清單中應用程式的應用程式集區，然後選取 [進階設定]。</span><span class="sxs-lookup"><span data-stu-id="375e5-970">Right-click the app's app pool in the list and select **Advanced Settings**.</span></span>
  1. <span data-ttu-id="375e5-971">預設的 [啟動模式] 是 [OnDemand]。</span><span class="sxs-lookup"><span data-stu-id="375e5-971">The default **Start Mode** is **OnDemand**.</span></span> <span data-ttu-id="375e5-972">將 [啟動模式] 設定為 [AlwaysRunning]。</span><span class="sxs-lookup"><span data-stu-id="375e5-972">Set the **Start Mode** to **AlwaysRunning**.</span></span> <span data-ttu-id="375e5-973">選取 **[確定]** 。</span><span class="sxs-lookup"><span data-stu-id="375e5-973">Select **OK**.</span></span>
  1. <span data-ttu-id="375e5-974">開啟 [連線] 面板中的 [站台] 節點。</span><span class="sxs-lookup"><span data-stu-id="375e5-974">Open the **Sites** node in the **Connections** panel.</span></span>
  1. <span data-ttu-id="375e5-975">以滑鼠右鍵按一下應用程式，然後選取 [**管理網站**] > [**高級設定**]。</span><span class="sxs-lookup"><span data-stu-id="375e5-975">Right-click the app and select **Manage Website** > **Advanced Settings**.</span></span>
  1. <span data-ttu-id="375e5-976">預設 [預先載入已啟用] 設定是 [False]。</span><span class="sxs-lookup"><span data-stu-id="375e5-976">The default **Preload Enabled** setting is **False**.</span></span> <span data-ttu-id="375e5-977">將 [預先載入已啟用] 設定為 [True]。</span><span class="sxs-lookup"><span data-stu-id="375e5-977">Set **Preload Enabled** to **True**.</span></span> <span data-ttu-id="375e5-978">選取 **[確定]** 。</span><span class="sxs-lookup"><span data-stu-id="375e5-978">Select **OK**.</span></span>

* <span data-ttu-id="375e5-979">使用 *web.config*，新增 `<applicationInitialization>` 元素並將 `doAppInitAfterRestart` 設定為 `true` 至應用程式 `<system.webServer>`web.config*檔案中的* 元素：</span><span class="sxs-lookup"><span data-stu-id="375e5-979">Using *web.config*, add the `<applicationInitialization>` element with `doAppInitAfterRestart` set to `true` to the `<system.webServer>` elements in the app's *web.config* file:</span></span>

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

### <a name="idle-timeout"></a><span data-ttu-id="375e5-980">閒置逾時</span><span class="sxs-lookup"><span data-stu-id="375e5-980">Idle Timeout</span></span>

<span data-ttu-id="375e5-981">*僅適用於應用程式裝載同處理序。*</span><span class="sxs-lookup"><span data-stu-id="375e5-981">*Only applies to apps hosted in-process.*</span></span>

<span data-ttu-id="375e5-982">若要防止應用程式閒置，請使用 IIS 管理員設定應用程式集區的閒置逾時：</span><span class="sxs-lookup"><span data-stu-id="375e5-982">To prevent the app from idling, set the app pool's idle timeout using IIS Manager:</span></span>

1. <span data-ttu-id="375e5-983">選取 [連線] 面板中的 [應用程式集區]。</span><span class="sxs-lookup"><span data-stu-id="375e5-983">Select **Application Pools** in the **Connections** panel.</span></span>
1. <span data-ttu-id="375e5-984">以滑鼠右鍵按一下清單中應用程式的應用程式集區，然後選取 [進階設定]。</span><span class="sxs-lookup"><span data-stu-id="375e5-984">Right-click the app's app pool in the list and select **Advanced Settings**.</span></span>
1. <span data-ttu-id="375e5-985">預設 [閒置逾時 (分鐘)] 是 **20** 分鐘。</span><span class="sxs-lookup"><span data-stu-id="375e5-985">The default **Idle Time-out (minutes)** is **20** minutes.</span></span> <span data-ttu-id="375e5-986">將 [閒置逾時 (分鐘)] 設定為 **0** (零)。</span><span class="sxs-lookup"><span data-stu-id="375e5-986">Set the **Idle Time-out (minutes)** to **0** (zero).</span></span> <span data-ttu-id="375e5-987">選取 **[確定]** 。</span><span class="sxs-lookup"><span data-stu-id="375e5-987">Select **OK**.</span></span>
1. <span data-ttu-id="375e5-988">回收背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="375e5-988">Recycle the worker process.</span></span>

<span data-ttu-id="375e5-989">若要防止應用程式裝載[非同處理序](#out-of-process-hosting-model)逾時，請使用下列任一方式：</span><span class="sxs-lookup"><span data-stu-id="375e5-989">To prevent apps hosted [out-of-process](#out-of-process-hosting-model) from timing out, use either of the following approaches:</span></span>

* <span data-ttu-id="375e5-990">從外部服務對應用程式執行 Ping 以讓它繼續執行。</span><span class="sxs-lookup"><span data-stu-id="375e5-990">Ping the app from an external service in order to keep it running.</span></span>
* <span data-ttu-id="375e5-991">若應用程式只裝載背景服務，避免 IIS 裝載並使用 [Windows 服務來裝載 ASP.NET Core 應用程式](xref:host-and-deploy/windows-service)。</span><span class="sxs-lookup"><span data-stu-id="375e5-991">If the app only hosts background services, avoid IIS hosting and use a [Windows Service to host the ASP.NET Core app](xref:host-and-deploy/windows-service).</span></span>

### <a name="application-initialization-module-and-idle-timeout-additional-resources"></a><span data-ttu-id="375e5-992">應用程式初始化模組與閒置逾時額外資源</span><span class="sxs-lookup"><span data-stu-id="375e5-992">Application Initialization Module and Idle Timeout additional resources</span></span>

* [<span data-ttu-id="375e5-993">IIS 8.0 應用程式初始化</span><span class="sxs-lookup"><span data-stu-id="375e5-993">IIS 8.0 Application Initialization</span></span>](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)
* <span data-ttu-id="375e5-994">[應用程式初始化 \<applicationInitialization>](/iis/configuration/system.webserver/applicationinitialization/)。</span><span class="sxs-lookup"><span data-stu-id="375e5-994">[Application Initialization \<applicationInitialization>](/iis/configuration/system.webserver/applicationinitialization/).</span></span>
* <span data-ttu-id="375e5-995">[應用程式集區的處理序模組設定 \<processModel>](/iis/configuration/system.applicationhost/applicationpools/add/processmodel)。</span><span class="sxs-lookup"><span data-stu-id="375e5-995">[Process Model Settings for an Application Pool \<processModel>](/iis/configuration/system.applicationhost/applicationpools/add/processmodel).</span></span>

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="375e5-996">IIS 系統管理員的部署資源</span><span class="sxs-lookup"><span data-stu-id="375e5-996">Deployment resources for IIS administrators</span></span>

* [<span data-ttu-id="375e5-997">IIS 文件</span><span class="sxs-lookup"><span data-stu-id="375e5-997">IIS documentation</span></span>](/iis)
* [<span data-ttu-id="375e5-998">IIS 中的 IIS 管理員使用者入門</span><span class="sxs-lookup"><span data-stu-id="375e5-998">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* [<span data-ttu-id="375e5-999">.NET Core 應用程式部署</span><span class="sxs-lookup"><span data-stu-id="375e5-999">.NET Core application deployment</span></span>](/dotnet/core/deploying/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/directory-structure>
* <xref:host-and-deploy/iis/modules>
* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>

## <a name="additional-resources"></a><span data-ttu-id="375e5-1000">其他資源</span><span class="sxs-lookup"><span data-stu-id="375e5-1000">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:index>
* [<span data-ttu-id="375e5-1001">Microsoft IIS 官方網站</span><span class="sxs-lookup"><span data-stu-id="375e5-1001">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="375e5-1002">Windows Server 技術內容庫</span><span class="sxs-lookup"><span data-stu-id="375e5-1002">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="375e5-1003">IIS 上的 HTTP/2</span><span class="sxs-lookup"><span data-stu-id="375e5-1003">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
* <xref:host-and-deploy/iis/transform-webconfig>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="375e5-1004">如需將 ASP.NET Core 應用程式發佈至 IIS 伺服器的教學課程體驗，請參閱<xref:tutorials/publish-to-iis>。</span><span class="sxs-lookup"><span data-stu-id="375e5-1004">For a tutorial experience on publishing an ASP.NET Core app to an IIS server, see <xref:tutorials/publish-to-iis>.</span></span>

[<span data-ttu-id="375e5-1005">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="375e5-1005">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="375e5-1006">支援的作業系統</span><span class="sxs-lookup"><span data-stu-id="375e5-1006">Supported operating systems</span></span>

<span data-ttu-id="375e5-1007">以下為支援的作業系統：</span><span class="sxs-lookup"><span data-stu-id="375e5-1007">The following operating systems are supported:</span></span>

* <span data-ttu-id="375e5-1008">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="375e5-1008">Windows 7 or later</span></span>
* <span data-ttu-id="375e5-1009">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="375e5-1009">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="375e5-1010">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys) (先前稱為 WebListener) 不適用搭配 IIS 的反向 Proxy 設定。</span><span class="sxs-lookup"><span data-stu-id="375e5-1010">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="375e5-1011">請使用 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1011">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="375e5-1012">如需在 Azure 中裝載的資訊，請參閱 <xref:host-and-deploy/azure-apps/index>。</span><span class="sxs-lookup"><span data-stu-id="375e5-1012">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

<span data-ttu-id="375e5-1013">如需疑難排解指引，請參閱 <xref:test/troubleshoot>。</span><span class="sxs-lookup"><span data-stu-id="375e5-1013">For troubleshooting guidance, see <xref:test/troubleshoot>.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="375e5-1014">支援的平台</span><span class="sxs-lookup"><span data-stu-id="375e5-1014">Supported platforms</span></span>

<span data-ttu-id="375e5-1015">支援針對 32 位元 (x86) 或 64 位元 (x64) 部署發行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-1015">Apps published for 32-bit (x86) or 64-bit (x64) deployment are supported.</span></span> <span data-ttu-id="375e5-1016">使用 32 位元 (x86) .NET Core SDK 部署 32 位元應用程式，除非應用程式：</span><span class="sxs-lookup"><span data-stu-id="375e5-1016">Deploy a 32-bit app with a 32-bit (x86) .NET Core SDK unless the app:</span></span>

* <span data-ttu-id="375e5-1017">需要提供給 64 位元應用程式使用的較大虛擬記憶體位址空間。</span><span class="sxs-lookup"><span data-stu-id="375e5-1017">Requires the larger virtual memory address space available to a 64-bit app.</span></span>
* <span data-ttu-id="375e5-1018">需要較大的 IIS 堆疊大小。</span><span class="sxs-lookup"><span data-stu-id="375e5-1018">Requires the larger IIS stack size.</span></span>
* <span data-ttu-id="375e5-1019">有 64 位元原生相依性。</span><span class="sxs-lookup"><span data-stu-id="375e5-1019">Has 64-bit native dependencies.</span></span>

<span data-ttu-id="375e5-1020">使用 64 位元 (x64) .NET Core SDK 來發行 64 位元應用程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-1020">Use a 64-bit (x64) .NET Core SDK to publish a 64-bit app.</span></span> <span data-ttu-id="375e5-1021">主機系統上必須有 64 位元執行階段存在。</span><span class="sxs-lookup"><span data-stu-id="375e5-1021">A 64-bit runtime must be present on the host system.</span></span>

<span data-ttu-id="375e5-1022">ASP.NET Core 隨附 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)，其為預設的跨平台 HTTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="375e5-1022">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), a default, cross-platform HTTP server.</span></span>

<span data-ttu-id="375e5-1023">在使用 [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) 或 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 時，應用程式會執行於從 IIS 背景工作處理序中分離出的處理序 (跨處理序)，並搭配 [Kestrel 伺服器](xref:fundamentals/servers/index#kestrel)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1023">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the app runs in a process separate from the IIS worker process (*out-of-process*) with the [Kestrel server](xref:fundamentals/servers/index#kestrel).</span></span>

<span data-ttu-id="375e5-1024">因為 ASP.NET Core 應用程式執行所在的處理序會與 IIS 工作者處理序分開，所以此模組會執行處理程序管理。</span><span class="sxs-lookup"><span data-stu-id="375e5-1024">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="375e5-1025">此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式關閉或損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="375e5-1025">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="375e5-1026">此行為基本上與執行同處理序，並由 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="375e5-1026">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="375e5-1027">下圖說明 IIS、ASP.NET Core 模組和跨處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="375e5-1027">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![ASP.NET Core 模組](index/_static/ancm-outofprocess.png)

<span data-ttu-id="375e5-1029">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-1029">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="375e5-1030">驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1030">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="375e5-1031">此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，而且不會是通訊埠 80 或 443。</span><span class="sxs-lookup"><span data-stu-id="375e5-1031">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="375e5-1032">此模組在啟動時透過環境變數指定連接埠，而 [IIS 整合中介軟體](xref:host-and-deploy/iis/index#enable-the-iisintegration-components)則會設定伺服器來接聽 `http://localhost:{port}`。</span><span class="sxs-lookup"><span data-stu-id="375e5-1032">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="375e5-1033">將會執行額外檢查，不是源自模組的要求都會遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="375e5-1033">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="375e5-1034">此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。</span><span class="sxs-lookup"><span data-stu-id="375e5-1034">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="375e5-1035">Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="375e5-1035">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="375e5-1036">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="375e5-1036">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="375e5-1037">IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="375e5-1037">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="375e5-1038">應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="375e5-1038">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="375e5-1039">`CreateDefaultBuilder` 會將 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器設為網頁伺服器，並設定 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)的基底路徑與連接埠來啟用 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="375e5-1039">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and enables IIS Integration by configuring the base path and port for the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="375e5-1040">ASP.NET Core 模組會產生要指派給後端處理序的動態連接埠。</span><span class="sxs-lookup"><span data-stu-id="375e5-1040">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="375e5-1041">`CreateDefaultBuilder` 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 方法。</span><span class="sxs-lookup"><span data-stu-id="375e5-1041">`CreateDefaultBuilder` calls the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> method.</span></span> <span data-ttu-id="375e5-1042">`UseIISIntegration` 會將 Kestrel 設定為在位於 localhost IP 位址 (`127.0.0.1`) 的動態連接埠上接聽。</span><span class="sxs-lookup"><span data-stu-id="375e5-1042">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="375e5-1043">若動態連接埠是 1234，Kestrel 會在 `127.0.0.1:1234` 接聽。</span><span class="sxs-lookup"><span data-stu-id="375e5-1043">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="375e5-1044">此設定會取代由下列項目提供的其他 URL 設定：</span><span class="sxs-lookup"><span data-stu-id="375e5-1044">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="375e5-1045">Kestrel 的接聽 API</span><span class="sxs-lookup"><span data-stu-id="375e5-1045">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="375e5-1046">[設定](xref:fundamentals/configuration/index) (或[命令列 --urls 選項](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="375e5-1046">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="375e5-1047">在使用模組時不需要呼叫 `UseUrls` 或 Kestrel 的 `Listen` API。</span><span class="sxs-lookup"><span data-stu-id="375e5-1047">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="375e5-1048">若呼叫 `UseUrls` 或 `Listen`，Kestrel 只會接聽未使用 IIS 執行應用程式時指定的連接埠。</span><span class="sxs-lookup"><span data-stu-id="375e5-1048">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

<span data-ttu-id="375e5-1049">如需 ASP.NET Core 模組組態指南，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="375e5-1049">For ASP.NET Core Module configuration guidance, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="375e5-1050">如需代管的詳細資訊，請參閱[在 ASP.NET Core 中代管](xref:fundamentals/index#host)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1050">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/index#host).</span></span>

## <a name="application-configuration"></a><span data-ttu-id="375e5-1051">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="375e5-1051">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="375e5-1052">啟用 IISIntegration 元件</span><span class="sxs-lookup"><span data-stu-id="375e5-1052">Enable the IISIntegration components</span></span>

<span data-ttu-id="375e5-1053">在 `CreateWebHostBuilder` （*Program.cs*）中建立主機時，請呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 以啟用 IIS 整合：</span><span class="sxs-lookup"><span data-stu-id="375e5-1053">When building a host in `CreateWebHostBuilder` (*Program.cs*), call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to enable IIS integration:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

<span data-ttu-id="375e5-1054">如需 `CreateDefaultBuilder` 的詳細資訊，請參閱 <xref:fundamentals/host/web-host#set-up-a-host>。</span><span class="sxs-lookup"><span data-stu-id="375e5-1054">For more information on `CreateDefaultBuilder`, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

### <a name="iis-options"></a><span data-ttu-id="375e5-1055">IIS 選項</span><span class="sxs-lookup"><span data-stu-id="375e5-1055">IIS options</span></span>

| <span data-ttu-id="375e5-1056">選項</span><span class="sxs-lookup"><span data-stu-id="375e5-1056">Option</span></span>                         | <span data-ttu-id="375e5-1057">預設</span><span class="sxs-lookup"><span data-stu-id="375e5-1057">Default</span></span> | <span data-ttu-id="375e5-1058">設定</span><span class="sxs-lookup"><span data-stu-id="375e5-1058">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="375e5-1059">若為 `true`，IIS 伺服器會設定由 `HttpContext.User`Windows 驗證[所驗證的 ](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1059">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="375e5-1060">若為 `false`，則伺服器僅會對 `HttpContext.User` 提供身分識別，並在 `AuthenticationScheme` 明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="375e5-1060">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="375e5-1061">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="375e5-1061">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="375e5-1062">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1062">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="375e5-1063">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="375e5-1063">Sets the display name shown to users on login pages.</span></span> |

<span data-ttu-id="375e5-1064">若要設定 IIS 選項，請在 <xref:Microsoft.AspNetCore.Builder.IISOptions> 中加入 <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="375e5-1064">To configure IIS options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="375e5-1065">下列範例會防止應用程式填入 `HttpContext.Connection.ClientCertificate`：</span><span class="sxs-lookup"><span data-stu-id="375e5-1065">The following example prevents the app from populating `HttpContext.Connection.ClientCertificate`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="375e5-1066">選項</span><span class="sxs-lookup"><span data-stu-id="375e5-1066">Option</span></span>                         | <span data-ttu-id="375e5-1067">預設</span><span class="sxs-lookup"><span data-stu-id="375e5-1067">Default</span></span> | <span data-ttu-id="375e5-1068">設定</span><span class="sxs-lookup"><span data-stu-id="375e5-1068">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="375e5-1069">若為 `true`，[IIS 整合中介軟體](#enable-the-iisintegration-components)會設定由 `HttpContext.User`Windows 驗證[所驗證的 ](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1069">If `true`, [IIS Integration Middleware](#enable-the-iisintegration-components) sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="375e5-1070">如果為 `false`，則驗證中介軟體僅針對 `HttpContext.User` 提供身分識別，並在游 `AuthenticationScheme` 提出明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="375e5-1070">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="375e5-1071">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="375e5-1071">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="375e5-1072">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="375e5-1072">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="375e5-1073">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="375e5-1073">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="375e5-1074">如果為 `true` 且 `MS-ASPNETCORE-CLIENTCERT` 要求標頭已存在，則會填入 `HttpContext.Connection.ClientCertificate`。</span><span class="sxs-lookup"><span data-stu-id="375e5-1074">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="375e5-1075">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="375e5-1075">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="375e5-1076">用來設定轉送標頭中介軟體及 ASP.NET Core 模組的 [IIS 整合中介軟體](#enable-the-iisintegration-components)會設定為轉送配置 (HTTP/HTTPS) 與發出要求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="375e5-1076">The [IIS Integration Middleware](#enable-the-iisintegration-components), which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="375e5-1077">其他 Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。</span><span class="sxs-lookup"><span data-stu-id="375e5-1077">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="375e5-1078">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1078">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="375e5-1079">web.config 檔案</span><span class="sxs-lookup"><span data-stu-id="375e5-1079">web.config file</span></span>

<span data-ttu-id="375e5-1080">*web.config* 檔案是用來設定 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1080">The *web.config* file configures the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="375e5-1081">發佈專案時，由 MSBuild 目標 ( *) 處理* web.config`_TransformWebConfig` 檔案的建立、轉換及發佈。</span><span class="sxs-lookup"><span data-stu-id="375e5-1081">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="375e5-1082">此目標存在於 Web SDK 目標 (`Microsoft.NET.Sdk.Web`)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1082">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="375e5-1083">SDK 設定在專案檔的頂端：</span><span class="sxs-lookup"><span data-stu-id="375e5-1083">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="375e5-1084">如果*web.config*檔案不存在於專案中，則會使用正確的*processPath*和*引數*來建立檔案，以設定 ASP.NET Core 模組並移至[已發行的輸出](xref:host-and-deploy/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1084">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="375e5-1085">如果 *web.config* 檔案存在於專案中，則系統會使用正確的 *processPath* 和 *arguments* 來轉換該檔案以設定 ASP.NET Core 模組，然後將它移至已發行的輸出。</span><span class="sxs-lookup"><span data-stu-id="375e5-1085">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="375e5-1086">轉換不會修改檔案中的 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="375e5-1086">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="375e5-1087">*web.config* 檔案可提供能控制作用中 IIS 模組的額外 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="375e5-1087">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="375e5-1088">如需能處理 ASP.NET Core 應用程式要求之 IIS 模組的相關資訊，請參閱 [IIS 模組](xref:host-and-deploy/iis/modules)主題。</span><span class="sxs-lookup"><span data-stu-id="375e5-1088">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="375e5-1089">為防止 Web SDK 轉換 *web.config* 檔案，請使用專案檔中的 **\<IsTransformWebConfigDisabled>** 屬性：</span><span class="sxs-lookup"><span data-stu-id="375e5-1089">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="375e5-1090">使 Web SDK 無法轉換檔案時，應該由開發人員手動設定 *processPath* 和 *arguments*。</span><span class="sxs-lookup"><span data-stu-id="375e5-1090">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="375e5-1091">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="375e5-1091">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="375e5-1092">web.config 檔案位置</span><span class="sxs-lookup"><span data-stu-id="375e5-1092">web.config file location</span></span>

<span data-ttu-id="375e5-1093">為了正確設定[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module) *，web.config 檔案*必須存在於已部署應用程式的[內容根](xref:fundamentals/index#content-root)路徑（通常是應用程式基底路徑）。</span><span class="sxs-lookup"><span data-stu-id="375e5-1093">In order to set up the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) correctly, the *web.config* file must be present at the [content root](xref:fundamentals/index#content-root) path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="375e5-1094">這是與提供給 IIS 的網站實體路徑相同的位置。</span><span class="sxs-lookup"><span data-stu-id="375e5-1094">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="375e5-1095">應用程式的根目錄需有 *web.config* 檔案，才能使用 Web Deploy 發行多個應用程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-1095">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="375e5-1096">機密檔案存在於應用程式的實體路徑上，例如 *\<組件>.runtimeconfig.json*、 *\<組件>.xml* (XML 文件註解)，以及 *\<組件>.deps.json*。</span><span class="sxs-lookup"><span data-stu-id="375e5-1096">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="375e5-1097">當 *web.config* 檔案存在且網站正常啟動時，如果有人要求機密檔案，IIS 不會予以提供。</span><span class="sxs-lookup"><span data-stu-id="375e5-1097">When the *web.config* file is present and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="375e5-1098">若 *web.config* 檔案遺失或沒有正確命名，或是無法設定網站以正常啟動，IIS 可能會公開提供機密檔案。</span><span class="sxs-lookup"><span data-stu-id="375e5-1098">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="375e5-1099">\***Web.config*檔案必須隨時存在於部署中、正確命名，而且能夠將網站設定為正常啟動。絕對不要從生產環境部署*移除 web.config 檔案\*。**</span><span class="sxs-lookup"><span data-stu-id="375e5-1099">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

### <a name="transform-webconfig"></a><span data-ttu-id="375e5-1100">轉換 web.config</span><span class="sxs-lookup"><span data-stu-id="375e5-1100">Transform web.config</span></span>

<span data-ttu-id="375e5-1101">如需在發佈時轉換 *web.config* (例如依據設定、設定檔或環境設定環境變數)，請參閱<xref:host-and-deploy/iis/transform-webconfig>。</span><span class="sxs-lookup"><span data-stu-id="375e5-1101">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="375e5-1102">IIS 組態</span><span class="sxs-lookup"><span data-stu-id="375e5-1102">IIS configuration</span></span>

<span data-ttu-id="375e5-1103">**Windows Server 作業系統**</span><span class="sxs-lookup"><span data-stu-id="375e5-1103">**Windows Server operating systems**</span></span>

<span data-ttu-id="375e5-1104">啟用**網頁伺服器 (IIS)** 伺服器角色，並建立角色服務。</span><span class="sxs-lookup"><span data-stu-id="375e5-1104">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="375e5-1105">使用來自 [管理] 功能表的 [新增角色及功能] 精靈，或是 [伺服器管理員] 中的連結。</span><span class="sxs-lookup"><span data-stu-id="375e5-1105">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="375e5-1106">在**伺服器角色**步驟中，核取 [網頁伺服器 (IIS)] 方塊。</span><span class="sxs-lookup"><span data-stu-id="375e5-1106">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![在選取伺服器角色步驟中選取網頁伺服器 IIS 角色。](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="375e5-1108">在 [功能] 步驟之後，[角色服務] 步驟會針對網頁伺服器 (IIS) 進行載入。</span><span class="sxs-lookup"><span data-stu-id="375e5-1108">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="375e5-1109">選取所需的 IIS 角色服務或接受所提供的預設角色服務。</span><span class="sxs-lookup"><span data-stu-id="375e5-1109">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![在選取角色服務步驟中，選取預設的角色服務。](index/_static/role-services-ws2016.png)

   <span data-ttu-id="375e5-1111">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="375e5-1111">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="375e5-1112">若要啟用 Windows 驗證，請展開下列節點：[網頁伺服器] > [安全性]。</span><span class="sxs-lookup"><span data-stu-id="375e5-1112">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="375e5-1113">選取 [Windows 驗證] 功能。</span><span class="sxs-lookup"><span data-stu-id="375e5-1113">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="375e5-1114">如需詳細資訊，請參閱 [Windows 驗證 \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) 和[設定 Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1114">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="375e5-1115">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="375e5-1115">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="375e5-1116">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="375e5-1116">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="375e5-1117">若要啟用 WebSocket，請展開下列節點：[網頁伺服器] > [應用程式開發]。</span><span class="sxs-lookup"><span data-stu-id="375e5-1117">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="375e5-1118">選取 [WebSocket 通訊協定] 功能。</span><span class="sxs-lookup"><span data-stu-id="375e5-1118">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="375e5-1119">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1119">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="375e5-1120">透過**確認**步驟繼續作業，安裝網頁伺服器角色和服務。</span><span class="sxs-lookup"><span data-stu-id="375e5-1120">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="375e5-1121">安裝**網頁伺服器 (IIS)** 角色之後，不需要重新啟動伺服器/IIS。</span><span class="sxs-lookup"><span data-stu-id="375e5-1121">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="375e5-1122">**Windows 桌面作業系統**</span><span class="sxs-lookup"><span data-stu-id="375e5-1122">**Windows desktop operating systems**</span></span>

<span data-ttu-id="375e5-1123">啟用 [IIS 管理主控台] 和 [World Wide Web 服務]。</span><span class="sxs-lookup"><span data-stu-id="375e5-1123">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="375e5-1124">流覽至 [**控制台**] >**程式**> [**程式和功能**] > [**開啟或關閉 Windows 功能**] （畫面左側）。</span><span class="sxs-lookup"><span data-stu-id="375e5-1124">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="375e5-1125">開啟 [Internet Information Services] 節點。</span><span class="sxs-lookup"><span data-stu-id="375e5-1125">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="375e5-1126">開啟 [Web 管理工具] 節點。</span><span class="sxs-lookup"><span data-stu-id="375e5-1126">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="375e5-1127">核取 [IIS 管理主控台] 方塊。</span><span class="sxs-lookup"><span data-stu-id="375e5-1127">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="375e5-1128">[World Wide Web Services] (全球資訊網服務) 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="375e5-1128">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="375e5-1129">接受**全球資訊網服務**的預設功能，或自訂 IIS 功能。</span><span class="sxs-lookup"><span data-stu-id="375e5-1129">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="375e5-1130">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="375e5-1130">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="375e5-1131">若要啟用 Windows 驗證，請展開下列節點：[World Wide Web 服務] > [安全性]。</span><span class="sxs-lookup"><span data-stu-id="375e5-1131">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="375e5-1132">選取 [Windows 驗證] 功能。</span><span class="sxs-lookup"><span data-stu-id="375e5-1132">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="375e5-1133">如需詳細資訊，請參閱 [Windows 驗證 \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) 和[設定 Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1133">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="375e5-1134">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="375e5-1134">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="375e5-1135">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="375e5-1135">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="375e5-1136">若要啟用 WebSocket，請展開下列節點：[World Wide Web 服務] > [應用程式開發功能]。</span><span class="sxs-lookup"><span data-stu-id="375e5-1136">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="375e5-1137">選取 [WebSocket 通訊協定] 功能。</span><span class="sxs-lookup"><span data-stu-id="375e5-1137">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="375e5-1138">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1138">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="375e5-1139">若 IIS 安裝需要重新啟動，請重新啟動系統。</span><span class="sxs-lookup"><span data-stu-id="375e5-1139">If the IIS installation requires a restart, restart the system.</span></span>

![選取 [Windows 功能] 中的 [IIS 管理主控台] 和 [World Wide Web Services] (全球資訊網服務)。](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="375e5-1141">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="375e5-1141">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="375e5-1142">在主控系統上安裝 .NET Core 裝載套件組合。</span><span class="sxs-lookup"><span data-stu-id="375e5-1142">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="375e5-1143">套件組合會安裝 .NET Core 執行階段、.NET Core 程式庫和 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1143">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="375e5-1144">此模組可讓 ASP.NET Core 應用程式在 IIS 背後執行。</span><span class="sxs-lookup"><span data-stu-id="375e5-1144">The module allows ASP.NET Core apps to run behind IIS.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="375e5-1145">若裝載套件組合在 IIS 之前安裝，則必須對該套件組合安裝進行修復。</span><span class="sxs-lookup"><span data-stu-id="375e5-1145">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="375e5-1146">請在安裝 IIS 之後，再次執行裝載套件組合安裝程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-1146">Run the Hosting Bundle installer again after installing IIS.</span></span>
>
> <span data-ttu-id="375e5-1147">如果在安裝 64 位元 (x64) 版本的 .NET Core 後才安裝裝載套件組合，那麼可能會遺漏 SDK ([未偵測到 .NET Core SDK](xref:test/troubleshoot#no-net-core-sdks-were-detected))。</span><span class="sxs-lookup"><span data-stu-id="375e5-1147">If the Hosting Bundle is installed after installing the 64-bit (x64) version of .NET Core, SDKs might appear to be missing ([No .NET Core SDKs were detected](xref:test/troubleshoot#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="375e5-1148">若要解決此問題，請參閱 <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>。</span><span class="sxs-lookup"><span data-stu-id="375e5-1148">To resolve the problem, see <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>.</span></span>

### <a name="direct-download-current-version"></a><span data-ttu-id="375e5-1149">直接下載 (目前版本)</span><span class="sxs-lookup"><span data-stu-id="375e5-1149">Direct download (current version)</span></span>

<span data-ttu-id="375e5-1150">使用下列連結下載安裝程式：</span><span class="sxs-lookup"><span data-stu-id="375e5-1150">Download the installer using the following link:</span></span>

[<span data-ttu-id="375e5-1151">目前的 .NET Core 裝載套件組合安裝程式 (直接下載)</span><span class="sxs-lookup"><span data-stu-id="375e5-1151">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a><span data-ttu-id="375e5-1152">安裝程式的先前版本</span><span class="sxs-lookup"><span data-stu-id="375e5-1152">Earlier versions of the installer</span></span>

<span data-ttu-id="375e5-1153">若要取得安裝程式的先前版本：</span><span class="sxs-lookup"><span data-stu-id="375e5-1153">To obtain an earlier version of the installer:</span></span>

1. <span data-ttu-id="375e5-1154">瀏覽至 [.NET 下載封存](https://www.microsoft.com/net/download/archives)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1154">Navigate to the [.NET download archives](https://www.microsoft.com/net/download/archives).</span></span>
1. <span data-ttu-id="375e5-1155">在 [.NET Core] 下，選取 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="375e5-1155">Under **.NET Core**, select the .NET Core version.</span></span>
1. <span data-ttu-id="375e5-1156">在 [執行應用程式 - 執行階段] 欄中，尋找想要的 .NET Core 執行階段版本列。</span><span class="sxs-lookup"><span data-stu-id="375e5-1156">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="375e5-1157">使用**執行階段與裝載套件組合**連結。</span><span class="sxs-lookup"><span data-stu-id="375e5-1157">Download the installer using the **Runtime & Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="375e5-1158">某些安裝程式包含已達到期生命週期結束 (EOL) 的發行版本，這些發行版本已不受 Microsoft 支援。</span><span class="sxs-lookup"><span data-stu-id="375e5-1158">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="375e5-1159">如需詳細資訊，請參閱[支援原則](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) \(英文 \)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1159">For more information, see the [support policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="375e5-1160">安裝裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="375e5-1160">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="375e5-1161">在伺服器上執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-1161">Run the installer on the server.</span></span> <span data-ttu-id="375e5-1162">從系統管理員命令殼層執行安裝程式時，有 下列參數可用：</span><span class="sxs-lookup"><span data-stu-id="375e5-1162">The following parameters are available when running the installer from an administrator command shell:</span></span>

   * <span data-ttu-id="375e5-1163">`OPT_NO_ANCM=1` &ndash; 略過安裝 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="375e5-1163">`OPT_NO_ANCM=1` &ndash; Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="375e5-1164">`OPT_NO_RUNTIME=1` &ndash; 略過安裝 .NET Core 執行時間。</span><span class="sxs-lookup"><span data-stu-id="375e5-1164">`OPT_NO_RUNTIME=1` &ndash; Skip installing the .NET Core runtime.</span></span> <span data-ttu-id="375e5-1165">當伺服器只裝載[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)時使用。</span><span class="sxs-lookup"><span data-stu-id="375e5-1165">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="375e5-1166">`OPT_NO_SHAREDFX=1` &ndash; 略過安裝 ASP.NET 共用架構（ASP.NET 執行時間）。</span><span class="sxs-lookup"><span data-stu-id="375e5-1166">`OPT_NO_SHAREDFX=1` &ndash; Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span> <span data-ttu-id="375e5-1167">當伺服器只裝載[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)時使用。</span><span class="sxs-lookup"><span data-stu-id="375e5-1167">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="375e5-1168">`OPT_NO_X86=1` &ndash; 略過安裝 x86 執行時間。</span><span class="sxs-lookup"><span data-stu-id="375e5-1168">`OPT_NO_X86=1` &ndash; Skip installing x86 runtimes.</span></span> <span data-ttu-id="375e5-1169">當您確定不會裝載 32 位元應用程式時，請使用此參數。</span><span class="sxs-lookup"><span data-stu-id="375e5-1169">Use this parameter when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="375e5-1170">如果將來有可能同時裝載 32 位元和 64 位元應用程式，請不要使用此參數並安裝這兩個執行階段。</span><span class="sxs-lookup"><span data-stu-id="375e5-1170">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this parameter and install both runtimes.</span></span>
   * <span data-ttu-id="375e5-1171">`OPT_NO_SHARED_CONFIG_CHECK=1` &ndash; 當共用設定（*applicationhost.config*）位於與 iis 安裝相同的電腦上時，請停用 [使用 iis 共用設定檢查]。</span><span class="sxs-lookup"><span data-stu-id="375e5-1171">`OPT_NO_SHARED_CONFIG_CHECK=1` &ndash; Disable the check for using an IIS Shared Configuration when the shared configuration (*applicationHost.config*) is on the same machine as the IIS installation.</span></span> <span data-ttu-id="375e5-1172">*只在 ASP.NET Core 2.2 或更新版本的裝載套件組合安裝程式上可用。*</span><span class="sxs-lookup"><span data-stu-id="375e5-1172">*Only available for ASP.NET Core 2.2 or later Hosting Bundler installers.*</span></span> <span data-ttu-id="375e5-1173">如需詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>。</span><span class="sxs-lookup"><span data-stu-id="375e5-1173">For more information, see <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>.</span></span>
1. <span data-ttu-id="375e5-1174">重新開機系統，或在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="375e5-1174">Restart the system or execute the following commands in a command shell:</span></span>

   ```console
   net stop was /y
   net start w3svc
   ```
   <span data-ttu-id="375e5-1175">重新啟動 IIS 將能偵測到由安裝程式對系統路徑 (此為環境變數) 所做出的變更。</span><span class="sxs-lookup"><span data-stu-id="375e5-1175">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

<span data-ttu-id="375e5-1176">安裝裝載套件組合時，不需要手動停止 IIS 中的個別網站。</span><span class="sxs-lookup"><span data-stu-id="375e5-1176">It isn't necessary to manually stop individual sites in IIS when installing the Hosting Bundle.</span></span> <span data-ttu-id="375e5-1177">裝載的應用程式（IIS 網站）會在 IIS 重新開機時重新開機。</span><span class="sxs-lookup"><span data-stu-id="375e5-1177">Hosted apps (IIS sites) restart when IIS restarts.</span></span> <span data-ttu-id="375e5-1178">應用程式會在收到第一個要求時重新開機，包括從[應用程式初始化模組](#application-initialization-module-and-idle-timeout)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1178">Apps start up again when they receive their first request, including from the [Application Initialization Module](#application-initialization-module-and-idle-timeout).</span></span>

<span data-ttu-id="375e5-1179">ASP.NET Core 採用共用架構封裝修補程式版本的向前復原行為。</span><span class="sxs-lookup"><span data-stu-id="375e5-1179">ASP.NET Core adopts roll-forward behavior for patch releases of shared framework packages.</span></span> <span data-ttu-id="375e5-1180">當 IIS 所裝載的應用程式使用 IIS 重新開機時，應用程式會在收到第一個要求時，以其所參考套件的最新修補程式版本來載入。</span><span class="sxs-lookup"><span data-stu-id="375e5-1180">When apps hosted by IIS restart with IIS, the apps load with the latest patch releases of their referenced packages when they receive their first request.</span></span> <span data-ttu-id="375e5-1181">如果未重新開機 IIS，應用程式會在其工作者進程回收時重新開機並展示向前復原行為，並接收其第一個要求。</span><span class="sxs-lookup"><span data-stu-id="375e5-1181">If IIS isn't restarted, apps restart and exhibit roll-forward behavior when their worker processes are recycled and they receive their first request.</span></span>

> [!NOTE]
> <span data-ttu-id="375e5-1182">如需 IIS 共用組態的資訊，請參閱[使用 IIS 共用組態的 ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1182">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="375e5-1183">使用 Visual Studio 發佈時安裝 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="375e5-1183">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="375e5-1184">將應用程式部署到具有 [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later) 的伺服器時，請在伺服器上安裝最新版的 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="375e5-1184">When deploying apps to servers with [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="375e5-1185">若要安裝 Web Deploy，請使用 [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) 或從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=43717)直接取得安裝程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-1185">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="375e5-1186">慣用的方法是使用 WebPI。</span><span class="sxs-lookup"><span data-stu-id="375e5-1186">The preferred method is to use WebPI.</span></span> <span data-ttu-id="375e5-1187">WebPI 提供獨立的安裝程式和組態以裝載提供者。</span><span class="sxs-lookup"><span data-stu-id="375e5-1187">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="375e5-1188">建立 IIS 網站</span><span class="sxs-lookup"><span data-stu-id="375e5-1188">Create the IIS site</span></span>

1. <span data-ttu-id="375e5-1189">在主控系統上，請建立資料夾以容納應用程式的已發行資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="375e5-1189">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="375e5-1190">在下列步驟中，您提供資料夾路徑給 IIS，作為應用程式的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="375e5-1190">In a following step, the folder's path is provided to IIS as the physical path to the app.</span></span> <span data-ttu-id="375e5-1191">如需應用程式之部署資料夾和檔案配置的詳細資訊，請參閱 <xref:host-and-deploy/directory-structure>。</span><span class="sxs-lookup"><span data-stu-id="375e5-1191">For more information on an app's deployment folder and file layout, see <xref:host-and-deploy/directory-structure>.</span></span>

1. <span data-ttu-id="375e5-1192">在 [IIS 管理員] 中，於 [連線] 面板中開啟伺服器的節點。</span><span class="sxs-lookup"><span data-stu-id="375e5-1192">In IIS Manager, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="375e5-1193">以滑鼠右鍵按一下 [網站] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="375e5-1193">Right-click the **Sites** folder.</span></span> <span data-ttu-id="375e5-1194">從操作功能表選取 [新增網站]。</span><span class="sxs-lookup"><span data-stu-id="375e5-1194">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="375e5-1195">提供**網站名稱**，並將**實體路徑**設定為應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="375e5-1195">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="375e5-1196">透過選取 [確定] 來提供**繫結**設定並建立網站：</span><span class="sxs-lookup"><span data-stu-id="375e5-1196">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![在新增網站步驟中提供站台名稱、實體路徑和主機名稱。](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="375e5-1198">請`http://*:80/`勿`http://+:80`使用最上層萬用字元繫結 (**與** )。</span><span class="sxs-lookup"><span data-stu-id="375e5-1198">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="375e5-1199">最上層萬用字元繫結可能暴露您的應用程式安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="375e5-1199">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="375e5-1200">這對強式與弱式萬用字元皆適用。</span><span class="sxs-lookup"><span data-stu-id="375e5-1200">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="375e5-1201">請使用明確主機名稱，而非萬用字元。</span><span class="sxs-lookup"><span data-stu-id="375e5-1201">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="375e5-1202">若您擁有整個父網域 (與具弱點的 `*.mysub.com` 相對) 的控制權，則子網域萬用字元繫結 (例如 `*.com`) 就沒有此安全性風險。</span><span class="sxs-lookup"><span data-stu-id="375e5-1202">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="375e5-1203">如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1203">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="375e5-1204">在伺服器的節點之下，選取 [應用程式集區]。</span><span class="sxs-lookup"><span data-stu-id="375e5-1204">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="375e5-1205">以滑鼠右鍵按一下網站的應用程式集區，然後從操作功能表選取 [基本設定]。</span><span class="sxs-lookup"><span data-stu-id="375e5-1205">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="375e5-1206">在 [編輯應用程式集區] 視窗中，將 [.NET CLR 版本] 設定為 [沒有受控碼]：</span><span class="sxs-lookup"><span data-stu-id="375e5-1206">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![將 .NET CLR 版本設為 [沒有受控碼]。](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="375e5-1208">ASP.NET Core 會在不同的處理序中執行，並管理執行階段。</span><span class="sxs-lookup"><span data-stu-id="375e5-1208">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="375e5-1209">ASP.NET Core 不仰賴載入桌面 CLR (.NET CLR)&mdash;會使用 .NET Core 的核心通用語言執行平台 (CoreCLR) 來開機以在背景工作處理序中裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-1209">ASP.NET Core doesn't rely on loading the desktop CLR (.NET CLR)&mdash;the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process.</span></span> <span data-ttu-id="375e5-1210">將 [.NET CLR 版本] 設定為 [沒有受控碼] 是選擇性的，但建議這樣做。</span><span class="sxs-lookup"><span data-stu-id="375e5-1210">Setting the **.NET CLR version** to **No Managed Code** is optional but recommended.</span></span>

1. <span data-ttu-id="375e5-1211">*ASP.NET Core 2.2 或更新版本*：對於使用[同處理序主控模型](/dotnet/core/deploying/#self-contained-deployments-scd)的 64 位元 (x64) [獨立式部署](#in-process-hosting-model)，會停用 32 位元 (x86) 處理序的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="375e5-1211">*ASP.NET Core 2.2 or later*: For a 64-bit (x64) [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) that uses the [in-process hosting model](#in-process-hosting-model), disable the app pool for 32-bit (x86) processes.</span></span>

   <span data-ttu-id="375e5-1212">在 IIS 管理員的 [動作] 資訊看板 > [應用程式集區] 中，選取 [設定應用程式集區預設值] 或 [進階設定]。</span><span class="sxs-lookup"><span data-stu-id="375e5-1212">In the **Actions** sidebar of IIS Manager > **Application Pools**, select **Set Application Pool Defaults** or **Advanced Settings**.</span></span> <span data-ttu-id="375e5-1213">找到 [啟用 32 位元應用程式]，然後將其值設定為 `False`。</span><span class="sxs-lookup"><span data-stu-id="375e5-1213">Locate **Enable 32-Bit Applications** and set the value to `False`.</span></span> <span data-ttu-id="375e5-1214">此設定不會影響為[處理程序外裝載](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model)部署的應用程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-1214">This setting doesn't affect apps deployed for [out-of-process hosting](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).</span></span>

1. <span data-ttu-id="375e5-1215">確認處理序模型身分識別具有適當的權限。</span><span class="sxs-lookup"><span data-stu-id="375e5-1215">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="375e5-1216">如果您將應用程式集區的預設身分識別 ([處理序模型] > [身分識別]) 從 **ApplicationPoolIdentity** 變更為其他身分識別，請確認新的身分識別具有必要權限，可存取應用程式的資料夾、資料庫和其他必要的資源。</span><span class="sxs-lookup"><span data-stu-id="375e5-1216">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="375e5-1217">例如，應用程式集區需要針對應用程式讀取和寫入檔案的資料夾取得讀取和寫入權限。</span><span class="sxs-lookup"><span data-stu-id="375e5-1217">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="375e5-1218">**Windows 驗證設定 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="375e5-1218">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="375e5-1219">如需詳細資訊，請參閱[設定 Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="375e5-1219">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="375e5-1220">部署應用程式</span><span class="sxs-lookup"><span data-stu-id="375e5-1220">Deploy the app</span></span>

<span data-ttu-id="375e5-1221">將應用程式部署至在**建立 IIS 網站**一節中所建立的 IIS [實體路徑](#create-the-iis-site)資料夾。</span><span class="sxs-lookup"><span data-stu-id="375e5-1221">Deploy the app to the IIS **Physical path** folder that was established in the [Create the IIS site](#create-the-iis-site) section.</span></span> <span data-ttu-id="375e5-1222">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 是建議的部署機制，但有數個選項可將應用程式從專案的 [publish] 資料夾移動至主機系統的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="375e5-1222">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment, but several options exist for moving the app from the project's *publish* folder to the hosting system's deployment folder.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="375e5-1223">使用 Visual Studio 的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="375e5-1223">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="375e5-1224">請參閱[適用於 ASP.NET Core 應用程式部署的 Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)主題，了解如何建立搭配 Web Deploy 使用的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="375e5-1224">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="375e5-1225">如果主機服務提供者提供或支援建立發行設定檔，請下載其設定檔，並使用 Visual Studio 的 [發行] 對話方塊匯入其設定檔：</span><span class="sxs-lookup"><span data-stu-id="375e5-1225">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog:</span></span>

![[發佈] 對話方塊頁](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="375e5-1227">Visual Studio 外部的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="375e5-1227">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="375e5-1228">透過命令列也可以在 Visual Studio 外部使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1228">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="375e5-1229">如需詳細資訊，請參閱 [Web 部署工具](/iis/publish/using-web-deploy/use-the-web-deployment-tool)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1229">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="375e5-1230">Web Deploy 的替代項目</span><span class="sxs-lookup"><span data-stu-id="375e5-1230">Alternatives to Web Deploy</span></span>

<span data-ttu-id="375e5-1231">使用數種方法的其中一種將應用程式移到主機系統，例如手動複製、[Xcopy](/windows-server/administration/windows-commands/xcopy)、[Robocopy](/windows-server/administration/windows-commands/robocopy) 或 [PowerShell](/powershell/)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1231">Use any of several methods to move the app to the hosting system, such as manual copy, [Xcopy](/windows-server/administration/windows-commands/xcopy), [Robocopy](/windows-server/administration/windows-commands/robocopy), or [PowerShell](/powershell/).</span></span>

<span data-ttu-id="375e5-1232">如需 IIS 的 ASP.NET Core 部署詳細資訊，請參閱 [IIS 系統管理員的部署資源](#deployment-resources-for-iis-administrators)一節。</span><span class="sxs-lookup"><span data-stu-id="375e5-1232">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="375e5-1233">瀏覽網站</span><span class="sxs-lookup"><span data-stu-id="375e5-1233">Browse the website</span></span>

<span data-ttu-id="375e5-1234">應用程式部署到主機系統之後，請向其中一個應用程式的公用端點發出要求。</span><span class="sxs-lookup"><span data-stu-id="375e5-1234">After the app is deployed to the hosting system, make a request to one of the app's public endpoints.</span></span>

<span data-ttu-id="375e5-1235">在下列範例中，網站系結至**埠**`80`上 `www.mysite.com` 的 IIS**主機名稱**。</span><span class="sxs-lookup"><span data-stu-id="375e5-1235">In the following example, the site is bound to an IIS **Host name** of `www.mysite.com` on **Port** `80`.</span></span> <span data-ttu-id="375e5-1236">對 `http://www.mysite.com` 發出要求：</span><span class="sxs-lookup"><span data-stu-id="375e5-1236">A request is made to `http://www.mysite.com`:</span></span>

![Microsoft Edge 瀏覽器已載入 IIS 啟動頁面。](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="375e5-1238">已鎖定的部署檔案</span><span class="sxs-lookup"><span data-stu-id="375e5-1238">Locked deployment files</span></span>

<span data-ttu-id="375e5-1239">當應用程式執行時，會鎖定部署資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="375e5-1239">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="375e5-1240">無法於部署期間覆寫已鎖定的檔案。</span><span class="sxs-lookup"><span data-stu-id="375e5-1240">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="375e5-1241">若要釋放部署中的已鎖定檔案，請使用下列其中**一種**方法停止應用程式集區：</span><span class="sxs-lookup"><span data-stu-id="375e5-1241">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="375e5-1242">使用 Web Deploy 並參考專案檔中的 `Microsoft.NET.Sdk.Web`。</span><span class="sxs-lookup"><span data-stu-id="375e5-1242">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="375e5-1243">*app_offline.htm* 檔案是放在 Web 應用程式目錄的根目錄中。</span><span class="sxs-lookup"><span data-stu-id="375e5-1243">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="375e5-1244">當檔案存在時，ASP.NET Core 模組會正常關閉應用程式，並在部署期間提供 *app_offline.htm* 檔案。</span><span class="sxs-lookup"><span data-stu-id="375e5-1244">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="375e5-1245">如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1245">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="375e5-1246">在伺服器上的 IIS 管理員中手動停止應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="375e5-1246">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="375e5-1247">使用 PowerShell 卸載*app_offline .htm* （需要 PowerShell 5 或更新版本）：</span><span class="sxs-lookup"><span data-stu-id="375e5-1247">Use PowerShell to drop *app_offline.htm* (requires PowerShell 5 or later):</span></span>

  ```powershell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a><span data-ttu-id="375e5-1248">資料保護</span><span class="sxs-lookup"><span data-stu-id="375e5-1248">Data protection</span></span>

<span data-ttu-id="375e5-1249">[ASP.NET Core 資料保護堆疊](xref:security/data-protection/introduction)是由數個 ASP.NET Core [中介軟體](xref:fundamentals/middleware/index)所使用，包括用於驗證的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="375e5-1249">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="375e5-1250">即使資料保護 API 不是由使用者程式碼呼叫，仍應使用部署指令碼或是在使用者程式碼中設定資料保護，以建立持續性的密碼編譯[金鑰存放區](xref:security/data-protection/implementation/key-management)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1250">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="375e5-1251">如不設定資料保護，金鑰會保留在記憶體中，並於應用程式重新啟動時捨棄。</span><span class="sxs-lookup"><span data-stu-id="375e5-1251">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="375e5-1252">如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：</span><span class="sxs-lookup"><span data-stu-id="375e5-1252">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="375e5-1253">所有以 Cookie 為基礎的驗證權杖都會失效。</span><span class="sxs-lookup"><span data-stu-id="375e5-1253">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="375e5-1254">當使用者提出下一個要求時，需要再次登入。</span><span class="sxs-lookup"><span data-stu-id="375e5-1254">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="375e5-1255">所有以 Keyring 保護的資料都無法再解密。</span><span class="sxs-lookup"><span data-stu-id="375e5-1255">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="375e5-1256">這可能會包含 [CSRF 權杖](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)和 [ASP.NET Core MVC TempData cookie](xref:fundamentals/app-state#tempdata)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1256">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="375e5-1257">若要在 IIS 下設定資料保護以保存 Keyring，請使用下列其中**一種**方法：</span><span class="sxs-lookup"><span data-stu-id="375e5-1257">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="375e5-1258">**建立資料保護登錄機碼**</span><span class="sxs-lookup"><span data-stu-id="375e5-1258">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="375e5-1259">ASP.NET Core 應用程式所使用的資料保護金鑰會儲存在應用程式外部的登錄中。</span><span class="sxs-lookup"><span data-stu-id="375e5-1259">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="375e5-1260">若要保存指定應用程式的金鑰，請為應用程式集區建立登錄機碼。</span><span class="sxs-lookup"><span data-stu-id="375e5-1260">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="375e5-1261">若為獨立的非Web 伺服陣列 IIS 安裝，請針對搭配使用 ASP.NET Core 應用程式的每個應用程式集區，使用[資料保護 Provision-AutoGenKeys.ps1 PowerShell 指令碼](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1261">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="375e5-1262">此指令碼會在 HKLM 登錄中建立登錄機碼，只有應用程式之應用程式集區的背景工作處理序帳戶可以存取它。</span><span class="sxs-lookup"><span data-stu-id="375e5-1262">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="375e5-1263">在待用期間使用 DPAPI 和全電腦金鑰加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="375e5-1263">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="375e5-1264">在 Web 伺服陣列案例中，應用程式可以設定成使用 UNC 路徑來儲存其資料保護 Keyring。</span><span class="sxs-lookup"><span data-stu-id="375e5-1264">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="375e5-1265">根據預設，資料保護金鑰不予加密。</span><span class="sxs-lookup"><span data-stu-id="375e5-1265">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="375e5-1266">請確保網路共用的檔案權限僅限於執行應用程式的 Windows 帳戶。</span><span class="sxs-lookup"><span data-stu-id="375e5-1266">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="375e5-1267">可以使用 X509 憑證來保護待用的金鑰。</span><span class="sxs-lookup"><span data-stu-id="375e5-1267">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="375e5-1268">請考慮下列讓使用者上傳憑證的機制：將憑證放入使用者的受信任憑證存放區，並確保在執行使用者應用程式的所有電腦上都能使用這些憑證。</span><span class="sxs-lookup"><span data-stu-id="375e5-1268">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="375e5-1269">如需詳細資訊，請參閱[設定 ASP.NET Core 資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1269">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="375e5-1270">**設定 IIS 應用程式集區載入使用者設定檔**</span><span class="sxs-lookup"><span data-stu-id="375e5-1270">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="375e5-1271">此設定位在應用程式集區 [進階設定] 下的 [處理序模型] 區段中。</span><span class="sxs-lookup"><span data-stu-id="375e5-1271">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="375e5-1272">將 [載入使用者設定檔] 設為 `True`。</span><span class="sxs-lookup"><span data-stu-id="375e5-1272">Set **Load User Profile** to `True`.</span></span> <span data-ttu-id="375e5-1273">當設定為 `True` 時，金鑰會儲存在使用者設定檔目錄中，且使用具有使用者帳戶專屬金鑰的 DPAPI 保護。</span><span class="sxs-lookup"><span data-stu-id="375e5-1273">When set to `True`, keys are stored in the user profile directory and protected using DPAPI with a key specific to the user account.</span></span> <span data-ttu-id="375e5-1274">金鑰會保存到 *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="375e5-1274">Keys are persisted to the *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* folder.</span></span>

  <span data-ttu-id="375e5-1275">應用程式集區的 [setProfileEnvironment 屬性](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration)也必須啟用。</span><span class="sxs-lookup"><span data-stu-id="375e5-1275">The app pool's [setProfileEnvironment attribute](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) must also be enabled.</span></span> <span data-ttu-id="375e5-1276">`setProfileEnvironment` 的預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="375e5-1276">The default value of `setProfileEnvironment` is `true`.</span></span> <span data-ttu-id="375e5-1277">在某些情況下 (例如 Windows OS)，`setProfileEnvironment` 會設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="375e5-1277">In some scenarios (for example, Windows OS), `setProfileEnvironment` is set to `false`.</span></span> <span data-ttu-id="375e5-1278">如果金鑰並未如預期地儲存在使用者設定檔目錄中：</span><span class="sxs-lookup"><span data-stu-id="375e5-1278">If keys aren't stored in the user profile directory as expected:</span></span>

  1. <span data-ttu-id="375e5-1279">瀏覽至 *%windir%/system32/inetsrv/config* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="375e5-1279">Navigate to the *%windir%/system32/inetsrv/config* folder.</span></span>
  1. <span data-ttu-id="375e5-1280">開啟 *applicationHost.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="375e5-1280">Open the *applicationHost.config* file.</span></span>
  1. <span data-ttu-id="375e5-1281">找出 `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` 元素。</span><span class="sxs-lookup"><span data-stu-id="375e5-1281">Locate the `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` element.</span></span>
  1. <span data-ttu-id="375e5-1282">確認 `setProfileEnvironment` 屬性不存在 (其預設值為 `true`)，或明確地將屬性值設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="375e5-1282">Confirm that the `setProfileEnvironment` attribute isn't present, which defaults the value to `true`, or explicitly set the attribute's value to `true`.</span></span>

* <span data-ttu-id="375e5-1283">**將檔案系統當作 Keyring 存放區使用**</span><span class="sxs-lookup"><span data-stu-id="375e5-1283">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="375e5-1284">調整應用程式程式碼，以[將檔案系統當作 Keyring 存放區使用](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1284">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="375e5-1285">使用 X509 憑證來保護 Keyring，並確保憑證是受信任的憑證。</span><span class="sxs-lookup"><span data-stu-id="375e5-1285">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="375e5-1286">如果憑證為自我簽署，請將憑證放在受信任的根存放區。</span><span class="sxs-lookup"><span data-stu-id="375e5-1286">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="375e5-1287">在 Web 伺服陣列中使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="375e5-1287">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="375e5-1288">請使用所有電腦都可以存取的檔案共用。</span><span class="sxs-lookup"><span data-stu-id="375e5-1288">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="375e5-1289">將 X509 憑證部署到每一部電腦。</span><span class="sxs-lookup"><span data-stu-id="375e5-1289">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="375e5-1290">設定[程式碼中的資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1290">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="375e5-1291">**設定資料保護的全電腦原則**</span><span class="sxs-lookup"><span data-stu-id="375e5-1291">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="375e5-1292">針對取用資料保護 API 的所有應用程式，資料保護系統僅支援有限的預設[全電腦原則](xref:security/data-protection/configuration/machine-wide-policy)設定。</span><span class="sxs-lookup"><span data-stu-id="375e5-1292">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="375e5-1293">如需詳細資訊，請參閱 <xref:security/data-protection/introduction>。</span><span class="sxs-lookup"><span data-stu-id="375e5-1293">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="virtual-directories"></a><span data-ttu-id="375e5-1294">虛擬目錄</span><span class="sxs-lookup"><span data-stu-id="375e5-1294">Virtual Directories</span></span>

<span data-ttu-id="375e5-1295">ASP.NET Core 應用程式不支援 [IIS 虛擬目錄](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1295">[IIS Virtual Directories](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) aren't supported with ASP.NET Core apps.</span></span> <span data-ttu-id="375e5-1296">應用程式能以[子應用程式](#sub-applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="375e5-1296">An app can be hosted as a [sub-application](#sub-applications).</span></span>

## <a name="sub-applications"></a><span data-ttu-id="375e5-1297">子應用程式</span><span class="sxs-lookup"><span data-stu-id="375e5-1297">Sub-applications</span></span>

<span data-ttu-id="375e5-1298">ASP.NET Core 應用程式能以 [IIS 子應用程式](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="375e5-1298">An ASP.NET Core app can be hosted as an [IIS sub-application (sub-app)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications).</span></span> <span data-ttu-id="375e5-1299">子應用程式的路徑會成為根應用程式 URL 的一部分。</span><span class="sxs-lookup"><span data-stu-id="375e5-1299">The sub-app's path becomes part of the root app's URL.</span></span>

<span data-ttu-id="375e5-1300">子應用程式不應該包括 ASP.NET Core 模組做為處理常式。</span><span class="sxs-lookup"><span data-stu-id="375e5-1300">A sub-app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="375e5-1301">如果您在子應用程式的 *web.config* 檔案中將模組新增為處理常式，則嘗試瀏覽子應用程式時，會收到參考錯誤設定檔的 *500.19 內部伺服器錯誤*。</span><span class="sxs-lookup"><span data-stu-id="375e5-1301">If the module is added as a handler in a sub-app's *web.config* file, a *500.19 Internal Server Error* referencing the faulty config file is received when attempting to browse the sub-app.</span></span>

<span data-ttu-id="375e5-1302">下列範例顯示 ASP.NET Core 子應用程式的已發佈 *web.config* 檔案：</span><span class="sxs-lookup"><span data-stu-id="375e5-1302">The following example shows a published *web.config* file for an ASP.NET Core sub-app:</span></span>

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

<span data-ttu-id="375e5-1303">在 ASP.NET Core 應用程式下裝載非 ASP.NET Core 子應用程式時，請明確移除子應用程式 *web.config* 檔案中已繼承的處理常式：</span><span class="sxs-lookup"><span data-stu-id="375e5-1303">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app's *web.config* file:</span></span>

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

<span data-ttu-id="375e5-1304">子應用程式內的靜態資產連結應該使用波狀符號與斜線 (`~/`) 標記法。</span><span class="sxs-lookup"><span data-stu-id="375e5-1304">Static asset links within the sub-app should use tilde-slash (`~/`) notation.</span></span> <span data-ttu-id="375e5-1305">波狀符號與斜線標記法會觸發[標記協助程式](xref:mvc/views/tag-helpers/intro)以將子應用程式的路徑基底附加到轉譯的相對連結前面。</span><span class="sxs-lookup"><span data-stu-id="375e5-1305">Tilde-slash notation triggers a [Tag Helper](xref:mvc/views/tag-helpers/intro) to prepend the sub-app's pathbase to the rendered relative link.</span></span> <span data-ttu-id="375e5-1306">針對位於 `/subapp_path` 的子應用程式，使用 `src="~/image.png"` 連結的影像會轉譯為 `src="/subapp_path/image.png"`。</span><span class="sxs-lookup"><span data-stu-id="375e5-1306">For a sub-app at `/subapp_path`, an image linked with `src="~/image.png"` is rendered as `src="/subapp_path/image.png"`.</span></span> <span data-ttu-id="375e5-1307">根應用程式的靜態檔案中介軟體不會處理靜態檔案要求。</span><span class="sxs-lookup"><span data-stu-id="375e5-1307">The root app's Static File Middleware doesn't process the static file request.</span></span> <span data-ttu-id="375e5-1308">要求會由子應用程式的靜態檔案中介軟體處理。</span><span class="sxs-lookup"><span data-stu-id="375e5-1308">The request is processed by the sub-app's Static File Middleware.</span></span>

<span data-ttu-id="375e5-1309">若靜態資產的 `src` 屬性是設定為絕對路徑 (例如，`src="/image.png"`)，會以不使用子應用程式路徑基底的方式轉譯連結。</span><span class="sxs-lookup"><span data-stu-id="375e5-1309">If a static asset's `src` attribute is set to an absolute path (for example, `src="/image.png"`), the link is rendered without the sub-app's pathbase.</span></span> <span data-ttu-id="375e5-1310">根應用程式的靜態檔案中介軟體會嘗試從根應用程式的 [webroot](xref:fundamentals/index#web-root) 提供資產，這會導致「404 - 找不到」回應 (除非根應用程式可存取靜態資產)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1310">The root app's Static File Middleware attempts to serve the asset from the root app's [web root](xref:fundamentals/index#web-root), which results in a *404 - Not Found* response unless the static asset is available from the root app.</span></span>

<span data-ttu-id="375e5-1311">裝載 ASP.NET Core 應用程式做為另一個 ASP.NET Core 應用程式下的子應用程式：</span><span class="sxs-lookup"><span data-stu-id="375e5-1311">To host an ASP.NET Core app as a sub-app under another ASP.NET Core app:</span></span>

1. <span data-ttu-id="375e5-1312">為子應用程式建立應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="375e5-1312">Establish an app pool for the sub-app.</span></span> <span data-ttu-id="375e5-1313">將 [.NET CLR 版本] 設定為 [沒有受控碼]，因為會將核心通用語言執行平台 (CoreCLR) 開機以在背景工作處理序中裝載應用程式，而非在桌面 CLR (.NET CLR) 中裝載。</span><span class="sxs-lookup"><span data-stu-id="375e5-1313">Set the **.NET CLR Version** to **No Managed Code** because the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process, not the desktop CLR (.NET CLR).</span></span>

1. <span data-ttu-id="375e5-1314">使用根網站下資料夾中的子應用程式在 IIS 管理員中新增根網站。</span><span class="sxs-lookup"><span data-stu-id="375e5-1314">Add the root site in IIS Manager with the sub-app in a folder under the root site.</span></span>

1. <span data-ttu-id="375e5-1315">以滑鼠右鍵按一下 IIS 管理員中的子應用程式資料夾，然後選取 [轉換成應用程式]。</span><span class="sxs-lookup"><span data-stu-id="375e5-1315">Right-click the sub-app folder in IIS Manager and select **Convert to Application**.</span></span>

1. <span data-ttu-id="375e5-1316">在 [新增應用程式] 對話方塊中，使用 [應用程式集區] 的[選取] 按鈕來指派您為子應用程式建立的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="375e5-1316">In the **Add Application** dialog, use the **Select** button for the **Application Pool** to assign the app pool that you created for the sub-app.</span></span> <span data-ttu-id="375e5-1317">選取 **[確定]** 。</span><span class="sxs-lookup"><span data-stu-id="375e5-1317">Select **OK**.</span></span>

<span data-ttu-id="375e5-1318">將不同的應用程式集區指派給子應用程式是使用同處理序裝載模型。</span><span class="sxs-lookup"><span data-stu-id="375e5-1318">The assignment of a separate app pool to the sub-app is a requirement when using the in-process hosting model.</span></span>

<span data-ttu-id="375e5-1319">如需有關同進程裝載模型和設定 ASP.NET Core 模組的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="375e5-1319">For more information on the in-process hosting model and configuring the ASP.NET Core Module, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="375e5-1320">使用 web.config 的 IIS 組態</span><span class="sxs-lookup"><span data-stu-id="375e5-1320">Configuration of IIS with web.config</span></span>

<span data-ttu-id="375e5-1321">在對使用了 ASP.NET Core 模組的 ASP.NET Core 有作用的 IIS 情境下，設定會受 `<system.webServer>`web.config*的* 區段影響。</span><span class="sxs-lookup"><span data-stu-id="375e5-1321">IIS configuration is influenced by the `<system.webServer>` section of *web.config* for IIS scenarios that are functional for ASP.NET Core apps with the ASP.NET Core Module.</span></span> <span data-ttu-id="375e5-1322">舉例來說，IIS 設定對動態壓縮有作用。</span><span class="sxs-lookup"><span data-stu-id="375e5-1322">For example, IIS configuration is functional for dynamic compression.</span></span> <span data-ttu-id="375e5-1323">如果在伺服器層級將 IIS 設為使用動態壓縮，應用程式 `<urlCompression>`web.config*檔案中的* 元素則可為 ASP.NET Core 應用程式予以停用。</span><span class="sxs-lookup"><span data-stu-id="375e5-1323">If IIS is configured at the server level to use dynamic compression, the `<urlCompression>` element in the app's *web.config* file can disable it for an ASP.NET Core app.</span></span>

<span data-ttu-id="375e5-1324">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="375e5-1324">For more information, see the following topics:</span></span>

* [<span data-ttu-id="375e5-1325">\<System.webserver > 的設定參考</span><span class="sxs-lookup"><span data-stu-id="375e5-1325">Configuration reference for \<system.webServer></span></span>](/iis/configuration/system.webServer/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>

<span data-ttu-id="375e5-1326">若要設定在隔離的應用程式集區中執行之個別應用程式的環境變數 (支援 IIS 10.0 或更新版本)，請參閱 IIS 參考文件之*環境變數* environmentVariables>[ 主題的 \<AppCmd.exe 命令](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe)一節。</span><span class="sxs-lookup"><span data-stu-id="375e5-1326">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="375e5-1327">web.config 的組態區段</span><span class="sxs-lookup"><span data-stu-id="375e5-1327">Configuration sections of web.config</span></span>

<span data-ttu-id="375e5-1328">ASP.NET Core 應用程式的設定不使用 *web.config* 中 ASP.NET 4.x 應用程式的設定區段：</span><span class="sxs-lookup"><span data-stu-id="375e5-1328">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

<span data-ttu-id="375e5-1329">使用其他組態提供者設定的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-1329">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="375e5-1330">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1330">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="375e5-1331">應用程式集區</span><span class="sxs-lookup"><span data-stu-id="375e5-1331">Application Pools</span></span>

<span data-ttu-id="375e5-1332">在伺服器上裝載多個網站時，建議您在其各自的應用程式集區中執行各個應用程式，讓應用程式彼此隔離。</span><span class="sxs-lookup"><span data-stu-id="375e5-1332">When hosting multiple websites on a server, we recommend isolating the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="375e5-1333">IIS [新增網站] 對話方塊預設成此組態。</span><span class="sxs-lookup"><span data-stu-id="375e5-1333">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="375e5-1334">當提供 [網站名稱] 時，文字會自動轉移至 [應用程式集區] 文字方塊。</span><span class="sxs-lookup"><span data-stu-id="375e5-1334">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="375e5-1335">新增網站時，會使用該網站名稱建立新的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="375e5-1335">A new app pool is created using the site name when the site is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="375e5-1336">應用程式集區識別碼</span><span class="sxs-lookup"><span data-stu-id="375e5-1336">Application Pool Identity</span></span>

<span data-ttu-id="375e5-1337">應用程式集區身分識別帳戶可讓應用程式在唯一的帳戶下執行，不必建立及管理網域或本機帳戶。</span><span class="sxs-lookup"><span data-stu-id="375e5-1337">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="375e5-1338">在 IIS 8.0 或更新版本中，IIS 管理背景工作處理序 (WAS) 會使用新的應用程式集區名稱建立虛擬帳戶，並預設在此帳戶下執行應用程式集區的背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="375e5-1338">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="375e5-1339">在 IIS 管理主控台中，於應用程式集區的 [進階設定] 下，確定 [身分識別] 設定為使用 **ApplicationPoolIdentity**：</span><span class="sxs-lookup"><span data-stu-id="375e5-1339">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![應用程式集區進階設定對話方塊](index/_static/apppool-identity.png)

<span data-ttu-id="375e5-1341">IIS 管理程序會在 Windows 安全系統中，以應用程式集區的名稱建立安全識別碼。</span><span class="sxs-lookup"><span data-stu-id="375e5-1341">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="375e5-1342">可使用此身分識別來保護資源。</span><span class="sxs-lookup"><span data-stu-id="375e5-1342">Resources can be secured using this identity.</span></span> <span data-ttu-id="375e5-1343">不過，此身分識別不是真正的使用者帳戶，也不會顯示在 Windows 使用者管理主控台中。</span><span class="sxs-lookup"><span data-stu-id="375e5-1343">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="375e5-1344">如果 IIS 背景工作處理序需要提升應用程式的存取權限，請修改包含應用程式的目錄存取控制清單 (ACL)：</span><span class="sxs-lookup"><span data-stu-id="375e5-1344">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="375e5-1345">開啟 Windows 檔案總管，巡覽至目錄。</span><span class="sxs-lookup"><span data-stu-id="375e5-1345">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="375e5-1346">以滑鼠右鍵按一下目錄並選取 [屬性]。</span><span class="sxs-lookup"><span data-stu-id="375e5-1346">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="375e5-1347">依序選取 [安全性] 索引標籤下的 [編輯] 按鈕和 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="375e5-1347">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="375e5-1348">選取 [位置] 按鈕，並確定選取系統。</span><span class="sxs-lookup"><span data-stu-id="375e5-1348">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="375e5-1349">在 [輸入要選取的物件名稱] **\\ 區域中，輸入** IIS AppPool **<app_pool_name>** 。</span><span class="sxs-lookup"><span data-stu-id="375e5-1349">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="375e5-1350">選取 [檢查名稱] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="375e5-1350">Select the **Check Names** button.</span></span> <span data-ttu-id="375e5-1351">針對 [DefaultAppPool]，請使用 **IIS AppPool\DefaultAppPool** 檢查名稱。</span><span class="sxs-lookup"><span data-stu-id="375e5-1351">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="375e5-1352">選取 [檢查名稱] 按鈕時，[DefaultAppPool] 的值便會顯示於物件名稱區域中。</span><span class="sxs-lookup"><span data-stu-id="375e5-1352">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="375e5-1353">您無法直接將應用程式集區名稱輸入至物件名稱區域。</span><span class="sxs-lookup"><span data-stu-id="375e5-1353">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="375e5-1354">檢查物件名稱時，請使用 **IIS AppPool\\<app_pool_name>** 的格式。</span><span class="sxs-lookup"><span data-stu-id="375e5-1354">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：在選取 [檢查名稱] 之前，"DefaultAppPool" 這個應用程式集區名稱在物件名稱區域中會附加至 "IIS AppPool\"。](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="375e5-1356">選取 **[確定]** 。</span><span class="sxs-lookup"><span data-stu-id="375e5-1356">Select **OK**.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：選取 [檢查名稱] 之後，物件名稱 "DefaultAppPool" 會顯示在物件名稱區域中。](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="375e5-1358">預設應該會授與讀取 &amp; 執行權限。</span><span class="sxs-lookup"><span data-stu-id="375e5-1358">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="375e5-1359">請視需要提供其他權限。</span><span class="sxs-lookup"><span data-stu-id="375e5-1359">Provide additional permissions as needed.</span></span>

<span data-ttu-id="375e5-1360">也可使用 **ICACLS** 工具透過命令提示字元授與存取權限。</span><span class="sxs-lookup"><span data-stu-id="375e5-1360">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="375e5-1361">使用 *DefaultAppPool*作為範例，將會使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="375e5-1361">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="375e5-1362">如需詳細資訊，請參閱 [icacls](/windows-server/administration/windows-commands/icacls) 主題。</span><span class="sxs-lookup"><span data-stu-id="375e5-1362">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="http2-support"></a><span data-ttu-id="375e5-1363">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="375e5-1363">HTTP/2 support</span></span>

<span data-ttu-id="375e5-1364">[HTTP/2](https://httpwg.org/specs/rfc7540.html) 支援符合下列基本需求的跨處理序部署：</span><span class="sxs-lookup"><span data-stu-id="375e5-1364">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported for out-of-process deployments that meet the following base requirements:</span></span>

* <span data-ttu-id="375e5-1365">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="375e5-1365">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
* <span data-ttu-id="375e5-1366">公開 Edge Server 連線使用 HTTP/2，但是對 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的反向 Proxy 連線使用 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="375e5-1366">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
* <span data-ttu-id="375e5-1367">目標 Framework：不適用於跨處理序部署，因為 HTTP/2 連線完全由 IIS 處理。</span><span class="sxs-lookup"><span data-stu-id="375e5-1367">Target framework: Not applicable to out-of-process deployments, since the HTTP/2 connection is handled entirely by IIS.</span></span>
* <span data-ttu-id="375e5-1368">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="375e5-1368">TLS 1.2 or later connection</span></span>

<span data-ttu-id="375e5-1369">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/1.1`。</span><span class="sxs-lookup"><span data-stu-id="375e5-1369">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="375e5-1370">HTTP/2 預設為啟用。</span><span class="sxs-lookup"><span data-stu-id="375e5-1370">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="375e5-1371">如果 HTTP/2 連線尚未建立，連線會退為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="375e5-1371">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="375e5-1372">如需使用 IIS 部署之 HTTP/2 設定的詳細資訊，請參閱 [IIS 上的 HTTP/2](/iis/get-started/whats-new-in-iis-10/http2-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1372">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="cors-preflight-requests"></a><span data-ttu-id="375e5-1373">CORS 預檢要求</span><span class="sxs-lookup"><span data-stu-id="375e5-1373">CORS preflight requests</span></span>

<span data-ttu-id="375e5-1374">*此節只適用於以 .NET Framework 為目標的 ASP.NET Core 應用程式。*</span><span class="sxs-lookup"><span data-stu-id="375e5-1374">*This section only applies to ASP.NET Core apps that target the .NET Framework.*</span></span>

<span data-ttu-id="375e5-1375">針對以 .NET Framework 為目標的 ASP.NET Core 應用程式，在 IIS 中OPTIONS 要求預設不會傳遞到應用程式。</span><span class="sxs-lookup"><span data-stu-id="375e5-1375">For an ASP.NET Core app that targets the .NET Framework, OPTIONS requests aren't passed to the app by default in IIS.</span></span> <span data-ttu-id="375e5-1376">若要瞭解如何在 web.config 中設定應用程式的 IIS 處理*程式來傳遞*選項要求，請參閱[在 ASP.NET Web API 2 中啟用跨原始來源要求： CORS 的運作方式](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works)。</span><span class="sxs-lookup"><span data-stu-id="375e5-1376">To learn how to configure the app's IIS handlers in *web.config* to pass OPTIONS requests, see [Enable cross-origin requests in ASP.NET Web API 2: How CORS Works](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).</span></span>

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="375e5-1377">IIS 系統管理員的部署資源</span><span class="sxs-lookup"><span data-stu-id="375e5-1377">Deployment resources for IIS administrators</span></span>

* [<span data-ttu-id="375e5-1378">IIS 文件</span><span class="sxs-lookup"><span data-stu-id="375e5-1378">IIS documentation</span></span>](/iis)
* [<span data-ttu-id="375e5-1379">IIS 中的 IIS 管理員使用者入門</span><span class="sxs-lookup"><span data-stu-id="375e5-1379">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* [<span data-ttu-id="375e5-1380">.NET Core 應用程式部署</span><span class="sxs-lookup"><span data-stu-id="375e5-1380">.NET Core application deployment</span></span>](/dotnet/core/deploying/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/directory-structure>
* <xref:host-and-deploy/iis/modules>
* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>

## <a name="additional-resources"></a><span data-ttu-id="375e5-1381">其他資源</span><span class="sxs-lookup"><span data-stu-id="375e5-1381">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:index>
* [<span data-ttu-id="375e5-1382">Microsoft IIS 官方網站</span><span class="sxs-lookup"><span data-stu-id="375e5-1382">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="375e5-1383">Windows Server 技術內容庫</span><span class="sxs-lookup"><span data-stu-id="375e5-1383">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="375e5-1384">IIS 上的 HTTP/2</span><span class="sxs-lookup"><span data-stu-id="375e5-1384">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
* <xref:host-and-deploy/iis/transform-webconfig>

::: moniker-end
