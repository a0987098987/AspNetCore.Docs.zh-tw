---
title: 在 Linux 上使用 Nginx 裝載 ASP.NET Core
author: rick-anderson
description: 描述如何設定 Nginx 做 Ubuntu 16.04 轉送 Kestrel 上執行 ASP.NET Core web 應用程式的 HTTP 流量上的反向 proxy。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: fe772203e5e3fceb7489e0a5866f60ea914b7329
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/10/2018
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="71d80-103">在 Linux 上使用 Nginx 裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="71d80-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="71d80-104">作者：[Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="71d80-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="71d80-105">本指南說明在 Ubuntu 16.04 伺服器上設定生產環境就緒的 ASP.NET Core 環境。</span><span class="sxs-lookup"><span data-stu-id="71d80-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

> [!NOTE]
> <span data-ttu-id="71d80-106">Ubuntu 14.04 如*supervisord*建議做為監視 Kestrel 程序的解決方案。</span><span class="sxs-lookup"><span data-stu-id="71d80-106">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="71d80-107">*systemd* Ubuntu 14.04 上無法使用。</span><span class="sxs-lookup"><span data-stu-id="71d80-107">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="71d80-108">[請參閱此文件的舊版](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)。</span><span class="sxs-lookup"><span data-stu-id="71d80-108">[See previous version of this document](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="71d80-109">本指南：</span><span class="sxs-lookup"><span data-stu-id="71d80-109">This guide:</span></span>

* <span data-ttu-id="71d80-110">將現有的 ASP.NET Core 應用程式的反向 proxy 伺服器後方。</span><span class="sxs-lookup"><span data-stu-id="71d80-110">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="71d80-111">設定反向 proxy 伺服器，將要求轉送到 Kestrel web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="71d80-111">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="71d80-112">可確保 web 應用程式啟動時執行的服務精靈為。</span><span class="sxs-lookup"><span data-stu-id="71d80-112">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="71d80-113">設定可協助您重新啟動 web 應用程式的程序管理工具。</span><span class="sxs-lookup"><span data-stu-id="71d80-113">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="71d80-114">必要條件</span><span class="sxs-lookup"><span data-stu-id="71d80-114">Prerequisites</span></span>

1. <span data-ttu-id="71d80-115">以 sudo 權限使用標準使用者帳戶存取 Ubuntu 16.04 伺服器</span><span class="sxs-lookup"><span data-stu-id="71d80-115">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
1. <span data-ttu-id="71d80-116">現有的 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="71d80-116">An existing ASP.NET Core app</span></span>

## <a name="copy-over-the-app"></a><span data-ttu-id="71d80-117">複製應用程式</span><span class="sxs-lookup"><span data-stu-id="71d80-117">Copy over the app</span></span>

<span data-ttu-id="71d80-118">執行[dotnet 發行](/dotnet/core/tools/dotnet-publish)從開發環境來封裝應用程式可以在伺服器執行的獨立目錄中。</span><span class="sxs-lookup"><span data-stu-id="71d80-118">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="71d80-119">將 ASP.NET Core 應用程式複製到使用任何工具整合到組織的工作流程 （例如，SCP，FTP） 伺服器。</span><span class="sxs-lookup"><span data-stu-id="71d80-119">Copy the ASP.NET Core app to the server using whatever tool integrates into the organization's workflow (for example, SCP, FTP).</span></span> <span data-ttu-id="71d80-120">測試應用程式，例如：</span><span class="sxs-lookup"><span data-stu-id="71d80-120">Test the app, for example:</span></span>

* <span data-ttu-id="71d80-121">從命令列執行`dotnet <app_assembly>.dll`。</span><span class="sxs-lookup"><span data-stu-id="71d80-121">From the command line, run `dotnet <app_assembly>.dll`.</span></span>
* <span data-ttu-id="71d80-122">在瀏覽器中，巡覽至 `http://<serveraddress>:<port>` 以確認應用程式可在 Linux 上正常運作。</span><span class="sxs-lookup"><span data-stu-id="71d80-122">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="71d80-123">設定反向 Proxy 伺服器</span><span class="sxs-lookup"><span data-stu-id="71d80-123">Configure a reverse proxy server</span></span>

<span data-ttu-id="71d80-124">反向 proxy 是常見的安裝程式來服務動態 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="71d80-124">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="71d80-125">反向 proxy 終止 HTTP 要求，並將它轉送至 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="71d80-125">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="71d80-126">為何使用反向 Proxy 伺服器？</span><span class="sxs-lookup"><span data-stu-id="71d80-126">Why use a reverse proxy server?</span></span>

<span data-ttu-id="71d80-127">Kestrel 適合用來從 ASP.NET Core 提供動態內容。</span><span class="sxs-lookup"><span data-stu-id="71d80-127">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="71d80-128">不過，web 服務功能不是做為伺服器，如 IIS、 Apache 或 Nginx 豐富的功能。</span><span class="sxs-lookup"><span data-stu-id="71d80-128">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="71d80-129">反向 proxy 伺服器可以卸載提供靜態內容、 快取要求、 壓縮要求，以及從 HTTP 伺服器的 SSL 終止的工作。</span><span class="sxs-lookup"><span data-stu-id="71d80-129">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="71d80-130">反向 Proxy 伺服器可能位在專用電腦上，或可能與 HTTP 伺服器一起部署。</span><span class="sxs-lookup"><span data-stu-id="71d80-130">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="71d80-131">為達到本指南的目的，使用 Nginx 的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="71d80-131">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="71d80-132">它會在相同的伺服器上和 HTTP 伺服器一起執行。</span><span class="sxs-lookup"><span data-stu-id="71d80-132">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="71d80-133">根據需求，也可以選擇不同的安裝程式。</span><span class="sxs-lookup"><span data-stu-id="71d80-133">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="71d80-134">要求會透過反向 proxy 來轉送的因為使用轉送標頭中介軟體從[Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/)封裝。</span><span class="sxs-lookup"><span data-stu-id="71d80-134">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="71d80-135">中介軟體更新`Request.Scheme`，並使用`X-Forwarded-Proto`標頭，以便讓該重新導向 Uri 和其他的安全性原則運作正確。</span><span class="sxs-lookup"><span data-stu-id="71d80-135">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="71d80-136">使用任何類型的驗證中介軟體時，必須先執行轉送標頭中介軟體。</span><span class="sxs-lookup"><span data-stu-id="71d80-136">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="71d80-137">這種排序可確保驗證中介軟體可以取用的標頭值，並產生正確的重新導向 Uri。</span><span class="sxs-lookup"><span data-stu-id="71d80-137">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="71d80-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="71d80-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="71d80-139">叫用[UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)方法中的`Startup.Configure`之前先呼叫[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication)或類似的驗證配置中介軟體。</span><span class="sxs-lookup"><span data-stu-id="71d80-139">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="71d80-140">設定要轉寄的中介軟體`X-Forwarded-For`和`X-Forwarded-Proto`標頭：</span><span class="sxs-lookup"><span data-stu-id="71d80-140">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="71d80-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="71d80-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="71d80-142">叫用[UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)方法中的`Startup.Configure`之前先呼叫[UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity)和[UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication)或類似的驗證配置中介軟體。</span><span class="sxs-lookup"><span data-stu-id="71d80-142">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="71d80-143">設定要轉寄的中介軟體`X-Forwarded-For`和`X-Forwarded-Proto`標頭：</span><span class="sxs-lookup"><span data-stu-id="71d80-143">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="71d80-144">如果沒有[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)指定給中介軟體，轉送的預設標頭`None`。</span><span class="sxs-lookup"><span data-stu-id="71d80-144">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="71d80-145">Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。</span><span class="sxs-lookup"><span data-stu-id="71d80-145">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="71d80-146">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="71d80-146">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-nginx"></a><span data-ttu-id="71d80-147">安裝 Nginx</span><span class="sxs-lookup"><span data-stu-id="71d80-147">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="71d80-148">如果要安裝選擇性 Nginx 模組，建立從來源 Nginx 可能需要。</span><span class="sxs-lookup"><span data-stu-id="71d80-148">If optional Nginx modules will be installed, building Nginx from source might be required.</span></span>

<span data-ttu-id="71d80-149">使用 `apt-get` 來安裝 Nginx。</span><span class="sxs-lookup"><span data-stu-id="71d80-149">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="71d80-150">安裝程式建立的系統 V init 初始化指令碼，會在系統啟動時將 Nginx 執行為精靈。</span><span class="sxs-lookup"><span data-stu-id="71d80-150">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="71d80-151">因為 Nginx 是第一次安裝，請透過執行明確啟動它：</span><span class="sxs-lookup"><span data-stu-id="71d80-151">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="71d80-152">確認瀏覽器會顯示 Nginx 的預設登陸頁面。</span><span class="sxs-lookup"><span data-stu-id="71d80-152">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="71d80-153">設定 Nginx</span><span class="sxs-lookup"><span data-stu-id="71d80-153">Configure Nginx</span></span>

<span data-ttu-id="71d80-154">若要設定 Nginx 做為轉寄要求至您的 ASP.NET Core 應用程式的反向 proxy，修改 */etc/nginx/sites-available/default*。</span><span class="sxs-lookup"><span data-stu-id="71d80-154">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="71d80-155">以文字編輯器開啟它，並以下列項目取代內容：</span><span class="sxs-lookup"><span data-stu-id="71d80-155">Open it in a text editor, and replace the contents with the following:</span></span>

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

<span data-ttu-id="71d80-156">若未`server_name`Nginx 相符項目會使用預設伺服器。</span><span class="sxs-lookup"><span data-stu-id="71d80-156">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="71d80-157">如果沒有預設的伺服器定義，在組態檔中的第一部伺服器是預設伺服器。</span><span class="sxs-lookup"><span data-stu-id="71d80-157">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="71d80-158">最佳作法是新增特定的預設伺服器會傳回狀態碼 444 組態檔中。</span><span class="sxs-lookup"><span data-stu-id="71d80-158">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="71d80-159">預設伺服器組態範例如下：</span><span class="sxs-lookup"><span data-stu-id="71d80-159">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="71d80-160">使用上述組態檔案和預設伺服器，Nginx 接受主機標頭的通訊埠 80 上的公用流量`example.com`或`*.example.com`。</span><span class="sxs-lookup"><span data-stu-id="71d80-160">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="71d80-161">要求不符合這些主機將不會取得轉送給 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="71d80-161">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="71d80-162">Nginx 要求轉送給比對在 Kestrel `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="71d80-162">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="71d80-163">請參閱[nginx 如何處理要求](https://nginx.org/docs/http/request_processing.html)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="71d80-163">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span>

> [!WARNING]
> <span data-ttu-id="71d80-164">無法指定適當的[server_name 指示詞](https://nginx.org/docs/http/server_names.html)會公開您的應用程式的安全性漏洞。</span><span class="sxs-lookup"><span data-stu-id="71d80-164">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="71d80-165">子網域萬用字元繫結 (例如， `*.example.com`) 不會造成安全性風險，如果您要控制整個父系網域 (與`*.com`，這是很容易遭受)。</span><span class="sxs-lookup"><span data-stu-id="71d80-165">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="71d80-166">如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="71d80-166">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="71d80-167">一旦建立 Nginx 組態之後，執行`sudo nginx -t`驗證組態檔的語法。</span><span class="sxs-lookup"><span data-stu-id="71d80-167">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="71d80-168">如果組態檔測試成功時，強制以收取這些變更藉由執行 Nginx `sudo nginx -s reload`。</span><span class="sxs-lookup"><span data-stu-id="71d80-168">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="71d80-169">監視應用程式</span><span class="sxs-lookup"><span data-stu-id="71d80-169">Monitoring the app</span></span>

<span data-ttu-id="71d80-170">伺服器是安裝所做的要求轉送給`http://<serveraddress>:80`入 Kestrel 在上執行 ASP.NET Core 應用程式`http://127.0.0.1:5000`。</span><span class="sxs-lookup"><span data-stu-id="71d80-170">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="71d80-171">不過，Nginx 並未設定來管理 Kestrel 程序。</span><span class="sxs-lookup"><span data-stu-id="71d80-171">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="71d80-172">*systemd*可用來建立服務檔案，以啟動及監視基礎的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="71d80-172">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="71d80-173">*systemd* 是 init 系統，提供許多強大的啟動、停止和管理處理程序功能。</span><span class="sxs-lookup"><span data-stu-id="71d80-173">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="71d80-174">建立服務檔</span><span class="sxs-lookup"><span data-stu-id="71d80-174">Create the service file</span></span>

<span data-ttu-id="71d80-175">建立服務定義檔：</span><span class="sxs-lookup"><span data-stu-id="71d80-175">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="71d80-176">以下是範例服務檔案的應用程式：</span><span class="sxs-lookup"><span data-stu-id="71d80-176">The following is an example service file for the app:</span></span>

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

<span data-ttu-id="71d80-177">**注意：** 如果使用者*www 資料*不會使用設定，此處定義的使用者必須先建立並提供適當的擁有權的檔案。</span><span class="sxs-lookup"><span data-stu-id="71d80-177">**Note:** If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>
<span data-ttu-id="71d80-178">**注意：** Linux 具有區分大小寫的檔案系統。</span><span class="sxs-lookup"><span data-stu-id="71d80-178">**Note:** Linux has a case-sensitive file system.</span></span> <span data-ttu-id="71d80-179">設定為 「 生產 」 會產生組態檔的搜尋 ASPNETCORE_ENVIRONMENT *appsettings。Production.json*，而非*appsettings.production.json*。</span><span class="sxs-lookup"><span data-stu-id="71d80-179">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="71d80-180">必須逸出讀取環境變數的組態提供者的某些值 （例如，SQL 連接字串）。</span><span class="sxs-lookup"><span data-stu-id="71d80-180">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="71d80-181">若要使用的正確逸出的值產生組態檔中使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="71d80-181">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="71d80-182">儲存檔案並啟用服務。</span><span class="sxs-lookup"><span data-stu-id="71d80-182">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="71d80-183">啟動服務並確認它正在執行。</span><span class="sxs-lookup"><span data-stu-id="71d80-183">Start the service and verify that it's running.</span></span>

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

<span data-ttu-id="71d80-184">Web 應用程式完全設定和可以從在本機電腦上的瀏覽器存取的反向 proxy 設定和管理透過 systemd Kestrel， `http://localhost`。</span><span class="sxs-lookup"><span data-stu-id="71d80-184">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="71d80-185">它也是可從遠端電腦，限制可能會封鎖任何防火牆存取。</span><span class="sxs-lookup"><span data-stu-id="71d80-185">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="71d80-186">檢查回應標頭`Server`標頭會顯示由 Kestrel ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="71d80-186">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="71d80-187">檢視記錄</span><span class="sxs-lookup"><span data-stu-id="71d80-187">Viewing logs</span></span>

<span data-ttu-id="71d80-188">因為 web 應用程式使用 Kestrel 管理使用`systemd`，所有事件和處理程序會都記錄到集中式的日誌。</span><span class="sxs-lookup"><span data-stu-id="71d80-188">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="71d80-189">不過，此日誌包含 `systemd` 管理的所有服務和處理程序的所有項目。</span><span class="sxs-lookup"><span data-stu-id="71d80-189">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="71d80-190">若要檢視 `kestrel-hellomvc.service` 的特定項目，請使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="71d80-190">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="71d80-191">如需進一步篩選，例如 `--since today`、`--until 1 hour ago` 或這些項目的組合等時間選項，可以減少傳回的項目數量。</span><span class="sxs-lookup"><span data-stu-id="71d80-191">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="71d80-192">保護應用程式</span><span class="sxs-lookup"><span data-stu-id="71d80-192">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="71d80-193">啟用 AppArmor</span><span class="sxs-lookup"><span data-stu-id="71d80-193">Enable AppArmor</span></span>

<span data-ttu-id="71d80-194">Linux 安全性模組 (LSM) 是一種架構，屬於後 Linux 2.6 Linux 核心。</span><span class="sxs-lookup"><span data-stu-id="71d80-194">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="71d80-195">LSM 支援不同的安全性模組實作。</span><span class="sxs-lookup"><span data-stu-id="71d80-195">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="71d80-196">[AppArmor](https://wiki.ubuntu.com/AppArmor) 是 LSM，它實作的必要存取控制系統，可將程式限定在有限的資源集中。</span><span class="sxs-lookup"><span data-stu-id="71d80-196">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="71d80-197">確定已啟用並正確設定 AppArmor。</span><span class="sxs-lookup"><span data-stu-id="71d80-197">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="71d80-198">設定防火牆</span><span class="sxs-lookup"><span data-stu-id="71d80-198">Configuring the firewall</span></span>

<span data-ttu-id="71d80-199">關閉所有不在使用中的外部連接埠。</span><span class="sxs-lookup"><span data-stu-id="71d80-199">Close off all external ports that are not in use.</span></span> <span data-ttu-id="71d80-200">簡單的防火牆 (ufw) 提供命令列介面供設定防火牆，為 `iptables` 提供前端。</span><span class="sxs-lookup"><span data-stu-id="71d80-200">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="71d80-201">確認`ufw`設定為允許任何所需的連接埠上的流量。</span><span class="sxs-lookup"><span data-stu-id="71d80-201">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="71d80-202">保護 Nginx</span><span class="sxs-lookup"><span data-stu-id="71d80-202">Securing Nginx</span></span>

<span data-ttu-id="71d80-203">Nginx 的預設分佈不會啟用 SSL。</span><span class="sxs-lookup"><span data-stu-id="71d80-203">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="71d80-204">為啟用其他的安全性功能，請從來源建置。</span><span class="sxs-lookup"><span data-stu-id="71d80-204">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="71d80-205">下載來源並安裝組建相依性</span><span class="sxs-lookup"><span data-stu-id="71d80-205">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download Nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="71d80-206">變更 Nginx 回應名稱</span><span class="sxs-lookup"><span data-stu-id="71d80-206">Change the Nginx response name</span></span>

<span data-ttu-id="71d80-207">編輯 *src/http/ngx_http_header_filter_module.c*：</span><span class="sxs-lookup"><span data-stu-id="71d80-207">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="71d80-208">設定選項和組建</span><span class="sxs-lookup"><span data-stu-id="71d80-208">Configure the options and build</span></span>

<span data-ttu-id="71d80-209">規則運算式需要 PCRE 程式庫。</span><span class="sxs-lookup"><span data-stu-id="71d80-209">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="71d80-210">規則運算式用於 ngx_http_rewrite_module 的位置指示詞。</span><span class="sxs-lookup"><span data-stu-id="71d80-210">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="71d80-211">http_ssl_module 新增 HTTPS 通訊協定的支援。</span><span class="sxs-lookup"><span data-stu-id="71d80-211">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="71d80-212">請考慮使用 web 應用程式防火牆像*ModSecurity*強化應用程式。</span><span class="sxs-lookup"><span data-stu-id="71d80-212">Consider using a web app firewall like *ModSecurity* to harden the app.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="71d80-213">設定 SSL</span><span class="sxs-lookup"><span data-stu-id="71d80-213">Configure SSL</span></span>

* <span data-ttu-id="71d80-214">將伺服器設定為接聽連接埠上的 HTTPS 流量`443`藉由指定核發由受信任憑證授權單位 (CA) 的有效憑證。</span><span class="sxs-lookup"><span data-stu-id="71d80-214">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="71d80-215">利用一些下列所述的作法強化安全性 */etc/nginx/nginx.conf*檔案。</span><span class="sxs-lookup"><span data-stu-id="71d80-215">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="71d80-216">範例包括選擇更強的加密，重新導向 HTTPS 到 HTTP 的所有流量。</span><span class="sxs-lookup"><span data-stu-id="71d80-216">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="71d80-217">新增 `HTTP Strict-Transport-Security` (HSTS) 標頭可確保用戶端提出的所有後續要求都只會透過 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="71d80-217">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="71d80-218">請勿將加入 Strict 傳輸安全性標頭，或選擇適當`max-age`如果未來將會停用 SSL。</span><span class="sxs-lookup"><span data-stu-id="71d80-218">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="71d80-219">新增 */etc/nginx/Proxy.conf* 組態檔：</span><span class="sxs-lookup"><span data-stu-id="71d80-219">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="71d80-220">編輯 */etc/nginx/Proxy.conf* 組態檔。</span><span class="sxs-lookup"><span data-stu-id="71d80-220">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="71d80-221">此範例在一個組態檔中同時包含 `http` 和 `server` 區段。</span><span class="sxs-lookup"><span data-stu-id="71d80-221">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="71d80-222">保護 Nginx 免於點閱綁架</span><span class="sxs-lookup"><span data-stu-id="71d80-222">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="71d80-223">點閱綁架是收集受感染使用者按一下動作的惡意技術。</span><span class="sxs-lookup"><span data-stu-id="71d80-223">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="71d80-224">點閱綁架誘騙受害者 (訪客) 到受感染的網站上執行按一下的動作。</span><span class="sxs-lookup"><span data-stu-id="71d80-224">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="71d80-225">使用 X-框架-選項來保護安全的站台。</span><span class="sxs-lookup"><span data-stu-id="71d80-225">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="71d80-226">編輯 *nginx.conf* 檔案：</span><span class="sxs-lookup"><span data-stu-id="71d80-226">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="71d80-227">新增行 `add_header X-Frame-Options "SAMEORIGIN";` 並儲存檔案，然後重新啟動 Nginx。</span><span class="sxs-lookup"><span data-stu-id="71d80-227">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="71d80-228">MIME 類型探查</span><span class="sxs-lookup"><span data-stu-id="71d80-228">MIME-type sniffing</span></span>

<span data-ttu-id="71d80-229">此標頭會防止大部分的瀏覽器對遠離宣告內容類型的回應進行 MIME 探查，因為標頭會指示瀏覽器不要覆寫回應內容類型。</span><span class="sxs-lookup"><span data-stu-id="71d80-229">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="71d80-230">使用 `nosniff` 選項，如果伺服器顯示的內容是 "text/html"，則瀏覽器就會將它轉譯為 "text/html"。</span><span class="sxs-lookup"><span data-stu-id="71d80-230">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="71d80-231">編輯 *nginx.conf* 檔案：</span><span class="sxs-lookup"><span data-stu-id="71d80-231">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="71d80-232">新增 `add_header X-Content-Type-Options "nosniff";` 行並儲存檔案，然後重新啟動 Nginx。</span><span class="sxs-lookup"><span data-stu-id="71d80-232">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>
