---
title: 在 Linux 上使用 Apache 裝載 ASP.NET Core
description: 了解如何在 CentOS 上將 Apache 設定為反向 Proxy 伺服器，以將 HTTP 流量重新導向至在 Kestrel 上執行的 ASP.NET Core Web 應用程式。
author: spboyer
ms.author: spboyer
ms.custom: mvc
ms.date: 03/13/2018
uid: host-and-deploy/linux-apache
ms.openlocfilehash: d02fbd82be37e6d67214a9a0bf5851662b577cb9
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433970"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="2c3bd-103">在 Linux 上使用 Apache 裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2c3bd-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="2c3bd-104">作者：[Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="2c3bd-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="2c3bd-105">使用本指南來了解如何在 [CentOS 7](https://www.centos.org/) 上將 [Apache](https://httpd.apache.org/) 設定為反向 Proxy 伺服器，以將 HTTP 流量重新導向至在 [Kestrel](xref:fundamentals/servers/kestrel) 上執行的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="2c3bd-106">[mod_proxy 延伸模組](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html)和相關的模組會建立伺服器的反向 Proxy。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2c3bd-107">必要條件</span><span class="sxs-lookup"><span data-stu-id="2c3bd-107">Prerequisites</span></span>

1. <span data-ttu-id="2c3bd-108">執行 CentOS 7 的伺服器搭配具有 sudo 權限的標準使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-108">Server running CentOS 7 with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="2c3bd-109">在伺服器上安裝 .NET Core 執行階段。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-109">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="2c3bd-110">請前往 [.NET Core 的 All Downloads (下載區)](https://www.microsoft.com/net/download/all) 頁面。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-110">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="2c3bd-111">在 [執行階段] 下的清單中選取最新的非預覽執行階段。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-111">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="2c3bd-112">選取並遵循 CentOS/Oracle 的指示。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-112">Select and follow the instructions for CentOS/Oracle.</span></span>
1. <span data-ttu-id="2c3bd-113">現有的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-113">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="2c3bd-114">跨應用程式發佈與複製</span><span class="sxs-lookup"><span data-stu-id="2c3bd-114">Publish and copy over the app</span></span>

<span data-ttu-id="2c3bd-115">為[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-115">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="2c3bd-116">從開發環境執行 [dotnet publish](/dotnet/core/tools/dotnet-publish) 將應用程式封裝到可在伺服器上執行的目錄 (例如，*bin/Release/&lt;target_framework_moniker&gt;/publish*)：</span><span class="sxs-lookup"><span data-stu-id="2c3bd-116">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="2c3bd-117">如果您不想在伺服器上維護 .NET Core 執行階段，應用程式也可以發佈為[獨立式部署](/dotnet/core/deploying/#self-contained-deployments-scd)。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-117">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="2c3bd-118">使用整合至組織工作流程的工具 (SCP、SFTP 等等) 將 ASP.NET Core 應用程式複製到伺服器。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-118">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="2c3bd-119">Web 應用程式通常可在 *var* 目錄下找到 (例如，*var/aspnetcore/hellomvc*)。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-119">It's common to locate web apps under the *var* directory (for example, *var/aspnetcore/hellomvc*).</span></span>

> [!NOTE]
> <span data-ttu-id="2c3bd-120">在生產環境部署案例中，持續整合工作流程會執行發佈應用程式並將資產複製到伺服器的工作。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-120">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

## <a name="configure-a-proxy-server"></a><span data-ttu-id="2c3bd-121">設定 Proxy 伺服器</span><span class="sxs-lookup"><span data-stu-id="2c3bd-121">Configure a proxy server</span></span>

<span data-ttu-id="2c3bd-122">反向 Proxy 是為動態 Web 應用程式提供服務的常見設定。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-122">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="2c3bd-123">反向 Proxy 會終止 HTTP 要求，並將它轉送至 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-123">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="2c3bd-124">Proxy 伺服器則是會將用戶端要求轉送至另一部伺服器，而不是自己完成這些要求。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-124">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="2c3bd-125">反向 Proxy 會轉送至固定目的地，通常代表任意的用戶端。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-125">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="2c3bd-126">在本指南中，是將 Apache 設定成反向 Proxy，且執行所在的伺服器與 Kestrel 為 ASP.NET Core 應用程式提供服務的伺服器相同。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-126">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="2c3bd-127">由於反向 Proxy 會轉送要求，因此請使用來自 [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) 套件的[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-127">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="2c3bd-128">此中介軟體會使用 `X-Forwarded-Proto` 標頭來更新 `Request.Scheme`，以便讓重新導向 URI 及其他安全性原則正確運作。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-128">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="2c3bd-129">任何依賴配置的元件，例如驗證、連結產生、重新導向和地理位置，都必須在叫用轉送的標頭中介軟體後放置。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-129">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="2c3bd-130">轉送的標頭中介軟體是一般規則，應該先於診斷和錯誤處理中介軟體以外的其他中介軟體執行。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-130">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="2c3bd-131">這種排序可確保依賴轉送標頭資訊的中介軟體可以耗用用於處理的標頭值。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-131">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

::: moniker range=">= aspnetcore-2.0"
> [!NOTE]
> <span data-ttu-id="2c3bd-132">不論設定是否具有反向 Proxy 伺服器，對於 ASP.NET Core 2.0 或更新版本的應用程式，其中之一都是有效且支援的裝載設定。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-132">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="2c3bd-133">如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-133">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>
::: moniker-end

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2c3bd-134">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2c3bd-134">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2c3bd-135">請先在 `Startup.Configure` 中叫用 [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) 方法，再呼叫 [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) 或類似的驗證配置中介軟體。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-135">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="2c3bd-136">請設定中介軟體來轉送 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭：</span><span class="sxs-lookup"><span data-stu-id="2c3bd-136">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2c3bd-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2c3bd-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2c3bd-138">請先在 `Startup.Configure` 中叫用 [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) 方法，再呼叫 [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) 和 [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) 或類似的驗證配置中介軟體。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-138">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="2c3bd-139">請設定中介軟體來轉送 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭：</span><span class="sxs-lookup"><span data-stu-id="2c3bd-139">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="2c3bd-140">如果未將任何 [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) 指定給中介軟體，則要轉送的預設標頭會是 `None`。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-140">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="2c3bd-141">Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-141">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="2c3bd-142">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-142">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-apache"></a><span data-ttu-id="2c3bd-143">安裝 Apache</span><span class="sxs-lookup"><span data-stu-id="2c3bd-143">Install Apache</span></span>

<span data-ttu-id="2c3bd-144">將 CentOS 套件更新至其最新穩定版本：</span><span class="sxs-lookup"><span data-stu-id="2c3bd-144">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="2c3bd-145">使用單一 `yum` 命令在 CentOS 上安裝 Apache 網頁伺服器：</span><span class="sxs-lookup"><span data-stu-id="2c3bd-145">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="2c3bd-146">執行命令之後的範例輸出：</span><span class="sxs-lookup"><span data-stu-id="2c3bd-146">Sample output after running the command:</span></span>

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
> <span data-ttu-id="2c3bd-147">在此範例中，輸出會反映 httpd.86_64，因為 CentOS 第 7 版是 64 位元。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-147">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="2c3bd-148">若要確認 Apache 的安裝位置，請從命令提示字元執行 `whereis httpd`。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-148">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache"></a><span data-ttu-id="2c3bd-149">設定 Apache</span><span class="sxs-lookup"><span data-stu-id="2c3bd-149">Configure Apache</span></span>

<span data-ttu-id="2c3bd-150">Apache 的組態檔是位於 `/etc/httpd/conf.d/` 目錄內。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-150">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="2c3bd-151">除了 `/etc/httpd/conf.modules.d/` (包含載入模組所需的所有設定檔) 中的模組設定檔之外，任何副檔名為 *.conf* 的檔案也會依字母順序處理。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-151">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="2c3bd-152">為應用程式建立名為 *hellomvc.conf*的設定檔：</span><span class="sxs-lookup"><span data-stu-id="2c3bd-152">Create a configuration file, named *hellomvc.conf*, for the app:</span></span>

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
    ErrorLog ${APACHE_LOG_DIR}hellomvc-error.log
    CustomLog ${APACHE_LOG_DIR}hellomvc-access.log common
</VirtualHost>
```

<span data-ttu-id="2c3bd-153">`VirtualHost` 區塊可以在伺服器上的一或多個檔案中出現多次。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-153">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="2c3bd-154">在上述設定檔中，Apache 會在連接埠 80 接受公用流量。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-154">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="2c3bd-155">所服務的網域是 `www.example.com`，而 `*.example.com` 別名則會解析成同一個網站。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-155">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="2c3bd-156">如需詳細資訊，請參閱[名稱型虛擬主機支援](https://httpd.apache.org/docs/current/vhosts/name-based.html) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-156">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="2c3bd-157">要求會在根目錄透過 Proxy 傳送至位於 127.0.0.1 之伺服器的連接埠 5000。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-157">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="2c3bd-158">如需進行雙向通訊，則必須要有 `ProxyPass` 和 `ProxyPassReverse`。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-158">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span> <span data-ttu-id="2c3bd-159">若要變更 Kestrel 的 IP/連接埠，請參閱 [Kestrel：端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration)。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-159">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="2c3bd-160">如果無法在 **VirtualHost** 區塊中指定適當的 [ServerName 指示詞](https://httpd.apache.org/docs/current/mod/core.html#servername)，就會讓應用程式暴露在安全性弱點的風險下。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-160">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="2c3bd-161">若您擁有整個父網域 (相對於易受攻擊的 `*.com`) 的控制權，子網域萬用字元繫結 (例如 `*.example.com`) 就沒有此安全性風險。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-161">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="2c3bd-162">如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-162">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="2c3bd-163">您可以使用 `ErrorLog` 和 `CustomLog` 指示詞來依 `VirtualHost` 設定記錄功能。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-163">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="2c3bd-164">`ErrorLog` 是伺服器記錄錯誤的位置，而 `CustomLog` 則會設定記錄檔的檔案名稱和格式。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-164">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="2c3bd-165">在此案例中，這就是記錄要求資訊的位置。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-165">In this case, this is where request information is logged.</span></span> <span data-ttu-id="2c3bd-166">每個要求都會有一行。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-166">There's one line for each request.</span></span>

<span data-ttu-id="2c3bd-167">請儲存檔案並測試設定。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-167">Save the file and test the configuration.</span></span> <span data-ttu-id="2c3bd-168">如果所有項目都通過，回應應該是 `Syntax [OK]`。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-168">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="2c3bd-169">重新啟動 Apache：</span><span class="sxs-lookup"><span data-stu-id="2c3bd-169">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="2c3bd-170">監視應用程式</span><span class="sxs-lookup"><span data-stu-id="2c3bd-170">Monitoring the app</span></span>

<span data-ttu-id="2c3bd-171">Apache 現在已設定完成，可將對 `http://localhost:80` 發出的要求轉送給在位於 `http://127.0.0.1:5000` 的 Kestrel 上執行的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-171">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="2c3bd-172">不過，並未設定 Apache 來管理 Kestrel 處理序。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-172">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="2c3bd-173">請使用 *systemd* 並建立服務檔案，以啟動並監視基礎 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-173">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="2c3bd-174">*systemd* 是 init 系統，提供許多強大的啟動、停止和管理處理程序功能。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-174">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="2c3bd-175">建立服務檔</span><span class="sxs-lookup"><span data-stu-id="2c3bd-175">Create the service file</span></span>

<span data-ttu-id="2c3bd-176">建立服務定義檔：</span><span class="sxs-lookup"><span data-stu-id="2c3bd-176">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="2c3bd-177">應用程式的範例服務檔：</span><span class="sxs-lookup"><span data-stu-id="2c3bd-177">An example service file for the app:</span></span>

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

> [!NOTE]
> <span data-ttu-id="2c3bd-178">**User** &mdash; 如果設定不是使用 *apache* 這個使用者，就必須先建立使用者並授與適當的檔案擁有權。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-178">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

> [!NOTE]
> <span data-ttu-id="2c3bd-179">有些值 (例如 SQL 連接字串) 必須以逸出方式處理，設定提供者才能讀取環境變數。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-179">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="2c3bd-180">請使用下列命令來產生要在設定檔中使用並已適當逸出的值：</span><span class="sxs-lookup"><span data-stu-id="2c3bd-180">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="2c3bd-181">儲存檔案並啟用服務：</span><span class="sxs-lookup"><span data-stu-id="2c3bd-181">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="2c3bd-182">啟動服務並確認它正在執行：</span><span class="sxs-lookup"><span data-stu-id="2c3bd-182">Start the service and verify that it's running:</span></span>

```bash
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on CentOS 7
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="2c3bd-183">設定好反向 Proxy 並透過 *systemd* 管理 Kestrel 之後，Web 應用程式便已完全設定妥當，而從本機電腦瀏覽器透過 `http://localhost` 即可存取它。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-183">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="2c3bd-184">檢查回應標頭時，**Server** 標頭會指出是由 Kestrel 為 ASP.NET Core 應用程式提供服務：</span><span class="sxs-lookup"><span data-stu-id="2c3bd-184">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="2c3bd-185">檢視記錄</span><span class="sxs-lookup"><span data-stu-id="2c3bd-185">Viewing logs</span></span>

<span data-ttu-id="2c3bd-186">由於是使用 *systemd* 來管理使用 Kestrel 的 Web 應用程式，因此會將事件和處理序都記錄在集中式日誌中。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-186">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="2c3bd-187">不過，此日誌包含 *systemd* 所管理全部服務和處理序的項目。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-187">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="2c3bd-188">若要檢視 `kestrel-hellomvc.service` 的特定項目，請使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="2c3bd-188">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="2c3bd-189">如需進行時間篩選，請搭配命令指定時間選項。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-189">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="2c3bd-190">例如，使用 `--since today` 來針對今天進行篩選，或使用 `--until 1 hour ago` 來查看前一個小時的項目。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-190">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="2c3bd-191">如需詳細資訊，請參閱 [journalctl 的手冊頁面](https://www.unix.com/man-page/centos/1/journalctl/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-191">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="2c3bd-192">確保應用程式的安全性</span><span class="sxs-lookup"><span data-stu-id="2c3bd-192">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="2c3bd-193">設定防火牆</span><span class="sxs-lookup"><span data-stu-id="2c3bd-193">Configure firewall</span></span>

<span data-ttu-id="2c3bd-194">*Firewalld* 是一個可管理防火牆並支援網路區域的動態精靈。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-194">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="2c3bd-195">您仍然可以使用 iptables 來管理連接埠和封包篩選。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-195">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="2c3bd-196">預設應該安裝 *Firewalld*。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-196">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="2c3bd-197">您可以使用 `yum` 來安裝套件或確認是否已安裝套件。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-197">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="2c3bd-198">請使用 `firewalld` 來僅開啟應用程式所需的連接埠。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-198">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="2c3bd-199">在此情況下，將使用連接埠 80 和 443。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-199">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="2c3bd-200">下列命令會將連接埠 80 和 443 永久設定為開啟：</span><span class="sxs-lookup"><span data-stu-id="2c3bd-200">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="2c3bd-201">重新載入防火牆設定。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-201">Reload the firewall settings.</span></span> <span data-ttu-id="2c3bd-202">檢查預設區域中可用的服務和連接埠。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-202">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="2c3bd-203">您可以檢查 `firewall-cmd -h` 來取得選項。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-203">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="2c3bd-204">SSL 組態</span><span class="sxs-lookup"><span data-stu-id="2c3bd-204">SSL configuration</span></span>

<span data-ttu-id="2c3bd-205">為了設定適用於 SSL 的 Apache，會使用 *mod_ssl* 模組。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-205">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="2c3bd-206">安裝 *httpd* 模組時，已一併安裝 *mod_ssl* 模組。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-206">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="2c3bd-207">如果未安裝該模組，請使用 `yum` 將它新增到設定中。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-207">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```

<span data-ttu-id="2c3bd-208">若要強制執行 SSL，請安裝 `mod_rewrite` 模組來啟用 URL 重寫：</span><span class="sxs-lookup"><span data-stu-id="2c3bd-208">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="2c3bd-209">修改 *hellomvc.conf* 檔案以啟用 URL 重寫並保護連接埠 443 上的通訊：</span><span class="sxs-lookup"><span data-stu-id="2c3bd-209">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

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
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

> [!NOTE]
> <span data-ttu-id="2c3bd-210">此範例使用本機產生的憑證。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-210">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="2c3bd-211">**SSLCertificateFile** 應該是網域名稱的主要憑證檔案。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-211">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="2c3bd-212">**SSLCertificateKeyFile** 應該是建立 CSR 時產生的金鑰檔。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-212">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="2c3bd-213">**SSLCertificateChainFile** 應該是憑證授權單位所提供的中繼憑證檔案 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-213">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="2c3bd-214">儲存檔案並測試設定：</span><span class="sxs-lookup"><span data-stu-id="2c3bd-214">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="2c3bd-215">重新啟動 Apache：</span><span class="sxs-lookup"><span data-stu-id="2c3bd-215">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="2c3bd-216">Apache 的其他建議</span><span class="sxs-lookup"><span data-stu-id="2c3bd-216">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="2c3bd-217">其他標頭</span><span class="sxs-lookup"><span data-stu-id="2c3bd-217">Additional headers</span></span>

<span data-ttu-id="2c3bd-218">為了防範惡意攻擊，應該要修改或新增一些標頭。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-218">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="2c3bd-219">請確認已安裝 `mod_headers` 模組：</span><span class="sxs-lookup"><span data-stu-id="2c3bd-219">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="2c3bd-220">保護 Apache 免於點閱綁架攻擊</span><span class="sxs-lookup"><span data-stu-id="2c3bd-220">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="2c3bd-221">[點閱綁架](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger)(也稱為「UI 偽裝攻擊」) 是一種惡意攻擊，會誘騙網站訪客點選與其目前所瀏覽頁面不同的頁面上連結或按鈕。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-221">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="2c3bd-222">請使用 `X-FRAME-OPTIONS` 來保護網站安全。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-222">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="2c3bd-223">編輯 *httpd.conf* 檔案：</span><span class="sxs-lookup"><span data-stu-id="2c3bd-223">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="2c3bd-224">新增 `Header append X-FRAME-OPTIONS "SAMEORIGIN"` 行。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-224">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="2c3bd-225">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-225">Save the file.</span></span> <span data-ttu-id="2c3bd-226">重新啟動 Apache。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-226">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="2c3bd-227">MIME 類型探查</span><span class="sxs-lookup"><span data-stu-id="2c3bd-227">MIME-type sniffing</span></span>

<span data-ttu-id="2c3bd-228">`X-Content-Type-Options` 標頭可防止 Internet Explorer 執行「MIME 探查」 (從檔案的內容判斷檔案的 `Content-Type`)。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-228">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="2c3bd-229">如果伺服器將 `Content-Type` 標頭設定為 `text/html` 並搭配設定 `nosniff` 選項，則不論檔案內容為何，Internet Explorer 都會將該內容轉譯為 `text/html`。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-229">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="2c3bd-230">編輯 *httpd.conf* 檔案：</span><span class="sxs-lookup"><span data-stu-id="2c3bd-230">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="2c3bd-231">新增 `Header set X-Content-Type-Options "nosniff"` 行。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-231">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="2c3bd-232">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-232">Save the file.</span></span> <span data-ttu-id="2c3bd-233">重新啟動 Apache。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-233">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="2c3bd-234">負載平衡</span><span class="sxs-lookup"><span data-stu-id="2c3bd-234">Load Balancing</span></span>

<span data-ttu-id="2c3bd-235">這個範例示範如何在 CentOS 7 上安裝和設定 Apache，以及如何在相同的執行個體電腦上安裝和設定 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-235">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="2c3bd-236">為了避免產生單一失敗點的情況，使用 *mod_proxy_balancer* 並修改 **VirtualHost**將可允許管理位於 Apache Proxy 伺服器後方的多個 Web 應用程式執行個體。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-236">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="2c3bd-237">在以下所示的設定檔中，已將一個額外的 `hellomvc` 應用程式執行個體設定在連接埠 5001 上執行。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-237">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="2c3bd-238">*Proxy* 區段中設定了平衡器設定，其中有兩個成員為 *byrequests* 進行負載平衡。</span><span class="sxs-lookup"><span data-stu-id="2c3bd-238">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

### <a name="rate-limits"></a><span data-ttu-id="2c3bd-239">速率限制</span><span class="sxs-lookup"><span data-stu-id="2c3bd-239">Rate Limits</span></span>

<span data-ttu-id="2c3bd-240">使用 *mod_ratelimit* (包含在 *httpd* 模組中) 時，可以限制用戶端的頻寬：</span><span class="sxs-lookup"><span data-stu-id="2c3bd-240">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="2c3bd-241">此範例檔案將根目錄位置下的頻寬限制為 600 KB/秒：</span><span class="sxs-lookup"><span data-stu-id="2c3bd-241">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

## <a name="additional-resources"></a><span data-ttu-id="2c3bd-242">其他資源</span><span class="sxs-lookup"><span data-stu-id="2c3bd-242">Additional resources</span></span>

* [<span data-ttu-id="2c3bd-243">設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器</span><span class="sxs-lookup"><span data-stu-id="2c3bd-243">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
