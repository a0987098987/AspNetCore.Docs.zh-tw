---
title: 在 Linux 上使用 Nginx 裝載 ASP.NET Core
author: rick-anderson
description: 描述如何在 Ubuntu 16.04 上將 Nginx 設定為反向 Proxy，以將 HTTP 流量轉送至在 Kestrel 上執行的 ASP.NET Core Web 應用程式。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: d37aa25c712d715aa4134587a84e5923f9cb5b79
ms.sourcegitcommit: 50d40c83fa641d283c097f986dde5341ebe1b44c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/22/2018
ms.locfileid: "34452551"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="b7ac3-103">在 Linux 上使用 Nginx 裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b7ac3-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="b7ac3-104">作者：[Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="b7ac3-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="b7ac3-105">本指南說明在 Ubuntu 16.04 伺服器上設定生產環境就緒的 ASP.NET Core 環境。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

> [!NOTE]
> <span data-ttu-id="b7ac3-106">針對 Ubuntu 14.04，建議使用 *supervisord*作為監視 Kestrel 處理序的解決方案。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-106">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="b7ac3-107">在 Ubuntu 14.04 上無法使用 *systemd*。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-107">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="b7ac3-108">[請參閱本文件的舊版本](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-108">[See previous version of this document](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="b7ac3-109">本指南會：</span><span class="sxs-lookup"><span data-stu-id="b7ac3-109">This guide:</span></span>

* <span data-ttu-id="b7ac3-110">將現有的 ASP.NET Core 應用程式放在反向 Proxy 伺服器後方。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-110">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="b7ac3-111">設定反向 Proxy 伺服器以將要求轉送至 Kestrel 網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-111">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="b7ac3-112">確保 Web 應用程式在啟動時以精靈的形式執行。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-112">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="b7ac3-113">設定程序管理工具以協助重新啟動 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-113">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b7ac3-114">必要條件</span><span class="sxs-lookup"><span data-stu-id="b7ac3-114">Prerequisites</span></span>

1. <span data-ttu-id="b7ac3-115">以 sudo 權限使用標準使用者帳戶存取 Ubuntu 16.04 伺服器</span><span class="sxs-lookup"><span data-stu-id="b7ac3-115">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
1. <span data-ttu-id="b7ac3-116">現有的 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="b7ac3-116">An existing ASP.NET Core app</span></span>

## <a name="copy-over-the-app"></a><span data-ttu-id="b7ac3-117">將應用程式複製過去</span><span class="sxs-lookup"><span data-stu-id="b7ac3-117">Copy over the app</span></span>

<span data-ttu-id="b7ac3-118">從開發環境執行 [dotnet publish](/dotnet/core/tools/dotnet-publish)，以將應用程式封裝至可在伺服器上執行的自封式目錄中。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-118">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="b7ac3-119">使用任何工具 (SCP、FTP 等等) 將 ASP.NET Core 應用程式複製到伺服器，以整合至組織的工作流程。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-119">Copy the ASP.NET Core app to the server using whatever tool integrates into the organization's workflow (for example, SCP, FTP).</span></span> <span data-ttu-id="b7ac3-120">測試應用程式，例如：</span><span class="sxs-lookup"><span data-stu-id="b7ac3-120">Test the app, for example:</span></span>

* <span data-ttu-id="b7ac3-121">從命令列執行 `dotnet <app_assembly>.dll`。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-121">From the command line, run `dotnet <app_assembly>.dll`.</span></span>
* <span data-ttu-id="b7ac3-122">在瀏覽器中，巡覽至 `http://<serveraddress>:<port>` 以確認應用程式可在 Linux 上正常運作。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-122">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="b7ac3-123">設定反向 Proxy 伺服器</span><span class="sxs-lookup"><span data-stu-id="b7ac3-123">Configure a reverse proxy server</span></span>

<span data-ttu-id="b7ac3-124">反向 Proxy 是為動態 Web 應用程式提供服務的常見設定。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-124">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="b7ac3-125">反向 Proxy 會終止 HTTP 要求，並將它轉送至 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-125">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="b7ac3-126">為何使用反向 Proxy 伺服器？</span><span class="sxs-lookup"><span data-stu-id="b7ac3-126">Why use a reverse proxy server?</span></span>

<span data-ttu-id="b7ac3-127">Kestrel 非常適用於從 ASP.NET Core 提供動態內容。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-127">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="b7ac3-128">不過，Web 服務功能不像 IIS、Apache 或 Nginx 這類伺服器那樣豐富。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-128">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="b7ac3-129">反向 Proxy 伺服器可以讓 HTTP 伺服器卸下提供靜態內容、快取要求、壓縮要求及終止 SSL 等工作的負擔。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-129">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="b7ac3-130">反向 Proxy 伺服器可能位在專用電腦上，或可能與 HTTP 伺服器一起部署。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-130">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="b7ac3-131">為達到本指南的目的，使用 Nginx 的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-131">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="b7ac3-132">它會在相同的伺服器上和 HTTP 伺服器一起執行。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-132">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="b7ac3-133">您可以根據需求，選擇不同的設定。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-133">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="b7ac3-134">由於反向 Proxy 會轉送要求，因此請使用來自 [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) 套件的「轉送的標頭中介軟體」。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-134">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="b7ac3-135">此中介軟體會使用 `X-Forwarded-Proto` 標頭來更新 `Request.Scheme`，以便讓重新導向 URI 及其他安全性原則正確運作。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-135">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="b7ac3-136">有使用任何類型的驗證中介軟體時，必須先執行「轉送的標頭中介軟體」。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-136">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="b7ac3-137">此排序可確保驗證中介軟體能夠取用標頭值，然後產生正確的重新導向 URI。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-137">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b7ac3-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b7ac3-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b7ac3-139">請先在 `Startup.Configure` 中叫用 [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) 方法，再呼叫 [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) 或類似的驗證配置中介軟體。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-139">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="b7ac3-140">請設定中介軟體來轉送 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭：</span><span class="sxs-lookup"><span data-stu-id="b7ac3-140">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b7ac3-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b7ac3-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b7ac3-142">請先在 `Startup.Configure` 中叫用 [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) 方法，再呼叫 [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) 和 [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) 或類似的驗證配置中介軟體。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-142">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="b7ac3-143">請設定中介軟體來轉送 `X-Forwarded-For` 和 `X-Forwarded-Proto` 標頭：</span><span class="sxs-lookup"><span data-stu-id="b7ac3-143">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="b7ac3-144">如果未將任何 [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) 指定給中介軟體，則要轉送的預設標頭會是 `None`。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-144">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="b7ac3-145">Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-145">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="b7ac3-146">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-146">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-nginx"></a><span data-ttu-id="b7ac3-147">安裝 Nginx</span><span class="sxs-lookup"><span data-stu-id="b7ac3-147">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="b7ac3-148">如果將安裝選用的 Nginx 模組，可能會需要從來源建置 Nginx。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-148">If optional Nginx modules will be installed, building Nginx from source might be required.</span></span>

<span data-ttu-id="b7ac3-149">使用 `apt-get` 來安裝 Nginx。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-149">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="b7ac3-150">安裝程式建立的系統 V init 初始化指令碼，會在系統啟動時將 Nginx 執行為精靈。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-150">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="b7ac3-151">因為 Nginx 是第一次安裝，請透過執行明確啟動它：</span><span class="sxs-lookup"><span data-stu-id="b7ac3-151">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="b7ac3-152">確認瀏覽器會顯示 Nginx 的預設登陸頁面。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-152">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="b7ac3-153">設定 Nginx</span><span class="sxs-lookup"><span data-stu-id="b7ac3-153">Configure Nginx</span></span>

<span data-ttu-id="b7ac3-154">若要將 Nginx 設定為反向 Proxy 以將要求轉送至您的 ASP.NET Core 應用程式，請修改 */etc/nginx/sites-available/default*。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-154">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="b7ac3-155">以文字編輯器開啟它，並以下列項目取代內容：</span><span class="sxs-lookup"><span data-stu-id="b7ac3-155">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

<span data-ttu-id="b7ac3-156">當沒有任何與 `server_name` 相符的項目時，Nginx 會使用預設伺服器。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-156">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="b7ac3-157">如果未定義任何預設伺服器，則設定檔中的第一個伺服器就是預設伺服器。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-157">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="b7ac3-158">最佳做法是，在您的設定檔中新增一個會傳回狀態碼 444 的特定預設伺服器。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-158">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="b7ac3-159">預設伺服器設定範例如下：</span><span class="sxs-lookup"><span data-stu-id="b7ac3-159">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="b7ac3-160">使用上述設定檔和預設伺服器時，Nginx 會在連接埠 80 接受主機標頭為 `example.com` 或 `*.example.com` 的公用流量。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-160">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="b7ac3-161">不符合這些主機的要求將不會轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-161">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="b7ac3-162">Nginx 會將相符的要求轉送至位於 `http://localhost:5000` 的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-162">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="b7ac3-163">如需詳細資訊，請參閱 [nginx 如何處理要求](https://nginx.org/docs/http/request_processing.html) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-163">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span>

> [!WARNING]
> <span data-ttu-id="b7ac3-164">如果無法指定適當的 [server_name 指示詞](https://nginx.org/docs/http/server_names.html)，就會讓應用程式暴露在安全性弱點的風險下。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-164">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="b7ac3-165">若您擁有整個父網域 (相對於易受攻擊的 `*.com`) 的控制權，子網域萬用字元繫結 (例如 `*.example.com`) 就沒有此安全性風險。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-165">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="b7ac3-166">如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-166">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="b7ac3-167">建立 Nginx 設定之後，請執行 `sudo nginx -t` 來確認設定檔的語法。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-167">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="b7ac3-168">如果設定檔測試成功，請執行 `sudo nginx -s reload` 來強制 Nginx 套用這些變更。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-168">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="b7ac3-169">監視應用程式</span><span class="sxs-lookup"><span data-stu-id="b7ac3-169">Monitoring the app</span></span>

<span data-ttu-id="b7ac3-170">伺服器已設定完成，可將對 `http://<serveraddress>:80` 發出的要求轉送給在位於 `http://127.0.0.1:5000` 的 Kestrel 上執行的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-170">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="b7ac3-171">不過，並未設定 Nginx 來管理 Kestrel 處理序。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-171">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="b7ac3-172">您可以使用 *systemd* 來建立服務檔案，以啟動並監視基礎 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-172">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="b7ac3-173">*systemd* 是 init 系統，提供許多強大的啟動、停止和管理處理程序功能。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-173">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="b7ac3-174">建立服務檔</span><span class="sxs-lookup"><span data-stu-id="b7ac3-174">Create the service file</span></span>

<span data-ttu-id="b7ac3-175">建立服務定義檔：</span><span class="sxs-lookup"><span data-stu-id="b7ac3-175">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="b7ac3-176">以下是一個應用程式範例服務檔：</span><span class="sxs-lookup"><span data-stu-id="b7ac3-176">The following is an example service file for the app:</span></span>

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
RestartSec=10  # Restart service after 10 seconds if dotnet service crashes
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="b7ac3-177">如果設定不是使用 *www-data* 這個使用者，就必須先建立這裡所定義的使用者，並授與適當的檔案擁有權。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-177">If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="b7ac3-178">Linux 的檔案系統會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-178">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="b7ac3-179">將 ASPNETCORE_ENVIRONMENT 設定為 "Production" 會導致搜尋 *appsettings.Production.json* 設定檔，而不是搜尋 *appsettings.production.json*。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-179">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="b7ac3-180">有些值 (例如 SQL 連接字串) 必須以逸出方式處理，設定提供者才能讀取環境變數。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-180">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="b7ac3-181">請使用下列命令來產生要在設定檔中使用並已適當逸出的值：</span><span class="sxs-lookup"><span data-stu-id="b7ac3-181">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="b7ac3-182">儲存檔案並啟用服務。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-182">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="b7ac3-183">啟動服務並確認它正在執行。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-183">Start the service and verify that it's running.</span></span>

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="b7ac3-184">設定好反向 Proxy 並透過 systemd 管理 Kestrel 之後，Web 應用程式便已完全設定妥當，而從本機電腦瀏覽器透過 `http://localhost` 即可存取它。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-184">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="b7ac3-185">此外，也可以從遠端電腦存取它，除非遭到任何防火牆封鎖。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-185">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="b7ac3-186">檢查回應標頭時，`Server` 標頭會顯示是由 Kestrel 為 ASP.NET Core 應用程式提供服務。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-186">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="b7ac3-187">檢視記錄</span><span class="sxs-lookup"><span data-stu-id="b7ac3-187">Viewing logs</span></span>

<span data-ttu-id="b7ac3-188">由於是使用 `systemd` 來管理使用 Kestrel 的 Web 應用程式，因此會將所有事件和處理序都記錄在集中式日誌中。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-188">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="b7ac3-189">不過，此日誌包含 `systemd` 管理的所有服務和處理程序的所有項目。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-189">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="b7ac3-190">若要檢視 `kestrel-hellomvc.service` 的特定項目，請使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="b7ac3-190">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="b7ac3-191">如需進一步篩選，例如 `--since today`、`--until 1 hour ago` 或這些項目的組合等時間選項，可以減少傳回的項目數量。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-191">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="b7ac3-192">確保應用程式的安全性</span><span class="sxs-lookup"><span data-stu-id="b7ac3-192">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="b7ac3-193">啟用 AppArmor</span><span class="sxs-lookup"><span data-stu-id="b7ac3-193">Enable AppArmor</span></span>

<span data-ttu-id="b7ac3-194">Linux 安全性模組 (LSM) 是 Linux 2.6 之後 Linux 核心所包含的一個架構。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-194">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="b7ac3-195">LSM 支援不同的安全性模組實作。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-195">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="b7ac3-196">[AppArmor](https://wiki.ubuntu.com/AppArmor) 是 LSM，它實作的必要存取控制系統，可將程式限定在有限的資源集中。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-196">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="b7ac3-197">確定已啟用並正確設定 AppArmor。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-197">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="b7ac3-198">設定防火牆</span><span class="sxs-lookup"><span data-stu-id="b7ac3-198">Configuring the firewall</span></span>

<span data-ttu-id="b7ac3-199">關閉所有不在使用中的外部連接埠。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-199">Close off all external ports that are not in use.</span></span> <span data-ttu-id="b7ac3-200">簡單的防火牆 (ufw) 提供命令列介面供設定防火牆，為 `iptables` 提供前端。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-200">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="b7ac3-201">確認已設定 `ufw` 來允許所需任何連接埠上的流量。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-201">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="b7ac3-202">保護 Nginx</span><span class="sxs-lookup"><span data-stu-id="b7ac3-202">Securing Nginx</span></span>

<span data-ttu-id="b7ac3-203">Nginx 的預設分佈不會啟用 SSL。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-203">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="b7ac3-204">為啟用其他的安全性功能，請從來源建置。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-204">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="b7ac3-205">下載來源並安裝組建相依性</span><span class="sxs-lookup"><span data-stu-id="b7ac3-205">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download Nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="b7ac3-206">變更 Nginx 回應名稱</span><span class="sxs-lookup"><span data-stu-id="b7ac3-206">Change the Nginx response name</span></span>

<span data-ttu-id="b7ac3-207">編輯 *src/http/ngx_http_header_filter_module.c*：</span><span class="sxs-lookup"><span data-stu-id="b7ac3-207">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="b7ac3-208">設定選項和組建</span><span class="sxs-lookup"><span data-stu-id="b7ac3-208">Configure the options and build</span></span>

<span data-ttu-id="b7ac3-209">規則運算式需要 PCRE 程式庫。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-209">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="b7ac3-210">規則運算式用於 ngx_http_rewrite_module 的位置指示詞。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-210">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="b7ac3-211">http_ssl_module 新增 HTTPS 通訊協定的支援。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-211">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="b7ac3-212">請考慮使用像 *ModSecurity* 這樣的 Web 應用程式防火牆來強化應用程式。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-212">Consider using a web app firewall like *ModSecurity* to harden the app.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="b7ac3-213">設定 SSL</span><span class="sxs-lookup"><span data-stu-id="b7ac3-213">Configure SSL</span></span>

* <span data-ttu-id="b7ac3-214">藉由指定由受信任憑證授權單位 (CA) 核發的有效憑證，將伺服器設定成在連接埠 `443` 上接聽 HTTPS 流量。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-214">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="b7ac3-215">採用以下 */etc/nginx/nginx.conf* 檔案所述的一些做法來強化安全性。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-215">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="b7ac3-216">範例包括選擇更強的加密，重新導向 HTTPS 到 HTTP 的所有流量。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-216">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="b7ac3-217">新增 `HTTP Strict-Transport-Security` (HSTS) 標頭可確保用戶端提出的所有後續要求都只會透過 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-217">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="b7ac3-218">如果未來將會停用 SSL，則不要新增 Strict-Transport-Security 標頭，或選擇適當的 `max-age`。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-218">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="b7ac3-219">新增 */etc/nginx/Proxy.conf* 組態檔：</span><span class="sxs-lookup"><span data-stu-id="b7ac3-219">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="b7ac3-220">編輯 */etc/nginx/Proxy.conf* 組態檔。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-220">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="b7ac3-221">此範例在一個組態檔中同時包含 `http` 和 `server` 區段。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-221">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="b7ac3-222">保護 Nginx 免於點閱綁架</span><span class="sxs-lookup"><span data-stu-id="b7ac3-222">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="b7ac3-223">點閱綁架是收集受感染使用者按一下動作的惡意技術。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-223">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="b7ac3-224">點閱綁架誘騙受害者 (訪客) 到受感染的網站上執行按一下的動作。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-224">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="b7ac3-225">使用 X-FRAME-OPTIONS 來保護網站。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-225">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="b7ac3-226">編輯 *nginx.conf* 檔案：</span><span class="sxs-lookup"><span data-stu-id="b7ac3-226">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="b7ac3-227">新增行 `add_header X-Frame-Options "SAMEORIGIN";` 並儲存檔案，然後重新啟動 Nginx。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-227">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="b7ac3-228">MIME 類型探查</span><span class="sxs-lookup"><span data-stu-id="b7ac3-228">MIME-type sniffing</span></span>

<span data-ttu-id="b7ac3-229">此標頭會防止大部分的瀏覽器對遠離宣告內容類型的回應進行 MIME 探查，因為標頭會指示瀏覽器不要覆寫回應內容類型。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-229">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="b7ac3-230">使用 `nosniff` 選項，如果伺服器顯示的內容是 "text/html"，則瀏覽器就會將它轉譯為 "text/html"。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-230">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="b7ac3-231">編輯 *nginx.conf* 檔案：</span><span class="sxs-lookup"><span data-stu-id="b7ac3-231">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="b7ac3-232">新增 `add_header X-Content-Type-Options "nosniff";` 行並儲存檔案，然後重新啟動 Nginx。</span><span class="sxs-lookup"><span data-stu-id="b7ac3-232">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>
