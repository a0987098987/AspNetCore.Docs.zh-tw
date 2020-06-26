---
title: 在 Linux 上使用 Nginx 裝載 ASP.NET Core
author: rick-anderson
description: 了解如何在 Ubuntu 16.04 上將 Nginx 設定為反向 Proxy，以將 HTTP 流量轉送至在 Kestrel 上執行的 ASP.NET Core Web 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: c936ff9a7aadd21ce99a0c37184ae8cf911c3070
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85403972"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="77ce7-103">在 Linux 上使用 Nginx 裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="77ce7-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="77ce7-104">由 [Sourabh Shirhatti](https://twitter.com/sshirhatti) 提供</span><span class="sxs-lookup"><span data-stu-id="77ce7-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="77ce7-105">本指南說明在 Ubuntu 16.04 伺服器上設定生產環境就緒的 ASP.NET Core 環境。</span><span class="sxs-lookup"><span data-stu-id="77ce7-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="77ce7-106">這些指示可能使用較新版本的 Ubuntu，但未經測試。</span><span class="sxs-lookup"><span data-stu-id="77ce7-106">These instructions likely work with newer versions of Ubuntu, but the instructions haven't been tested with newer versions.</span></span>

<span data-ttu-id="77ce7-107">如需有關 ASP.NET Core 支援之其他 Linux 發行版本的資訊，請參閱 [Linux 上 .NET Core 的必要條件](/dotnet/core/linux-prerequisites)。</span><span class="sxs-lookup"><span data-stu-id="77ce7-107">For information on other Linux distributions supported by ASP.NET Core, see [Prerequisites for .NET Core on Linux](/dotnet/core/linux-prerequisites).</span></span>

> [!NOTE]
> <span data-ttu-id="77ce7-108">針對 Ubuntu 14.04，建議使用 *supervisord*作為監視 Kestrel 處理序的解決方案。</span><span class="sxs-lookup"><span data-stu-id="77ce7-108">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="77ce7-109">在 Ubuntu 14.04 上無法使用 *systemd*。</span><span class="sxs-lookup"><span data-stu-id="77ce7-109">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="77ce7-110">如需 Ubuntu 14.04 指示，請參閱[本主題前一版本](https://github.com/dotnet/AspNetCore.Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)。</span><span class="sxs-lookup"><span data-stu-id="77ce7-110">For Ubuntu 14.04 instructions, see the [previous version of this topic](https://github.com/dotnet/AspNetCore.Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="77ce7-111">本指南：</span><span class="sxs-lookup"><span data-stu-id="77ce7-111">This guide:</span></span>

* <span data-ttu-id="77ce7-112">將現有的 ASP.NET Core 應用程式放在反向 Proxy 伺服器後方。</span><span class="sxs-lookup"><span data-stu-id="77ce7-112">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="77ce7-113">設定反向 Proxy 伺服器以將要求轉送至 Kestrel 網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="77ce7-113">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="77ce7-114">確保 Web 應用程式在啟動時以精靈的形式執行。</span><span class="sxs-lookup"><span data-stu-id="77ce7-114">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="77ce7-115">設定程序管理工具以協助重新啟動 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="77ce7-115">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="77ce7-116">必要條件</span><span class="sxs-lookup"><span data-stu-id="77ce7-116">Prerequisites</span></span>

1. <span data-ttu-id="77ce7-117">以 sudo 權限使用標準使用者帳戶存取 Ubuntu 16.04 伺服器。</span><span class="sxs-lookup"><span data-stu-id="77ce7-117">Access to an Ubuntu 16.04 server with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="77ce7-118">在伺服器上安裝 .NET Core 執行階段。</span><span class="sxs-lookup"><span data-stu-id="77ce7-118">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="77ce7-119">請造訪[下載 .Net Core 頁面](https://dotnet.microsoft.com/download/dotnet-core)。</span><span class="sxs-lookup"><span data-stu-id="77ce7-119">Visit the [Download .NET Core page](https://dotnet.microsoft.com/download/dotnet-core).</span></span>
   1. <span data-ttu-id="77ce7-120">選取最新的非預覽 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="77ce7-120">Select the latest non-preview .NET Core version.</span></span>
   1. <span data-ttu-id="77ce7-121">在 [**執行應用程式-運行**時間] 下的表格中，下載最新的非預覽執行時間。</span><span class="sxs-lookup"><span data-stu-id="77ce7-121">Download the latest non-preview runtime in the table under **Run apps - Runtime**.</span></span>
   1. <span data-ttu-id="77ce7-122">選取 [Linux**套件管理員指示**] 連結，並遵循 ubuntu 版本的 ubuntu 指示。</span><span class="sxs-lookup"><span data-stu-id="77ce7-122">Select the Linux **Package manager instructions** link and follow the Ubuntu instructions for your version of Ubuntu.</span></span>
1. <span data-ttu-id="77ce7-123">現有的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="77ce7-123">An existing ASP.NET Core app.</span></span>

<span data-ttu-id="77ce7-124">在未來升級共用架構之後的任何時間點，重新開機伺服器所裝載的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="77ce7-124">At any point in the future after upgrading the shared framework, restart the ASP.NET Core apps hosted by the server.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="77ce7-125">跨應用程式發佈與複製</span><span class="sxs-lookup"><span data-stu-id="77ce7-125">Publish and copy over the app</span></span>

<span data-ttu-id="77ce7-126">為[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="77ce7-126">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="77ce7-127">如果應用程式在本機執行且未設定為進行安全連線 (HTTPS)，請採用下列任一方法：</span><span class="sxs-lookup"><span data-stu-id="77ce7-127">If the app is run locally and isn't configured to make secure connections (HTTPS), adopt either of the following approaches:</span></span>

* <span data-ttu-id="77ce7-128">設定應用程式以處理安全的本機連線。</span><span class="sxs-lookup"><span data-stu-id="77ce7-128">Configure the app to handle secure local connections.</span></span> <span data-ttu-id="77ce7-129">如需詳細資訊，請參閱 [HTTPS 組態](#https-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="77ce7-129">For more information, see the [HTTPS configuration](#https-configuration) section.</span></span>
* <span data-ttu-id="77ce7-130">從 *Properties/launchSettings.json* 檔案中的 `applicationUrl` 屬性移除 `https://localhost:5001` (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="77ce7-130">Remove `https://localhost:5001` (if present) from the `applicationUrl` property in the *Properties/launchSettings.json* file.</span></span>

<span data-ttu-id="77ce7-131">從開發環境執行 [dotnet publish](/dotnet/core/tools/dotnet-publish) 將應用程式封裝到可在伺服器上執行的目錄 (例如，*bin/Release/&lt;target_framework_moniker&gt;/publish*)：</span><span class="sxs-lookup"><span data-stu-id="77ce7-131">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```dotnetcli
dotnet publish --configuration Release
```

<span data-ttu-id="77ce7-132">如果您不想在伺服器上維護 .NET Core 執行階段，應用程式也可以發佈為[獨立式部署](/dotnet/core/deploying/#self-contained-deployments-scd)。</span><span class="sxs-lookup"><span data-stu-id="77ce7-132">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="77ce7-133">使用整合至組織工作流程的工具 (SCP、SFTP 等等) 將 ASP.NET Core 應用程式複製到伺服器。</span><span class="sxs-lookup"><span data-stu-id="77ce7-133">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="77ce7-134">Web 應用程式通常可在 *var* 目錄下找到 (例如 *var/www/helloapp*)。</span><span class="sxs-lookup"><span data-stu-id="77ce7-134">It's common to locate web apps under the *var* directory (for example, *var/www/helloapp*).</span></span>

> [!NOTE]
> <span data-ttu-id="77ce7-135">在生產環境部署案例中，持續整合工作流程會執行發佈應用程式並將資產複製到伺服器的工作。</span><span class="sxs-lookup"><span data-stu-id="77ce7-135">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

<span data-ttu-id="77ce7-136">測試應用程式：</span><span class="sxs-lookup"><span data-stu-id="77ce7-136">Test the app:</span></span>

1. <span data-ttu-id="77ce7-137">從命令列執行應用程式：`dotnet <app_assembly>.dll`。</span><span class="sxs-lookup"><span data-stu-id="77ce7-137">From the command line, run the app: `dotnet <app_assembly>.dll`.</span></span>
1. <span data-ttu-id="77ce7-138">在瀏覽器中，巡覽至 `http://<serveraddress>:<port>` 以確認應用程式可在 Linux 本機上正常運作。</span><span class="sxs-lookup"><span data-stu-id="77ce7-138">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux locally.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="77ce7-139">設定反向 Proxy 伺服器</span><span class="sxs-lookup"><span data-stu-id="77ce7-139">Configure a reverse proxy server</span></span>

<span data-ttu-id="77ce7-140">反向 Proxy 是為動態 Web 應用程式提供服務的常見設定。</span><span class="sxs-lookup"><span data-stu-id="77ce7-140">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="77ce7-141">反向 Proxy 會終止 HTTP 要求，並將它轉送至 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="77ce7-141">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="use-a-reverse-proxy-server"></a><span data-ttu-id="77ce7-142">使用反向 Proxy 伺服器</span><span class="sxs-lookup"><span data-stu-id="77ce7-142">Use a reverse proxy server</span></span>

<span data-ttu-id="77ce7-143">Kestrel 非常適用於從 ASP.NET Core 提供動態內容。</span><span class="sxs-lookup"><span data-stu-id="77ce7-143">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="77ce7-144">不過，Web 服務功能不像 IIS、Apache 或 Nginx 這類伺服器那樣豐富。</span><span class="sxs-lookup"><span data-stu-id="77ce7-144">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="77ce7-145">反向 Proxy 伺服器可以讓 HTTP 伺服器卸下提供靜態內容、快取要求、壓縮要求及終止 HTTPS 等工作的負擔。</span><span class="sxs-lookup"><span data-stu-id="77ce7-145">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and HTTPS termination from the HTTP server.</span></span> <span data-ttu-id="77ce7-146">反向 Proxy 伺服器可能位在專用電腦上，或可能與 HTTP 伺服器一起部署。</span><span class="sxs-lookup"><span data-stu-id="77ce7-146">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="77ce7-147">為達到本指南的目的，使用 Nginx 的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="77ce7-147">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="77ce7-148">它會在相同的伺服器上和 HTTP 伺服器一起執行。</span><span class="sxs-lookup"><span data-stu-id="77ce7-148">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="77ce7-149">您可以根據需求，選擇不同的設定。</span><span class="sxs-lookup"><span data-stu-id="77ce7-149">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="77ce7-150">因為反向 proxy 會轉送要求，所以請使用[AspNetCore. 來自 microsoft.aspnetcore.HTTPoverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/)套件中的[轉送標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="77ce7-150">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="77ce7-151">此中介軟體會使用 `X-Forwarded-Proto` 標頭來更新 `Request.Scheme`，以便讓重新導向 URI 及其他安全性原則正確運作。</span><span class="sxs-lookup"><span data-stu-id="77ce7-151">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>


[!INCLUDE[](~/includes/ForwardedHeaders.md)]

<span data-ttu-id="77ce7-152"><xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> `Startup.Configure` 呼叫其他中介軟體之前，先叫用頂端的方法。</span><span class="sxs-lookup"><span data-stu-id="77ce7-152">Invoke the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> method at the top of `Startup.Configure` before calling other middleware.</span></span> <span data-ttu-id="77ce7-153">請設定中介軟體來轉送 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭：</span><span class="sxs-lookup"><span data-stu-id="77ce7-153">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
// using Microsoft.AspNetCore.HttpOverrides;

app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

<span data-ttu-id="77ce7-154">如果未將任何 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> 指定給中介軟體，則要轉送的預設標頭會是 `None`。</span><span class="sxs-lookup"><span data-stu-id="77ce7-154">If no <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="77ce7-155">在預設情況下，在回送位址 (127.0.0.0/8, [::1]) 上執行的 Proxy (包括標準的本機位址 (127.0.0.1)) 是受信任的。</span><span class="sxs-lookup"><span data-stu-id="77ce7-155">Proxies running on loopback addresses (127.0.0.0/8, [::1]), including the standard localhost address (127.0.0.1), are trusted by default.</span></span> <span data-ttu-id="77ce7-156">如果組織內有其他受信任的 Proxy 或網路處理網際網路與網頁伺服器之間的要求，請使用 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>，將其新增至 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> 或 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> 清單。</span><span class="sxs-lookup"><span data-stu-id="77ce7-156">If other trusted proxies or networks within the organization handle requests between the Internet and the web server, add them to the list of <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> or <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span></span> <span data-ttu-id="77ce7-157">下列範例會將 IP 位址 10.0.0.100 的受信任 Proxy 伺服器新增至 `Startup.ConfigureServices` 中「轉送的標頭中介軟體」的 `KnownProxies`：</span><span class="sxs-lookup"><span data-stu-id="77ce7-157">The following example adds a trusted proxy server at IP address 10.0.0.100 to the Forwarded Headers Middleware `KnownProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
// using System.Net;

services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

<span data-ttu-id="77ce7-158">如需詳細資訊，請參閱 <xref:host-and-deploy/proxy-load-balancer> 。</span><span class="sxs-lookup"><span data-stu-id="77ce7-158">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

### <a name="install-nginx"></a><span data-ttu-id="77ce7-159">安裝 Nginx</span><span class="sxs-lookup"><span data-stu-id="77ce7-159">Install Nginx</span></span>

<span data-ttu-id="77ce7-160">使用 `apt-get` 來安裝 Nginx。</span><span class="sxs-lookup"><span data-stu-id="77ce7-160">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="77ce7-161">安裝程式建立的 *systemd* init 指令碼，會在系統啟動時將 Nginx 執行為精靈。</span><span class="sxs-lookup"><span data-stu-id="77ce7-161">The installer creates a *systemd* init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="77ce7-162">請遵循 [Nginx：Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages) (Nginx：官方 Debian/Ubuntu 套件) 中適用於 Ubuntu 的安裝指示。</span><span class="sxs-lookup"><span data-stu-id="77ce7-162">Follow the installation instructions for Ubuntu at [Nginx: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span></span>

> [!NOTE]
> <span data-ttu-id="77ce7-163">如果需要選用的 Nginx 模組，可能要從來源建置 Nginx。</span><span class="sxs-lookup"><span data-stu-id="77ce7-163">If optional Nginx modules are required, building Nginx from source might be required.</span></span>

<span data-ttu-id="77ce7-164">因為 Nginx 是第一次安裝，請透過執行明確啟動它：</span><span class="sxs-lookup"><span data-stu-id="77ce7-164">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="77ce7-165">確認瀏覽器會顯示 Nginx 的預設登陸頁面。</span><span class="sxs-lookup"><span data-stu-id="77ce7-165">Verify a browser displays the default landing page for Nginx.</span></span> <span data-ttu-id="77ce7-166">登陸頁面位於 `http://<server_IP_address>/index.nginx-debian.html`。</span><span class="sxs-lookup"><span data-stu-id="77ce7-166">The landing page is reachable at `http://<server_IP_address>/index.nginx-debian.html`.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="77ce7-167">設定 Nginx</span><span class="sxs-lookup"><span data-stu-id="77ce7-167">Configure Nginx</span></span>

<span data-ttu-id="77ce7-168">若要將 Nginx 設定為反向 Proxy 以將要求轉送至您的 ASP.NET Core 應用程式，請修改 */etc/nginx/sites-available/default*。</span><span class="sxs-lookup"><span data-stu-id="77ce7-168">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="77ce7-169">以文字編輯器開啟它，並以下列項目取代內容：</span><span class="sxs-lookup"><span data-stu-id="77ce7-169">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

<span data-ttu-id="77ce7-170">如果應用程式是 Blazor Server 依賴 websocket 的應用程式 SignalR ，請參閱，以 <xref:blazor/host-and-deploy/server#linux-with-nginx> 取得如何設定標頭的相關資訊 `Connection` 。</span><span class="sxs-lookup"><span data-stu-id="77ce7-170">If the app is a Blazor Server app that relies on SignalR WebSockets, see <xref:blazor/host-and-deploy/server#linux-with-nginx> for information on how to set the `Connection` header.</span></span>

<span data-ttu-id="77ce7-171">當沒有任何與 `server_name` 相符的項目時，Nginx 會使用預設伺服器。</span><span class="sxs-lookup"><span data-stu-id="77ce7-171">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="77ce7-172">如果未定義任何預設伺服器，則設定檔中的第一個伺服器就是預設伺服器。</span><span class="sxs-lookup"><span data-stu-id="77ce7-172">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="77ce7-173">最佳做法是，在您的設定檔中新增一個會傳回狀態碼 444 的特定預設伺服器。</span><span class="sxs-lookup"><span data-stu-id="77ce7-173">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="77ce7-174">預設伺服器設定範例如下：</span><span class="sxs-lookup"><span data-stu-id="77ce7-174">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="77ce7-175">使用上述設定檔和預設伺服器時，Nginx 會在連接埠 80 接受主機標頭為 `example.com` 或 `*.example.com` 的公用流量。</span><span class="sxs-lookup"><span data-stu-id="77ce7-175">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="77ce7-176">不符合這些主機的要求將不會轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="77ce7-176">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="77ce7-177">Nginx 會將相符的要求轉送至位於 `http://localhost:5000` 的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="77ce7-177">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="77ce7-178">如需詳細資訊，請參閱 [nginx 如何處理要求](https://nginx.org/docs/http/request_processing.html) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="77ce7-178">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span> <span data-ttu-id="77ce7-179">若要變更 Kestrel 的 IP/連接埠，請參閱 [Kestrel：端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration)。</span><span class="sxs-lookup"><span data-stu-id="77ce7-179">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="77ce7-180">如果無法指定適當的 [server_name 指示詞](https://nginx.org/docs/http/server_names.html)，就會讓應用程式暴露在安全性弱點的風險下。</span><span class="sxs-lookup"><span data-stu-id="77ce7-180">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="77ce7-181">若您擁有整個父網域 (相對於易受攻擊的 `*.com`) 的控制權，子網域萬用字元繫結 (例如 `*.example.com`) 就沒有此安全性風險。</span><span class="sxs-lookup"><span data-stu-id="77ce7-181">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="77ce7-182">如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="77ce7-182">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="77ce7-183">建立 Nginx 設定之後，請執行 `sudo nginx -t` 來確認設定檔的語法。</span><span class="sxs-lookup"><span data-stu-id="77ce7-183">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="77ce7-184">如果設定檔測試成功，請執行 `sudo nginx -s reload` 來強制 Nginx 套用這些變更。</span><span class="sxs-lookup"><span data-stu-id="77ce7-184">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

<span data-ttu-id="77ce7-185">直接在伺服器上執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="77ce7-185">To directly run the app on the server:</span></span>

1. <span data-ttu-id="77ce7-186">巡覽至應用程式目錄。</span><span class="sxs-lookup"><span data-stu-id="77ce7-186">Navigate to the app's directory.</span></span>
1. <span data-ttu-id="77ce7-187">執行應用程式：`dotnet <app_assembly.dll>`，其中 `app_assembly.dll` 是應用程式的組件檔名稱。</span><span class="sxs-lookup"><span data-stu-id="77ce7-187">Run the app: `dotnet <app_assembly.dll>`, where `app_assembly.dll` is the assembly file name of the app.</span></span>

<span data-ttu-id="77ce7-188">如果應用程式在伺服器上執行，但無法透過網際網路回應，請檢查伺服器的防火牆，確認連接埠 80 已開啟。</span><span class="sxs-lookup"><span data-stu-id="77ce7-188">If the app runs on the server but fails to respond over the Internet, check the server's firewall and confirm that port 80 is open.</span></span> <span data-ttu-id="77ce7-189">如果使用的是 Azure Ubuntu VM，請新增啓用輸入連接埠 80 流量的網路安全性群組 (NSG) 規則。</span><span class="sxs-lookup"><span data-stu-id="77ce7-189">If using an Azure Ubuntu VM, add a Network Security Group (NSG) rule that enables inbound port 80 traffic.</span></span> <span data-ttu-id="77ce7-190">沒有必要啟用輸出連接埠 80 規則，因為啓用輸入規則時會自動授與輸出流量。</span><span class="sxs-lookup"><span data-stu-id="77ce7-190">There's no need to enable an outbound port 80 rule, as the outbound traffic is automatically granted when the inbound rule is enabled.</span></span>

<span data-ttu-id="77ce7-191">應用程式測試完成後，請在命令提示字元以 `Ctrl+C` 關閉應用程式。</span><span class="sxs-lookup"><span data-stu-id="77ce7-191">When done testing the app, shut the app down with `Ctrl+C` at the command prompt.</span></span>

## <a name="monitor-the-app"></a><span data-ttu-id="77ce7-192">監視應用程式</span><span class="sxs-lookup"><span data-stu-id="77ce7-192">Monitor the app</span></span>

<span data-ttu-id="77ce7-193">伺服器已設定完成，可將對 `http://<serveraddress>:80` 發出的要求轉送給在位於 `http://127.0.0.1:5000` 的 Kestrel 上執行的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="77ce7-193">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="77ce7-194">不過，並未設定 Nginx 來管理 Kestrel 處理序。</span><span class="sxs-lookup"><span data-stu-id="77ce7-194">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="77ce7-195">您可以使用 *systemd* 來建立服務檔案，以啟動並監視基礎 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="77ce7-195">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="77ce7-196">*systemd*是 init 系統，提供許多強大的功能來啟動、停止和管理進程。</span><span class="sxs-lookup"><span data-stu-id="77ce7-196">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="77ce7-197">建立服務檔</span><span class="sxs-lookup"><span data-stu-id="77ce7-197">Create the service file</span></span>

<span data-ttu-id="77ce7-198">建立服務定義檔：</span><span class="sxs-lookup"><span data-stu-id="77ce7-198">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-helloapp.service
```

<span data-ttu-id="77ce7-199">以下是一個應用程式範例服務檔：</span><span class="sxs-lookup"><span data-stu-id="77ce7-199">The following is an example service file for the app:</span></span>

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/www/helloapp
ExecStart=/usr/bin/dotnet /var/www/helloapp/helloapp.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="77ce7-200">在上述範例中，管理服務的使用者是由 `User` 選項指定。</span><span class="sxs-lookup"><span data-stu-id="77ce7-200">In the preceding example, the user that manages the service is specified by the `User` option.</span></span> <span data-ttu-id="77ce7-201">使用者（ `www-data` ）必須存在，且具有應用程式檔案的適當擁有權。</span><span class="sxs-lookup"><span data-stu-id="77ce7-201">The user (`www-data`) must exist and have proper ownership of the app's files.</span></span>

<span data-ttu-id="77ce7-202">使用 `TimeoutStopSec` 可設定應用程式收到初始中斷訊號之後等待關閉的時間。</span><span class="sxs-lookup"><span data-stu-id="77ce7-202">Use `TimeoutStopSec` to configure the duration of time to wait for the app to shut down after it receives the initial interrupt signal.</span></span> <span data-ttu-id="77ce7-203">如果應用程式在此期間後未關閉，則會發出 SIGKILL 來終止應用程式。</span><span class="sxs-lookup"><span data-stu-id="77ce7-203">If the app doesn't shut down in this period, SIGKILL is issued to terminate the app.</span></span> <span data-ttu-id="77ce7-204">提供不具單位的秒值 (例如 `150`)、時間範圍值 (例如 `2min 30s`) 或 `infinity` (表示停用逾時)。</span><span class="sxs-lookup"><span data-stu-id="77ce7-204">Provide the value as unitless seconds (for example, `150`), a time span value (for example, `2min 30s`), or `infinity` to disable the timeout.</span></span> <span data-ttu-id="77ce7-205">`TimeoutStopSec` 在管理員設定檔 (*systemd-system.conf*、*system.conf.d*、*systemd-user.conf*、*user.conf.d*) 的預設值為 `DefaultTimeoutStopSec`。</span><span class="sxs-lookup"><span data-stu-id="77ce7-205">`TimeoutStopSec` defaults to the value of `DefaultTimeoutStopSec` in the manager configuration file (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*, *user.conf.d*).</span></span> <span data-ttu-id="77ce7-206">大多數發行版本的預設逾時為 90 秒。</span><span class="sxs-lookup"><span data-stu-id="77ce7-206">The default timeout for most distributions is 90 seconds.</span></span>

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

<span data-ttu-id="77ce7-207">Linux 的檔案系統會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="77ce7-207">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="77ce7-208">將 ASPNETCORE_ENVIRONMENT 設定為 "Production" 會導致搜尋 *appsettings.Production.json* 設定檔，而不是搜尋 *appsettings.production.json*。</span><span class="sxs-lookup"><span data-stu-id="77ce7-208">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

<span data-ttu-id="77ce7-209">有些值 (例如 SQL 連接字串) 必須以逸出方式處理，設定提供者才能讀取環境變數。</span><span class="sxs-lookup"><span data-stu-id="77ce7-209">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="77ce7-210">請使用下列命令來產生要在設定檔中使用並已適當逸出的值：</span><span class="sxs-lookup"><span data-stu-id="77ce7-210">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>

```console
systemd-escape "<value-to-escape>"
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="77ce7-211">環境變數名稱不支援冒號 (`:`) 分隔符號。</span><span class="sxs-lookup"><span data-stu-id="77ce7-211">Colon (`:`) separators aren't supported in environment variable names.</span></span> <span data-ttu-id="77ce7-212">請使用雙底線 (`__`) 來取代冒號。</span><span class="sxs-lookup"><span data-stu-id="77ce7-212">Use a double underscore (`__`) in place of a colon.</span></span> <span data-ttu-id="77ce7-213">[環境變數組態提供者](xref:fundamentals/configuration/index#environment-variables)會在將環境變數讀入組態時，將雙底線轉換為冒號。</span><span class="sxs-lookup"><span data-stu-id="77ce7-213">The [Environment Variables configuration provider](xref:fundamentals/configuration/index#environment-variables) converts double-underscores into colons when environment variables are read into configuration.</span></span> <span data-ttu-id="77ce7-214">在下列範例中，連接字串索引鍵 `ConnectionStrings:DefaultConnection` 會設定為服務定義檔中的 `ConnectionStrings__DefaultConnection`：</span><span class="sxs-lookup"><span data-stu-id="77ce7-214">In the following example, the connection string key `ConnectionStrings:DefaultConnection` is set into the service definition file as `ConnectionStrings__DefaultConnection`:</span></span>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="77ce7-215">環境變數名稱不支援冒號 (`:`) 分隔符號。</span><span class="sxs-lookup"><span data-stu-id="77ce7-215">Colon (`:`) separators aren't supported in environment variable names.</span></span> <span data-ttu-id="77ce7-216">請使用雙底線 (`__`) 來取代冒號。</span><span class="sxs-lookup"><span data-stu-id="77ce7-216">Use a double underscore (`__`) in place of a colon.</span></span> <span data-ttu-id="77ce7-217">[環境變數組態提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)會在將環境變數讀入組態時，將雙底線轉換為冒號。</span><span class="sxs-lookup"><span data-stu-id="77ce7-217">The [Environment Variables configuration provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider) converts double-underscores into colons when environment variables are read into configuration.</span></span> <span data-ttu-id="77ce7-218">在下列範例中，連接字串索引鍵 `ConnectionStrings:DefaultConnection` 會設定為服務定義檔中的 `ConnectionStrings__DefaultConnection`：</span><span class="sxs-lookup"><span data-stu-id="77ce7-218">In the following example, the connection string key `ConnectionStrings:DefaultConnection` is set into the service definition file as `ConnectionStrings__DefaultConnection`:</span></span>

::: moniker-end

```
Environment=ConnectionStrings__DefaultConnection={Connection String}
```

<span data-ttu-id="77ce7-219">儲存檔案並啟用服務。</span><span class="sxs-lookup"><span data-stu-id="77ce7-219">Save the file and enable the service.</span></span>

```bash
sudo systemctl enable kestrel-helloapp.service
```

<span data-ttu-id="77ce7-220">啟動服務並確認它正在執行。</span><span class="sxs-lookup"><span data-stu-id="77ce7-220">Start the service and verify that it's running.</span></span>

```
sudo systemctl start kestrel-helloapp.service
sudo systemctl status kestrel-helloapp.service

◝ kestrel-helloapp.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-helloapp.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-helloapp.service
            └─9021 /usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
```

<span data-ttu-id="77ce7-221">設定好反向 Proxy 並透過 systemd 管理 Kestrel 之後，Web 應用程式便已完全設定妥當，而從本機電腦瀏覽器透過 `http://localhost` 即可存取它。</span><span class="sxs-lookup"><span data-stu-id="77ce7-221">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="77ce7-222">此外，也可以從遠端電腦存取它，除非遭到任何防火牆封鎖。</span><span class="sxs-lookup"><span data-stu-id="77ce7-222">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="77ce7-223">檢查回應標頭時，`Server` 標頭會顯示是由 Kestrel 為 ASP.NET Core 應用程式提供服務。</span><span class="sxs-lookup"><span data-stu-id="77ce7-223">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="view-logs"></a><span data-ttu-id="77ce7-224">檢視記錄</span><span class="sxs-lookup"><span data-stu-id="77ce7-224">View logs</span></span>

<span data-ttu-id="77ce7-225">由於是使用 `systemd` 來管理使用 Kestrel 的 Web 應用程式，因此會將所有事件和處理序都記錄在集中式日誌中。</span><span class="sxs-lookup"><span data-stu-id="77ce7-225">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="77ce7-226">不過，此日誌包含 `systemd` 管理的所有服務和處理程序的所有項目。</span><span class="sxs-lookup"><span data-stu-id="77ce7-226">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="77ce7-227">若要檢視 `kestrel-helloapp.service` 的特定項目，請使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="77ce7-227">To view the `kestrel-helloapp.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service
```

<span data-ttu-id="77ce7-228">如需進一步篩選，例如 `--since today`、`--until 1 hour ago` 或這些項目的組合等時間選項，可以減少傳回的項目數量。</span><span class="sxs-lookup"><span data-stu-id="77ce7-228">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a><span data-ttu-id="77ce7-229">資料保護</span><span class="sxs-lookup"><span data-stu-id="77ce7-229">Data protection</span></span>

<span data-ttu-id="77ce7-230">[ASP.NET Core 資料保護堆疊](xref:security/data-protection/introduction)由數個 ASP.NET Core[ 中介軟體](xref:fundamentals/middleware/index)使用，包括驗證中介軟體 (例如，cookie 中介軟體) 與跨網站偽造要求 (CSRF) 保護。</span><span class="sxs-lookup"><span data-stu-id="77ce7-230">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including authentication middleware (for example, cookie middleware) and cross-site request forgery (CSRF) protections.</span></span> <span data-ttu-id="77ce7-231">即使資料保護 API 並非由使用者程式碼呼叫，仍應設定資料保護，以建立持續密碼編譯[金鑰存放區](xref:security/data-protection/implementation/key-management)。</span><span class="sxs-lookup"><span data-stu-id="77ce7-231">Even if Data Protection APIs aren't called by user code, data protection should be configured to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="77ce7-232">如不設定資料保護，金鑰會保留在記憶體中，並於應用程式重新啟動時捨棄。</span><span class="sxs-lookup"><span data-stu-id="77ce7-232">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="77ce7-233">如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：</span><span class="sxs-lookup"><span data-stu-id="77ce7-233">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="77ce7-234">所有以 Cookie 為基礎的驗證權杖都會失效。</span><span class="sxs-lookup"><span data-stu-id="77ce7-234">All cookie-based authentication tokens are invalidated.</span></span>
* <span data-ttu-id="77ce7-235">當使用者提出下一個要求時，需要再次登入。</span><span class="sxs-lookup"><span data-stu-id="77ce7-235">Users are required to sign in again on their next request.</span></span>
* <span data-ttu-id="77ce7-236">所有以 Keyring 保護的資料都無法再解密。</span><span class="sxs-lookup"><span data-stu-id="77ce7-236">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="77ce7-237">這可能會包含 [CSRF 權杖](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)和 [ASP.NET Core MVC TempData cookie](xref:fundamentals/app-state#tempdata)。</span><span class="sxs-lookup"><span data-stu-id="77ce7-237">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="77ce7-238">若要設定資料保護來保存及加密金鑰環，請參閱：</span><span class="sxs-lookup"><span data-stu-id="77ce7-238">To configure data protection to persist and encrypt the key ring, see:</span></span>

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="long-request-header-fields"></a><span data-ttu-id="77ce7-239">要求標頭欄位太長</span><span class="sxs-lookup"><span data-stu-id="77ce7-239">Long request header fields</span></span>

<span data-ttu-id="77ce7-240">Proxy 伺服器預設設定通常會將要求標頭欄位限制為 4 K 或 8 K （視平臺而定）。</span><span class="sxs-lookup"><span data-stu-id="77ce7-240">Proxy server default settings typically limit request header fields to 4 K or 8 K depending on the platform.</span></span> <span data-ttu-id="77ce7-241">應用程式可能需要比預設值更長的欄位（例如，使用[Azure Active Directory](https://azure.microsoft.com/services/active-directory/)的應用程式）。</span><span class="sxs-lookup"><span data-stu-id="77ce7-241">An app may require fields longer than the default (for example, apps that use [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)).</span></span> <span data-ttu-id="77ce7-242">如果需要較長的欄位，proxy 伺服器的預設設定需要調整。</span><span class="sxs-lookup"><span data-stu-id="77ce7-242">If longer fields are required, the proxy server's default settings require adjustment.</span></span> <span data-ttu-id="77ce7-243">要套用的值取決於案例。</span><span class="sxs-lookup"><span data-stu-id="77ce7-243">The values to apply depend on the scenario.</span></span> <span data-ttu-id="77ce7-244">如需詳細資訊，請參閱您的伺服器文件。</span><span class="sxs-lookup"><span data-stu-id="77ce7-244">For more information, see your server's documentation.</span></span>

* [<span data-ttu-id="77ce7-245">proxy_buffer_size</span><span class="sxs-lookup"><span data-stu-id="77ce7-245">proxy_buffer_size</span></span>](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_buffer_size)
* [<span data-ttu-id="77ce7-246">proxy_buffers</span><span class="sxs-lookup"><span data-stu-id="77ce7-246">proxy_buffers</span></span>](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_buffers)
* [<span data-ttu-id="77ce7-247">proxy_busy_buffers_size</span><span class="sxs-lookup"><span data-stu-id="77ce7-247">proxy_busy_buffers_size</span></span>](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_busy_buffers_size)
* [<span data-ttu-id="77ce7-248">large_client_header_buffers</span><span class="sxs-lookup"><span data-stu-id="77ce7-248">large_client_header_buffers</span></span>](https://nginx.org/docs/http/ngx_http_core_module.html#large_client_header_buffers)

> [!WARNING]
> <span data-ttu-id="77ce7-249">除非必要，否則請勿增加 Proxy 緩衝區的預設值。</span><span class="sxs-lookup"><span data-stu-id="77ce7-249">Don't increase the default values of proxy buffers unless necessary.</span></span> <span data-ttu-id="77ce7-250">增加這些值會提高緩衝區溢位及惡意使用者發動拒絕服務 (DoS) 攻擊的風險。</span><span class="sxs-lookup"><span data-stu-id="77ce7-250">Increasing these values increases the risk of buffer overrun (overflow) and Denial of Service (DoS) attacks by malicious users.</span></span>

## <a name="secure-the-app"></a><span data-ttu-id="77ce7-251">保護應用程式</span><span class="sxs-lookup"><span data-stu-id="77ce7-251">Secure the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="77ce7-252">啟用 AppArmor</span><span class="sxs-lookup"><span data-stu-id="77ce7-252">Enable AppArmor</span></span>

<span data-ttu-id="77ce7-253">Linux 安全性模組 (LSM) 是 Linux 2.6 之後 Linux 核心所包含的一個架構。</span><span class="sxs-lookup"><span data-stu-id="77ce7-253">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="77ce7-254">LSM 支援不同的安全性模組實作。</span><span class="sxs-lookup"><span data-stu-id="77ce7-254">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="77ce7-255">[AppArmor](https://wiki.ubuntu.com/AppArmor) 是 LSM，它實作的必要存取控制系統，可將程式限定在有限的資源集中。</span><span class="sxs-lookup"><span data-stu-id="77ce7-255">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="77ce7-256">確定已啟用並正確設定 AppArmor。</span><span class="sxs-lookup"><span data-stu-id="77ce7-256">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configure-the-firewall"></a><span data-ttu-id="77ce7-257">設定防火牆</span><span class="sxs-lookup"><span data-stu-id="77ce7-257">Configure the firewall</span></span>

<span data-ttu-id="77ce7-258">關閉所有不在使用中的外部連接埠。</span><span class="sxs-lookup"><span data-stu-id="77ce7-258">Close off all external ports that are not in use.</span></span> <span data-ttu-id="77ce7-259">簡單的防火牆（ufw）提供用來設定防火牆的 CLI，藉以提供的前端 `iptables` 。</span><span class="sxs-lookup"><span data-stu-id="77ce7-259">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a CLI for configuring the firewall.</span></span>

> [!WARNING]
> <span data-ttu-id="77ce7-260">如未正確設定，防火牆會禁止存取整個系統。</span><span class="sxs-lookup"><span data-stu-id="77ce7-260">A firewall will prevent access to the whole system if not configured correctly.</span></span> <span data-ttu-id="77ce7-261">未指定正確的 SSH 連接埠，將會導致您無法存取系統 (若您使用 SSH 連線至該連接埠)。</span><span class="sxs-lookup"><span data-stu-id="77ce7-261">Failure to specify the correct SSH port will effectively lock you out of the system if you are using SSH to connect to it.</span></span> <span data-ttu-id="77ce7-262">預設連接埠為 22。</span><span class="sxs-lookup"><span data-stu-id="77ce7-262">The default port is 22.</span></span> <span data-ttu-id="77ce7-263">如需詳細資訊，請參閱 [ufw 簡介](https://help.ubuntu.com/community/UFW)與[手冊](https://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html)。</span><span class="sxs-lookup"><span data-stu-id="77ce7-263">For more information, see the [introduction to ufw](https://help.ubuntu.com/community/UFW) and the [manual](https://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).</span></span>

<span data-ttu-id="77ce7-264">安裝 `ufw` 並將其設定為允許任何所需連接埠上的流量。</span><span class="sxs-lookup"><span data-stu-id="77ce7-264">Install `ufw` and configure it to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw

sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

sudo ufw enable
```

### <a name="secure-nginx"></a><span data-ttu-id="77ce7-265">保護 Nginx</span><span class="sxs-lookup"><span data-stu-id="77ce7-265">Secure Nginx</span></span>

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="77ce7-266">變更 Nginx 回應名稱</span><span class="sxs-lookup"><span data-stu-id="77ce7-266">Change the Nginx response name</span></span>

<span data-ttu-id="77ce7-267">編輯 *src/http/ngx_http_header_filter_module.c*：</span><span class="sxs-lookup"><span data-stu-id="77ce7-267">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a><span data-ttu-id="77ce7-268">設定選項</span><span class="sxs-lookup"><span data-stu-id="77ce7-268">Configure options</span></span>

<span data-ttu-id="77ce7-269">設定伺服器的其他所需模組。</span><span class="sxs-lookup"><span data-stu-id="77ce7-269">Configure the server with additional required modules.</span></span> <span data-ttu-id="77ce7-270">請考慮使用 [ModSecurity](https://www.modsecurity.org/) 等 Web 應用程式防火牆來強化應用程式。</span><span class="sxs-lookup"><span data-stu-id="77ce7-270">Consider using a web app firewall, such as [ModSecurity](https://www.modsecurity.org/), to harden the app.</span></span>

#### <a name="https-configuration"></a><span data-ttu-id="77ce7-271">HTTPS 設定</span><span class="sxs-lookup"><span data-stu-id="77ce7-271">HTTPS configuration</span></span>

<span data-ttu-id="77ce7-272">**設定應用程式以進行安全的本機連線 (HTTPS)**</span><span class="sxs-lookup"><span data-stu-id="77ce7-272">**Configure the app for secure (HTTPS) local connections**</span></span>

<span data-ttu-id="77ce7-273">[dotnet run](/dotnet/core/tools/dotnet-run) 命令使用應用程式的 *Properties/launchSettings.json* 檔案，其設定應用程式在 `applicationUrl` 屬性所提供的 URL 上接聽 (例如 `https://localhost:5001;http://localhost:5000`)。</span><span class="sxs-lookup"><span data-stu-id="77ce7-273">The [dotnet run](/dotnet/core/tools/dotnet-run) command uses the app's *Properties/launchSettings.json* file, which configures the app to listen on the URLs provided by the `applicationUrl` property (for example, `https://localhost:5001;http://localhost:5000`).</span></span>

<span data-ttu-id="77ce7-274">使用下列其中一種方法，設定應用程式將憑證用在針對 `dotnet run` 命令的開發，或用在開發環境 (F5，若在 Visual Studio Code 中則為 Ctrl+F5)：</span><span class="sxs-lookup"><span data-stu-id="77ce7-274">Configure the app to use a certificate in development for the `dotnet run` command or development environment (F5 or Ctrl+F5 in Visual Studio Code) using one of the following approaches:</span></span>

* <span data-ttu-id="77ce7-275">[取代組態中的預設憑證](xref:fundamentals/servers/kestrel#configuration) (建議使用\*\*)</span><span class="sxs-lookup"><span data-stu-id="77ce7-275">[Replace the default certificate from configuration](xref:fundamentals/servers/kestrel#configuration) (*Recommended*)</span></span>
* [<span data-ttu-id="77ce7-276">KestrelServerOptions.ConfigureHttpsDefaults</span><span class="sxs-lookup"><span data-stu-id="77ce7-276">KestrelServerOptions.ConfigureHttpsDefaults</span></span>](xref:fundamentals/servers/kestrel#configurehttpsdefaultsactionhttpsconnectionadapteroptions)

<span data-ttu-id="77ce7-277">**設定反向 Prooxy 以進行安全的用戶端連線 (HTTPS)**</span><span class="sxs-lookup"><span data-stu-id="77ce7-277">**Configure the reverse proxy for secure (HTTPS) client connections**</span></span>

* <span data-ttu-id="77ce7-278">藉由指定由受信任憑證授權單位 (CA) 核發的有效憑證，將伺服器設定成在連接埠 `443` 上接聽 HTTPS 流量。</span><span class="sxs-lookup"><span data-stu-id="77ce7-278">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="77ce7-279">採用以下 */etc/nginx/nginx.conf* 檔案所述的一些做法來強化安全性。</span><span class="sxs-lookup"><span data-stu-id="77ce7-279">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="77ce7-280">範例包括選擇更強的加密，重新導向 HTTPS 到 HTTP 的所有流量。</span><span class="sxs-lookup"><span data-stu-id="77ce7-280">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="77ce7-281">新增 `HTTP Strict-Transport-Security` (HSTS) 標頭可確保用戶端提出的所有後續要求都會透過 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="77ce7-281">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS.</span></span>

* <span data-ttu-id="77ce7-282">如果未來將會停用 HTTPS，請使用下列其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="77ce7-282">If HTTPS will be disabled in the future, use one of the following approaches:</span></span>

  * <span data-ttu-id="77ce7-283">不要新增 HSTS 標頭。</span><span class="sxs-lookup"><span data-stu-id="77ce7-283">Don't add the HSTS header.</span></span>
  * <span data-ttu-id="77ce7-284">選擇 short `max-age` 值。</span><span class="sxs-lookup"><span data-stu-id="77ce7-284">Choose a short `max-age` value.</span></span>

<span data-ttu-id="77ce7-285">新增 */etc/nginx/Proxy.conf* 組態檔：</span><span class="sxs-lookup"><span data-stu-id="77ce7-285">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="77ce7-286">編輯 */etc/nginx/Proxy.conf* 組態檔。</span><span class="sxs-lookup"><span data-stu-id="77ce7-286">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="77ce7-287">此範例在一個組態檔中同時包含 `http` 和 `server` 區段。</span><span class="sxs-lookup"><span data-stu-id="77ce7-287">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="77ce7-288">保護 Nginx 免於點閱綁架</span><span class="sxs-lookup"><span data-stu-id="77ce7-288">Secure Nginx from clickjacking</span></span>

<span data-ttu-id="77ce7-289">[點閱綁架](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger)(也稱為「UI 偽裝攻擊」\*\*) 是一種惡意攻擊，會誘騙網站訪客點選與其目前所瀏覽頁面不同的頁面上連結或按鈕。</span><span class="sxs-lookup"><span data-stu-id="77ce7-289">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="77ce7-290">請使用 `X-FRAME-OPTIONS` 來保護網站安全。</span><span class="sxs-lookup"><span data-stu-id="77ce7-290">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="77ce7-291">減輕點擊劫持攻擊：</span><span class="sxs-lookup"><span data-stu-id="77ce7-291">To mitigate clickjacking attacks:</span></span>

1. <span data-ttu-id="77ce7-292">編輯 *nginx.conf* 檔案：</span><span class="sxs-lookup"><span data-stu-id="77ce7-292">Edit the *nginx.conf* file:</span></span>

   ```bash
   sudo nano /etc/nginx/nginx.conf
   ```

   <span data-ttu-id="77ce7-293">新增 `add_header X-Frame-Options "SAMEORIGIN";` 行。</span><span class="sxs-lookup"><span data-stu-id="77ce7-293">Add the line `add_header X-Frame-Options "SAMEORIGIN";`.</span></span>
1. <span data-ttu-id="77ce7-294">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="77ce7-294">Save the file.</span></span>
1. <span data-ttu-id="77ce7-295">重新啟動 Nginx。</span><span class="sxs-lookup"><span data-stu-id="77ce7-295">Restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="77ce7-296">MIME 類型探查</span><span class="sxs-lookup"><span data-stu-id="77ce7-296">MIME-type sniffing</span></span>

<span data-ttu-id="77ce7-297">此標頭會防止大部分的瀏覽器對遠離宣告內容類型的回應進行 MIME 探查，因為標頭會指示瀏覽器不要覆寫回應內容類型。</span><span class="sxs-lookup"><span data-stu-id="77ce7-297">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="77ce7-298">使用 `nosniff` 選項，如果伺服器顯示的內容是 "text/html"，則瀏覽器就會將它轉譯為 "text/html"。</span><span class="sxs-lookup"><span data-stu-id="77ce7-298">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="77ce7-299">編輯 *nginx.conf* 檔案：</span><span class="sxs-lookup"><span data-stu-id="77ce7-299">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="77ce7-300">新增 `add_header X-Content-Type-Options "nosniff";` 行並儲存檔案，然後重新啟動 Nginx。</span><span class="sxs-lookup"><span data-stu-id="77ce7-300">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>

## <a name="additional-nginx-suggestions"></a><span data-ttu-id="77ce7-301">其他 Nginx 建議</span><span class="sxs-lookup"><span data-stu-id="77ce7-301">Additional Nginx suggestions</span></span>

<span data-ttu-id="77ce7-302">升級伺服器上的共用架構之後，請重新開機伺服器所主控的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="77ce7-302">After upgrading the shared framework on the server, restart the ASP.NET Core apps hosted by the server.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="77ce7-303">其他資源</span><span class="sxs-lookup"><span data-stu-id="77ce7-303">Additional resources</span></span>

* [<span data-ttu-id="77ce7-304">Linux 上 .NET Core 的必要條件</span><span class="sxs-lookup"><span data-stu-id="77ce7-304">Prerequisites for .NET Core on Linux</span></span>](/dotnet/core/linux-prerequisites)
* <span data-ttu-id="77ce7-305">[Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages) (Nginx：二進位版本：正式的 Debian Ubuntu 套件)</span><span class="sxs-lookup"><span data-stu-id="77ce7-305">[Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)</span></span>
* <xref:test/troubleshoot>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="77ce7-306">NGINX：使用轉送的標頭</span><span class="sxs-lookup"><span data-stu-id="77ce7-306">NGINX: Using the Forwarded header</span></span>](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
