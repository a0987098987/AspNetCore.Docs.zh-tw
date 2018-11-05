---
title: 在 Linux 上使用 Nginx 裝載 ASP.NET Core
author: rick-anderson
description: 了解如何在 Ubuntu 16.04 上將 Nginx 設定為反向 Proxy，以將 HTTP 流量轉送至在 Kestrel 上執行的 ASP.NET Core Web 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 10/23/2018
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: d29a9287cbce27a54e779fadfa05e57febec0413
ms.sourcegitcommit: 4a6bbe84db24c2f3dd2de065de418fde952c8d40
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "50253113"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="9d3d9-103">在 Linux 上使用 Nginx 裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9d3d9-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="9d3d9-104">作者：[Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="9d3d9-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="9d3d9-105">本指南說明在 Ubuntu 16.04 伺服器上設定生產環境就緒的 ASP.NET Core 環境。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="9d3d9-106">這些指示可能使用較新版本的 Ubuntu，但未經測試。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-106">These instructions likely work with newer versions of Ubuntu, but the instructions haven't been tested with newer versions.</span></span>

<span data-ttu-id="9d3d9-107">如需有關 ASP.NET Core 支援之其他 Linux 發行版本的資訊，請參閱 [Linux 上 .NET Core 的必要條件](/dotnet/core/linux-prerequisites)。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-107">For information on other Linux distributions supported by ASP.NET Core, see [Prerequisites for .NET Core on Linux](/dotnet/core/linux-prerequisites).</span></span>

> [!NOTE]
> <span data-ttu-id="9d3d9-108">針對 Ubuntu 14.04，建議使用 *supervisord*作為監視 Kestrel 處理序的解決方案。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-108">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="9d3d9-109">在 Ubuntu 14.04 上無法使用 *systemd*。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-109">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="9d3d9-110">如需 Ubuntu 14.04 指示，請參閱[本主題前一版本](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-110">For Ubuntu 14.04 instructions, see the [previous version of this topic](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="9d3d9-111">本指南：</span><span class="sxs-lookup"><span data-stu-id="9d3d9-111">This guide:</span></span>

* <span data-ttu-id="9d3d9-112">將現有的 ASP.NET Core 應用程式放在反向 Proxy 伺服器後方。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-112">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="9d3d9-113">設定反向 Proxy 伺服器以將要求轉送至 Kestrel 網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-113">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="9d3d9-114">確保 Web 應用程式在啟動時以精靈的形式執行。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-114">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="9d3d9-115">設定程序管理工具以協助重新啟動 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-115">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9d3d9-116">必要條件</span><span class="sxs-lookup"><span data-stu-id="9d3d9-116">Prerequisites</span></span>

1. <span data-ttu-id="9d3d9-117">以 sudo 權限使用標準使用者帳戶存取 Ubuntu 16.04 伺服器。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-117">Access to an Ubuntu 16.04 server with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="9d3d9-118">在伺服器上安裝 .NET Core 執行階段。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-118">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="9d3d9-119">請前往 [.NET Core 的 All Downloads (下載區)](https://www.microsoft.com/net/download/all) 頁面。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-119">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="9d3d9-120">在 [執行階段] 下的清單中選取最新的非預覽執行階段。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-120">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="9d3d9-121">選取並遵循符合伺服器 Ubuntu 版本的 Ubuntu 指示。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-121">Select and follow the instructions for Ubuntu that match the Ubuntu version of the server.</span></span>
1. <span data-ttu-id="9d3d9-122">現有的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-122">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="9d3d9-123">跨應用程式發佈與複製</span><span class="sxs-lookup"><span data-stu-id="9d3d9-123">Publish and copy over the app</span></span>

<span data-ttu-id="9d3d9-124">為[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-124">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="9d3d9-125">從開發環境執行 [dotnet publish](/dotnet/core/tools/dotnet-publish) 將應用程式封裝到可在伺服器上執行的目錄 (例如，*bin/Release/&lt;target_framework_moniker&gt;/publish*)：</span><span class="sxs-lookup"><span data-stu-id="9d3d9-125">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="9d3d9-126">如果您不想在伺服器上維護 .NET Core 執行階段，應用程式也可以發佈為[獨立式部署](/dotnet/core/deploying/#self-contained-deployments-scd)。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-126">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="9d3d9-127">使用整合至組織工作流程的工具 (SCP、SFTP 等等) 將 ASP.NET Core 應用程式複製到伺服器。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-127">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="9d3d9-128">Web 應用程式通常可在 *var* 目錄下找到 (例如 *var/www/helloapp*)。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-128">It's common to locate web apps under the *var* directory (for example, *var/www/helloapp*).</span></span>

> [!NOTE]
> <span data-ttu-id="9d3d9-129">在生產環境部署案例中，持續整合工作流程會執行發佈應用程式並將資產複製到伺服器的工作。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-129">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

<span data-ttu-id="9d3d9-130">測試應用程式：</span><span class="sxs-lookup"><span data-stu-id="9d3d9-130">Test the app:</span></span>

1. <span data-ttu-id="9d3d9-131">從命令列執行應用程式：`dotnet <app_assembly>.dll`。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-131">From the command line, run the app: `dotnet <app_assembly>.dll`.</span></span>
1. <span data-ttu-id="9d3d9-132">在瀏覽器中，巡覽至 `http://<serveraddress>:<port>` 以確認應用程式可在 Linux 本機上正常運作。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-132">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux locally.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="9d3d9-133">設定反向 Proxy 伺服器</span><span class="sxs-lookup"><span data-stu-id="9d3d9-133">Configure a reverse proxy server</span></span>

<span data-ttu-id="9d3d9-134">反向 Proxy 是為動態 Web 應用程式提供服務的常見設定。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-134">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="9d3d9-135">反向 Proxy 會終止 HTTP 要求，並將它轉送至 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-135">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="use-a-reverse-proxy-server"></a><span data-ttu-id="9d3d9-136">使用反向 Proxy 伺服器</span><span class="sxs-lookup"><span data-stu-id="9d3d9-136">Use a reverse proxy server</span></span>

<span data-ttu-id="9d3d9-137">Kestrel 非常適用於從 ASP.NET Core 提供動態內容。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-137">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="9d3d9-138">不過，Web 服務功能不像 IIS、Apache 或 Nginx 這類伺服器那樣豐富。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-138">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="9d3d9-139">反向 Proxy 伺服器可以讓 HTTP 伺服器卸下提供靜態內容、快取要求、壓縮要求及終止 SSL 等工作的負擔。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-139">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="9d3d9-140">反向 Proxy 伺服器可能位在專用電腦上，或可能與 HTTP 伺服器一起部署。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-140">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="9d3d9-141">為達到本指南的目的，使用 Nginx 的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-141">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="9d3d9-142">它會在相同的伺服器上和 HTTP 伺服器一起執行。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-142">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="9d3d9-143">您可以根據需求，選擇不同的設定。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-143">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="9d3d9-144">由於反向 Proxy 會轉送要求，因此請使用來自 [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) 套件的[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-144">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="9d3d9-145">此中介軟體會使用 `X-Forwarded-Proto` 標頭來更新 `Request.Scheme`，以便讓重新導向 URI 及其他安全性原則正確運作。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-145">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="9d3d9-146">任何依賴配置的元件，例如驗證、連結產生、重新導向和地理位置，都必須在叫用轉送的標頭中介軟體後放置。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-146">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="9d3d9-147">轉送的標頭中介軟體是一般規則，應該先於診斷和錯誤處理中介軟體以外的其他中介軟體執行。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-147">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="9d3d9-148">這種排序可確保依賴轉送標頭資訊的中介軟體可以耗用用於處理的標頭值。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-148">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9d3d9-149">請先在 `Startup.Configure` 中叫用 [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) 方法，再呼叫 [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) 或類似的驗證配置中介軟體。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-149">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="9d3d9-150">請設定中介軟體來轉送 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭：</span><span class="sxs-lookup"><span data-stu-id="9d3d9-150">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="9d3d9-151">請先在 `Startup.Configure` 中叫用 [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) 方法，再呼叫 [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) 和 [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) 或類似的驗證配置中介軟體。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-151">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="9d3d9-152">請設定中介軟體來轉送 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭：</span><span class="sxs-lookup"><span data-stu-id="9d3d9-152">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseIdentity();
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

::: moniker-end

<span data-ttu-id="9d3d9-153">如果未將任何 [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) 指定給中介軟體，則要轉送的預設標頭會是 `None`。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-153">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="9d3d9-154">預設只會信任在 localhost (127.0.0.1, [::1]) 上執行的 Proxy。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-154">Only proxies running on localhost (127.0.0.1, [::1]) are trusted by default.</span></span> <span data-ttu-id="9d3d9-155">如果組織內有其他受信任的 Proxy 或網路處理網際網路與網頁伺服器之間的要求，請使用 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>，將其新增至 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> 或 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> 清單。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-155">If other trusted proxies or networks within the organization handle requests between the Internet and the web server, add them to the list of <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> or <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span></span> <span data-ttu-id="9d3d9-156">下列範例會將 IP 位址 10.0.0.100 的受信任 Proxy 伺服器新增至 `Startup.ConfigureServices` 中「轉送的標頭中介軟體」的 `KnownProxies`：</span><span class="sxs-lookup"><span data-stu-id="9d3d9-156">The following example adds a trusted proxy server at IP address 10.0.0.100 to the Forwarded Headers Middleware `KnownProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

<span data-ttu-id="9d3d9-157">如需詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-157">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

### <a name="install-nginx"></a><span data-ttu-id="9d3d9-158">安裝 Nginx</span><span class="sxs-lookup"><span data-stu-id="9d3d9-158">Install Nginx</span></span>

<span data-ttu-id="9d3d9-159">使用 `apt-get` 來安裝 Nginx。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-159">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="9d3d9-160">安裝程式建立的 *systemd* init 指令碼，會在系統啟動時將 Nginx 執行為精靈。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-160">The installer creates a *systemd* init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="9d3d9-161">請遵循 [Nginx：Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages) (Nginx：官方 Debian/Ubuntu 套件) 中適用於 Ubuntu 的安裝指示。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-161">Follow the installation instructions for Ubuntu at [Nginx: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span></span>

> [!NOTE]
> <span data-ttu-id="9d3d9-162">如果需要選用的 Nginx 模組，可能要從來源建置 Nginx。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-162">If optional Nginx modules are required, building Nginx from source might be required.</span></span>

<span data-ttu-id="9d3d9-163">因為 Nginx 是第一次安裝，請透過執行明確啟動它：</span><span class="sxs-lookup"><span data-stu-id="9d3d9-163">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="9d3d9-164">確認瀏覽器會顯示 Nginx 的預設登陸頁面。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-164">Verify a browser displays the default landing page for Nginx.</span></span> <span data-ttu-id="9d3d9-165">登陸頁面位於 `http://<server_IP_address>/index.nginx-debian.html`。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-165">The landing page is reachable at `http://<server_IP_address>/index.nginx-debian.html`.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="9d3d9-166">設定 Nginx</span><span class="sxs-lookup"><span data-stu-id="9d3d9-166">Configure Nginx</span></span>

<span data-ttu-id="9d3d9-167">若要將 Nginx 設定為反向 Proxy 以將要求轉送至您的 ASP.NET Core 應用程式，請修改 */etc/nginx/sites-available/default*。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-167">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="9d3d9-168">以文字編輯器開啟它，並以下列項目取代內容：</span><span class="sxs-lookup"><span data-stu-id="9d3d9-168">Open it in a text editor, and replace the contents with the following:</span></span>

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

<span data-ttu-id="9d3d9-169">當沒有任何與 `server_name` 相符的項目時，Nginx 會使用預設伺服器。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-169">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="9d3d9-170">如果未定義任何預設伺服器，則設定檔中的第一個伺服器就是預設伺服器。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-170">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="9d3d9-171">最佳做法是，在您的設定檔中新增一個會傳回狀態碼 444 的特定預設伺服器。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-171">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="9d3d9-172">預設伺服器設定範例如下：</span><span class="sxs-lookup"><span data-stu-id="9d3d9-172">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="9d3d9-173">使用上述設定檔和預設伺服器時，Nginx 會在連接埠 80 接受主機標頭為 `example.com` 或 `*.example.com` 的公用流量。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-173">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="9d3d9-174">不符合這些主機的要求將不會轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-174">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="9d3d9-175">Nginx 會將相符的要求轉送至位於 `http://localhost:5000` 的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-175">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="9d3d9-176">如需詳細資訊，請參閱 [nginx 如何處理要求](https://nginx.org/docs/http/request_processing.html) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-176">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span> <span data-ttu-id="9d3d9-177">若要變更 Kestrel 的 IP/連接埠，請參閱 [Kestrel：端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration)。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-177">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="9d3d9-178">如果無法指定適當的 [server_name 指示詞](https://nginx.org/docs/http/server_names.html)，就會讓應用程式暴露在安全性弱點的風險下。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-178">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="9d3d9-179">若您擁有整個父網域 (相對於易受攻擊的 `*.com`) 的控制權，子網域萬用字元繫結 (例如 `*.example.com`) 就沒有此安全性風險。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-179">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="9d3d9-180">如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-180">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="9d3d9-181">建立 Nginx 設定之後，請執行 `sudo nginx -t` 來確認設定檔的語法。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-181">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="9d3d9-182">如果設定檔測試成功，請執行 `sudo nginx -s reload` 來強制 Nginx 套用這些變更。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-182">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

<span data-ttu-id="9d3d9-183">直接在伺服器上執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="9d3d9-183">To directly run the app on the server:</span></span>

1. <span data-ttu-id="9d3d9-184">巡覽至應用程式目錄。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-184">Navigate to the app's directory.</span></span>
1. <span data-ttu-id="9d3d9-185">執行應用程式：`dotnet <app_assembly.dll>`，其中 `app_assembly.dll` 是應用程式的組件檔名稱。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-185">Run the app: `dotnet <app_assembly.dll>`, where `app_assembly.dll` is the assembly file name of the app.</span></span>

<span data-ttu-id="9d3d9-186">如果應用程式在伺服器上執行，但無法透過網際網路回應，請檢查伺服器的防火牆，確認連接埠 80 已開啟。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-186">If the app runs on the server but fails to respond over the Internet, check the server's firewall and confirm that port 80 is open.</span></span> <span data-ttu-id="9d3d9-187">如果使用的是 Azure Ubuntu VM，請新增啓用輸入連接埠 80 流量的網路安全性群組 (NSG) 規則。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-187">If using an Azure Ubuntu VM, add a Network Security Group (NSG) rule that enables inbound port 80 traffic.</span></span> <span data-ttu-id="9d3d9-188">沒有必要啟用輸出連接埠 80 規則，因為啓用輸入規則時會自動授與輸出流量。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-188">There's no need to enable an outbound port 80 rule, as the outbound traffic is automatically granted when the inbound rule is enabled.</span></span>

<span data-ttu-id="9d3d9-189">應用程式測試完成後，請在命令提示字元以 `Ctrl+C` 關閉應用程式。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-189">When done testing the app, shut the app down with `Ctrl+C` at the command prompt.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="9d3d9-190">監視應用程式</span><span class="sxs-lookup"><span data-stu-id="9d3d9-190">Monitoring the app</span></span>

<span data-ttu-id="9d3d9-191">伺服器已設定完成，可將對 `http://<serveraddress>:80` 發出的要求轉送給在位於 `http://127.0.0.1:5000` 的 Kestrel 上執行的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-191">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="9d3d9-192">不過，並未設定 Nginx 來管理 Kestrel 處理序。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-192">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="9d3d9-193">您可以使用 *systemd* 來建立服務檔案，以啟動並監視基礎 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-193">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="9d3d9-194">*systemd* 是 init 系統，提供許多強大的啟動、停止和管理處理程序功能。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-194">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="9d3d9-195">建立服務檔</span><span class="sxs-lookup"><span data-stu-id="9d3d9-195">Create the service file</span></span>

<span data-ttu-id="9d3d9-196">建立服務定義檔：</span><span class="sxs-lookup"><span data-stu-id="9d3d9-196">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-helloapp.service
```

<span data-ttu-id="9d3d9-197">以下是一個應用程式範例服務檔：</span><span class="sxs-lookup"><span data-stu-id="9d3d9-197">The following is an example service file for the app:</span></span>

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

<span data-ttu-id="9d3d9-198">如果設定不是使用 *www-data* 這個使用者，就必須先建立這裡所定義的使用者，並授與適當的檔案擁有權。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-198">If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="9d3d9-199">使用 `TimeoutStopSec` 可設定應用程式收到初始中斷訊號之後等待關閉的時間。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-199">Use `TimeoutStopSec` to configure the duration of time to wait for the app to shut down after it receives the initial interrupt signal.</span></span> <span data-ttu-id="9d3d9-200">如果應用程式在此期間後未關閉，則會發出 SIGKILL 來終止應用程式。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-200">If the app doesn't shut down in this period, SIGKILL is issued to terminate the app.</span></span> <span data-ttu-id="9d3d9-201">提供不具單位的秒值 (例如 `150`)、時間範圍值 (例如 `2min 30s`) 或 `infinity` (表示停用逾時)。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-201">Provide the value as unitless seconds (for example, `150`), a time span value (for example, `2min 30s`), or `infinity` to disable the timeout.</span></span> <span data-ttu-id="9d3d9-202">`TimeoutStopSec` 在管理員設定檔 (*systemd-system.conf*、*system.conf.d*、*systemd-user.conf*、*user.conf.d*) 的預設值為 `DefaultTimeoutStopSec`。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-202">`TimeoutStopSec` defaults to the value of `DefaultTimeoutStopSec` in the manager configuration file (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*, *user.conf.d*).</span></span> <span data-ttu-id="9d3d9-203">大多數發行版本的預設逾時為 90 秒。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-203">The default timeout for most distributions is 90 seconds.</span></span>

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

<span data-ttu-id="9d3d9-204">Linux 的檔案系統會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-204">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="9d3d9-205">將 ASPNETCORE_ENVIRONMENT 設定為 "Production" 會導致搜尋 *appsettings.Production.json* 設定檔，而不是搜尋 *appsettings.production.json*。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-205">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

<span data-ttu-id="9d3d9-206">有些值 (例如 SQL 連接字串) 必須以逸出方式處理，設定提供者才能讀取環境變數。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-206">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="9d3d9-207">請使用下列命令來產生要在設定檔中使用並已適當逸出的值：</span><span class="sxs-lookup"><span data-stu-id="9d3d9-207">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>

```console
systemd-escape "<value-to-escape>"
```

<span data-ttu-id="9d3d9-208">儲存檔案並啟用服務。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-208">Save the file and enable the service.</span></span>

```bash
sudo systemctl enable kestrel-helloapp.service
```

<span data-ttu-id="9d3d9-209">啟動服務並確認它正在執行。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-209">Start the service and verify that it's running.</span></span>

```
sudo systemctl start kestrel-helloapp.service
sudo systemctl status kestrel-helloapp.service

● kestrel-helloapp.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-helloapp.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-helloapp.service
            └─9021 /usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
```

<span data-ttu-id="9d3d9-210">設定好反向 Proxy 並透過 systemd 管理 Kestrel 之後，Web 應用程式便已完全設定妥當，而從本機電腦瀏覽器透過 `http://localhost` 即可存取它。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-210">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="9d3d9-211">此外，也可以從遠端電腦存取它，除非遭到任何防火牆封鎖。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-211">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="9d3d9-212">檢查回應標頭時，`Server` 標頭會顯示是由 Kestrel 為 ASP.NET Core 應用程式提供服務。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-212">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="9d3d9-213">檢視記錄</span><span class="sxs-lookup"><span data-stu-id="9d3d9-213">Viewing logs</span></span>

<span data-ttu-id="9d3d9-214">由於是使用 `systemd` 來管理使用 Kestrel 的 Web 應用程式，因此會將所有事件和處理序都記錄在集中式日誌中。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-214">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="9d3d9-215">不過，此日誌包含 `systemd` 管理的所有服務和處理程序的所有項目。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-215">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="9d3d9-216">若要檢視 `kestrel-helloapp.service` 的特定項目，請使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="9d3d9-216">To view the `kestrel-helloapp.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service
```

<span data-ttu-id="9d3d9-217">如需進一步篩選，例如 `--since today`、`--until 1 hour ago` 或這些項目的組合等時間選項，可以減少傳回的項目數量。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-217">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a><span data-ttu-id="9d3d9-218">資料保護</span><span class="sxs-lookup"><span data-stu-id="9d3d9-218">Data protection</span></span>

<span data-ttu-id="9d3d9-219">[ASP.NET Core 資料保護堆疊](xref:security/data-protection/introduction)由數個 ASP.NET Core[ 中介軟體](xref:fundamentals/middleware/index)使用，包括驗證中介軟體 (例如，cookie 中介軟體) 與跨網站偽造要求 (CSRF) 保護。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-219">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including authentication middleware (for example, cookie middleware) and cross-site request forgery (CSRF) protections.</span></span> <span data-ttu-id="9d3d9-220">即使資料保護 API 並非由使用者程式碼呼叫，仍應設定資料保護，以建立持續密碼編譯[金鑰存放區](xref:security/data-protection/implementation/key-management)。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-220">Even if Data Protection APIs aren't called by user code, data protection should be configured to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="9d3d9-221">如不設定資料保護，金鑰會保留在記憶體中，並於應用程式重新啟動時捨棄。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-221">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="9d3d9-222">如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：</span><span class="sxs-lookup"><span data-stu-id="9d3d9-222">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="9d3d9-223">所有以 Cookie 為基礎的驗證權杖都會失效。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-223">All cookie-based authentication tokens are invalidated.</span></span>
* <span data-ttu-id="9d3d9-224">當使用者提出下一個要求時，需要再次登入。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-224">Users are required to sign in again on their next request.</span></span>
* <span data-ttu-id="9d3d9-225">所有以 Keyring 保護的資料都無法再解密。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-225">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="9d3d9-226">這可能會包含 [CSRF 權杖](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)和 [ASP.NET Core MVC TempData cookie](xref:fundamentals/app-state#tempdata)。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-226">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="9d3d9-227">若要設定資料保護來保存及加密金鑰環，請參閱：</span><span class="sxs-lookup"><span data-stu-id="9d3d9-227">To configure data protection to persist and encrypt the key ring, see:</span></span>

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="securing-the-app"></a><span data-ttu-id="9d3d9-228">確保應用程式的安全性</span><span class="sxs-lookup"><span data-stu-id="9d3d9-228">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="9d3d9-229">啟用 AppArmor</span><span class="sxs-lookup"><span data-stu-id="9d3d9-229">Enable AppArmor</span></span>

<span data-ttu-id="9d3d9-230">Linux 安全性模組 (LSM) 是 Linux 2.6 之後 Linux 核心所包含的一個架構。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-230">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="9d3d9-231">LSM 支援不同的安全性模組實作。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-231">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="9d3d9-232">[AppArmor](https://wiki.ubuntu.com/AppArmor) 是 LSM，它實作的必要存取控制系統，可將程式限定在有限的資源集中。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-232">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="9d3d9-233">確定已啟用並正確設定 AppArmor。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-233">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="9d3d9-234">設定防火牆</span><span class="sxs-lookup"><span data-stu-id="9d3d9-234">Configuring the firewall</span></span>

<span data-ttu-id="9d3d9-235">關閉所有不在使用中的外部連接埠。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-235">Close off all external ports that are not in use.</span></span> <span data-ttu-id="9d3d9-236">簡單的防火牆 (ufw) 提供命令列介面供設定防火牆，為 `iptables` 提供前端。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-236">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span>

> [!WARNING]
> <span data-ttu-id="9d3d9-237">如未正確設定，防火牆會禁止存取整個系統。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-237">A firewall will prevent access to the whole system if not configured correctly.</span></span> <span data-ttu-id="9d3d9-238">未指定正確的 SSH 連接埠，將會導致您無法存取系統 (若您使用 SSH 連線至該連接埠)。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-238">Failure to specify the correct SSH port will effectively lock you out of the system if you are using SSH to connect to it.</span></span> <span data-ttu-id="9d3d9-239">預設連接埠為 22。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-239">The default port is 22.</span></span> <span data-ttu-id="9d3d9-240">如需詳細資訊，請參閱 [ufw 簡介](https://help.ubuntu.com/community/UFW)與[手冊](http://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html)。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-240">For more information, see the [introduction to ufw](https://help.ubuntu.com/community/UFW) and the [manual](http://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).</span></span>

<span data-ttu-id="9d3d9-241">安裝 `ufw` 並將其設定為允許任何所需連接埠上的流量。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-241">Install `ufw` and configure it to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw

sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

sudo ufw enable
```

### <a name="securing-nginx"></a><span data-ttu-id="9d3d9-242">保護 Nginx</span><span class="sxs-lookup"><span data-stu-id="9d3d9-242">Securing Nginx</span></span>

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="9d3d9-243">變更 Nginx 回應名稱</span><span class="sxs-lookup"><span data-stu-id="9d3d9-243">Change the Nginx response name</span></span>

<span data-ttu-id="9d3d9-244">編輯 *src/http/ngx_http_header_filter_module.c*：</span><span class="sxs-lookup"><span data-stu-id="9d3d9-244">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a><span data-ttu-id="9d3d9-245">設定選項</span><span class="sxs-lookup"><span data-stu-id="9d3d9-245">Configure options</span></span>

<span data-ttu-id="9d3d9-246">設定伺服器的其他所需模組。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-246">Configure the server with additional required modules.</span></span> <span data-ttu-id="9d3d9-247">請考慮使用 [ModSecurity](https://www.modsecurity.org/) 等 Web 應用程式防火牆來強化應用程式。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-247">Consider using a web app firewall, such as [ModSecurity](https://www.modsecurity.org/), to harden the app.</span></span>

#### <a name="configure-ssl"></a><span data-ttu-id="9d3d9-248">設定 SSL</span><span class="sxs-lookup"><span data-stu-id="9d3d9-248">Configure SSL</span></span>

* <span data-ttu-id="9d3d9-249">藉由指定由受信任憑證授權單位 (CA) 核發的有效憑證，將伺服器設定成在連接埠 `443` 上接聽 HTTPS 流量。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-249">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="9d3d9-250">採用以下 */etc/nginx/nginx.conf* 檔案所述的一些做法來強化安全性。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-250">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="9d3d9-251">範例包括選擇更強的加密，重新導向 HTTPS 到 HTTP 的所有流量。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-251">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="9d3d9-252">新增 `HTTP Strict-Transport-Security` (HSTS) 標頭可確保用戶端提出的所有後續要求都會透過 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-252">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS.</span></span>

* <span data-ttu-id="9d3d9-253">如果未來將會停用 SSL，請不要新增 HSTS 標頭或選擇適當的 `max-age`。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-253">Don't add the HSTS header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="9d3d9-254">新增 */etc/nginx/Proxy.conf* 組態檔：</span><span class="sxs-lookup"><span data-stu-id="9d3d9-254">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="9d3d9-255">編輯 */etc/nginx/Proxy.conf* 組態檔。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-255">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="9d3d9-256">此範例在一個組態檔中同時包含 `http` 和 `server` 區段。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-256">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="9d3d9-257">保護 Nginx 免於點閱綁架</span><span class="sxs-lookup"><span data-stu-id="9d3d9-257">Secure Nginx from clickjacking</span></span>

<span data-ttu-id="9d3d9-258">[點閱綁架](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger)(也稱為「UI 偽裝攻擊」) 是一種惡意攻擊，會誘騙網站訪客點選與其目前所瀏覽頁面不同的頁面上連結或按鈕。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-258">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="9d3d9-259">請使用 `X-FRAME-OPTIONS` 來保護網站安全。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-259">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="9d3d9-260">減輕點擊劫持攻擊：</span><span class="sxs-lookup"><span data-stu-id="9d3d9-260">To mitigate clickjacking attacks:</span></span>

1. <span data-ttu-id="9d3d9-261">編輯 *nginx.conf* 檔案：</span><span class="sxs-lookup"><span data-stu-id="9d3d9-261">Edit the *nginx.conf* file:</span></span>

   ```bash
   sudo nano /etc/nginx/nginx.conf
   ```

   <span data-ttu-id="9d3d9-262">新增 `add_header X-Frame-Options "SAMEORIGIN";` 行。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-262">Add the line `add_header X-Frame-Options "SAMEORIGIN";`.</span></span>
1. <span data-ttu-id="9d3d9-263">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-263">Save the file.</span></span>
1. <span data-ttu-id="9d3d9-264">重新啟動 Nginx。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-264">Restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="9d3d9-265">MIME 類型探查</span><span class="sxs-lookup"><span data-stu-id="9d3d9-265">MIME-type sniffing</span></span>

<span data-ttu-id="9d3d9-266">此標頭會防止大部分的瀏覽器對遠離宣告內容類型的回應進行 MIME 探查，因為標頭會指示瀏覽器不要覆寫回應內容類型。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-266">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="9d3d9-267">使用 `nosniff` 選項，如果伺服器顯示的內容是 "text/html"，則瀏覽器就會將它轉譯為 "text/html"。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-267">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="9d3d9-268">編輯 *nginx.conf* 檔案：</span><span class="sxs-lookup"><span data-stu-id="9d3d9-268">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="9d3d9-269">新增行 `add_header X-Content-Type-Options "nosniff";` 並儲存檔案，然後重新啟動 Nginx。</span><span class="sxs-lookup"><span data-stu-id="9d3d9-269">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9d3d9-270">其他資源</span><span class="sxs-lookup"><span data-stu-id="9d3d9-270">Additional resources</span></span>

* [<span data-ttu-id="9d3d9-271">Linux 上 .NET Core 的必要條件</span><span class="sxs-lookup"><span data-stu-id="9d3d9-271">Prerequisites for .NET Core on Linux</span></span>](/dotnet/core/linux-prerequisites)
* <span data-ttu-id="9d3d9-272">[Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages) (Nginx：二進位版本：正式的 Debian Ubuntu 套件)</span><span class="sxs-lookup"><span data-stu-id="9d3d9-272">[Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)</span></span>
* [<span data-ttu-id="9d3d9-273">設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器</span><span class="sxs-lookup"><span data-stu-id="9d3d9-273">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
* [<span data-ttu-id="9d3d9-274">NGINX：使用轉送的標頭</span><span class="sxs-lookup"><span data-stu-id="9d3d9-274">NGINX: Using the Forwarded header</span></span>](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
