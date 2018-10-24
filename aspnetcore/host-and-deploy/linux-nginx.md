---
title: 在 Linux 上使用 Nginx 裝載 ASP.NET Core
author: rick-anderson
description: 了解如何在 Ubuntu 16.04 上將 Nginx 設定為反向 Proxy，以將 HTTP 流量轉送至在 Kestrel 上執行的 ASP.NET Core Web 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 10/09/2018
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 8d3c158b44c9f30e7c0746398306aa1c0fd9e15b
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912112"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="5ae08-103">在 Linux 上使用 Nginx 裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5ae08-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="5ae08-104">作者：[Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="5ae08-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="5ae08-105">本指南說明在 Ubuntu 16.04 伺服器上設定生產環境就緒的 ASP.NET Core 環境。</span><span class="sxs-lookup"><span data-stu-id="5ae08-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="5ae08-106">這些指示可能使用較新版本的 Ubuntu，但未經測試。</span><span class="sxs-lookup"><span data-stu-id="5ae08-106">These instructions likely work with newer versions of Ubuntu, but the instructions haven't been tested with newer versions.</span></span>

<span data-ttu-id="5ae08-107">如需有關 ASP.NET Core 支援之其他 Linux 發行版本的資訊，請參閱 [Linux 上 .NET Core 的必要條件](/dotnet/core/linux-prerequisites)。</span><span class="sxs-lookup"><span data-stu-id="5ae08-107">For information on other Linux distributions supported by ASP.NET Core, see [Prerequisites for .NET Core on Linux](/dotnet/core/linux-prerequisites).</span></span>

> [!NOTE]
> <span data-ttu-id="5ae08-108">針對 Ubuntu 14.04，建議使用 *supervisord*作為監視 Kestrel 處理序的解決方案。</span><span class="sxs-lookup"><span data-stu-id="5ae08-108">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="5ae08-109">在 Ubuntu 14.04 上無法使用 *systemd*。</span><span class="sxs-lookup"><span data-stu-id="5ae08-109">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="5ae08-110">如需 Ubuntu 14.04 指示，請參閱[本主題前一版本](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)。</span><span class="sxs-lookup"><span data-stu-id="5ae08-110">For Ubuntu 14.04 instructions, see the [previous version of this topic](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="5ae08-111">本指南：</span><span class="sxs-lookup"><span data-stu-id="5ae08-111">This guide:</span></span>

* <span data-ttu-id="5ae08-112">將現有的 ASP.NET Core 應用程式放在反向 Proxy 伺服器後方。</span><span class="sxs-lookup"><span data-stu-id="5ae08-112">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="5ae08-113">設定反向 Proxy 伺服器以將要求轉送至 Kestrel 網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="5ae08-113">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="5ae08-114">確保 Web 應用程式在啟動時以精靈的形式執行。</span><span class="sxs-lookup"><span data-stu-id="5ae08-114">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="5ae08-115">設定程序管理工具以協助重新啟動 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5ae08-115">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5ae08-116">必要條件</span><span class="sxs-lookup"><span data-stu-id="5ae08-116">Prerequisites</span></span>

1. <span data-ttu-id="5ae08-117">以 sudo 權限使用標準使用者帳戶存取 Ubuntu 16.04 伺服器。</span><span class="sxs-lookup"><span data-stu-id="5ae08-117">Access to an Ubuntu 16.04 server with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="5ae08-118">在伺服器上安裝 .NET Core 執行階段。</span><span class="sxs-lookup"><span data-stu-id="5ae08-118">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="5ae08-119">請前往 [.NET Core 的 All Downloads (下載區)](https://www.microsoft.com/net/download/all) 頁面。</span><span class="sxs-lookup"><span data-stu-id="5ae08-119">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="5ae08-120">在 [執行階段] 下的清單中選取最新的非預覽執行階段。</span><span class="sxs-lookup"><span data-stu-id="5ae08-120">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="5ae08-121">選取並遵循符合伺服器 Ubuntu 版本的 Ubuntu 指示。</span><span class="sxs-lookup"><span data-stu-id="5ae08-121">Select and follow the instructions for Ubuntu that match the Ubuntu version of the server.</span></span>
1. <span data-ttu-id="5ae08-122">現有的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5ae08-122">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="5ae08-123">跨應用程式發佈與複製</span><span class="sxs-lookup"><span data-stu-id="5ae08-123">Publish and copy over the app</span></span>

<span data-ttu-id="5ae08-124">為[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="5ae08-124">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="5ae08-125">從開發環境執行 [dotnet publish](/dotnet/core/tools/dotnet-publish) 將應用程式封裝到可在伺服器上執行的目錄 (例如，*bin/Release/&lt;target_framework_moniker&gt;/publish*)：</span><span class="sxs-lookup"><span data-stu-id="5ae08-125">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="5ae08-126">如果您不想在伺服器上維護 .NET Core 執行階段，應用程式也可以發佈為[獨立式部署](/dotnet/core/deploying/#self-contained-deployments-scd)。</span><span class="sxs-lookup"><span data-stu-id="5ae08-126">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="5ae08-127">使用整合至組織工作流程的工具 (SCP、SFTP 等等) 將 ASP.NET Core 應用程式複製到伺服器。</span><span class="sxs-lookup"><span data-stu-id="5ae08-127">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="5ae08-128">Web 應用程式通常可在 *var* 目錄下找到 (例如 *var/www/helloapp*)。</span><span class="sxs-lookup"><span data-stu-id="5ae08-128">It's common to locate web apps under the *var* directory (for example, *var/www/helloapp*).</span></span>

> [!NOTE]
> <span data-ttu-id="5ae08-129">在生產環境部署案例中，持續整合工作流程會執行發佈應用程式並將資產複製到伺服器的工作。</span><span class="sxs-lookup"><span data-stu-id="5ae08-129">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

<span data-ttu-id="5ae08-130">測試應用程式：</span><span class="sxs-lookup"><span data-stu-id="5ae08-130">Test the app:</span></span>

1. <span data-ttu-id="5ae08-131">從命令列執行應用程式：`dotnet <app_assembly>.dll`。</span><span class="sxs-lookup"><span data-stu-id="5ae08-131">From the command line, run the app: `dotnet <app_assembly>.dll`.</span></span>
1. <span data-ttu-id="5ae08-132">在瀏覽器中，巡覽至 `http://<serveraddress>:<port>` 以確認應用程式可在 Linux 本機上正常運作。</span><span class="sxs-lookup"><span data-stu-id="5ae08-132">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux locally.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="5ae08-133">設定反向 Proxy 伺服器</span><span class="sxs-lookup"><span data-stu-id="5ae08-133">Configure a reverse proxy server</span></span>

<span data-ttu-id="5ae08-134">反向 Proxy 是為動態 Web 應用程式提供服務的常見設定。</span><span class="sxs-lookup"><span data-stu-id="5ae08-134">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="5ae08-135">反向 Proxy 會終止 HTTP 要求，並將它轉送至 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5ae08-135">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="5ae08-136">不論設定是否具有反向 Proxy 伺服器，對於 ASP.NET Core 2.0 或更新版本的應用程式，其中之一都是有效且支援的裝載設定。</span><span class="sxs-lookup"><span data-stu-id="5ae08-136">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="5ae08-137">如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)。</span><span class="sxs-lookup"><span data-stu-id="5ae08-137">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

### <a name="use-a-reverse-proxy-server"></a><span data-ttu-id="5ae08-138">使用反向 Proxy 伺服器</span><span class="sxs-lookup"><span data-stu-id="5ae08-138">Use a reverse proxy server</span></span>

<span data-ttu-id="5ae08-139">Kestrel 非常適用於從 ASP.NET Core 提供動態內容。</span><span class="sxs-lookup"><span data-stu-id="5ae08-139">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="5ae08-140">不過，Web 服務功能不像 IIS、Apache 或 Nginx 這類伺服器那樣豐富。</span><span class="sxs-lookup"><span data-stu-id="5ae08-140">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="5ae08-141">反向 Proxy 伺服器可以讓 HTTP 伺服器卸下提供靜態內容、快取要求、壓縮要求及終止 SSL 等工作的負擔。</span><span class="sxs-lookup"><span data-stu-id="5ae08-141">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="5ae08-142">反向 Proxy 伺服器可能位在專用電腦上，或可能與 HTTP 伺服器一起部署。</span><span class="sxs-lookup"><span data-stu-id="5ae08-142">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="5ae08-143">為達到本指南的目的，使用 Nginx 的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="5ae08-143">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="5ae08-144">它會在相同的伺服器上和 HTTP 伺服器一起執行。</span><span class="sxs-lookup"><span data-stu-id="5ae08-144">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="5ae08-145">您可以根據需求，選擇不同的設定。</span><span class="sxs-lookup"><span data-stu-id="5ae08-145">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="5ae08-146">由於反向 Proxy 會轉送要求，因此請使用來自 [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) 套件的[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="5ae08-146">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="5ae08-147">此中介軟體會使用 `X-Forwarded-Proto` 標頭來更新 `Request.Scheme`，以便讓重新導向 URI 及其他安全性原則正確運作。</span><span class="sxs-lookup"><span data-stu-id="5ae08-147">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="5ae08-148">任何依賴配置的元件，例如驗證、連結產生、重新導向和地理位置，都必須在叫用轉送的標頭中介軟體後放置。</span><span class="sxs-lookup"><span data-stu-id="5ae08-148">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="5ae08-149">轉送的標頭中介軟體是一般規則，應該先於診斷和錯誤處理中介軟體以外的其他中介軟體執行。</span><span class="sxs-lookup"><span data-stu-id="5ae08-149">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="5ae08-150">這種排序可確保依賴轉送標頭資訊的中介軟體可以耗用用於處理的標頭值。</span><span class="sxs-lookup"><span data-stu-id="5ae08-150">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5ae08-151">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5ae08-151">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5ae08-152">請先在 `Startup.Configure` 中叫用 [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) 方法，再呼叫 [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) 或類似的驗證配置中介軟體。</span><span class="sxs-lookup"><span data-stu-id="5ae08-152">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="5ae08-153">請設定中介軟體來轉送 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭：</span><span class="sxs-lookup"><span data-stu-id="5ae08-153">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5ae08-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5ae08-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5ae08-155">請先在 `Startup.Configure` 中叫用 [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) 方法，再呼叫 [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) 和 [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) 或類似的驗證配置中介軟體。</span><span class="sxs-lookup"><span data-stu-id="5ae08-155">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="5ae08-156">請設定中介軟體來轉送 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭：</span><span class="sxs-lookup"><span data-stu-id="5ae08-156">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

---

<span data-ttu-id="5ae08-157">如果未將任何 [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) 指定給中介軟體，則要轉送的預設標頭會是 `None`。</span><span class="sxs-lookup"><span data-stu-id="5ae08-157">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="5ae08-158">預設只會信任在 localhost (127.0.0.1, [::1]) 上執行的 Proxy。</span><span class="sxs-lookup"><span data-stu-id="5ae08-158">Only proxies running on localhost (127.0.0.1, [::1]) are trusted by default.</span></span> <span data-ttu-id="5ae08-159">如果組織內有其他受信任的 Proxy 或網路處理網際網路與網頁伺服器之間的要求，請使用 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>，將其新增至 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> 或 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> 清單。</span><span class="sxs-lookup"><span data-stu-id="5ae08-159">If other trusted proxies or networks within the organization handle requests between the Internet and the web server, add them to the list of <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> or <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span></span> <span data-ttu-id="5ae08-160">下列範例會將 IP 位址 10.0.0.100 的受信任 Proxy 伺服器新增至 `Startup.ConfigureServices` 中「轉送的標頭中介軟體」的 `KnownProxies`：</span><span class="sxs-lookup"><span data-stu-id="5ae08-160">The following example adds a trusted proxy server at IP address 10.0.0.100 to the Forwarded Headers Middleware `KnownProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

<span data-ttu-id="5ae08-161">如需詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="5ae08-161">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

### <a name="install-nginx"></a><span data-ttu-id="5ae08-162">安裝 Nginx</span><span class="sxs-lookup"><span data-stu-id="5ae08-162">Install Nginx</span></span>

<span data-ttu-id="5ae08-163">使用 `apt-get` 來安裝 Nginx。</span><span class="sxs-lookup"><span data-stu-id="5ae08-163">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="5ae08-164">安裝程式建立的 *systemd* init 指令碼，會在系統啟動時將 Nginx 執行為精靈。</span><span class="sxs-lookup"><span data-stu-id="5ae08-164">The installer creates a *systemd* init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="5ae08-165">請遵循 [Nginx：Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages) (Nginx：官方 Debian/Ubuntu 套件) 中適用於 Ubuntu 的安裝指示。</span><span class="sxs-lookup"><span data-stu-id="5ae08-165">Follow the installation instructions for Ubuntu at [Nginx: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span></span>

> [!NOTE]
> <span data-ttu-id="5ae08-166">如果需要選用的 Nginx 模組，可能要從來源建置 Nginx。</span><span class="sxs-lookup"><span data-stu-id="5ae08-166">If optional Nginx modules are required, building Nginx from source might be required.</span></span>

<span data-ttu-id="5ae08-167">因為 Nginx 是第一次安裝，請透過執行明確啟動它：</span><span class="sxs-lookup"><span data-stu-id="5ae08-167">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="5ae08-168">確認瀏覽器會顯示 Nginx 的預設登陸頁面。</span><span class="sxs-lookup"><span data-stu-id="5ae08-168">Verify a browser displays the default landing page for Nginx.</span></span> <span data-ttu-id="5ae08-169">登陸頁面位於 `http://<server_IP_address>/index.nginx-debian.html`。</span><span class="sxs-lookup"><span data-stu-id="5ae08-169">The landing page is reachable at `http://<server_IP_address>/index.nginx-debian.html`.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="5ae08-170">設定 Nginx</span><span class="sxs-lookup"><span data-stu-id="5ae08-170">Configure Nginx</span></span>

<span data-ttu-id="5ae08-171">若要將 Nginx 設定為反向 Proxy 以將要求轉送至您的 ASP.NET Core 應用程式，請修改 */etc/nginx/sites-available/default*。</span><span class="sxs-lookup"><span data-stu-id="5ae08-171">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="5ae08-172">以文字編輯器開啟它，並以下列項目取代內容：</span><span class="sxs-lookup"><span data-stu-id="5ae08-172">Open it in a text editor, and replace the contents with the following:</span></span>

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

<span data-ttu-id="5ae08-173">當沒有任何與 `server_name` 相符的項目時，Nginx 會使用預設伺服器。</span><span class="sxs-lookup"><span data-stu-id="5ae08-173">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="5ae08-174">如果未定義任何預設伺服器，則設定檔中的第一個伺服器就是預設伺服器。</span><span class="sxs-lookup"><span data-stu-id="5ae08-174">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="5ae08-175">最佳做法是，在您的設定檔中新增一個會傳回狀態碼 444 的特定預設伺服器。</span><span class="sxs-lookup"><span data-stu-id="5ae08-175">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="5ae08-176">預設伺服器設定範例如下：</span><span class="sxs-lookup"><span data-stu-id="5ae08-176">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="5ae08-177">使用上述設定檔和預設伺服器時，Nginx 會在連接埠 80 接受主機標頭為 `example.com` 或 `*.example.com` 的公用流量。</span><span class="sxs-lookup"><span data-stu-id="5ae08-177">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="5ae08-178">不符合這些主機的要求將不會轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="5ae08-178">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="5ae08-179">Nginx 會將相符的要求轉送至位於 `http://localhost:5000` 的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="5ae08-179">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="5ae08-180">如需詳細資訊，請參閱 [nginx 如何處理要求](https://nginx.org/docs/http/request_processing.html) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="5ae08-180">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span> <span data-ttu-id="5ae08-181">若要變更 Kestrel 的 IP/連接埠，請參閱 [Kestrel：端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration)。</span><span class="sxs-lookup"><span data-stu-id="5ae08-181">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="5ae08-182">如果無法指定適當的 [server_name 指示詞](https://nginx.org/docs/http/server_names.html)，就會讓應用程式暴露在安全性弱點的風險下。</span><span class="sxs-lookup"><span data-stu-id="5ae08-182">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="5ae08-183">若您擁有整個父網域 (相對於易受攻擊的 `*.com`) 的控制權，子網域萬用字元繫結 (例如 `*.example.com`) 就沒有此安全性風險。</span><span class="sxs-lookup"><span data-stu-id="5ae08-183">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="5ae08-184">如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="5ae08-184">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="5ae08-185">建立 Nginx 設定之後，請執行 `sudo nginx -t` 來確認設定檔的語法。</span><span class="sxs-lookup"><span data-stu-id="5ae08-185">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="5ae08-186">如果設定檔測試成功，請執行 `sudo nginx -s reload` 來強制 Nginx 套用這些變更。</span><span class="sxs-lookup"><span data-stu-id="5ae08-186">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

<span data-ttu-id="5ae08-187">直接在伺服器上執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="5ae08-187">To directly run the app on the server:</span></span>

1. <span data-ttu-id="5ae08-188">巡覽至應用程式目錄。</span><span class="sxs-lookup"><span data-stu-id="5ae08-188">Navigate to the app's directory.</span></span>
1. <span data-ttu-id="5ae08-189">執行應用程式：`dotnet <app_assembly.dll>`，其中 `app_assembly.dll` 是應用程式的組件檔名稱。</span><span class="sxs-lookup"><span data-stu-id="5ae08-189">Run the app: `dotnet <app_assembly.dll>`, where `app_assembly.dll` is the assembly file name of the app.</span></span>

<span data-ttu-id="5ae08-190">如果應用程式在伺服器上執行，但無法透過網際網路回應，請檢查伺服器的防火牆，確認連接埠 80 已開啟。</span><span class="sxs-lookup"><span data-stu-id="5ae08-190">If the app runs on the server but fails to respond over the Internet, check the server's firewall and confirm that port 80 is open.</span></span> <span data-ttu-id="5ae08-191">如果使用的是 Azure Ubuntu VM，請新增啓用輸入連接埠 80 流量的網路安全性群組 (NSG) 規則。</span><span class="sxs-lookup"><span data-stu-id="5ae08-191">If using an Azure Ubuntu VM, add a Network Security Group (NSG) rule that enables inbound port 80 traffic.</span></span> <span data-ttu-id="5ae08-192">沒有必要啟用輸出連接埠 80 規則，因為啓用輸入規則時會自動授與輸出流量。</span><span class="sxs-lookup"><span data-stu-id="5ae08-192">There's no need to enable an outbound port 80 rule, as the outbound traffic is automatically granted when the inbound rule is enabled.</span></span>

<span data-ttu-id="5ae08-193">應用程式測試完成後，請在命令提示字元以 `Ctrl+C` 關閉應用程式。</span><span class="sxs-lookup"><span data-stu-id="5ae08-193">When done testing the app, shut the app down with `Ctrl+C` at the command prompt.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="5ae08-194">監視應用程式</span><span class="sxs-lookup"><span data-stu-id="5ae08-194">Monitoring the app</span></span>

<span data-ttu-id="5ae08-195">伺服器已設定完成，可將對 `http://<serveraddress>:80` 發出的要求轉送給在位於 `http://127.0.0.1:5000` 的 Kestrel 上執行的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5ae08-195">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="5ae08-196">不過，並未設定 Nginx 來管理 Kestrel 處理序。</span><span class="sxs-lookup"><span data-stu-id="5ae08-196">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="5ae08-197">您可以使用 *systemd* 來建立服務檔案，以啟動並監視基礎 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5ae08-197">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="5ae08-198">*systemd* 是 init 系統，提供許多強大的啟動、停止和管理處理程序功能。</span><span class="sxs-lookup"><span data-stu-id="5ae08-198">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="5ae08-199">建立服務檔</span><span class="sxs-lookup"><span data-stu-id="5ae08-199">Create the service file</span></span>

<span data-ttu-id="5ae08-200">建立服務定義檔：</span><span class="sxs-lookup"><span data-stu-id="5ae08-200">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-helloapp.service
```

<span data-ttu-id="5ae08-201">以下是一個應用程式範例服務檔：</span><span class="sxs-lookup"><span data-stu-id="5ae08-201">The following is an example service file for the app:</span></span>

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

<span data-ttu-id="5ae08-202">如果設定不是使用 *www-data* 這個使用者，就必須先建立這裡所定義的使用者，並授與適當的檔案擁有權。</span><span class="sxs-lookup"><span data-stu-id="5ae08-202">If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="5ae08-203">使用 `TimeoutStopSec` 可設定應用程式收到初始中斷訊號之後等待關閉的時間。</span><span class="sxs-lookup"><span data-stu-id="5ae08-203">Use `TimeoutStopSec` to configure the duration of time to wait for the app to shut down after it receives the initial interrupt signal.</span></span> <span data-ttu-id="5ae08-204">如果應用程式在此期間後未關閉，則會發出 SIGKILL 來終止應用程式。</span><span class="sxs-lookup"><span data-stu-id="5ae08-204">If the app doesn't shut down in this period, SIGKILL is issued to terminate the app.</span></span> <span data-ttu-id="5ae08-205">提供不具單位的秒值 (例如 `150`)、時間範圍值 (例如 `2min 30s`) 或 `infinity` (表示停用逾時)。</span><span class="sxs-lookup"><span data-stu-id="5ae08-205">Provide the value as unitless seconds (for example, `150`), a time span value (for example, `2min 30s`), or `infinity` to disable the timeout.</span></span> <span data-ttu-id="5ae08-206">`TimeoutStopSec` 在管理員設定檔 (*systemd-system.conf*、*system.conf.d*、*systemd-user.conf*、*user.conf.d*) 的預設值為 `DefaultTimeoutStopSec`。</span><span class="sxs-lookup"><span data-stu-id="5ae08-206">`TimeoutStopSec` defaults to the value of `DefaultTimeoutStopSec` in the manager configuration file (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*, *user.conf.d*).</span></span> <span data-ttu-id="5ae08-207">大多數發行版本的預設逾時為 90 秒。</span><span class="sxs-lookup"><span data-stu-id="5ae08-207">The default timeout for most distributions is 90 seconds.</span></span>

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

<span data-ttu-id="5ae08-208">Linux 的檔案系統會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="5ae08-208">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="5ae08-209">將 ASPNETCORE_ENVIRONMENT 設定為 "Production" 會導致搜尋 *appsettings.Production.json* 設定檔，而不是搜尋 *appsettings.production.json*。</span><span class="sxs-lookup"><span data-stu-id="5ae08-209">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

<span data-ttu-id="5ae08-210">有些值 (例如 SQL 連接字串) 必須以逸出方式處理，設定提供者才能讀取環境變數。</span><span class="sxs-lookup"><span data-stu-id="5ae08-210">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="5ae08-211">請使用下列命令來產生要在設定檔中使用並已適當逸出的值：</span><span class="sxs-lookup"><span data-stu-id="5ae08-211">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>

```console
systemd-escape "<value-to-escape>"
```

<span data-ttu-id="5ae08-212">儲存檔案並啟用服務。</span><span class="sxs-lookup"><span data-stu-id="5ae08-212">Save the file and enable the service.</span></span>

```bash
sudo systemctl enable kestrel-helloapp.service
```

<span data-ttu-id="5ae08-213">啟動服務並確認它正在執行。</span><span class="sxs-lookup"><span data-stu-id="5ae08-213">Start the service and verify that it's running.</span></span>

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

<span data-ttu-id="5ae08-214">設定好反向 Proxy 並透過 systemd 管理 Kestrel 之後，Web 應用程式便已完全設定妥當，而從本機電腦瀏覽器透過 `http://localhost` 即可存取它。</span><span class="sxs-lookup"><span data-stu-id="5ae08-214">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="5ae08-215">此外，也可以從遠端電腦存取它，除非遭到任何防火牆封鎖。</span><span class="sxs-lookup"><span data-stu-id="5ae08-215">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="5ae08-216">檢查回應標頭時，`Server` 標頭會顯示是由 Kestrel 為 ASP.NET Core 應用程式提供服務。</span><span class="sxs-lookup"><span data-stu-id="5ae08-216">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="5ae08-217">檢視記錄</span><span class="sxs-lookup"><span data-stu-id="5ae08-217">Viewing logs</span></span>

<span data-ttu-id="5ae08-218">由於是使用 `systemd` 來管理使用 Kestrel 的 Web 應用程式，因此會將所有事件和處理序都記錄在集中式日誌中。</span><span class="sxs-lookup"><span data-stu-id="5ae08-218">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="5ae08-219">不過，此日誌包含 `systemd` 管理的所有服務和處理程序的所有項目。</span><span class="sxs-lookup"><span data-stu-id="5ae08-219">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="5ae08-220">若要檢視 `kestrel-helloapp.service` 的特定項目，請使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="5ae08-220">To view the `kestrel-helloapp.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service
```

<span data-ttu-id="5ae08-221">如需進一步篩選，例如 `--since today`、`--until 1 hour ago` 或這些項目的組合等時間選項，可以減少傳回的項目數量。</span><span class="sxs-lookup"><span data-stu-id="5ae08-221">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a><span data-ttu-id="5ae08-222">資料保護</span><span class="sxs-lookup"><span data-stu-id="5ae08-222">Data protection</span></span>

<span data-ttu-id="5ae08-223">[ASP.NET Core 資料保護堆疊](xref:security/data-protection/index)由數個 ASP.NET Core[ 中介軟體](xref:fundamentals/middleware/index)使用，包括驗證中介軟體 (例如，cookie 中介軟體) 與跨網站偽造要求 (CSRF) 保護。</span><span class="sxs-lookup"><span data-stu-id="5ae08-223">The [ASP.NET Core Data Protection stack](xref:security/data-protection/index) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including authentication middleware (for example, cookie middleware) and cross-site request forgery (CSRF) protections.</span></span> <span data-ttu-id="5ae08-224">即使資料保護 API 並非由使用者程式碼呼叫，仍應設定資料保護，以建立持續密碼編譯[金鑰存放區](xref:security/data-protection/implementation/key-management)。</span><span class="sxs-lookup"><span data-stu-id="5ae08-224">Even if Data Protection APIs aren't called by user code, data protection should be configured to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="5ae08-225">如不設定資料保護，金鑰會保留在記憶體中，並於應用程式重新啟動時捨棄。</span><span class="sxs-lookup"><span data-stu-id="5ae08-225">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="5ae08-226">如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：</span><span class="sxs-lookup"><span data-stu-id="5ae08-226">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="5ae08-227">所有以 Cookie 為基礎的驗證權杖都會失效。</span><span class="sxs-lookup"><span data-stu-id="5ae08-227">All cookie-based authentication tokens are invalidated.</span></span>
* <span data-ttu-id="5ae08-228">當使用者提出下一個要求時，需要再次登入。</span><span class="sxs-lookup"><span data-stu-id="5ae08-228">Users are required to sign in again on their next request.</span></span>
* <span data-ttu-id="5ae08-229">所有以 Keyring 保護的資料都無法再解密。</span><span class="sxs-lookup"><span data-stu-id="5ae08-229">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="5ae08-230">這可能會包含 [CSRF 權杖](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)和 [ASP.NET Core MVC TempData cookie](xref:fundamentals/app-state#tempdata)。</span><span class="sxs-lookup"><span data-stu-id="5ae08-230">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="5ae08-231">若要設定資料保護來保存及加密金鑰環，請參閱：</span><span class="sxs-lookup"><span data-stu-id="5ae08-231">To configure data protection to persist and encrypt the key ring, see:</span></span>

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="securing-the-app"></a><span data-ttu-id="5ae08-232">確保應用程式的安全性</span><span class="sxs-lookup"><span data-stu-id="5ae08-232">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="5ae08-233">啟用 AppArmor</span><span class="sxs-lookup"><span data-stu-id="5ae08-233">Enable AppArmor</span></span>

<span data-ttu-id="5ae08-234">Linux 安全性模組 (LSM) 是 Linux 2.6 之後 Linux 核心所包含的一個架構。</span><span class="sxs-lookup"><span data-stu-id="5ae08-234">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="5ae08-235">LSM 支援不同的安全性模組實作。</span><span class="sxs-lookup"><span data-stu-id="5ae08-235">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="5ae08-236">[AppArmor](https://wiki.ubuntu.com/AppArmor) 是 LSM，它實作的必要存取控制系統，可將程式限定在有限的資源集中。</span><span class="sxs-lookup"><span data-stu-id="5ae08-236">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="5ae08-237">確定已啟用並正確設定 AppArmor。</span><span class="sxs-lookup"><span data-stu-id="5ae08-237">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="5ae08-238">設定防火牆</span><span class="sxs-lookup"><span data-stu-id="5ae08-238">Configuring the firewall</span></span>

<span data-ttu-id="5ae08-239">關閉所有不在使用中的外部連接埠。</span><span class="sxs-lookup"><span data-stu-id="5ae08-239">Close off all external ports that are not in use.</span></span> <span data-ttu-id="5ae08-240">簡單的防火牆 (ufw) 提供命令列介面供設定防火牆，為 `iptables` 提供前端。</span><span class="sxs-lookup"><span data-stu-id="5ae08-240">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span>

> [!WARNING]
> <span data-ttu-id="5ae08-241">如未正確設定，防火牆會禁止存取整個系統。</span><span class="sxs-lookup"><span data-stu-id="5ae08-241">A firewall will prevent access to the whole system if not configured correctly.</span></span> <span data-ttu-id="5ae08-242">未指定正確的 SSH 連接埠，將會導致您無法存取系統 (若您使用 SSH 連線至該連接埠)。</span><span class="sxs-lookup"><span data-stu-id="5ae08-242">Failure to specify the correct SSH port will effectively lock you out of the system if you are using SSH to connect to it.</span></span> <span data-ttu-id="5ae08-243">預設連接埠為 22。</span><span class="sxs-lookup"><span data-stu-id="5ae08-243">The default port is 22.</span></span> <span data-ttu-id="5ae08-244">如需詳細資訊，請參閱 [ufw 簡介](https://help.ubuntu.com/community/UFW)與[手冊](http://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html)。</span><span class="sxs-lookup"><span data-stu-id="5ae08-244">For more information, see the [introduction to ufw](https://help.ubuntu.com/community/UFW) and the [manual](http://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).</span></span>

<span data-ttu-id="5ae08-245">安裝 `ufw` 並將其設定為允許任何所需連接埠上的流量。</span><span class="sxs-lookup"><span data-stu-id="5ae08-245">Install `ufw` and configure it to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw

sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

sudo ufw enable
```

### <a name="securing-nginx"></a><span data-ttu-id="5ae08-246">保護 Nginx</span><span class="sxs-lookup"><span data-stu-id="5ae08-246">Securing Nginx</span></span>

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="5ae08-247">變更 Nginx 回應名稱</span><span class="sxs-lookup"><span data-stu-id="5ae08-247">Change the Nginx response name</span></span>

<span data-ttu-id="5ae08-248">編輯 *src/http/ngx_http_header_filter_module.c*：</span><span class="sxs-lookup"><span data-stu-id="5ae08-248">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a><span data-ttu-id="5ae08-249">設定選項</span><span class="sxs-lookup"><span data-stu-id="5ae08-249">Configure options</span></span>

<span data-ttu-id="5ae08-250">設定伺服器的其他所需模組。</span><span class="sxs-lookup"><span data-stu-id="5ae08-250">Configure the server with additional required modules.</span></span> <span data-ttu-id="5ae08-251">請考慮使用 [ModSecurity](https://www.modsecurity.org/) 等 Web 應用程式防火牆來強化應用程式。</span><span class="sxs-lookup"><span data-stu-id="5ae08-251">Consider using a web app firewall, such as [ModSecurity](https://www.modsecurity.org/), to harden the app.</span></span>

#### <a name="configure-ssl"></a><span data-ttu-id="5ae08-252">設定 SSL</span><span class="sxs-lookup"><span data-stu-id="5ae08-252">Configure SSL</span></span>

* <span data-ttu-id="5ae08-253">藉由指定由受信任憑證授權單位 (CA) 核發的有效憑證，將伺服器設定成在連接埠 `443` 上接聽 HTTPS 流量。</span><span class="sxs-lookup"><span data-stu-id="5ae08-253">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="5ae08-254">採用以下 */etc/nginx/nginx.conf* 檔案所述的一些做法來強化安全性。</span><span class="sxs-lookup"><span data-stu-id="5ae08-254">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="5ae08-255">範例包括選擇更強的加密，重新導向 HTTPS 到 HTTP 的所有流量。</span><span class="sxs-lookup"><span data-stu-id="5ae08-255">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="5ae08-256">新增 `HTTP Strict-Transport-Security` (HSTS) 標頭可確保用戶端提出的所有後續要求都只會透過 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="5ae08-256">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="5ae08-257">如果未來將會停用 SSL，則不要新增 Strict-Transport-Security 標頭，或選擇適當的 `max-age`。</span><span class="sxs-lookup"><span data-stu-id="5ae08-257">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="5ae08-258">新增 */etc/nginx/Proxy.conf* 組態檔：</span><span class="sxs-lookup"><span data-stu-id="5ae08-258">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="5ae08-259">編輯 */etc/nginx/Proxy.conf* 組態檔。</span><span class="sxs-lookup"><span data-stu-id="5ae08-259">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="5ae08-260">此範例在一個組態檔中同時包含 `http` 和 `server` 區段。</span><span class="sxs-lookup"><span data-stu-id="5ae08-260">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="5ae08-261">保護 Nginx 免於點閱綁架</span><span class="sxs-lookup"><span data-stu-id="5ae08-261">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="5ae08-262">點閱綁架是收集受感染使用者按一下動作的惡意技術。</span><span class="sxs-lookup"><span data-stu-id="5ae08-262">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="5ae08-263">點閱綁架誘騙受害者 (訪客) 到受感染的網站上執行按一下的動作。</span><span class="sxs-lookup"><span data-stu-id="5ae08-263">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="5ae08-264">使用 X-FRAME-OPTIONS 來保護網站。</span><span class="sxs-lookup"><span data-stu-id="5ae08-264">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="5ae08-265">編輯 *nginx.conf* 檔案：</span><span class="sxs-lookup"><span data-stu-id="5ae08-265">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="5ae08-266">新增行 `add_header X-Frame-Options "SAMEORIGIN";` 並儲存檔案，然後重新啟動 Nginx。</span><span class="sxs-lookup"><span data-stu-id="5ae08-266">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="5ae08-267">MIME 類型探查</span><span class="sxs-lookup"><span data-stu-id="5ae08-267">MIME-type sniffing</span></span>

<span data-ttu-id="5ae08-268">此標頭會防止大部分的瀏覽器對遠離宣告內容類型的回應進行 MIME 探查，因為標頭會指示瀏覽器不要覆寫回應內容類型。</span><span class="sxs-lookup"><span data-stu-id="5ae08-268">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="5ae08-269">使用 `nosniff` 選項，如果伺服器顯示的內容是 "text/html"，則瀏覽器就會將它轉譯為 "text/html"。</span><span class="sxs-lookup"><span data-stu-id="5ae08-269">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="5ae08-270">編輯 *nginx.conf* 檔案：</span><span class="sxs-lookup"><span data-stu-id="5ae08-270">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="5ae08-271">新增行 `add_header X-Content-Type-Options "nosniff";` 並儲存檔案，然後重新啟動 Nginx。</span><span class="sxs-lookup"><span data-stu-id="5ae08-271">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5ae08-272">其他資源</span><span class="sxs-lookup"><span data-stu-id="5ae08-272">Additional resources</span></span>

* [<span data-ttu-id="5ae08-273">Linux 上 .NET Core 的必要條件</span><span class="sxs-lookup"><span data-stu-id="5ae08-273">Prerequisites for .NET Core on Linux</span></span>](/dotnet/core/linux-prerequisites)
* <span data-ttu-id="5ae08-274">[Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages) (Nginx：二進位版本：正式的 Debian Ubuntu 套件)</span><span class="sxs-lookup"><span data-stu-id="5ae08-274">[Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)</span></span>
* [<span data-ttu-id="5ae08-275">設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器</span><span class="sxs-lookup"><span data-stu-id="5ae08-275">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
* [<span data-ttu-id="5ae08-276">NGINX：使用轉送的標頭</span><span class="sxs-lookup"><span data-stu-id="5ae08-276">NGINX: Using the Forwarded header</span></span>](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
