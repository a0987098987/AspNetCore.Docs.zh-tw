---
title: 在使用 IIS 的 Windows 上裝載 ASP.NET Core
author: rick-anderson
description: 了解如何在 Windows Server Internet Information Services (IIS) 上裝載 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/17/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: host-and-deploy/iis/index
ms.openlocfilehash: 72f433ffdc7d08e23fb68fc6ed9903a39959363b
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82775982"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="3f193-103">在使用 IIS 的 Windows 上裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f193-103">Host ASP.NET Core on Windows with IIS</span></span>

<!-- 

    NOTE FOR 5.0
    
    When making the 5.0 version of this topic, remove the Hosting Bundle
    direct download section from the (new) <5.0 & >2.2 version and modify 
    the text and heading for the *Earlier versions of the installer* 
    section. See the 2.2 version for an example.
    
-->

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3f193-104">如需將 ASP.NET Core 應用程式發佈至 IIS 伺服器的教學課程體驗，請參閱<xref:tutorials/publish-to-iis>。</span><span class="sxs-lookup"><span data-stu-id="3f193-104">For a tutorial experience on publishing an ASP.NET Core app to an IIS server, see <xref:tutorials/publish-to-iis>.</span></span>

[<span data-ttu-id="3f193-105">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="3f193-105">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="3f193-106">支援的作業系統</span><span class="sxs-lookup"><span data-stu-id="3f193-106">Supported operating systems</span></span>

<span data-ttu-id="3f193-107">以下為支援的作業系統：</span><span class="sxs-lookup"><span data-stu-id="3f193-107">The following operating systems are supported:</span></span>

* <span data-ttu-id="3f193-108">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="3f193-108">Windows 7 or later</span></span>
* <span data-ttu-id="3f193-109">Windows Server 2012 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="3f193-109">Windows Server 2012 R2 or later</span></span>

<span data-ttu-id="3f193-110">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys) (先前稱為 WebListener) 不適用搭配 IIS 的反向 Proxy 設定。</span><span class="sxs-lookup"><span data-stu-id="3f193-110">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="3f193-111">請使用 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="3f193-111">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="3f193-112">如需在 Azure 中裝載的資訊，請參閱 <xref:host-and-deploy/azure-apps/index>。</span><span class="sxs-lookup"><span data-stu-id="3f193-112">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

<span data-ttu-id="3f193-113">如需疑難排解指引，請參閱 <xref:test/troubleshoot>。</span><span class="sxs-lookup"><span data-stu-id="3f193-113">For troubleshooting guidance, see <xref:test/troubleshoot>.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="3f193-114">支援的平台</span><span class="sxs-lookup"><span data-stu-id="3f193-114">Supported platforms</span></span>

<span data-ttu-id="3f193-115">支援針對 32 位元 (x86) 或 64 位元 (x64) 部署發行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-115">Apps published for 32-bit (x86) or 64-bit (x64) deployment are supported.</span></span> <span data-ttu-id="3f193-116">使用 32 位元 (x86) .NET Core SDK 部署 32 位元應用程式，除非應用程式：</span><span class="sxs-lookup"><span data-stu-id="3f193-116">Deploy a 32-bit app with a 32-bit (x86) .NET Core SDK unless the app:</span></span>

* <span data-ttu-id="3f193-117">需要提供給 64 位元應用程式使用的較大虛擬記憶體位址空間。</span><span class="sxs-lookup"><span data-stu-id="3f193-117">Requires the larger virtual memory address space available to a 64-bit app.</span></span>
* <span data-ttu-id="3f193-118">需要較大的 IIS 堆疊大小。</span><span class="sxs-lookup"><span data-stu-id="3f193-118">Requires the larger IIS stack size.</span></span>
* <span data-ttu-id="3f193-119">有 64 位元原生相依性。</span><span class="sxs-lookup"><span data-stu-id="3f193-119">Has 64-bit native dependencies.</span></span>

<span data-ttu-id="3f193-120">使用 64 位元 (x64) .NET Core SDK 來發行 64 位元應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-120">Use a 64-bit (x64) .NET Core SDK to publish a 64-bit app.</span></span> <span data-ttu-id="3f193-121">主機系統上必須有 64 位元執行階段存在。</span><span class="sxs-lookup"><span data-stu-id="3f193-121">A 64-bit runtime must be present on the host system.</span></span>

## <a name="hosting-models"></a><span data-ttu-id="3f193-122">裝載模型</span><span class="sxs-lookup"><span data-stu-id="3f193-122">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="3f193-123">同處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="3f193-123">In-process hosting model</span></span>

<span data-ttu-id="3f193-124">使用同處理序裝載，ASP.NET Core 應用程式會在與其 IIS 工作者處理序相同的處理序中執行。</span><span class="sxs-lookup"><span data-stu-id="3f193-124">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="3f193-125">因為要求未透過回送介面卡 (將連出網路流量傳回同一部電腦的網路介面) 進行 proxy 處理，所以同處理序裝載會提供優於跨處理序裝載的效能。</span><span class="sxs-lookup"><span data-stu-id="3f193-125">In-process hosting provides improved performance over out-of-process hosting because requests aren't proxied over the loopback adapter, a network interface that returns outgoing network traffic back to the same machine.</span></span> <span data-ttu-id="3f193-126">IIS 透過 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 來執行處理程序管理。</span><span class="sxs-lookup"><span data-stu-id="3f193-126">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="3f193-127">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)：</span><span class="sxs-lookup"><span data-stu-id="3f193-127">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module):</span></span>

* <span data-ttu-id="3f193-128">執行應用程式初始化。</span><span class="sxs-lookup"><span data-stu-id="3f193-128">Performs app initialization.</span></span>
  * <span data-ttu-id="3f193-129">載入 [CoreCLR](/dotnet/standard/glossary#coreclr)。</span><span class="sxs-lookup"><span data-stu-id="3f193-129">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="3f193-130">呼叫 `Program.Main`。</span><span class="sxs-lookup"><span data-stu-id="3f193-130">Calls `Program.Main`.</span></span>
* <span data-ttu-id="3f193-131">處理 IIS 原生要求的存留期。</span><span class="sxs-lookup"><span data-stu-id="3f193-131">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="3f193-132">以 .NET Framework 為目標的 ASP.NET Core 應用程式不支援處理序內裝載模型。</span><span class="sxs-lookup"><span data-stu-id="3f193-132">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="3f193-133">下圖說明 IIS、ASP.NET Core 模組和同處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="3f193-133">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![同處理序代管內的 ASP.NET Core 模組案例](index/_static/ancm-inprocess.png)

<span data-ttu-id="3f193-135">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-135">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="3f193-136">驅動程式會在網站設定的連接埠上將原生要求路由至 IIS，此連接埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="3f193-136">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="3f193-137">ASP.NET Core 模組會接收原生要求，並將它傳遞至 IIS HTTP`IISHttpServer`伺服器（）。</span><span class="sxs-lookup"><span data-stu-id="3f193-137">The ASP.NET Core Module receives the native request and passes it to IIS HTTP Server (`IISHttpServer`).</span></span> <span data-ttu-id="3f193-138">IIS HTTP 伺服器是 IIS 的同處理序伺服程式實作，可將要求從原生轉換為受控。</span><span class="sxs-lookup"><span data-stu-id="3f193-138">IIS HTTP Server is an in-process server implementation for IIS that converts the request from native to managed.</span></span>

<span data-ttu-id="3f193-139">IIS HTTP 伺服器處理要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="3f193-139">After the IIS HTTP Server processes the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="3f193-140">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="3f193-140">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="3f193-141">應用程式的回應會透過 IIS HTTP 伺服器傳回 IIS。</span><span class="sxs-lookup"><span data-stu-id="3f193-141">The app's response is passed back to IIS through IIS HTTP Server.</span></span> <span data-ttu-id="3f193-142">IIS 會將回應傳送到起始該要求的用戶端。</span><span class="sxs-lookup"><span data-stu-id="3f193-142">IIS sends the response to the client that initiated the request.</span></span>

<span data-ttu-id="3f193-143">現有的應用程式可以選擇同處理序裝載，但 [dotnet new](/dotnet/core/tools/dotnet-new) 範本預設會對所有 IIS 和 IIS Express 案例使用同處理序裝載模型。</span><span class="sxs-lookup"><span data-stu-id="3f193-143">In-process hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="3f193-144">`CreateDefaultBuilder` 會透過呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> 方法來啟動 [CoreCLR](/dotnet/standard/glossary#coreclr) 以新增 <xref:Microsoft.AspNetCore.Hosting.Server.IServer>，並在 IIS 工作者處理序 (*w3wp.exe* 或 *iisexpress.exe*) 內裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-144">`CreateDefaultBuilder` adds an <xref:Microsoft.AspNetCore.Hosting.Server.IServer> instance by calling the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> method to boot the [CoreCLR](/dotnet/standard/glossary#coreclr) and host the app inside of the IIS worker process (*w3wp.exe* or *iisexpress.exe*).</span></span> <span data-ttu-id="3f193-145">效能測試指出，相較於跨處理序裝載 .NET Core 應用程式，並將要求 Proxy 處理至 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器，裝載於同處理序可提供明顯更高的要求輸送量。</span><span class="sxs-lookup"><span data-stu-id="3f193-145">Performance tests indicate that hosting a .NET Core app in-process delivers significantly higher request throughput compared to hosting the app out-of-process and proxying requests to [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

> [!NOTE]
> <span data-ttu-id="3f193-146">發佈為單一檔案可執行檔的應用程式無法由同處理序裝載模型載入。</span><span class="sxs-lookup"><span data-stu-id="3f193-146">Apps published as a single file executable can't be loaded by the in-process hosting model.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="3f193-147">跨處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="3f193-147">Out-of-process hosting model</span></span>

<span data-ttu-id="3f193-148">因為 ASP.NET Core 應用程式會在與 IIS 背景工作進程不同的進程中執行，所以 ASP.NET Core 模組會處理進程管理。</span><span class="sxs-lookup"><span data-stu-id="3f193-148">Because ASP.NET Core apps run in a process separate from the IIS worker process, the ASP.NET Core Module handles process management.</span></span> <span data-ttu-id="3f193-149">此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式關閉或損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="3f193-149">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="3f193-150">此行為基本上與執行同處理序，並由 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="3f193-150">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="3f193-151">下圖說明 IIS、ASP.NET Core 模組和跨處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="3f193-151">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![非同處理序代管內的 ASP.NET Core 模組案例](index/_static/ancm-outofprocess.png)

<span data-ttu-id="3f193-153">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-153">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="3f193-154">驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="3f193-154">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="3f193-155">此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，而且不會是通訊埠 80 或 443。</span><span class="sxs-lookup"><span data-stu-id="3f193-155">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="3f193-156">此模組在啟動時透過環境變數指定連接埠，而 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 延伸模組則會設定伺服器來接聽 `http://localhost:{PORT}`。</span><span class="sxs-lookup"><span data-stu-id="3f193-156">The module specifies the port via an environment variable at startup, and the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> extension configures the server to listen on `http://localhost:{PORT}`.</span></span> <span data-ttu-id="3f193-157">將會執行額外檢查，不是源自模組的要求都會遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="3f193-157">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="3f193-158">此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。</span><span class="sxs-lookup"><span data-stu-id="3f193-158">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="3f193-159">Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="3f193-159">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="3f193-160">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="3f193-160">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="3f193-161">IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="3f193-161">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="3f193-162">應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="3f193-162">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="3f193-163">如需 ASP.NET Core 模組組態指南，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="3f193-163">For ASP.NET Core Module configuration guidance, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="3f193-164">如需代管的詳細資訊，請參閱[在 ASP.NET Core 中代管](xref:fundamentals/index#host)。</span><span class="sxs-lookup"><span data-stu-id="3f193-164">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/index#host).</span></span>

## <a name="application-configuration"></a><span data-ttu-id="3f193-165">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="3f193-165">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="3f193-166">啟用 IISIntegration 元件</span><span class="sxs-lookup"><span data-stu-id="3f193-166">Enable the IISIntegration components</span></span>

<span data-ttu-id="3f193-167">在（Program.cs）中`CreateHostBuilder`建立*Program.cs*主機時，請<xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>呼叫以啟用 IIS 整合：</span><span class="sxs-lookup"><span data-stu-id="3f193-167">When building a host in `CreateHostBuilder` (*Program.cs*), call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> to enable IIS integration:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        ...
```

<span data-ttu-id="3f193-168">如需 `CreateDefaultBuilder` 的詳細資訊，請參閱 <xref:fundamentals/host/generic-host#default-builder-settings>。</span><span class="sxs-lookup"><span data-stu-id="3f193-168">For more information on `CreateDefaultBuilder`, see <xref:fundamentals/host/generic-host#default-builder-settings>.</span></span>

### <a name="iis-options"></a><span data-ttu-id="3f193-169">IIS 選項</span><span class="sxs-lookup"><span data-stu-id="3f193-169">IIS options</span></span>

<span data-ttu-id="3f193-170">**同處理序主控模型**</span><span class="sxs-lookup"><span data-stu-id="3f193-170">**In-process hosting model**</span></span>

<span data-ttu-id="3f193-171">若要設定 IIS 伺服器選項，請在 <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 中加入 <xref:Microsoft.AspNetCore.Builder.IISServerOptions> 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="3f193-171">To configure IIS Server options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISServerOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="3f193-172">下列範例會停用 AutomaticAuthentication：</span><span class="sxs-lookup"><span data-stu-id="3f193-172">The following example disables AutomaticAuthentication:</span></span>

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

| <span data-ttu-id="3f193-173">選項</span><span class="sxs-lookup"><span data-stu-id="3f193-173">Option</span></span>                         | <span data-ttu-id="3f193-174">預設</span><span class="sxs-lookup"><span data-stu-id="3f193-174">Default</span></span> | <span data-ttu-id="3f193-175">設定</span><span class="sxs-lookup"><span data-stu-id="3f193-175">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="3f193-176">若為 `true`，IIS 伺服器會設定由 [Windows 驗證](xref:security/authentication/windowsauth)所驗證的 `HttpContext.User`。</span><span class="sxs-lookup"><span data-stu-id="3f193-176">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="3f193-177">若為 `false`，則伺服器僅會對 `HttpContext.User` 提供身分識別，並在 `AuthenticationScheme` 明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="3f193-177">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="3f193-178">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="3f193-178">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="3f193-179">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="3f193-179">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="3f193-180">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="3f193-180">Sets the display name shown to users on login pages.</span></span> |
| `AllowSynchronousIO`           | `false` | <span data-ttu-id="3f193-181">是否允許`HttpContext.Request`和的同步 i/o `HttpContext.Response`。</span><span class="sxs-lookup"><span data-stu-id="3f193-181">Whether synchronous I/O is allowed for the `HttpContext.Request` and the `HttpContext.Response`.</span></span> |
| `MaxRequestBodySize`           | `30000000`  | <span data-ttu-id="3f193-182">取得或設定 `HttpRequest` 的要求本文大小上限。</span><span class="sxs-lookup"><span data-stu-id="3f193-182">Gets or sets the max request body size for the `HttpRequest`.</span></span> <span data-ttu-id="3f193-183">請注意，IIS 本身具有限制 `maxAllowedContentLength`，此限制將在 `IISServerOptions` 中設定 `MaxRequestBodySize` 時處理。</span><span class="sxs-lookup"><span data-stu-id="3f193-183">Note that IIS itself has the limit `maxAllowedContentLength` which will be processed before the `MaxRequestBodySize` set in the `IISServerOptions`.</span></span> <span data-ttu-id="3f193-184">變更 `MaxRequestBodySize` 將不會影響 `maxAllowedContentLength`。</span><span class="sxs-lookup"><span data-stu-id="3f193-184">Changing the `MaxRequestBodySize` won't affect the `maxAllowedContentLength`.</span></span> <span data-ttu-id="3f193-185">若要增加 `maxAllowedContentLength`，請在 *web.config* 中新增項目，以將 `maxAllowedContentLength` 設定為較高的值。</span><span class="sxs-lookup"><span data-stu-id="3f193-185">To increase `maxAllowedContentLength`, add an entry in the *web.config* to set `maxAllowedContentLength` to a higher value.</span></span> <span data-ttu-id="3f193-186">如需更多詳細資料，請參閱[組態](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/#configuration)。</span><span class="sxs-lookup"><span data-stu-id="3f193-186">For more details, see [Configuration](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/#configuration).</span></span> |

<span data-ttu-id="3f193-187">**跨處理序裝載模型**</span><span class="sxs-lookup"><span data-stu-id="3f193-187">**Out-of-process hosting model**</span></span>

<span data-ttu-id="3f193-188">若要設定 IIS 選項，請在 <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 中加入 <xref:Microsoft.AspNetCore.Builder.IISOptions> 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="3f193-188">To configure IIS options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="3f193-189">下列範例會防止應用程式填入 `HttpContext.Connection.ClientCertificate`：</span><span class="sxs-lookup"><span data-stu-id="3f193-189">The following example prevents the app from populating `HttpContext.Connection.ClientCertificate`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="3f193-190">選項</span><span class="sxs-lookup"><span data-stu-id="3f193-190">Option</span></span>                         | <span data-ttu-id="3f193-191">預設</span><span class="sxs-lookup"><span data-stu-id="3f193-191">Default</span></span> | <span data-ttu-id="3f193-192">設定</span><span class="sxs-lookup"><span data-stu-id="3f193-192">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="3f193-193">若為 `true`，[IIS 整合中介軟體](#enable-the-iisintegration-components)會設定由 [Windows 驗證](xref:security/authentication/windowsauth)所驗證的 `HttpContext.User`。</span><span class="sxs-lookup"><span data-stu-id="3f193-193">If `true`, [IIS Integration Middleware](#enable-the-iisintegration-components) sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="3f193-194">如果為 `false`，則驗證中介軟體僅針對 `HttpContext.User` 提供身分識別，並在游 `AuthenticationScheme` 提出明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="3f193-194">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="3f193-195">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="3f193-195">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="3f193-196">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="3f193-196">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="3f193-197">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="3f193-197">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="3f193-198">如果為 `true` 且 `MS-ASPNETCORE-CLIENTCERT` 要求標頭已存在，則會填入 `HttpContext.Connection.ClientCertificate`。</span><span class="sxs-lookup"><span data-stu-id="3f193-198">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="3f193-199">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="3f193-199">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="3f193-200">用來設定轉送標頭中介軟體及 ASP.NET Core 模組的 [IIS 整合中介軟體](#enable-the-iisintegration-components)會設定為轉送配置 (HTTP/HTTPS) 與發出要求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="3f193-200">The [IIS Integration Middleware](#enable-the-iisintegration-components), which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="3f193-201">其他 Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。</span><span class="sxs-lookup"><span data-stu-id="3f193-201">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="3f193-202">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="3f193-202">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="3f193-203">web.config 檔案</span><span class="sxs-lookup"><span data-stu-id="3f193-203">web.config file</span></span>

<span data-ttu-id="3f193-204">*web.config* 檔案是用來設定 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="3f193-204">The *web.config* file configures the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="3f193-205">發佈專案時，由 MSBuild 目標 (`_TransformWebConfig`) 處理 *web.config* 檔案的建立、轉換及發佈。</span><span class="sxs-lookup"><span data-stu-id="3f193-205">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="3f193-206">此目標存在於 Web SDK 目標 (`Microsoft.NET.Sdk.Web`)。</span><span class="sxs-lookup"><span data-stu-id="3f193-206">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="3f193-207">SDK 設定在專案檔的頂端：</span><span class="sxs-lookup"><span data-stu-id="3f193-207">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="3f193-208">如果專案中沒有 *web.config* 檔案，則系統會使用正確的 *processPath* 和 *arguments* 建立該檔案以設定 ASP.NET Core 模組，並將該檔案移至[已發行的輸出](xref:host-and-deploy/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="3f193-208">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="3f193-209">如果 *web.config* 檔案存在於專案中，則系統會使用正確的 *processPath* 和 *arguments* 來轉換該檔案以設定 ASP.NET Core 模組，然後將它移至已發行的輸出。</span><span class="sxs-lookup"><span data-stu-id="3f193-209">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="3f193-210">轉換不會修改檔案中的 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="3f193-210">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="3f193-211">*web.config* 檔案可提供能控制作用中 IIS 模組的額外 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="3f193-211">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="3f193-212">如需能處理 ASP.NET Core 應用程式要求之 IIS 模組的相關資訊，請參閱 [IIS 模組](xref:host-and-deploy/iis/modules)主題。</span><span class="sxs-lookup"><span data-stu-id="3f193-212">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="3f193-213">為防止 Web SDK 轉換 *web.config* 檔案，請使用專案檔中的 **\<IsTransformWebConfigDisabled>** 屬性：</span><span class="sxs-lookup"><span data-stu-id="3f193-213">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="3f193-214">使 Web SDK 無法轉換檔案時，應該由開發人員手動設定 *processPath* 和 *arguments*。</span><span class="sxs-lookup"><span data-stu-id="3f193-214">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="3f193-215">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="3f193-215">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="3f193-216">web.config 檔案位置</span><span class="sxs-lookup"><span data-stu-id="3f193-216">web.config file location</span></span>

<span data-ttu-id="3f193-217">為了正確設定[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module) *，web.config 檔案*必須存在於已部署應用程式的[內容根](xref:fundamentals/index#content-root)路徑（通常是應用程式基底路徑）。</span><span class="sxs-lookup"><span data-stu-id="3f193-217">In order to set up the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) correctly, the *web.config* file must be present at the [content root](xref:fundamentals/index#content-root) path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="3f193-218">這是與提供給 IIS 的網站實體路徑相同的位置。</span><span class="sxs-lookup"><span data-stu-id="3f193-218">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="3f193-219">應用程式的根目錄需有 *web.config* 檔案，才能使用 Web Deploy 發行多個應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-219">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="3f193-220">機密檔案存在於應用程式的實體路徑，例如\* \<元件>. .runtimeconfig.json. json*、 \* \<元件> .xml* （xml 檔批註）和\* \<元件>. .deps.json。\*</span><span class="sxs-lookup"><span data-stu-id="3f193-220">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="3f193-221">當 *web.config* 檔案存在且網站正常啟動時，如果有人要求機密檔案，IIS 不會予以提供。</span><span class="sxs-lookup"><span data-stu-id="3f193-221">When the *web.config* file is present and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="3f193-222">若 *web.config* 檔案遺失或沒有正確命名，或是無法設定網站以正常啟動，IIS 可能會公開提供機密檔案。</span><span class="sxs-lookup"><span data-stu-id="3f193-222">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="3f193-223">\***Web.config*檔案必須隨時存在於部署中、正確命名，而且能夠將網站設定為正常啟動。絕對不要從生產環境部署*移除 web.config 檔案\*。**</span><span class="sxs-lookup"><span data-stu-id="3f193-223">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

### <a name="transform-webconfig"></a><span data-ttu-id="3f193-224">轉換 web.config</span><span class="sxs-lookup"><span data-stu-id="3f193-224">Transform web.config</span></span>

<span data-ttu-id="3f193-225">如需在發佈時轉換 *web.config* (例如依據設定、設定檔或環境設定環境變數)，請參閱<xref:host-and-deploy/iis/transform-webconfig>。</span><span class="sxs-lookup"><span data-stu-id="3f193-225">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="3f193-226">IIS 組態</span><span class="sxs-lookup"><span data-stu-id="3f193-226">IIS configuration</span></span>

<span data-ttu-id="3f193-227">**Windows Server 作業系統**</span><span class="sxs-lookup"><span data-stu-id="3f193-227">**Windows Server operating systems**</span></span>

<span data-ttu-id="3f193-228">啟用**網頁伺服器 (IIS)** 伺服器角色，並建立角色服務。</span><span class="sxs-lookup"><span data-stu-id="3f193-228">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="3f193-229">使用來自 [管理]\*\*\*\* 功能表的 [新增角色及功能]\*\*\*\* 精靈，或是 [伺服器管理員]\*\*\*\* 中的連結。</span><span class="sxs-lookup"><span data-stu-id="3f193-229">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="3f193-230">在**伺服器角色**步驟中，核取 [網頁伺服器 (IIS)]\*\*\*\* 方塊。</span><span class="sxs-lookup"><span data-stu-id="3f193-230">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![在選取伺服器角色步驟中選取網頁伺服器 IIS 角色。](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="3f193-232">在 [功能]\*\*\*\* 步驟之後，[角色服務]\*\*\*\* 步驟會針對網頁伺服器 (IIS) 進行載入。</span><span class="sxs-lookup"><span data-stu-id="3f193-232">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="3f193-233">選取所需的 IIS 角色服務或接受所提供的預設角色服務。</span><span class="sxs-lookup"><span data-stu-id="3f193-233">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![在選取角色服務步驟中，選取預設的角色服務。](index/_static/role-services-ws2016.png)

   <span data-ttu-id="3f193-235">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="3f193-235">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="3f193-236">若要啟用 Windows 驗證，請展開下列節點： [**網頁伺服器** > **安全性**]。</span><span class="sxs-lookup"><span data-stu-id="3f193-236">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="3f193-237">選取 [Windows 驗證]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="3f193-237">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="3f193-238">如需詳細資訊，請參閱 [Windows 驗證 \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) 和[設定 Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="3f193-238">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="3f193-239">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="3f193-239">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="3f193-240">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="3f193-240">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="3f193-241">若要啟用 websocket，請展開下列節點： [**網頁伺服器** > **應用程式開發**]。</span><span class="sxs-lookup"><span data-stu-id="3f193-241">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="3f193-242">選取 [WebSocket 通訊協定]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="3f193-242">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="3f193-243">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="3f193-243">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="3f193-244">透過**確認**步驟繼續作業，安裝網頁伺服器角色和服務。</span><span class="sxs-lookup"><span data-stu-id="3f193-244">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="3f193-245">安裝**網頁伺服器（iis）** 角色之後，不需要重新開機伺服器/iis。</span><span class="sxs-lookup"><span data-stu-id="3f193-245">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="3f193-246">**Windows 桌面作業系統**</span><span class="sxs-lookup"><span data-stu-id="3f193-246">**Windows desktop operating systems**</span></span>

<span data-ttu-id="3f193-247">啟用 [IIS 管理主控台]\*\*\*\* 和 [World Wide Web 服務]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-247">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="3f193-248">瀏覽到 [控制台]\*\* [程式]\*\* > \*\* [程式和功能]\*\* > \*\*\*\* > **[開啟或關閉 Windows 功能]** \(畫面左側\)。</span><span class="sxs-lookup"><span data-stu-id="3f193-248">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="3f193-249">開啟 [Internet Information Services]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="3f193-249">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="3f193-250">開啟 [Web 管理工具]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="3f193-250">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="3f193-251">核取 [IIS 管理主控台]\*\*\*\* 方塊。</span><span class="sxs-lookup"><span data-stu-id="3f193-251">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="3f193-252">[World Wide Web Services] (全球資訊網服務)\*\*\*\* 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="3f193-252">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="3f193-253">接受**全球資訊網服務**的預設功能，或自訂 IIS 功能。</span><span class="sxs-lookup"><span data-stu-id="3f193-253">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="3f193-254">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="3f193-254">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="3f193-255">若要啟用 Windows 驗證，請展開下列節點： **World Wide Web 服務** > **安全性**。</span><span class="sxs-lookup"><span data-stu-id="3f193-255">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="3f193-256">選取 [Windows 驗證]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="3f193-256">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="3f193-257">如需詳細資訊，請參閱 [Windows 驗證 \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) 和[設定 Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="3f193-257">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="3f193-258">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="3f193-258">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="3f193-259">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="3f193-259">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="3f193-260">若要啟用 websocket，請展開下列節點： [ **World Wide Web 服務** > ] [**應用程式開發] 功能**。</span><span class="sxs-lookup"><span data-stu-id="3f193-260">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="3f193-261">選取 [WebSocket 通訊協定]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="3f193-261">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="3f193-262">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="3f193-262">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="3f193-263">若 IIS 安裝需要重新啟動，請重新啟動系統。</span><span class="sxs-lookup"><span data-stu-id="3f193-263">If the IIS installation requires a restart, restart the system.</span></span>

![選取 [Windows 功能] 中的 [IIS 管理主控台] 和 [World Wide Web Services] (全球資訊網服務)。](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="3f193-265">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="3f193-265">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="3f193-266">在主控系統上安裝 .NET Core 裝載套件組合\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-266">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="3f193-267">套件組合會安裝 .NET Core 執行時間、.NET Core 程式庫和[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="3f193-267">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="3f193-268">此模組可讓 ASP.NET Core 應用程式在 IIS 背後執行。</span><span class="sxs-lookup"><span data-stu-id="3f193-268">The module allows ASP.NET Core apps to run behind IIS.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3f193-269">若裝載套件組合在 IIS 之前安裝，則必須對該套件組合安裝進行修復。</span><span class="sxs-lookup"><span data-stu-id="3f193-269">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="3f193-270">請在安裝 IIS 之後，再次執行裝載套件組合安裝程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-270">Run the Hosting Bundle installer again after installing IIS.</span></span>
>
> <span data-ttu-id="3f193-271">如果在安裝 64 位元 (x64) 版本的 .NET Core 後才安裝裝載套件組合，那麼可能會遺漏 SDK ([未偵測到 .NET Core SDK](xref:test/troubleshoot#no-net-core-sdks-were-detected))。</span><span class="sxs-lookup"><span data-stu-id="3f193-271">If the Hosting Bundle is installed after installing the 64-bit (x64) version of .NET Core, SDKs might appear to be missing ([No .NET Core SDKs were detected](xref:test/troubleshoot#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="3f193-272">若要解決此問題，請參閱 <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>。</span><span class="sxs-lookup"><span data-stu-id="3f193-272">To resolve the problem, see <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>.</span></span>

### <a name="direct-download-current-version"></a><span data-ttu-id="3f193-273">直接下載 (目前版本)</span><span class="sxs-lookup"><span data-stu-id="3f193-273">Direct download (current version)</span></span>

<span data-ttu-id="3f193-274">使用下列連結下載安裝程式：</span><span class="sxs-lookup"><span data-stu-id="3f193-274">Download the installer using the following link:</span></span>

[<span data-ttu-id="3f193-275">目前的 .NET Core 裝載套件組合安裝程式 (直接下載)</span><span class="sxs-lookup"><span data-stu-id="3f193-275">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a><span data-ttu-id="3f193-276">安裝程式的先前版本</span><span class="sxs-lookup"><span data-stu-id="3f193-276">Earlier versions of the installer</span></span>

<span data-ttu-id="3f193-277">若要取得安裝程式的先前版本：</span><span class="sxs-lookup"><span data-stu-id="3f193-277">To obtain an earlier version of the installer:</span></span>

1. <span data-ttu-id="3f193-278">流覽至 [[下載 .Net Core](https://dotnet.microsoft.com/download/dotnet-core) ] 頁面。</span><span class="sxs-lookup"><span data-stu-id="3f193-278">Navigate to the [Download .NET Core](https://dotnet.microsoft.com/download/dotnet-core) page.</span></span>
1. <span data-ttu-id="3f193-279">選取所需的 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="3f193-279">Select the desired .NET Core version.</span></span>
1. <span data-ttu-id="3f193-280">在 [執行應用程式 - 執行階段]\*\*\*\* 欄中，尋找想要的 .NET Core 執行階段版本列。</span><span class="sxs-lookup"><span data-stu-id="3f193-280">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="3f193-281">使用**裝載**套件組合連結來下載安裝程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-281">Download the installer using the **Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="3f193-282">某些安裝程式包含已達到期生命週期結束 (EOL) 的發行版本，這些發行版本已不受 Microsoft 支援。</span><span class="sxs-lookup"><span data-stu-id="3f193-282">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="3f193-283">如需詳細資訊，請參閱[支援原則](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) \(英文 \)。</span><span class="sxs-lookup"><span data-stu-id="3f193-283">For more information, see the [support policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="3f193-284">安裝裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="3f193-284">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="3f193-285">在伺服器上執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-285">Run the installer on the server.</span></span> <span data-ttu-id="3f193-286">從系統管理員命令殼層執行安裝程式時，有 下列參數可用：</span><span class="sxs-lookup"><span data-stu-id="3f193-286">The following parameters are available when running the installer from an administrator command shell:</span></span>

   * <span data-ttu-id="3f193-287">`OPT_NO_ANCM=1`&ndash;略過安裝 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="3f193-287">`OPT_NO_ANCM=1` &ndash; Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="3f193-288">`OPT_NO_RUNTIME=1`&ndash;略過安裝 .net Core 執行時間。</span><span class="sxs-lookup"><span data-stu-id="3f193-288">`OPT_NO_RUNTIME=1` &ndash; Skip installing the .NET Core runtime.</span></span> <span data-ttu-id="3f193-289">當伺服器只裝載[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)時使用。</span><span class="sxs-lookup"><span data-stu-id="3f193-289">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="3f193-290">`OPT_NO_SHAREDFX=1`&ndash;略過安裝 ASP.NET 共用架構（ASP.NET 執行時間）。</span><span class="sxs-lookup"><span data-stu-id="3f193-290">`OPT_NO_SHAREDFX=1` &ndash; Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span> <span data-ttu-id="3f193-291">當伺服器只裝載[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)時使用。</span><span class="sxs-lookup"><span data-stu-id="3f193-291">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="3f193-292">`OPT_NO_X86=1`&ndash;略過安裝 x86 執行時間。</span><span class="sxs-lookup"><span data-stu-id="3f193-292">`OPT_NO_X86=1` &ndash; Skip installing x86 runtimes.</span></span> <span data-ttu-id="3f193-293">當您確定不會裝載 32 位元應用程式時，請使用此參數。</span><span class="sxs-lookup"><span data-stu-id="3f193-293">Use this parameter when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="3f193-294">如果將來有可能同時裝載 32 位元和 64 位元應用程式，請不要使用此參數並安裝這兩個執行階段。</span><span class="sxs-lookup"><span data-stu-id="3f193-294">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this parameter and install both runtimes.</span></span>
   * <span data-ttu-id="3f193-295">`OPT_NO_SHARED_CONFIG_CHECK=1` &ndash; 停用使用 IIS 共用設定 (當共用設定 (*applicationHost.config*) 位於與 IIS 安裝相同的機器上時) 進行檢查。</span><span class="sxs-lookup"><span data-stu-id="3f193-295">`OPT_NO_SHARED_CONFIG_CHECK=1` &ndash; Disable the check for using an IIS Shared Configuration when the shared configuration (*applicationHost.config*) is on the same machine as the IIS installation.</span></span> <span data-ttu-id="3f193-296">*只在 ASP.NET Core 2.2 或更新版本的裝載套件組合安裝程式上可用。*</span><span class="sxs-lookup"><span data-stu-id="3f193-296">*Only available for ASP.NET Core 2.2 or later Hosting Bundler installers.*</span></span> <span data-ttu-id="3f193-297">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>。</span><span class="sxs-lookup"><span data-stu-id="3f193-297">For more information, see <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>.</span></span>
1. <span data-ttu-id="3f193-298">重新開機系統，或在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="3f193-298">Restart the system or execute the following commands in a command shell:</span></span>

   ```console
   net stop was /y
   net start w3svc
   ```
   <span data-ttu-id="3f193-299">重新啟動 IIS 將能偵測到由安裝程式對系統路徑 (此為環境變數) 所做出的變更。</span><span class="sxs-lookup"><span data-stu-id="3f193-299">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

<span data-ttu-id="3f193-300">ASP.NET Core 不採用共用架構封裝修補程式版本的向前復原行為。</span><span class="sxs-lookup"><span data-stu-id="3f193-300">ASP.NET Core doesn't adopt roll-forward behavior for patch releases of shared framework packages.</span></span> <span data-ttu-id="3f193-301">藉由安裝新的裝載套件組合來升級共用架構之後，請重新開機系統，或在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="3f193-301">After upgrading the shared framework by installing a new hosting bundle, restart the system or execute the following commands in a command shell:</span></span>

```console
net stop was /y
net start w3svc
```

> [!NOTE]
> <span data-ttu-id="3f193-302">如需 IIS 共用組態的資訊，請參閱[使用 IIS 共用組態的 ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)。</span><span class="sxs-lookup"><span data-stu-id="3f193-302">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="3f193-303">使用 Visual Studio 發佈時安裝 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="3f193-303">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="3f193-304">將應用程式部署到具有 [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later) 的伺服器時，請在伺服器上安裝最新版的 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="3f193-304">When deploying apps to servers with [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="3f193-305">若要安裝 Web Deploy，請使用 [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) 或從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=43717)直接取得安裝程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-305">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="3f193-306">慣用的方法是使用 WebPI。</span><span class="sxs-lookup"><span data-stu-id="3f193-306">The preferred method is to use WebPI.</span></span> <span data-ttu-id="3f193-307">WebPI 提供獨立的安裝程式和組態以裝載提供者。</span><span class="sxs-lookup"><span data-stu-id="3f193-307">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="3f193-308">建立 IIS 網站</span><span class="sxs-lookup"><span data-stu-id="3f193-308">Create the IIS site</span></span>

1. <span data-ttu-id="3f193-309">在主控系統上，請建立資料夾以容納應用程式的已發行資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="3f193-309">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="3f193-310">在下列步驟中，您提供資料夾路徑給 IIS，作為應用程式的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="3f193-310">In a following step, the folder's path is provided to IIS as the physical path to the app.</span></span> <span data-ttu-id="3f193-311">如需應用程式之部署資料夾和檔案配置的詳細資訊，請參閱 <xref:host-and-deploy/directory-structure>。</span><span class="sxs-lookup"><span data-stu-id="3f193-311">For more information on an app's deployment folder and file layout, see <xref:host-and-deploy/directory-structure>.</span></span>

1. <span data-ttu-id="3f193-312">在 [IIS 管理員] 中 **，在 [** 連線] 面板中開啟伺服器的節點。</span><span class="sxs-lookup"><span data-stu-id="3f193-312">In IIS Manager, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="3f193-313">以滑鼠右鍵按一下 [網站]\*\*\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3f193-313">Right-click the **Sites** folder.</span></span> <span data-ttu-id="3f193-314">從操作功能表選取 [新增網站]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-314">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="3f193-315">提供**網站名稱**，並將**實體路徑**設定為應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="3f193-315">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="3f193-316">藉由**Binding**選取 **[確定]** 來提供系結設定並建立網站：</span><span class="sxs-lookup"><span data-stu-id="3f193-316">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![在新增網站步驟中提供站台名稱、實體路徑和主機名稱。](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="3f193-318">請**勿**使用最上層萬用字元繫結 (`http://*:80/`與 `http://+:80`)。</span><span class="sxs-lookup"><span data-stu-id="3f193-318">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="3f193-319">最上層萬用字元繫結可能暴露您的應用程式安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="3f193-319">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="3f193-320">這對強式與弱式萬用字元皆適用。</span><span class="sxs-lookup"><span data-stu-id="3f193-320">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="3f193-321">請使用明確主機名稱，而非萬用字元。</span><span class="sxs-lookup"><span data-stu-id="3f193-321">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="3f193-322">若您擁有整個父網域 (與具弱點的 `*.com` 相對) 的控制權，則子網域萬用字元繫結 (例如 `*.mysub.com`) 就沒有此安全性風險。</span><span class="sxs-lookup"><span data-stu-id="3f193-322">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="3f193-323">如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="3f193-323">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="3f193-324">在伺服器的節點之下，選取 [應用程式集區]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-324">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="3f193-325">以滑鼠右鍵按一下網站的應用程式集區，然後從操作功能表選取 [基本設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-325">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="3f193-326">在 [編輯應用程式集區]\*\*\*\* 視窗中，將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\*：</span><span class="sxs-lookup"><span data-stu-id="3f193-326">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![將 .NET CLR 版本設為 [沒有受控碼]。](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="3f193-328">ASP.NET Core 會在不同的處理序中執行，並管理執行階段。</span><span class="sxs-lookup"><span data-stu-id="3f193-328">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="3f193-329">ASP.NET Core 不仰賴載入桌面 CLR (.NET CLR)&mdash;會使用 .NET Core 的核心通用語言執行平台 (CoreCLR) 來開機以在背景工作處理序中裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-329">ASP.NET Core doesn't rely on loading the desktop CLR (.NET CLR)&mdash;the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process.</span></span> <span data-ttu-id="3f193-330">將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\* 是選擇性的，但建議這樣做。</span><span class="sxs-lookup"><span data-stu-id="3f193-330">Setting the **.NET CLR version** to **No Managed Code** is optional but recommended.</span></span>

1. <span data-ttu-id="3f193-331">*ASP.NET Core 2.2 或更新版本*：對於使用[同處理序主控模型](#in-process-hosting-model)的 64 位元 (x64) [獨立式部署](/dotnet/core/deploying/#self-contained-deployments-scd)，會停用 32 位元 (x86) 處理序的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="3f193-331">*ASP.NET Core 2.2 or later*: For a 64-bit (x64) [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) that uses the [in-process hosting model](#in-process-hosting-model), disable the app pool for 32-bit (x86) processes.</span></span>

   <span data-ttu-id="3f193-332">在 IIS 管理員的 [動作]\*\*\*\* 資訊看板 > [應用程式集區]\*\*\*\* 中，選取 [設定應用程式集區預設值]\*\*\*\* 或 [進階設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-332">In the **Actions** sidebar of IIS Manager > **Application Pools**, select **Set Application Pool Defaults** or **Advanced Settings**.</span></span> <span data-ttu-id="3f193-333">找到 [啟用 32 位元應用程式]\*\*\*\*，然後將其值設定為 `False`。</span><span class="sxs-lookup"><span data-stu-id="3f193-333">Locate **Enable 32-Bit Applications** and set the value to `False`.</span></span> <span data-ttu-id="3f193-334">此設定不會影響為[處理程序外裝載](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model)部署的應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-334">This setting doesn't affect apps deployed for [out-of-process hosting](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).</span></span>

1. <span data-ttu-id="3f193-335">確認處理序模型身分識別具有適當的權限。</span><span class="sxs-lookup"><span data-stu-id="3f193-335">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="3f193-336">如果應用程式集區的預設識別（**進程模型** > **識別**）從**ApplicationPoolIdentity**變更為另一個身分識別，請確認新的身分識別具有存取應用程式資料夾、資料庫和其他必要資源的必要許可權。</span><span class="sxs-lookup"><span data-stu-id="3f193-336">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="3f193-337">例如，應用程式集區需要針對應用程式讀取和寫入檔案的資料夾取得讀取和寫入權限。</span><span class="sxs-lookup"><span data-stu-id="3f193-337">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="3f193-338">**Windows 驗證設定 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="3f193-338">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="3f193-339">如需詳細資訊，請參閱[設定 Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="3f193-339">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="3f193-340">部署應用程式</span><span class="sxs-lookup"><span data-stu-id="3f193-340">Deploy the app</span></span>

<span data-ttu-id="3f193-341">將應用程式部署至在[建立 IIS 網站](#create-the-iis-site)一節中所建立的 IIS **實體路徑**資料夾。</span><span class="sxs-lookup"><span data-stu-id="3f193-341">Deploy the app to the IIS **Physical path** folder that was established in the [Create the IIS site](#create-the-iis-site) section.</span></span> <span data-ttu-id="3f193-342">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 是建議的部署機制，但有數個選項可將應用程式從專案的 [publish]\*\* 資料夾移動至主機系統的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="3f193-342">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment, but several options exist for moving the app from the project's *publish* folder to the hosting system's deployment folder.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="3f193-343">使用 Visual Studio 的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="3f193-343">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="3f193-344">請參閱[適用於 ASP.NET Core 應用程式部署的 Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)主題，了解如何建立搭配 Web Deploy 使用的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="3f193-344">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="3f193-345">如果主機服務提供者提供或支援建立發行設定檔，請下載其設定檔，並使用 Visual Studio 的 [發行]\*\*\*\* 對話方塊匯入其設定檔：</span><span class="sxs-lookup"><span data-stu-id="3f193-345">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog:</span></span>

![[發佈] 對話方塊頁](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="3f193-347">Visual Studio 外部的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="3f193-347">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="3f193-348">透過命令列也可以在 Visual Studio 外部使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy)。</span><span class="sxs-lookup"><span data-stu-id="3f193-348">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="3f193-349">如需詳細資訊，請參閱 [Web 部署工具](/iis/publish/using-web-deploy/use-the-web-deployment-tool)。</span><span class="sxs-lookup"><span data-stu-id="3f193-349">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="3f193-350">Web Deploy 的替代項目</span><span class="sxs-lookup"><span data-stu-id="3f193-350">Alternatives to Web Deploy</span></span>

<span data-ttu-id="3f193-351">使用數種方法的其中一種將應用程式移到主機系統，例如手動複製、[Xcopy](/windows-server/administration/windows-commands/xcopy)、[Robocopy](/windows-server/administration/windows-commands/robocopy) 或 [PowerShell](/powershell/)。</span><span class="sxs-lookup"><span data-stu-id="3f193-351">Use any of several methods to move the app to the hosting system, such as manual copy, [Xcopy](/windows-server/administration/windows-commands/xcopy), [Robocopy](/windows-server/administration/windows-commands/robocopy), or [PowerShell](/powershell/).</span></span>

<span data-ttu-id="3f193-352">如需 IIS 的 ASP.NET Core 部署詳細資訊，請參閱 [IIS 系統管理員的部署資源](#deployment-resources-for-iis-administrators)一節。</span><span class="sxs-lookup"><span data-stu-id="3f193-352">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="3f193-353">瀏覽網站</span><span class="sxs-lookup"><span data-stu-id="3f193-353">Browse the website</span></span>

<span data-ttu-id="3f193-354">應用程式部署到主機系統之後，請向其中一個應用程式的公用端點發出要求。</span><span class="sxs-lookup"><span data-stu-id="3f193-354">After the app is deployed to the hosting system, make a request to one of the app's public endpoints.</span></span>

<span data-ttu-id="3f193-355">在下列範例中，網站繫結至 IIS 上的**連接埠** `80`，其**主機名稱**為 `www.mysite.com`。</span><span class="sxs-lookup"><span data-stu-id="3f193-355">In the following example, the site is bound to an IIS **Host name** of `www.mysite.com` on **Port** `80`.</span></span> <span data-ttu-id="3f193-356">對 `http://www.mysite.com` 發出要求：</span><span class="sxs-lookup"><span data-stu-id="3f193-356">A request is made to `http://www.mysite.com`:</span></span>

![Microsoft Edge 瀏覽器已載入 IIS 啟動頁面。](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="3f193-358">已鎖定的部署檔案</span><span class="sxs-lookup"><span data-stu-id="3f193-358">Locked deployment files</span></span>

<span data-ttu-id="3f193-359">當應用程式執行時，會鎖定部署資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="3f193-359">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="3f193-360">無法於部署期間覆寫已鎖定的檔案。</span><span class="sxs-lookup"><span data-stu-id="3f193-360">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="3f193-361">若要釋放部署中的已鎖定檔案，請使用下列其中**一種**方法停止應用程式集區：</span><span class="sxs-lookup"><span data-stu-id="3f193-361">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="3f193-362">使用 Web Deploy 並參考專案檔中的 `Microsoft.NET.Sdk.Web`。</span><span class="sxs-lookup"><span data-stu-id="3f193-362">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="3f193-363">*app_offline.htm* 檔案是放在 Web 應用程式目錄的根目錄中。</span><span class="sxs-lookup"><span data-stu-id="3f193-363">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="3f193-364">當檔案存在時，ASP.NET Core 模組會正常關閉應用程式，並在部署期間提供 *app_offline.htm* 檔案。</span><span class="sxs-lookup"><span data-stu-id="3f193-364">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="3f193-365">如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)。</span><span class="sxs-lookup"><span data-stu-id="3f193-365">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="3f193-366">在伺服器上的 IIS 管理員中手動停止應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="3f193-366">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="3f193-367">使用 PowerShell 卸載*app_offline .htm* （需要 PowerShell 5 或更新版本）：</span><span class="sxs-lookup"><span data-stu-id="3f193-367">Use PowerShell to drop *app_offline.htm* (requires PowerShell 5 or later):</span></span>

  ```powershell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a><span data-ttu-id="3f193-368">資料保護</span><span class="sxs-lookup"><span data-stu-id="3f193-368">Data protection</span></span>

<span data-ttu-id="3f193-369">[ASP.NET Core 資料保護堆疊](xref:security/data-protection/introduction)是由數個 ASP.NET Core [中介軟體](xref:fundamentals/middleware/index)所使用，包括用於驗證的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="3f193-369">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="3f193-370">即使資料保護 API 不是由使用者程式碼呼叫，仍應使用部署指令碼或是在使用者程式碼中設定資料保護，以建立持續性的密碼編譯[金鑰存放區](xref:security/data-protection/implementation/key-management)。</span><span class="sxs-lookup"><span data-stu-id="3f193-370">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="3f193-371">如不設定資料保護，金鑰會保留在記憶體中，並於應用程式重新啟動時捨棄。</span><span class="sxs-lookup"><span data-stu-id="3f193-371">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="3f193-372">如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：</span><span class="sxs-lookup"><span data-stu-id="3f193-372">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="3f193-373">所有以 Cookie 為基礎的驗證權杖都會失效。</span><span class="sxs-lookup"><span data-stu-id="3f193-373">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="3f193-374">當使用者提出下一個要求時，需要再次登入。</span><span class="sxs-lookup"><span data-stu-id="3f193-374">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="3f193-375">所有以 Keyring 保護的資料都無法再解密。</span><span class="sxs-lookup"><span data-stu-id="3f193-375">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="3f193-376">這可能會包含 [CSRF 權杖](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)和 [ASP.NET Core MVC TempData cookie](xref:fundamentals/app-state#tempdata)。</span><span class="sxs-lookup"><span data-stu-id="3f193-376">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="3f193-377">若要在 IIS 下設定資料保護以保存 Keyring，請使用下列其中**一種**方法：</span><span class="sxs-lookup"><span data-stu-id="3f193-377">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="3f193-378">**建立資料保護登錄機碼**</span><span class="sxs-lookup"><span data-stu-id="3f193-378">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="3f193-379">ASP.NET Core 應用程式所使用的資料保護金鑰會儲存在應用程式外部的登錄中。</span><span class="sxs-lookup"><span data-stu-id="3f193-379">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="3f193-380">若要保存指定應用程式的金鑰，請為應用程式集區建立登錄機碼。</span><span class="sxs-lookup"><span data-stu-id="3f193-380">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="3f193-381">若為獨立的非Web 伺服陣列 IIS 安裝，請針對搭配使用 ASP.NET Core 應用程式的每個應用程式集區，使用[資料保護 Provision-AutoGenKeys.ps1 PowerShell 指令碼](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1)。</span><span class="sxs-lookup"><span data-stu-id="3f193-381">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="3f193-382">此指令碼會在 HKLM 登錄中建立登錄機碼，只有應用程式之應用程式集區的背景工作處理序帳戶可以存取它。</span><span class="sxs-lookup"><span data-stu-id="3f193-382">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="3f193-383">在待用期間使用 DPAPI 和全電腦金鑰加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="3f193-383">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="3f193-384">在 Web 伺服陣列案例中，應用程式可以設定成使用 UNC 路徑來儲存其資料保護 Keyring。</span><span class="sxs-lookup"><span data-stu-id="3f193-384">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="3f193-385">根據預設，資料保護金鑰不予加密。</span><span class="sxs-lookup"><span data-stu-id="3f193-385">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="3f193-386">請確保網路共用的檔案權限僅限於執行應用程式的 Windows 帳戶。</span><span class="sxs-lookup"><span data-stu-id="3f193-386">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="3f193-387">可以使用 X509 憑證來保護待用的金鑰。</span><span class="sxs-lookup"><span data-stu-id="3f193-387">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="3f193-388">請考慮下列讓使用者上傳憑證的機制：將憑證放入使用者的受信任憑證存放區，並確保在執行使用者應用程式的所有電腦上都能使用這些憑證。</span><span class="sxs-lookup"><span data-stu-id="3f193-388">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="3f193-389">如需詳細資訊，請參閱[設定 ASP.NET Core 資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="3f193-389">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="3f193-390">**設定 IIS 應用程式集區載入使用者設定檔**</span><span class="sxs-lookup"><span data-stu-id="3f193-390">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="3f193-391">此設定位在應用程式集區 [進階設定]\*\*\*\* 下的 [處理序模型]\*\*\*\* 區段中。</span><span class="sxs-lookup"><span data-stu-id="3f193-391">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="3f193-392">將 [載入使用者設定檔]\*\*\*\* 設為 `True`。</span><span class="sxs-lookup"><span data-stu-id="3f193-392">Set **Load User Profile** to `True`.</span></span> <span data-ttu-id="3f193-393">當設定為 `True` 時，金鑰會儲存在使用者設定檔目錄中，且使用具有使用者帳戶專屬金鑰的 DPAPI 保護。</span><span class="sxs-lookup"><span data-stu-id="3f193-393">When set to `True`, keys are stored in the user profile directory and protected using DPAPI with a key specific to the user account.</span></span> <span data-ttu-id="3f193-394">金鑰會保存到 *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3f193-394">Keys are persisted to the *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* folder.</span></span>

  <span data-ttu-id="3f193-395">應用程式集區的 [setProfileEnvironment 屬性](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration)也必須啟用。</span><span class="sxs-lookup"><span data-stu-id="3f193-395">The app pool's [setProfileEnvironment attribute](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) must also be enabled.</span></span> <span data-ttu-id="3f193-396">`setProfileEnvironment` 的預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="3f193-396">The default value of `setProfileEnvironment` is `true`.</span></span> <span data-ttu-id="3f193-397">在某些情況下 (例如 Windows OS)，`setProfileEnvironment` 會設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="3f193-397">In some scenarios (for example, Windows OS), `setProfileEnvironment` is set to `false`.</span></span> <span data-ttu-id="3f193-398">如果金鑰並未如預期地儲存在使用者設定檔目錄中：</span><span class="sxs-lookup"><span data-stu-id="3f193-398">If keys aren't stored in the user profile directory as expected:</span></span>

  1. <span data-ttu-id="3f193-399">瀏覽至 *%windir%/system32/inetsrv/config* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3f193-399">Navigate to the *%windir%/system32/inetsrv/config* folder.</span></span>
  1. <span data-ttu-id="3f193-400">開啟 *applicationHost.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="3f193-400">Open the *applicationHost.config* file.</span></span>
  1. <span data-ttu-id="3f193-401">找出 `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` 元素。</span><span class="sxs-lookup"><span data-stu-id="3f193-401">Locate the `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` element.</span></span>
  1. <span data-ttu-id="3f193-402">確認 `setProfileEnvironment` 屬性不存在 (其預設值為 `true`)，或明確地將屬性值設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="3f193-402">Confirm that the `setProfileEnvironment` attribute isn't present, which defaults the value to `true`, or explicitly set the attribute's value to `true`.</span></span>

* <span data-ttu-id="3f193-403">**將檔案系統當作 Keyring 存放區使用**</span><span class="sxs-lookup"><span data-stu-id="3f193-403">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="3f193-404">調整應用程式程式碼，以[將檔案系統當作 Keyring 存放區使用](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="3f193-404">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="3f193-405">使用 X509 憑證來保護 Keyring，並確保憑證是受信任的憑證。</span><span class="sxs-lookup"><span data-stu-id="3f193-405">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="3f193-406">如果憑證為自我簽署，請將憑證放在受信任的根存放區。</span><span class="sxs-lookup"><span data-stu-id="3f193-406">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="3f193-407">在 Web 伺服陣列中使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="3f193-407">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="3f193-408">請使用所有電腦都可以存取的檔案共用。</span><span class="sxs-lookup"><span data-stu-id="3f193-408">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="3f193-409">將 X509 憑證部署到每一部電腦。</span><span class="sxs-lookup"><span data-stu-id="3f193-409">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="3f193-410">設定[程式碼中的資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="3f193-410">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="3f193-411">**設定資料保護的全電腦原則**</span><span class="sxs-lookup"><span data-stu-id="3f193-411">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="3f193-412">針對取用資料保護 API 的所有應用程式，資料保護系統僅支援有限的預設[全電腦原則](xref:security/data-protection/configuration/machine-wide-policy)設定。</span><span class="sxs-lookup"><span data-stu-id="3f193-412">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="3f193-413">如需詳細資訊，請參閱<xref:security/data-protection/introduction>。</span><span class="sxs-lookup"><span data-stu-id="3f193-413">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="virtual-directories"></a><span data-ttu-id="3f193-414">虛擬目錄</span><span class="sxs-lookup"><span data-stu-id="3f193-414">Virtual Directories</span></span>

<span data-ttu-id="3f193-415">ASP.NET Core 應用程式不支援 [IIS 虛擬目錄](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories)。</span><span class="sxs-lookup"><span data-stu-id="3f193-415">[IIS Virtual Directories](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) aren't supported with ASP.NET Core apps.</span></span> <span data-ttu-id="3f193-416">應用程式能以[子應用程式](#sub-applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="3f193-416">An app can be hosted as a [sub-application](#sub-applications).</span></span>

## <a name="sub-applications"></a><span data-ttu-id="3f193-417">子應用程式</span><span class="sxs-lookup"><span data-stu-id="3f193-417">Sub-applications</span></span>

<span data-ttu-id="3f193-418">ASP.NET Core 應用程式能以 [IIS 子應用程式](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="3f193-418">An ASP.NET Core app can be hosted as an [IIS sub-application (sub-app)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications).</span></span> <span data-ttu-id="3f193-419">子應用程式的路徑會成為根應用程式 URL 的一部分。</span><span class="sxs-lookup"><span data-stu-id="3f193-419">The sub-app's path becomes part of the root app's URL.</span></span>

<span data-ttu-id="3f193-420">子應用程式內的靜態資產連結應該使用波狀符號與斜線 (`~/`) 標記法。</span><span class="sxs-lookup"><span data-stu-id="3f193-420">Static asset links within the sub-app should use tilde-slash (`~/`) notation.</span></span> <span data-ttu-id="3f193-421">波狀符號與斜線標記法會觸發[標記協助程式](xref:mvc/views/tag-helpers/intro)以將子應用程式的路徑基底附加到轉譯的相對連結前面。</span><span class="sxs-lookup"><span data-stu-id="3f193-421">Tilde-slash notation triggers a [Tag Helper](xref:mvc/views/tag-helpers/intro) to prepend the sub-app's pathbase to the rendered relative link.</span></span> <span data-ttu-id="3f193-422">針對位於 `/subapp_path` 的子應用程式，使用 `src="~/image.png"` 連結的影像會轉譯為 `src="/subapp_path/image.png"`。</span><span class="sxs-lookup"><span data-stu-id="3f193-422">For a sub-app at `/subapp_path`, an image linked with `src="~/image.png"` is rendered as `src="/subapp_path/image.png"`.</span></span> <span data-ttu-id="3f193-423">根應用程式的靜態檔案中介軟體不會處理靜態檔案要求。</span><span class="sxs-lookup"><span data-stu-id="3f193-423">The root app's Static File Middleware doesn't process the static file request.</span></span> <span data-ttu-id="3f193-424">要求會由子應用程式的靜態檔案中介軟體處理。</span><span class="sxs-lookup"><span data-stu-id="3f193-424">The request is processed by the sub-app's Static File Middleware.</span></span>

<span data-ttu-id="3f193-425">若靜態資產的 `src` 屬性是設定為絕對路徑 (例如，`src="/image.png"`)，會以不使用子應用程式路徑基底的方式轉譯連結。</span><span class="sxs-lookup"><span data-stu-id="3f193-425">If a static asset's `src` attribute is set to an absolute path (for example, `src="/image.png"`), the link is rendered without the sub-app's pathbase.</span></span> <span data-ttu-id="3f193-426">根應用程式的靜態檔案中介軟體會嘗試從根應用程式的 [webroot](xref:fundamentals/index#web-root) 提供資產，這會導致「404 - 找不到」\*\* 回應 (除非根應用程式可存取靜態資產)。</span><span class="sxs-lookup"><span data-stu-id="3f193-426">The root app's Static File Middleware attempts to serve the asset from the root app's [web root](xref:fundamentals/index#web-root), which results in a *404 - Not Found* response unless the static asset is available from the root app.</span></span>

<span data-ttu-id="3f193-427">裝載 ASP.NET Core 應用程式做為另一個 ASP.NET Core 應用程式下的子應用程式：</span><span class="sxs-lookup"><span data-stu-id="3f193-427">To host an ASP.NET Core app as a sub-app under another ASP.NET Core app:</span></span>

1. <span data-ttu-id="3f193-428">為子應用程式建立應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="3f193-428">Establish an app pool for the sub-app.</span></span> <span data-ttu-id="3f193-429">將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\*，因為會將核心通用語言執行平台 (CoreCLR) 開機以在背景工作處理序中裝載應用程式，而非在桌面 CLR (.NET CLR) 中裝載。</span><span class="sxs-lookup"><span data-stu-id="3f193-429">Set the **.NET CLR Version** to **No Managed Code** because the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process, not the desktop CLR (.NET CLR).</span></span>

1. <span data-ttu-id="3f193-430">使用根網站下資料夾中的子應用程式在 IIS 管理員中新增根網站。</span><span class="sxs-lookup"><span data-stu-id="3f193-430">Add the root site in IIS Manager with the sub-app in a folder under the root site.</span></span>

1. <span data-ttu-id="3f193-431">以滑鼠右鍵按一下 IIS 管理員中的子應用程式資料夾，然後選取 [轉換成應用程式]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-431">Right-click the sub-app folder in IIS Manager and select **Convert to Application**.</span></span>

1. <span data-ttu-id="3f193-432">在 [新增應用程式]\*\*\*\* 對話方塊中，使用 [應用程式集區]\*\*\*\* 的[選取]\*\*\*\* 按鈕來指派您為子應用程式建立的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="3f193-432">In the **Add Application** dialog, use the **Select** button for the **Application Pool** to assign the app pool that you created for the sub-app.</span></span> <span data-ttu-id="3f193-433">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="3f193-433">Select **OK**.</span></span>

<span data-ttu-id="3f193-434">將不同的應用程式集區指派給子應用程式是使用同處理序裝載模型。</span><span class="sxs-lookup"><span data-stu-id="3f193-434">The assignment of a separate app pool to the sub-app is a requirement when using the in-process hosting model.</span></span>

<span data-ttu-id="3f193-435">如需有關同進程裝載模型和設定 ASP.NET Core 模組的詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="3f193-435">For more information on the in-process hosting model and configuring the ASP.NET Core Module, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="3f193-436">使用 web.config 的 IIS 組態</span><span class="sxs-lookup"><span data-stu-id="3f193-436">Configuration of IIS with web.config</span></span>

<span data-ttu-id="3f193-437">在對使用了 ASP.NET Core 模組的 ASP.NET Core 有作用的 IIS 情境下，設定會受 *web.config* 的 `<system.webServer>` 區段影響。</span><span class="sxs-lookup"><span data-stu-id="3f193-437">IIS configuration is influenced by the `<system.webServer>` section of *web.config* for IIS scenarios that are functional for ASP.NET Core apps with the ASP.NET Core Module.</span></span> <span data-ttu-id="3f193-438">舉例來說，IIS 設定對動態壓縮有作用。</span><span class="sxs-lookup"><span data-stu-id="3f193-438">For example, IIS configuration is functional for dynamic compression.</span></span> <span data-ttu-id="3f193-439">如果在伺服器層級將 IIS 設為使用動態壓縮，應用程式 *web.config* 檔案中的 `<urlCompression>` 元素則可為 ASP.NET Core 應用程式予以停用。</span><span class="sxs-lookup"><span data-stu-id="3f193-439">If IIS is configured at the server level to use dynamic compression, the `<urlCompression>` element in the app's *web.config* file can disable it for an ASP.NET Core app.</span></span>

<span data-ttu-id="3f193-440">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="3f193-440">For more information, see the following topics:</span></span>

* [<span data-ttu-id="3f193-441">System.webserver>的\<設定參考</span><span class="sxs-lookup"><span data-stu-id="3f193-441">Configuration reference for \<system.webServer></span></span>](/iis/configuration/system.webServer/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>

<span data-ttu-id="3f193-442">若要設定在隔離的應用程式集區中執行之個別應用程式的環境變數 (支援 IIS 10.0 或更新版本)，請參閱 IIS 參考文件之[環境變數 \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的 *AppCmd.exe 命令*一節。</span><span class="sxs-lookup"><span data-stu-id="3f193-442">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="3f193-443">web.config 的組態區段</span><span class="sxs-lookup"><span data-stu-id="3f193-443">Configuration sections of web.config</span></span>

<span data-ttu-id="3f193-444">ASP.NET Core 應用程式的設定不使用 *web.config* 中 ASP.NET 4.x 應用程式的設定區段：</span><span class="sxs-lookup"><span data-stu-id="3f193-444">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

<span data-ttu-id="3f193-445">使用其他組態提供者設定的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-445">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="3f193-446">如需詳細資訊，請參閱[Configuration](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="3f193-446">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="3f193-447">應用程式集區</span><span class="sxs-lookup"><span data-stu-id="3f193-447">Application Pools</span></span>

<span data-ttu-id="3f193-448">應用程式集區隔離取決於裝載模型：</span><span class="sxs-lookup"><span data-stu-id="3f193-448">App pool isolation is determined by the hosting model:</span></span>

* <span data-ttu-id="3f193-449">處理序內裝載 &ndash; 應用程式必須在分開的應用程式集區中執行。</span><span class="sxs-lookup"><span data-stu-id="3f193-449">In-process hosting &ndash; Apps are required to run in separate app pools.</span></span>
* <span data-ttu-id="3f193-450">處理序外裝載 &ndash; 建議藉由在各自的應用程式集區中執行每個應用程式，以將應用程式互相隔離。</span><span class="sxs-lookup"><span data-stu-id="3f193-450">Out-of-process hosting &ndash; We recommend isolating the apps from each other by running each app in its own app pool.</span></span>

<span data-ttu-id="3f193-451">IIS [新增網站]\*\*\*\* 對話方塊預設每個應用程式皆為單一應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="3f193-451">The IIS **Add Website** dialog defaults to a single app pool per app.</span></span> <span data-ttu-id="3f193-452">當提供**網站名稱**時，文字會自動轉移至 [應用程式集區]\*\*\*\* 文字方塊。</span><span class="sxs-lookup"><span data-stu-id="3f193-452">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="3f193-453">新增網站時，會使用該網站名稱建立新的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="3f193-453">A new app pool is created using the site name when the site is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="3f193-454">應用程式集區身分識別</span><span class="sxs-lookup"><span data-stu-id="3f193-454">Application Pool Identity</span></span>

<span data-ttu-id="3f193-455">應用程式集區身分識別帳戶可讓應用程式在唯一的帳戶下執行，不必建立及管理網域或本機帳戶。</span><span class="sxs-lookup"><span data-stu-id="3f193-455">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="3f193-456">在 IIS 8.0 或更新版本中，IIS 管理背景工作處理序 (WAS) 會使用新的應用程式集區名稱建立虛擬帳戶，並預設在此帳戶下執行應用程式集區的背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="3f193-456">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="3f193-457">在 IIS 管理主控台中，於應用程式集區的 [進階設定]\*\*\*\* 下，確定 [身分識別]\*\*\*\* 設定為使用 **ApplicationPoolIdentity**：</span><span class="sxs-lookup"><span data-stu-id="3f193-457">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![應用程式集區進階設定對話方塊](index/_static/apppool-identity.png)

<span data-ttu-id="3f193-459">IIS 管理程序會在 Windows 安全系統中，以應用程式集區的名稱建立安全識別碼。</span><span class="sxs-lookup"><span data-stu-id="3f193-459">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="3f193-460">可使用此身分識別來保護資源。</span><span class="sxs-lookup"><span data-stu-id="3f193-460">Resources can be secured using this identity.</span></span> <span data-ttu-id="3f193-461">不過，此身分識別不是真正的使用者帳戶，也不會顯示在 Windows 使用者管理主控台中。</span><span class="sxs-lookup"><span data-stu-id="3f193-461">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="3f193-462">如果 IIS 背景工作處理序需要提升應用程式的存取權限，請修改包含應用程式的目錄存取控制清單 (ACL)：</span><span class="sxs-lookup"><span data-stu-id="3f193-462">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="3f193-463">開啟 Windows 檔案總管，巡覽至目錄。</span><span class="sxs-lookup"><span data-stu-id="3f193-463">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="3f193-464">以滑鼠右鍵按一下目錄並選取 [屬性]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-464">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="3f193-465">依序選取 [安全性]\*\*\*\* 索引標籤下的 [編輯]\*\*\*\* 按鈕和 [新增]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="3f193-465">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="3f193-466">選取 [位置]\*\*\*\* 按鈕，並確定選取系統。</span><span class="sxs-lookup"><span data-stu-id="3f193-466">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="3f193-467">在 [輸入要選取的物件名稱]\*\*\*\* 區域中，輸入 **IIS AppPool\\<app_pool_name>**。</span><span class="sxs-lookup"><span data-stu-id="3f193-467">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="3f193-468">選取 [檢查名稱]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="3f193-468">Select the **Check Names** button.</span></span> <span data-ttu-id="3f193-469">針對 [DefaultAppPool]\*\*，請使用 **IIS AppPool\DefaultAppPool** 檢查名稱。</span><span class="sxs-lookup"><span data-stu-id="3f193-469">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="3f193-470">選取 [檢查名稱]\*\*\*\* 按鈕時，[DefaultAppPool]\*\*\*\* 的值便會顯示於物件名稱區域中。</span><span class="sxs-lookup"><span data-stu-id="3f193-470">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="3f193-471">您無法直接將應用程式集區名稱輸入至物件名稱區域。</span><span class="sxs-lookup"><span data-stu-id="3f193-471">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="3f193-472">檢查物件名稱時，請使用 **IIS AppPool\\<app_pool_name>** 的格式。</span><span class="sxs-lookup"><span data-stu-id="3f193-472">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：在選取 [檢查名稱] 之前，"DefaultAppPool" 這個應用程式集區名稱在物件名稱區域中會附加至 "IIS AppPool\"。](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="3f193-474">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="3f193-474">Select **OK**.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：選取 [檢查名稱] 之後，物件名稱 "DefaultAppPool" 會顯示在物件名稱區域中。](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="3f193-476">預設應該會授與讀取 &amp; 執行權限。</span><span class="sxs-lookup"><span data-stu-id="3f193-476">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="3f193-477">請視需要提供其他權限。</span><span class="sxs-lookup"><span data-stu-id="3f193-477">Provide additional permissions as needed.</span></span>

<span data-ttu-id="3f193-478">也可使用 **ICACLS** 工具透過命令提示字元授與存取權限。</span><span class="sxs-lookup"><span data-stu-id="3f193-478">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="3f193-479">使用 *DefaultAppPool*作為範例，將會使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="3f193-479">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="3f193-480">如需詳細資訊，請參閱 [icacls](/windows-server/administration/windows-commands/icacls) 主題。</span><span class="sxs-lookup"><span data-stu-id="3f193-480">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="http2-support"></a><span data-ttu-id="3f193-481">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="3f193-481">HTTP/2 support</span></span>

<span data-ttu-id="3f193-482">在下列 IIS 部署案例中，ASP.NET Core 支援 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="3f193-482">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following IIS deployment scenarios:</span></span>

* <span data-ttu-id="3f193-483">內含式</span><span class="sxs-lookup"><span data-stu-id="3f193-483">In-process</span></span>
  * <span data-ttu-id="3f193-484">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="3f193-484">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="3f193-485">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="3f193-485">TLS 1.2 or later connection</span></span>
* <span data-ttu-id="3f193-486">跨處理序</span><span class="sxs-lookup"><span data-stu-id="3f193-486">Out-of-process</span></span>
  * <span data-ttu-id="3f193-487">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="3f193-487">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="3f193-488">公開 Edge Server 連線使用 HTTP/2，但是對 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的反向 Proxy 連線使用 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="3f193-488">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
  * <span data-ttu-id="3f193-489">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="3f193-489">TLS 1.2 or later connection</span></span>

<span data-ttu-id="3f193-490">針對建立 HTTP/2 連線時的同處理序部署，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會回報 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="3f193-490">For an in-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span> <span data-ttu-id="3f193-491">針對建立 HTTP/2 連線時的跨處理序部署，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會回報 `HTTP/1.1`。</span><span class="sxs-lookup"><span data-stu-id="3f193-491">For an out-of-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="3f193-492">如需有關同處理序和跨處理序主控模型的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="3f193-492">For more information on the in-process and out-of-process hosting models, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="3f193-493">HTTP/2 預設為啟用。</span><span class="sxs-lookup"><span data-stu-id="3f193-493">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="3f193-494">如果 HTTP/2 連線尚未建立，連線會退為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="3f193-494">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="3f193-495">如需使用 IIS 部署之 HTTP/2 設定的詳細資訊，請參閱 [IIS 上的 HTTP/2](/iis/get-started/whats-new-in-iis-10/http2-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="3f193-495">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="cors-preflight-requests"></a><span data-ttu-id="3f193-496">CORS 預檢要求</span><span class="sxs-lookup"><span data-stu-id="3f193-496">CORS preflight requests</span></span>

<span data-ttu-id="3f193-497">*此節只適用於以 .NET Framework 為目標的 ASP.NET Core 應用程式。*</span><span class="sxs-lookup"><span data-stu-id="3f193-497">*This section only applies to ASP.NET Core apps that target the .NET Framework.*</span></span>

<span data-ttu-id="3f193-498">針對以 .NET Framework 為目標的 ASP.NET Core 應用程式，在 IIS 中OPTIONS 要求預設不會傳遞到應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-498">For an ASP.NET Core app that targets the .NET Framework, OPTIONS requests aren't passed to the app by default in IIS.</span></span> <span data-ttu-id="3f193-499">若要瞭解如何在 web.config 中設定應用程式的 IIS 處理*程式來傳遞*選項要求，請參閱[在 ASP.NET Web API 2 中啟用跨原始來源要求： CORS 的運作方式](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works)。</span><span class="sxs-lookup"><span data-stu-id="3f193-499">To learn how to configure the app's IIS handlers in *web.config* to pass OPTIONS requests, see [Enable cross-origin requests in ASP.NET Web API 2: How CORS Works](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).</span></span>

## <a name="application-initialization-module-and-idle-timeout"></a><span data-ttu-id="3f193-500">應用程式初始化模組與閒置逾時</span><span class="sxs-lookup"><span data-stu-id="3f193-500">Application Initialization Module and Idle Timeout</span></span>

<span data-ttu-id="3f193-501">在 IIS 中由 ASP.NET Core 模組版本 2 裝載時：</span><span class="sxs-lookup"><span data-stu-id="3f193-501">When hosted in IIS by the ASP.NET Core Module version 2:</span></span>

* <span data-ttu-id="3f193-502">[應用程式初始化模組](#application-initialization-module) &ndash;應用程式的裝載同[進程](#in-process-hosting-model)或跨[進程](#out-of-process-hosting-model)，可以設定為在背景工作進程重新開機或伺服器重新開機時自動啟動。</span><span class="sxs-lookup"><span data-stu-id="3f193-502">[Application Initialization Module](#application-initialization-module) &ndash; App's hosted [in-process](#in-process-hosting-model) or [out-of-process](#out-of-process-hosting-model) can be configured to start automatically on a worker process restart or server restart.</span></span>
* <span data-ttu-id="3f193-503">[閒置逾時](#idle-timeout) &ndash; 應用程式的裝載 [同處理序](#in-process-hosting-model)可設定為在無活動期間不逾時。</span><span class="sxs-lookup"><span data-stu-id="3f193-503">[Idle Timeout](#idle-timeout) &ndash; App's hosted [in-process](#in-process-hosting-model) can be configured not to timeout during periods of inactivity.</span></span>

### <a name="application-initialization-module"></a><span data-ttu-id="3f193-504">應用程式初始化模組</span><span class="sxs-lookup"><span data-stu-id="3f193-504">Application Initialization Module</span></span>

<span data-ttu-id="3f193-505">*套用到應用程式裝載同處理序與非同處理序。*</span><span class="sxs-lookup"><span data-stu-id="3f193-505">*Applies to apps hosted in-process and out-of-process.*</span></span>

<span data-ttu-id="3f193-506">[IIS 應用程式初始化](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)是一項 IIS 功能，可在應用程式集區啟動或回收時，將 HTTP 要求傳送給應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-506">[IIS Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) is an IIS feature that sends an HTTP request to the app when the app pool starts or is recycled.</span></span> <span data-ttu-id="3f193-507">要求會觸發應用程式啟動。</span><span class="sxs-lookup"><span data-stu-id="3f193-507">The request triggers the app to start.</span></span> <span data-ttu-id="3f193-508">根據預設，IIS 會發出應用程式根 URL (`/`) 的要求以初始化應用程式 (如需有關設定的詳細資訊，請參閱[額外資源](#application-initialization-module-and-idle-timeout-additional-resources))。</span><span class="sxs-lookup"><span data-stu-id="3f193-508">By default, IIS issues a request to the app's root URL (`/`) to initialize the app (see the [additional resources](#application-initialization-module-and-idle-timeout-additional-resources) for more details on configuration).</span></span>

<span data-ttu-id="3f193-509">確認已啟用「IIS 應用程式初始化」角色功能：</span><span class="sxs-lookup"><span data-stu-id="3f193-509">Confirm that the IIS Application Initialization role feature in enabled:</span></span>

<span data-ttu-id="3f193-510">在 Windows 7 或更新的電腦系統上，當在本機使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="3f193-510">On Windows 7 or later desktop systems when using IIS locally:</span></span>

1. <span data-ttu-id="3f193-511">瀏覽到 [控制台]\*\* [程式]\*\* > \*\* [程式和功能]\*\* > \*\*\*\* > **[開啟或關閉 Windows 功能]** \(畫面左側\)。</span><span class="sxs-lookup"><span data-stu-id="3f193-511">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="3f193-512">開啟 [網際網路資訊服務]\*\* [World Wide Web 服務]\*\* > \*\* [應用程式開發功能]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-512">Open **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="3f193-513">選取 [應用程式初始化]\*\*\*\* 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="3f193-513">Select the check box for **Application Initialization**.</span></span>

<span data-ttu-id="3f193-514">在 Windows Server 2008 R2 或更新版本上：</span><span class="sxs-lookup"><span data-stu-id="3f193-514">On Windows Server 2008 R2 or later:</span></span>

1. <span data-ttu-id="3f193-515">開啟「新增角色與功能精靈」\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-515">Open the **Add Roles and Features Wizard**.</span></span>
1. <span data-ttu-id="3f193-516">在 [選取角色服務]\*\*\*\* 面板中，開啟 [應用程式開發]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="3f193-516">In the **Select role services** panel, open the **Application Development** node.</span></span>
1. <span data-ttu-id="3f193-517">選取 [應用程式初始化]\*\*\*\* 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="3f193-517">Select the check box for **Application Initialization**.</span></span>

<span data-ttu-id="3f193-518">使用下列任一方式為網站啟用應用程式初始化模組：</span><span class="sxs-lookup"><span data-stu-id="3f193-518">Use either of the following approaches to enable the Application Initialization Module for the site:</span></span>

* <span data-ttu-id="3f193-519">使用 IIS 管理員：</span><span class="sxs-lookup"><span data-stu-id="3f193-519">Using IIS Manager:</span></span>

  1. <span data-ttu-id="3f193-520">選取 [連線]\*\*\*\* 面板中的 [應用程式集區]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-520">Select **Application Pools** in the **Connections** panel.</span></span>
  1. <span data-ttu-id="3f193-521">以滑鼠右鍵按一下清單中應用程式的應用程式集區，然後選取 [進階設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-521">Right-click the app's app pool in the list and select **Advanced Settings**.</span></span>
  1. <span data-ttu-id="3f193-522">預設的 [啟動模式]\*\*\*\* 是 [OnDemand]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-522">The default **Start Mode** is **OnDemand**.</span></span> <span data-ttu-id="3f193-523">將 [啟動模式]\*\*\*\* 設定為 [AlwaysRunning]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-523">Set the **Start Mode** to **AlwaysRunning**.</span></span> <span data-ttu-id="3f193-524">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="3f193-524">Select **OK**.</span></span>
  1. <span data-ttu-id="3f193-525">開啟 [連線]\*\*\*\* 面板中的 [站台]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="3f193-525">Open the **Sites** node in the **Connections** panel.</span></span>
  1. <span data-ttu-id="3f193-526">以滑鼠右鍵按一下應用程式，然後選取 [管理網站]\*\* [進階設定]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-526">Right-click the app and select **Manage Website** > **Advanced Settings**.</span></span>
  1. <span data-ttu-id="3f193-527">預設 [預先載入已啟用]\*\*\*\* 設定是 [False]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-527">The default **Preload Enabled** setting is **False**.</span></span> <span data-ttu-id="3f193-528">將 [預先載入已啟用]\*\*\*\* 設定為 [True]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-528">Set **Preload Enabled** to **True**.</span></span> <span data-ttu-id="3f193-529">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="3f193-529">Select **OK**.</span></span>

* <span data-ttu-id="3f193-530">使用 *web.config*，新增 `<applicationInitialization>` 元素並將 `doAppInitAfterRestart` 設定為 `true` 至應用程式 *web.config* 檔案中的 `<system.webServer>` 元素：</span><span class="sxs-lookup"><span data-stu-id="3f193-530">Using *web.config*, add the `<applicationInitialization>` element with `doAppInitAfterRestart` set to `true` to the `<system.webServer>` elements in the app's *web.config* file:</span></span>

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

### <a name="idle-timeout"></a><span data-ttu-id="3f193-531">閒置逾時</span><span class="sxs-lookup"><span data-stu-id="3f193-531">Idle Timeout</span></span>

<span data-ttu-id="3f193-532">*僅適用於應用程式裝載同處理序。*</span><span class="sxs-lookup"><span data-stu-id="3f193-532">*Only applies to apps hosted in-process.*</span></span>

<span data-ttu-id="3f193-533">若要防止應用程式閒置，請使用 IIS 管理員設定應用程式集區的閒置逾時：</span><span class="sxs-lookup"><span data-stu-id="3f193-533">To prevent the app from idling, set the app pool's idle timeout using IIS Manager:</span></span>

1. <span data-ttu-id="3f193-534">選取 [連線]\*\*\*\* 面板中的 [應用程式集區]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-534">Select **Application Pools** in the **Connections** panel.</span></span>
1. <span data-ttu-id="3f193-535">以滑鼠右鍵按一下清單中應用程式的應用程式集區，然後選取 [進階設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-535">Right-click the app's app pool in the list and select **Advanced Settings**.</span></span>
1. <span data-ttu-id="3f193-536">預設 [閒置逾時 (分鐘)]\*\*\*\* 是 **20** 分鐘。</span><span class="sxs-lookup"><span data-stu-id="3f193-536">The default **Idle Time-out (minutes)** is **20** minutes.</span></span> <span data-ttu-id="3f193-537">將 [閒置逾時 (分鐘)]\*\*\*\* 設定為 **0** (零)。</span><span class="sxs-lookup"><span data-stu-id="3f193-537">Set the **Idle Time-out (minutes)** to **0** (zero).</span></span> <span data-ttu-id="3f193-538">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="3f193-538">Select **OK**.</span></span>
1. <span data-ttu-id="3f193-539">回收背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="3f193-539">Recycle the worker process.</span></span>

<span data-ttu-id="3f193-540">若要防止應用程式裝載[非同處理序](#out-of-process-hosting-model)逾時，請使用下列任一方式：</span><span class="sxs-lookup"><span data-stu-id="3f193-540">To prevent apps hosted [out-of-process](#out-of-process-hosting-model) from timing out, use either of the following approaches:</span></span>

* <span data-ttu-id="3f193-541">從外部服務對應用程式執行 Ping 以讓它繼續執行。</span><span class="sxs-lookup"><span data-stu-id="3f193-541">Ping the app from an external service in order to keep it running.</span></span>
* <span data-ttu-id="3f193-542">若應用程式只裝載背景服務，避免 IIS 裝載並使用 [Windows 服務來裝載 ASP.NET Core 應用程式](xref:host-and-deploy/windows-service)。</span><span class="sxs-lookup"><span data-stu-id="3f193-542">If the app only hosts background services, avoid IIS hosting and use a [Windows Service to host the ASP.NET Core app](xref:host-and-deploy/windows-service).</span></span>

### <a name="application-initialization-module-and-idle-timeout-additional-resources"></a><span data-ttu-id="3f193-543">應用程式初始化模組與閒置逾時額外資源</span><span class="sxs-lookup"><span data-stu-id="3f193-543">Application Initialization Module and Idle Timeout additional resources</span></span>

* [<span data-ttu-id="3f193-544">IIS 8.0 應用程式初始化</span><span class="sxs-lookup"><span data-stu-id="3f193-544">IIS 8.0 Application Initialization</span></span>](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)
* <span data-ttu-id="3f193-545">[應用程式初始化 \<applicationInitialization>](/iis/configuration/system.webserver/applicationinitialization/)。</span><span class="sxs-lookup"><span data-stu-id="3f193-545">[Application Initialization \<applicationInitialization>](/iis/configuration/system.webserver/applicationinitialization/).</span></span>
* <span data-ttu-id="3f193-546">[應用程式集區的處理序模組設定 \<processModel>](/iis/configuration/system.applicationhost/applicationpools/add/processmodel)。</span><span class="sxs-lookup"><span data-stu-id="3f193-546">[Process Model Settings for an Application Pool \<processModel>](/iis/configuration/system.applicationhost/applicationpools/add/processmodel).</span></span>

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="3f193-547">IIS 系統管理員的部署資源</span><span class="sxs-lookup"><span data-stu-id="3f193-547">Deployment resources for IIS administrators</span></span>

* [<span data-ttu-id="3f193-548">IIS 文件</span><span class="sxs-lookup"><span data-stu-id="3f193-548">IIS documentation</span></span>](/iis)
* [<span data-ttu-id="3f193-549">IIS 中的 IIS 管理員使用者入門</span><span class="sxs-lookup"><span data-stu-id="3f193-549">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* [<span data-ttu-id="3f193-550">.NET Core 應用程式部署</span><span class="sxs-lookup"><span data-stu-id="3f193-550">.NET Core application deployment</span></span>](/dotnet/core/deploying/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/directory-structure>
* <xref:host-and-deploy/iis/modules>
* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>

## <a name="additional-resources"></a><span data-ttu-id="3f193-551">其他資源</span><span class="sxs-lookup"><span data-stu-id="3f193-551">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:index>
* [<span data-ttu-id="3f193-552">Microsoft IIS 官方網站</span><span class="sxs-lookup"><span data-stu-id="3f193-552">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="3f193-553">Windows Server 技術內容庫</span><span class="sxs-lookup"><span data-stu-id="3f193-553">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="3f193-554">IIS 上的 HTTP/2</span><span class="sxs-lookup"><span data-stu-id="3f193-554">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
* <xref:host-and-deploy/iis/transform-webconfig>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="3f193-555">如需將 ASP.NET Core 應用程式發佈至 IIS 伺服器的教學課程體驗，請參閱<xref:tutorials/publish-to-iis>。</span><span class="sxs-lookup"><span data-stu-id="3f193-555">For a tutorial experience on publishing an ASP.NET Core app to an IIS server, see <xref:tutorials/publish-to-iis>.</span></span>

[<span data-ttu-id="3f193-556">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="3f193-556">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="3f193-557">支援的作業系統</span><span class="sxs-lookup"><span data-stu-id="3f193-557">Supported operating systems</span></span>

<span data-ttu-id="3f193-558">以下為支援的作業系統：</span><span class="sxs-lookup"><span data-stu-id="3f193-558">The following operating systems are supported:</span></span>

* <span data-ttu-id="3f193-559">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="3f193-559">Windows 7 or later</span></span>
* <span data-ttu-id="3f193-560">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="3f193-560">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="3f193-561">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys) (先前稱為 WebListener) 不適用搭配 IIS 的反向 Proxy 設定。</span><span class="sxs-lookup"><span data-stu-id="3f193-561">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="3f193-562">請使用 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="3f193-562">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="3f193-563">如需在 Azure 中裝載的資訊，請參閱 <xref:host-and-deploy/azure-apps/index>。</span><span class="sxs-lookup"><span data-stu-id="3f193-563">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

<span data-ttu-id="3f193-564">如需疑難排解指引，請參閱 <xref:test/troubleshoot>。</span><span class="sxs-lookup"><span data-stu-id="3f193-564">For troubleshooting guidance, see <xref:test/troubleshoot>.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="3f193-565">支援的平台</span><span class="sxs-lookup"><span data-stu-id="3f193-565">Supported platforms</span></span>

<span data-ttu-id="3f193-566">支援針對 32 位元 (x86) 或 64 位元 (x64) 部署發行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-566">Apps published for 32-bit (x86) or 64-bit (x64) deployment are supported.</span></span> <span data-ttu-id="3f193-567">使用 32 位元 (x86) .NET Core SDK 部署 32 位元應用程式，除非應用程式：</span><span class="sxs-lookup"><span data-stu-id="3f193-567">Deploy a 32-bit app with a 32-bit (x86) .NET Core SDK unless the app:</span></span>

* <span data-ttu-id="3f193-568">需要提供給 64 位元應用程式使用的較大虛擬記憶體位址空間。</span><span class="sxs-lookup"><span data-stu-id="3f193-568">Requires the larger virtual memory address space available to a 64-bit app.</span></span>
* <span data-ttu-id="3f193-569">需要較大的 IIS 堆疊大小。</span><span class="sxs-lookup"><span data-stu-id="3f193-569">Requires the larger IIS stack size.</span></span>
* <span data-ttu-id="3f193-570">有 64 位元原生相依性。</span><span class="sxs-lookup"><span data-stu-id="3f193-570">Has 64-bit native dependencies.</span></span>

<span data-ttu-id="3f193-571">使用 64 位元 (x64) .NET Core SDK 來發行 64 位元應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-571">Use a 64-bit (x64) .NET Core SDK to publish a 64-bit app.</span></span> <span data-ttu-id="3f193-572">主機系統上必須有 64 位元執行階段存在。</span><span class="sxs-lookup"><span data-stu-id="3f193-572">A 64-bit runtime must be present on the host system.</span></span>

## <a name="hosting-models"></a><span data-ttu-id="3f193-573">裝載模型</span><span class="sxs-lookup"><span data-stu-id="3f193-573">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="3f193-574">同處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="3f193-574">In-process hosting model</span></span>

<span data-ttu-id="3f193-575">使用同處理序裝載，ASP.NET Core 應用程式會在與其 IIS 工作者處理序相同的處理序中執行。</span><span class="sxs-lookup"><span data-stu-id="3f193-575">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="3f193-576">因為要求未透過回送介面卡 (將連出網路流量傳回同一部電腦的網路介面) 進行 proxy 處理，所以同處理序裝載會提供優於跨處理序裝載的效能。</span><span class="sxs-lookup"><span data-stu-id="3f193-576">In-process hosting provides improved performance over out-of-process hosting because requests aren't proxied over the loopback adapter, a network interface that returns outgoing network traffic back to the same machine.</span></span> <span data-ttu-id="3f193-577">IIS 透過 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 來執行處理程序管理。</span><span class="sxs-lookup"><span data-stu-id="3f193-577">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="3f193-578">[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)：</span><span class="sxs-lookup"><span data-stu-id="3f193-578">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module):</span></span>

* <span data-ttu-id="3f193-579">執行應用程式初始化。</span><span class="sxs-lookup"><span data-stu-id="3f193-579">Performs app initialization.</span></span>
  * <span data-ttu-id="3f193-580">載入 [CoreCLR](/dotnet/standard/glossary#coreclr)。</span><span class="sxs-lookup"><span data-stu-id="3f193-580">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="3f193-581">呼叫 `Program.Main`。</span><span class="sxs-lookup"><span data-stu-id="3f193-581">Calls `Program.Main`.</span></span>
* <span data-ttu-id="3f193-582">處理 IIS 原生要求的存留期。</span><span class="sxs-lookup"><span data-stu-id="3f193-582">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="3f193-583">以 .NET Framework 為目標的 ASP.NET Core 應用程式不支援處理序內裝載模型。</span><span class="sxs-lookup"><span data-stu-id="3f193-583">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="3f193-584">下圖說明 IIS、ASP.NET Core 模組和同處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="3f193-584">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![同處理序代管內的 ASP.NET Core 模組案例](index/_static/ancm-inprocess.png)

<span data-ttu-id="3f193-586">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-586">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="3f193-587">驅動程式會在網站設定的連接埠上將原生要求路由至 IIS，此連接埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="3f193-587">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="3f193-588">ASP.NET Core 模組會接收原生要求，並將它傳遞至 IIS HTTP`IISHttpServer`伺服器（）。</span><span class="sxs-lookup"><span data-stu-id="3f193-588">The ASP.NET Core Module receives the native request and passes it to IIS HTTP Server (`IISHttpServer`).</span></span> <span data-ttu-id="3f193-589">IIS HTTP 伺服器是 IIS 的同處理序伺服程式實作，可將要求從原生轉換為受控。</span><span class="sxs-lookup"><span data-stu-id="3f193-589">IIS HTTP Server is an in-process server implementation for IIS that converts the request from native to managed.</span></span>

<span data-ttu-id="3f193-590">IIS HTTP 伺服器處理要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="3f193-590">After the IIS HTTP Server processes the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="3f193-591">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="3f193-591">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="3f193-592">應用程式的回應會透過 IIS HTTP 伺服器傳回 IIS。</span><span class="sxs-lookup"><span data-stu-id="3f193-592">The app's response is passed back to IIS through IIS HTTP Server.</span></span> <span data-ttu-id="3f193-593">IIS 會將回應傳送到起始該要求的用戶端。</span><span class="sxs-lookup"><span data-stu-id="3f193-593">IIS sends the response to the client that initiated the request.</span></span>

<span data-ttu-id="3f193-594">現有的應用程式可以選擇同處理序裝載，但 [dotnet new](/dotnet/core/tools/dotnet-new) 範本預設會對所有 IIS 和 IIS Express 案例使用同處理序裝載模型。</span><span class="sxs-lookup"><span data-stu-id="3f193-594">In-process hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="3f193-595">`CreateDefaultBuilder` 會透過呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> 方法來啟動 [CoreCLR](/dotnet/standard/glossary#coreclr) 以新增 <xref:Microsoft.AspNetCore.Hosting.Server.IServer>，並在 IIS 工作者處理序 (*w3wp.exe* 或 *iisexpress.exe*) 內裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-595">`CreateDefaultBuilder` adds an <xref:Microsoft.AspNetCore.Hosting.Server.IServer> instance by calling the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> method to boot the [CoreCLR](/dotnet/standard/glossary#coreclr) and host the app inside of the IIS worker process (*w3wp.exe* or *iisexpress.exe*).</span></span> <span data-ttu-id="3f193-596">效能測試指出，相較於跨處理序裝載 .NET Core 應用程式，並將要求 Proxy 處理至 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器，裝載於同處理序可提供明顯更高的要求輸送量。</span><span class="sxs-lookup"><span data-stu-id="3f193-596">Performance tests indicate that hosting a .NET Core app in-process delivers significantly higher request throughput compared to hosting the app out-of-process and proxying requests to [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="3f193-597">跨處理序裝載模型</span><span class="sxs-lookup"><span data-stu-id="3f193-597">Out-of-process hosting model</span></span>

<span data-ttu-id="3f193-598">因為 ASP.NET Core 應用程式會在與 IIS 背景工作進程不同的進程中執行，所以 ASP.NET Core 模組會處理進程管理。</span><span class="sxs-lookup"><span data-stu-id="3f193-598">Because ASP.NET Core apps run in a process separate from the IIS worker process, the ASP.NET Core Module handles process management.</span></span> <span data-ttu-id="3f193-599">此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式關閉或損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="3f193-599">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="3f193-600">此行為基本上與執行同處理序，並由 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="3f193-600">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="3f193-601">下圖說明 IIS、ASP.NET Core 模組和跨處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="3f193-601">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![非同處理序代管內的 ASP.NET Core 模組案例](index/_static/ancm-outofprocess.png)

<span data-ttu-id="3f193-603">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-603">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="3f193-604">驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="3f193-604">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="3f193-605">此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，而且不會是通訊埠 80 或 443。</span><span class="sxs-lookup"><span data-stu-id="3f193-605">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="3f193-606">此模組在啟動時透過環境變數指定連接埠，而 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 延伸模組則會設定伺服器來接聽 `http://localhost:{PORT}`。</span><span class="sxs-lookup"><span data-stu-id="3f193-606">The module specifies the port via an environment variable at startup, and the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> extension configures the server to listen on `http://localhost:{PORT}`.</span></span> <span data-ttu-id="3f193-607">將會執行額外檢查，不是源自模組的要求都會遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="3f193-607">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="3f193-608">此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。</span><span class="sxs-lookup"><span data-stu-id="3f193-608">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="3f193-609">Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="3f193-609">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="3f193-610">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="3f193-610">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="3f193-611">IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="3f193-611">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="3f193-612">應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="3f193-612">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="3f193-613">如需 ASP.NET Core 模組組態指南，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="3f193-613">For ASP.NET Core Module configuration guidance, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="3f193-614">如需代管的詳細資訊，請參閱[在 ASP.NET Core 中代管](xref:fundamentals/index#host)。</span><span class="sxs-lookup"><span data-stu-id="3f193-614">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/index#host).</span></span>

## <a name="application-configuration"></a><span data-ttu-id="3f193-615">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="3f193-615">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="3f193-616">啟用 IISIntegration 元件</span><span class="sxs-lookup"><span data-stu-id="3f193-616">Enable the IISIntegration components</span></span>

<span data-ttu-id="3f193-617">在（Program.cs）中`CreateWebHostBuilder`建立*Program.cs*主機時，請<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>呼叫以啟用 IIS 整合：</span><span class="sxs-lookup"><span data-stu-id="3f193-617">When building a host in `CreateWebHostBuilder` (*Program.cs*), call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to enable IIS integration:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

<span data-ttu-id="3f193-618">如需 `CreateDefaultBuilder` 的詳細資訊，請參閱 <xref:fundamentals/host/web-host#set-up-a-host>。</span><span class="sxs-lookup"><span data-stu-id="3f193-618">For more information on `CreateDefaultBuilder`, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

### <a name="iis-options"></a><span data-ttu-id="3f193-619">IIS 選項</span><span class="sxs-lookup"><span data-stu-id="3f193-619">IIS options</span></span>

<span data-ttu-id="3f193-620">**同處理序主控模型**</span><span class="sxs-lookup"><span data-stu-id="3f193-620">**In-process hosting model**</span></span>

<span data-ttu-id="3f193-621">若要設定 IIS 伺服器選項，請在 <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 中加入 <xref:Microsoft.AspNetCore.Builder.IISServerOptions> 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="3f193-621">To configure IIS Server options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISServerOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="3f193-622">下列範例會停用 AutomaticAuthentication：</span><span class="sxs-lookup"><span data-stu-id="3f193-622">The following example disables AutomaticAuthentication:</span></span>

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

| <span data-ttu-id="3f193-623">選項</span><span class="sxs-lookup"><span data-stu-id="3f193-623">Option</span></span>                         | <span data-ttu-id="3f193-624">預設</span><span class="sxs-lookup"><span data-stu-id="3f193-624">Default</span></span> | <span data-ttu-id="3f193-625">設定</span><span class="sxs-lookup"><span data-stu-id="3f193-625">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="3f193-626">若為 `true`，IIS 伺服器會設定由 [Windows 驗證](xref:security/authentication/windowsauth)所驗證的 `HttpContext.User`。</span><span class="sxs-lookup"><span data-stu-id="3f193-626">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="3f193-627">若為 `false`，則伺服器僅會對 `HttpContext.User` 提供身分識別，並在 `AuthenticationScheme` 明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="3f193-627">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="3f193-628">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="3f193-628">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="3f193-629">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="3f193-629">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="3f193-630">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="3f193-630">Sets the display name shown to users on login pages.</span></span> |

<span data-ttu-id="3f193-631">**跨處理序裝載模型**</span><span class="sxs-lookup"><span data-stu-id="3f193-631">**Out-of-process hosting model**</span></span>

<span data-ttu-id="3f193-632">若要設定 IIS 選項，請在 <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 中加入 <xref:Microsoft.AspNetCore.Builder.IISOptions> 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="3f193-632">To configure IIS options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="3f193-633">下列範例會防止應用程式填入 `HttpContext.Connection.ClientCertificate`：</span><span class="sxs-lookup"><span data-stu-id="3f193-633">The following example prevents the app from populating `HttpContext.Connection.ClientCertificate`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="3f193-634">選項</span><span class="sxs-lookup"><span data-stu-id="3f193-634">Option</span></span>                         | <span data-ttu-id="3f193-635">預設</span><span class="sxs-lookup"><span data-stu-id="3f193-635">Default</span></span> | <span data-ttu-id="3f193-636">設定</span><span class="sxs-lookup"><span data-stu-id="3f193-636">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="3f193-637">若為 `true`，[IIS 整合中介軟體](#enable-the-iisintegration-components)會設定由 [Windows 驗證](xref:security/authentication/windowsauth)所驗證的 `HttpContext.User`。</span><span class="sxs-lookup"><span data-stu-id="3f193-637">If `true`, [IIS Integration Middleware](#enable-the-iisintegration-components) sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="3f193-638">如果為 `false`，則驗證中介軟體僅針對 `HttpContext.User` 提供身分識別，並在游 `AuthenticationScheme` 提出明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="3f193-638">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="3f193-639">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="3f193-639">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="3f193-640">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="3f193-640">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="3f193-641">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="3f193-641">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="3f193-642">如果為 `true` 且 `MS-ASPNETCORE-CLIENTCERT` 要求標頭已存在，則會填入 `HttpContext.Connection.ClientCertificate`。</span><span class="sxs-lookup"><span data-stu-id="3f193-642">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="3f193-643">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="3f193-643">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="3f193-644">用來設定轉送標頭中介軟體及 ASP.NET Core 模組的 [IIS 整合中介軟體](#enable-the-iisintegration-components)會設定為轉送配置 (HTTP/HTTPS) 與發出要求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="3f193-644">The [IIS Integration Middleware](#enable-the-iisintegration-components), which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="3f193-645">其他 Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。</span><span class="sxs-lookup"><span data-stu-id="3f193-645">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="3f193-646">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="3f193-646">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="3f193-647">web.config 檔案</span><span class="sxs-lookup"><span data-stu-id="3f193-647">web.config file</span></span>

<span data-ttu-id="3f193-648">*web.config* 檔案是用來設定 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="3f193-648">The *web.config* file configures the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="3f193-649">發佈專案時，由 MSBuild 目標 (`_TransformWebConfig`) 處理 *web.config* 檔案的建立、轉換及發佈。</span><span class="sxs-lookup"><span data-stu-id="3f193-649">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="3f193-650">此目標存在於 Web SDK 目標 (`Microsoft.NET.Sdk.Web`)。</span><span class="sxs-lookup"><span data-stu-id="3f193-650">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="3f193-651">SDK 設定在專案檔的頂端：</span><span class="sxs-lookup"><span data-stu-id="3f193-651">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="3f193-652">如果專案中沒有 *web.config* 檔案，則系統會使用正確的 *processPath* 和 *arguments* 建立該檔案以設定 ASP.NET Core 模組，並將該檔案移至[已發行的輸出](xref:host-and-deploy/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="3f193-652">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="3f193-653">如果 *web.config* 檔案存在於專案中，則系統會使用正確的 *processPath* 和 *arguments* 來轉換該檔案以設定 ASP.NET Core 模組，然後將它移至已發行的輸出。</span><span class="sxs-lookup"><span data-stu-id="3f193-653">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="3f193-654">轉換不會修改檔案中的 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="3f193-654">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="3f193-655">*web.config* 檔案可提供能控制作用中 IIS 模組的額外 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="3f193-655">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="3f193-656">如需能處理 ASP.NET Core 應用程式要求之 IIS 模組的相關資訊，請參閱 [IIS 模組](xref:host-and-deploy/iis/modules)主題。</span><span class="sxs-lookup"><span data-stu-id="3f193-656">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="3f193-657">為防止 Web SDK 轉換 *web.config* 檔案，請使用專案檔中的 **\<IsTransformWebConfigDisabled>** 屬性：</span><span class="sxs-lookup"><span data-stu-id="3f193-657">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="3f193-658">使 Web SDK 無法轉換檔案時，應該由開發人員手動設定 *processPath* 和 *arguments*。</span><span class="sxs-lookup"><span data-stu-id="3f193-658">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="3f193-659">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="3f193-659">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="3f193-660">web.config 檔案位置</span><span class="sxs-lookup"><span data-stu-id="3f193-660">web.config file location</span></span>

<span data-ttu-id="3f193-661">為了正確設定[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module) *，web.config 檔案*必須存在於已部署應用程式的[內容根](xref:fundamentals/index#content-root)路徑（通常是應用程式基底路徑）。</span><span class="sxs-lookup"><span data-stu-id="3f193-661">In order to set up the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) correctly, the *web.config* file must be present at the [content root](xref:fundamentals/index#content-root) path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="3f193-662">這是與提供給 IIS 的網站實體路徑相同的位置。</span><span class="sxs-lookup"><span data-stu-id="3f193-662">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="3f193-663">應用程式的根目錄需有 *web.config* 檔案，才能使用 Web Deploy 發行多個應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-663">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="3f193-664">機密檔案存在於應用程式的實體路徑，例如\* \<元件>. .runtimeconfig.json. json*、 \* \<元件> .xml* （xml 檔批註）和\* \<元件>. .deps.json。\*</span><span class="sxs-lookup"><span data-stu-id="3f193-664">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="3f193-665">當 *web.config* 檔案存在且網站正常啟動時，如果有人要求機密檔案，IIS 不會予以提供。</span><span class="sxs-lookup"><span data-stu-id="3f193-665">When the *web.config* file is present and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="3f193-666">若 *web.config* 檔案遺失或沒有正確命名，或是無法設定網站以正常啟動，IIS 可能會公開提供機密檔案。</span><span class="sxs-lookup"><span data-stu-id="3f193-666">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="3f193-667">\***Web.config*檔案必須隨時存在於部署中、正確命名，而且能夠將網站設定為正常啟動。絕對不要從生產環境部署*移除 web.config 檔案\*。**</span><span class="sxs-lookup"><span data-stu-id="3f193-667">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

### <a name="transform-webconfig"></a><span data-ttu-id="3f193-668">轉換 web.config</span><span class="sxs-lookup"><span data-stu-id="3f193-668">Transform web.config</span></span>

<span data-ttu-id="3f193-669">如需在發佈時轉換 *web.config* (例如依據設定、設定檔或環境設定環境變數)，請參閱<xref:host-and-deploy/iis/transform-webconfig>。</span><span class="sxs-lookup"><span data-stu-id="3f193-669">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="3f193-670">IIS 組態</span><span class="sxs-lookup"><span data-stu-id="3f193-670">IIS configuration</span></span>

<span data-ttu-id="3f193-671">**Windows Server 作業系統**</span><span class="sxs-lookup"><span data-stu-id="3f193-671">**Windows Server operating systems**</span></span>

<span data-ttu-id="3f193-672">啟用**網頁伺服器 (IIS)** 伺服器角色，並建立角色服務。</span><span class="sxs-lookup"><span data-stu-id="3f193-672">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="3f193-673">使用來自 [管理]\*\*\*\* 功能表的 [新增角色及功能]\*\*\*\* 精靈，或是 [伺服器管理員]\*\*\*\* 中的連結。</span><span class="sxs-lookup"><span data-stu-id="3f193-673">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="3f193-674">在**伺服器角色**步驟中，核取 [網頁伺服器 (IIS)]\*\*\*\* 方塊。</span><span class="sxs-lookup"><span data-stu-id="3f193-674">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![在選取伺服器角色步驟中選取網頁伺服器 IIS 角色。](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="3f193-676">在 [功能]\*\*\*\* 步驟之後，[角色服務]\*\*\*\* 步驟會針對網頁伺服器 (IIS) 進行載入。</span><span class="sxs-lookup"><span data-stu-id="3f193-676">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="3f193-677">選取所需的 IIS 角色服務或接受所提供的預設角色服務。</span><span class="sxs-lookup"><span data-stu-id="3f193-677">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![在選取角色服務步驟中，選取預設的角色服務。](index/_static/role-services-ws2016.png)

   <span data-ttu-id="3f193-679">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="3f193-679">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="3f193-680">若要啟用 Windows 驗證，請展開下列節點： [**網頁伺服器** > **安全性**]。</span><span class="sxs-lookup"><span data-stu-id="3f193-680">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="3f193-681">選取 [Windows 驗證]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="3f193-681">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="3f193-682">如需詳細資訊，請參閱 [Windows 驗證 \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) 和[設定 Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="3f193-682">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="3f193-683">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="3f193-683">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="3f193-684">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="3f193-684">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="3f193-685">若要啟用 websocket，請展開下列節點： [**網頁伺服器** > **應用程式開發**]。</span><span class="sxs-lookup"><span data-stu-id="3f193-685">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="3f193-686">選取 [WebSocket 通訊協定]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="3f193-686">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="3f193-687">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="3f193-687">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="3f193-688">透過**確認**步驟繼續作業，安裝網頁伺服器角色和服務。</span><span class="sxs-lookup"><span data-stu-id="3f193-688">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="3f193-689">安裝**網頁伺服器（iis）** 角色之後，不需要重新開機伺服器/iis。</span><span class="sxs-lookup"><span data-stu-id="3f193-689">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="3f193-690">**Windows 桌面作業系統**</span><span class="sxs-lookup"><span data-stu-id="3f193-690">**Windows desktop operating systems**</span></span>

<span data-ttu-id="3f193-691">啟用 [IIS 管理主控台]\*\*\*\* 和 [World Wide Web 服務]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-691">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="3f193-692">瀏覽到 [控制台]\*\* [程式]\*\* > \*\* [程式和功能]\*\* > \*\*\*\* > **[開啟或關閉 Windows 功能]** \(畫面左側\)。</span><span class="sxs-lookup"><span data-stu-id="3f193-692">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="3f193-693">開啟 [Internet Information Services]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="3f193-693">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="3f193-694">開啟 [Web 管理工具]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="3f193-694">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="3f193-695">核取 [IIS 管理主控台]\*\*\*\* 方塊。</span><span class="sxs-lookup"><span data-stu-id="3f193-695">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="3f193-696">[World Wide Web Services] (全球資訊網服務)\*\*\*\* 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="3f193-696">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="3f193-697">接受**全球資訊網服務**的預設功能，或自訂 IIS 功能。</span><span class="sxs-lookup"><span data-stu-id="3f193-697">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="3f193-698">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="3f193-698">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="3f193-699">若要啟用 Windows 驗證，請展開下列節點： **World Wide Web 服務** > **安全性**。</span><span class="sxs-lookup"><span data-stu-id="3f193-699">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="3f193-700">選取 [Windows 驗證]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="3f193-700">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="3f193-701">如需詳細資訊，請參閱 [Windows 驗證 \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) 和[設定 Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="3f193-701">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="3f193-702">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="3f193-702">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="3f193-703">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="3f193-703">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="3f193-704">若要啟用 websocket，請展開下列節點： [ **World Wide Web 服務** > ] [**應用程式開發] 功能**。</span><span class="sxs-lookup"><span data-stu-id="3f193-704">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="3f193-705">選取 [WebSocket 通訊協定]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="3f193-705">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="3f193-706">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="3f193-706">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="3f193-707">若 IIS 安裝需要重新啟動，請重新啟動系統。</span><span class="sxs-lookup"><span data-stu-id="3f193-707">If the IIS installation requires a restart, restart the system.</span></span>

![選取 [Windows 功能] 中的 [IIS 管理主控台] 和 [World Wide Web Services] (全球資訊網服務)。](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="3f193-709">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="3f193-709">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="3f193-710">在主控系統上安裝 .NET Core 裝載套件組合\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-710">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="3f193-711">套件組合會安裝 .NET Core 執行時間、.NET Core 程式庫和[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="3f193-711">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="3f193-712">此模組可讓 ASP.NET Core 應用程式在 IIS 背後執行。</span><span class="sxs-lookup"><span data-stu-id="3f193-712">The module allows ASP.NET Core apps to run behind IIS.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3f193-713">若裝載套件組合在 IIS 之前安裝，則必須對該套件組合安裝進行修復。</span><span class="sxs-lookup"><span data-stu-id="3f193-713">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="3f193-714">請在安裝 IIS 之後，再次執行裝載套件組合安裝程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-714">Run the Hosting Bundle installer again after installing IIS.</span></span>
>
> <span data-ttu-id="3f193-715">如果在安裝 64 位元 (x64) 版本的 .NET Core 後才安裝裝載套件組合，那麼可能會遺漏 SDK ([未偵測到 .NET Core SDK](xref:test/troubleshoot#no-net-core-sdks-were-detected))。</span><span class="sxs-lookup"><span data-stu-id="3f193-715">If the Hosting Bundle is installed after installing the 64-bit (x64) version of .NET Core, SDKs might appear to be missing ([No .NET Core SDKs were detected](xref:test/troubleshoot#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="3f193-716">若要解決此問題，請參閱 <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>。</span><span class="sxs-lookup"><span data-stu-id="3f193-716">To resolve the problem, see <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>.</span></span>

### <a name="download"></a><span data-ttu-id="3f193-717">下載</span><span class="sxs-lookup"><span data-stu-id="3f193-717">Download</span></span>

1. <span data-ttu-id="3f193-718">流覽至 [[下載 .Net Core](https://dotnet.microsoft.com/download/dotnet-core) ] 頁面。</span><span class="sxs-lookup"><span data-stu-id="3f193-718">Navigate to the [Download .NET Core](https://dotnet.microsoft.com/download/dotnet-core) page.</span></span>
1. <span data-ttu-id="3f193-719">選取所需的 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="3f193-719">Select the desired .NET Core version.</span></span>
1. <span data-ttu-id="3f193-720">在 [執行應用程式 - 執行階段]\*\*\*\* 欄中，尋找想要的 .NET Core 執行階段版本列。</span><span class="sxs-lookup"><span data-stu-id="3f193-720">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="3f193-721">使用**裝載**套件組合連結來下載安裝程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-721">Download the installer using the **Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="3f193-722">某些安裝程式包含已達到期生命週期結束 (EOL) 的發行版本，這些發行版本已不受 Microsoft 支援。</span><span class="sxs-lookup"><span data-stu-id="3f193-722">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="3f193-723">如需詳細資訊，請參閱[支援原則](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) \(英文 \)。</span><span class="sxs-lookup"><span data-stu-id="3f193-723">For more information, see the [support policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="3f193-724">安裝裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="3f193-724">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="3f193-725">在伺服器上執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-725">Run the installer on the server.</span></span> <span data-ttu-id="3f193-726">從系統管理員命令殼層執行安裝程式時，有 下列參數可用：</span><span class="sxs-lookup"><span data-stu-id="3f193-726">The following parameters are available when running the installer from an administrator command shell:</span></span>

   * <span data-ttu-id="3f193-727">`OPT_NO_ANCM=1`&ndash;略過安裝 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="3f193-727">`OPT_NO_ANCM=1` &ndash; Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="3f193-728">`OPT_NO_RUNTIME=1`&ndash;略過安裝 .net Core 執行時間。</span><span class="sxs-lookup"><span data-stu-id="3f193-728">`OPT_NO_RUNTIME=1` &ndash; Skip installing the .NET Core runtime.</span></span> <span data-ttu-id="3f193-729">當伺服器只裝載[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)時使用。</span><span class="sxs-lookup"><span data-stu-id="3f193-729">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="3f193-730">`OPT_NO_SHAREDFX=1`&ndash;略過安裝 ASP.NET 共用架構（ASP.NET 執行時間）。</span><span class="sxs-lookup"><span data-stu-id="3f193-730">`OPT_NO_SHAREDFX=1` &ndash; Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span> <span data-ttu-id="3f193-731">當伺服器只裝載[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)時使用。</span><span class="sxs-lookup"><span data-stu-id="3f193-731">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="3f193-732">`OPT_NO_X86=1`&ndash;略過安裝 x86 執行時間。</span><span class="sxs-lookup"><span data-stu-id="3f193-732">`OPT_NO_X86=1` &ndash; Skip installing x86 runtimes.</span></span> <span data-ttu-id="3f193-733">當您確定不會裝載 32 位元應用程式時，請使用此參數。</span><span class="sxs-lookup"><span data-stu-id="3f193-733">Use this parameter when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="3f193-734">如果將來有可能同時裝載 32 位元和 64 位元應用程式，請不要使用此參數並安裝這兩個執行階段。</span><span class="sxs-lookup"><span data-stu-id="3f193-734">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this parameter and install both runtimes.</span></span>
   * <span data-ttu-id="3f193-735">`OPT_NO_SHARED_CONFIG_CHECK=1` &ndash; 停用使用 IIS 共用設定 (當共用設定 (*applicationHost.config*) 位於與 IIS 安裝相同的機器上時) 進行檢查。</span><span class="sxs-lookup"><span data-stu-id="3f193-735">`OPT_NO_SHARED_CONFIG_CHECK=1` &ndash; Disable the check for using an IIS Shared Configuration when the shared configuration (*applicationHost.config*) is on the same machine as the IIS installation.</span></span> <span data-ttu-id="3f193-736">*只在 ASP.NET Core 2.2 或更新版本的裝載套件組合安裝程式上可用。*</span><span class="sxs-lookup"><span data-stu-id="3f193-736">*Only available for ASP.NET Core 2.2 or later Hosting Bundler installers.*</span></span> <span data-ttu-id="3f193-737">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>。</span><span class="sxs-lookup"><span data-stu-id="3f193-737">For more information, see <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>.</span></span>
1. <span data-ttu-id="3f193-738">重新開機系統，或在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="3f193-738">Restart the system or execute the following commands in a command shell:</span></span>

   ```console
   net stop was /y
   net start w3svc
   ```
   <span data-ttu-id="3f193-739">重新啟動 IIS 將能偵測到由安裝程式對系統路徑 (此為環境變數) 所做出的變更。</span><span class="sxs-lookup"><span data-stu-id="3f193-739">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

<span data-ttu-id="3f193-740">安裝裝載套件組合時，不需要手動停止 IIS 中的個別網站。</span><span class="sxs-lookup"><span data-stu-id="3f193-740">It isn't necessary to manually stop individual sites in IIS when installing the Hosting Bundle.</span></span> <span data-ttu-id="3f193-741">裝載的應用程式（IIS 網站）會在 IIS 重新開機時重新開機。</span><span class="sxs-lookup"><span data-stu-id="3f193-741">Hosted apps (IIS sites) restart when IIS restarts.</span></span> <span data-ttu-id="3f193-742">應用程式會在收到第一個要求時重新開機，包括從[應用程式初始化模組](#application-initialization-module-and-idle-timeout)。</span><span class="sxs-lookup"><span data-stu-id="3f193-742">Apps start up again when they receive their first request, including from the [Application Initialization Module](#application-initialization-module-and-idle-timeout).</span></span>

<span data-ttu-id="3f193-743">ASP.NET Core 採用共用架構封裝修補程式版本的向前復原行為。</span><span class="sxs-lookup"><span data-stu-id="3f193-743">ASP.NET Core adopts roll-forward behavior for patch releases of shared framework packages.</span></span> <span data-ttu-id="3f193-744">當 IIS 所裝載的應用程式使用 IIS 重新開機時，應用程式會在收到第一個要求時，以其所參考套件的最新修補程式版本來載入。</span><span class="sxs-lookup"><span data-stu-id="3f193-744">When apps hosted by IIS restart with IIS, the apps load with the latest patch releases of their referenced packages when they receive their first request.</span></span> <span data-ttu-id="3f193-745">如果未重新開機 IIS，應用程式會在其工作者進程回收時重新開機並展示向前復原行為，並接收其第一個要求。</span><span class="sxs-lookup"><span data-stu-id="3f193-745">If IIS isn't restarted, apps restart and exhibit roll-forward behavior when their worker processes are recycled and they receive their first request.</span></span>

> [!NOTE]
> <span data-ttu-id="3f193-746">如需 IIS 共用組態的資訊，請參閱[使用 IIS 共用組態的 ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)。</span><span class="sxs-lookup"><span data-stu-id="3f193-746">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="3f193-747">使用 Visual Studio 發佈時安裝 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="3f193-747">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="3f193-748">將應用程式部署到具有 [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later) 的伺服器時，請在伺服器上安裝最新版的 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="3f193-748">When deploying apps to servers with [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="3f193-749">若要安裝 Web Deploy，請使用 [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) 或從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=43717)直接取得安裝程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-749">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="3f193-750">慣用的方法是使用 WebPI。</span><span class="sxs-lookup"><span data-stu-id="3f193-750">The preferred method is to use WebPI.</span></span> <span data-ttu-id="3f193-751">WebPI 提供獨立的安裝程式和組態以裝載提供者。</span><span class="sxs-lookup"><span data-stu-id="3f193-751">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="3f193-752">建立 IIS 網站</span><span class="sxs-lookup"><span data-stu-id="3f193-752">Create the IIS site</span></span>

1. <span data-ttu-id="3f193-753">在主控系統上，請建立資料夾以容納應用程式的已發行資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="3f193-753">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="3f193-754">在下列步驟中，您提供資料夾路徑給 IIS，作為應用程式的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="3f193-754">In a following step, the folder's path is provided to IIS as the physical path to the app.</span></span> <span data-ttu-id="3f193-755">如需應用程式之部署資料夾和檔案配置的詳細資訊，請參閱 <xref:host-and-deploy/directory-structure>。</span><span class="sxs-lookup"><span data-stu-id="3f193-755">For more information on an app's deployment folder and file layout, see <xref:host-and-deploy/directory-structure>.</span></span>

1. <span data-ttu-id="3f193-756">在 [IIS 管理員] 中 **，在 [** 連線] 面板中開啟伺服器的節點。</span><span class="sxs-lookup"><span data-stu-id="3f193-756">In IIS Manager, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="3f193-757">以滑鼠右鍵按一下 [網站]\*\*\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3f193-757">Right-click the **Sites** folder.</span></span> <span data-ttu-id="3f193-758">從操作功能表選取 [新增網站]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-758">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="3f193-759">提供**網站名稱**，並將**實體路徑**設定為應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="3f193-759">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="3f193-760">藉由**Binding**選取 **[確定]** 來提供系結設定並建立網站：</span><span class="sxs-lookup"><span data-stu-id="3f193-760">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![在新增網站步驟中提供站台名稱、實體路徑和主機名稱。](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="3f193-762">請**勿**使用最上層萬用字元繫結 (`http://*:80/`與 `http://+:80`)。</span><span class="sxs-lookup"><span data-stu-id="3f193-762">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="3f193-763">最上層萬用字元繫結可能暴露您的應用程式安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="3f193-763">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="3f193-764">這對強式與弱式萬用字元皆適用。</span><span class="sxs-lookup"><span data-stu-id="3f193-764">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="3f193-765">請使用明確主機名稱，而非萬用字元。</span><span class="sxs-lookup"><span data-stu-id="3f193-765">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="3f193-766">若您擁有整個父網域 (與具弱點的 `*.com` 相對) 的控制權，則子網域萬用字元繫結 (例如 `*.mysub.com`) 就沒有此安全性風險。</span><span class="sxs-lookup"><span data-stu-id="3f193-766">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="3f193-767">如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="3f193-767">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="3f193-768">在伺服器的節點之下，選取 [應用程式集區]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-768">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="3f193-769">以滑鼠右鍵按一下網站的應用程式集區，然後從操作功能表選取 [基本設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-769">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="3f193-770">在 [編輯應用程式集區]\*\*\*\* 視窗中，將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\*：</span><span class="sxs-lookup"><span data-stu-id="3f193-770">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![將 .NET CLR 版本設為 [沒有受控碼]。](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="3f193-772">ASP.NET Core 會在不同的處理序中執行，並管理執行階段。</span><span class="sxs-lookup"><span data-stu-id="3f193-772">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="3f193-773">ASP.NET Core 不仰賴載入桌面 CLR (.NET CLR)&mdash;會使用 .NET Core 的核心通用語言執行平台 (CoreCLR) 來開機以在背景工作處理序中裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-773">ASP.NET Core doesn't rely on loading the desktop CLR (.NET CLR)&mdash;the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process.</span></span> <span data-ttu-id="3f193-774">將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\* 是選擇性的，但建議這樣做。</span><span class="sxs-lookup"><span data-stu-id="3f193-774">Setting the **.NET CLR version** to **No Managed Code** is optional but recommended.</span></span>

1. <span data-ttu-id="3f193-775">*ASP.NET Core 2.2 或更新版本*：對於使用[同處理序主控模型](#in-process-hosting-model)的 64 位元 (x64) [獨立式部署](/dotnet/core/deploying/#self-contained-deployments-scd)，會停用 32 位元 (x86) 處理序的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="3f193-775">*ASP.NET Core 2.2 or later*: For a 64-bit (x64) [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) that uses the [in-process hosting model](#in-process-hosting-model), disable the app pool for 32-bit (x86) processes.</span></span>

   <span data-ttu-id="3f193-776">在 IIS 管理員的 [動作]\*\*\*\* 資訊看板 > [應用程式集區]\*\*\*\* 中，選取 [設定應用程式集區預設值]\*\*\*\* 或 [進階設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-776">In the **Actions** sidebar of IIS Manager > **Application Pools**, select **Set Application Pool Defaults** or **Advanced Settings**.</span></span> <span data-ttu-id="3f193-777">找到 [啟用 32 位元應用程式]\*\*\*\*，然後將其值設定為 `False`。</span><span class="sxs-lookup"><span data-stu-id="3f193-777">Locate **Enable 32-Bit Applications** and set the value to `False`.</span></span> <span data-ttu-id="3f193-778">此設定不會影響為[處理程序外裝載](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model)部署的應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-778">This setting doesn't affect apps deployed for [out-of-process hosting](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).</span></span>

1. <span data-ttu-id="3f193-779">確認處理序模型身分識別具有適當的權限。</span><span class="sxs-lookup"><span data-stu-id="3f193-779">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="3f193-780">如果應用程式集區的預設識別（**進程模型** > **識別**）從**ApplicationPoolIdentity**變更為另一個身分識別，請確認新的身分識別具有存取應用程式資料夾、資料庫和其他必要資源的必要許可權。</span><span class="sxs-lookup"><span data-stu-id="3f193-780">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="3f193-781">例如，應用程式集區需要針對應用程式讀取和寫入檔案的資料夾取得讀取和寫入權限。</span><span class="sxs-lookup"><span data-stu-id="3f193-781">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="3f193-782">**Windows 驗證設定 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="3f193-782">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="3f193-783">如需詳細資訊，請參閱[設定 Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="3f193-783">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="3f193-784">部署應用程式</span><span class="sxs-lookup"><span data-stu-id="3f193-784">Deploy the app</span></span>

<span data-ttu-id="3f193-785">將應用程式部署至在[建立 IIS 網站](#create-the-iis-site)一節中所建立的 IIS **實體路徑**資料夾。</span><span class="sxs-lookup"><span data-stu-id="3f193-785">Deploy the app to the IIS **Physical path** folder that was established in the [Create the IIS site](#create-the-iis-site) section.</span></span> <span data-ttu-id="3f193-786">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 是建議的部署機制，但有數個選項可將應用程式從專案的 [publish]\*\* 資料夾移動至主機系統的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="3f193-786">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment, but several options exist for moving the app from the project's *publish* folder to the hosting system's deployment folder.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="3f193-787">使用 Visual Studio 的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="3f193-787">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="3f193-788">請參閱[適用於 ASP.NET Core 應用程式部署的 Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)主題，了解如何建立搭配 Web Deploy 使用的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="3f193-788">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="3f193-789">如果主機服務提供者提供或支援建立發行設定檔，請下載其設定檔，並使用 Visual Studio 的 [發行]\*\*\*\* 對話方塊匯入其設定檔：</span><span class="sxs-lookup"><span data-stu-id="3f193-789">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog:</span></span>

![[發佈] 對話方塊頁](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="3f193-791">Visual Studio 外部的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="3f193-791">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="3f193-792">透過命令列也可以在 Visual Studio 外部使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy)。</span><span class="sxs-lookup"><span data-stu-id="3f193-792">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="3f193-793">如需詳細資訊，請參閱 [Web 部署工具](/iis/publish/using-web-deploy/use-the-web-deployment-tool)。</span><span class="sxs-lookup"><span data-stu-id="3f193-793">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="3f193-794">Web Deploy 的替代項目</span><span class="sxs-lookup"><span data-stu-id="3f193-794">Alternatives to Web Deploy</span></span>

<span data-ttu-id="3f193-795">使用數種方法的其中一種將應用程式移到主機系統，例如手動複製、[Xcopy](/windows-server/administration/windows-commands/xcopy)、[Robocopy](/windows-server/administration/windows-commands/robocopy) 或 [PowerShell](/powershell/)。</span><span class="sxs-lookup"><span data-stu-id="3f193-795">Use any of several methods to move the app to the hosting system, such as manual copy, [Xcopy](/windows-server/administration/windows-commands/xcopy), [Robocopy](/windows-server/administration/windows-commands/robocopy), or [PowerShell](/powershell/).</span></span>

<span data-ttu-id="3f193-796">如需 IIS 的 ASP.NET Core 部署詳細資訊，請參閱 [IIS 系統管理員的部署資源](#deployment-resources-for-iis-administrators)一節。</span><span class="sxs-lookup"><span data-stu-id="3f193-796">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="3f193-797">瀏覽網站</span><span class="sxs-lookup"><span data-stu-id="3f193-797">Browse the website</span></span>

<span data-ttu-id="3f193-798">應用程式部署到主機系統之後，請向其中一個應用程式的公用端點發出要求。</span><span class="sxs-lookup"><span data-stu-id="3f193-798">After the app is deployed to the hosting system, make a request to one of the app's public endpoints.</span></span>

<span data-ttu-id="3f193-799">在下列範例中，網站繫結至 IIS 上的**連接埠** `80`，其**主機名稱**為 `www.mysite.com`。</span><span class="sxs-lookup"><span data-stu-id="3f193-799">In the following example, the site is bound to an IIS **Host name** of `www.mysite.com` on **Port** `80`.</span></span> <span data-ttu-id="3f193-800">對 `http://www.mysite.com` 發出要求：</span><span class="sxs-lookup"><span data-stu-id="3f193-800">A request is made to `http://www.mysite.com`:</span></span>

![Microsoft Edge 瀏覽器已載入 IIS 啟動頁面。](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="3f193-802">已鎖定的部署檔案</span><span class="sxs-lookup"><span data-stu-id="3f193-802">Locked deployment files</span></span>

<span data-ttu-id="3f193-803">當應用程式執行時，會鎖定部署資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="3f193-803">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="3f193-804">無法於部署期間覆寫已鎖定的檔案。</span><span class="sxs-lookup"><span data-stu-id="3f193-804">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="3f193-805">若要釋放部署中的已鎖定檔案，請使用下列其中**一種**方法停止應用程式集區：</span><span class="sxs-lookup"><span data-stu-id="3f193-805">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="3f193-806">使用 Web Deploy 並參考專案檔中的 `Microsoft.NET.Sdk.Web`。</span><span class="sxs-lookup"><span data-stu-id="3f193-806">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="3f193-807">*app_offline.htm* 檔案是放在 Web 應用程式目錄的根目錄中。</span><span class="sxs-lookup"><span data-stu-id="3f193-807">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="3f193-808">當檔案存在時，ASP.NET Core 模組會正常關閉應用程式，並在部署期間提供 *app_offline.htm* 檔案。</span><span class="sxs-lookup"><span data-stu-id="3f193-808">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="3f193-809">如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)。</span><span class="sxs-lookup"><span data-stu-id="3f193-809">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="3f193-810">在伺服器上的 IIS 管理員中手動停止應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="3f193-810">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="3f193-811">使用 PowerShell 卸載*app_offline .htm* （需要 PowerShell 5 或更新版本）：</span><span class="sxs-lookup"><span data-stu-id="3f193-811">Use PowerShell to drop *app_offline.htm* (requires PowerShell 5 or later):</span></span>

  ```powershell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a><span data-ttu-id="3f193-812">資料保護</span><span class="sxs-lookup"><span data-stu-id="3f193-812">Data protection</span></span>

<span data-ttu-id="3f193-813">[ASP.NET Core 資料保護堆疊](xref:security/data-protection/introduction)是由數個 ASP.NET Core [中介軟體](xref:fundamentals/middleware/index)所使用，包括用於驗證的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="3f193-813">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="3f193-814">即使資料保護 API 不是由使用者程式碼呼叫，仍應使用部署指令碼或是在使用者程式碼中設定資料保護，以建立持續性的密碼編譯[金鑰存放區](xref:security/data-protection/implementation/key-management)。</span><span class="sxs-lookup"><span data-stu-id="3f193-814">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="3f193-815">如不設定資料保護，金鑰會保留在記憶體中，並於應用程式重新啟動時捨棄。</span><span class="sxs-lookup"><span data-stu-id="3f193-815">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="3f193-816">如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：</span><span class="sxs-lookup"><span data-stu-id="3f193-816">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="3f193-817">所有以 Cookie 為基礎的驗證權杖都會失效。</span><span class="sxs-lookup"><span data-stu-id="3f193-817">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="3f193-818">當使用者提出下一個要求時，需要再次登入。</span><span class="sxs-lookup"><span data-stu-id="3f193-818">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="3f193-819">所有以 Keyring 保護的資料都無法再解密。</span><span class="sxs-lookup"><span data-stu-id="3f193-819">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="3f193-820">這可能會包含 [CSRF 權杖](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)和 [ASP.NET Core MVC TempData cookie](xref:fundamentals/app-state#tempdata)。</span><span class="sxs-lookup"><span data-stu-id="3f193-820">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="3f193-821">若要在 IIS 下設定資料保護以保存 Keyring，請使用下列其中**一種**方法：</span><span class="sxs-lookup"><span data-stu-id="3f193-821">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="3f193-822">**建立資料保護登錄機碼**</span><span class="sxs-lookup"><span data-stu-id="3f193-822">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="3f193-823">ASP.NET Core 應用程式所使用的資料保護金鑰會儲存在應用程式外部的登錄中。</span><span class="sxs-lookup"><span data-stu-id="3f193-823">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="3f193-824">若要保存指定應用程式的金鑰，請為應用程式集區建立登錄機碼。</span><span class="sxs-lookup"><span data-stu-id="3f193-824">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="3f193-825">若為獨立的非Web 伺服陣列 IIS 安裝，請針對搭配使用 ASP.NET Core 應用程式的每個應用程式集區，使用[資料保護 Provision-AutoGenKeys.ps1 PowerShell 指令碼](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1)。</span><span class="sxs-lookup"><span data-stu-id="3f193-825">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="3f193-826">此指令碼會在 HKLM 登錄中建立登錄機碼，只有應用程式之應用程式集區的背景工作處理序帳戶可以存取它。</span><span class="sxs-lookup"><span data-stu-id="3f193-826">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="3f193-827">在待用期間使用 DPAPI 和全電腦金鑰加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="3f193-827">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="3f193-828">在 Web 伺服陣列案例中，應用程式可以設定成使用 UNC 路徑來儲存其資料保護 Keyring。</span><span class="sxs-lookup"><span data-stu-id="3f193-828">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="3f193-829">根據預設，資料保護金鑰不予加密。</span><span class="sxs-lookup"><span data-stu-id="3f193-829">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="3f193-830">請確保網路共用的檔案權限僅限於執行應用程式的 Windows 帳戶。</span><span class="sxs-lookup"><span data-stu-id="3f193-830">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="3f193-831">可以使用 X509 憑證來保護待用的金鑰。</span><span class="sxs-lookup"><span data-stu-id="3f193-831">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="3f193-832">請考慮下列讓使用者上傳憑證的機制：將憑證放入使用者的受信任憑證存放區，並確保在執行使用者應用程式的所有電腦上都能使用這些憑證。</span><span class="sxs-lookup"><span data-stu-id="3f193-832">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="3f193-833">如需詳細資訊，請參閱[設定 ASP.NET Core 資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="3f193-833">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="3f193-834">**設定 IIS 應用程式集區載入使用者設定檔**</span><span class="sxs-lookup"><span data-stu-id="3f193-834">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="3f193-835">此設定位在應用程式集區 [進階設定]\*\*\*\* 下的 [處理序模型]\*\*\*\* 區段中。</span><span class="sxs-lookup"><span data-stu-id="3f193-835">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="3f193-836">將 [載入使用者設定檔]\*\*\*\* 設為 `True`。</span><span class="sxs-lookup"><span data-stu-id="3f193-836">Set **Load User Profile** to `True`.</span></span> <span data-ttu-id="3f193-837">當設定為 `True` 時，金鑰會儲存在使用者設定檔目錄中，且使用具有使用者帳戶專屬金鑰的 DPAPI 保護。</span><span class="sxs-lookup"><span data-stu-id="3f193-837">When set to `True`, keys are stored in the user profile directory and protected using DPAPI with a key specific to the user account.</span></span> <span data-ttu-id="3f193-838">金鑰會保存到 *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3f193-838">Keys are persisted to the *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* folder.</span></span>

  <span data-ttu-id="3f193-839">應用程式集區的 [setProfileEnvironment 屬性](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration)也必須啟用。</span><span class="sxs-lookup"><span data-stu-id="3f193-839">The app pool's [setProfileEnvironment attribute](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) must also be enabled.</span></span> <span data-ttu-id="3f193-840">`setProfileEnvironment` 的預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="3f193-840">The default value of `setProfileEnvironment` is `true`.</span></span> <span data-ttu-id="3f193-841">在某些情況下 (例如 Windows OS)，`setProfileEnvironment` 會設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="3f193-841">In some scenarios (for example, Windows OS), `setProfileEnvironment` is set to `false`.</span></span> <span data-ttu-id="3f193-842">如果金鑰並未如預期地儲存在使用者設定檔目錄中：</span><span class="sxs-lookup"><span data-stu-id="3f193-842">If keys aren't stored in the user profile directory as expected:</span></span>

  1. <span data-ttu-id="3f193-843">瀏覽至 *%windir%/system32/inetsrv/config* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3f193-843">Navigate to the *%windir%/system32/inetsrv/config* folder.</span></span>
  1. <span data-ttu-id="3f193-844">開啟 *applicationHost.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="3f193-844">Open the *applicationHost.config* file.</span></span>
  1. <span data-ttu-id="3f193-845">找出 `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` 元素。</span><span class="sxs-lookup"><span data-stu-id="3f193-845">Locate the `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` element.</span></span>
  1. <span data-ttu-id="3f193-846">確認 `setProfileEnvironment` 屬性不存在 (其預設值為 `true`)，或明確地將屬性值設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="3f193-846">Confirm that the `setProfileEnvironment` attribute isn't present, which defaults the value to `true`, or explicitly set the attribute's value to `true`.</span></span>

* <span data-ttu-id="3f193-847">**將檔案系統當作 Keyring 存放區使用**</span><span class="sxs-lookup"><span data-stu-id="3f193-847">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="3f193-848">調整應用程式程式碼，以[將檔案系統當作 Keyring 存放區使用](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="3f193-848">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="3f193-849">使用 X509 憑證來保護 Keyring，並確保憑證是受信任的憑證。</span><span class="sxs-lookup"><span data-stu-id="3f193-849">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="3f193-850">如果憑證為自我簽署，請將憑證放在受信任的根存放區。</span><span class="sxs-lookup"><span data-stu-id="3f193-850">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="3f193-851">在 Web 伺服陣列中使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="3f193-851">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="3f193-852">請使用所有電腦都可以存取的檔案共用。</span><span class="sxs-lookup"><span data-stu-id="3f193-852">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="3f193-853">將 X509 憑證部署到每一部電腦。</span><span class="sxs-lookup"><span data-stu-id="3f193-853">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="3f193-854">設定[程式碼中的資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="3f193-854">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="3f193-855">**設定資料保護的全電腦原則**</span><span class="sxs-lookup"><span data-stu-id="3f193-855">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="3f193-856">針對取用資料保護 API 的所有應用程式，資料保護系統僅支援有限的預設[全電腦原則](xref:security/data-protection/configuration/machine-wide-policy)設定。</span><span class="sxs-lookup"><span data-stu-id="3f193-856">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="3f193-857">如需詳細資訊，請參閱<xref:security/data-protection/introduction>。</span><span class="sxs-lookup"><span data-stu-id="3f193-857">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="virtual-directories"></a><span data-ttu-id="3f193-858">虛擬目錄</span><span class="sxs-lookup"><span data-stu-id="3f193-858">Virtual Directories</span></span>

<span data-ttu-id="3f193-859">ASP.NET Core 應用程式不支援 [IIS 虛擬目錄](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories)。</span><span class="sxs-lookup"><span data-stu-id="3f193-859">[IIS Virtual Directories](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) aren't supported with ASP.NET Core apps.</span></span> <span data-ttu-id="3f193-860">應用程式能以[子應用程式](#sub-applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="3f193-860">An app can be hosted as a [sub-application](#sub-applications).</span></span>

## <a name="sub-applications"></a><span data-ttu-id="3f193-861">子應用程式</span><span class="sxs-lookup"><span data-stu-id="3f193-861">Sub-applications</span></span>

<span data-ttu-id="3f193-862">ASP.NET Core 應用程式能以 [IIS 子應用程式](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="3f193-862">An ASP.NET Core app can be hosted as an [IIS sub-application (sub-app)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications).</span></span> <span data-ttu-id="3f193-863">子應用程式的路徑會成為根應用程式 URL 的一部分。</span><span class="sxs-lookup"><span data-stu-id="3f193-863">The sub-app's path becomes part of the root app's URL.</span></span>

<span data-ttu-id="3f193-864">子應用程式內的靜態資產連結應該使用波狀符號與斜線 (`~/`) 標記法。</span><span class="sxs-lookup"><span data-stu-id="3f193-864">Static asset links within the sub-app should use tilde-slash (`~/`) notation.</span></span> <span data-ttu-id="3f193-865">波狀符號與斜線標記法會觸發[標記協助程式](xref:mvc/views/tag-helpers/intro)以將子應用程式的路徑基底附加到轉譯的相對連結前面。</span><span class="sxs-lookup"><span data-stu-id="3f193-865">Tilde-slash notation triggers a [Tag Helper](xref:mvc/views/tag-helpers/intro) to prepend the sub-app's pathbase to the rendered relative link.</span></span> <span data-ttu-id="3f193-866">針對位於 `/subapp_path` 的子應用程式，使用 `src="~/image.png"` 連結的影像會轉譯為 `src="/subapp_path/image.png"`。</span><span class="sxs-lookup"><span data-stu-id="3f193-866">For a sub-app at `/subapp_path`, an image linked with `src="~/image.png"` is rendered as `src="/subapp_path/image.png"`.</span></span> <span data-ttu-id="3f193-867">根應用程式的靜態檔案中介軟體不會處理靜態檔案要求。</span><span class="sxs-lookup"><span data-stu-id="3f193-867">The root app's Static File Middleware doesn't process the static file request.</span></span> <span data-ttu-id="3f193-868">要求會由子應用程式的靜態檔案中介軟體處理。</span><span class="sxs-lookup"><span data-stu-id="3f193-868">The request is processed by the sub-app's Static File Middleware.</span></span>

<span data-ttu-id="3f193-869">若靜態資產的 `src` 屬性是設定為絕對路徑 (例如，`src="/image.png"`)，會以不使用子應用程式路徑基底的方式轉譯連結。</span><span class="sxs-lookup"><span data-stu-id="3f193-869">If a static asset's `src` attribute is set to an absolute path (for example, `src="/image.png"`), the link is rendered without the sub-app's pathbase.</span></span> <span data-ttu-id="3f193-870">根應用程式的靜態檔案中介軟體會嘗試從根應用程式的 [webroot](xref:fundamentals/index#web-root) 提供資產，這會導致「404 - 找不到」\*\* 回應 (除非根應用程式可存取靜態資產)。</span><span class="sxs-lookup"><span data-stu-id="3f193-870">The root app's Static File Middleware attempts to serve the asset from the root app's [web root](xref:fundamentals/index#web-root), which results in a *404 - Not Found* response unless the static asset is available from the root app.</span></span>

<span data-ttu-id="3f193-871">裝載 ASP.NET Core 應用程式做為另一個 ASP.NET Core 應用程式下的子應用程式：</span><span class="sxs-lookup"><span data-stu-id="3f193-871">To host an ASP.NET Core app as a sub-app under another ASP.NET Core app:</span></span>

1. <span data-ttu-id="3f193-872">為子應用程式建立應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="3f193-872">Establish an app pool for the sub-app.</span></span> <span data-ttu-id="3f193-873">將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\*，因為會將核心通用語言執行平台 (CoreCLR) 開機以在背景工作處理序中裝載應用程式，而非在桌面 CLR (.NET CLR) 中裝載。</span><span class="sxs-lookup"><span data-stu-id="3f193-873">Set the **.NET CLR Version** to **No Managed Code** because the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process, not the desktop CLR (.NET CLR).</span></span>

1. <span data-ttu-id="3f193-874">使用根網站下資料夾中的子應用程式在 IIS 管理員中新增根網站。</span><span class="sxs-lookup"><span data-stu-id="3f193-874">Add the root site in IIS Manager with the sub-app in a folder under the root site.</span></span>

1. <span data-ttu-id="3f193-875">以滑鼠右鍵按一下 IIS 管理員中的子應用程式資料夾，然後選取 [轉換成應用程式]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-875">Right-click the sub-app folder in IIS Manager and select **Convert to Application**.</span></span>

1. <span data-ttu-id="3f193-876">在 [新增應用程式]\*\*\*\* 對話方塊中，使用 [應用程式集區]\*\*\*\* 的[選取]\*\*\*\* 按鈕來指派您為子應用程式建立的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="3f193-876">In the **Add Application** dialog, use the **Select** button for the **Application Pool** to assign the app pool that you created for the sub-app.</span></span> <span data-ttu-id="3f193-877">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="3f193-877">Select **OK**.</span></span>

<span data-ttu-id="3f193-878">將不同的應用程式集區指派給子應用程式是使用同處理序裝載模型。</span><span class="sxs-lookup"><span data-stu-id="3f193-878">The assignment of a separate app pool to the sub-app is a requirement when using the in-process hosting model.</span></span>

<span data-ttu-id="3f193-879">如需有關同進程裝載模型和設定 ASP.NET Core 模組的詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="3f193-879">For more information on the in-process hosting model and configuring the ASP.NET Core Module, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="3f193-880">使用 web.config 的 IIS 組態</span><span class="sxs-lookup"><span data-stu-id="3f193-880">Configuration of IIS with web.config</span></span>

<span data-ttu-id="3f193-881">在對使用了 ASP.NET Core 模組的 ASP.NET Core 有作用的 IIS 情境下，設定會受 *web.config* 的 `<system.webServer>` 區段影響。</span><span class="sxs-lookup"><span data-stu-id="3f193-881">IIS configuration is influenced by the `<system.webServer>` section of *web.config* for IIS scenarios that are functional for ASP.NET Core apps with the ASP.NET Core Module.</span></span> <span data-ttu-id="3f193-882">舉例來說，IIS 設定對動態壓縮有作用。</span><span class="sxs-lookup"><span data-stu-id="3f193-882">For example, IIS configuration is functional for dynamic compression.</span></span> <span data-ttu-id="3f193-883">如果在伺服器層級將 IIS 設為使用動態壓縮，應用程式 *web.config* 檔案中的 `<urlCompression>` 元素則可為 ASP.NET Core 應用程式予以停用。</span><span class="sxs-lookup"><span data-stu-id="3f193-883">If IIS is configured at the server level to use dynamic compression, the `<urlCompression>` element in the app's *web.config* file can disable it for an ASP.NET Core app.</span></span>

<span data-ttu-id="3f193-884">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="3f193-884">For more information, see the following topics:</span></span>

* [<span data-ttu-id="3f193-885">System.webserver>的\<設定參考</span><span class="sxs-lookup"><span data-stu-id="3f193-885">Configuration reference for \<system.webServer></span></span>](/iis/configuration/system.webServer/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>

<span data-ttu-id="3f193-886">若要設定在隔離的應用程式集區中執行之個別應用程式的環境變數 (支援 IIS 10.0 或更新版本)，請參閱 IIS 參考文件之[環境變數 \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的 *AppCmd.exe 命令*一節。</span><span class="sxs-lookup"><span data-stu-id="3f193-886">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="3f193-887">web.config 的組態區段</span><span class="sxs-lookup"><span data-stu-id="3f193-887">Configuration sections of web.config</span></span>

<span data-ttu-id="3f193-888">ASP.NET Core 應用程式的設定不使用 *web.config* 中 ASP.NET 4.x 應用程式的設定區段：</span><span class="sxs-lookup"><span data-stu-id="3f193-888">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

<span data-ttu-id="3f193-889">使用其他組態提供者設定的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-889">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="3f193-890">如需詳細資訊，請參閱[Configuration](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="3f193-890">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="3f193-891">應用程式集區</span><span class="sxs-lookup"><span data-stu-id="3f193-891">Application Pools</span></span>

<span data-ttu-id="3f193-892">應用程式集區隔離取決於裝載模型：</span><span class="sxs-lookup"><span data-stu-id="3f193-892">App pool isolation is determined by the hosting model:</span></span>

* <span data-ttu-id="3f193-893">處理序內裝載 &ndash; 應用程式必須在分開的應用程式集區中執行。</span><span class="sxs-lookup"><span data-stu-id="3f193-893">In-process hosting &ndash; Apps are required to run in separate app pools.</span></span>
* <span data-ttu-id="3f193-894">處理序外裝載 &ndash; 建議藉由在各自的應用程式集區中執行每個應用程式，以將應用程式互相隔離。</span><span class="sxs-lookup"><span data-stu-id="3f193-894">Out-of-process hosting &ndash; We recommend isolating the apps from each other by running each app in its own app pool.</span></span>

<span data-ttu-id="3f193-895">IIS [新增網站]\*\*\*\* 對話方塊預設每個應用程式皆為單一應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="3f193-895">The IIS **Add Website** dialog defaults to a single app pool per app.</span></span> <span data-ttu-id="3f193-896">當提供**網站名稱**時，文字會自動轉移至 [應用程式集區]\*\*\*\* 文字方塊。</span><span class="sxs-lookup"><span data-stu-id="3f193-896">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="3f193-897">新增網站時，會使用該網站名稱建立新的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="3f193-897">A new app pool is created using the site name when the site is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="3f193-898">應用程式集區身分識別</span><span class="sxs-lookup"><span data-stu-id="3f193-898">Application Pool Identity</span></span>

<span data-ttu-id="3f193-899">應用程式集區身分識別帳戶可讓應用程式在唯一的帳戶下執行，不必建立及管理網域或本機帳戶。</span><span class="sxs-lookup"><span data-stu-id="3f193-899">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="3f193-900">在 IIS 8.0 或更新版本中，IIS 管理背景工作處理序 (WAS) 會使用新的應用程式集區名稱建立虛擬帳戶，並預設在此帳戶下執行應用程式集區的背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="3f193-900">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="3f193-901">在 IIS 管理主控台中，於應用程式集區的 [進階設定]\*\*\*\* 下，確定 [身分識別]\*\*\*\* 設定為使用 **ApplicationPoolIdentity**：</span><span class="sxs-lookup"><span data-stu-id="3f193-901">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![應用程式集區進階設定對話方塊](index/_static/apppool-identity.png)

<span data-ttu-id="3f193-903">IIS 管理程序會在 Windows 安全系統中，以應用程式集區的名稱建立安全識別碼。</span><span class="sxs-lookup"><span data-stu-id="3f193-903">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="3f193-904">可使用此身分識別來保護資源。</span><span class="sxs-lookup"><span data-stu-id="3f193-904">Resources can be secured using this identity.</span></span> <span data-ttu-id="3f193-905">不過，此身分識別不是真正的使用者帳戶，也不會顯示在 Windows 使用者管理主控台中。</span><span class="sxs-lookup"><span data-stu-id="3f193-905">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="3f193-906">如果 IIS 背景工作處理序需要提升應用程式的存取權限，請修改包含應用程式的目錄存取控制清單 (ACL)：</span><span class="sxs-lookup"><span data-stu-id="3f193-906">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="3f193-907">開啟 Windows 檔案總管，巡覽至目錄。</span><span class="sxs-lookup"><span data-stu-id="3f193-907">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="3f193-908">以滑鼠右鍵按一下目錄並選取 [屬性]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-908">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="3f193-909">依序選取 [安全性]\*\*\*\* 索引標籤下的 [編輯]\*\*\*\* 按鈕和 [新增]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="3f193-909">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="3f193-910">選取 [位置]\*\*\*\* 按鈕，並確定選取系統。</span><span class="sxs-lookup"><span data-stu-id="3f193-910">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="3f193-911">在 [輸入要選取的物件名稱]\*\*\*\* 區域中，輸入 **IIS AppPool\\<app_pool_name>**。</span><span class="sxs-lookup"><span data-stu-id="3f193-911">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="3f193-912">選取 [檢查名稱]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="3f193-912">Select the **Check Names** button.</span></span> <span data-ttu-id="3f193-913">針對 [DefaultAppPool]\*\*，請使用 **IIS AppPool\DefaultAppPool** 檢查名稱。</span><span class="sxs-lookup"><span data-stu-id="3f193-913">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="3f193-914">選取 [檢查名稱]\*\*\*\* 按鈕時，[DefaultAppPool]\*\*\*\* 的值便會顯示於物件名稱區域中。</span><span class="sxs-lookup"><span data-stu-id="3f193-914">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="3f193-915">您無法直接將應用程式集區名稱輸入至物件名稱區域。</span><span class="sxs-lookup"><span data-stu-id="3f193-915">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="3f193-916">檢查物件名稱時，請使用 **IIS AppPool\\<app_pool_name>** 的格式。</span><span class="sxs-lookup"><span data-stu-id="3f193-916">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：在選取 [檢查名稱] 之前，"DefaultAppPool" 這個應用程式集區名稱在物件名稱區域中會附加至 "IIS AppPool\"。](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="3f193-918">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="3f193-918">Select **OK**.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：選取 [檢查名稱] 之後，物件名稱 "DefaultAppPool" 會顯示在物件名稱區域中。](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="3f193-920">預設應該會授與讀取 &amp; 執行權限。</span><span class="sxs-lookup"><span data-stu-id="3f193-920">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="3f193-921">請視需要提供其他權限。</span><span class="sxs-lookup"><span data-stu-id="3f193-921">Provide additional permissions as needed.</span></span>

<span data-ttu-id="3f193-922">也可使用 **ICACLS** 工具透過命令提示字元授與存取權限。</span><span class="sxs-lookup"><span data-stu-id="3f193-922">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="3f193-923">使用 *DefaultAppPool*作為範例，將會使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="3f193-923">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="3f193-924">如需詳細資訊，請參閱 [icacls](/windows-server/administration/windows-commands/icacls) 主題。</span><span class="sxs-lookup"><span data-stu-id="3f193-924">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="http2-support"></a><span data-ttu-id="3f193-925">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="3f193-925">HTTP/2 support</span></span>

<span data-ttu-id="3f193-926">在下列 IIS 部署案例中，ASP.NET Core 支援 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="3f193-926">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following IIS deployment scenarios:</span></span>

* <span data-ttu-id="3f193-927">內含式</span><span class="sxs-lookup"><span data-stu-id="3f193-927">In-process</span></span>
  * <span data-ttu-id="3f193-928">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="3f193-928">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="3f193-929">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="3f193-929">TLS 1.2 or later connection</span></span>
* <span data-ttu-id="3f193-930">跨處理序</span><span class="sxs-lookup"><span data-stu-id="3f193-930">Out-of-process</span></span>
  * <span data-ttu-id="3f193-931">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="3f193-931">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="3f193-932">公開 Edge Server 連線使用 HTTP/2，但是對 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的反向 Proxy 連線使用 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="3f193-932">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
  * <span data-ttu-id="3f193-933">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="3f193-933">TLS 1.2 or later connection</span></span>

<span data-ttu-id="3f193-934">針對建立 HTTP/2 連線時的同處理序部署，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會回報 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="3f193-934">For an in-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span> <span data-ttu-id="3f193-935">針對建立 HTTP/2 連線時的跨處理序部署，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會回報 `HTTP/1.1`。</span><span class="sxs-lookup"><span data-stu-id="3f193-935">For an out-of-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="3f193-936">如需有關同處理序和跨處理序主控模型的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="3f193-936">For more information on the in-process and out-of-process hosting models, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="3f193-937">HTTP/2 預設為啟用。</span><span class="sxs-lookup"><span data-stu-id="3f193-937">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="3f193-938">如果 HTTP/2 連線尚未建立，連線會退為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="3f193-938">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="3f193-939">如需使用 IIS 部署之 HTTP/2 設定的詳細資訊，請參閱 [IIS 上的 HTTP/2](/iis/get-started/whats-new-in-iis-10/http2-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="3f193-939">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="cors-preflight-requests"></a><span data-ttu-id="3f193-940">CORS 預檢要求</span><span class="sxs-lookup"><span data-stu-id="3f193-940">CORS preflight requests</span></span>

<span data-ttu-id="3f193-941">*此節只適用於以 .NET Framework 為目標的 ASP.NET Core 應用程式。*</span><span class="sxs-lookup"><span data-stu-id="3f193-941">*This section only applies to ASP.NET Core apps that target the .NET Framework.*</span></span>

<span data-ttu-id="3f193-942">針對以 .NET Framework 為目標的 ASP.NET Core 應用程式，在 IIS 中OPTIONS 要求預設不會傳遞到應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-942">For an ASP.NET Core app that targets the .NET Framework, OPTIONS requests aren't passed to the app by default in IIS.</span></span> <span data-ttu-id="3f193-943">若要瞭解如何在 web.config 中設定應用程式的 IIS 處理*程式來傳遞*選項要求，請參閱[在 ASP.NET Web API 2 中啟用跨原始來源要求： CORS 的運作方式](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works)。</span><span class="sxs-lookup"><span data-stu-id="3f193-943">To learn how to configure the app's IIS handlers in *web.config* to pass OPTIONS requests, see [Enable cross-origin requests in ASP.NET Web API 2: How CORS Works](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).</span></span>

## <a name="application-initialization-module-and-idle-timeout"></a><span data-ttu-id="3f193-944">應用程式初始化模組與閒置逾時</span><span class="sxs-lookup"><span data-stu-id="3f193-944">Application Initialization Module and Idle Timeout</span></span>

<span data-ttu-id="3f193-945">在 IIS 中由 ASP.NET Core 模組版本 2 裝載時：</span><span class="sxs-lookup"><span data-stu-id="3f193-945">When hosted in IIS by the ASP.NET Core Module version 2:</span></span>

* <span data-ttu-id="3f193-946">[應用程式初始化模組](#application-initialization-module) &ndash;應用程式的裝載同[進程](#in-process-hosting-model)或跨[進程](#out-of-process-hosting-model)，可以設定為在背景工作進程重新開機或伺服器重新開機時自動啟動。</span><span class="sxs-lookup"><span data-stu-id="3f193-946">[Application Initialization Module](#application-initialization-module) &ndash; App's hosted [in-process](#in-process-hosting-model) or [out-of-process](#out-of-process-hosting-model) can be configured to start automatically on a worker process restart or server restart.</span></span>
* <span data-ttu-id="3f193-947">[閒置逾時](#idle-timeout) &ndash; 應用程式的裝載 [同處理序](#in-process-hosting-model)可設定為在無活動期間不逾時。</span><span class="sxs-lookup"><span data-stu-id="3f193-947">[Idle Timeout](#idle-timeout) &ndash; App's hosted [in-process](#in-process-hosting-model) can be configured not to timeout during periods of inactivity.</span></span>

### <a name="application-initialization-module"></a><span data-ttu-id="3f193-948">應用程式初始化模組</span><span class="sxs-lookup"><span data-stu-id="3f193-948">Application Initialization Module</span></span>

<span data-ttu-id="3f193-949">*套用到應用程式裝載同處理序與非同處理序。*</span><span class="sxs-lookup"><span data-stu-id="3f193-949">*Applies to apps hosted in-process and out-of-process.*</span></span>

<span data-ttu-id="3f193-950">[IIS 應用程式初始化](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)是一項 IIS 功能，可在應用程式集區啟動或回收時，將 HTTP 要求傳送給應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-950">[IIS Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) is an IIS feature that sends an HTTP request to the app when the app pool starts or is recycled.</span></span> <span data-ttu-id="3f193-951">要求會觸發應用程式啟動。</span><span class="sxs-lookup"><span data-stu-id="3f193-951">The request triggers the app to start.</span></span> <span data-ttu-id="3f193-952">根據預設，IIS 會發出應用程式根 URL (`/`) 的要求以初始化應用程式 (如需有關設定的詳細資訊，請參閱[額外資源](#application-initialization-module-and-idle-timeout-additional-resources))。</span><span class="sxs-lookup"><span data-stu-id="3f193-952">By default, IIS issues a request to the app's root URL (`/`) to initialize the app (see the [additional resources](#application-initialization-module-and-idle-timeout-additional-resources) for more details on configuration).</span></span>

<span data-ttu-id="3f193-953">確認已啟用「IIS 應用程式初始化」角色功能：</span><span class="sxs-lookup"><span data-stu-id="3f193-953">Confirm that the IIS Application Initialization role feature in enabled:</span></span>

<span data-ttu-id="3f193-954">在 Windows 7 或更新的電腦系統上，當在本機使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="3f193-954">On Windows 7 or later desktop systems when using IIS locally:</span></span>

1. <span data-ttu-id="3f193-955">瀏覽到 [控制台]\*\* [程式]\*\* > \*\* [程式和功能]\*\* > \*\*\*\* > **[開啟或關閉 Windows 功能]** \(畫面左側\)。</span><span class="sxs-lookup"><span data-stu-id="3f193-955">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="3f193-956">開啟 [網際網路資訊服務]\*\* [World Wide Web 服務]\*\* > \*\* [應用程式開發功能]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-956">Open **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="3f193-957">選取 [應用程式初始化]\*\*\*\* 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="3f193-957">Select the check box for **Application Initialization**.</span></span>

<span data-ttu-id="3f193-958">在 Windows Server 2008 R2 或更新版本上：</span><span class="sxs-lookup"><span data-stu-id="3f193-958">On Windows Server 2008 R2 or later:</span></span>

1. <span data-ttu-id="3f193-959">開啟「新增角色與功能精靈」\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-959">Open the **Add Roles and Features Wizard**.</span></span>
1. <span data-ttu-id="3f193-960">在 [選取角色服務]\*\*\*\* 面板中，開啟 [應用程式開發]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="3f193-960">In the **Select role services** panel, open the **Application Development** node.</span></span>
1. <span data-ttu-id="3f193-961">選取 [應用程式初始化]\*\*\*\* 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="3f193-961">Select the check box for **Application Initialization**.</span></span>

<span data-ttu-id="3f193-962">使用下列任一方式為網站啟用應用程式初始化模組：</span><span class="sxs-lookup"><span data-stu-id="3f193-962">Use either of the following approaches to enable the Application Initialization Module for the site:</span></span>

* <span data-ttu-id="3f193-963">使用 IIS 管理員：</span><span class="sxs-lookup"><span data-stu-id="3f193-963">Using IIS Manager:</span></span>

  1. <span data-ttu-id="3f193-964">選取 [連線]\*\*\*\* 面板中的 [應用程式集區]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-964">Select **Application Pools** in the **Connections** panel.</span></span>
  1. <span data-ttu-id="3f193-965">以滑鼠右鍵按一下清單中應用程式的應用程式集區，然後選取 [進階設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-965">Right-click the app's app pool in the list and select **Advanced Settings**.</span></span>
  1. <span data-ttu-id="3f193-966">預設的 [啟動模式]\*\*\*\* 是 [OnDemand]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-966">The default **Start Mode** is **OnDemand**.</span></span> <span data-ttu-id="3f193-967">將 [啟動模式]\*\*\*\* 設定為 [AlwaysRunning]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-967">Set the **Start Mode** to **AlwaysRunning**.</span></span> <span data-ttu-id="3f193-968">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="3f193-968">Select **OK**.</span></span>
  1. <span data-ttu-id="3f193-969">開啟 [連線]\*\*\*\* 面板中的 [站台]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="3f193-969">Open the **Sites** node in the **Connections** panel.</span></span>
  1. <span data-ttu-id="3f193-970">以滑鼠右鍵按一下應用程式，然後選取 [管理網站]\*\* [進階設定]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-970">Right-click the app and select **Manage Website** > **Advanced Settings**.</span></span>
  1. <span data-ttu-id="3f193-971">預設 [預先載入已啟用]\*\*\*\* 設定是 [False]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-971">The default **Preload Enabled** setting is **False**.</span></span> <span data-ttu-id="3f193-972">將 [預先載入已啟用]\*\*\*\* 設定為 [True]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-972">Set **Preload Enabled** to **True**.</span></span> <span data-ttu-id="3f193-973">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="3f193-973">Select **OK**.</span></span>

* <span data-ttu-id="3f193-974">使用 *web.config*，新增 `<applicationInitialization>` 元素並將 `doAppInitAfterRestart` 設定為 `true` 至應用程式 *web.config* 檔案中的 `<system.webServer>` 元素：</span><span class="sxs-lookup"><span data-stu-id="3f193-974">Using *web.config*, add the `<applicationInitialization>` element with `doAppInitAfterRestart` set to `true` to the `<system.webServer>` elements in the app's *web.config* file:</span></span>

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

### <a name="idle-timeout"></a><span data-ttu-id="3f193-975">閒置逾時</span><span class="sxs-lookup"><span data-stu-id="3f193-975">Idle Timeout</span></span>

<span data-ttu-id="3f193-976">*僅適用於應用程式裝載同處理序。*</span><span class="sxs-lookup"><span data-stu-id="3f193-976">*Only applies to apps hosted in-process.*</span></span>

<span data-ttu-id="3f193-977">若要防止應用程式閒置，請使用 IIS 管理員設定應用程式集區的閒置逾時：</span><span class="sxs-lookup"><span data-stu-id="3f193-977">To prevent the app from idling, set the app pool's idle timeout using IIS Manager:</span></span>

1. <span data-ttu-id="3f193-978">選取 [連線]\*\*\*\* 面板中的 [應用程式集區]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-978">Select **Application Pools** in the **Connections** panel.</span></span>
1. <span data-ttu-id="3f193-979">以滑鼠右鍵按一下清單中應用程式的應用程式集區，然後選取 [進階設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-979">Right-click the app's app pool in the list and select **Advanced Settings**.</span></span>
1. <span data-ttu-id="3f193-980">預設 [閒置逾時 (分鐘)]\*\*\*\* 是 **20** 分鐘。</span><span class="sxs-lookup"><span data-stu-id="3f193-980">The default **Idle Time-out (minutes)** is **20** minutes.</span></span> <span data-ttu-id="3f193-981">將 [閒置逾時 (分鐘)]\*\*\*\* 設定為 **0** (零)。</span><span class="sxs-lookup"><span data-stu-id="3f193-981">Set the **Idle Time-out (minutes)** to **0** (zero).</span></span> <span data-ttu-id="3f193-982">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="3f193-982">Select **OK**.</span></span>
1. <span data-ttu-id="3f193-983">回收背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="3f193-983">Recycle the worker process.</span></span>

<span data-ttu-id="3f193-984">若要防止應用程式裝載[非同處理序](#out-of-process-hosting-model)逾時，請使用下列任一方式：</span><span class="sxs-lookup"><span data-stu-id="3f193-984">To prevent apps hosted [out-of-process](#out-of-process-hosting-model) from timing out, use either of the following approaches:</span></span>

* <span data-ttu-id="3f193-985">從外部服務對應用程式執行 Ping 以讓它繼續執行。</span><span class="sxs-lookup"><span data-stu-id="3f193-985">Ping the app from an external service in order to keep it running.</span></span>
* <span data-ttu-id="3f193-986">若應用程式只裝載背景服務，避免 IIS 裝載並使用 [Windows 服務來裝載 ASP.NET Core 應用程式](xref:host-and-deploy/windows-service)。</span><span class="sxs-lookup"><span data-stu-id="3f193-986">If the app only hosts background services, avoid IIS hosting and use a [Windows Service to host the ASP.NET Core app](xref:host-and-deploy/windows-service).</span></span>

### <a name="application-initialization-module-and-idle-timeout-additional-resources"></a><span data-ttu-id="3f193-987">應用程式初始化模組與閒置逾時額外資源</span><span class="sxs-lookup"><span data-stu-id="3f193-987">Application Initialization Module and Idle Timeout additional resources</span></span>

* [<span data-ttu-id="3f193-988">IIS 8.0 應用程式初始化</span><span class="sxs-lookup"><span data-stu-id="3f193-988">IIS 8.0 Application Initialization</span></span>](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)
* <span data-ttu-id="3f193-989">[應用程式初始化 \<applicationInitialization>](/iis/configuration/system.webserver/applicationinitialization/)。</span><span class="sxs-lookup"><span data-stu-id="3f193-989">[Application Initialization \<applicationInitialization>](/iis/configuration/system.webserver/applicationinitialization/).</span></span>
* <span data-ttu-id="3f193-990">[應用程式集區的處理序模組設定 \<processModel>](/iis/configuration/system.applicationhost/applicationpools/add/processmodel)。</span><span class="sxs-lookup"><span data-stu-id="3f193-990">[Process Model Settings for an Application Pool \<processModel>](/iis/configuration/system.applicationhost/applicationpools/add/processmodel).</span></span>

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="3f193-991">IIS 系統管理員的部署資源</span><span class="sxs-lookup"><span data-stu-id="3f193-991">Deployment resources for IIS administrators</span></span>

* [<span data-ttu-id="3f193-992">IIS 文件</span><span class="sxs-lookup"><span data-stu-id="3f193-992">IIS documentation</span></span>](/iis)
* [<span data-ttu-id="3f193-993">IIS 中的 IIS 管理員使用者入門</span><span class="sxs-lookup"><span data-stu-id="3f193-993">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* [<span data-ttu-id="3f193-994">.NET Core 應用程式部署</span><span class="sxs-lookup"><span data-stu-id="3f193-994">.NET Core application deployment</span></span>](/dotnet/core/deploying/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/directory-structure>
* <xref:host-and-deploy/iis/modules>
* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>

## <a name="additional-resources"></a><span data-ttu-id="3f193-995">其他資源</span><span class="sxs-lookup"><span data-stu-id="3f193-995">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:index>
* [<span data-ttu-id="3f193-996">Microsoft IIS 官方網站</span><span class="sxs-lookup"><span data-stu-id="3f193-996">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="3f193-997">Windows Server 技術內容庫</span><span class="sxs-lookup"><span data-stu-id="3f193-997">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="3f193-998">IIS 上的 HTTP/2</span><span class="sxs-lookup"><span data-stu-id="3f193-998">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
* <xref:host-and-deploy/iis/transform-webconfig>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="3f193-999">如需將 ASP.NET Core 應用程式發佈至 IIS 伺服器的教學課程體驗，請參閱<xref:tutorials/publish-to-iis>。</span><span class="sxs-lookup"><span data-stu-id="3f193-999">For a tutorial experience on publishing an ASP.NET Core app to an IIS server, see <xref:tutorials/publish-to-iis>.</span></span>

[<span data-ttu-id="3f193-1000">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="3f193-1000">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="3f193-1001">支援的作業系統</span><span class="sxs-lookup"><span data-stu-id="3f193-1001">Supported operating systems</span></span>

<span data-ttu-id="3f193-1002">以下為支援的作業系統：</span><span class="sxs-lookup"><span data-stu-id="3f193-1002">The following operating systems are supported:</span></span>

* <span data-ttu-id="3f193-1003">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="3f193-1003">Windows 7 or later</span></span>
* <span data-ttu-id="3f193-1004">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="3f193-1004">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="3f193-1005">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys) (先前稱為 WebListener) 不適用搭配 IIS 的反向 Proxy 設定。</span><span class="sxs-lookup"><span data-stu-id="3f193-1005">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="3f193-1006">請使用 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1006">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="3f193-1007">如需在 Azure 中裝載的資訊，請參閱 <xref:host-and-deploy/azure-apps/index>。</span><span class="sxs-lookup"><span data-stu-id="3f193-1007">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

<span data-ttu-id="3f193-1008">如需疑難排解指引，請參閱 <xref:test/troubleshoot>。</span><span class="sxs-lookup"><span data-stu-id="3f193-1008">For troubleshooting guidance, see <xref:test/troubleshoot>.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="3f193-1009">支援的平台</span><span class="sxs-lookup"><span data-stu-id="3f193-1009">Supported platforms</span></span>

<span data-ttu-id="3f193-1010">支援針對 32 位元 (x86) 或 64 位元 (x64) 部署發行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-1010">Apps published for 32-bit (x86) or 64-bit (x64) deployment are supported.</span></span> <span data-ttu-id="3f193-1011">使用 32 位元 (x86) .NET Core SDK 部署 32 位元應用程式，除非應用程式：</span><span class="sxs-lookup"><span data-stu-id="3f193-1011">Deploy a 32-bit app with a 32-bit (x86) .NET Core SDK unless the app:</span></span>

* <span data-ttu-id="3f193-1012">需要提供給 64 位元應用程式使用的較大虛擬記憶體位址空間。</span><span class="sxs-lookup"><span data-stu-id="3f193-1012">Requires the larger virtual memory address space available to a 64-bit app.</span></span>
* <span data-ttu-id="3f193-1013">需要較大的 IIS 堆疊大小。</span><span class="sxs-lookup"><span data-stu-id="3f193-1013">Requires the larger IIS stack size.</span></span>
* <span data-ttu-id="3f193-1014">有 64 位元原生相依性。</span><span class="sxs-lookup"><span data-stu-id="3f193-1014">Has 64-bit native dependencies.</span></span>

<span data-ttu-id="3f193-1015">使用 64 位元 (x64) .NET Core SDK 來發行 64 位元應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-1015">Use a 64-bit (x64) .NET Core SDK to publish a 64-bit app.</span></span> <span data-ttu-id="3f193-1016">主機系統上必須有 64 位元執行階段存在。</span><span class="sxs-lookup"><span data-stu-id="3f193-1016">A 64-bit runtime must be present on the host system.</span></span>

<span data-ttu-id="3f193-1017">ASP.NET Core 隨附 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)，其為預設的跨平台 HTTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="3f193-1017">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), a default, cross-platform HTTP server.</span></span>

<span data-ttu-id="3f193-1018">在使用 [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) 或 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 時，應用程式會執行於從 IIS 背景工作處理序中分離出的處理序 (跨處理序\*\*)，並搭配 [Kestrel 伺服器](xref:fundamentals/servers/index#kestrel)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1018">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the app runs in a process separate from the IIS worker process (*out-of-process*) with the [Kestrel server](xref:fundamentals/servers/index#kestrel).</span></span>

<span data-ttu-id="3f193-1019">因為 ASP.NET Core 應用程式執行所在的處理序會與 IIS 工作者處理序分開，所以此模組會執行處理程序管理。</span><span class="sxs-lookup"><span data-stu-id="3f193-1019">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="3f193-1020">此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式關閉或損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="3f193-1020">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="3f193-1021">此行為基本上與執行同處理序，並由 [Windows 處理序啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="3f193-1021">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="3f193-1022">下圖說明 IIS、ASP.NET Core 模組和跨處理序裝載應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="3f193-1022">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![ASP.NET Core 模組](index/_static/ancm-outofprocess.png)

<span data-ttu-id="3f193-1024">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-1024">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="3f193-1025">驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1025">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="3f193-1026">此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，而且不會是通訊埠 80 或 443。</span><span class="sxs-lookup"><span data-stu-id="3f193-1026">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="3f193-1027">此模組會在啟動時透過環境變數指定埠，而[IIS 整合中介軟體](xref:host-and-deploy/iis/index#enable-the-iisintegration-components)會設定伺服器來接聽`http://localhost:{port}`。</span><span class="sxs-lookup"><span data-stu-id="3f193-1027">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="3f193-1028">將會執行額外檢查，不是源自模組的要求都會遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="3f193-1028">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="3f193-1029">此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。</span><span class="sxs-lookup"><span data-stu-id="3f193-1029">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="3f193-1030">Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="3f193-1030">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="3f193-1031">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="3f193-1031">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="3f193-1032">IIS Integration 新增的中介軟體會更新配置、遠端 IP 和帳戶路徑基底，以將要求轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="3f193-1032">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="3f193-1033">應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="3f193-1033">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="3f193-1034">`CreateDefaultBuilder` 會將 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器設為網頁伺服器，並設定 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)的基底路徑與連接埠來啟用 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="3f193-1034">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and enables IIS Integration by configuring the base path and port for the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="3f193-1035">ASP.NET Core 模組會產生要指派給後端處理序的動態連接埠。</span><span class="sxs-lookup"><span data-stu-id="3f193-1035">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="3f193-1036">`CreateDefaultBuilder` 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 方法。</span><span class="sxs-lookup"><span data-stu-id="3f193-1036">`CreateDefaultBuilder` calls the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> method.</span></span> <span data-ttu-id="3f193-1037">`UseIISIntegration` 會將 Kestrel 設定為在位於 localhost IP 位址 (`127.0.0.1`) 的動態連接埠上接聽。</span><span class="sxs-lookup"><span data-stu-id="3f193-1037">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="3f193-1038">若動態連接埠是 1234，Kestrel 會在 `127.0.0.1:1234` 接聽。</span><span class="sxs-lookup"><span data-stu-id="3f193-1038">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="3f193-1039">此設定會取代由下列項目提供的其他 URL 設定：</span><span class="sxs-lookup"><span data-stu-id="3f193-1039">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="3f193-1040">Kestrel 的接聽 API</span><span class="sxs-lookup"><span data-stu-id="3f193-1040">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="3f193-1041">[設定](xref:fundamentals/configuration/index) (或[命令列 --urls 選項](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="3f193-1041">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="3f193-1042">在使用模組時不需要呼叫 `UseUrls` 或 Kestrel 的 `Listen` API。</span><span class="sxs-lookup"><span data-stu-id="3f193-1042">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="3f193-1043">若呼叫 `UseUrls` 或 `Listen`，Kestrel 只會接聽未使用 IIS 執行應用程式時指定的連接埠。</span><span class="sxs-lookup"><span data-stu-id="3f193-1043">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

<span data-ttu-id="3f193-1044">如需 ASP.NET Core 模組組態指南，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="3f193-1044">For ASP.NET Core Module configuration guidance, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="3f193-1045">如需代管的詳細資訊，請參閱[在 ASP.NET Core 中代管](xref:fundamentals/index#host)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1045">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/index#host).</span></span>

## <a name="application-configuration"></a><span data-ttu-id="3f193-1046">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="3f193-1046">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="3f193-1047">啟用 IISIntegration 元件</span><span class="sxs-lookup"><span data-stu-id="3f193-1047">Enable the IISIntegration components</span></span>

<span data-ttu-id="3f193-1048">在（Program.cs）中`CreateWebHostBuilder`建立*Program.cs*主機時，請<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>呼叫以啟用 IIS 整合：</span><span class="sxs-lookup"><span data-stu-id="3f193-1048">When building a host in `CreateWebHostBuilder` (*Program.cs*), call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to enable IIS integration:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

<span data-ttu-id="3f193-1049">如需 `CreateDefaultBuilder` 的詳細資訊，請參閱 <xref:fundamentals/host/web-host#set-up-a-host>。</span><span class="sxs-lookup"><span data-stu-id="3f193-1049">For more information on `CreateDefaultBuilder`, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

### <a name="iis-options"></a><span data-ttu-id="3f193-1050">IIS 選項</span><span class="sxs-lookup"><span data-stu-id="3f193-1050">IIS options</span></span>

| <span data-ttu-id="3f193-1051">選項</span><span class="sxs-lookup"><span data-stu-id="3f193-1051">Option</span></span>                         | <span data-ttu-id="3f193-1052">預設</span><span class="sxs-lookup"><span data-stu-id="3f193-1052">Default</span></span> | <span data-ttu-id="3f193-1053">設定</span><span class="sxs-lookup"><span data-stu-id="3f193-1053">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="3f193-1054">若為 `true`，IIS 伺服器會設定由 [Windows 驗證](xref:security/authentication/windowsauth)所驗證的 `HttpContext.User`。</span><span class="sxs-lookup"><span data-stu-id="3f193-1054">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="3f193-1055">若為 `false`，則伺服器僅會對 `HttpContext.User` 提供身分識別，並在 `AuthenticationScheme` 明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="3f193-1055">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="3f193-1056">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="3f193-1056">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="3f193-1057">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1057">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="3f193-1058">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="3f193-1058">Sets the display name shown to users on login pages.</span></span> |

<span data-ttu-id="3f193-1059">若要設定 IIS 選項，請在 <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 中加入 <xref:Microsoft.AspNetCore.Builder.IISOptions> 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="3f193-1059">To configure IIS options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="3f193-1060">下列範例會防止應用程式填入 `HttpContext.Connection.ClientCertificate`：</span><span class="sxs-lookup"><span data-stu-id="3f193-1060">The following example prevents the app from populating `HttpContext.Connection.ClientCertificate`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="3f193-1061">選項</span><span class="sxs-lookup"><span data-stu-id="3f193-1061">Option</span></span>                         | <span data-ttu-id="3f193-1062">預設</span><span class="sxs-lookup"><span data-stu-id="3f193-1062">Default</span></span> | <span data-ttu-id="3f193-1063">設定</span><span class="sxs-lookup"><span data-stu-id="3f193-1063">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="3f193-1064">若為 `true`，[IIS 整合中介軟體](#enable-the-iisintegration-components)會設定由 [Windows 驗證](xref:security/authentication/windowsauth)所驗證的 `HttpContext.User`。</span><span class="sxs-lookup"><span data-stu-id="3f193-1064">If `true`, [IIS Integration Middleware](#enable-the-iisintegration-components) sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="3f193-1065">如果為 `false`，則驗證中介軟體僅針對 `HttpContext.User` 提供身分識別，並在游 `AuthenticationScheme` 提出明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="3f193-1065">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="3f193-1066">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="3f193-1066">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="3f193-1067">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="3f193-1067">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="3f193-1068">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="3f193-1068">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="3f193-1069">如果為 `true` 且 `MS-ASPNETCORE-CLIENTCERT` 要求標頭已存在，則會填入 `HttpContext.Connection.ClientCertificate`。</span><span class="sxs-lookup"><span data-stu-id="3f193-1069">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="3f193-1070">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="3f193-1070">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="3f193-1071">用來設定轉送標頭中介軟體及 ASP.NET Core 模組的 [IIS 整合中介軟體](#enable-the-iisintegration-components)會設定為轉送配置 (HTTP/HTTPS) 與發出要求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="3f193-1071">The [IIS Integration Middleware](#enable-the-iisintegration-components), which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="3f193-1072">其他 Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。</span><span class="sxs-lookup"><span data-stu-id="3f193-1072">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="3f193-1073">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1073">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="3f193-1074">web.config 檔案</span><span class="sxs-lookup"><span data-stu-id="3f193-1074">web.config file</span></span>

<span data-ttu-id="3f193-1075">*web.config* 檔案是用來設定 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1075">The *web.config* file configures the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="3f193-1076">發佈專案時，由 MSBuild 目標 (`_TransformWebConfig`) 處理 *web.config* 檔案的建立、轉換及發佈。</span><span class="sxs-lookup"><span data-stu-id="3f193-1076">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="3f193-1077">此目標存在於 Web SDK 目標 (`Microsoft.NET.Sdk.Web`)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1077">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="3f193-1078">SDK 設定在專案檔的頂端：</span><span class="sxs-lookup"><span data-stu-id="3f193-1078">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="3f193-1079">如果專案中沒有 *web.config* 檔案，則系統會使用正確的 *processPath* 和 *arguments* 建立該檔案以設定 ASP.NET Core 模組，並將該檔案移至[已發行的輸出](xref:host-and-deploy/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1079">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="3f193-1080">如果 *web.config* 檔案存在於專案中，則系統會使用正確的 *processPath* 和 *arguments* 來轉換該檔案以設定 ASP.NET Core 模組，然後將它移至已發行的輸出。</span><span class="sxs-lookup"><span data-stu-id="3f193-1080">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="3f193-1081">轉換不會修改檔案中的 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="3f193-1081">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="3f193-1082">*web.config* 檔案可提供能控制作用中 IIS 模組的額外 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="3f193-1082">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="3f193-1083">如需能處理 ASP.NET Core 應用程式要求之 IIS 模組的相關資訊，請參閱 [IIS 模組](xref:host-and-deploy/iis/modules)主題。</span><span class="sxs-lookup"><span data-stu-id="3f193-1083">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="3f193-1084">為防止 Web SDK 轉換 *web.config* 檔案，請使用專案檔中的 **\<IsTransformWebConfigDisabled>** 屬性：</span><span class="sxs-lookup"><span data-stu-id="3f193-1084">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="3f193-1085">使 Web SDK 無法轉換檔案時，應該由開發人員手動設定 *processPath* 和 *arguments*。</span><span class="sxs-lookup"><span data-stu-id="3f193-1085">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="3f193-1086">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="3f193-1086">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="3f193-1087">web.config 檔案位置</span><span class="sxs-lookup"><span data-stu-id="3f193-1087">web.config file location</span></span>

<span data-ttu-id="3f193-1088">為了正確設定[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module) *，web.config 檔案*必須存在於已部署應用程式的[內容根](xref:fundamentals/index#content-root)路徑（通常是應用程式基底路徑）。</span><span class="sxs-lookup"><span data-stu-id="3f193-1088">In order to set up the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) correctly, the *web.config* file must be present at the [content root](xref:fundamentals/index#content-root) path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="3f193-1089">這是與提供給 IIS 的網站實體路徑相同的位置。</span><span class="sxs-lookup"><span data-stu-id="3f193-1089">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="3f193-1090">應用程式的根目錄需有 *web.config* 檔案，才能使用 Web Deploy 發行多個應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-1090">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="3f193-1091">機密檔案存在於應用程式的實體路徑，例如\* \<元件>. .runtimeconfig.json. json*、 \* \<元件> .xml* （xml 檔批註）和\* \<元件>. .deps.json。\*</span><span class="sxs-lookup"><span data-stu-id="3f193-1091">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="3f193-1092">當 *web.config* 檔案存在且網站正常啟動時，如果有人要求機密檔案，IIS 不會予以提供。</span><span class="sxs-lookup"><span data-stu-id="3f193-1092">When the *web.config* file is present and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="3f193-1093">若 *web.config* 檔案遺失或沒有正確命名，或是無法設定網站以正常啟動，IIS 可能會公開提供機密檔案。</span><span class="sxs-lookup"><span data-stu-id="3f193-1093">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="3f193-1094">\***Web.config*檔案必須隨時存在於部署中、正確命名，而且能夠將網站設定為正常啟動。絕對不要從生產環境部署*移除 web.config 檔案\*。**</span><span class="sxs-lookup"><span data-stu-id="3f193-1094">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

### <a name="transform-webconfig"></a><span data-ttu-id="3f193-1095">轉換 web.config</span><span class="sxs-lookup"><span data-stu-id="3f193-1095">Transform web.config</span></span>

<span data-ttu-id="3f193-1096">如需在發佈時轉換 *web.config* (例如依據設定、設定檔或環境設定環境變數)，請參閱<xref:host-and-deploy/iis/transform-webconfig>。</span><span class="sxs-lookup"><span data-stu-id="3f193-1096">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="3f193-1097">IIS 組態</span><span class="sxs-lookup"><span data-stu-id="3f193-1097">IIS configuration</span></span>

<span data-ttu-id="3f193-1098">**Windows Server 作業系統**</span><span class="sxs-lookup"><span data-stu-id="3f193-1098">**Windows Server operating systems**</span></span>

<span data-ttu-id="3f193-1099">啟用**網頁伺服器 (IIS)** 伺服器角色，並建立角色服務。</span><span class="sxs-lookup"><span data-stu-id="3f193-1099">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="3f193-1100">使用來自 [管理]\*\*\*\* 功能表的 [新增角色及功能]\*\*\*\* 精靈，或是 [伺服器管理員]\*\*\*\* 中的連結。</span><span class="sxs-lookup"><span data-stu-id="3f193-1100">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="3f193-1101">在**伺服器角色**步驟中，核取 [網頁伺服器 (IIS)]\*\*\*\* 方塊。</span><span class="sxs-lookup"><span data-stu-id="3f193-1101">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![在選取伺服器角色步驟中選取網頁伺服器 IIS 角色。](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="3f193-1103">在 [功能]\*\*\*\* 步驟之後，[角色服務]\*\*\*\* 步驟會針對網頁伺服器 (IIS) 進行載入。</span><span class="sxs-lookup"><span data-stu-id="3f193-1103">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="3f193-1104">選取所需的 IIS 角色服務或接受所提供的預設角色服務。</span><span class="sxs-lookup"><span data-stu-id="3f193-1104">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![在選取角色服務步驟中，選取預設的角色服務。](index/_static/role-services-ws2016.png)

   <span data-ttu-id="3f193-1106">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="3f193-1106">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="3f193-1107">若要啟用 Windows 驗證，請展開下列節點： [**網頁伺服器** > **安全性**]。</span><span class="sxs-lookup"><span data-stu-id="3f193-1107">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="3f193-1108">選取 [Windows 驗證]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="3f193-1108">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="3f193-1109">如需詳細資訊，請參閱 [Windows 驗證 \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) 和[設定 Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1109">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="3f193-1110">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="3f193-1110">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="3f193-1111">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="3f193-1111">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="3f193-1112">若要啟用 websocket，請展開下列節點： [**網頁伺服器** > **應用程式開發**]。</span><span class="sxs-lookup"><span data-stu-id="3f193-1112">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="3f193-1113">選取 [WebSocket 通訊協定]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="3f193-1113">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="3f193-1114">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1114">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="3f193-1115">透過**確認**步驟繼續作業，安裝網頁伺服器角色和服務。</span><span class="sxs-lookup"><span data-stu-id="3f193-1115">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="3f193-1116">安裝**網頁伺服器（iis）** 角色之後，不需要重新開機伺服器/iis。</span><span class="sxs-lookup"><span data-stu-id="3f193-1116">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="3f193-1117">**Windows 桌面作業系統**</span><span class="sxs-lookup"><span data-stu-id="3f193-1117">**Windows desktop operating systems**</span></span>

<span data-ttu-id="3f193-1118">啟用 [IIS 管理主控台]\*\*\*\* 和 [World Wide Web 服務]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-1118">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="3f193-1119">瀏覽到 [控制台]\*\* [程式]\*\* > \*\* [程式和功能]\*\* > \*\*\*\* > **[開啟或關閉 Windows 功能]** \(畫面左側\)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1119">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="3f193-1120">開啟 [Internet Information Services]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="3f193-1120">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="3f193-1121">開啟 [Web 管理工具]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="3f193-1121">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="3f193-1122">核取 [IIS 管理主控台]\*\*\*\* 方塊。</span><span class="sxs-lookup"><span data-stu-id="3f193-1122">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="3f193-1123">[World Wide Web Services] (全球資訊網服務)\*\*\*\* 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="3f193-1123">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="3f193-1124">接受**全球資訊網服務**的預設功能，或自訂 IIS 功能。</span><span class="sxs-lookup"><span data-stu-id="3f193-1124">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="3f193-1125">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="3f193-1125">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="3f193-1126">若要啟用 Windows 驗證，請展開下列節點： **World Wide Web 服務** > **安全性**。</span><span class="sxs-lookup"><span data-stu-id="3f193-1126">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="3f193-1127">選取 [Windows 驗證]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="3f193-1127">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="3f193-1128">如需詳細資訊，請參閱 [Windows 驗證 \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) 和[設定 Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1128">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="3f193-1129">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="3f193-1129">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="3f193-1130">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="3f193-1130">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="3f193-1131">若要啟用 websocket，請展開下列節點： [ **World Wide Web 服務** > ] [**應用程式開發] 功能**。</span><span class="sxs-lookup"><span data-stu-id="3f193-1131">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="3f193-1132">選取 [WebSocket 通訊協定]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="3f193-1132">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="3f193-1133">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1133">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="3f193-1134">若 IIS 安裝需要重新啟動，請重新啟動系統。</span><span class="sxs-lookup"><span data-stu-id="3f193-1134">If the IIS installation requires a restart, restart the system.</span></span>

![選取 [Windows 功能] 中的 [IIS 管理主控台] 和 [World Wide Web Services] (全球資訊網服務)。](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="3f193-1136">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="3f193-1136">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="3f193-1137">在主控系統上安裝 .NET Core 裝載套件組合\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-1137">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="3f193-1138">套件組合會安裝 .NET Core 執行時間、.NET Core 程式庫和[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1138">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="3f193-1139">此模組可讓 ASP.NET Core 應用程式在 IIS 背後執行。</span><span class="sxs-lookup"><span data-stu-id="3f193-1139">The module allows ASP.NET Core apps to run behind IIS.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3f193-1140">若裝載套件組合在 IIS 之前安裝，則必須對該套件組合安裝進行修復。</span><span class="sxs-lookup"><span data-stu-id="3f193-1140">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="3f193-1141">請在安裝 IIS 之後，再次執行裝載套件組合安裝程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-1141">Run the Hosting Bundle installer again after installing IIS.</span></span>
>
> <span data-ttu-id="3f193-1142">如果在安裝 64 位元 (x64) 版本的 .NET Core 後才安裝裝載套件組合，那麼可能會遺漏 SDK ([未偵測到 .NET Core SDK](xref:test/troubleshoot#no-net-core-sdks-were-detected))。</span><span class="sxs-lookup"><span data-stu-id="3f193-1142">If the Hosting Bundle is installed after installing the 64-bit (x64) version of .NET Core, SDKs might appear to be missing ([No .NET Core SDKs were detected](xref:test/troubleshoot#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="3f193-1143">若要解決此問題，請參閱 <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>。</span><span class="sxs-lookup"><span data-stu-id="3f193-1143">To resolve the problem, see <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>.</span></span>

### <a name="download"></a><span data-ttu-id="3f193-1144">下載</span><span class="sxs-lookup"><span data-stu-id="3f193-1144">Download</span></span>

1. <span data-ttu-id="3f193-1145">流覽至 [[下載 .Net Core](https://dotnet.microsoft.com/download/dotnet-core) ] 頁面。</span><span class="sxs-lookup"><span data-stu-id="3f193-1145">Navigate to the [Download .NET Core](https://dotnet.microsoft.com/download/dotnet-core) page.</span></span>
1. <span data-ttu-id="3f193-1146">選取所需的 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="3f193-1146">Select the desired .NET Core version.</span></span>
1. <span data-ttu-id="3f193-1147">在 [執行應用程式 - 執行階段]\*\*\*\* 欄中，尋找想要的 .NET Core 執行階段版本列。</span><span class="sxs-lookup"><span data-stu-id="3f193-1147">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="3f193-1148">使用**裝載**套件組合連結來下載安裝程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-1148">Download the installer using the **Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="3f193-1149">某些安裝程式包含已達到期生命週期結束 (EOL) 的發行版本，這些發行版本已不受 Microsoft 支援。</span><span class="sxs-lookup"><span data-stu-id="3f193-1149">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="3f193-1150">如需詳細資訊，請參閱[支援原則](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) \(英文 \)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1150">For more information, see the [support policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="3f193-1151">安裝裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="3f193-1151">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="3f193-1152">在伺服器上執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-1152">Run the installer on the server.</span></span> <span data-ttu-id="3f193-1153">從系統管理員命令殼層執行安裝程式時，有 下列參數可用：</span><span class="sxs-lookup"><span data-stu-id="3f193-1153">The following parameters are available when running the installer from an administrator command shell:</span></span>

   * <span data-ttu-id="3f193-1154">`OPT_NO_ANCM=1`&ndash;略過安裝 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="3f193-1154">`OPT_NO_ANCM=1` &ndash; Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="3f193-1155">`OPT_NO_RUNTIME=1`&ndash;略過安裝 .net Core 執行時間。</span><span class="sxs-lookup"><span data-stu-id="3f193-1155">`OPT_NO_RUNTIME=1` &ndash; Skip installing the .NET Core runtime.</span></span> <span data-ttu-id="3f193-1156">當伺服器只裝載[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)時使用。</span><span class="sxs-lookup"><span data-stu-id="3f193-1156">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="3f193-1157">`OPT_NO_SHAREDFX=1`&ndash;略過安裝 ASP.NET 共用架構（ASP.NET 執行時間）。</span><span class="sxs-lookup"><span data-stu-id="3f193-1157">`OPT_NO_SHAREDFX=1` &ndash; Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span> <span data-ttu-id="3f193-1158">當伺服器只裝載[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)時使用。</span><span class="sxs-lookup"><span data-stu-id="3f193-1158">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="3f193-1159">`OPT_NO_X86=1`&ndash;略過安裝 x86 執行時間。</span><span class="sxs-lookup"><span data-stu-id="3f193-1159">`OPT_NO_X86=1` &ndash; Skip installing x86 runtimes.</span></span> <span data-ttu-id="3f193-1160">當您確定不會裝載 32 位元應用程式時，請使用此參數。</span><span class="sxs-lookup"><span data-stu-id="3f193-1160">Use this parameter when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="3f193-1161">如果將來有可能同時裝載 32 位元和 64 位元應用程式，請不要使用此參數並安裝這兩個執行階段。</span><span class="sxs-lookup"><span data-stu-id="3f193-1161">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this parameter and install both runtimes.</span></span>
   * <span data-ttu-id="3f193-1162">`OPT_NO_SHARED_CONFIG_CHECK=1` &ndash; 停用使用 IIS 共用設定 (當共用設定 (*applicationHost.config*) 位於與 IIS 安裝相同的機器上時) 進行檢查。</span><span class="sxs-lookup"><span data-stu-id="3f193-1162">`OPT_NO_SHARED_CONFIG_CHECK=1` &ndash; Disable the check for using an IIS Shared Configuration when the shared configuration (*applicationHost.config*) is on the same machine as the IIS installation.</span></span> <span data-ttu-id="3f193-1163">*只在 ASP.NET Core 2.2 或更新版本的裝載套件組合安裝程式上可用。*</span><span class="sxs-lookup"><span data-stu-id="3f193-1163">*Only available for ASP.NET Core 2.2 or later Hosting Bundler installers.*</span></span> <span data-ttu-id="3f193-1164">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>。</span><span class="sxs-lookup"><span data-stu-id="3f193-1164">For more information, see <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>.</span></span>
1. <span data-ttu-id="3f193-1165">重新開機系統，或在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="3f193-1165">Restart the system or execute the following commands in a command shell:</span></span>

   ```console
   net stop was /y
   net start w3svc
   ```
   <span data-ttu-id="3f193-1166">重新啟動 IIS 將能偵測到由安裝程式對系統路徑 (此為環境變數) 所做出的變更。</span><span class="sxs-lookup"><span data-stu-id="3f193-1166">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

<span data-ttu-id="3f193-1167">安裝裝載套件組合時，不需要手動停止 IIS 中的個別網站。</span><span class="sxs-lookup"><span data-stu-id="3f193-1167">It isn't necessary to manually stop individual sites in IIS when installing the Hosting Bundle.</span></span> <span data-ttu-id="3f193-1168">裝載的應用程式（IIS 網站）會在 IIS 重新開機時重新開機。</span><span class="sxs-lookup"><span data-stu-id="3f193-1168">Hosted apps (IIS sites) restart when IIS restarts.</span></span> <span data-ttu-id="3f193-1169">應用程式會在收到第一個要求時重新開機，包括從[應用程式初始化模組](#application-initialization-module-and-idle-timeout)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1169">Apps start up again when they receive their first request, including from the [Application Initialization Module](#application-initialization-module-and-idle-timeout).</span></span>

<span data-ttu-id="3f193-1170">ASP.NET Core 採用共用架構封裝修補程式版本的向前復原行為。</span><span class="sxs-lookup"><span data-stu-id="3f193-1170">ASP.NET Core adopts roll-forward behavior for patch releases of shared framework packages.</span></span> <span data-ttu-id="3f193-1171">當 IIS 所裝載的應用程式使用 IIS 重新開機時，應用程式會在收到第一個要求時，以其所參考套件的最新修補程式版本來載入。</span><span class="sxs-lookup"><span data-stu-id="3f193-1171">When apps hosted by IIS restart with IIS, the apps load with the latest patch releases of their referenced packages when they receive their first request.</span></span> <span data-ttu-id="3f193-1172">如果未重新開機 IIS，應用程式會在其工作者進程回收時重新開機並展示向前復原行為，並接收其第一個要求。</span><span class="sxs-lookup"><span data-stu-id="3f193-1172">If IIS isn't restarted, apps restart and exhibit roll-forward behavior when their worker processes are recycled and they receive their first request.</span></span>

> [!NOTE]
> <span data-ttu-id="3f193-1173">如需 IIS 共用組態的資訊，請參閱[使用 IIS 共用組態的 ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1173">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="3f193-1174">使用 Visual Studio 發佈時安裝 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="3f193-1174">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="3f193-1175">將應用程式部署到具有 [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later) 的伺服器時，請在伺服器上安裝最新版的 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="3f193-1175">When deploying apps to servers with [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="3f193-1176">若要安裝 Web Deploy，請使用 [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) 或從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=43717)直接取得安裝程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-1176">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="3f193-1177">慣用的方法是使用 WebPI。</span><span class="sxs-lookup"><span data-stu-id="3f193-1177">The preferred method is to use WebPI.</span></span> <span data-ttu-id="3f193-1178">WebPI 提供獨立的安裝程式和組態以裝載提供者。</span><span class="sxs-lookup"><span data-stu-id="3f193-1178">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="3f193-1179">建立 IIS 網站</span><span class="sxs-lookup"><span data-stu-id="3f193-1179">Create the IIS site</span></span>

1. <span data-ttu-id="3f193-1180">在主控系統上，請建立資料夾以容納應用程式的已發行資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="3f193-1180">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="3f193-1181">在下列步驟中，您提供資料夾路徑給 IIS，作為應用程式的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="3f193-1181">In a following step, the folder's path is provided to IIS as the physical path to the app.</span></span> <span data-ttu-id="3f193-1182">如需應用程式之部署資料夾和檔案配置的詳細資訊，請參閱 <xref:host-and-deploy/directory-structure>。</span><span class="sxs-lookup"><span data-stu-id="3f193-1182">For more information on an app's deployment folder and file layout, see <xref:host-and-deploy/directory-structure>.</span></span>

1. <span data-ttu-id="3f193-1183">在 [IIS 管理員] 中 **，在 [** 連線] 面板中開啟伺服器的節點。</span><span class="sxs-lookup"><span data-stu-id="3f193-1183">In IIS Manager, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="3f193-1184">以滑鼠右鍵按一下 [網站]\*\*\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3f193-1184">Right-click the **Sites** folder.</span></span> <span data-ttu-id="3f193-1185">從操作功能表選取 [新增網站]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-1185">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="3f193-1186">提供**網站名稱**，並將**實體路徑**設定為應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="3f193-1186">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="3f193-1187">藉由**Binding**選取 **[確定]** 來提供系結設定並建立網站：</span><span class="sxs-lookup"><span data-stu-id="3f193-1187">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![在新增網站步驟中提供站台名稱、實體路徑和主機名稱。](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="3f193-1189">請**勿**使用最上層萬用字元繫結 (`http://*:80/`與 `http://+:80`)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1189">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="3f193-1190">最上層萬用字元繫結可能暴露您的應用程式安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="3f193-1190">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="3f193-1191">這對強式與弱式萬用字元皆適用。</span><span class="sxs-lookup"><span data-stu-id="3f193-1191">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="3f193-1192">請使用明確主機名稱，而非萬用字元。</span><span class="sxs-lookup"><span data-stu-id="3f193-1192">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="3f193-1193">若您擁有整個父網域 (與具弱點的 `*.com` 相對) 的控制權，則子網域萬用字元繫結 (例如 `*.mysub.com`) 就沒有此安全性風險。</span><span class="sxs-lookup"><span data-stu-id="3f193-1193">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="3f193-1194">如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1194">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="3f193-1195">在伺服器的節點之下，選取 [應用程式集區]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-1195">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="3f193-1196">以滑鼠右鍵按一下網站的應用程式集區，然後從操作功能表選取 [基本設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-1196">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="3f193-1197">在 [編輯應用程式集區]\*\*\*\* 視窗中，將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\*：</span><span class="sxs-lookup"><span data-stu-id="3f193-1197">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![將 .NET CLR 版本設為 [沒有受控碼]。](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="3f193-1199">ASP.NET Core 會在不同的處理序中執行，並管理執行階段。</span><span class="sxs-lookup"><span data-stu-id="3f193-1199">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="3f193-1200">ASP.NET Core 不仰賴載入桌面 CLR (.NET CLR)&mdash;會使用 .NET Core 的核心通用語言執行平台 (CoreCLR) 來開機以在背景工作處理序中裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-1200">ASP.NET Core doesn't rely on loading the desktop CLR (.NET CLR)&mdash;the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process.</span></span> <span data-ttu-id="3f193-1201">將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\* 是選擇性的，但建議這樣做。</span><span class="sxs-lookup"><span data-stu-id="3f193-1201">Setting the **.NET CLR version** to **No Managed Code** is optional but recommended.</span></span>

1. <span data-ttu-id="3f193-1202">*ASP.NET Core 2.2 或更新版本*：對於使用[同處理序主控模型](#in-process-hosting-model)的 64 位元 (x64) [獨立式部署](/dotnet/core/deploying/#self-contained-deployments-scd)，會停用 32 位元 (x86) 處理序的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="3f193-1202">*ASP.NET Core 2.2 or later*: For a 64-bit (x64) [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) that uses the [in-process hosting model](#in-process-hosting-model), disable the app pool for 32-bit (x86) processes.</span></span>

   <span data-ttu-id="3f193-1203">在 IIS 管理員的 [動作]\*\*\*\* 資訊看板 > [應用程式集區]\*\*\*\* 中，選取 [設定應用程式集區預設值]\*\*\*\* 或 [進階設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-1203">In the **Actions** sidebar of IIS Manager > **Application Pools**, select **Set Application Pool Defaults** or **Advanced Settings**.</span></span> <span data-ttu-id="3f193-1204">找到 [啟用 32 位元應用程式]\*\*\*\*，然後將其值設定為 `False`。</span><span class="sxs-lookup"><span data-stu-id="3f193-1204">Locate **Enable 32-Bit Applications** and set the value to `False`.</span></span> <span data-ttu-id="3f193-1205">此設定不會影響為[處理程序外裝載](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model)部署的應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-1205">This setting doesn't affect apps deployed for [out-of-process hosting](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).</span></span>

1. <span data-ttu-id="3f193-1206">確認處理序模型身分識別具有適當的權限。</span><span class="sxs-lookup"><span data-stu-id="3f193-1206">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="3f193-1207">如果應用程式集區的預設識別（**進程模型** > **Identity**）從**ApplicationPoolIdentity**變更為另一個身分識別，請確認新的身分識別具有存取應用程式資料夾、資料庫和其他必要資源的必要許可權。</span><span class="sxs-lookup"><span data-stu-id="3f193-1207">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="3f193-1208">例如，應用程式集區需要針對應用程式讀取和寫入檔案的資料夾取得讀取和寫入權限。</span><span class="sxs-lookup"><span data-stu-id="3f193-1208">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="3f193-1209">**Windows 驗證設定 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="3f193-1209">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="3f193-1210">如需詳細資訊，請參閱[設定 Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="3f193-1210">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="3f193-1211">部署應用程式</span><span class="sxs-lookup"><span data-stu-id="3f193-1211">Deploy the app</span></span>

<span data-ttu-id="3f193-1212">將應用程式部署至在[建立 IIS 網站](#create-the-iis-site)一節中所建立的 IIS **實體路徑**資料夾。</span><span class="sxs-lookup"><span data-stu-id="3f193-1212">Deploy the app to the IIS **Physical path** folder that was established in the [Create the IIS site](#create-the-iis-site) section.</span></span> <span data-ttu-id="3f193-1213">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 是建議的部署機制，但有數個選項可將應用程式從專案的 [publish]\*\* 資料夾移動至主機系統的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="3f193-1213">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment, but several options exist for moving the app from the project's *publish* folder to the hosting system's deployment folder.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="3f193-1214">使用 Visual Studio 的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="3f193-1214">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="3f193-1215">請參閱[適用於 ASP.NET Core 應用程式部署的 Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)主題，了解如何建立搭配 Web Deploy 使用的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="3f193-1215">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="3f193-1216">如果主機服務提供者提供或支援建立發行設定檔，請下載其設定檔，並使用 Visual Studio 的 [發行]\*\*\*\* 對話方塊匯入其設定檔：</span><span class="sxs-lookup"><span data-stu-id="3f193-1216">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog:</span></span>

![[發佈] 對話方塊頁](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="3f193-1218">Visual Studio 外部的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="3f193-1218">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="3f193-1219">透過命令列也可以在 Visual Studio 外部使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1219">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="3f193-1220">如需詳細資訊，請參閱 [Web 部署工具](/iis/publish/using-web-deploy/use-the-web-deployment-tool)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1220">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="3f193-1221">Web Deploy 的替代項目</span><span class="sxs-lookup"><span data-stu-id="3f193-1221">Alternatives to Web Deploy</span></span>

<span data-ttu-id="3f193-1222">使用數種方法的其中一種將應用程式移到主機系統，例如手動複製、[Xcopy](/windows-server/administration/windows-commands/xcopy)、[Robocopy](/windows-server/administration/windows-commands/robocopy) 或 [PowerShell](/powershell/)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1222">Use any of several methods to move the app to the hosting system, such as manual copy, [Xcopy](/windows-server/administration/windows-commands/xcopy), [Robocopy](/windows-server/administration/windows-commands/robocopy), or [PowerShell](/powershell/).</span></span>

<span data-ttu-id="3f193-1223">如需 IIS 的 ASP.NET Core 部署詳細資訊，請參閱 [IIS 系統管理員的部署資源](#deployment-resources-for-iis-administrators)一節。</span><span class="sxs-lookup"><span data-stu-id="3f193-1223">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="3f193-1224">瀏覽網站</span><span class="sxs-lookup"><span data-stu-id="3f193-1224">Browse the website</span></span>

<span data-ttu-id="3f193-1225">應用程式部署到主機系統之後，請向其中一個應用程式的公用端點發出要求。</span><span class="sxs-lookup"><span data-stu-id="3f193-1225">After the app is deployed to the hosting system, make a request to one of the app's public endpoints.</span></span>

<span data-ttu-id="3f193-1226">在下列範例中，網站繫結至 IIS 上的**連接埠** `80`，其**主機名稱**為 `www.mysite.com`。</span><span class="sxs-lookup"><span data-stu-id="3f193-1226">In the following example, the site is bound to an IIS **Host name** of `www.mysite.com` on **Port** `80`.</span></span> <span data-ttu-id="3f193-1227">對 `http://www.mysite.com` 發出要求：</span><span class="sxs-lookup"><span data-stu-id="3f193-1227">A request is made to `http://www.mysite.com`:</span></span>

![Microsoft Edge 瀏覽器已載入 IIS 啟動頁面。](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="3f193-1229">已鎖定的部署檔案</span><span class="sxs-lookup"><span data-stu-id="3f193-1229">Locked deployment files</span></span>

<span data-ttu-id="3f193-1230">當應用程式執行時，會鎖定部署資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="3f193-1230">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="3f193-1231">無法於部署期間覆寫已鎖定的檔案。</span><span class="sxs-lookup"><span data-stu-id="3f193-1231">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="3f193-1232">若要釋放部署中的已鎖定檔案，請使用下列其中**一種**方法停止應用程式集區：</span><span class="sxs-lookup"><span data-stu-id="3f193-1232">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="3f193-1233">使用 Web Deploy 並參考專案檔中的 `Microsoft.NET.Sdk.Web`。</span><span class="sxs-lookup"><span data-stu-id="3f193-1233">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="3f193-1234">*app_offline.htm* 檔案是放在 Web 應用程式目錄的根目錄中。</span><span class="sxs-lookup"><span data-stu-id="3f193-1234">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="3f193-1235">當檔案存在時，ASP.NET Core 模組會正常關閉應用程式，並在部署期間提供 *app_offline.htm* 檔案。</span><span class="sxs-lookup"><span data-stu-id="3f193-1235">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="3f193-1236">如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1236">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="3f193-1237">在伺服器上的 IIS 管理員中手動停止應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="3f193-1237">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="3f193-1238">使用 PowerShell 卸載*app_offline .htm* （需要 PowerShell 5 或更新版本）：</span><span class="sxs-lookup"><span data-stu-id="3f193-1238">Use PowerShell to drop *app_offline.htm* (requires PowerShell 5 or later):</span></span>

  ```powershell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a><span data-ttu-id="3f193-1239">資料保護</span><span class="sxs-lookup"><span data-stu-id="3f193-1239">Data protection</span></span>

<span data-ttu-id="3f193-1240">[ASP.NET Core 資料保護堆疊](xref:security/data-protection/introduction)是由數個 ASP.NET Core [中介軟體](xref:fundamentals/middleware/index)所使用，包括用於驗證的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="3f193-1240">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="3f193-1241">即使資料保護 API 不是由使用者程式碼呼叫，仍應使用部署指令碼或是在使用者程式碼中設定資料保護，以建立持續性的密碼編譯[金鑰存放區](xref:security/data-protection/implementation/key-management)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1241">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="3f193-1242">如不設定資料保護，金鑰會保留在記憶體中，並於應用程式重新啟動時捨棄。</span><span class="sxs-lookup"><span data-stu-id="3f193-1242">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="3f193-1243">如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：</span><span class="sxs-lookup"><span data-stu-id="3f193-1243">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="3f193-1244">所有以 Cookie 為基礎的驗證權杖都會失效。</span><span class="sxs-lookup"><span data-stu-id="3f193-1244">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="3f193-1245">當使用者提出下一個要求時，需要再次登入。</span><span class="sxs-lookup"><span data-stu-id="3f193-1245">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="3f193-1246">所有以 Keyring 保護的資料都無法再解密。</span><span class="sxs-lookup"><span data-stu-id="3f193-1246">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="3f193-1247">這可能會包含 [CSRF 權杖](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)和 [ASP.NET Core MVC TempData cookie](xref:fundamentals/app-state#tempdata)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1247">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="3f193-1248">若要在 IIS 下設定資料保護以保存 Keyring，請使用下列其中**一種**方法：</span><span class="sxs-lookup"><span data-stu-id="3f193-1248">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="3f193-1249">**建立資料保護登錄機碼**</span><span class="sxs-lookup"><span data-stu-id="3f193-1249">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="3f193-1250">ASP.NET Core 應用程式所使用的資料保護金鑰會儲存在應用程式外部的登錄中。</span><span class="sxs-lookup"><span data-stu-id="3f193-1250">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="3f193-1251">若要保存指定應用程式的金鑰，請為應用程式集區建立登錄機碼。</span><span class="sxs-lookup"><span data-stu-id="3f193-1251">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="3f193-1252">若為獨立的非Web 伺服陣列 IIS 安裝，請針對搭配使用 ASP.NET Core 應用程式的每個應用程式集區，使用[資料保護 Provision-AutoGenKeys.ps1 PowerShell 指令碼](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1252">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="3f193-1253">此指令碼會在 HKLM 登錄中建立登錄機碼，只有應用程式之應用程式集區的背景工作處理序帳戶可以存取它。</span><span class="sxs-lookup"><span data-stu-id="3f193-1253">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="3f193-1254">在待用期間使用 DPAPI 和全電腦金鑰加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="3f193-1254">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="3f193-1255">在 Web 伺服陣列案例中，應用程式可以設定成使用 UNC 路徑來儲存其資料保護 Keyring。</span><span class="sxs-lookup"><span data-stu-id="3f193-1255">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="3f193-1256">根據預設，資料保護金鑰不予加密。</span><span class="sxs-lookup"><span data-stu-id="3f193-1256">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="3f193-1257">請確保網路共用的檔案權限僅限於執行應用程式的 Windows 帳戶。</span><span class="sxs-lookup"><span data-stu-id="3f193-1257">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="3f193-1258">可以使用 X509 憑證來保護待用的金鑰。</span><span class="sxs-lookup"><span data-stu-id="3f193-1258">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="3f193-1259">請考慮下列讓使用者上傳憑證的機制：將憑證放入使用者的受信任憑證存放區，並確保在執行使用者應用程式的所有電腦上都能使用這些憑證。</span><span class="sxs-lookup"><span data-stu-id="3f193-1259">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="3f193-1260">如需詳細資訊，請參閱[設定 ASP.NET Core 資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1260">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="3f193-1261">**設定 IIS 應用程式集區載入使用者設定檔**</span><span class="sxs-lookup"><span data-stu-id="3f193-1261">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="3f193-1262">此設定位在應用程式集區 [進階設定]\*\*\*\* 下的 [處理序模型]\*\*\*\* 區段中。</span><span class="sxs-lookup"><span data-stu-id="3f193-1262">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="3f193-1263">將 [載入使用者設定檔]\*\*\*\* 設為 `True`。</span><span class="sxs-lookup"><span data-stu-id="3f193-1263">Set **Load User Profile** to `True`.</span></span> <span data-ttu-id="3f193-1264">當設定為 `True` 時，金鑰會儲存在使用者設定檔目錄中，且使用具有使用者帳戶專屬金鑰的 DPAPI 保護。</span><span class="sxs-lookup"><span data-stu-id="3f193-1264">When set to `True`, keys are stored in the user profile directory and protected using DPAPI with a key specific to the user account.</span></span> <span data-ttu-id="3f193-1265">金鑰會保存到 *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3f193-1265">Keys are persisted to the *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* folder.</span></span>

  <span data-ttu-id="3f193-1266">應用程式集區的 [setProfileEnvironment 屬性](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration)也必須啟用。</span><span class="sxs-lookup"><span data-stu-id="3f193-1266">The app pool's [setProfileEnvironment attribute](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) must also be enabled.</span></span> <span data-ttu-id="3f193-1267">`setProfileEnvironment` 的預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="3f193-1267">The default value of `setProfileEnvironment` is `true`.</span></span> <span data-ttu-id="3f193-1268">在某些情況下 (例如 Windows OS)，`setProfileEnvironment` 會設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="3f193-1268">In some scenarios (for example, Windows OS), `setProfileEnvironment` is set to `false`.</span></span> <span data-ttu-id="3f193-1269">如果金鑰並未如預期地儲存在使用者設定檔目錄中：</span><span class="sxs-lookup"><span data-stu-id="3f193-1269">If keys aren't stored in the user profile directory as expected:</span></span>

  1. <span data-ttu-id="3f193-1270">瀏覽至 *%windir%/system32/inetsrv/config* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3f193-1270">Navigate to the *%windir%/system32/inetsrv/config* folder.</span></span>
  1. <span data-ttu-id="3f193-1271">開啟 *applicationHost.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="3f193-1271">Open the *applicationHost.config* file.</span></span>
  1. <span data-ttu-id="3f193-1272">找出 `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` 元素。</span><span class="sxs-lookup"><span data-stu-id="3f193-1272">Locate the `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` element.</span></span>
  1. <span data-ttu-id="3f193-1273">確認 `setProfileEnvironment` 屬性不存在 (其預設值為 `true`)，或明確地將屬性值設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="3f193-1273">Confirm that the `setProfileEnvironment` attribute isn't present, which defaults the value to `true`, or explicitly set the attribute's value to `true`.</span></span>

* <span data-ttu-id="3f193-1274">**將檔案系統當作 Keyring 存放區使用**</span><span class="sxs-lookup"><span data-stu-id="3f193-1274">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="3f193-1275">調整應用程式程式碼，以[將檔案系統當作 Keyring 存放區使用](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1275">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="3f193-1276">使用 X509 憑證來保護 Keyring，並確保憑證是受信任的憑證。</span><span class="sxs-lookup"><span data-stu-id="3f193-1276">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="3f193-1277">如果憑證為自我簽署，請將憑證放在受信任的根存放區。</span><span class="sxs-lookup"><span data-stu-id="3f193-1277">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="3f193-1278">在 Web 伺服陣列中使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="3f193-1278">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="3f193-1279">請使用所有電腦都可以存取的檔案共用。</span><span class="sxs-lookup"><span data-stu-id="3f193-1279">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="3f193-1280">將 X509 憑證部署到每一部電腦。</span><span class="sxs-lookup"><span data-stu-id="3f193-1280">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="3f193-1281">設定[程式碼中的資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1281">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="3f193-1282">**設定資料保護的全電腦原則**</span><span class="sxs-lookup"><span data-stu-id="3f193-1282">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="3f193-1283">針對取用資料保護 API 的所有應用程式，資料保護系統僅支援有限的預設[全電腦原則](xref:security/data-protection/configuration/machine-wide-policy)設定。</span><span class="sxs-lookup"><span data-stu-id="3f193-1283">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="3f193-1284">如需詳細資訊，請參閱<xref:security/data-protection/introduction>。</span><span class="sxs-lookup"><span data-stu-id="3f193-1284">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="virtual-directories"></a><span data-ttu-id="3f193-1285">虛擬目錄</span><span class="sxs-lookup"><span data-stu-id="3f193-1285">Virtual Directories</span></span>

<span data-ttu-id="3f193-1286">ASP.NET Core 應用程式不支援 [IIS 虛擬目錄](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1286">[IIS Virtual Directories](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) aren't supported with ASP.NET Core apps.</span></span> <span data-ttu-id="3f193-1287">應用程式能以[子應用程式](#sub-applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="3f193-1287">An app can be hosted as a [sub-application](#sub-applications).</span></span>

## <a name="sub-applications"></a><span data-ttu-id="3f193-1288">子應用程式</span><span class="sxs-lookup"><span data-stu-id="3f193-1288">Sub-applications</span></span>

<span data-ttu-id="3f193-1289">ASP.NET Core 應用程式能以 [IIS 子應用程式](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="3f193-1289">An ASP.NET Core app can be hosted as an [IIS sub-application (sub-app)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications).</span></span> <span data-ttu-id="3f193-1290">子應用程式的路徑會成為根應用程式 URL 的一部分。</span><span class="sxs-lookup"><span data-stu-id="3f193-1290">The sub-app's path becomes part of the root app's URL.</span></span>

<span data-ttu-id="3f193-1291">子應用程式不應該包括 ASP.NET Core 模組做為處理常式。</span><span class="sxs-lookup"><span data-stu-id="3f193-1291">A sub-app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="3f193-1292">如果您在子應用程式的 *web.config* 檔案中將模組新增為處理常式，則嘗試瀏覽子應用程式時，會收到參考錯誤設定檔的 *500.19 內部伺服器錯誤*。</span><span class="sxs-lookup"><span data-stu-id="3f193-1292">If the module is added as a handler in a sub-app's *web.config* file, a *500.19 Internal Server Error* referencing the faulty config file is received when attempting to browse the sub-app.</span></span>

<span data-ttu-id="3f193-1293">下列範例顯示 ASP.NET Core 子應用程式的已發佈 *web.config* 檔案：</span><span class="sxs-lookup"><span data-stu-id="3f193-1293">The following example shows a published *web.config* file for an ASP.NET Core sub-app:</span></span>

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

<span data-ttu-id="3f193-1294">在 ASP.NET Core 應用程式下裝載非 ASP.NET Core 子應用程式時，請明確移除子應用程式 *web.config* 檔案中已繼承的處理常式：</span><span class="sxs-lookup"><span data-stu-id="3f193-1294">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app's *web.config* file:</span></span>

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

<span data-ttu-id="3f193-1295">子應用程式內的靜態資產連結應該使用波狀符號與斜線 (`~/`) 標記法。</span><span class="sxs-lookup"><span data-stu-id="3f193-1295">Static asset links within the sub-app should use tilde-slash (`~/`) notation.</span></span> <span data-ttu-id="3f193-1296">波狀符號與斜線標記法會觸發[標記協助程式](xref:mvc/views/tag-helpers/intro)以將子應用程式的路徑基底附加到轉譯的相對連結前面。</span><span class="sxs-lookup"><span data-stu-id="3f193-1296">Tilde-slash notation triggers a [Tag Helper](xref:mvc/views/tag-helpers/intro) to prepend the sub-app's pathbase to the rendered relative link.</span></span> <span data-ttu-id="3f193-1297">針對位於 `/subapp_path` 的子應用程式，使用 `src="~/image.png"` 連結的影像會轉譯為 `src="/subapp_path/image.png"`。</span><span class="sxs-lookup"><span data-stu-id="3f193-1297">For a sub-app at `/subapp_path`, an image linked with `src="~/image.png"` is rendered as `src="/subapp_path/image.png"`.</span></span> <span data-ttu-id="3f193-1298">根應用程式的靜態檔案中介軟體不會處理靜態檔案要求。</span><span class="sxs-lookup"><span data-stu-id="3f193-1298">The root app's Static File Middleware doesn't process the static file request.</span></span> <span data-ttu-id="3f193-1299">要求會由子應用程式的靜態檔案中介軟體處理。</span><span class="sxs-lookup"><span data-stu-id="3f193-1299">The request is processed by the sub-app's Static File Middleware.</span></span>

<span data-ttu-id="3f193-1300">若靜態資產的 `src` 屬性是設定為絕對路徑 (例如，`src="/image.png"`)，會以不使用子應用程式路徑基底的方式轉譯連結。</span><span class="sxs-lookup"><span data-stu-id="3f193-1300">If a static asset's `src` attribute is set to an absolute path (for example, `src="/image.png"`), the link is rendered without the sub-app's pathbase.</span></span> <span data-ttu-id="3f193-1301">根應用程式的靜態檔案中介軟體會嘗試從根應用程式的 [webroot](xref:fundamentals/index#web-root) 提供資產，這會導致「404 - 找不到」\*\* 回應 (除非根應用程式可存取靜態資產)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1301">The root app's Static File Middleware attempts to serve the asset from the root app's [web root](xref:fundamentals/index#web-root), which results in a *404 - Not Found* response unless the static asset is available from the root app.</span></span>

<span data-ttu-id="3f193-1302">裝載 ASP.NET Core 應用程式做為另一個 ASP.NET Core 應用程式下的子應用程式：</span><span class="sxs-lookup"><span data-stu-id="3f193-1302">To host an ASP.NET Core app as a sub-app under another ASP.NET Core app:</span></span>

1. <span data-ttu-id="3f193-1303">為子應用程式建立應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="3f193-1303">Establish an app pool for the sub-app.</span></span> <span data-ttu-id="3f193-1304">將 [.NET CLR 版本]\*\*\*\* 設定為 [沒有受控碼]\*\*\*\*，因為會將核心通用語言執行平台 (CoreCLR) 開機以在背景工作處理序中裝載應用程式，而非在桌面 CLR (.NET CLR) 中裝載。</span><span class="sxs-lookup"><span data-stu-id="3f193-1304">Set the **.NET CLR Version** to **No Managed Code** because the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process, not the desktop CLR (.NET CLR).</span></span>

1. <span data-ttu-id="3f193-1305">使用根網站下資料夾中的子應用程式在 IIS 管理員中新增根網站。</span><span class="sxs-lookup"><span data-stu-id="3f193-1305">Add the root site in IIS Manager with the sub-app in a folder under the root site.</span></span>

1. <span data-ttu-id="3f193-1306">以滑鼠右鍵按一下 IIS 管理員中的子應用程式資料夾，然後選取 [轉換成應用程式]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-1306">Right-click the sub-app folder in IIS Manager and select **Convert to Application**.</span></span>

1. <span data-ttu-id="3f193-1307">在 [新增應用程式]\*\*\*\* 對話方塊中，使用 [應用程式集區]\*\*\*\* 的[選取]\*\*\*\* 按鈕來指派您為子應用程式建立的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="3f193-1307">In the **Add Application** dialog, use the **Select** button for the **Application Pool** to assign the app pool that you created for the sub-app.</span></span> <span data-ttu-id="3f193-1308">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="3f193-1308">Select **OK**.</span></span>

<span data-ttu-id="3f193-1309">將不同的應用程式集區指派給子應用程式是使用同處理序裝載模型。</span><span class="sxs-lookup"><span data-stu-id="3f193-1309">The assignment of a separate app pool to the sub-app is a requirement when using the in-process hosting model.</span></span>

<span data-ttu-id="3f193-1310">如需有關同進程裝載模型和設定 ASP.NET Core 模組的詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="3f193-1310">For more information on the in-process hosting model and configuring the ASP.NET Core Module, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="3f193-1311">使用 web.config 的 IIS 組態</span><span class="sxs-lookup"><span data-stu-id="3f193-1311">Configuration of IIS with web.config</span></span>

<span data-ttu-id="3f193-1312">在對使用了 ASP.NET Core 模組的 ASP.NET Core 有作用的 IIS 情境下，設定會受 *web.config* 的 `<system.webServer>` 區段影響。</span><span class="sxs-lookup"><span data-stu-id="3f193-1312">IIS configuration is influenced by the `<system.webServer>` section of *web.config* for IIS scenarios that are functional for ASP.NET Core apps with the ASP.NET Core Module.</span></span> <span data-ttu-id="3f193-1313">舉例來說，IIS 設定對動態壓縮有作用。</span><span class="sxs-lookup"><span data-stu-id="3f193-1313">For example, IIS configuration is functional for dynamic compression.</span></span> <span data-ttu-id="3f193-1314">如果在伺服器層級將 IIS 設為使用動態壓縮，應用程式 *web.config* 檔案中的 `<urlCompression>` 元素則可為 ASP.NET Core 應用程式予以停用。</span><span class="sxs-lookup"><span data-stu-id="3f193-1314">If IIS is configured at the server level to use dynamic compression, the `<urlCompression>` element in the app's *web.config* file can disable it for an ASP.NET Core app.</span></span>

<span data-ttu-id="3f193-1315">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="3f193-1315">For more information, see the following topics:</span></span>

* [<span data-ttu-id="3f193-1316">System.webserver>的\<設定參考</span><span class="sxs-lookup"><span data-stu-id="3f193-1316">Configuration reference for \<system.webServer></span></span>](/iis/configuration/system.webServer/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>

<span data-ttu-id="3f193-1317">若要設定在隔離的應用程式集區中執行之個別應用程式的環境變數 (支援 IIS 10.0 或更新版本)，請參閱 IIS 參考文件之[環境變數 \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的 *AppCmd.exe 命令*一節。</span><span class="sxs-lookup"><span data-stu-id="3f193-1317">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="3f193-1318">web.config 的組態區段</span><span class="sxs-lookup"><span data-stu-id="3f193-1318">Configuration sections of web.config</span></span>

<span data-ttu-id="3f193-1319">ASP.NET Core 應用程式的設定不使用 *web.config* 中 ASP.NET 4.x 應用程式的設定區段：</span><span class="sxs-lookup"><span data-stu-id="3f193-1319">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

<span data-ttu-id="3f193-1320">使用其他組態提供者設定的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-1320">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="3f193-1321">如需詳細資訊，請參閱[Configuration](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1321">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="3f193-1322">應用程式集區</span><span class="sxs-lookup"><span data-stu-id="3f193-1322">Application Pools</span></span>

<span data-ttu-id="3f193-1323">在伺服器上裝載多個網站時，建議您在其各自的應用程式集區中執行各個應用程式，讓應用程式彼此隔離。</span><span class="sxs-lookup"><span data-stu-id="3f193-1323">When hosting multiple websites on a server, we recommend isolating the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="3f193-1324">IIS [新增網站]\*\*\*\* 對話方塊預設成此組態。</span><span class="sxs-lookup"><span data-stu-id="3f193-1324">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="3f193-1325">當提供**網站名稱**時，文字會自動轉移至 [應用程式集區]\*\*\*\* 文字方塊。</span><span class="sxs-lookup"><span data-stu-id="3f193-1325">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="3f193-1326">新增網站時，會使用該網站名稱建立新的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="3f193-1326">A new app pool is created using the site name when the site is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="3f193-1327">應用程式集區Identity</span><span class="sxs-lookup"><span data-stu-id="3f193-1327">Application Pool Identity</span></span>

<span data-ttu-id="3f193-1328">應用程式集區身分識別帳戶可讓應用程式在唯一的帳戶下執行，不必建立及管理網域或本機帳戶。</span><span class="sxs-lookup"><span data-stu-id="3f193-1328">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="3f193-1329">在 IIS 8.0 或更新版本中，IIS 管理背景工作處理序 (WAS) 會使用新的應用程式集區名稱建立虛擬帳戶，並預設在此帳戶下執行應用程式集區的背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="3f193-1329">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="3f193-1330">在 IIS 管理主控台中，于應用程式集區的 [**高級設定**] **Identity** 底下，確定已設定為使用**ApplicationPoolIdentity**：</span><span class="sxs-lookup"><span data-stu-id="3f193-1330">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![應用程式集區進階設定對話方塊](index/_static/apppool-identity.png)

<span data-ttu-id="3f193-1332">IIS 管理程序會在 Windows 安全系統中，以應用程式集區的名稱建立安全識別碼。</span><span class="sxs-lookup"><span data-stu-id="3f193-1332">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="3f193-1333">可使用此身分識別來保護資源。</span><span class="sxs-lookup"><span data-stu-id="3f193-1333">Resources can be secured using this identity.</span></span> <span data-ttu-id="3f193-1334">不過，此身分識別不是真正的使用者帳戶，也不會顯示在 Windows 使用者管理主控台中。</span><span class="sxs-lookup"><span data-stu-id="3f193-1334">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="3f193-1335">如果 IIS 背景工作處理序需要提升應用程式的存取權限，請修改包含應用程式的目錄存取控制清單 (ACL)：</span><span class="sxs-lookup"><span data-stu-id="3f193-1335">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="3f193-1336">開啟 Windows 檔案總管，巡覽至目錄。</span><span class="sxs-lookup"><span data-stu-id="3f193-1336">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="3f193-1337">以滑鼠右鍵按一下目錄並選取 [屬性]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3f193-1337">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="3f193-1338">依序選取 [安全性]\*\*\*\* 索引標籤下的 [編輯]\*\*\*\* 按鈕和 [新增]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="3f193-1338">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="3f193-1339">選取 [位置]\*\*\*\* 按鈕，並確定選取系統。</span><span class="sxs-lookup"><span data-stu-id="3f193-1339">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="3f193-1340">在 [輸入要選取的物件名稱]\*\*\*\* 區域中，輸入 **IIS AppPool\\<app_pool_name>**。</span><span class="sxs-lookup"><span data-stu-id="3f193-1340">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="3f193-1341">選取 [檢查名稱]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="3f193-1341">Select the **Check Names** button.</span></span> <span data-ttu-id="3f193-1342">針對 [DefaultAppPool]\*\*，請使用 **IIS AppPool\DefaultAppPool** 檢查名稱。</span><span class="sxs-lookup"><span data-stu-id="3f193-1342">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="3f193-1343">選取 [檢查名稱]\*\*\*\* 按鈕時，[DefaultAppPool]\*\*\*\* 的值便會顯示於物件名稱區域中。</span><span class="sxs-lookup"><span data-stu-id="3f193-1343">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="3f193-1344">您無法直接將應用程式集區名稱輸入至物件名稱區域。</span><span class="sxs-lookup"><span data-stu-id="3f193-1344">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="3f193-1345">檢查物件名稱時，請使用 **IIS AppPool\\<app_pool_name>** 的格式。</span><span class="sxs-lookup"><span data-stu-id="3f193-1345">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：在選取 [檢查名稱] 之前，"DefaultAppPool" 這個應用程式集區名稱在物件名稱區域中會附加至 "IIS AppPool\"。](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="3f193-1347">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="3f193-1347">Select **OK**.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：選取 [檢查名稱] 之後，物件名稱 "DefaultAppPool" 會顯示在物件名稱區域中。](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="3f193-1349">預設應該會授與讀取 &amp; 執行權限。</span><span class="sxs-lookup"><span data-stu-id="3f193-1349">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="3f193-1350">請視需要提供其他權限。</span><span class="sxs-lookup"><span data-stu-id="3f193-1350">Provide additional permissions as needed.</span></span>

<span data-ttu-id="3f193-1351">也可使用 **ICACLS** 工具透過命令提示字元授與存取權限。</span><span class="sxs-lookup"><span data-stu-id="3f193-1351">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="3f193-1352">使用 *DefaultAppPool*作為範例，將會使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="3f193-1352">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="3f193-1353">如需詳細資訊，請參閱 [icacls](/windows-server/administration/windows-commands/icacls) 主題。</span><span class="sxs-lookup"><span data-stu-id="3f193-1353">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="http2-support"></a><span data-ttu-id="3f193-1354">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="3f193-1354">HTTP/2 support</span></span>

<span data-ttu-id="3f193-1355">[HTTP/2](https://httpwg.org/specs/rfc7540.html) 支援符合下列基本需求的跨處理序部署：</span><span class="sxs-lookup"><span data-stu-id="3f193-1355">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported for out-of-process deployments that meet the following base requirements:</span></span>

* <span data-ttu-id="3f193-1356">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="3f193-1356">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
* <span data-ttu-id="3f193-1357">公開 Edge Server 連線使用 HTTP/2，但是對 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的反向 Proxy 連線使用 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="3f193-1357">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
* <span data-ttu-id="3f193-1358">目標 Framework：不適用於跨處理序部署，因為 HTTP/2 連線完全由 IIS 處理。</span><span class="sxs-lookup"><span data-stu-id="3f193-1358">Target framework: Not applicable to out-of-process deployments, since the HTTP/2 connection is handled entirely by IIS.</span></span>
* <span data-ttu-id="3f193-1359">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="3f193-1359">TLS 1.2 or later connection</span></span>

<span data-ttu-id="3f193-1360">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/1.1`。</span><span class="sxs-lookup"><span data-stu-id="3f193-1360">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="3f193-1361">HTTP/2 預設為啟用。</span><span class="sxs-lookup"><span data-stu-id="3f193-1361">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="3f193-1362">如果 HTTP/2 連線尚未建立，連線會退為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="3f193-1362">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="3f193-1363">如需使用 IIS 部署之 HTTP/2 設定的詳細資訊，請參閱 [IIS 上的 HTTP/2](/iis/get-started/whats-new-in-iis-10/http2-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1363">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="cors-preflight-requests"></a><span data-ttu-id="3f193-1364">CORS 預檢要求</span><span class="sxs-lookup"><span data-stu-id="3f193-1364">CORS preflight requests</span></span>

<span data-ttu-id="3f193-1365">*此節只適用於以 .NET Framework 為目標的 ASP.NET Core 應用程式。*</span><span class="sxs-lookup"><span data-stu-id="3f193-1365">*This section only applies to ASP.NET Core apps that target the .NET Framework.*</span></span>

<span data-ttu-id="3f193-1366">針對以 .NET Framework 為目標的 ASP.NET Core 應用程式，在 IIS 中OPTIONS 要求預設不會傳遞到應用程式。</span><span class="sxs-lookup"><span data-stu-id="3f193-1366">For an ASP.NET Core app that targets the .NET Framework, OPTIONS requests aren't passed to the app by default in IIS.</span></span> <span data-ttu-id="3f193-1367">若要瞭解如何在 web.config 中設定應用程式的 IIS 處理*程式來傳遞*選項要求，請參閱[在 ASP.NET Web API 2 中啟用跨原始來源要求： CORS 的運作方式](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works)。</span><span class="sxs-lookup"><span data-stu-id="3f193-1367">To learn how to configure the app's IIS handlers in *web.config* to pass OPTIONS requests, see [Enable cross-origin requests in ASP.NET Web API 2: How CORS Works](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).</span></span>

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="3f193-1368">IIS 系統管理員的部署資源</span><span class="sxs-lookup"><span data-stu-id="3f193-1368">Deployment resources for IIS administrators</span></span>

* [<span data-ttu-id="3f193-1369">IIS 文件</span><span class="sxs-lookup"><span data-stu-id="3f193-1369">IIS documentation</span></span>](/iis)
* [<span data-ttu-id="3f193-1370">IIS 中的 IIS 管理員使用者入門</span><span class="sxs-lookup"><span data-stu-id="3f193-1370">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* [<span data-ttu-id="3f193-1371">.NET Core 應用程式部署</span><span class="sxs-lookup"><span data-stu-id="3f193-1371">.NET Core application deployment</span></span>](/dotnet/core/deploying/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/directory-structure>
* <xref:host-and-deploy/iis/modules>
* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>

## <a name="additional-resources"></a><span data-ttu-id="3f193-1372">其他資源</span><span class="sxs-lookup"><span data-stu-id="3f193-1372">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:index>
* [<span data-ttu-id="3f193-1373">Microsoft IIS 官方網站</span><span class="sxs-lookup"><span data-stu-id="3f193-1373">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="3f193-1374">Windows Server 技術內容庫</span><span class="sxs-lookup"><span data-stu-id="3f193-1374">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="3f193-1375">IIS 上的 HTTP/2</span><span class="sxs-lookup"><span data-stu-id="3f193-1375">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
* <xref:host-and-deploy/iis/transform-webconfig>

::: moniker-end
