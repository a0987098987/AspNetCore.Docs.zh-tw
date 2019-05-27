---
title: 在使用 IIS 的 Windows 上裝載 ASP.NET Core
author: guardrex
description: 了解如何在 Windows Server Internet Information Services (IIS) 上裝載 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/24/2019
uid: host-and-deploy/iis/index
ms.openlocfilehash: f0efe6c0edc71c5e2c45aeaa175c8a5643ef0fde
ms.sourcegitcommit: e1623d8279b27ff83d8ad67a1e7ef439259decdf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/25/2019
ms.locfileid: "66223137"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="79f54-103">在使用 IIS 的 Windows 上裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="79f54-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="79f54-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="79f54-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[<span data-ttu-id="79f54-105">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="79f54-105">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="79f54-106">支援的作業系統</span><span class="sxs-lookup"><span data-stu-id="79f54-106">Supported operating systems</span></span>

<span data-ttu-id="79f54-107">支援下列作業系統：</span><span class="sxs-lookup"><span data-stu-id="79f54-107">The following operating systems are supported:</span></span>

* <span data-ttu-id="79f54-108">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="79f54-108">Windows 7 or later</span></span>
* <span data-ttu-id="79f54-109">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="79f54-109">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="79f54-110">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys) (先前稱為 WebListener) 不適用搭配 IIS 的反向 Proxy 設定。</span><span class="sxs-lookup"><span data-stu-id="79f54-110">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="79f54-111">請使用 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="79f54-111">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="79f54-112">如需在 Azure 中裝載的資訊，請參閱 <xref:host-and-deploy/azure-apps/index>。</span><span class="sxs-lookup"><span data-stu-id="79f54-112">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="79f54-113">支援的平台</span><span class="sxs-lookup"><span data-stu-id="79f54-113">Supported platforms</span></span>

<span data-ttu-id="79f54-114">支援針對 32 位元 (x86) 或 64 位元 (x64) 部署發行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="79f54-114">Apps published for 32-bit (x86) or 64-bit (x64) deployment are supported.</span></span> <span data-ttu-id="79f54-115">使用 32 位元 (x86) .NET Core SDK 部署 32 位元應用程式，除非應用程式：</span><span class="sxs-lookup"><span data-stu-id="79f54-115">Deploy a 32-bit app with a 32-bit (x86) .NET Core SDK unless the app:</span></span>

* <span data-ttu-id="79f54-116">需要提供給 64 位元應用程式使用的較大虛擬記憶體位址空間。</span><span class="sxs-lookup"><span data-stu-id="79f54-116">Requires the larger virtual memory address space available to a 64-bit app.</span></span>
* <span data-ttu-id="79f54-117">需要較大的 IIS 堆疊大小。</span><span class="sxs-lookup"><span data-stu-id="79f54-117">Requires the larger IIS stack size.</span></span>
* <span data-ttu-id="79f54-118">有 64 位元原生相依性。</span><span class="sxs-lookup"><span data-stu-id="79f54-118">Has 64-bit native dependencies.</span></span>

<span data-ttu-id="79f54-119">使用 64 位元 (x64) .NET Core SDK 來發行 64 位元應用程式。</span><span class="sxs-lookup"><span data-stu-id="79f54-119">Use a 64-bit (x64) .NET Core SDK to publish a 64-bit app.</span></span> <span data-ttu-id="79f54-120">主機系統上必須有 64 位元執行階段存在。</span><span class="sxs-lookup"><span data-stu-id="79f54-120">A 64-bit runtime must be present on the host system.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="79f54-121">應用程式組態</span><span class="sxs-lookup"><span data-stu-id="79f54-121">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="79f54-122">啟用 IISIntegration 元件</span><span class="sxs-lookup"><span data-stu-id="79f54-122">Enable the IISIntegration components</span></span>

<span data-ttu-id="79f54-123">一般的 *Program.cs* 會呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 以開始設定主機：</span><span class="sxs-lookup"><span data-stu-id="79f54-123">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="79f54-124">**同處理序主控模型**</span><span class="sxs-lookup"><span data-stu-id="79f54-124">**In-process hosting model**</span></span>

<span data-ttu-id="79f54-125">`CreateDefaultBuilder` 會呼叫 `UseIIS` 方法來啟動 [CoreCLR](/dotnet/standard/glossary#coreclr)，並在 IIS 工作者處理序 (*w3wp.exe* 或 *iisexpress.exe*) 內裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="79f54-125">`CreateDefaultBuilder` calls the `UseIIS` method to boot the [CoreCLR](/dotnet/standard/glossary#coreclr) and host the app inside of the IIS worker process (*w3wp.exe* or *iisexpress.exe*).</span></span> <span data-ttu-id="79f54-126">效能測試指出，相較於跨處理序裝載 .NET Core 應用程式，並將要求 Proxy 處理至 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器，裝載於同處理序可提供明顯更高的要求輸送量。</span><span class="sxs-lookup"><span data-stu-id="79f54-126">Performance tests indicate that hosting a .NET Core app in-process delivers significantly higher request throughput compared to hosting the app out-of-process and proxying requests to [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="79f54-127">以 .NET Framework 為目標的 ASP.NET Core 應用程式不支援處理序內裝載模型。</span><span class="sxs-lookup"><span data-stu-id="79f54-127">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="79f54-128">**跨處理序裝載模型**</span><span class="sxs-lookup"><span data-stu-id="79f54-128">**Out-of-process hosting model**</span></span>

<span data-ttu-id="79f54-129">若是使用 IIS 的跨處理序裝載，`CreateDefaultBuilder` 會將 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器設為網頁伺服器，並設定 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)的基底路徑與連接埠來啟用 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="79f54-129">For out-of-process hosting with IIS, `CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and enables IIS Integration by configuring the base path and port for the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="79f54-130">ASP.NET Core 模組會產生要指派給後端處理序的動態連接埠。</span><span class="sxs-lookup"><span data-stu-id="79f54-130">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="79f54-131">`CreateDefaultBuilder` 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 方法。</span><span class="sxs-lookup"><span data-stu-id="79f54-131">`CreateDefaultBuilder` calls the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> method.</span></span> <span data-ttu-id="79f54-132">`UseIISIntegration` 會將 Kestrel 設定為在位於 localhost IP 位址 (`127.0.0.1`) 的動態連接埠上接聽。</span><span class="sxs-lookup"><span data-stu-id="79f54-132">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="79f54-133">若動態連接埠是 1234，Kestrel 會在 `127.0.0.1:1234` 接聽。</span><span class="sxs-lookup"><span data-stu-id="79f54-133">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="79f54-134">此設定會取代由下列項目提供的其他 URL 設定：</span><span class="sxs-lookup"><span data-stu-id="79f54-134">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="79f54-135">Kestrel 的接聽 API</span><span class="sxs-lookup"><span data-stu-id="79f54-135">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="79f54-136">[設定](xref:fundamentals/configuration/index) (或[命令列 --urls 選項](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="79f54-136">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="79f54-137">在使用模組時不需要呼叫 `UseUrls` 或 Kestrel 的 `Listen` API。</span><span class="sxs-lookup"><span data-stu-id="79f54-137">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="79f54-138">若呼叫 `UseUrls` 或 `Listen`，Kestrel 只會接聽未使用 IIS 執行應用程式時指定的連接埠。</span><span class="sxs-lookup"><span data-stu-id="79f54-138">If `UseUrls` or `Listen` is called, Kestrel listens on the ports specified only when running the app without IIS.</span></span>

<span data-ttu-id="79f54-139">如需處理序內和處理序外裝載模型的詳細資訊，請參閱 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)及 [ASP.NET Core 模組設定參考](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="79f54-139">For more information on the in-process and out-of-process hosting models, see [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="79f54-140">`CreateDefaultBuilder` 會將 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器設為網頁伺服器，並設定 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)的基底路徑與連接埠來啟用 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="79f54-140">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and enables IIS Integration by configuring the base path and port for the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="79f54-141">ASP.NET Core 模組會產生要指派給後端處理序的動態連接埠。</span><span class="sxs-lookup"><span data-stu-id="79f54-141">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="79f54-142">`CreateDefaultBuilder` 會呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 方法。</span><span class="sxs-lookup"><span data-stu-id="79f54-142">`CreateDefaultBuilder` calls the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> method.</span></span> <span data-ttu-id="79f54-143">`UseIISIntegration` 會將 Kestrel 設定為在位於 localhost IP 位址 (`127.0.0.1`) 的動態連接埠上接聽。</span><span class="sxs-lookup"><span data-stu-id="79f54-143">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="79f54-144">若動態連接埠是 1234，Kestrel 會在 `127.0.0.1:1234` 接聽。</span><span class="sxs-lookup"><span data-stu-id="79f54-144">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="79f54-145">此設定會取代由下列項目提供的其他 URL 設定：</span><span class="sxs-lookup"><span data-stu-id="79f54-145">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="79f54-146">Kestrel 的接聽 API</span><span class="sxs-lookup"><span data-stu-id="79f54-146">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="79f54-147">[設定](xref:fundamentals/configuration/index) (或[命令列 --urls 選項](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="79f54-147">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="79f54-148">在使用模組時不需要呼叫 `UseUrls` 或 Kestrel 的 `Listen` API。</span><span class="sxs-lookup"><span data-stu-id="79f54-148">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="79f54-149">若呼叫 `UseUrls` 或 `Listen`，Kestrel 只會接聽未使用 IIS 執行應用程式時指定的連接埠。</span><span class="sxs-lookup"><span data-stu-id="79f54-149">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

::: moniker-end

<span data-ttu-id="79f54-150">如需代管的詳細資訊，請參閱[在 ASP.NET Core 中代管](xref:fundamentals/index#host)。</span><span class="sxs-lookup"><span data-stu-id="79f54-150">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/index#host).</span></span>

### <a name="iis-options"></a><span data-ttu-id="79f54-151">IIS 選項</span><span class="sxs-lookup"><span data-stu-id="79f54-151">IIS options</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="79f54-152">**同處理序主控模型**</span><span class="sxs-lookup"><span data-stu-id="79f54-152">**In-process hosting model**</span></span>

<span data-ttu-id="79f54-153">若要設定 IIS 伺服器選項，請在 <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 中加入 <xref:Microsoft.AspNetCore.Builder.IISServerOptions> 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="79f54-153">To configure IIS Server options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISServerOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="79f54-154">下列範例會停用 AutomaticAuthentication：</span><span class="sxs-lookup"><span data-stu-id="79f54-154">The following example disables AutomaticAuthentication:</span></span>

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="79f54-155">選項</span><span class="sxs-lookup"><span data-stu-id="79f54-155">Option</span></span>                         | <span data-ttu-id="79f54-156">預設</span><span class="sxs-lookup"><span data-stu-id="79f54-156">Default</span></span> | <span data-ttu-id="79f54-157">設定</span><span class="sxs-lookup"><span data-stu-id="79f54-157">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="79f54-158">若為 `true`，IIS 伺服器會設定由 [Windows 驗證](xref:security/authentication/windowsauth)所驗證的 `HttpContext.User`。</span><span class="sxs-lookup"><span data-stu-id="79f54-158">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="79f54-159">若為 `false`，則伺服器僅會對 `HttpContext.User` 提供身分識別，並在 `AuthenticationScheme` 明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="79f54-159">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="79f54-160">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="79f54-160">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="79f54-161">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="79f54-161">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="79f54-162">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="79f54-162">Sets the display name shown to users on login pages.</span></span> |
| `AllowSynchronousIO`           | `false` | <span data-ttu-id="79f54-163">是否要針對 `HttpContext.Request` 和 `HttpContext.Response` 允許同步 IO。</span><span class="sxs-lookup"><span data-stu-id="79f54-163">Whether synchronous IO is allowed for the `HttpContext.Request` and the `HttpContext.Response`.</span></span> |
| `MaxRequestBodySize`           | `30000000`  | <span data-ttu-id="79f54-164">取得或設定 `HttpRequest` 的要求本文大小上限。</span><span class="sxs-lookup"><span data-stu-id="79f54-164">Gets or sets the the max request body size for the `HttpRequest`.</span></span> <span data-ttu-id="79f54-165">請注意，IIS 本身具有限制 `maxAllowedContentLength`，此限制將在 `IISServerOptions` 中設定 `MaxRequestBodySize` 時處理。</span><span class="sxs-lookup"><span data-stu-id="79f54-165">Note that IIS itself has the limit `maxAllowedContentLength` which will be processed before the `MaxRequestBodySize` set in the `IISServerOptions`.</span></span> <span data-ttu-id="79f54-166">變更 `MaxRequestBodySize` 將不會影響 `maxAllowedContentLength`。</span><span class="sxs-lookup"><span data-stu-id="79f54-166">Changing the `MaxRequestBodySize` won't affect the `maxAllowedContentLength`.</span></span> <span data-ttu-id="79f54-167">若要增加 `maxAllowedContentLength`，請在 *web.config* 中新增項目，以將 `maxAllowedContentLength` 設定為較高的值。</span><span class="sxs-lookup"><span data-stu-id="79f54-167">To increase `maxAllowedContentLength`, add an entry in the *web.config* to set `maxAllowedContentLength` to a higher value.</span></span> <span data-ttu-id="79f54-168">如需更多詳細資料，請參閱[組態](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/#configuration)。</span><span class="sxs-lookup"><span data-stu-id="79f54-168">For more details, see [Configuration](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/#configuration).</span></span> |

<span data-ttu-id="79f54-169">**跨處理序裝載模型**</span><span class="sxs-lookup"><span data-stu-id="79f54-169">**Out-of-process hosting model**</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="79f54-170">選項</span><span class="sxs-lookup"><span data-stu-id="79f54-170">Option</span></span>                         | <span data-ttu-id="79f54-171">預設</span><span class="sxs-lookup"><span data-stu-id="79f54-171">Default</span></span> | <span data-ttu-id="79f54-172">設定</span><span class="sxs-lookup"><span data-stu-id="79f54-172">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="79f54-173">若為 `true`，IIS 伺服器會設定由 [Windows 驗證](xref:security/authentication/windowsauth)所驗證的 `HttpContext.User`。</span><span class="sxs-lookup"><span data-stu-id="79f54-173">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="79f54-174">若為 `false`，則伺服器僅會對 `HttpContext.User` 提供身分識別，並在 `AuthenticationScheme` 明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="79f54-174">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="79f54-175">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="79f54-175">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="79f54-176">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="79f54-176">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="79f54-177">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="79f54-177">Sets the display name shown to users on login pages.</span></span> |

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="79f54-178">**跨處理序裝載模型**</span><span class="sxs-lookup"><span data-stu-id="79f54-178">**Out-of-process hosting model**</span></span>

::: moniker-end

<span data-ttu-id="79f54-179">若要設定 IIS 選項，請在 <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 中加入 <xref:Microsoft.AspNetCore.Builder.IISOptions> 的服務設定。</span><span class="sxs-lookup"><span data-stu-id="79f54-179">To configure IIS options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="79f54-180">下列範例會防止應用程式填入 `HttpContext.Connection.ClientCertificate`：</span><span class="sxs-lookup"><span data-stu-id="79f54-180">The following example prevents the app from populating `HttpContext.Connection.ClientCertificate`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="79f54-181">選項</span><span class="sxs-lookup"><span data-stu-id="79f54-181">Option</span></span>                         | <span data-ttu-id="79f54-182">預設</span><span class="sxs-lookup"><span data-stu-id="79f54-182">Default</span></span> | <span data-ttu-id="79f54-183">設定</span><span class="sxs-lookup"><span data-stu-id="79f54-183">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="79f54-184">若為 `true`，IIS 整合中介軟體會設定由 [Windows Authentication](xref:security/authentication/windowsauth) 所驗證的 `HttpContext.User`。</span><span class="sxs-lookup"><span data-stu-id="79f54-184">If `true`, IIS Integration Middleware sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="79f54-185">如果為 `false`，則驗證中介軟體僅針對 `HttpContext.User` 提供身分識別，並在游 `AuthenticationScheme` 提出明確要求時回應挑戰。</span><span class="sxs-lookup"><span data-stu-id="79f54-185">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="79f54-186">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="79f54-186">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="79f54-187">如需詳細資訊，請參閱 [Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="79f54-187">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="79f54-188">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="79f54-188">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="79f54-189">如果為 `true` 且 `MS-ASPNETCORE-CLIENTCERT` 要求標頭已存在，則會填入 `HttpContext.Connection.ClientCertificate`。</span><span class="sxs-lookup"><span data-stu-id="79f54-189">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="79f54-190">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="79f54-190">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="79f54-191">用來設定轉送標頭中介軟體及 ASP.NET Core 模組的 IIS Integration 中介軟體會設定為轉送配置 (HTTP/HTTPS) 及發出要求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="79f54-191">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="79f54-192">其他 Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。</span><span class="sxs-lookup"><span data-stu-id="79f54-192">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="79f54-193">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="79f54-193">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="79f54-194">web.config 檔案</span><span class="sxs-lookup"><span data-stu-id="79f54-194">web.config file</span></span>

<span data-ttu-id="79f54-195">*web.config* 檔案是用來設定 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="79f54-195">The *web.config* file configures the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="79f54-196">發佈專案時，由 MSBuild 目標 (`_TransformWebConfig`) 處理 *web.config* 檔案的建立、轉換及發佈。</span><span class="sxs-lookup"><span data-stu-id="79f54-196">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="79f54-197">此目標存在於 Web SDK 目標 (`Microsoft.NET.Sdk.Web`)。</span><span class="sxs-lookup"><span data-stu-id="79f54-197">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="79f54-198">SDK 設定在專案檔的頂端：</span><span class="sxs-lookup"><span data-stu-id="79f54-198">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="79f54-199">如果專案中沒有 *web.config* 檔案，則系統會使用正確的 *processPath* 和 *arguments* 建立該檔案以設定 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)，並將該檔案移至[已發行的輸出](xref:host-and-deploy/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="79f54-199">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="79f54-200">如果 *web.config* 檔案存在於專案中，則系統會使用正確的 *processPath* 和 *arguments* 來轉換該檔案以設定 ASP.NET Core 模組，然後將它移至已發行的輸出。</span><span class="sxs-lookup"><span data-stu-id="79f54-200">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="79f54-201">轉換不會修改檔案中的 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="79f54-201">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="79f54-202">*web.config* 檔案可提供能控制作用中 IIS 模組的額外 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="79f54-202">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="79f54-203">如需能處理 ASP.NET Core 應用程式要求之 IIS 模組的相關資訊，請參閱 [IIS 模組](xref:host-and-deploy/iis/modules)主題。</span><span class="sxs-lookup"><span data-stu-id="79f54-203">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="79f54-204">為防止 Web SDK 轉換 *web.config* 檔案，請使用專案檔中的 **\<IsTransformWebConfigDisabled>** 屬性：</span><span class="sxs-lookup"><span data-stu-id="79f54-204">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="79f54-205">使 Web SDK 無法轉換檔案時，應該由開發人員手動設定 *processPath* 和 *arguments*。</span><span class="sxs-lookup"><span data-stu-id="79f54-205">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="79f54-206">如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="79f54-206">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="79f54-207">web.config 檔案位置</span><span class="sxs-lookup"><span data-stu-id="79f54-207">web.config file location</span></span>

<span data-ttu-id="79f54-208">為了正確設定 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)，*web.config* 檔案必須存在於已部署應用程式的內容根路徑 (通常是應用程式基底路徑)。</span><span class="sxs-lookup"><span data-stu-id="79f54-208">In order to set up the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) correctly, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="79f54-209">這是與提供給 IIS 的網站實體路徑相同的位置。</span><span class="sxs-lookup"><span data-stu-id="79f54-209">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="79f54-210">應用程式的根目錄需有 *web.config* 檔案，才能使用 Web Deploy 發行多個應用程式。</span><span class="sxs-lookup"><span data-stu-id="79f54-210">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="79f54-211">機密檔案存在於應用程式的實體路徑上，例如 *\<組件>.runtimeconfig.json*、 *\<組件>.xml* (XML 文件註解)，以及 *\<組件>.deps.json*。</span><span class="sxs-lookup"><span data-stu-id="79f54-211">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="79f54-212">當 *web.config* 檔案存在且網站正常啟動時，如果有人要求機密檔案，IIS 不會予以提供。</span><span class="sxs-lookup"><span data-stu-id="79f54-212">When the *web.config* file is present and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="79f54-213">若 *web.config* 檔案遺失或沒有正確命名，或是無法設定網站以正常啟動，IIS 可能會公開提供機密檔案。</span><span class="sxs-lookup"><span data-stu-id="79f54-213">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="79f54-214">***web.config* 檔案必須持續存在於部署之中、已正確命名，並能夠設定網站以正常啟動。無論在任何情況下，請都不要從生產環境部署移除 *web.config* 檔案。**</span><span class="sxs-lookup"><span data-stu-id="79f54-214">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

### <a name="transform-webconfig"></a><span data-ttu-id="79f54-215">轉換 web.config</span><span class="sxs-lookup"><span data-stu-id="79f54-215">Transform web.config</span></span>

<span data-ttu-id="79f54-216">如需在發佈時轉換 *web.config* (例如依據設定、設定檔或環境設定環境變數)，請參閱<xref:host-and-deploy/iis/transform-webconfig>。</span><span class="sxs-lookup"><span data-stu-id="79f54-216">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="79f54-217">IIS 組態</span><span class="sxs-lookup"><span data-stu-id="79f54-217">IIS configuration</span></span>

<span data-ttu-id="79f54-218">**Windows Server 作業系統**</span><span class="sxs-lookup"><span data-stu-id="79f54-218">**Windows Server operating systems**</span></span>

<span data-ttu-id="79f54-219">啟用**網頁伺服器 (IIS)** 伺服器角色，並建立角色服務。</span><span class="sxs-lookup"><span data-stu-id="79f54-219">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="79f54-220">使用來自 [管理] 功能表的 [新增角色及功能] 精靈，或是 [伺服器管理員] 中的連結。</span><span class="sxs-lookup"><span data-stu-id="79f54-220">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="79f54-221">在**伺服器角色**步驟中，核取 [網頁伺服器 (IIS)] 方塊。</span><span class="sxs-lookup"><span data-stu-id="79f54-221">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![在選取伺服器角色步驟中選取網頁伺服器 IIS 角色。](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="79f54-223">在 [功能] 步驟之後，[角色服務] 步驟會針對網頁伺服器 (IIS) 進行載入。</span><span class="sxs-lookup"><span data-stu-id="79f54-223">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="79f54-224">選取所需的 IIS 角色服務或接受所提供的預設角色服務。</span><span class="sxs-lookup"><span data-stu-id="79f54-224">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![在選取角色服務步驟中，選取預設的角色服務。](index/_static/role-services-ws2016.png)

   <span data-ttu-id="79f54-226">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="79f54-226">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="79f54-227">若要啟用 Windows 驗證，請展開下列節點：[網頁伺服器] > [安全性]。</span><span class="sxs-lookup"><span data-stu-id="79f54-227">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="79f54-228">選取 [Windows 驗證] 功能。</span><span class="sxs-lookup"><span data-stu-id="79f54-228">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="79f54-229">如需詳細資訊，請參閱 [Windows 驗證 \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) 和[設定 Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="79f54-229">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="79f54-230">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="79f54-230">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="79f54-231">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="79f54-231">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="79f54-232">若要啟用 WebSockets，請展開下列節點：[網頁伺服器] > [應用程式開發]。</span><span class="sxs-lookup"><span data-stu-id="79f54-232">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="79f54-233">選取 [WebSocket 通訊協定] 功能。</span><span class="sxs-lookup"><span data-stu-id="79f54-233">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="79f54-234">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="79f54-234">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="79f54-235">透過**確認**步驟繼續作業，安裝網頁伺服器角色和服務。</span><span class="sxs-lookup"><span data-stu-id="79f54-235">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="79f54-236">安裝**網頁伺服器 (IIS)** 角色之後，不需要重新啟動伺服器/IIS。</span><span class="sxs-lookup"><span data-stu-id="79f54-236">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="79f54-237">**Windows 桌面作業系統**</span><span class="sxs-lookup"><span data-stu-id="79f54-237">**Windows desktop operating systems**</span></span>

<span data-ttu-id="79f54-238">啟用 [IIS 管理主控台] 和 [World Wide Web 服務]。</span><span class="sxs-lookup"><span data-stu-id="79f54-238">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="79f54-239">瀏覽至控制台 > [程式] > [程式和功能] > [開啟或關閉 Windows 功能] (畫面左側)。</span><span class="sxs-lookup"><span data-stu-id="79f54-239">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="79f54-240">開啟 [Internet Information Services] 節點。</span><span class="sxs-lookup"><span data-stu-id="79f54-240">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="79f54-241">開啟 [Web 管理工具] 節點。</span><span class="sxs-lookup"><span data-stu-id="79f54-241">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="79f54-242">核取 [IIS 管理主控台] 方塊。</span><span class="sxs-lookup"><span data-stu-id="79f54-242">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="79f54-243">[World Wide Web Services] (全球資訊網服務) 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="79f54-243">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="79f54-244">接受**全球資訊網服務**的預設功能，或自訂 IIS 功能。</span><span class="sxs-lookup"><span data-stu-id="79f54-244">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="79f54-245">**Windows 驗證 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="79f54-245">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="79f54-246">若要啟用 Windows 驗證，請展開下列節點：[World Wide Web 服務] > [安全性]。</span><span class="sxs-lookup"><span data-stu-id="79f54-246">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="79f54-247">選取 [Windows 驗證] 功能。</span><span class="sxs-lookup"><span data-stu-id="79f54-247">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="79f54-248">如需詳細資訊，請參閱 [Windows 驗證 \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) 和[設定 Windows 驗證](xref:security/authentication/windowsauth)。</span><span class="sxs-lookup"><span data-stu-id="79f54-248">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="79f54-249">**WebSocket (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="79f54-249">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="79f54-250">WebSocket 由 ASP.NET Core 1.1 或更新版本所支援。</span><span class="sxs-lookup"><span data-stu-id="79f54-250">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="79f54-251">若要啟用 WebSockets，請展開下列節點：[World Wide Web 服務] > [應用程式開發功能]。</span><span class="sxs-lookup"><span data-stu-id="79f54-251">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="79f54-252">選取 [WebSocket 通訊協定] 功能。</span><span class="sxs-lookup"><span data-stu-id="79f54-252">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="79f54-253">如需詳細資訊，請參閱 [WebSockets](xref:fundamentals/websockets)。</span><span class="sxs-lookup"><span data-stu-id="79f54-253">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="79f54-254">若 IIS 安裝需要重新啟動，請重新啟動系統。</span><span class="sxs-lookup"><span data-stu-id="79f54-254">If the IIS installation requires a restart, restart the system.</span></span>

![選取 [Windows 功能] 中的 [IIS 管理主控台] 和 [World Wide Web Services] (全球資訊網服務)。](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="79f54-256">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="79f54-256">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="79f54-257">在主控系統上安裝 .NET Core 裝載套件組合。</span><span class="sxs-lookup"><span data-stu-id="79f54-257">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="79f54-258">套件組合會安裝 .NET Core 執行階段、.NET Core 程式庫和 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="79f54-258">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="79f54-259">此模組可讓 ASP.NET Core 應用程式在 IIS 背後執行。</span><span class="sxs-lookup"><span data-stu-id="79f54-259">The module allows ASP.NET Core apps to run behind IIS.</span></span> <span data-ttu-id="79f54-260">如果系統沒有網際網路連線，請先取得並安裝 [Microsoft Visual C++ 2015 可轉散發套件](https://www.microsoft.com/download/details.aspx?id=53840)，再安裝 .NET Core 裝載套件組合。</span><span class="sxs-lookup"><span data-stu-id="79f54-260">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Hosting Bundle.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="79f54-261">若裝載套件組合在 IIS 之前安裝，則必須對該套件組合安裝進行修復。</span><span class="sxs-lookup"><span data-stu-id="79f54-261">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="79f54-262">請在安裝 IIS 之後，再次執行裝載套件組合安裝程式。</span><span class="sxs-lookup"><span data-stu-id="79f54-262">Run the Hosting Bundle installer again after installing IIS.</span></span>
>
> <span data-ttu-id="79f54-263">如果在安裝 64 位元 (x64) 版本的 .NET Core 後才安裝裝載套件組合，那麼可能會遺漏 SDK ([未偵測到 .NET Core SDK](xref:test/troubleshoot#no-net-core-sdks-were-detected))。</span><span class="sxs-lookup"><span data-stu-id="79f54-263">If the Hosting Bundle is installed after installing the 64-bit (x64) version of .NET Core, SDKs might appear to be missing ([No .NET Core SDKs were detected](xref:test/troubleshoot#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="79f54-264">若要解決此問題，請參閱 <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>。</span><span class="sxs-lookup"><span data-stu-id="79f54-264">To resolve the problem, see <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>.</span></span>

### <a name="direct-download-current-version"></a><span data-ttu-id="79f54-265">直接下載 (目前版本)</span><span class="sxs-lookup"><span data-stu-id="79f54-265">Direct download (current version)</span></span>

<span data-ttu-id="79f54-266">使用下列連結下載安裝程式：</span><span class="sxs-lookup"><span data-stu-id="79f54-266">Download the installer using the following link:</span></span>

[<span data-ttu-id="79f54-267">目前的 .NET Core 裝載套件組合安裝程式 (直接下載)</span><span class="sxs-lookup"><span data-stu-id="79f54-267">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a><span data-ttu-id="79f54-268">安裝程式的先前版本</span><span class="sxs-lookup"><span data-stu-id="79f54-268">Earlier versions of the installer</span></span>

<span data-ttu-id="79f54-269">若要取得安裝程式的先前版本：</span><span class="sxs-lookup"><span data-stu-id="79f54-269">To obtain an earlier version of the installer:</span></span>

1. <span data-ttu-id="79f54-270">瀏覽至 [.NET 下載封存](https://www.microsoft.com/net/download/archives)。</span><span class="sxs-lookup"><span data-stu-id="79f54-270">Navigate to the [.NET download archives](https://www.microsoft.com/net/download/archives).</span></span>
1. <span data-ttu-id="79f54-271">在 [.NET Core] 下，選取 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="79f54-271">Under **.NET Core**, select the .NET Core version.</span></span>
1. <span data-ttu-id="79f54-272">在 [執行應用程式 - 執行階段] 欄中，尋找想要的 .NET Core 執行階段版本列。</span><span class="sxs-lookup"><span data-stu-id="79f54-272">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="79f54-273">使用**執行階段與裝載套件組合**連結。</span><span class="sxs-lookup"><span data-stu-id="79f54-273">Download the installer using the **Runtime & Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="79f54-274">某些安裝程式包含已達到期生命週期結束 (EOL) 的發行版本，這些發行版本已不受 Microsoft 支援。</span><span class="sxs-lookup"><span data-stu-id="79f54-274">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="79f54-275">如需詳細資訊，請參閱[支援原則](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) \(英文 \)。</span><span class="sxs-lookup"><span data-stu-id="79f54-275">For more information, see the [support policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="79f54-276">安裝裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="79f54-276">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="79f54-277">在伺服器上執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="79f54-277">Run the installer on the server.</span></span> <span data-ttu-id="79f54-278">從系統管理員命令殼層執行安裝程式時，有 下列參數可用：</span><span class="sxs-lookup"><span data-stu-id="79f54-278">The following parameters are available when running the installer from an administrator command shell:</span></span>

   * <span data-ttu-id="79f54-279">`OPT_NO_ANCM=1` &ndash; 跳過安裝 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="79f54-279">`OPT_NO_ANCM=1` &ndash; Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="79f54-280">`OPT_NO_RUNTIME=1` &ndash; 跳過安裝 .NET Core 執行階段。</span><span class="sxs-lookup"><span data-stu-id="79f54-280">`OPT_NO_RUNTIME=1` &ndash; Skip installing the .NET Core runtime.</span></span>
   * <span data-ttu-id="79f54-281">`OPT_NO_SHAREDFX=1` &ndash; 跳過安裝 ASP.NET 共用架構 (ASP.NET 執行階段)。</span><span class="sxs-lookup"><span data-stu-id="79f54-281">`OPT_NO_SHAREDFX=1` &ndash; Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span>
   * <span data-ttu-id="79f54-282">`OPT_NO_X86=1` &ndash; 跳過安裝 x86 執行階段。</span><span class="sxs-lookup"><span data-stu-id="79f54-282">`OPT_NO_X86=1` &ndash; Skip installing x86 runtimes.</span></span> <span data-ttu-id="79f54-283">當您確定不會裝載 32 位元應用程式時，請使用此參數。</span><span class="sxs-lookup"><span data-stu-id="79f54-283">Use this parameter when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="79f54-284">如果將來有可能同時裝載 32 位元和 64 位元應用程式，請不要使用此參數並安裝這兩個執行階段。</span><span class="sxs-lookup"><span data-stu-id="79f54-284">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this parameter and install both runtimes.</span></span>
   * <span data-ttu-id="79f54-285">`OPT_NO_SHARED_CONFIG_CHECK=1` &ndash; 停用使用 IIS 共用設定 (當共用設定 (*applicationHost.config*) 位於與 IIS 安裝相同的機器上時) 進行檢查。</span><span class="sxs-lookup"><span data-stu-id="79f54-285">`OPT_NO_SHARED_CONFIG_CHECK=1` &ndash; Disable the check for using an IIS Shared Configuration when the shared configuration (*applicationHost.config*) is on the same machine as the IIS installation.</span></span> <span data-ttu-id="79f54-286">*只在 ASP.NET Core 2.2 或更新版本的裝載套件組合安裝程式上可用。*</span><span class="sxs-lookup"><span data-stu-id="79f54-286">*Only available for ASP.NET Core 2.2 or later Hosting Bundler installers.*</span></span> <span data-ttu-id="79f54-287">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>。</span><span class="sxs-lookup"><span data-stu-id="79f54-287">For more information, see <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>.</span></span>
1. <span data-ttu-id="79f54-288">請重新啟動系統，或是從命令殼層依序執行 **net stop was /y** 和 **net start w3svc**。</span><span class="sxs-lookup"><span data-stu-id="79f54-288">Restart the system or execute **net stop was /y**, followed by **net start w3svc** from a command shell.</span></span> <span data-ttu-id="79f54-289">重新啟動 IIS 將能偵測到由安裝程式對系統路徑 (此為環境變數) 所做出的變更。</span><span class="sxs-lookup"><span data-stu-id="79f54-289">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

> [!NOTE]
> <span data-ttu-id="79f54-290">如需 IIS 共用組態的資訊，請參閱[使用 IIS 共用組態的 ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)。</span><span class="sxs-lookup"><span data-stu-id="79f54-290">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="79f54-291">使用 Visual Studio 發佈時安裝 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="79f54-291">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="79f54-292">將應用程式部署到具有 [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later) 的伺服器時，請在伺服器上安裝最新版的 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="79f54-292">When deploying apps to servers with [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="79f54-293">若要安裝 Web Deploy，請使用 [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) 或從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=43717)直接取得安裝程式。</span><span class="sxs-lookup"><span data-stu-id="79f54-293">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="79f54-294">慣用的方法是使用 WebPI。</span><span class="sxs-lookup"><span data-stu-id="79f54-294">The preferred method is to use WebPI.</span></span> <span data-ttu-id="79f54-295">WebPI 提供獨立的安裝程式和組態以裝載提供者。</span><span class="sxs-lookup"><span data-stu-id="79f54-295">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="79f54-296">建立 IIS 網站</span><span class="sxs-lookup"><span data-stu-id="79f54-296">Create the IIS site</span></span>

1. <span data-ttu-id="79f54-297">在主控系統上，請建立資料夾以容納應用程式的已發行資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="79f54-297">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="79f54-298">應用程式的部署配置已詳述於[目錄結構](xref:host-and-deploy/directory-structure)主題。</span><span class="sxs-lookup"><span data-stu-id="79f54-298">An app's deployment layout is described in the [Directory Structure](xref:host-and-deploy/directory-structure) topic.</span></span>

1. <span data-ttu-id="79f54-299">在 [IIS 管理員] 中，於 [連線] 面板中開啟伺服器的節點。</span><span class="sxs-lookup"><span data-stu-id="79f54-299">In IIS Manager, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="79f54-300">以滑鼠右鍵按一下 [網站] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="79f54-300">Right-click the **Sites** folder.</span></span> <span data-ttu-id="79f54-301">從操作功能表選取 [新增網站]。</span><span class="sxs-lookup"><span data-stu-id="79f54-301">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="79f54-302">提供**網站名稱**，並將**實體路徑**設定為應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="79f54-302">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="79f54-303">透過選取 [確定] 來提供**繫結**設定並建立網站：</span><span class="sxs-lookup"><span data-stu-id="79f54-303">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![在新增網站步驟中提供站台名稱、實體路徑和主機名稱。](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="79f54-305">請**勿**使用最上層萬用字元繫結 (`http://*:80/`與 `http://+:80`)。</span><span class="sxs-lookup"><span data-stu-id="79f54-305">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="79f54-306">最上層萬用字元繫結可能暴露您的應用程式安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="79f54-306">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="79f54-307">這對強式與弱式萬用字元皆適用。</span><span class="sxs-lookup"><span data-stu-id="79f54-307">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="79f54-308">請使用明確主機名稱，而非萬用字元。</span><span class="sxs-lookup"><span data-stu-id="79f54-308">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="79f54-309">若您擁有整個父網域 (與具弱點的 `*.com` 相對) 的控制權，則子網域萬用字元繫結 (例如 `*.mysub.com`) 就沒有此安全性風險。</span><span class="sxs-lookup"><span data-stu-id="79f54-309">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="79f54-310">如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="79f54-310">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="79f54-311">在伺服器的節點之下，選取 [應用程式集區]。</span><span class="sxs-lookup"><span data-stu-id="79f54-311">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="79f54-312">以滑鼠右鍵按一下網站的應用程式集區，然後從操作功能表選取 [基本設定]。</span><span class="sxs-lookup"><span data-stu-id="79f54-312">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="79f54-313">在 [編輯應用程式集區] 視窗中，將 [.NET CLR 版本] 設定為 [沒有受控碼]：</span><span class="sxs-lookup"><span data-stu-id="79f54-313">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![將 .NET CLR 版本設為 [沒有受控碼]。](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="79f54-315">ASP.NET Core 會在不同的處理序中執行，並管理執行階段。</span><span class="sxs-lookup"><span data-stu-id="79f54-315">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="79f54-316">ASP.NET Core 不仰賴載入桌面 CLR (.NET CLR)&mdash;會使用 .NET Core 的核心通用語言執行平台 (CoreCLR) 來開機以在背景工作處理序中裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="79f54-316">ASP.NET Core doesn't rely on loading the desktop CLR (.NET CLR)&mdash;the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process.</span></span> <span data-ttu-id="79f54-317">將 [.NET CLR 版本] 設定為 [沒有受控碼] 是選擇性的，但建議這樣做。</span><span class="sxs-lookup"><span data-stu-id="79f54-317">Setting the **.NET CLR version** to **No Managed Code** is optional but recommended.</span></span>

1. <span data-ttu-id="79f54-318">*ASP.NET Core 2.2 或更新版本*：對於使用[同處理序主控模型](xref:fundamentals/servers/index#in-process-hosting-model)的 64 位元 (x64) [自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)，會停用 32 位元 (x86) 處理序的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="79f54-318">*ASP.NET Core 2.2 or later*: For a 64-bit (x64) [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) that uses the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model), disable the app pool for 32-bit (x86) processes.</span></span>

   <span data-ttu-id="79f54-319">在 IIS 管理員的 [動作] 資訊看板 > [應用程式集區] 中，選取 [設定應用程式集區預設值] 或 [進階設定]。</span><span class="sxs-lookup"><span data-stu-id="79f54-319">In the **Actions** sidebar of IIS Manager > **Application Pools**, select **Set Application Pool Defaults** or **Advanced Settings**.</span></span> <span data-ttu-id="79f54-320">找到 [啟用 32 位元應用程式]，然後將其值設定為 `False`。</span><span class="sxs-lookup"><span data-stu-id="79f54-320">Locate **Enable 32-Bit Applications** and set the value to `False`.</span></span> <span data-ttu-id="79f54-321">此設定不會影響為[處理程序外裝載](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model)部署的應用程式。</span><span class="sxs-lookup"><span data-stu-id="79f54-321">This setting doesn't affect apps deployed for [out-of-process hosting](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).</span></span>

1. <span data-ttu-id="79f54-322">確認處理序模型身分識別具有適當的權限。</span><span class="sxs-lookup"><span data-stu-id="79f54-322">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="79f54-323">如果您將應用程式集區的預設身分識別 ([處理序模型] > [身分識別]) 從 **ApplicationPoolIdentity** 變更為其他身分識別，請確認新的身分識別具有必要權限，可存取應用程式的資料夾、資料庫和其他必要的資源。</span><span class="sxs-lookup"><span data-stu-id="79f54-323">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="79f54-324">例如，應用程式集區需要針對應用程式讀取和寫入檔案的資料夾取得讀取和寫入權限。</span><span class="sxs-lookup"><span data-stu-id="79f54-324">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="79f54-325">**Windows 驗證設定 (選擇性)**</span><span class="sxs-lookup"><span data-stu-id="79f54-325">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="79f54-326">如需詳細資訊，請參閱[設定 Windows 驗證](xref:security/authentication/windowsauth)主題。</span><span class="sxs-lookup"><span data-stu-id="79f54-326">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="79f54-327">部署應用程式</span><span class="sxs-lookup"><span data-stu-id="79f54-327">Deploy the app</span></span>

<span data-ttu-id="79f54-328">將應用程式部署至在主控系統上建立的資料夾。</span><span class="sxs-lookup"><span data-stu-id="79f54-328">Deploy the app to the folder created on the hosting system.</span></span> <span data-ttu-id="79f54-329">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 是建議的部署機制。</span><span class="sxs-lookup"><span data-stu-id="79f54-329">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="79f54-330">使用 Visual Studio 的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="79f54-330">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="79f54-331">請參閱[適用於 ASP.NET Core 應用程式部署的 Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)主題，了解如何建立搭配 Web Deploy 使用的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="79f54-331">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="79f54-332">如果裝載提供者提供或支援建立發行設定檔，請下載其設定檔，並使用 Visual Studio 的 [發行] 對話方塊匯入其設定檔。</span><span class="sxs-lookup"><span data-stu-id="79f54-332">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![[發佈] 對話方塊頁](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="79f54-334">Visual Studio 外部的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="79f54-334">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="79f54-335">透過命令列也可以在 Visual Studio 外部使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy)。</span><span class="sxs-lookup"><span data-stu-id="79f54-335">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="79f54-336">如需詳細資訊，請參閱 [Web 部署工具](/iis/publish/using-web-deploy/use-the-web-deployment-tool)。</span><span class="sxs-lookup"><span data-stu-id="79f54-336">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="79f54-337">Web Deploy 的替代項目</span><span class="sxs-lookup"><span data-stu-id="79f54-337">Alternatives to Web Deploy</span></span>

<span data-ttu-id="79f54-338">使用數種方法的其中一種將應用程式移到主控系統，例如手動複製、Xcopy、Robocopy 或 PowerShell。</span><span class="sxs-lookup"><span data-stu-id="79f54-338">Use any of several methods to move the app to the hosting system, such as manual copy, Xcopy, Robocopy, or PowerShell.</span></span>

<span data-ttu-id="79f54-339">如需 IIS 的 ASP.NET Core 部署詳細資訊，請參閱 [IIS 系統管理員的部署資源](#deployment-resources-for-iis-administrators)一節。</span><span class="sxs-lookup"><span data-stu-id="79f54-339">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="79f54-340">瀏覽網站</span><span class="sxs-lookup"><span data-stu-id="79f54-340">Browse the website</span></span>

![Microsoft Edge 瀏覽器已載入 IIS 啟動頁面。](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="79f54-342">已鎖定的部署檔案</span><span class="sxs-lookup"><span data-stu-id="79f54-342">Locked deployment files</span></span>

<span data-ttu-id="79f54-343">當應用程式執行時，會鎖定部署資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="79f54-343">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="79f54-344">無法於部署期間覆寫已鎖定的檔案。</span><span class="sxs-lookup"><span data-stu-id="79f54-344">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="79f54-345">若要釋放部署中的已鎖定檔案，請使用下列其中**一種**方法停止應用程式集區：</span><span class="sxs-lookup"><span data-stu-id="79f54-345">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="79f54-346">使用 Web Deploy 並參考專案檔中的 `Microsoft.NET.Sdk.Web`。</span><span class="sxs-lookup"><span data-stu-id="79f54-346">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="79f54-347">*app_offline.htm* 檔案是放在 Web 應用程式目錄的根目錄中。</span><span class="sxs-lookup"><span data-stu-id="79f54-347">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="79f54-348">當檔案存在時，ASP.NET Core 模組會正常關閉應用程式，並在部署期間提供 *app_offline.htm* 檔案。</span><span class="sxs-lookup"><span data-stu-id="79f54-348">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="79f54-349">如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)。</span><span class="sxs-lookup"><span data-stu-id="79f54-349">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="79f54-350">在伺服器上的 IIS 管理員中手動停止應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="79f54-350">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="79f54-351">使用 PowerShell 卸除 *app_offline.html* (需要 PowerShell 5 或更新版本)：</span><span class="sxs-lookup"><span data-stu-id="79f54-351">Use PowerShell to drop *app_offline.html* (requires PowerShell 5 or later):</span></span>

  ```PowerShell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a><span data-ttu-id="79f54-352">資料保護</span><span class="sxs-lookup"><span data-stu-id="79f54-352">Data protection</span></span>

<span data-ttu-id="79f54-353">[ASP.NET Core 資料保護堆疊](xref:security/data-protection/introduction)是由數個 ASP.NET Core [中介軟體](xref:fundamentals/middleware/index)所使用，包括用於驗證的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="79f54-353">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="79f54-354">即使資料保護 API 不是由使用者程式碼呼叫，仍應使用部署指令碼或是在使用者程式碼中設定資料保護，以建立持續性的密碼編譯[金鑰存放區](xref:security/data-protection/implementation/key-management)。</span><span class="sxs-lookup"><span data-stu-id="79f54-354">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="79f54-355">如不設定資料保護，金鑰會保留在記憶體中，並於應用程式重新啟動時捨棄。</span><span class="sxs-lookup"><span data-stu-id="79f54-355">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="79f54-356">如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：</span><span class="sxs-lookup"><span data-stu-id="79f54-356">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="79f54-357">所有以 Cookie 為基礎的驗證權杖都會失效。</span><span class="sxs-lookup"><span data-stu-id="79f54-357">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="79f54-358">當使用者提出下一個要求時，需要再次登入。</span><span class="sxs-lookup"><span data-stu-id="79f54-358">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="79f54-359">所有以 Keyring 保護的資料都無法再解密。</span><span class="sxs-lookup"><span data-stu-id="79f54-359">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="79f54-360">這可能會包含 [CSRF 權杖](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)和 [ASP.NET Core MVC TempData cookie](xref:fundamentals/app-state#tempdata)。</span><span class="sxs-lookup"><span data-stu-id="79f54-360">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="79f54-361">若要在 IIS 下設定資料保護以保存 Keyring，請使用下列其中**一種**方法：</span><span class="sxs-lookup"><span data-stu-id="79f54-361">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="79f54-362">**建立資料保護登錄機碼**</span><span class="sxs-lookup"><span data-stu-id="79f54-362">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="79f54-363">ASP.NET Core 應用程式所使用的資料保護金鑰會儲存在應用程式外部的登錄中。</span><span class="sxs-lookup"><span data-stu-id="79f54-363">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="79f54-364">若要保存指定應用程式的金鑰，請為應用程式集區建立登錄機碼。</span><span class="sxs-lookup"><span data-stu-id="79f54-364">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="79f54-365">若為獨立的非Web 伺服陣列 IIS 安裝，請針對搭配使用 ASP.NET Core 應用程式的每個應用程式集區，使用[資料保護 Provision-AutoGenKeys.ps1 PowerShell 指令碼](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1)。</span><span class="sxs-lookup"><span data-stu-id="79f54-365">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="79f54-366">此指令碼會在 HKLM 登錄中建立登錄機碼，只有應用程式之應用程式集區的背景工作處理序帳戶可以存取它。</span><span class="sxs-lookup"><span data-stu-id="79f54-366">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="79f54-367">在待用期間使用 DPAPI 和全電腦金鑰加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="79f54-367">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="79f54-368">在 Web 伺服陣列案例中，應用程式可以設定成使用 UNC 路徑來儲存其資料保護 Keyring。</span><span class="sxs-lookup"><span data-stu-id="79f54-368">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="79f54-369">根據預設，資料保護金鑰不予加密。</span><span class="sxs-lookup"><span data-stu-id="79f54-369">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="79f54-370">請確保網路共用的檔案權限僅限於執行應用程式的 Windows 帳戶。</span><span class="sxs-lookup"><span data-stu-id="79f54-370">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="79f54-371">可以使用 X509 憑證來保護待用的金鑰。</span><span class="sxs-lookup"><span data-stu-id="79f54-371">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="79f54-372">請考慮下列讓使用者上傳憑證的機制：將憑證放入使用者的受信任憑證存放區，並確保在執行使用者應用程式的所有電腦上都能使用這些憑證。</span><span class="sxs-lookup"><span data-stu-id="79f54-372">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="79f54-373">如需詳細資訊，請參閱[設定 ASP.NET Core 資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="79f54-373">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="79f54-374">**設定 IIS 應用程式集區載入使用者設定檔**</span><span class="sxs-lookup"><span data-stu-id="79f54-374">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="79f54-375">此設定位在應用程式集區 [進階設定] 下的 [處理序模型] 區段中。</span><span class="sxs-lookup"><span data-stu-id="79f54-375">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="79f54-376">將 [載入使用者設定檔] 設為 `True`。</span><span class="sxs-lookup"><span data-stu-id="79f54-376">Set **Load User Profile** to `True`.</span></span> <span data-ttu-id="79f54-377">當設定為 `True` 時，金鑰會儲存在使用者設定檔目錄中，且使用具有使用者帳戶專屬金鑰的 DPAPI 保護。</span><span class="sxs-lookup"><span data-stu-id="79f54-377">When set to `True`, keys are stored in the user profile directory and protected using DPAPI with a key specific to the user account.</span></span> <span data-ttu-id="79f54-378">金鑰會保存到 *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="79f54-378">Keys are persisted to the *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* folder.</span></span>

  <span data-ttu-id="79f54-379">應用程式集區的 [setProfileEnvironment 屬性](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration)也必須啟用。</span><span class="sxs-lookup"><span data-stu-id="79f54-379">The app pool's [setProfileEnvironment attribute](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) must also be enabled.</span></span> <span data-ttu-id="79f54-380">`setProfileEnvironment` 的預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="79f54-380">The default value of `setProfileEnvironment` is `true`.</span></span> <span data-ttu-id="79f54-381">在某些情況下 (例如 Windows OS)，`setProfileEnvironment` 會設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="79f54-381">In some scenarios (for example, Windows OS), `setProfileEnvironment` is set to `false`.</span></span> <span data-ttu-id="79f54-382">如果金鑰並未如預期地儲存在使用者設定檔目錄中：</span><span class="sxs-lookup"><span data-stu-id="79f54-382">If keys aren't stored in the user profile directory as expected:</span></span>

  1. <span data-ttu-id="79f54-383">瀏覽至 *%windir%/system32/inetsrv/config* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="79f54-383">Navigate to the *%windir%/system32/inetsrv/config* folder.</span></span>
  1. <span data-ttu-id="79f54-384">開啟 *applicationHost.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="79f54-384">Open the *applicationHost.config* file.</span></span>
  1. <span data-ttu-id="79f54-385">找到 `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` 項目。</span><span class="sxs-lookup"><span data-stu-id="79f54-385">Locate the `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` element.</span></span>
  1. <span data-ttu-id="79f54-386">確認 `setProfileEnvironment` 屬性不存在 (其預設值為 `true`)，或明確地將屬性值設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="79f54-386">Confirm that the `setProfileEnvironment` attribute isn't present, which defaults the value to `true`, or explicitly set the attribute's value to `true`.</span></span>

* <span data-ttu-id="79f54-387">**將檔案系統當作 Keyring 存放區使用**</span><span class="sxs-lookup"><span data-stu-id="79f54-387">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="79f54-388">調整應用程式程式碼，以[將檔案系統當作 Keyring 存放區使用](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="79f54-388">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="79f54-389">使用 X509 憑證來保護 Keyring，並確保憑證是受信任的憑證。</span><span class="sxs-lookup"><span data-stu-id="79f54-389">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="79f54-390">如果憑證為自我簽署，請將憑證放在受信任的根存放區。</span><span class="sxs-lookup"><span data-stu-id="79f54-390">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="79f54-391">在 Web 伺服陣列中使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="79f54-391">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="79f54-392">請使用所有電腦都可以存取的檔案共用。</span><span class="sxs-lookup"><span data-stu-id="79f54-392">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="79f54-393">將 X509 憑證部署到每一部電腦。</span><span class="sxs-lookup"><span data-stu-id="79f54-393">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="79f54-394">設定[程式碼中的資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="79f54-394">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="79f54-395">**設定資料保護的全電腦原則**</span><span class="sxs-lookup"><span data-stu-id="79f54-395">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="79f54-396">針對取用資料保護 API 的所有應用程式，資料保護系統僅支援有限的預設[全電腦原則](xref:security/data-protection/configuration/machine-wide-policy)設定。</span><span class="sxs-lookup"><span data-stu-id="79f54-396">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="79f54-397">如需詳細資訊，請參閱<xref:security/data-protection/introduction>。</span><span class="sxs-lookup"><span data-stu-id="79f54-397">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="virtual-directories"></a><span data-ttu-id="79f54-398">虛擬目錄</span><span class="sxs-lookup"><span data-stu-id="79f54-398">Virtual Directories</span></span>

<span data-ttu-id="79f54-399">ASP.NET Core 應用程式不支援 [IIS 虛擬目錄](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories)。</span><span class="sxs-lookup"><span data-stu-id="79f54-399">[IIS Virtual Directories](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) aren't supported with ASP.NET Core apps.</span></span> <span data-ttu-id="79f54-400">應用程式能以[子應用程式](#sub-applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="79f54-400">An app can be hosted as a [sub-application](#sub-applications).</span></span>

## <a name="sub-applications"></a><span data-ttu-id="79f54-401">子應用程式</span><span class="sxs-lookup"><span data-stu-id="79f54-401">Sub-applications</span></span>

<span data-ttu-id="79f54-402">ASP.NET Core 應用程式能以 [IIS 子應用程式](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications)的形式裝載。</span><span class="sxs-lookup"><span data-stu-id="79f54-402">An ASP.NET Core app can be hosted as an [IIS sub-application (sub-app)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications).</span></span> <span data-ttu-id="79f54-403">子應用程式的路徑會成為根應用程式 URL 的一部分。</span><span class="sxs-lookup"><span data-stu-id="79f54-403">The sub-app's path becomes part of the root app's URL.</span></span>

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="79f54-404">子應用程式不應該包括 ASP.NET Core 模組做為處理常式。</span><span class="sxs-lookup"><span data-stu-id="79f54-404">A sub-app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="79f54-405">如果您在子應用程式的 *web.config* 檔案中將模組新增為處理常式，則嘗試瀏覽子應用程式時，會收到參考錯誤設定檔的 *500.19 內部伺服器錯誤*。</span><span class="sxs-lookup"><span data-stu-id="79f54-405">If the module is added as a handler in a sub-app's *web.config* file, a *500.19 Internal Server Error* referencing the faulty config file is received when attempting to browse the sub-app.</span></span>

<span data-ttu-id="79f54-406">下列範例顯示 ASP.NET Core 子應用程式的已發佈 *web.config* 檔案：</span><span class="sxs-lookup"><span data-stu-id="79f54-406">The following example shows a published *web.config* file for an ASP.NET Core sub-app:</span></span>

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

<span data-ttu-id="79f54-407">在 ASP.NET Core 應用程式下裝載非 ASP.NET Core 子應用程式時，請明確移除子應用程式 *web.config* 檔案中已繼承的處理常式：</span><span class="sxs-lookup"><span data-stu-id="79f54-407">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app's *web.config* file:</span></span>

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

<span data-ttu-id="79f54-408">子應用程式內的靜態資產連結應該使用波狀符號與斜線 (`~/`) 標記法。</span><span class="sxs-lookup"><span data-stu-id="79f54-408">Static asset links within the sub-app should use tilde-slash (`~/`) notation.</span></span> <span data-ttu-id="79f54-409">波狀符號與斜線標記法會觸發[標記協助程式](xref:mvc/views/tag-helpers/intro)以將子應用程式的路徑基底附加到轉譯的相對連結前面。</span><span class="sxs-lookup"><span data-stu-id="79f54-409">Tilde-slash notation triggers a [Tag Helper](xref:mvc/views/tag-helpers/intro) to prepend the sub-app's pathbase to the rendered relative link.</span></span> <span data-ttu-id="79f54-410">針對位於 `/subapp_path` 的子應用程式，使用 `src="~/image.png"` 連結的影像會轉譯為 `src="/subapp_path/image.png"`。</span><span class="sxs-lookup"><span data-stu-id="79f54-410">For a sub-app at `/subapp_path`, an image linked with `src="~/image.png"` is rendered as `src="/subapp_path/image.png"`.</span></span> <span data-ttu-id="79f54-411">根應用程式的靜態檔案中介軟體不會處理靜態檔案要求。</span><span class="sxs-lookup"><span data-stu-id="79f54-411">The root app's Static File Middleware doesn't process the static file request.</span></span> <span data-ttu-id="79f54-412">要求會由子應用程式的靜態檔案中介軟體處理。</span><span class="sxs-lookup"><span data-stu-id="79f54-412">The request is processed by the sub-app's Static File Middleware.</span></span>

<span data-ttu-id="79f54-413">若靜態資產的 `src` 屬性是設定為絕對路徑 (例如，`src="/image.png"`)，會以不使用子應用程式路徑基底的方式轉譯連結。</span><span class="sxs-lookup"><span data-stu-id="79f54-413">If a static asset's `src` attribute is set to an absolute path (for example, `src="/image.png"`), the link is rendered without the sub-app's pathbase.</span></span> <span data-ttu-id="79f54-414">根應用程式的靜態檔案中介軟體會嘗試從根應用程式的 [webroot](xref:fundamentals/index#web-root) 提供資產，這會導致「404 - 找不到」回應 (除非根應用程式可存取靜態資產)。</span><span class="sxs-lookup"><span data-stu-id="79f54-414">The root app's Static File Middleware attempts to serve the asset from the root app's [web root](xref:fundamentals/index#web-root), which results in a *404 - Not Found* response unless the static asset is available from the root app.</span></span>

<span data-ttu-id="79f54-415">裝載 ASP.NET Core 應用程式做為另一個 ASP.NET Core 應用程式下的子應用程式：</span><span class="sxs-lookup"><span data-stu-id="79f54-415">To host an ASP.NET Core app as a sub-app under another ASP.NET Core app:</span></span>

1. <span data-ttu-id="79f54-416">為子應用程式建立應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="79f54-416">Establish an app pool for the sub-app.</span></span> <span data-ttu-id="79f54-417">將 [.NET CLR 版本] 設定為 [沒有受控碼]，因為會將核心通用語言執行平台 (CoreCLR) 開機以在背景工作處理序中裝載應用程式，而非在桌面 CLR (.NET CLR) 中裝載。</span><span class="sxs-lookup"><span data-stu-id="79f54-417">Set the **.NET CLR Version** to **No Managed Code** because the Core Common Language Runtime (CoreCLR) for .NET Core is is booted to host the app in the worker process, not the desktop CLR (.NET CLR).</span></span>

1. <span data-ttu-id="79f54-418">使用根網站下資料夾中的子應用程式在 IIS 管理員中新增根網站。</span><span class="sxs-lookup"><span data-stu-id="79f54-418">Add the root site in IIS Manager with the sub-app in a folder under the root site.</span></span>

1. <span data-ttu-id="79f54-419">以滑鼠右鍵按一下 IIS 管理員中的子應用程式資料夾，然後選取 [轉換成應用程式]。</span><span class="sxs-lookup"><span data-stu-id="79f54-419">Right-click the sub-app folder in IIS Manager and select **Convert to Application**.</span></span>

1. <span data-ttu-id="79f54-420">在 [新增應用程式] 對話方塊中，使用 [應用程式集區] 的[選取] 按鈕來指派您為子應用程式建立的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="79f54-420">In the **Add Application** dialog, use the **Select** button for the **Application Pool** to assign the app pool that you created for the sub-app.</span></span> <span data-ttu-id="79f54-421">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="79f54-421">Select **OK**.</span></span>

<span data-ttu-id="79f54-422">將不同的應用程式集區指派給子應用程式是使用同處理序裝載模型。</span><span class="sxs-lookup"><span data-stu-id="79f54-422">The assignment of a separate app pool to the sub-app is a requirement when using the in-process hosting model.</span></span>

<span data-ttu-id="79f54-423">如需有關同處理序裝載模型與如何設定 ASP.NET Core 模組的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module> 與 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="79f54-423">For more information on the in-process hosting model and configuring the ASP.NET Core Module, see <xref:host-and-deploy/aspnet-core-module> and <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="79f54-424">使用 web.config 的 IIS 組態</span><span class="sxs-lookup"><span data-stu-id="79f54-424">Configuration of IIS with web.config</span></span>

<span data-ttu-id="79f54-425">在對使用了 ASP.NET Core 模組的 ASP.NET Core 有作用的 IIS 情境下，設定會受 *web.config* 的 `<system.webServer>` 區段影響。</span><span class="sxs-lookup"><span data-stu-id="79f54-425">IIS configuration is influenced by the `<system.webServer>` section of *web.config* for IIS scenarios that are functional for ASP.NET Core apps with the ASP.NET Core Module.</span></span> <span data-ttu-id="79f54-426">舉例來說，IIS 設定對動態壓縮有作用。</span><span class="sxs-lookup"><span data-stu-id="79f54-426">For example, IIS configuration is functional for dynamic compression.</span></span> <span data-ttu-id="79f54-427">如果在伺服器層級將 IIS 設為使用動態壓縮，應用程式 *web.config* 檔案中的 `<urlCompression>` 元素則可為 ASP.NET Core 應用程式予以停用。</span><span class="sxs-lookup"><span data-stu-id="79f54-427">If IIS is configured at the server level to use dynamic compression, the `<urlCompression>` element in the app's *web.config* file can disable it for an ASP.NET Core app.</span></span>

<span data-ttu-id="79f54-428">如需詳細資訊，請參閱 [\<system.webServer> 的設定參考](/iis/configuration/system.webServer/)、[ASP.NET Core 模組設定參考](xref:host-and-deploy/aspnet-core-module)以及 [IIS 模組與 ASP.NET Core](xref:host-and-deploy/iis/modules)。</span><span class="sxs-lookup"><span data-stu-id="79f54-428">For more information, see the [configuration reference for \<system.webServer>](/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module), and [IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="79f54-429">若要設定在隔離的應用程式集區中執行之個別應用程式的環境變數 (支援 IIS 10.0 或更新版本)，請參閱 IIS 參考文件之[環境變數 \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的 *AppCmd.exe 命令*一節。</span><span class="sxs-lookup"><span data-stu-id="79f54-429">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="79f54-430">web.config 的組態區段</span><span class="sxs-lookup"><span data-stu-id="79f54-430">Configuration sections of web.config</span></span>

<span data-ttu-id="79f54-431">ASP.NET Core 應用程式的設定不使用 *web.config* 中 ASP.NET 4.x 應用程式的設定區段：</span><span class="sxs-lookup"><span data-stu-id="79f54-431">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

<span data-ttu-id="79f54-432">使用其他組態提供者設定的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="79f54-432">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="79f54-433">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="79f54-433">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="79f54-434">應用程式集區</span><span class="sxs-lookup"><span data-stu-id="79f54-434">Application Pools</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="79f54-435">應用程式集區隔離取決於裝載模型：</span><span class="sxs-lookup"><span data-stu-id="79f54-435">App pool isolation is determined by the hosting model:</span></span>

* <span data-ttu-id="79f54-436">處理序內裝載 &ndash; 應用程式必須在分開的應用程式集區中執行。</span><span class="sxs-lookup"><span data-stu-id="79f54-436">In-process hosting &ndash; Apps are required to run in separate app pools.</span></span>
* <span data-ttu-id="79f54-437">處理序外裝載 &ndash; 建議藉由在各自的應用程式集區中執行每個應用程式，以將應用程式互相隔離。</span><span class="sxs-lookup"><span data-stu-id="79f54-437">Out-of-process hosting &ndash; We recommend isolating the apps from each other by running each app in its own app pool.</span></span>

<span data-ttu-id="79f54-438">IIS [新增網站] 對話方塊預設每個應用程式皆為單一應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="79f54-438">The IIS **Add Website** dialog defaults to a single app pool per app.</span></span> <span data-ttu-id="79f54-439">當提供 [網站名稱] 時，文字會自動轉移至 [應用程式集區] 文字方塊。</span><span class="sxs-lookup"><span data-stu-id="79f54-439">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="79f54-440">新增網站時，會使用該網站名稱建立新的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="79f54-440">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="79f54-441">在伺服器上裝載多個網站時，建議您在其各自的應用程式集區中執行各個應用程式，讓應用程式彼此隔離。</span><span class="sxs-lookup"><span data-stu-id="79f54-441">When hosting multiple websites on a server, we recommend isolating the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="79f54-442">IIS [新增網站] 對話方塊預設成此組態。</span><span class="sxs-lookup"><span data-stu-id="79f54-442">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="79f54-443">當提供 [網站名稱] 時，文字會自動轉移至 [應用程式集區] 文字方塊。</span><span class="sxs-lookup"><span data-stu-id="79f54-443">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="79f54-444">新增網站時，會使用該網站名稱建立新的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="79f54-444">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

## <a name="application-pool-identity"></a><span data-ttu-id="79f54-445">應用程式集區身分識別</span><span class="sxs-lookup"><span data-stu-id="79f54-445">Application Pool Identity</span></span>

<span data-ttu-id="79f54-446">應用程式集區身分識別帳戶可讓應用程式在唯一的帳戶下執行，不必建立及管理網域或本機帳戶。</span><span class="sxs-lookup"><span data-stu-id="79f54-446">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="79f54-447">在 IIS 8.0 或更新版本中，IIS 管理背景工作處理序 (WAS) 會使用新的應用程式集區名稱建立虛擬帳戶，並預設在此帳戶下執行應用程式集區的背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="79f54-447">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="79f54-448">在 IIS 管理主控台中，於應用程式集區的 [進階設定] 下，確定 [身分識別] 設定為使用 **ApplicationPoolIdentity**：</span><span class="sxs-lookup"><span data-stu-id="79f54-448">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![應用程式集區進階設定對話方塊](index/_static/apppool-identity.png)

<span data-ttu-id="79f54-450">IIS 管理程序會在 Windows 安全系統中，以應用程式集區的名稱建立安全識別碼。</span><span class="sxs-lookup"><span data-stu-id="79f54-450">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="79f54-451">可使用此身分識別來保護資源。</span><span class="sxs-lookup"><span data-stu-id="79f54-451">Resources can be secured using this identity.</span></span> <span data-ttu-id="79f54-452">不過，此身分識別不是真正的使用者帳戶，也不會顯示在 Windows 使用者管理主控台中。</span><span class="sxs-lookup"><span data-stu-id="79f54-452">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="79f54-453">如果 IIS 背景工作處理序需要提升應用程式的存取權限，請修改包含應用程式的目錄存取控制清單 (ACL)：</span><span class="sxs-lookup"><span data-stu-id="79f54-453">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="79f54-454">開啟 Windows 檔案總管，巡覽至目錄。</span><span class="sxs-lookup"><span data-stu-id="79f54-454">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="79f54-455">以滑鼠右鍵按一下目錄並選取 [屬性]。</span><span class="sxs-lookup"><span data-stu-id="79f54-455">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="79f54-456">依序選取 [安全性] 索引標籤下的 [編輯] 按鈕和 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="79f54-456">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="79f54-457">選取 [位置] 按鈕，並確定選取系統。</span><span class="sxs-lookup"><span data-stu-id="79f54-457">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="79f54-458">在 [輸入要選取的物件名稱] 區域中，輸入 **IIS AppPool\\<app_pool_name>** 。</span><span class="sxs-lookup"><span data-stu-id="79f54-458">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="79f54-459">選取 [檢查名稱] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="79f54-459">Select the **Check Names** button.</span></span> <span data-ttu-id="79f54-460">針對 [DefaultAppPool]，請使用 **IIS AppPool\DefaultAppPool** 檢查名稱。</span><span class="sxs-lookup"><span data-stu-id="79f54-460">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="79f54-461">選取 [檢查名稱] 按鈕時，[DefaultAppPool] 的值便會顯示於物件名稱區域中。</span><span class="sxs-lookup"><span data-stu-id="79f54-461">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="79f54-462">您無法直接將應用程式集區名稱輸入至物件名稱區域。</span><span class="sxs-lookup"><span data-stu-id="79f54-462">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="79f54-463">檢查物件名稱時，請使用 **IIS AppPool\\<app_pool_name>** 的格式。</span><span class="sxs-lookup"><span data-stu-id="79f54-463">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：在選取 [檢查名稱] 之前，"DefaultAppPool" 這個應用程式集區名稱在物件名稱區域中會附加至 "IIS AppPool\"。](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="79f54-465">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="79f54-465">Select **OK**.</span></span>

   ![針對應用程式資料夾選取使用者或群組對話方塊：選取 [檢查名稱] 之後，物件名稱 "DefaultAppPool" 會顯示在物件名稱區域中。](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="79f54-467">預設應該會授與讀取 &amp; 執行權限。</span><span class="sxs-lookup"><span data-stu-id="79f54-467">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="79f54-468">請視需要提供其他權限。</span><span class="sxs-lookup"><span data-stu-id="79f54-468">Provide additional permissions as needed.</span></span>

<span data-ttu-id="79f54-469">也可使用 **ICACLS** 工具透過命令提示字元授與存取權限。</span><span class="sxs-lookup"><span data-stu-id="79f54-469">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="79f54-470">使用 *DefaultAppPool*作為範例，將會使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="79f54-470">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="79f54-471">如需詳細資訊，請參閱 [icacls](/windows-server/administration/windows-commands/icacls) 主題。</span><span class="sxs-lookup"><span data-stu-id="79f54-471">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="http2-support"></a><span data-ttu-id="79f54-472">HTTP/2 支援</span><span class="sxs-lookup"><span data-stu-id="79f54-472">HTTP/2 support</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="79f54-473">在下列 IIS 部署案例中，ASP.NET Core 支援 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：</span><span class="sxs-lookup"><span data-stu-id="79f54-473">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following IIS deployment scenarios:</span></span>

* <span data-ttu-id="79f54-474">同處理序</span><span class="sxs-lookup"><span data-stu-id="79f54-474">In-process</span></span>
  * <span data-ttu-id="79f54-475">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="79f54-475">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="79f54-476">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="79f54-476">TLS 1.2 or later connection</span></span>
* <span data-ttu-id="79f54-477">跨處理序</span><span class="sxs-lookup"><span data-stu-id="79f54-477">Out-of-process</span></span>
  * <span data-ttu-id="79f54-478">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="79f54-478">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="79f54-479">公開 Edge Server 連線使用 HTTP/2，但是對 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的反向 Proxy 連線使用 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="79f54-479">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
  * <span data-ttu-id="79f54-480">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="79f54-480">TLS 1.2 or later connection</span></span>

<span data-ttu-id="79f54-481">針對建立 HTTP/2 連線時的同處理序部署，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會回報 `HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="79f54-481">For an in-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span> <span data-ttu-id="79f54-482">針對建立 HTTP/2 連線時的跨處理序部署，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會回報 `HTTP/1.1`。</span><span class="sxs-lookup"><span data-stu-id="79f54-482">For an out-of-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="79f54-483">如需有關同處理序和跨處理序主控模型的詳細資訊，請參閱 <xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="79f54-483">For more information on the in-process and out-of-process hosting models, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="79f54-484">[HTTP/2](https://httpwg.org/specs/rfc7540.html) 支援符合下列基本需求的跨處理序部署：</span><span class="sxs-lookup"><span data-stu-id="79f54-484">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported for out-of-process deployments that meet the following base requirements:</span></span>

* <span data-ttu-id="79f54-485">Windows Server 2016/Windows 10 或更新版本；IIS 10 或更新版本</span><span class="sxs-lookup"><span data-stu-id="79f54-485">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
* <span data-ttu-id="79f54-486">公開 Edge Server 連線使用 HTTP/2，但是對 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)的反向 Proxy 連線使用 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="79f54-486">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
* <span data-ttu-id="79f54-487">目標 Framework：不適用於跨處理序部署，因為 HTTP/2 連線完全由 IIS 處理。</span><span class="sxs-lookup"><span data-stu-id="79f54-487">Target framework: Not applicable to out-of-process deployments, since the HTTP/2 connection is handled entirely by IIS.</span></span>
* <span data-ttu-id="79f54-488">TLS 1.2 或更新版本連線</span><span class="sxs-lookup"><span data-stu-id="79f54-488">TLS 1.2 or later connection</span></span>

<span data-ttu-id="79f54-489">如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/1.1`。</span><span class="sxs-lookup"><span data-stu-id="79f54-489">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="79f54-490">HTTP/2 預設為啟用。</span><span class="sxs-lookup"><span data-stu-id="79f54-490">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="79f54-491">如果 HTTP/2 連線尚未建立，連線會退為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="79f54-491">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="79f54-492">如需使用 IIS 部署之 HTTP/2 設定的詳細資訊，請參閱 [IIS 上的 HTTP/2](/iis/get-started/whats-new-in-iis-10/http2-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="79f54-492">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="cors-preflight-requests"></a><span data-ttu-id="79f54-493">CORS 預檢要求</span><span class="sxs-lookup"><span data-stu-id="79f54-493">CORS preflight requests</span></span>

<span data-ttu-id="79f54-494">*此節只適用於以 .NET Framework 為目標的 ASP.NET Core 應用程式。*</span><span class="sxs-lookup"><span data-stu-id="79f54-494">*This section only applies to ASP.NET Core apps that target the .NET Framework.*</span></span>

<span data-ttu-id="79f54-495">針對以 .NET Framework 為目標的 ASP.NET Core 應用程式，在 IIS 中OPTIONS 要求預設不會傳遞到應用程式。</span><span class="sxs-lookup"><span data-stu-id="79f54-495">For an ASP.NET Core app that targets the .NET Framework, OPTIONS requests aren't passed to the app by default in IIS.</span></span> <span data-ttu-id="79f54-496">若要了解如何在 *web.config* 中設定應用程式的 IIS 處理常式以傳遞 OPTIONS 要求，請參閱[在 ASP.NET Web API 2 中啟用跨原始來源要求：CORS 如何運作](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works)。</span><span class="sxs-lookup"><span data-stu-id="79f54-496">To learn how to configure the app's IIS handlers in *web.config* to pass OPTIONS requests, see [Enable cross-origin requests in ASP.NET Web API 2: How CORS Works](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="application-initialization-module-and-idle-timeout"></a><span data-ttu-id="79f54-497">應用程式初始化模組與閒置逾時</span><span class="sxs-lookup"><span data-stu-id="79f54-497">Application Initialization Module and Idle Timeout</span></span>

<span data-ttu-id="79f54-498">在 IIS 中由 ASP.NET Core 模組版本 2 裝載時：</span><span class="sxs-lookup"><span data-stu-id="79f54-498">When hosted in IIS by the ASP.NET Core Module version 2:</span></span>

* <span data-ttu-id="79f54-499">[應用程式初始化模組](#application-initialization-module) &ndash; 應用程式的裝載[同處理序](xref:fundamentals/servers/index#in-process-hosting-model)或 [非同處理序](xref:fundamentals/servers/index#out-of-process-hosting-model)，可設定為在背景工作處理序重新啟動或伺服器重新啟動時自動啟動。</span><span class="sxs-lookup"><span data-stu-id="79f54-499">[Application Initialization Module](#application-initialization-module) &ndash; App's hosted [in-process](xref:fundamentals/servers/index#in-process-hosting-model) or [out-of-process](xref:fundamentals/servers/index#out-of-process-hosting-model), can be configured to start automatically on a worker process restart or server restart.</span></span>
* <span data-ttu-id="79f54-500">[閒置逾時](#idle-timeout) &ndash; 應用程式的裝載 [同處理序](xref:fundamentals/servers/index#in-process-hosting-model)可設定為在無活動期間不逾時。</span><span class="sxs-lookup"><span data-stu-id="79f54-500">[Idle Timeout](#idle-timeout) &ndash; App's hosted [in-process](xref:fundamentals/servers/index#in-process-hosting-model) can be configured not to timeout during periods of inactivity.</span></span>

### <a name="application-initialization-module"></a><span data-ttu-id="79f54-501">應用程式初始化模組</span><span class="sxs-lookup"><span data-stu-id="79f54-501">Application Initialization Module</span></span>

<span data-ttu-id="79f54-502">*套用到應用程式裝載同處理序與非同處理序。*</span><span class="sxs-lookup"><span data-stu-id="79f54-502">*Applies to apps hosted in-process and out-of-process.*</span></span>

<span data-ttu-id="79f54-503">[IIS 應用程式初始化](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)是一項 IIS 功能，可在應用程式集區啟動或回收時，將 HTTP 要求傳送給應用程式。</span><span class="sxs-lookup"><span data-stu-id="79f54-503">[IIS Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) is an IIS feature that sends an HTTP request to the app when the app pool starts or is recycled.</span></span> <span data-ttu-id="79f54-504">要求會觸發應用程式啟動。</span><span class="sxs-lookup"><span data-stu-id="79f54-504">The request triggers the app to start.</span></span> <span data-ttu-id="79f54-505">根據預設，IIS 會發出應用程式根 URL (`/`) 的要求以初始化應用程式 (如需有關設定的詳細資訊，請參閱[額外資源](#application-initialization-module-and-idle-timeout-additional-resources))。</span><span class="sxs-lookup"><span data-stu-id="79f54-505">By default, IIS issues a request to the app's root URL (`/`) to initialize the app (see the [additional resources](#application-initialization-module-and-idle-timeout-additional-resources) for more details on configuration).</span></span>

<span data-ttu-id="79f54-506">確認已啟用「IIS 應用程式初始化」角色功能：</span><span class="sxs-lookup"><span data-stu-id="79f54-506">Confirm that the IIS Application Initialization role feature in enabled:</span></span>

<span data-ttu-id="79f54-507">在 Windows 7 或更新的電腦系統上，當在本機使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="79f54-507">On Windows 7 or later desktop systems when using IIS locally:</span></span>

1. <span data-ttu-id="79f54-508">瀏覽到 [控制台] > [程式] > [程式和功能] > **[開啟或關閉 Windows 功能]** \(畫面左側\)。</span><span class="sxs-lookup"><span data-stu-id="79f54-508">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="79f54-509">開啟 [網際網路資訊服務]> [World Wide Web 服務]> [應用程式開發功能]。</span><span class="sxs-lookup"><span data-stu-id="79f54-509">Open **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="79f54-510">選取 [應用程式初始化]的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="79f54-510">Select the check box for **Application Initialization**.</span></span>

<span data-ttu-id="79f54-511">在 Windows Server 2008 R2 或更新版本上：</span><span class="sxs-lookup"><span data-stu-id="79f54-511">On Windows Server 2008 R2 or later:</span></span>

1. <span data-ttu-id="79f54-512">開啟「新增角色與功能精靈」。</span><span class="sxs-lookup"><span data-stu-id="79f54-512">Open the **Add Roles and Features Wizard**.</span></span>
1. <span data-ttu-id="79f54-513">在 [選取角色服務] 面板中，開啟 [應用程式開發] 節點。</span><span class="sxs-lookup"><span data-stu-id="79f54-513">In the **Select role services** panel, open the **Application Development** node.</span></span>
1. <span data-ttu-id="79f54-514">選取 [應用程式初始化]的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="79f54-514">Select the check box for **Application Initialization**.</span></span>

<span data-ttu-id="79f54-515">使用下列任一方式為網站啟用應用程式初始化模組：</span><span class="sxs-lookup"><span data-stu-id="79f54-515">Use either of the following approaches to enable the Application Initialization Module for the site:</span></span>

* <span data-ttu-id="79f54-516">使用 IIS 管理員：</span><span class="sxs-lookup"><span data-stu-id="79f54-516">Using IIS Manager:</span></span>

  1. <span data-ttu-id="79f54-517">選取 [連線] 面板中的 [應用程式集區]。</span><span class="sxs-lookup"><span data-stu-id="79f54-517">Select **Application Pools** in the **Connections** panel.</span></span>
  1. <span data-ttu-id="79f54-518">以滑鼠右鍵按一下清單中應用程式的應用程式集區，然後選取 [進階設定]。</span><span class="sxs-lookup"><span data-stu-id="79f54-518">Right-click the app's app pool in the list and select **Advanced Settings**.</span></span>
  1. <span data-ttu-id="79f54-519">預設的 [啟動模式] 是 [OnDemand]。</span><span class="sxs-lookup"><span data-stu-id="79f54-519">The default **Start Mode** is **OnDemand**.</span></span> <span data-ttu-id="79f54-520">將 [啟動模式] 設定為 [AlwaysRunning]。</span><span class="sxs-lookup"><span data-stu-id="79f54-520">Set the **Start Mode** to **AlwaysRunning**.</span></span> <span data-ttu-id="79f54-521">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="79f54-521">Select **OK**.</span></span>
  1. <span data-ttu-id="79f54-522">開啟 [連線] 面板中的 [站台] 節點。</span><span class="sxs-lookup"><span data-stu-id="79f54-522">Open the **Sites** node in the **Connections** panel.</span></span>
  1. <span data-ttu-id="79f54-523">以滑鼠右鍵按一下應用程式，然後選取 [管理網站] > [進階設定]。</span><span class="sxs-lookup"><span data-stu-id="79f54-523">Right-click the app and select **Manage Website** > **Advanced Settings**.</span></span>
  1. <span data-ttu-id="79f54-524">預設 [預先載入已啟用] 設定是 [False]。</span><span class="sxs-lookup"><span data-stu-id="79f54-524">The default **Preload Enabled** setting is **False**.</span></span> <span data-ttu-id="79f54-525">將 [預先載入已啟用] 設定為 [True]。</span><span class="sxs-lookup"><span data-stu-id="79f54-525">Set **Preload Enabled** to **True**.</span></span> <span data-ttu-id="79f54-526">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="79f54-526">Select **OK**.</span></span>

* <span data-ttu-id="79f54-527">使用 *web.config*，新增 `<applicationInitialization>` 元素並將 `doAppInitAfterRestart` 設定為 `true` 至應用程式 *web.config* 檔案中的 `<system.webServer>` 元素：</span><span class="sxs-lookup"><span data-stu-id="79f54-527">Using *web.config*, add the `<applicationInitialization>` element with `doAppInitAfterRestart` set to `true` to the `<system.webServer>` elements in the app's *web.config* file:</span></span>

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

### <a name="idle-timeout"></a><span data-ttu-id="79f54-528">閒置逾時</span><span class="sxs-lookup"><span data-stu-id="79f54-528">Idle Timeout</span></span>

<span data-ttu-id="79f54-529">*僅適用於應用程式裝載同處理序。*</span><span class="sxs-lookup"><span data-stu-id="79f54-529">*Only applies to apps hosted in-process.*</span></span>

<span data-ttu-id="79f54-530">若要防止應用程式閒置，請使用 IIS 管理員設定應用程式集區的閒置逾時：</span><span class="sxs-lookup"><span data-stu-id="79f54-530">To prevent the app from idling, set the app pool's idle timeout using IIS Manager:</span></span>

1. <span data-ttu-id="79f54-531">選取 [連線] 面板中的 [應用程式集區]。</span><span class="sxs-lookup"><span data-stu-id="79f54-531">Select **Application Pools** in the **Connections** panel.</span></span>
1. <span data-ttu-id="79f54-532">以滑鼠右鍵按一下清單中應用程式的應用程式集區，然後選取 [進階設定]。</span><span class="sxs-lookup"><span data-stu-id="79f54-532">Right-click the app's app pool in the list and select **Advanced Settings**.</span></span>
1. <span data-ttu-id="79f54-533">預設 [閒置逾時 (分鐘)] 是 **20** 分鐘。</span><span class="sxs-lookup"><span data-stu-id="79f54-533">The default **Idle Time-out (minutes)** is **20** minutes.</span></span> <span data-ttu-id="79f54-534">將 [閒置逾時 (分鐘)] 設定為 **0** (零)。</span><span class="sxs-lookup"><span data-stu-id="79f54-534">Set the **Idle Time-out (minutes)** to **0** (zero).</span></span> <span data-ttu-id="79f54-535">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="79f54-535">Select **OK**.</span></span>
1. <span data-ttu-id="79f54-536">回收背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="79f54-536">Recycle the worker process.</span></span>

<span data-ttu-id="79f54-537">若要防止應用程式裝載[非同處理序](xref:fundamentals/servers/index#out-of-process-hosting-model)逾時，請使用下列任一方式：</span><span class="sxs-lookup"><span data-stu-id="79f54-537">To prevent apps hosted [out-of-process](xref:fundamentals/servers/index#out-of-process-hosting-model) from timing out, use either of the following approaches:</span></span>

* <span data-ttu-id="79f54-538">從外部服務對應用程式執行 Ping 以讓它繼續執行。</span><span class="sxs-lookup"><span data-stu-id="79f54-538">Ping the app from an external service in order to keep it running.</span></span>
* <span data-ttu-id="79f54-539">若應用程式只裝載背景服務，避免 IIS 裝載並使用 [Windows 服務來裝載 ASP.NET Core 應用程式](xref:host-and-deploy/windows-service)。</span><span class="sxs-lookup"><span data-stu-id="79f54-539">If the app only hosts background services, avoid IIS hosting and use a [Windows Service to host the ASP.NET Core app](xref:host-and-deploy/windows-service).</span></span>

### <a name="application-initialization-module-and-idle-timeout-additional-resources"></a><span data-ttu-id="79f54-540">應用程式初始化模組與閒置逾時額外資源</span><span class="sxs-lookup"><span data-stu-id="79f54-540">Application Initialization Module and Idle Timeout additional resources</span></span>

* [<span data-ttu-id="79f54-541">IIS 8.0 應用程式初始化</span><span class="sxs-lookup"><span data-stu-id="79f54-541">IIS 8.0 Application Initialization</span></span>](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)
* <span data-ttu-id="79f54-542">[應用程式初始化 \<applicationInitialization>](/iis/configuration/system.webserver/applicationinitialization/)。</span><span class="sxs-lookup"><span data-stu-id="79f54-542">[Application Initialization \<applicationInitialization>](/iis/configuration/system.webserver/applicationinitialization/).</span></span>
* <span data-ttu-id="79f54-543">[應用程式集區的處理序模組設定 \<processModel>](/iis/configuration/system.applicationhost/applicationpools/add/processmodel)。</span><span class="sxs-lookup"><span data-stu-id="79f54-543">[Process Model Settings for an Application Pool \<processModel>](/iis/configuration/system.applicationhost/applicationpools/add/processmodel).</span></span>

::: moniker-end

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="79f54-544">IIS 系統管理員的部署資源</span><span class="sxs-lookup"><span data-stu-id="79f54-544">Deployment resources for IIS administrators</span></span>

<span data-ttu-id="79f54-545">請參閱 IIS 文件以深入了解 IIS。</span><span class="sxs-lookup"><span data-stu-id="79f54-545">Learn about IIS in-depth in the IIS documentation.</span></span>  
[<span data-ttu-id="79f54-546">IIS 文件</span><span class="sxs-lookup"><span data-stu-id="79f54-546">IIS documentation</span></span>](/iis)

<span data-ttu-id="79f54-547">了解 .NET Core 應用程式部署模型。</span><span class="sxs-lookup"><span data-stu-id="79f54-547">Learn about .NET Core app deployment models.</span></span>  
[<span data-ttu-id="79f54-548">.NET Core 應用程式部署</span><span class="sxs-lookup"><span data-stu-id="79f54-548">.NET Core application deployment</span></span>](/dotnet/core/deploying/)

<span data-ttu-id="79f54-549">了解 ASP.NET Core 模組如何讓 Kestrel Web 伺服器將 IIS 或 IIS Express 作為反向 Proxy 伺服器使用。</span><span class="sxs-lookup"><span data-stu-id="79f54-549">Learn how the ASP.NET Core Module allows the Kestrel web server to use IIS or IIS Express as a reverse proxy server.</span></span>  
[<span data-ttu-id="79f54-550">ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="79f54-550">ASP.NET Core Module</span></span>](xref:host-and-deploy/aspnet-core-module)

<span data-ttu-id="79f54-551">了解如何設定 ASP.NET Core 模組以裝載 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="79f54-551">Learn how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span>  
[<span data-ttu-id="79f54-552">ASP.NET Core 模組組態參考</span><span class="sxs-lookup"><span data-stu-id="79f54-552">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)

<span data-ttu-id="79f54-553">了解已發行之 ASP.NET Core 應用程式的目錄結構。</span><span class="sxs-lookup"><span data-stu-id="79f54-553">Learn about the directory structure of published ASP.NET Core apps.</span></span>  
[<span data-ttu-id="79f54-554">目錄結構</span><span class="sxs-lookup"><span data-stu-id="79f54-554">Directory structure</span></span>](xref:host-and-deploy/directory-structure)

<span data-ttu-id="79f54-555">探索 ASP.NET Core 應用程式的使用中和非使用中 IIS 模組，管理 IIS 模組的方式。</span><span class="sxs-lookup"><span data-stu-id="79f54-555">Discover active and inactive IIS modules for ASP.NET Core apps and how to manage IIS modules.</span></span>  
[<span data-ttu-id="79f54-556">IIS 模組</span><span class="sxs-lookup"><span data-stu-id="79f54-556">IIS modules</span></span>](xref:host-and-deploy/iis/troubleshoot)

<span data-ttu-id="79f54-557">了解如何診斷 ASP.NET Core 應用程式的 IIS 部署問題。</span><span class="sxs-lookup"><span data-stu-id="79f54-557">Learn how to diagnose problems with IIS deployments of ASP.NET Core apps.</span></span>  
[<span data-ttu-id="79f54-558">疑難排解</span><span class="sxs-lookup"><span data-stu-id="79f54-558">Troubleshoot</span></span>](xref:host-and-deploy/iis/troubleshoot)

<span data-ttu-id="79f54-559">區分在 IIS 上裝載 ASP.NET Core 應用程式時的常見錯誤。</span><span class="sxs-lookup"><span data-stu-id="79f54-559">Distinguish common errors when hosting ASP.NET Core apps on IIS.</span></span>  
[<span data-ttu-id="79f54-560">Azure App Service 和 IIS 常見的錯誤參考</span><span class="sxs-lookup"><span data-stu-id="79f54-560">Common errors reference for Azure App Service and IIS</span></span>](xref:host-and-deploy/azure-iis-errors-reference)

## <a name="additional-resources"></a><span data-ttu-id="79f54-561">其他資源</span><span class="sxs-lookup"><span data-stu-id="79f54-561">Additional resources</span></span>

* <xref:test/troubleshoot>
* [<span data-ttu-id="79f54-562">ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="79f54-562">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="79f54-563">Microsoft IIS 官方網站</span><span class="sxs-lookup"><span data-stu-id="79f54-563">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="79f54-564">Windows Server 技術內容庫</span><span class="sxs-lookup"><span data-stu-id="79f54-564">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="79f54-565">ISS 上的 HTTP/2</span><span class="sxs-lookup"><span data-stu-id="79f54-565">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
* <xref:host-and-deploy/iis/transform-webconfig>
