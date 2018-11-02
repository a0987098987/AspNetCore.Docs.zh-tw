---
title: 在使用 IIS 的 Windows 上裝載 ASP.NET Core
author: guardrex
description: 了解如何在 Windows Server Internet Information Services (IIS) 上裝載 ASP.NET Core 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2018
uid: host-and-deploy/iis/index
ms.openlocfilehash: b204eeb35f8765bbafc85b3bb283809727356d3e
ms.sourcegitcommit: 4a6bbe84db24c2f3dd2de065de418fde952c8d40
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "50253204"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="0eacb-103">在使用 IIS 的 Windows 上裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0eacb-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="0eacb-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0eacb-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[<span data-ttu-id="0eacb-105">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="0eacb-105">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="0eacb-106">支援的作業系統</span><span class="sxs-lookup"><span data-stu-id="0eacb-106">Supported operating systems</span></span>

<span data-ttu-id="0eacb-107">支援下列作業系統：</span><span class="sxs-lookup"><span data-stu-id="0eacb-107">The following operating systems are supported:</span></span>

* <span data-ttu-id="0eacb-108">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="0eacb-108">Windows 7 or later</span></span>
* <span data-ttu-id="0eacb-109">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="0eacb-109">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="0eacb-110">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys) (先前稱為 [WebListener](xref:fundamentals/servers/weblistener)) 不適用搭配 IIS 的反向 Proxy 設定。</span><span class="sxs-lookup"><span data-stu-id="0eacb-110">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="0eacb-111">請使用 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-111">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="0eacb-112">如需在 Azure 中裝載的資訊，請參閱 <xref:host-and-deploy/azure-apps/index>。</span><span class="sxs-lookup"><span data-stu-id="0eacb-112">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

## <a name="http2-support"></a><span data-ttu-id="0eacb-113">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="0eacb-113">HTTP/2 support</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="0eacb-114">在下列 IIS 部署案例中，ASP.NET Core 支援 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="0eacb-114">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following IIS deployment scenarios:</span></span>

* <span data-ttu-id="0eacb-115">同處理序</span><span class="sxs-lookup"><span data-stu-id="0eacb-115">In-process</span></span>
  * <span data-ttu-id="0eacb-116">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="0eacb-116">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="0eacb-117">目標 Framework：.NET Core 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="0eacb-117">Target framework: .NET Core 2.2 or later</span></span>
  * <span data-ttu-id="0eacb-118">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="0eacb-118">TLS 1.2 or later connection</span></span>
* <span data-ttu-id="0eacb-119">跨處理序</span><span class="sxs-lookup"><span data-stu-id="0eacb-119">Out-of-process</span></span>
  * <span data-ttu-id="0eacb-120">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="0eacb-120">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="0eacb-121">公開 Edge Server 連線使用 HTTP/2，但是對 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的反向 Proxy 連線使用 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="0eacb-121">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
  * <span data-ttu-id="0eacb-122">目標 Framework：不適用於跨處理序部署，因為 HTTP/2 連線完全由 IIS 處理。</span><span class="sxs-lookup"><span data-stu-id="0eacb-122">Target framework: Not applicable to out-of-process deployments, since the HTTP/2 connection is handled entirely by IIS.</span></span>
  * <span data-ttu-id="0eacb-123">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="0eacb-123">TLS 1.2 or later connection</span></span>

<span data-ttu-id="0eacb-124">針對建立 HTTP/2 連線時的同處理序部署，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會回報 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="0eacb-124">For an in-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span> <span data-ttu-id="0eacb-125">針對建立 HTTP/2 連線時的跨處理序部署，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會回報 `HTTP/1.1`。</span><span class="sxs-lookup"><span data-stu-id="0eacb-125">For an out-of-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="0eacb-126">如需有關同處理序和跨處理序主控模型的詳細資訊，請參閱 <xref:fundamentals/servers/aspnet-core-module> 主題和 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="0eacb-126">For more information on the in-process and out-of-process hosting models, see the <xref:fundamentals/servers/aspnet-core-module> topic and the <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="0eacb-127">[HTTP/2](https://httpwg.org/specs/rfc7540.html) 支援符合下列基本需求的跨處理序部署：</span><span class="sxs-lookup"><span data-stu-id="0eacb-127">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported for out-of-process deployments that meet the following base requirements:</span></span>

* <span data-ttu-id="0eacb-128">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="0eacb-128">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
* <span data-ttu-id="0eacb-129">公開 Edge Server 連線使用 HTTP/2，但是對 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的反向 Proxy 連線使用 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="0eacb-129">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
* <span data-ttu-id="0eacb-130">目標 Framework：不適用於跨處理序部署，因為 HTTP/2 連線完全由 IIS 處理。</span><span class="sxs-lookup"><span data-stu-id="0eacb-130">Target framework: Not applicable to out-of-process deployments, since the HTTP/2 connection is handled entirely by IIS.</span></span>
* <span data-ttu-id="0eacb-131">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="0eacb-131">TLS 1.2 or later connection</span></span>

<span data-ttu-id="0eacb-132">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/1.1`。</span><span class="sxs-lookup"><span data-stu-id="0eacb-132">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="0eacb-133">HTTP/2 預設為啟用。</span><span class="sxs-lookup"><span data-stu-id="0eacb-133">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="0eacb-134">如果 HTTP/2 連線尚未建立，連線會退為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="0eacb-134">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="0eacb-135">如需使用 IIS 部署之 HTTP/2 設定的詳細資訊，請參閱 [IIS 上的 HTTP/2](/iis/get-started/whats-new-in-iis-10/http2-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-135">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="application-configuration"></a><span data-ttu-id="0eacb-136">應用程式組態</span><span class="sxs-lookup"><span data-stu-id="0eacb-136">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="0eacb-137">啟用 IISIntegration 元件</span><span class="sxs-lookup"><span data-stu-id="0eacb-137">Enable the IISIntegration components</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="0eacb-138">**同處理序主控模型**</span><span class="sxs-lookup"><span data-stu-id="0eacb-138">**In-process hosting model**</span></span>

<span data-ttu-id="0eacb-139">一般的 *Program.cs* 會呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 以開始設定主機。</span><span class="sxs-lookup"><span data-stu-id="0eacb-139">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host.</span></span> <span data-ttu-id="0eacb-140">`CreateDefaultBuilder` 會呼叫 `UseIIS` 方法來啟動 [CoreCLR](/dotnet/standard/glossary#coreclr)，並在 IIS 工作者處理序 (`w3wp.exe`) 內裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="0eacb-140">`CreateDefaultBuilder` calls the `UseIIS` method to boot the [CoreCLR](/dotnet/standard/glossary#coreclr) and host the app inside of the IIS worker process (`w3wp.exe`).</span></span> <span data-ttu-id="0eacb-141">效能測試指出，相較於對 [Kestrel](xref:fundamentals/servers/kestrel) 以跨處理序方式裝載應用程式和 Proxy 要求相比，以同處理序方式裝載 .NET Core 應用程式可提供更高的要求輸送量。</span><span class="sxs-lookup"><span data-stu-id="0eacb-141">Performance tests indicate that hosting a .NET Core app in-process delivers higher request throughput compared to hosting the app out-of-process and proxying requests to [Kestrel](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="0eacb-142">**跨處理序裝載模型**</span><span class="sxs-lookup"><span data-stu-id="0eacb-142">**Out-of-process hosting model**</span></span>

<span data-ttu-id="0eacb-143">一般的 *Program.cs* 會呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 以開始設定主機。</span><span class="sxs-lookup"><span data-stu-id="0eacb-143">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host.</span></span> <span data-ttu-id="0eacb-144">針對使用 IIS 的跨處理序裝載，`CreateDefaultBuilder` 會將 [Kestrel](xref:fundamentals/servers/kestrel) 設為 Web 伺服器，並設定 [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) 的基底路徑與連接埠來啟用 IIS 整合：</span><span class="sxs-lookup"><span data-stu-id="0eacb-144">For out-of-process hosting with IIS, `CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

<span data-ttu-id="0eacb-145">ASP.NET Core 模組會產生要指派給後端處理序的動態連接埠。</span><span class="sxs-lookup"><span data-stu-id="0eacb-145">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="0eacb-146">`CreateDefaultBuilder` 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 方法。</span><span class="sxs-lookup"><span data-stu-id="0eacb-146">`CreateDefaultBuilder` calls the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> method.</span></span> <span data-ttu-id="0eacb-147">`UseIISIntegration` 會將 Kestrel 設定為在位於 localhost IP 位址 (`127.0.0.1`) 的動態連接埠上接聽。</span><span class="sxs-lookup"><span data-stu-id="0eacb-147">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="0eacb-148">若動態連接埠是 1234，Kestrel 會在 `127.0.0.1:1234` 接聽。</span><span class="sxs-lookup"><span data-stu-id="0eacb-148">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="0eacb-149">此設定會取代由下列項目提供的其他 URL 設定：</span><span class="sxs-lookup"><span data-stu-id="0eacb-149">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="0eacb-150">Kestrel 的接聽 API</span><span class="sxs-lookup"><span data-stu-id="0eacb-150">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="0eacb-151">[設定](xref:fundamentals/configuration/index) (或[命令列 --urls 選項](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="0eacb-151">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="0eacb-152">在使用模組時不需要呼叫 `UseUrls` 或 Kestrel 的 `Listen` API。</span><span class="sxs-lookup"><span data-stu-id="0eacb-152">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="0eacb-153">若呼叫 `UseUrls` 或 `Listen`，Kestrel 只會接聽未使用 IIS 執行應用程式時指定的連接埠。</span><span class="sxs-lookup"><span data-stu-id="0eacb-153">If `UseUrls` or `Listen` is called, Kestrel listens on the ports specified only when running the app without IIS.</span></span>

<span data-ttu-id="0eacb-154">如需有關同處理序和跨處理序主控模型的詳細資訊，請參閱 <xref:fundamentals/servers/aspnet-core-module> 主題和 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="0eacb-154">For more information on the in-process and out-of-process hosting models, see the <xref:fundamentals/servers/aspnet-core-module> topic and the <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="0eacb-155">一般的 *Program.cs* 會呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 以開始設定主機。</span><span class="sxs-lookup"><span data-stu-id="0eacb-155">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host.</span></span> <span data-ttu-id="0eacb-156">`CreateDefaultBuilder` 會將 [Kestrel](xref:fundamentals/servers/kestrel) 設為網頁伺服器，並設定 [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) 的基底路徑與連接埠來啟用 IIS 整合：</span><span class="sxs-lookup"><span data-stu-id="0eacb-156">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

<span data-ttu-id="0eacb-157">ASP.NET Core 模組會產生要指派給後端處理序的動態連接埠。</span><span class="sxs-lookup"><span data-stu-id="0eacb-157">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="0eacb-158">`CreateDefaultBuilder` 會呼叫 [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) 方法。</span><span class="sxs-lookup"><span data-stu-id="0eacb-158">`CreateDefaultBuilder` calls the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) method.</span></span> <span data-ttu-id="0eacb-159">`UseIISIntegration` 會將 Kestrel 設定為在位於 localhost IP 位址 (`127.0.0.1`) 的動態連接埠上接聽。</span><span class="sxs-lookup"><span data-stu-id="0eacb-159">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="0eacb-160">若動態連接埠是 1234，Kestrel 會在 `127.0.0.1:1234` 接聽。</span><span class="sxs-lookup"><span data-stu-id="0eacb-160">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="0eacb-161">此設定會取代由下列項目提供的其他 URL 設定：</span><span class="sxs-lookup"><span data-stu-id="0eacb-161">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="0eacb-162">Kestrel 的接聽 API</span><span class="sxs-lookup"><span data-stu-id="0eacb-162">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="0eacb-163">[設定](xref:fundamentals/configuration/index) (或[命令列 --urls 選項](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="0eacb-163">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="0eacb-164">在使用模組時不需要呼叫 `UseUrls` 或 Kestrel 的 `Listen` API。</span><span class="sxs-lookup"><span data-stu-id="0eacb-164">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="0eacb-165">若呼叫 `UseUrls` 或 `Listen`，Kestrel 只會接聽未使用 IIS 執行應用程式時指定的連接埠。</span><span class="sxs-lookup"><span data-stu-id="0eacb-165">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="0eacb-166">一般的 *Program.cs* 會呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 以開始設定主機。</span><span class="sxs-lookup"><span data-stu-id="0eacb-166">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host.</span></span> <span data-ttu-id="0eacb-167">`CreateDefaultBuilder` 會將 [Kestrel](xref:fundamentals/servers/kestrel) 設為網頁伺服器，並設定 [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) 的基底路徑與連接埠來啟用 IIS 整合：</span><span class="sxs-lookup"><span data-stu-id="0eacb-167">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

<span data-ttu-id="0eacb-168">ASP.NET Core 模組會產生要指派給後端處理序的動態連接埠。</span><span class="sxs-lookup"><span data-stu-id="0eacb-168">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="0eacb-169">`CreateDefaultBuilder` 會呼叫 [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) 方法。</span><span class="sxs-lookup"><span data-stu-id="0eacb-169">`CreateDefaultBuilder` calls the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) method.</span></span> <span data-ttu-id="0eacb-170">`UseIISIntegration` 會將 Kestrel 設定為在位於 localhost IP 位址 (`localhost`) 的動態連接埠上接聽。</span><span class="sxs-lookup"><span data-stu-id="0eacb-170">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`localhost`).</span></span> <span data-ttu-id="0eacb-171">若動態連接埠是 1234，Kestrel 會在 `localhost:1234` 接聽。</span><span class="sxs-lookup"><span data-stu-id="0eacb-171">If the dynamic port is 1234, Kestrel listens at `localhost:1234`.</span></span> <span data-ttu-id="0eacb-172">此設定會取代由下列項目提供的其他 URL 設定：</span><span class="sxs-lookup"><span data-stu-id="0eacb-172">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="0eacb-173">Kestrel 的接聽 API</span><span class="sxs-lookup"><span data-stu-id="0eacb-173">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="0eacb-174">[設定](xref:fundamentals/configuration/index) (或[命令列 --urls 選項](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="0eacb-174">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="0eacb-175">在使用模組時不需要呼叫 `UseUrls` 或 Kestrel 的 `Listen` API。</span><span class="sxs-lookup"><span data-stu-id="0eacb-175">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="0eacb-176">若呼叫 `UseUrls` 或 `Listen`，Kestrel 只會接聽未使用 IIS 執行應用程式時指定的連接埠。</span><span class="sxs-lookup"><span data-stu-id="0eacb-176">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0eacb-177">在應用程式相依性的 [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) 套件中包含相依性。</span><span class="sxs-lookup"><span data-stu-id="0eacb-177">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the app's dependencies.</span></span> <span data-ttu-id="0eacb-178">將 [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) 擴充方法新增至 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)，以使用 IIS 整合中介軟體：</span><span class="sxs-lookup"><span data-stu-id="0eacb-178">Use IIS Integration middleware by adding the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) extension method to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder):</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="0eacb-179">同時需要 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) 和 [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-179">Both [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) and [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) are required.</span></span> <span data-ttu-id="0eacb-180">呼叫 `UseIISIntegration` 的程式碼並不會影響程式碼的可攜性。</span><span class="sxs-lookup"><span data-stu-id="0eacb-180">Code calling `UseIISIntegration` doesn't affect code portability.</span></span> <span data-ttu-id="0eacb-181">若應用程式不在 IIS 背後執行 (例如，應用程式直接在 Kestrel 上執行)，`UseIISIntegration` 就不會運作。</span><span class="sxs-lookup"><span data-stu-id="0eacb-181">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` doesn't operate.</span></span>

<span data-ttu-id="0eacb-182">ASP.NET Core 模組會產生要指派給後端處理序的動態連接埠。</span><span class="sxs-lookup"><span data-stu-id="0eacb-182">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="0eacb-183">`UseIISIntegration` 會將 Kestrel 設定為在位於 localhost IP 位址 (`localhost`) 的動態連接埠上接聽。</span><span class="sxs-lookup"><span data-stu-id="0eacb-183">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`localhost`).</span></span> <span data-ttu-id="0eacb-184">若動態連接埠是 1234，Kestrel 會在 `localhost:1234` 接聽。</span><span class="sxs-lookup"><span data-stu-id="0eacb-184">If the dynamic port is 1234, Kestrel listens at `localhost:1234`.</span></span> <span data-ttu-id="0eacb-185">此設定會取代由下列項目提供的其他 URL 設定：</span><span class="sxs-lookup"><span data-stu-id="0eacb-185">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* <span data-ttu-id="0eacb-186">[設定](xref:fundamentals/configuration/index) (或[命令列 --urls 選項](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="0eacb-186">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="0eacb-187">使用模組時不需要呼叫 `UseUrls`。</span><span class="sxs-lookup"><span data-stu-id="0eacb-187">A call to `UseUrls` isn't required when using the module.</span></span> <span data-ttu-id="0eacb-188">若呼叫 `UseUrls`，Kestrel 只會接聽未使用 IIS 執行應用程式時指定的連接埠。</span><span class="sxs-lookup"><span data-stu-id="0eacb-188">If `UseUrls` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

<span data-ttu-id="0eacb-189">若要在 ASP.NET Core 1.0 應用程式上呼叫 `UseUrls`，請在呼叫 `UseIISIntegration`**前**予以呼叫，以避免模組設定的連接埠受到覆寫。</span><span class="sxs-lookup"><span data-stu-id="0eacb-189">If `UseUrls` is called in an ASP.NET Core 1.0 app, call it **before** calling `UseIISIntegration` so that the module-configured port isn't overwritten.</span></span> <span data-ttu-id="0eacb-190">在 ASP.NET Core 1.1 中，因為模組設定會覆寫 `UseUrls`，所以不需要呼叫順序。</span><span class="sxs-lookup"><span data-stu-id="0eacb-190">This calling order isn't required with ASP.NET Core 1.1 because the module setting overrides `UseUrls`.</span></span>

::: moniker-end

<span data-ttu-id="0eacb-191">如需代管的詳細資訊，請參閱[在 ASP.NET Core 中代管](xref:fundamentals/host/index)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-191">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/host/index).</span></span>

### <a name="iis-options"></a><span data-ttu-id="0eacb-192">IIS 選項</span><span class="sxs-lookup"><span data-stu-id="0eacb-192">IIS options</span></span>

<span data-ttu-id="0eacb-193">若要設定 IIS 選項，請在 [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices) 中包括 [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="0eacb-193">To configure IIS options, include a service configuration for [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) in [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span></span> <span data-ttu-id="0eacb-194">在下列範例中，已停用將用戶端憑證轉送至應用程式以填入 `HttpContext.Connection.ClientCertificate` 的功能：</span><span class="sxs-lookup"><span data-stu-id="0eacb-194">In the following example, forwarding client certificates to the app to populate `HttpContext.Connection.ClientCertificate` is disabled:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="0eacb-195">選項</span><span class="sxs-lookup"><span data-stu-id="0eacb-195">Option</span></span>                         | <span data-ttu-id="0eacb-196">預設</span><span class="sxs-lookup"><span data-stu-id="0eacb-196">Default</span></span> | <span data-ttu-id="0eacb-197">設定</span><span class="sxs-lookup"><span data-stu-id="0eacb-197">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="0eacb-198">若為 `true`，IIS 整合中介軟體會設定由 [Windows Authentication](xref:security/authentication/windowsauth) 所驗證的 `HttpContext.User`。</span><span class="sxs-lookup"><span data-stu-id="0eacb-198">If `true`, IIS Integration Middleware sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="0eacb-199">如果為 `false`，則驗證中介軟體僅針對 `HttpContext.User` 提供身分識別，並在游 `AuthenticationScheme` 提出明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="0eacb-199">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="0eacb-200">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="0eacb-200">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="0eacb-201">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="0eacb-201">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="0eacb-202">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="0eacb-202">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="0eacb-203">如果為 `true` 且 `MS-ASPNETCORE-CLIENTCERT` 要求標頭已存在，則會填入 `HttpContext.Connection.ClientCertificate`。</span><span class="sxs-lookup"><span data-stu-id="0eacb-203">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="0eacb-204">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="0eacb-204">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="0eacb-205">用來設定轉送標頭中介軟體及 ASP.NET Core 模組的 IIS Integration 中介軟體會設定為轉送配置 (HTTP/HTTPS) 及發出要求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="0eacb-205">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="0eacb-206">其他 Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。</span><span class="sxs-lookup"><span data-stu-id="0eacb-206">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="0eacb-207">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-207">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="0eacb-208">web.config 檔案</span><span class="sxs-lookup"><span data-stu-id="0eacb-208">web.config file</span></span>

<span data-ttu-id="0eacb-209">*web.config* 檔案是用來設定 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-209">The *web.config* file configures the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="0eacb-210">發佈專案時，由 MSBuild 目標 (`_TransformWebConfig`) 處理 *web.config* 檔案的建立、轉換及發佈。</span><span class="sxs-lookup"><span data-stu-id="0eacb-210">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="0eacb-211">此目標存在於 Web SDK 目標 (`Microsoft.NET.Sdk.Web`)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-211">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="0eacb-212">SDK 設定在專案檔的頂端：</span><span class="sxs-lookup"><span data-stu-id="0eacb-212">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="0eacb-213">如果專案中沒有 *web.config* 檔案，則系統會使用正確的 *processPath* 和 *arguments* 建立該檔案以設定 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)，並將該檔案移至[已發行的輸出](xref:host-and-deploy/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-213">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="0eacb-214">如果 *web.config* 檔案存在於專案中，則系統會使用正確的 *processPath* 和 *arguments* 來轉換該檔案以設定 ASP.NET Core 模組，然後將它移至已發行的輸出。</span><span class="sxs-lookup"><span data-stu-id="0eacb-214">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="0eacb-215">轉換不會修改檔案中的 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="0eacb-215">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="0eacb-216">*web.config* 檔案可提供能控制作用中 IIS 模組的額外 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="0eacb-216">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="0eacb-217">如需能處理 ASP.NET Core 應用程式要求之 IIS 模組的相關資訊，請參閱 [IIS 模組](xref:host-and-deploy/iis/modules)主題。</span><span class="sxs-lookup"><span data-stu-id="0eacb-217">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="0eacb-218">為防止 Web SDK 轉換 *web.config* 檔案，請使用專案檔中的 **\<IsTransformWebConfigDisabled>** 屬性：</span><span class="sxs-lookup"><span data-stu-id="0eacb-218">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="0eacb-219">使 Web SDK 無法轉換檔案時，應該由開發人員手動設定 *processPath* 和 *arguments*。</span><span class="sxs-lookup"><span data-stu-id="0eacb-219">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="0eacb-220">如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-220">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="0eacb-221">web.config 檔案位置</span><span class="sxs-lookup"><span data-stu-id="0eacb-221">web.config file location</span></span>

<span data-ttu-id="0eacb-222">為了在 IIS 與 Kestrel 伺服器之間建立反向 Proxy，*web.config* 檔案必須存在於已部署應用程式的內容根路徑 (通常是應用程式基底路徑)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-222">In order to create the reverse proxy between IIS and the Kestrel server, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="0eacb-223">這是與提供給 IIS 的網站實體路徑相同的位置。</span><span class="sxs-lookup"><span data-stu-id="0eacb-223">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="0eacb-224">應用程式的根目錄需有 *web.config* 檔案，才能使用 Web Deploy 發行多個應用程式。</span><span class="sxs-lookup"><span data-stu-id="0eacb-224">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="0eacb-225">機密檔案存在於應用程式的實體路徑上，例如 *\<組件>.runtimeconfig.json*、*\<組件>.xml* (XML 文件註解)，以及 *\<組件>.deps.json*。</span><span class="sxs-lookup"><span data-stu-id="0eacb-225">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="0eacb-226">當 *web.config* 檔案存在且網站正常啟動時，在有人要求機密檔案的情況下，IIS 並不會提供它們。</span><span class="sxs-lookup"><span data-stu-id="0eacb-226">When the *web.config* file is present and and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="0eacb-227">若 *web.config* 檔案遺失或沒有正確命名，或是無法設定網站以正常啟動，IIS 可能會公開提供機密檔案。</span><span class="sxs-lookup"><span data-stu-id="0eacb-227">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="0eacb-228">***web.config* 檔案必須持續存在於部署之中、已正確命名，並能夠設定網站以正常啟動。無論在任何情況下，請都不要從生產環境部署移除 *web.config* 檔案。**</span><span class="sxs-lookup"><span data-stu-id="0eacb-228">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="0eacb-229">IIS 組態</span><span class="sxs-lookup"><span data-stu-id="0eacb-229">IIS configuration</span></span>

<span data-ttu-id="0eacb-230">**Windows Server 作業系統**</span><span class="sxs-lookup"><span data-stu-id="0eacb-230">**Windows Server operating systems**</span></span>

<span data-ttu-id="0eacb-231">啟用**網頁伺服器 (IIS)** 伺服器角色，並建立角色服務。</span><span class="sxs-lookup"><span data-stu-id="0eacb-231">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="0eacb-232">使用來自 [管理] 功能表的 [新增角色及功能] 精靈，或是 [伺服器管理員] 中的連結。</span><span class="sxs-lookup"><span data-stu-id="0eacb-232">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="0eacb-233">在**伺服器角色**步驟中，核取 [網頁伺服器 (IIS)] 方塊。</span><span class="sxs-lookup"><span data-stu-id="0eacb-233">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![在選取伺服器角色步驟中選取網頁伺服器 IIS 角色。](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="0eacb-235">在 [功能] 步驟之後，[角色服務] 步驟會針對網頁伺服器 (IIS) 進行載入。</span><span class="sxs-lookup"><span data-stu-id="0eacb-235">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="0eacb-236">選取所需的 IIS 角色服務或接受所提供的預設角色服務。</span><span class="sxs-lookup"><span data-stu-id="0eacb-236">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![在選取角色服務步驟中，選取預設的角色服務。](index/_static/role-services-ws2016.png)

   <span data-ttu-id="0eacb-238">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="0eacb-238">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="0eacb-239">若要啟用 Windows 驗證，請展開下列節點：[網頁伺服器] > [安全性]。</span><span class="sxs-lookup"><span data-stu-id="0eacb-239">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="0eacb-240">選取 [Windows 驗證] 功能。</span><span class="sxs-lookup"><span data-stu-id="0eacb-240">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="0eacb-241">如需詳細資訊，請參閱 [Windows 驗證 \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) 和[設定 Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-241">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="0eacb-242">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="0eacb-242">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="0eacb-243">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="0eacb-243">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="0eacb-244">若要啟用 WebSocket，請展開下列節點：[網頁伺服器] > [應用程式開發]。</span><span class="sxs-lookup"><span data-stu-id="0eacb-244">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="0eacb-245">選取 [WebSocket 通訊協定] 功能。</span><span class="sxs-lookup"><span data-stu-id="0eacb-245">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="0eacb-246">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-246">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="0eacb-247">透過**確認**步驟繼續作業，安裝網頁伺服器角色和服務。</span><span class="sxs-lookup"><span data-stu-id="0eacb-247">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="0eacb-248">安裝**網頁伺服器 (IIS)** 角色之後，不需要重新啟動伺服器/IIS。</span><span class="sxs-lookup"><span data-stu-id="0eacb-248">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="0eacb-249">**Windows 桌面作業系統**</span><span class="sxs-lookup"><span data-stu-id="0eacb-249">**Windows desktop operating systems**</span></span>

<span data-ttu-id="0eacb-250">啟用 [IIS 管理主控台] 和 [World Wide Web 服務]。</span><span class="sxs-lookup"><span data-stu-id="0eacb-250">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="0eacb-251">瀏覽至控制台 > [程式] > [程式和功能] > [開啟或關閉 Windows 功能] (畫面左側)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-251">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="0eacb-252">開啟 [Internet Information Services] 節點。</span><span class="sxs-lookup"><span data-stu-id="0eacb-252">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="0eacb-253">開啟 [Web 管理工具] 節點。</span><span class="sxs-lookup"><span data-stu-id="0eacb-253">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="0eacb-254">核取 [IIS 管理主控台] 方塊。</span><span class="sxs-lookup"><span data-stu-id="0eacb-254">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="0eacb-255">[World Wide Web Services] (全球資訊網服務) 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="0eacb-255">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="0eacb-256">接受**全球資訊網服務**的預設功能，或自訂 IIS 功能。</span><span class="sxs-lookup"><span data-stu-id="0eacb-256">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="0eacb-257">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="0eacb-257">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="0eacb-258">若要啟用 Windows 驗證，請展開下列節點：[World Wide Web 服務] > [安全性]。</span><span class="sxs-lookup"><span data-stu-id="0eacb-258">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="0eacb-259">選取 [Windows 驗證] 功能。</span><span class="sxs-lookup"><span data-stu-id="0eacb-259">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="0eacb-260">如需詳細資訊，請參閱 [Windows 驗證 \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) 和[設定 Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-260">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="0eacb-261">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="0eacb-261">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="0eacb-262">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="0eacb-262">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="0eacb-263">若要啟用 WebSocket，請展開下列節點：[World Wide Web 服務] > [應用程式開發功能]。</span><span class="sxs-lookup"><span data-stu-id="0eacb-263">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="0eacb-264">選取 [WebSocket 通訊協定] 功能。</span><span class="sxs-lookup"><span data-stu-id="0eacb-264">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="0eacb-265">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-265">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="0eacb-266">若 IIS 安裝需要重新啟動，請重新啟動系統。</span><span class="sxs-lookup"><span data-stu-id="0eacb-266">If the IIS installation requires a restart, restart the system.</span></span>

![選取 [Windows 功能] 中的 [IIS 管理主控台] 和 [World Wide Web Services] (全球資訊網服務)。](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="0eacb-268">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="0eacb-268">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="0eacb-269">在主控系統上安裝 .NET Core 裝載套件組合。</span><span class="sxs-lookup"><span data-stu-id="0eacb-269">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="0eacb-270">套件組合會安裝 .NET Core 執行階段、.NET Core 程式庫和 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-270">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="0eacb-271">此模組會在 IIS 和 Kestrel 伺服器之間建立反向 Proxy。</span><span class="sxs-lookup"><span data-stu-id="0eacb-271">The module creates the reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="0eacb-272">如果系統沒有網際網路連線，請先取得並安裝 [Microsoft Visual C++ 2015 可轉散發套件](https://www.microsoft.com/download/details.aspx?id=53840)，再安裝 .NET Core 裝載套件組合。</span><span class="sxs-lookup"><span data-stu-id="0eacb-272">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Hosting Bundle.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0eacb-273">若裝載套件組合在 IIS 之前安裝，則必須對該套件組合安裝進行修復。</span><span class="sxs-lookup"><span data-stu-id="0eacb-273">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="0eacb-274">請在安裝 IIS 之後，再次執行裝載套件組合安裝程式。</span><span class="sxs-lookup"><span data-stu-id="0eacb-274">Run the Hosting Bundle installer again after installing IIS.</span></span>

### <a name="direct-download-current-version"></a><span data-ttu-id="0eacb-275">直接下載 (目前版本)</span><span class="sxs-lookup"><span data-stu-id="0eacb-275">Direct download (current version)</span></span>

<span data-ttu-id="0eacb-276">使用下列連結下載安裝程式：</span><span class="sxs-lookup"><span data-stu-id="0eacb-276">Download the installer using the following link:</span></span>

[<span data-ttu-id="0eacb-277">目前的 .NET Core 裝載套件組合安裝程式 (直接下載)</span><span class="sxs-lookup"><span data-stu-id="0eacb-277">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a><span data-ttu-id="0eacb-278">安裝程式的先前版本</span><span class="sxs-lookup"><span data-stu-id="0eacb-278">Earlier versions of the installer</span></span>

<span data-ttu-id="0eacb-279">若要取得安裝程式的先前版本：</span><span class="sxs-lookup"><span data-stu-id="0eacb-279">To obtain an earlier version of the installer:</span></span>

1. <span data-ttu-id="0eacb-280">瀏覽至 [.NET 下載封存](https://www.microsoft.com/net/download/archives)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-280">Navigate to the [.NET download archives](https://www.microsoft.com/net/download/archives).</span></span>
1. <span data-ttu-id="0eacb-281">在 [.NET Core] 下，選取 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="0eacb-281">Under **.NET Core**, select the .NET Core version.</span></span>
1. <span data-ttu-id="0eacb-282">在 [執行應用程式 - 執行階段] 欄中，尋找想要的 .NET Core 執行階段版本列。</span><span class="sxs-lookup"><span data-stu-id="0eacb-282">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="0eacb-283">使用**執行階段與裝載套件組合**連結。</span><span class="sxs-lookup"><span data-stu-id="0eacb-283">Download the installer using the **Runtime & Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="0eacb-284">某些安裝程式包含已達到期生命週期結束 (EOL) 的發行版本，這些發行版本已不受 Microsoft 支援。</span><span class="sxs-lookup"><span data-stu-id="0eacb-284">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="0eacb-285">如需詳細資訊，請參閱[支援原則](https://www.microsoft.com/net/download/dotnet-core/2.0) \(英文 \)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-285">For more information, see the [support policy](https://www.microsoft.com/net/download/dotnet-core/2.0).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="0eacb-286">安裝裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="0eacb-286">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="0eacb-287">在伺服器上執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="0eacb-287">Run the installer on the server.</span></span> <span data-ttu-id="0eacb-288">從系統管理員命令提示字元執行安裝程式時，有 下列切換參數可用：</span><span class="sxs-lookup"><span data-stu-id="0eacb-288">The following switches are available when running the installer from an administrator command prompt:</span></span>

   * <span data-ttu-id="0eacb-289">`OPT_NO_ANCM=1` &ndash; 跳過安裝 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="0eacb-289">`OPT_NO_ANCM=1` &ndash; Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="0eacb-290">`OPT_NO_RUNTIME=1` &ndash; 跳過安裝 .NET Core 執行階段。</span><span class="sxs-lookup"><span data-stu-id="0eacb-290">`OPT_NO_RUNTIME=1` &ndash; Skip installing the .NET Core runtime.</span></span>
   * <span data-ttu-id="0eacb-291">`OPT_NO_SHAREDFX=1` &ndash; 跳過安裝 ASP.NET 共用架構 (ASP.NET 執行階段)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-291">`OPT_NO_SHAREDFX=1` &ndash; Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span>
   * <span data-ttu-id="0eacb-292">`OPT_NO_X86=1` &ndash; 跳過安裝 x86 執行階段。</span><span class="sxs-lookup"><span data-stu-id="0eacb-292">`OPT_NO_X86=1` &ndash; Skip installing x86 runtimes.</span></span> <span data-ttu-id="0eacb-293">當您確定不會裝載 32 位元應用程式時，請使用此切換。</span><span class="sxs-lookup"><span data-stu-id="0eacb-293">Use this switch when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="0eacb-294">如果將來有可能同時裝載 32 位元和 64 位元應用程式，請不要使用此切換並安裝這兩個執行階段。</span><span class="sxs-lookup"><span data-stu-id="0eacb-294">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this switch and install both runtimes.</span></span>
1. <span data-ttu-id="0eacb-295">請重新啟動系統，或是從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**。</span><span class="sxs-lookup"><span data-stu-id="0eacb-295">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span> <span data-ttu-id="0eacb-296">重新啟動 IIS 將能偵測到由安裝程式對系統路徑 (此為環境變數) 所做出的變更。</span><span class="sxs-lookup"><span data-stu-id="0eacb-296">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

<span data-ttu-id="0eacb-297">如果 Windows 裝載套件組合安裝程式偵測到 IIS 需要重設才能完成安裝，安裝程式會重設 IIS。</span><span class="sxs-lookup"><span data-stu-id="0eacb-297">If the Windows Hosting Bundle installer detects that IIS requires a reset in order to complete installation, the installer resets IIS.</span></span> <span data-ttu-id="0eacb-298">如果安裝程式觸發 IIS 重設，所有 IIS 應用程式集區和網站都會重新啟動。</span><span class="sxs-lookup"><span data-stu-id="0eacb-298">If the installer triggers an IIS reset, all of the IIS app pools and websites are restarted.</span></span>

> [!NOTE]
> <span data-ttu-id="0eacb-299">如需 IIS 共用組態的資訊，請參閱[使用 IIS 共用組態的 ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-299">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="0eacb-300">使用 Visual Studio 發佈時安裝 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="0eacb-300">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="0eacb-301">將應用程式部署到具有 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 的伺服器時，請在伺服器上安裝最新版的 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="0eacb-301">When deploying apps to servers with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="0eacb-302">若要安裝 Web Deploy，請使用 [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) 或從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=43717)直接取得安裝程式。</span><span class="sxs-lookup"><span data-stu-id="0eacb-302">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="0eacb-303">慣用的方法是使用 WebPI。</span><span class="sxs-lookup"><span data-stu-id="0eacb-303">The preferred method is to use WebPI.</span></span> <span data-ttu-id="0eacb-304">WebPI 提供獨立的安裝程式和組態以裝載提供者。</span><span class="sxs-lookup"><span data-stu-id="0eacb-304">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="0eacb-305">建立 IIS 網站</span><span class="sxs-lookup"><span data-stu-id="0eacb-305">Create the IIS site</span></span>

1. <span data-ttu-id="0eacb-306">在主控系統上，請建立資料夾以容納應用程式的已發行資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="0eacb-306">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="0eacb-307">應用程式的部署配置已詳述於[目錄結構](xref:host-and-deploy/directory-structure)主題。</span><span class="sxs-lookup"><span data-stu-id="0eacb-307">An app's deployment layout is described in the [Directory Structure](xref:host-and-deploy/directory-structure) topic.</span></span>

1. <span data-ttu-id="0eacb-308">在新的資料夾內，建立 *logs* 資料夾，以在啟用 stdout 記錄時保留 ASP.NET Core 模組 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="0eacb-308">Within the new folder, create a *logs* folder to hold ASP.NET Core Module stdout logs when stdout logging is enabled.</span></span> <span data-ttu-id="0eacb-309">如果應用程式已在承載中部署 *logs* 資料夾，請略過此步驟。</span><span class="sxs-lookup"><span data-stu-id="0eacb-309">If the app is deployed with a *logs* folder in the payload, skip this step.</span></span> <span data-ttu-id="0eacb-310">如需在於本機建置專案的情況下使 MSBuild 自動建立 *logs* 資料夾的相關指示，請參閱[目錄結構](xref:host-and-deploy/directory-structure)主題。</span><span class="sxs-lookup"><span data-stu-id="0eacb-310">For instructions on how to enable MSBuild to create the *logs* folder automatically when the project is built locally, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="0eacb-311">請只使用 stdout 記錄來對應用程式啟動失敗進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="0eacb-311">Only use the stdout log to troubleshoot app startup failures.</span></span> <span data-ttu-id="0eacb-312">請永遠不要將 stdout 記錄用於例行的應用程式記錄。</span><span class="sxs-lookup"><span data-stu-id="0eacb-312">Never use stdout logging for routine app logging.</span></span> <span data-ttu-id="0eacb-313">因為它並沒有記錄檔大小或數量上的限制。</span><span class="sxs-lookup"><span data-stu-id="0eacb-313">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="0eacb-314">應用程式集區必須具有記錄檔寫入位置的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="0eacb-314">The app pool must have write access to the location where the logs are written.</span></span> <span data-ttu-id="0eacb-315">所有位於記錄檔位置路徑上的資料夾都必須存在。</span><span class="sxs-lookup"><span data-stu-id="0eacb-315">All of the folders on the path to the log location must exist.</span></span> <span data-ttu-id="0eacb-316">如需 stdout 記錄的詳細資訊，請參閱[記錄建立和重新導向](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-316">For more information on the stdout log, see [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span> <span data-ttu-id="0eacb-317">如需在 ASP.NET Core 應用程式中進行記錄的相關資訊，請參閱[記錄](xref:fundamentals/logging/index)主題。</span><span class="sxs-lookup"><span data-stu-id="0eacb-317">For information on logging in an ASP.NET Core app, see the [Logging](xref:fundamentals/logging/index) topic.</span></span>

1. <span data-ttu-id="0eacb-318">在 [IIS 管理員] 中，於 [連線] 面板中開啟伺服器的節點。</span><span class="sxs-lookup"><span data-stu-id="0eacb-318">In **IIS Manager**, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="0eacb-319">以滑鼠右鍵按一下 [網站] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="0eacb-319">Right-click the **Sites** folder.</span></span> <span data-ttu-id="0eacb-320">從操作功能表選取 [新增網站]。</span><span class="sxs-lookup"><span data-stu-id="0eacb-320">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="0eacb-321">提供**網站名稱**，並將**實體路徑**設定為應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="0eacb-321">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="0eacb-322">透過選取 [確定] 來提供**繫結**設定並建立網站：</span><span class="sxs-lookup"><span data-stu-id="0eacb-322">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![在新增網站步驟中提供站台名稱、實體路徑和主機名稱。](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="0eacb-324">請**勿**使用最上層萬用字元繫結 (`http://*:80/`與 `http://+:80`)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-324">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="0eacb-325">最上層萬用字元繫結可能暴露您的應用程式安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="0eacb-325">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="0eacb-326">這對強式與弱式萬用字元皆適用。</span><span class="sxs-lookup"><span data-stu-id="0eacb-326">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="0eacb-327">請使用明確主機名稱，而非萬用字元。</span><span class="sxs-lookup"><span data-stu-id="0eacb-327">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="0eacb-328">若您擁有整個父網域 (與具弱點的 `*.com` 相對) 的控制權，則子網域萬用字元繫結 (例如 `*.mysub.com`) 就沒有此安全性風險。</span><span class="sxs-lookup"><span data-stu-id="0eacb-328">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="0eacb-329">如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-329">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="0eacb-330">在伺服器的節點之下，選取 [應用程式集區]。</span><span class="sxs-lookup"><span data-stu-id="0eacb-330">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="0eacb-331">以滑鼠右鍵按一下網站的應用程式集區，然後從操作功能表選取 [基本設定]。</span><span class="sxs-lookup"><span data-stu-id="0eacb-331">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="0eacb-332">在 [編輯應用程式集區] 視窗中，將 [.NET CLR 版本] 設定為 [沒有受控碼]：</span><span class="sxs-lookup"><span data-stu-id="0eacb-332">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![將 .NET CLR 版本設為 [沒有受控碼]。](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="0eacb-334">ASP.NET Core 會在不同的處理序中執行，並管理執行階段。</span><span class="sxs-lookup"><span data-stu-id="0eacb-334">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="0eacb-335">ASP.NET Core 不依賴載入桌面 CLR。</span><span class="sxs-lookup"><span data-stu-id="0eacb-335">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span> <span data-ttu-id="0eacb-336">將 [.NET CLR 版本] 設為 [沒有受控碼] 是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="0eacb-336">Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span>

1. <span data-ttu-id="0eacb-337">確認處理序模型身分識別具有適當的權限。</span><span class="sxs-lookup"><span data-stu-id="0eacb-337">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="0eacb-338">如果您將應用程式集區的預設身分識別 ([處理序模型] > [身分識別]) 從 **ApplicationPoolIdentity** 變更為其他身分識別，請確認新的身分識別具有必要權限，可存取應用程式的資料夾、資料庫和其他必要的資源。</span><span class="sxs-lookup"><span data-stu-id="0eacb-338">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="0eacb-339">例如，應用程式集區需要針對應用程式讀取和寫入檔案的資料夾取得讀取和寫入權限。</span><span class="sxs-lookup"><span data-stu-id="0eacb-339">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="0eacb-340">**Windows 驗證設定 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="0eacb-340">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="0eacb-341">如需詳細資訊，請參閱[設定 Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="0eacb-341">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="0eacb-342">部署應用程式</span><span class="sxs-lookup"><span data-stu-id="0eacb-342">Deploy the app</span></span>

<span data-ttu-id="0eacb-343">將應用程式部署至在主控系統上建立的資料夾。</span><span class="sxs-lookup"><span data-stu-id="0eacb-343">Deploy the app to the folder created on the hosting system.</span></span> <span data-ttu-id="0eacb-344">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 是建議的部署機制。</span><span class="sxs-lookup"><span data-stu-id="0eacb-344">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="0eacb-345">使用 Visual Studio 的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="0eacb-345">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="0eacb-346">請參閱[適用於 ASP.NET Core 應用程式部署的 Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)主題，了解如何建立搭配 Web Deploy 使用的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="0eacb-346">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="0eacb-347">如果裝載提供者提供或支援建立發行設定檔，請下載其設定檔，並使用 Visual Studio 的 [發行] 對話方塊匯入其設定檔。</span><span class="sxs-lookup"><span data-stu-id="0eacb-347">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![[發佈] 對話方塊頁](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="0eacb-349">Visual Studio 外部的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="0eacb-349">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="0eacb-350">透過命令列也可以在 Visual Studio 外部使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-350">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="0eacb-351">如需詳細資訊，請參閱 [Web 部署工具](/iis/publish/using-web-deploy/use-the-web-deployment-tool)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-351">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="0eacb-352">Web Deploy 的替代項目</span><span class="sxs-lookup"><span data-stu-id="0eacb-352">Alternatives to Web Deploy</span></span>

<span data-ttu-id="0eacb-353">使用數種方法的其中一種將應用程式移到主控系統，例如手動複製、Xcopy、Robocopy 或 PowerShell。</span><span class="sxs-lookup"><span data-stu-id="0eacb-353">Use any of several methods to move the app to the hosting system, such as manual copy, Xcopy, Robocopy, or PowerShell.</span></span>

<span data-ttu-id="0eacb-354">如需 IIS 的 ASP.NET Core 部署詳細資訊，請參閱 [IIS 系統管理員的部署資源](#deployment-resources-for-iis-administrators)一節。</span><span class="sxs-lookup"><span data-stu-id="0eacb-354">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="0eacb-355">瀏覽網站</span><span class="sxs-lookup"><span data-stu-id="0eacb-355">Browse the website</span></span>

![Microsoft Edge 瀏覽器已載入 IIS 啟動頁面。](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="0eacb-357">已鎖定的部署檔案</span><span class="sxs-lookup"><span data-stu-id="0eacb-357">Locked deployment files</span></span>

<span data-ttu-id="0eacb-358">當應用程式執行時，會鎖定部署資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="0eacb-358">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="0eacb-359">無法於部署期間覆寫已鎖定的檔案。</span><span class="sxs-lookup"><span data-stu-id="0eacb-359">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="0eacb-360">若要釋放部署中的已鎖定檔案，請使用下列其中**一種**方法停止應用程式集區：</span><span class="sxs-lookup"><span data-stu-id="0eacb-360">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="0eacb-361">使用 Web Deploy 並參考專案檔中的 `Microsoft.NET.Sdk.Web`。</span><span class="sxs-lookup"><span data-stu-id="0eacb-361">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="0eacb-362">*app_offline.htm* 檔案是放在 Web 應用程式目錄的根目錄中。</span><span class="sxs-lookup"><span data-stu-id="0eacb-362">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="0eacb-363">當檔案存在時，ASP.NET Core 模組會正常關閉應用程式，並在部署期間提供 *app_offline.htm* 檔案。</span><span class="sxs-lookup"><span data-stu-id="0eacb-363">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="0eacb-364">如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-364">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="0eacb-365">在伺服器上的 IIS 管理員中手動停止應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="0eacb-365">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="0eacb-366">使用 PowerShell 停止和重新啟動應用程式集區 (需要 PowerShell 5 或更新版本)：</span><span class="sxs-lookup"><span data-stu-id="0eacb-366">Use PowerShell to stop and restart the app pool (requires PowerShell 5 or later):</span></span>

  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

## <a name="data-protection"></a><span data-ttu-id="0eacb-367">資料保護</span><span class="sxs-lookup"><span data-stu-id="0eacb-367">Data protection</span></span>

<span data-ttu-id="0eacb-368">[ASP.NET Core 資料保護堆疊](xref:security/data-protection/introduction)是由數個 ASP.NET Core [中介軟體](xref:fundamentals/middleware/index)所使用，包括用於驗證的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="0eacb-368">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="0eacb-369">即使資料保護 API 不是由使用者程式碼呼叫，仍應使用部署指令碼或是在使用者程式碼中設定資料保護，以建立持續性的密碼編譯[金鑰存放區](xref:security/data-protection/implementation/key-management)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-369">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="0eacb-370">如不設定資料保護，金鑰會保留在記憶體中，並於應用程式重新啟動時捨棄。</span><span class="sxs-lookup"><span data-stu-id="0eacb-370">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="0eacb-371">如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：</span><span class="sxs-lookup"><span data-stu-id="0eacb-371">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="0eacb-372">所有以 Cookie 為基礎的驗證權杖都會失效。</span><span class="sxs-lookup"><span data-stu-id="0eacb-372">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="0eacb-373">當使用者提出下一個要求時，需要再次登入。</span><span class="sxs-lookup"><span data-stu-id="0eacb-373">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="0eacb-374">所有以 Keyring 保護的資料都無法再解密。</span><span class="sxs-lookup"><span data-stu-id="0eacb-374">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="0eacb-375">這可能會包含 [CSRF 權杖](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)和 [ASP.NET Core MVC TempData cookie](xref:fundamentals/app-state#tempdata)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-375">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="0eacb-376">若要在 IIS 下設定資料保護以保存 Keyring，請使用下列其中**一種**方法：</span><span class="sxs-lookup"><span data-stu-id="0eacb-376">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="0eacb-377">**建立資料保護登錄機碼**</span><span class="sxs-lookup"><span data-stu-id="0eacb-377">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="0eacb-378">ASP.NET Core 應用程式所使用的資料保護金鑰會儲存在應用程式外部的登錄中。</span><span class="sxs-lookup"><span data-stu-id="0eacb-378">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="0eacb-379">若要保存指定應用程式的金鑰，請為應用程式集區建立登錄機碼。</span><span class="sxs-lookup"><span data-stu-id="0eacb-379">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="0eacb-380">若為獨立的非Web 伺服陣列 IIS 安裝，請針對搭配使用 ASP.NET Core 應用程式的每個應用程式集區，使用[資料保護 Provision-AutoGenKeys.ps1 PowerShell 指令碼](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-380">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="0eacb-381">此指令碼會在 HKLM 登錄中建立登錄機碼，只有應用程式之應用程式集區的背景工作處理序帳戶可以存取它。</span><span class="sxs-lookup"><span data-stu-id="0eacb-381">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="0eacb-382">在待用期間使用 DPAPI 和全電腦金鑰加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="0eacb-382">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="0eacb-383">在 Web 伺服陣列案例中，應用程式可以設定成使用 UNC 路徑來儲存其資料保護 Keyring。</span><span class="sxs-lookup"><span data-stu-id="0eacb-383">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="0eacb-384">根據預設，資料保護金鑰不予加密。</span><span class="sxs-lookup"><span data-stu-id="0eacb-384">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="0eacb-385">請確保網路共用的檔案權限僅限於執行應用程式的 Windows 帳戶。</span><span class="sxs-lookup"><span data-stu-id="0eacb-385">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="0eacb-386">可以使用 X509 憑證來保護待用的金鑰。</span><span class="sxs-lookup"><span data-stu-id="0eacb-386">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="0eacb-387">請考慮下列讓使用者上傳憑證的機制：將憑證放入使用者的受信任憑證存放區，並確保在執行使用者應用程式的所有電腦上都能使用這些憑證。</span><span class="sxs-lookup"><span data-stu-id="0eacb-387">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="0eacb-388">如需詳細資訊，請參閱[設定 ASP.NET Core 資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-388">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="0eacb-389">**設定 IIS 應用程式集區載入使用者設定檔**</span><span class="sxs-lookup"><span data-stu-id="0eacb-389">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="0eacb-390">此設定位在應用程式集區 [進階設定] 下的 [處理序模型] 區段中。</span><span class="sxs-lookup"><span data-stu-id="0eacb-390">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="0eacb-391">將 [載入使用者設定檔] 設為 `True`。</span><span class="sxs-lookup"><span data-stu-id="0eacb-391">Set Load User Profile to `True`.</span></span> <span data-ttu-id="0eacb-392">這會將金鑰儲存在使用者設定檔目錄下，並使用 DPAPI 與應用程式集區所用的特定使用者帳戶金鑰加以保護。</span><span class="sxs-lookup"><span data-stu-id="0eacb-392">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used by the app pool.</span></span>

* <span data-ttu-id="0eacb-393">**將檔案系統當作 Keyring 存放區使用**</span><span class="sxs-lookup"><span data-stu-id="0eacb-393">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="0eacb-394">調整應用程式程式碼，以[將檔案系統當作 Keyring 存放區使用](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-394">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="0eacb-395">使用 X509 憑證來保護 Keyring，並確保憑證是受信任的憑證。</span><span class="sxs-lookup"><span data-stu-id="0eacb-395">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="0eacb-396">如果憑證為自我簽署，請將憑證放在受信任的根存放區。</span><span class="sxs-lookup"><span data-stu-id="0eacb-396">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="0eacb-397">在 Web 伺服陣列中使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="0eacb-397">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="0eacb-398">請使用所有電腦都可以存取的檔案共用。</span><span class="sxs-lookup"><span data-stu-id="0eacb-398">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="0eacb-399">將 X509 憑證部署到每一部電腦。</span><span class="sxs-lookup"><span data-stu-id="0eacb-399">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="0eacb-400">設定[程式碼中的資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-400">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="0eacb-401">**設定資料保護的全電腦原則**</span><span class="sxs-lookup"><span data-stu-id="0eacb-401">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="0eacb-402">針對取用資料保護 API 的所有應用程式，資料保護系統僅支援有限的預設[全電腦原則](xref:security/data-protection/configuration/machine-wide-policy)設定。</span><span class="sxs-lookup"><span data-stu-id="0eacb-402">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="0eacb-403">如需詳細資訊，請參閱<xref:security/data-protection/introduction>。</span><span class="sxs-lookup"><span data-stu-id="0eacb-403">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="sub-application-configuration"></a><span data-ttu-id="0eacb-404">子應用程式設定</span><span class="sxs-lookup"><span data-stu-id="0eacb-404">Sub-application configuration</span></span>

<span data-ttu-id="0eacb-405">根應用程式下新增的子應用程式不應包含當作處理常式的 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="0eacb-405">Sub-apps added under the root app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="0eacb-406">如果您在子應用程式的 *web.config* 檔案中將模組新增為處理常式，則嘗試瀏覽子應用程式時，會收到參考錯誤設定檔的 *500.19 內部伺服器錯誤*。</span><span class="sxs-lookup"><span data-stu-id="0eacb-406">If the module is added as a handler in a sub-app's *web.config* file, a *500.19 Internal Server Error* referencing the faulty config file is received when attempting to browse the sub-app.</span></span>

<span data-ttu-id="0eacb-407">下列範例顯示 ASP.NET Core 子應用程式的已發佈 *web.config* 檔案：</span><span class="sxs-lookup"><span data-stu-id="0eacb-407">The following example shows a published *web.config* file for an ASP.NET Core sub-app:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="0eacb-408">在 ASP.NET Core 應用程式下裝載非 ASP.NET Core 子應用程式時，請明確移除子應用程式 *web.config* 檔案中已繼承的處理常式：</span><span class="sxs-lookup"><span data-stu-id="0eacb-408">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore" />
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="0eacb-409">如需設定 ASP.NET Core 模組的詳細資訊，請參閱 [ASP.NET Core 模組簡介](xref:fundamentals/servers/aspnet-core-module)主題和 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-409">For more information on configuring the ASP.NET Core Module, see the [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic and the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="0eacb-410">使用 web.config 的 IIS 組態</span><span class="sxs-lookup"><span data-stu-id="0eacb-410">Configuration of IIS with web.config</span></span>

<span data-ttu-id="0eacb-411">IIS 組態受到這些適用於反向 Proxy 組態之 IIS 功能 *web.config* 的 **\<system.webServer>** 區段所影響。</span><span class="sxs-lookup"><span data-stu-id="0eacb-411">IIS configuration is influenced by the **\<system.webServer>** section of *web.config* for those IIS features that apply to a reverse proxy configuration.</span></span> <span data-ttu-id="0eacb-412">如果在伺服器層級設定 IIS 使用動態壓縮，則在應用程式 *web.config* 檔案中的 **\<urlCompression>** 元素將可停用它。</span><span class="sxs-lookup"><span data-stu-id="0eacb-412">If IIS is configured at the server level to use dynamic compression, the **\<urlCompression>** element in the app's *web.config* file can disable it.</span></span>

<span data-ttu-id="0eacb-413">如需詳細資訊，請參閱 [\<system.webServer> 的設定參考](/iis/configuration/system.webServer/)、[ASP.NET Core 模組設定參考](xref:host-and-deploy/aspnet-core-module)以及 [IIS 模組與 ASP.NET Core](xref:host-and-deploy/iis/modules)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-413">For more information, see the [configuration reference for \<system.webServer>](/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module), and [IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="0eacb-414">若要設定在隔離的應用程式集區中執行之個別應用程式的環境變數 (支援 IIS 10.0 或更新版本)，請參閱 IIS 參考文件之[環境變數 \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的 *AppCmd.exe 命令*一節。</span><span class="sxs-lookup"><span data-stu-id="0eacb-414">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="0eacb-415">web.config 的組態區段</span><span class="sxs-lookup"><span data-stu-id="0eacb-415">Configuration sections of web.config</span></span>

<span data-ttu-id="0eacb-416">ASP.NET Core 應用程式的設定不使用 *web.config* 中 ASP.NET 4.x 應用程式的設定區段：</span><span class="sxs-lookup"><span data-stu-id="0eacb-416">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* <span data-ttu-id="0eacb-417">**\<system.web>**</span><span class="sxs-lookup"><span data-stu-id="0eacb-417">**\<system.web>**</span></span>
* <span data-ttu-id="0eacb-418">**\<appSettings>**</span><span class="sxs-lookup"><span data-stu-id="0eacb-418">**\<appSettings>**</span></span>
* <span data-ttu-id="0eacb-419">**\<connectionStrings>**</span><span class="sxs-lookup"><span data-stu-id="0eacb-419">**\<connectionStrings>**</span></span>
* <span data-ttu-id="0eacb-420">**\<位置>**</span><span class="sxs-lookup"><span data-stu-id="0eacb-420">**\<location>**</span></span>

<span data-ttu-id="0eacb-421">使用其他組態提供者設定的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0eacb-421">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="0eacb-422">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="0eacb-422">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="0eacb-423">應用程式集區</span><span class="sxs-lookup"><span data-stu-id="0eacb-423">Application Pools</span></span>

<span data-ttu-id="0eacb-424">在伺服器上裝載多個網站時，建議您在其各自的應用程式集區中執行各個應用程式，讓應用程式彼此隔離。</span><span class="sxs-lookup"><span data-stu-id="0eacb-424">When hosting multiple websites on a server, we recommend isolating the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="0eacb-425">IIS [新增網站] 對話方塊預設成此組態。</span><span class="sxs-lookup"><span data-stu-id="0eacb-425">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="0eacb-426">當您提供**網站名稱**時，文字會自動轉移至 [應用程式集區] 文字方塊。</span><span class="sxs-lookup"><span data-stu-id="0eacb-426">When **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="0eacb-427">新增網站時，會使用該網站名稱建立新的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="0eacb-427">A new app pool is created using the site name when the site is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="0eacb-428">應用程式集區身分識別</span><span class="sxs-lookup"><span data-stu-id="0eacb-428">Application Pool Identity</span></span>

<span data-ttu-id="0eacb-429">應用程式集區身分識別帳戶可讓應用程式在唯一的帳戶下執行，不必建立及管理網域或本機帳戶。</span><span class="sxs-lookup"><span data-stu-id="0eacb-429">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="0eacb-430">在 IIS 8.0 或更新版本中，IIS 管理背景工作處理序 (WAS) 會使用新的應用程式集區名稱建立虛擬帳戶，並預設在此帳戶下執行應用程式集區的背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="0eacb-430">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="0eacb-431">在 IIS 管理主控台中，於應用程式集區的 [進階設定] 下，確定 [身分識別] 設定為使用 **ApplicationPoolIdentity**：</span><span class="sxs-lookup"><span data-stu-id="0eacb-431">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![應用程式集區進階設定對話方塊](index/_static/apppool-identity.png)

<span data-ttu-id="0eacb-433">IIS 管理程序會在 Windows 安全系統中，以應用程式集區的名稱建立安全識別碼。</span><span class="sxs-lookup"><span data-stu-id="0eacb-433">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="0eacb-434">可使用此身分識別來保護資源。</span><span class="sxs-lookup"><span data-stu-id="0eacb-434">Resources can be secured using this identity.</span></span> <span data-ttu-id="0eacb-435">不過，此身分識別不是真正的使用者帳戶，也不會顯示在 Windows 使用者管理主控台中。</span><span class="sxs-lookup"><span data-stu-id="0eacb-435">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="0eacb-436">如果 IIS 背景工作處理序需要提升應用程式的存取權限，請修改包含應用程式的目錄存取控制清單 (ACL)：</span><span class="sxs-lookup"><span data-stu-id="0eacb-436">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="0eacb-437">開啟 Windows 檔案總管，巡覽至目錄。</span><span class="sxs-lookup"><span data-stu-id="0eacb-437">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="0eacb-438">以滑鼠右鍵按一下目錄並選取 [屬性]。</span><span class="sxs-lookup"><span data-stu-id="0eacb-438">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="0eacb-439">依序選取 [安全性] 索引標籤下的 [編輯] 按鈕和 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="0eacb-439">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="0eacb-440">選取 [位置] 按鈕，並確定選取系統。</span><span class="sxs-lookup"><span data-stu-id="0eacb-440">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="0eacb-441">在 [輸入要選取的物件名稱] 區域中，輸入 **IIS AppPool\\<app_pool_name>**。</span><span class="sxs-lookup"><span data-stu-id="0eacb-441">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="0eacb-442">選取 [檢查名稱] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="0eacb-442">Select the **Check Names** button.</span></span> <span data-ttu-id="0eacb-443">針對 [DefaultAppPool]，請使用 **IIS AppPool\DefaultAppPool** 檢查名稱。</span><span class="sxs-lookup"><span data-stu-id="0eacb-443">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="0eacb-444">選取 [檢查名稱] 按鈕時，[DefaultAppPool] 的值便會顯示於物件名稱區域中。</span><span class="sxs-lookup"><span data-stu-id="0eacb-444">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="0eacb-445">您無法直接將應用程式集區名稱輸入至物件名稱區域。</span><span class="sxs-lookup"><span data-stu-id="0eacb-445">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="0eacb-446">檢查物件名稱時，請使用 **IIS AppPool\\<app_pool_name>** 的格式。</span><span class="sxs-lookup"><span data-stu-id="0eacb-446">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：在選取 [檢查名稱] 之前，"DefaultAppPool" 這個應用程式集區名稱在物件名稱區域中會附加至 "IIS AppPool\"。](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="0eacb-448">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="0eacb-448">Select **OK**.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：選取 [檢查名稱] 之後，物件名稱 "DefaultAppPool" 會顯示在物件名稱區域中。](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="0eacb-450">預設應該會授與讀取 &amp; 執行權限。</span><span class="sxs-lookup"><span data-stu-id="0eacb-450">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="0eacb-451">請視需要提供其他權限。</span><span class="sxs-lookup"><span data-stu-id="0eacb-451">Provide additional permissions as needed.</span></span>

<span data-ttu-id="0eacb-452">也可使用 **ICACLS** 工具透過命令提示字元授與存取權限。</span><span class="sxs-lookup"><span data-stu-id="0eacb-452">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="0eacb-453">使用 *DefaultAppPool*作為範例，將會使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="0eacb-453">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="0eacb-454">如需詳細資訊，請參閱 [icacls](/windows-server/administration/windows-commands/icacls) 主題。</span><span class="sxs-lookup"><span data-stu-id="0eacb-454">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="0eacb-455">IIS 系統管理員的部署資源</span><span class="sxs-lookup"><span data-stu-id="0eacb-455">Deployment resources for IIS administrators</span></span>

<span data-ttu-id="0eacb-456">請參閱 IIS 文件以深入了解 IIS。</span><span class="sxs-lookup"><span data-stu-id="0eacb-456">Learn about IIS in-depth in the IIS documentation.</span></span>  
[<span data-ttu-id="0eacb-457">IIS 文件</span><span class="sxs-lookup"><span data-stu-id="0eacb-457">IIS documentation</span></span>](/iis)

<span data-ttu-id="0eacb-458">了解 .NET Core 應用程式部署模型。</span><span class="sxs-lookup"><span data-stu-id="0eacb-458">Learn about .NET Core app deployment models.</span></span>  
[<span data-ttu-id="0eacb-459">.NET Core 應用程式部署</span><span class="sxs-lookup"><span data-stu-id="0eacb-459">.NET Core application deployment</span></span>](/dotnet/core/deploying/)

<span data-ttu-id="0eacb-460">了解 ASP.NET Core 模組如何讓 Kestrel Web 伺服器將 IIS 或 IIS Express 作為反向 Proxy 伺服器使用。</span><span class="sxs-lookup"><span data-stu-id="0eacb-460">Learn how the ASP.NET Core Module allows the Kestrel web server to use IIS or IIS Express as a reverse proxy server.</span></span>  
[<span data-ttu-id="0eacb-461">ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="0eacb-461">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)

<span data-ttu-id="0eacb-462">了解如何設定 ASP.NET Core 模組以裝載 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0eacb-462">Learn how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span>  
[<span data-ttu-id="0eacb-463">ASP.NET Core 模組組態參考</span><span class="sxs-lookup"><span data-stu-id="0eacb-463">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)

<span data-ttu-id="0eacb-464">了解已發行之 ASP.NET Core 應用程式的目錄結構。</span><span class="sxs-lookup"><span data-stu-id="0eacb-464">Learn about the directory structure of published ASP.NET Core apps.</span></span>  
[<span data-ttu-id="0eacb-465">目錄結構</span><span class="sxs-lookup"><span data-stu-id="0eacb-465">Directory structure</span></span>](xref:host-and-deploy/directory-structure)

<span data-ttu-id="0eacb-466">探索 ASP.NET Core 應用程式的使用中和非使用中 IIS 模組，管理 IIS 模組的方式。</span><span class="sxs-lookup"><span data-stu-id="0eacb-466">Discover active and inactive IIS modules for ASP.NET Core apps and how to manage IIS modules.</span></span>  
[<span data-ttu-id="0eacb-467">IIS 模組</span><span class="sxs-lookup"><span data-stu-id="0eacb-467">IIS modules</span></span>](xref:host-and-deploy/iis/troubleshoot)

<span data-ttu-id="0eacb-468">了解如何診斷 ASP.NET Core 應用程式的 IIS 部署問題。</span><span class="sxs-lookup"><span data-stu-id="0eacb-468">Learn how to diagnose problems with IIS deployments of ASP.NET Core apps.</span></span>  
[<span data-ttu-id="0eacb-469">疑難排解</span><span class="sxs-lookup"><span data-stu-id="0eacb-469">Troubleshoot</span></span>](xref:host-and-deploy/iis/troubleshoot)

<span data-ttu-id="0eacb-470">區分在 IIS 上裝載 ASP.NET Core 應用程式時的常見錯誤。</span><span class="sxs-lookup"><span data-stu-id="0eacb-470">Distinguish common errors when hosting ASP.NET Core apps on IIS.</span></span>  
[<span data-ttu-id="0eacb-471">Azure App Service 和 IIS 常見的錯誤參考</span><span class="sxs-lookup"><span data-stu-id="0eacb-471">Common errors reference for Azure App Service and IIS</span></span>](xref:host-and-deploy/azure-iis-errors-reference)

## <a name="additional-resources"></a><span data-ttu-id="0eacb-472">其他資源</span><span class="sxs-lookup"><span data-stu-id="0eacb-472">Additional resources</span></span>

* [<span data-ttu-id="0eacb-473">ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="0eacb-473">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="0eacb-474">Microsoft IIS 官方網站</span><span class="sxs-lookup"><span data-stu-id="0eacb-474">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="0eacb-475">Windows Server 技術內容庫</span><span class="sxs-lookup"><span data-stu-id="0eacb-475">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="0eacb-476">ISS 上的 HTTP/2</span><span class="sxs-lookup"><span data-stu-id="0eacb-476">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
