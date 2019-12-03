---
title: 在 Linux 上使用 Apache 裝載 ASP.NET Core
author: guardrex
description: 了解如何在 CentOS 上將 Apache 設定為反向 Proxy 伺服器，以將 HTTP 流量重新導向至在 Kestrel 上執行的 ASP.NET Core Web 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: shboyer
ms.custom: mvc
ms.date: 12/02/2019
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 730ed1847ec5728657d56db3ccf0f1f5fab6b5dd
ms.sourcegitcommit: 3b6b0a54b20dc99b0c8c5978400c60adf431072f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2019
ms.locfileid: "74717360"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="d183d-103">在 Linux 上使用 Apache 裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d183d-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="d183d-104">作者：[Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="d183d-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="d183d-105">使用本指南來了解如何在 [CentOS 7](https://www.centos.org/) 上將 [Apache](https://httpd.apache.org/) 設定為反向 Proxy 伺服器，以將 HTTP 流量重新導向至在 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器上執行的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d183d-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="d183d-106">[mod_proxy 延伸模組](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html)和相關的模組會建立伺服器的反向 Proxy。</span><span class="sxs-lookup"><span data-stu-id="d183d-106">The [mod_proxy extension](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d183d-107">必要條件：</span><span class="sxs-lookup"><span data-stu-id="d183d-107">Prerequisites</span></span>

* <span data-ttu-id="d183d-108">執行 CentOS 7 的伺服器搭配具有 sudo 權限的標準使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="d183d-108">Server running CentOS 7 with a standard user account with sudo privilege.</span></span>
* <span data-ttu-id="d183d-109">在伺服器上安裝 .NET Core 執行階段。</span><span class="sxs-lookup"><span data-stu-id="d183d-109">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="d183d-110">請造訪[下載 .Net Core 頁面](https://dotnet.microsoft.com/download/dotnet-core)。</span><span class="sxs-lookup"><span data-stu-id="d183d-110">Visit the [Download .NET Core page](https://dotnet.microsoft.com/download/dotnet-core).</span></span>
   1. <span data-ttu-id="d183d-111">選取最新的非預覽 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="d183d-111">Select the latest non-preview .NET Core version.</span></span>
   1. <span data-ttu-id="d183d-112">在 [**執行應用程式-運行**時間] 下的表格中，下載最新的非預覽執行時間。</span><span class="sxs-lookup"><span data-stu-id="d183d-112">Download the latest non-preview runtime in the table under **Run apps - Runtime**.</span></span>
   1. <span data-ttu-id="d183d-113">選取 [Linux**套件管理員指示**] 連結，並遵循 CentOS 指示。</span><span class="sxs-lookup"><span data-stu-id="d183d-113">Select the Linux **Package manager instructions** link and follow the CentOS instructions.</span></span>
* <span data-ttu-id="d183d-114">現有的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d183d-114">An existing ASP.NET Core app.</span></span>

<span data-ttu-id="d183d-115">在未來升級共用架構之後的任何時間點，重新開機伺服器所裝載的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d183d-115">At any point in the future after upgrading the shared framework, restart the ASP.NET Core apps hosted by the server.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="d183d-116">跨應用程式發佈與複製</span><span class="sxs-lookup"><span data-stu-id="d183d-116">Publish and copy over the app</span></span>

<span data-ttu-id="d183d-117">為[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="d183d-117">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="d183d-118">如果應用程式在本機執行且未設定為進行安全連線 (HTTPS)，請採用下列任一方法：</span><span class="sxs-lookup"><span data-stu-id="d183d-118">If the app is run locally and isn't configured to make secure connections (HTTPS), adopt either of the following approaches:</span></span>

* <span data-ttu-id="d183d-119">設定應用程式以處理安全的本機連線。</span><span class="sxs-lookup"><span data-stu-id="d183d-119">Configure the app to handle secure local connections.</span></span> <span data-ttu-id="d183d-120">如需詳細資訊，請參閱 [HTTPS 組態](#https-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="d183d-120">For more information, see the [HTTPS configuration](#https-configuration) section.</span></span>
* <span data-ttu-id="d183d-121">從 *Properties/launchSettings.json* 檔案中的 `applicationUrl` 屬性移除 `https://localhost:5001` (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="d183d-121">Remove `https://localhost:5001` (if present) from the `applicationUrl` property in the *Properties/launchSettings.json* file.</span></span>

<span data-ttu-id="d183d-122">從開發環境執行 [dotnet publish](/dotnet/core/tools/dotnet-publish) 將應用程式封裝到可在伺服器上執行的目錄 (例如，*bin/Release/&lt;target_framework_moniker&gt;/publish*)：</span><span class="sxs-lookup"><span data-stu-id="d183d-122">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```dotnetcli
dotnet publish --configuration Release
```

<span data-ttu-id="d183d-123">如果您不想在伺服器上維護 .NET Core 執行階段，應用程式也可以發佈為[獨立式部署](/dotnet/core/deploying/#self-contained-deployments-scd)。</span><span class="sxs-lookup"><span data-stu-id="d183d-123">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="d183d-124">使用整合至組織工作流程的工具 (SCP、SFTP 等等) 將 ASP.NET Core 應用程式複製到伺服器。</span><span class="sxs-lookup"><span data-stu-id="d183d-124">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="d183d-125">Web 應用程式通常可在 *var* 目錄下找到 (例如 *var/www/helloapp*)。</span><span class="sxs-lookup"><span data-stu-id="d183d-125">It's common to locate web apps under the *var* directory (for example, *var/www/helloapp*).</span></span>

> [!NOTE]
> <span data-ttu-id="d183d-126">在生產環境部署案例中，持續整合工作流程會執行發佈應用程式並將資產複製到伺服器的工作。</span><span class="sxs-lookup"><span data-stu-id="d183d-126">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

## <a name="configure-a-proxy-server"></a><span data-ttu-id="d183d-127">設定 Proxy 伺服器</span><span class="sxs-lookup"><span data-stu-id="d183d-127">Configure a proxy server</span></span>

<span data-ttu-id="d183d-128">反向 Proxy 是為動態 Web 應用程式提供服務的常見設定。</span><span class="sxs-lookup"><span data-stu-id="d183d-128">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="d183d-129">反向 Proxy 會終止 HTTP 要求，並將它轉送至 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d183d-129">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="d183d-130">Proxy 伺服器則是會將用戶端要求轉送至另一部伺服器，而不是自己完成這些要求。</span><span class="sxs-lookup"><span data-stu-id="d183d-130">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="d183d-131">反向 Proxy 會轉送至固定目的地，通常代表任意的用戶端。</span><span class="sxs-lookup"><span data-stu-id="d183d-131">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="d183d-132">在本指南中，是將 Apache 設定成反向 Proxy，且執行所在的伺服器與 Kestrel 為 ASP.NET Core 應用程式提供服務的伺服器相同。</span><span class="sxs-lookup"><span data-stu-id="d183d-132">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="d183d-133">由於反向 Proxy 會轉送要求，因此請使用來自 [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) 套件的[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="d183d-133">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="d183d-134">此中介軟體會使用 `X-Forwarded-Proto` 標頭來更新 `Request.Scheme`，以便讓重新導向 URI 及其他安全性原則正確運作。</span><span class="sxs-lookup"><span data-stu-id="d183d-134">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="d183d-135">任何依賴配置的元件，例如驗證、連結產生、重新導向和地理位置，都必須在叫用轉送的標頭中介軟體後放置。</span><span class="sxs-lookup"><span data-stu-id="d183d-135">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="d183d-136">轉送的標頭中介軟體是一般規則，應該先於診斷和錯誤處理中介軟體以外的其他中介軟體執行。</span><span class="sxs-lookup"><span data-stu-id="d183d-136">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="d183d-137">這種排序可確保依賴轉送標頭資訊的中介軟體可以耗用用於處理的標頭值。</span><span class="sxs-lookup"><span data-stu-id="d183d-137">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

<span data-ttu-id="d183d-138">請先在 `Startup.Configure` 中叫用 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> 方法，再呼叫 <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> 或類似的驗證配置中介軟體。</span><span class="sxs-lookup"><span data-stu-id="d183d-138">Invoke the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> method in `Startup.Configure` before calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> or similar authentication scheme middleware.</span></span> <span data-ttu-id="d183d-139">請設定中介軟體來轉送 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭：</span><span class="sxs-lookup"><span data-stu-id="d183d-139">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

<span data-ttu-id="d183d-140">如果未將任何 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> 指定給中介軟體，則要轉送的預設標頭會是 `None`。</span><span class="sxs-lookup"><span data-stu-id="d183d-140">If no <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="d183d-141">在預設情況下，在回送位址 (127.0.0.0/8, [::1]) 上執行的 Proxy (包括標準的本機位址 (127.0.0.1)) 是受信任的。</span><span class="sxs-lookup"><span data-stu-id="d183d-141">Proxies running on loopback addresses (127.0.0.0/8, [::1]), including the standard localhost address (127.0.0.1), are trusted by default.</span></span> <span data-ttu-id="d183d-142">如果組織內有其他受信任的 Proxy 或網路處理網際網路與網頁伺服器之間的要求，請使用 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>，將其新增至 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> 或 <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> 清單。</span><span class="sxs-lookup"><span data-stu-id="d183d-142">If other trusted proxies or networks within the organization handle requests between the Internet and the web server, add them to the list of <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> or <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span></span> <span data-ttu-id="d183d-143">下列範例會將 IP 位址 10.0.0.100 的受信任 Proxy 伺服器新增至 `Startup.ConfigureServices` 中「轉送的標頭中介軟體」的 `KnownProxies`：</span><span class="sxs-lookup"><span data-stu-id="d183d-143">The following example adds a trusted proxy server at IP address 10.0.0.100 to the Forwarded Headers Middleware `KnownProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

<span data-ttu-id="d183d-144">如需詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="d183d-144">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

### <a name="install-apache"></a><span data-ttu-id="d183d-145">安裝 Apache</span><span class="sxs-lookup"><span data-stu-id="d183d-145">Install Apache</span></span>

<span data-ttu-id="d183d-146">將 CentOS 套件更新至其最新穩定版本：</span><span class="sxs-lookup"><span data-stu-id="d183d-146">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="d183d-147">使用單一 `yum` 命令在 CentOS 上安裝 Apache 網頁伺服器：</span><span class="sxs-lookup"><span data-stu-id="d183d-147">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="d183d-148">執行命令之後的範例輸出：</span><span class="sxs-lookup"><span data-stu-id="d183d-148">Sample output after running the command:</span></span>

```bash
Downloading packages:
httpd-2.4.6-40.el7.centos.4.x86_64.rpm               | 2.7 MB  00:00:01
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
Installing : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 
Verifying  : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 

Installed:
httpd.x86_64 0:2.4.6-40.el7.centos.4

Complete!
```

> [!NOTE]
> <span data-ttu-id="d183d-149">在此範例中，輸出會反映 httpd.86_64，因為 CentOS 第 7 版是 64 位元。</span><span class="sxs-lookup"><span data-stu-id="d183d-149">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="d183d-150">若要確認 Apache 的安裝位置，請從命令提示字元執行 `whereis httpd`。</span><span class="sxs-lookup"><span data-stu-id="d183d-150">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache"></a><span data-ttu-id="d183d-151">設定 Apache</span><span class="sxs-lookup"><span data-stu-id="d183d-151">Configure Apache</span></span>

<span data-ttu-id="d183d-152">Apache 的組態檔是位於 `/etc/httpd/conf.d/` 目錄內。</span><span class="sxs-lookup"><span data-stu-id="d183d-152">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="d183d-153">除了 `/etc/httpd/conf.modules.d/` (包含載入模組所需的所有設定檔) 中的模組設定檔之外，任何副檔名為 *.conf* 的檔案也會依字母順序處理。</span><span class="sxs-lookup"><span data-stu-id="d183d-153">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="d183d-154">為應用程式建立名為 *helloapp.conf* 的設定檔：</span><span class="sxs-lookup"><span data-stu-id="d183d-154">Create a configuration file, named *helloapp.conf*, for the app:</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ServerName www.example.com
    ServerAlias *.example.com
    ErrorLog ${APACHE_LOG_DIR}helloapp-error.log
    CustomLog ${APACHE_LOG_DIR}helloapp-access.log common
</VirtualHost>
```

<span data-ttu-id="d183d-155">`VirtualHost` 區塊可以在伺服器上的一或多個檔案中出現多次。</span><span class="sxs-lookup"><span data-stu-id="d183d-155">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="d183d-156">在上述設定檔中，Apache 會在連接埠 80 接受公用流量。</span><span class="sxs-lookup"><span data-stu-id="d183d-156">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="d183d-157">所服務的網域是 `www.example.com`，而 `*.example.com` 別名則會解析成同一個網站。</span><span class="sxs-lookup"><span data-stu-id="d183d-157">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="d183d-158">如需詳細資訊，請參閱[名稱型虛擬主機支援](https://httpd.apache.org/docs/current/vhosts/name-based.html) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="d183d-158">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="d183d-159">要求會在根目錄透過 Proxy 傳送至位於 127.0.0.1 之伺服器的連接埠 5000。</span><span class="sxs-lookup"><span data-stu-id="d183d-159">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="d183d-160">如需進行雙向通訊，則必須要有 `ProxyPass` 和 `ProxyPassReverse`。</span><span class="sxs-lookup"><span data-stu-id="d183d-160">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span> <span data-ttu-id="d183d-161">若要變更 Kestrel 的 IP/連接埠，請參閱 [Kestrel：端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration)。</span><span class="sxs-lookup"><span data-stu-id="d183d-161">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="d183d-162">如果無法在 **VirtualHost** 區塊中指定適當的 [ServerName 指示詞](https://httpd.apache.org/docs/current/mod/core.html#servername)，就會讓應用程式暴露在安全性弱點的風險下。</span><span class="sxs-lookup"><span data-stu-id="d183d-162">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="d183d-163">若您擁有整個父網域 (相對於易受攻擊的 `*.com`) 的控制權，子網域萬用字元繫結 (例如 `*.example.com`) 就沒有此安全性風險。</span><span class="sxs-lookup"><span data-stu-id="d183d-163">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="d183d-164">如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="d183d-164">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="d183d-165">您可以使用 `ErrorLog` 和 `CustomLog` 指示詞來依 `VirtualHost` 設定記錄功能。</span><span class="sxs-lookup"><span data-stu-id="d183d-165">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="d183d-166">`ErrorLog` 是伺服器記錄錯誤的位置，而 `CustomLog` 則會設定記錄檔的檔案名稱和格式。</span><span class="sxs-lookup"><span data-stu-id="d183d-166">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="d183d-167">在此案例中，這就是記錄要求資訊的位置。</span><span class="sxs-lookup"><span data-stu-id="d183d-167">In this case, this is where request information is logged.</span></span> <span data-ttu-id="d183d-168">每個要求都會有一行。</span><span class="sxs-lookup"><span data-stu-id="d183d-168">There's one line for each request.</span></span>

<span data-ttu-id="d183d-169">請儲存檔案並測試設定。</span><span class="sxs-lookup"><span data-stu-id="d183d-169">Save the file and test the configuration.</span></span> <span data-ttu-id="d183d-170">如果所有項目都通過，回應應該是 `Syntax [OK]`。</span><span class="sxs-lookup"><span data-stu-id="d183d-170">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="d183d-171">重新啟動 Apache：</span><span class="sxs-lookup"><span data-stu-id="d183d-171">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitor-the-app"></a><span data-ttu-id="d183d-172">監視應用程式</span><span class="sxs-lookup"><span data-stu-id="d183d-172">Monitor the app</span></span>

<span data-ttu-id="d183d-173">Apache 現在已設定完成，可將對 `http://localhost:80` 發出的要求轉送給在位於 `http://127.0.0.1:5000` 的 Kestrel 上執行的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d183d-173">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="d183d-174">不過，並未設定 Apache 來管理 Kestrel 處理序。</span><span class="sxs-lookup"><span data-stu-id="d183d-174">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="d183d-175">請使用 *systemd* 並建立服務檔案，以啟動並監視基礎 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d183d-175">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="d183d-176">*systemd* 是 init 系統，提供許多強大的啟動、停止和管理處理程序功能。</span><span class="sxs-lookup"><span data-stu-id="d183d-176">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span>

### <a name="create-the-service-file"></a><span data-ttu-id="d183d-177">建立服務檔</span><span class="sxs-lookup"><span data-stu-id="d183d-177">Create the service file</span></span>

<span data-ttu-id="d183d-178">建立服務定義檔：</span><span class="sxs-lookup"><span data-stu-id="d183d-178">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-helloapp.service
```

<span data-ttu-id="d183d-179">應用程式的範例服務檔：</span><span class="sxs-lookup"><span data-stu-id="d183d-179">An example service file for the app:</span></span>

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/www/helloapp
ExecStart=/usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="d183d-180">如果設定不是使用 *apache* 這個使用者，就必須先建立使用者並授與適當的檔案擁有權。</span><span class="sxs-lookup"><span data-stu-id="d183d-180">If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership of files.</span></span>

<span data-ttu-id="d183d-181">使用 `TimeoutStopSec` 可設定應用程式收到初始中斷訊號之後等待關閉的時間。</span><span class="sxs-lookup"><span data-stu-id="d183d-181">Use `TimeoutStopSec` to configure the duration of time to wait for the app to shut down after it receives the initial interrupt signal.</span></span> <span data-ttu-id="d183d-182">如果應用程式在此期間後未關閉，則會發出 SIGKILL 來終止應用程式。</span><span class="sxs-lookup"><span data-stu-id="d183d-182">If the app doesn't shut down in this period, SIGKILL is issued to terminate the app.</span></span> <span data-ttu-id="d183d-183">提供不具單位的秒值 (例如 `150`)、時間範圍值 (例如 `2min 30s`) 或 `infinity` (表示停用逾時)。</span><span class="sxs-lookup"><span data-stu-id="d183d-183">Provide the value as unitless seconds (for example, `150`), a time span value (for example, `2min 30s`), or `infinity` to disable the timeout.</span></span> <span data-ttu-id="d183d-184">`TimeoutStopSec` 在管理員設定檔 (*systemd-system.conf*、*system.conf.d*、*systemd-user.conf*、*user.conf.d*) 的預設值為 `DefaultTimeoutStopSec`。</span><span class="sxs-lookup"><span data-stu-id="d183d-184">`TimeoutStopSec` defaults to the value of `DefaultTimeoutStopSec` in the manager configuration file (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*, *user.conf.d*).</span></span> <span data-ttu-id="d183d-185">大多數發行版本的預設逾時為 90 秒。</span><span class="sxs-lookup"><span data-stu-id="d183d-185">The default timeout for most distributions is 90 seconds.</span></span>

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

<span data-ttu-id="d183d-186">有些值 (例如 SQL 連接字串) 必須以逸出方式處理，設定提供者才能讀取環境變數。</span><span class="sxs-lookup"><span data-stu-id="d183d-186">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="d183d-187">請使用下列命令來產生要在設定檔中使用並已適當逸出的值：</span><span class="sxs-lookup"><span data-stu-id="d183d-187">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>

```console
systemd-escape "<value-to-escape>"
```

<span data-ttu-id="d183d-188">環境變數名稱不支援冒號 (`:`) 分隔符號。</span><span class="sxs-lookup"><span data-stu-id="d183d-188">Colon (`:`) separators aren't supported in environment variable names.</span></span> <span data-ttu-id="d183d-189">請使用雙底線 (`__`) 來取代冒號。</span><span class="sxs-lookup"><span data-stu-id="d183d-189">Use a double underscore (`__`) in place of a colon.</span></span> <span data-ttu-id="d183d-190">[環境變數組態提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)會在將環境變數讀入組態時，將雙底線轉換為冒號。</span><span class="sxs-lookup"><span data-stu-id="d183d-190">The [Environment Variables configuration provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider) converts double-underscores into colons when environment variables are read into configuration.</span></span> <span data-ttu-id="d183d-191">在下列範例中，連接字串索引鍵 `ConnectionStrings:DefaultConnection` 會設定為服務定義檔中的 `ConnectionStrings__DefaultConnection`：</span><span class="sxs-lookup"><span data-stu-id="d183d-191">In the following example, the connection string key `ConnectionStrings:DefaultConnection` is set into the service definition file as `ConnectionStrings__DefaultConnection`:</span></span>

```
Environment=ConnectionStrings__DefaultConnection={Connection String}
```

<span data-ttu-id="d183d-192">儲存檔案並啟用服務：</span><span class="sxs-lookup"><span data-stu-id="d183d-192">Save the file and enable the service:</span></span>

```bash
sudo systemctl enable kestrel-helloapp.service
```

<span data-ttu-id="d183d-193">啟動服務並確認它正在執行：</span><span class="sxs-lookup"><span data-stu-id="d183d-193">Start the service and verify that it's running:</span></span>

```bash
sudo systemctl start kestrel-helloapp.service
sudo systemctl status kestrel-helloapp.service

● kestrel-helloapp.service - Example .NET Web API App running on CentOS 7
    Loaded: loaded (/etc/systemd/system/kestrel-helloapp.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-helloapp.service
            └─9021 /usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
```

<span data-ttu-id="d183d-194">設定好反向 Proxy 並透過 *systemd* 管理 Kestrel 之後，Web 應用程式便已完全設定妥當，而從本機電腦瀏覽器透過 `http://localhost` 即可存取它。</span><span class="sxs-lookup"><span data-stu-id="d183d-194">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="d183d-195">檢查回應標頭時，**Server** 標頭會指出是由 Kestrel 為 ASP.NET Core 應用程式提供服務：</span><span class="sxs-lookup"><span data-stu-id="d183d-195">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="view-logs"></a><span data-ttu-id="d183d-196">檢視記錄</span><span class="sxs-lookup"><span data-stu-id="d183d-196">View logs</span></span>

<span data-ttu-id="d183d-197">由於是使用 *systemd* 來管理使用 Kestrel 的 Web 應用程式，因此會將事件和處理序都記錄在集中式日誌中。</span><span class="sxs-lookup"><span data-stu-id="d183d-197">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="d183d-198">不過，此日誌包含 *systemd* 所管理全部服務和處理序的項目。</span><span class="sxs-lookup"><span data-stu-id="d183d-198">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="d183d-199">若要檢視 `kestrel-helloapp.service` 的特定項目，請使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="d183d-199">To view the `kestrel-helloapp.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service
```

<span data-ttu-id="d183d-200">如需進行時間篩選，請搭配命令指定時間選項。</span><span class="sxs-lookup"><span data-stu-id="d183d-200">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="d183d-201">例如，使用 `--since today` 來針對今天進行篩選，或使用 `--until 1 hour ago` 來查看前一個小時的項目。</span><span class="sxs-lookup"><span data-stu-id="d183d-201">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="d183d-202">如需詳細資訊，請參閱 [journalctl 的手冊頁面](https://www.unix.com/man-page/centos/1/journalctl/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="d183d-202">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a><span data-ttu-id="d183d-203">資料保護</span><span class="sxs-lookup"><span data-stu-id="d183d-203">Data protection</span></span>

<span data-ttu-id="d183d-204">[ASP.NET Core 資料保護堆疊](xref:security/data-protection/introduction)由數個 ASP.NET Core[ 中介軟體](xref:fundamentals/middleware/index)使用，包括驗證中介軟體 (例如，cookie 中介軟體) 與跨網站偽造要求 (CSRF) 保護。</span><span class="sxs-lookup"><span data-stu-id="d183d-204">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including authentication middleware (for example, cookie middleware) and cross-site request forgery (CSRF) protections.</span></span> <span data-ttu-id="d183d-205">即使資料保護 API 並非由使用者程式碼呼叫，仍應設定資料保護，以建立持續密碼編譯[金鑰存放區](xref:security/data-protection/implementation/key-management)。</span><span class="sxs-lookup"><span data-stu-id="d183d-205">Even if Data Protection APIs aren't called by user code, data protection should be configured to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="d183d-206">如不設定資料保護，金鑰會保留在記憶體中，並於應用程式重新啟動時捨棄。</span><span class="sxs-lookup"><span data-stu-id="d183d-206">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="d183d-207">如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：</span><span class="sxs-lookup"><span data-stu-id="d183d-207">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="d183d-208">所有以 Cookie 為基礎的驗證權杖都會失效。</span><span class="sxs-lookup"><span data-stu-id="d183d-208">All cookie-based authentication tokens are invalidated.</span></span>
* <span data-ttu-id="d183d-209">當使用者提出下一個要求時，需要再次登入。</span><span class="sxs-lookup"><span data-stu-id="d183d-209">Users are required to sign in again on their next request.</span></span>
* <span data-ttu-id="d183d-210">所有以 Keyring 保護的資料都無法再解密。</span><span class="sxs-lookup"><span data-stu-id="d183d-210">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="d183d-211">這可能會包含 [CSRF 權杖](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)和 [ASP.NET Core MVC TempData cookie](xref:fundamentals/app-state#tempdata)。</span><span class="sxs-lookup"><span data-stu-id="d183d-211">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="d183d-212">若要設定資料保護來保存及加密金鑰環，請參閱：</span><span class="sxs-lookup"><span data-stu-id="d183d-212">To configure data protection to persist and encrypt the key ring, see:</span></span>

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="secure-the-app"></a><span data-ttu-id="d183d-213">保護應用程式</span><span class="sxs-lookup"><span data-stu-id="d183d-213">Secure the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="d183d-214">設定防火牆</span><span class="sxs-lookup"><span data-stu-id="d183d-214">Configure firewall</span></span>

<span data-ttu-id="d183d-215">*Firewalld* 是一個可管理防火牆並支援網路區域的動態精靈。</span><span class="sxs-lookup"><span data-stu-id="d183d-215">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="d183d-216">您仍然可以使用 iptables 來管理連接埠和封包篩選。</span><span class="sxs-lookup"><span data-stu-id="d183d-216">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="d183d-217">預設應該安裝 *Firewalld*。</span><span class="sxs-lookup"><span data-stu-id="d183d-217">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="d183d-218">您可以使用 `yum` 來安裝套件或確認是否已安裝套件。</span><span class="sxs-lookup"><span data-stu-id="d183d-218">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="d183d-219">請使用 `firewalld` 來僅開啟應用程式所需的連接埠。</span><span class="sxs-lookup"><span data-stu-id="d183d-219">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="d183d-220">在此情況下，將使用連接埠 80 和 443。</span><span class="sxs-lookup"><span data-stu-id="d183d-220">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="d183d-221">下列命令會將連接埠 80 和 443 永久設定為開啟：</span><span class="sxs-lookup"><span data-stu-id="d183d-221">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="d183d-222">重新載入防火牆設定。</span><span class="sxs-lookup"><span data-stu-id="d183d-222">Reload the firewall settings.</span></span> <span data-ttu-id="d183d-223">檢查預設區域中可用的服務和連接埠。</span><span class="sxs-lookup"><span data-stu-id="d183d-223">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="d183d-224">您可以檢查 `firewall-cmd -h` 來取得選項。</span><span class="sxs-lookup"><span data-stu-id="d183d-224">Options are available by inspecting `firewall-cmd -h`.</span></span>

```bash
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

```bash
public (default, active)
interfaces: eth0
sources: 
services: dhcpv6-client
ports: 443/tcp 80/tcp
masquerade: no
forward-ports: 
icmp-blocks: 
rich rules: 
```

### <a name="https-configuration"></a><span data-ttu-id="d183d-225">HTTPS 設定</span><span class="sxs-lookup"><span data-stu-id="d183d-225">HTTPS configuration</span></span>

<span data-ttu-id="d183d-226">**設定應用程式以進行安全的本機連線 (HTTPS)**</span><span class="sxs-lookup"><span data-stu-id="d183d-226">**Configure the app for secure (HTTPS) local connections**</span></span>

<span data-ttu-id="d183d-227">[dotnet run](/dotnet/core/tools/dotnet-run) 命令使用應用程式的 *Properties/launchSettings.json* 檔案，其設定應用程式在 `applicationUrl` 屬性所提供的 URL 上接聽 (例如 `https://localhost:5001; http://localhost:5000`)。</span><span class="sxs-lookup"><span data-stu-id="d183d-227">The [dotnet run](/dotnet/core/tools/dotnet-run) command uses the app's *Properties/launchSettings.json* file, which configures the app to listen on the URLs provided by the `applicationUrl` property (for example, `https://localhost:5001;http://localhost:5000`).</span></span>

<span data-ttu-id="d183d-228">使用下列其中一種方法，設定應用程式將憑證用在針對 `dotnet run` 命令的開發，或用在開發環境 (F5，若在 Visual Studio Code 中則為 Ctrl+F5)：</span><span class="sxs-lookup"><span data-stu-id="d183d-228">Configure the app to use a certificate in development for the `dotnet run` command or development environment (F5 or Ctrl+F5 in Visual Studio Code) using one of the following approaches:</span></span>

* <span data-ttu-id="d183d-229">[取代組態中的預設憑證](xref:fundamentals/servers/kestrel#configuration) (建議使用)</span><span class="sxs-lookup"><span data-stu-id="d183d-229">[Replace the default certificate from configuration](xref:fundamentals/servers/kestrel#configuration) (*Recommended*)</span></span>
* [<span data-ttu-id="d183d-230">KestrelServerOptions.ConfigureHttpsDefaults</span><span class="sxs-lookup"><span data-stu-id="d183d-230">KestrelServerOptions.ConfigureHttpsDefaults</span></span>](xref:fundamentals/servers/kestrel#configurehttpsdefaultsactionhttpsconnectionadapteroptions)

<span data-ttu-id="d183d-231">**設定反向 Prooxy 以進行安全的用戶端連線 (HTTPS)**</span><span class="sxs-lookup"><span data-stu-id="d183d-231">**Configure the reverse proxy for secure (HTTPS) client connections**</span></span>

<span data-ttu-id="d183d-232">為了設定適用於 HTTPS 的 Apache，會使用 *mod_ssl* 模組。</span><span class="sxs-lookup"><span data-stu-id="d183d-232">To configure Apache for HTTPS, the *mod_ssl* module is used.</span></span> <span data-ttu-id="d183d-233">安裝 *httpd* 模組時，已一併安裝 *mod_ssl* 模組。</span><span class="sxs-lookup"><span data-stu-id="d183d-233">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="d183d-234">如果未安裝該模組，請使用 `yum` 將它新增到設定中。</span><span class="sxs-lookup"><span data-stu-id="d183d-234">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```

<span data-ttu-id="d183d-235">若要強制執行 HTTPS，請安裝 `mod_rewrite` 模組來啟用 URL 重寫：</span><span class="sxs-lookup"><span data-stu-id="d183d-235">To enforce HTTPS, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="d183d-236">修改 *helloapp.conf* 檔案以啟用 URL 重寫並保護連接埠 443 上的通訊：</span><span class="sxs-lookup"><span data-stu-id="d183d-236">Modify the *helloapp.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/helloapp-error.log
    CustomLog /var/log/httpd/helloapp-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

> [!NOTE]
> <span data-ttu-id="d183d-237">此範例使用本機產生的憑證。</span><span class="sxs-lookup"><span data-stu-id="d183d-237">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="d183d-238">**SSLCertificateFile** 應該是網域名稱的主要憑證檔案。</span><span class="sxs-lookup"><span data-stu-id="d183d-238">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="d183d-239">**SSLCertificateKeyFile** 應該是建立 CSR 時產生的金鑰檔。</span><span class="sxs-lookup"><span data-stu-id="d183d-239">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="d183d-240">**SSLCertificateChainFile** 應該是憑證授權單位所提供的中繼憑證檔案 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="d183d-240">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="d183d-241">儲存檔案並測試設定：</span><span class="sxs-lookup"><span data-stu-id="d183d-241">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="d183d-242">重新啟動 Apache：</span><span class="sxs-lookup"><span data-stu-id="d183d-242">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="d183d-243">Apache 的其他建議</span><span class="sxs-lookup"><span data-stu-id="d183d-243">Additional Apache suggestions</span></span>

### <a name="restart-apps-with-shared-framework-updates"></a><span data-ttu-id="d183d-244">使用共用架構更新重新開機應用程式</span><span class="sxs-lookup"><span data-stu-id="d183d-244">Restart apps with shared framework updates</span></span>

<span data-ttu-id="d183d-245">升級伺服器上的共用架構之後，請重新開機伺服器所主控的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d183d-245">After upgrading the shared framework on the server, restart the ASP.NET Core apps hosted by the server.</span></span>

### <a name="additional-headers"></a><span data-ttu-id="d183d-246">其他標頭</span><span class="sxs-lookup"><span data-stu-id="d183d-246">Additional headers</span></span>

<span data-ttu-id="d183d-247">為了防範惡意攻擊，應該要修改或新增一些標頭。</span><span class="sxs-lookup"><span data-stu-id="d183d-247">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="d183d-248">請確認已安裝 `mod_headers` 模組：</span><span class="sxs-lookup"><span data-stu-id="d183d-248">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="d183d-249">保護 Apache 免於點閱綁架攻擊</span><span class="sxs-lookup"><span data-stu-id="d183d-249">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="d183d-250">[點閱綁架](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger)(也稱為「UI 偽裝攻擊」) 是一種惡意攻擊，會誘騙網站訪客點選與其目前所瀏覽頁面不同的頁面上連結或按鈕。</span><span class="sxs-lookup"><span data-stu-id="d183d-250">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="d183d-251">請使用 `X-FRAME-OPTIONS` 來保護網站安全。</span><span class="sxs-lookup"><span data-stu-id="d183d-251">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="d183d-252">減輕點擊劫持攻擊：</span><span class="sxs-lookup"><span data-stu-id="d183d-252">To mitigate clickjacking attacks:</span></span>

1. <span data-ttu-id="d183d-253">編輯 *httpd.conf* 檔案：</span><span class="sxs-lookup"><span data-stu-id="d183d-253">Edit the *httpd.conf* file:</span></span>

   ```bash
   sudo nano /etc/httpd/conf/httpd.conf
   ```

   <span data-ttu-id="d183d-254">新增 `Header append X-FRAME-OPTIONS "SAMEORIGIN"` 行。</span><span class="sxs-lookup"><span data-stu-id="d183d-254">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span>
1. <span data-ttu-id="d183d-255">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="d183d-255">Save the file.</span></span>
1. <span data-ttu-id="d183d-256">重新啟動 Apache。</span><span class="sxs-lookup"><span data-stu-id="d183d-256">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="d183d-257">MIME 類型探查</span><span class="sxs-lookup"><span data-stu-id="d183d-257">MIME-type sniffing</span></span>

<span data-ttu-id="d183d-258">`X-Content-Type-Options` 標頭可防止 Internet Explorer 執行「MIME 探查」 (從檔案的內容判斷檔案的 `Content-Type`)。</span><span class="sxs-lookup"><span data-stu-id="d183d-258">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="d183d-259">如果伺服器將 `Content-Type` 標頭設定為 `text/html` 並搭配設定 `nosniff` 選項，則不論檔案內容為何，Internet Explorer 都會將該內容轉譯為 `text/html`。</span><span class="sxs-lookup"><span data-stu-id="d183d-259">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="d183d-260">編輯 *httpd.conf* 檔案：</span><span class="sxs-lookup"><span data-stu-id="d183d-260">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="d183d-261">新增 `Header set X-Content-Type-Options "nosniff"` 行。</span><span class="sxs-lookup"><span data-stu-id="d183d-261">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="d183d-262">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="d183d-262">Save the file.</span></span> <span data-ttu-id="d183d-263">重新啟動 Apache。</span><span class="sxs-lookup"><span data-stu-id="d183d-263">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="d183d-264">負載平衡</span><span class="sxs-lookup"><span data-stu-id="d183d-264">Load Balancing</span></span>

<span data-ttu-id="d183d-265">這個範例示範如何在 CentOS 7 上安裝和設定 Apache，以及如何在相同的執行個體電腦上安裝和設定 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="d183d-265">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="d183d-266">為了避免產生單一失敗點的情況，使用 *mod_proxy_balancer* 並修改 **VirtualHost**將可允許管理位於 Apache Proxy 伺服器後方的多個 Web 應用程式執行個體。</span><span class="sxs-lookup"><span data-stu-id="d183d-266">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="d183d-267">在以下所示的設定檔中，已將一個額外的 `helloapp` 執行個體設定在連接埠 5001 上執行。</span><span class="sxs-lookup"><span data-stu-id="d183d-267">In the configuration file shown below, an additional instance of the `helloapp` is set up to run on port 5001.</span></span> <span data-ttu-id="d183d-268">*Proxy* 區段中設定了平衡器設定，其中有兩個成員為 *byrequests* 進行負載平衡。</span><span class="sxs-lookup"><span data-stu-id="d183d-268">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPass / balancer://mycluster/ 

    ProxyPassReverse / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5001/

    <Proxy balancer://mycluster>
        BalancerMember http://127.0.0.1:5000
        BalancerMember http://127.0.0.1:5001 
        ProxySet lbmethod=byrequests
    </Proxy>

    <Location />
        SetHandler balancer
    </Location>
    ErrorLog /var/log/httpd/helloapp-error.log
    CustomLog /var/log/httpd/helloapp-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

### <a name="rate-limits"></a><span data-ttu-id="d183d-269">速率限制</span><span class="sxs-lookup"><span data-stu-id="d183d-269">Rate Limits</span></span>

<span data-ttu-id="d183d-270">使用 *mod_ratelimit* (包含在 *httpd* 模組中) 時，可以限制用戶端的頻寬：</span><span class="sxs-lookup"><span data-stu-id="d183d-270">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```

<span data-ttu-id="d183d-271">此範例檔案將根目錄位置下的頻寬限制為 600 KB/秒：</span><span class="sxs-lookup"><span data-stu-id="d183d-271">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

### <a name="long-request-header-fields"></a><span data-ttu-id="d183d-272">要求標頭欄位太長</span><span class="sxs-lookup"><span data-stu-id="d183d-272">Long request header fields</span></span>

<span data-ttu-id="d183d-273">Proxy 伺服器預設設定通常會將要求標頭欄位限制為8190個位元組。</span><span class="sxs-lookup"><span data-stu-id="d183d-273">Proxy server default settings typically limit request header fields to 8,190 bytes.</span></span> <span data-ttu-id="d183d-274">應用程式可能需要比預設值更長的欄位（例如，使用[Azure Active Directory](https://azure.microsoft.com/services/active-directory/)的應用程式）。</span><span class="sxs-lookup"><span data-stu-id="d183d-274">An app may require fields longer than the default (for example, apps that use [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)).</span></span> <span data-ttu-id="d183d-275">如果需要較長的欄位，proxy 伺服器的[LimitRequestFieldSize](https://httpd.apache.org/docs/2.4/mod/core.html#LimitRequestFieldSize)指示詞需要調整。</span><span class="sxs-lookup"><span data-stu-id="d183d-275">If longer fields are required, the proxy server's [LimitRequestFieldSize](https://httpd.apache.org/docs/2.4/mod/core.html#LimitRequestFieldSize) directive requires adjustment.</span></span> <span data-ttu-id="d183d-276">要套用的值取決於案例。</span><span class="sxs-lookup"><span data-stu-id="d183d-276">The value to apply depends on the scenario.</span></span> <span data-ttu-id="d183d-277">如需詳細資訊，請參閱您的伺服器文件。</span><span class="sxs-lookup"><span data-stu-id="d183d-277">For more information, see your server's documentation.</span></span>

> [!WARNING]
> <span data-ttu-id="d183d-278">除非必要，否則請勿增加 `LimitRequestFieldSize` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="d183d-278">Don't increase the default value of `LimitRequestFieldSize` unless necessary.</span></span> <span data-ttu-id="d183d-279">增加此值會提高緩衝區溢位及惡意使用者發動拒絕服務 (DoS) 攻擊的風險。</span><span class="sxs-lookup"><span data-stu-id="d183d-279">Increasing the value increases the risk of buffer overrun (overflow) and Denial of Service (DoS) attacks by malicious users.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d183d-280">其他資源</span><span class="sxs-lookup"><span data-stu-id="d183d-280">Additional resources</span></span>

* [<span data-ttu-id="d183d-281">Linux 上 .NET Core 的必要條件</span><span class="sxs-lookup"><span data-stu-id="d183d-281">Prerequisites for .NET Core on Linux</span></span>](/dotnet/core/linux-prerequisites)
* <xref:test/troubleshoot>
* <xref:host-and-deploy/proxy-load-balancer>
