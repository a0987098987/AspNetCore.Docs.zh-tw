---
title: "在 Linux 上使用 Nginx 裝載 ASP.NET Core"
description: "描述如何將 Nginx 設定為 Ubuntu 16.04 上的反向 Proxy，將 HTTP 流量轉送至在 Kestrel 上執行的 ASP.NET Core Web 應用程式。"
keywords: "ASP.NET Core, Linux, nginx, Ubuntu, 反向 Proxy"
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 08/21/2017
ms.topic: article
ms.assetid: 1c33e576-33de-481a-8ad3-896b94fde0e3
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/linuxproduction
ms.openlocfilehash: 01768263fe82dc75a7da0e113b1850c8d788bfd3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-nginx-and-deploy-to-it"></a><span data-ttu-id="50500-104">在 Linux 上使用 Nginx 設定並部署到 ASP.NET Core 裝載環境</span><span class="sxs-lookup"><span data-stu-id="50500-104">Set up a hosting environment for ASP.NET Core on Linux with Nginx, and deploy to it</span></span>

<span data-ttu-id="50500-105">作者：[Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="50500-105">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="50500-106">本指南說明在 Ubuntu 16.04 伺服器上設定生產環境就緒的 ASP.NET Core 環境。</span><span class="sxs-lookup"><span data-stu-id="50500-106">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

<span data-ttu-id="50500-107">**注意：**若為 Ubuntu 14.04，建議當成監視 Kestrel 程序的解決方案監督。</span><span class="sxs-lookup"><span data-stu-id="50500-107">**Note:** For Ubuntu 14.04, supervisord is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="50500-108">Ubuntu 14.04 無法使用 systemd。</span><span class="sxs-lookup"><span data-stu-id="50500-108">systemd is not available on Ubuntu 14.04.</span></span> [<span data-ttu-id="50500-109">請參閱本文件的舊版本</span><span class="sxs-lookup"><span data-stu-id="50500-109">See previous version of this document</span></span>](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

<span data-ttu-id="50500-110">本指南：</span><span class="sxs-lookup"><span data-stu-id="50500-110">This guide:</span></span>

* <span data-ttu-id="50500-111">將現有的 ASP.NET Core 應用程式放在反向 Proxy 伺服器後端</span><span class="sxs-lookup"><span data-stu-id="50500-111">Places an existing ASP.NET Core application behind a reverse proxy server</span></span>
* <span data-ttu-id="50500-112">設定反向 Proxy 伺服器，將要求轉送到 Kestrel 網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="50500-112">Sets up the reverse proxy server to forward requests to the Kestrel web server</span></span>
* <span data-ttu-id="50500-113">確保 Web 應用程式在啟動時執行為精靈</span><span class="sxs-lookup"><span data-stu-id="50500-113">Ensures the web application runs on startup as a daemon</span></span>
* <span data-ttu-id="50500-114">設定程序管理工具以協助重新啟動 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="50500-114">Configures a process management tool to help restart the web application</span></span>

## <a name="prerequisites"></a><span data-ttu-id="50500-115">必要條件</span><span class="sxs-lookup"><span data-stu-id="50500-115">Prerequisites</span></span>

1. <span data-ttu-id="50500-116">以 sudo 權限使用標準使用者帳戶存取 Ubuntu 16.04 伺服器</span><span class="sxs-lookup"><span data-stu-id="50500-116">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="50500-117">現有的 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="50500-117">An existing ASP.NET Core application</span></span>

## <a name="copy-over-your-app"></a><span data-ttu-id="50500-118">複製蓋過您的應用程式</span><span class="sxs-lookup"><span data-stu-id="50500-118">Copy over your app</span></span>

<span data-ttu-id="50500-119">從開發環境執行 `dotnet publish`，將應用程式封裝到可在伺服器執行的獨立目錄中。</span><span class="sxs-lookup"><span data-stu-id="50500-119">Run `dotnet publish` from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="50500-120">使用任何工具 (SCP、FTP 等等) 將 ASP.NET Core 應用程式複製到伺服器，整合至您的工作流程。</span><span class="sxs-lookup"><span data-stu-id="50500-120">Copy the ASP.NET Core app to the server using whatever tool (SCP, FTP, etc.) integrates into your workflow.</span></span> <span data-ttu-id="50500-121">測試應用程式，例如：</span><span class="sxs-lookup"><span data-stu-id="50500-121">Test the app, for example:</span></span>
 - <span data-ttu-id="50500-122">從命令列執行 `dotnet yourapp.dll`</span><span class="sxs-lookup"><span data-stu-id="50500-122">From the command line, run `dotnet yourapp.dll`</span></span>
 - <span data-ttu-id="50500-123">在瀏覽器中，巡覽至 `http://<serveraddress>:<port>` 以確認應用程式可在 Linux 上正常運作。</span><span class="sxs-lookup"><span data-stu-id="50500-123">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
<span data-ttu-id="50500-124">**注意：**使用 [Yeoman](xref:client-side/yeoman) 針對新專案建立新的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="50500-124">**Note:** Use [Yeoman](xref:client-side/yeoman) to create a new ASP.NET Core app for a new project.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="50500-125">設定反向 Proxy 伺服器</span><span class="sxs-lookup"><span data-stu-id="50500-125">Configure a reverse proxy server</span></span>

<span data-ttu-id="50500-126">反向 Proxy 是服務動態 Web 應用程式的常見安裝程式。</span><span class="sxs-lookup"><span data-stu-id="50500-126">A reverse proxy is a common setup for serving dynamic web applications.</span></span> <span data-ttu-id="50500-127">反向 Proxy 會終止 HTTP 要求，並將它轉送至 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="50500-127">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core application.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="50500-128">為何使用反向 Proxy 伺服器？</span><span class="sxs-lookup"><span data-stu-id="50500-128">Why use a reverse proxy server?</span></span>

<span data-ttu-id="50500-129">Kestrel 非常適合提供 ASP.NET Core 的動態內容，但 Web 服務組件不像 IIS、Apache 或 Nginx 等伺服器的功能豐富。</span><span class="sxs-lookup"><span data-stu-id="50500-129">Kestrel is great for serving dynamic content from ASP.NET Core; however, the web serving parts aren’t as feature rich as servers like IIS, Apache, or Nginx.</span></span> <span data-ttu-id="50500-130">反向 Proxy 伺服器可以卸載像提供靜態內容、快取要求、壓縮要求，以及從 HTTP 伺服器終止 SSL 等工作。</span><span class="sxs-lookup"><span data-stu-id="50500-130">A reverse proxy server can offload work like serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="50500-131">反向 Proxy 伺服器可能位在專用電腦上，或可能與 HTTP 伺服器一起部署。</span><span class="sxs-lookup"><span data-stu-id="50500-131">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="50500-132">為達到本指南的目的，使用 Nginx 的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="50500-132">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="50500-133">它會在相同的伺服器上和 HTTP 伺服器一起執行。</span><span class="sxs-lookup"><span data-stu-id="50500-133">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="50500-134">根據您的需求，您可以選擇不同的安裝程式。</span><span class="sxs-lookup"><span data-stu-id="50500-134">Based on your requirements, you may choose a different setup.</span></span>

<span data-ttu-id="50500-135">因為反向 Proxy 會轉送要求，請從 `Microsoft.AspNetCore.HttpOverrides` 套件使用 `ForwardedHeaders` 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="50500-135">Because requests are forwarded by reverse proxy, use the `ForwardedHeaders` middleware from the `Microsoft.AspNetCore.HttpOverrides` package.</span></span> <span data-ttu-id="50500-136">此中介軟體會使用 `X-Forwarded-Proto` 標頭更新 `Request.Scheme`，讓重新導向 URI 和其他安全性原則正確運作。</span><span class="sxs-lookup"><span data-stu-id="50500-136">This middleware updates `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="50500-137">設定反向 Proxy 伺服器時，驗證中介軟體需要 `UseForwardedHeaders` 先執行。</span><span class="sxs-lookup"><span data-stu-id="50500-137">When setting up a reverse proxy server, the authentication middleware needs `UseForwardedHeaders` to run first.</span></span> <span data-ttu-id="50500-138">這種排序可確保驗證中介軟體能夠取用受影響的值，並產生正確的重新導向 URI。</span><span class="sxs-lookup"><span data-stu-id="50500-138">This ordering ensures that the authentication middleware can consume the affected values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="50500-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="50500-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="50500-140">先叫用 `UseForwardedHeaders` 方法 (在 *Startup.cs* 的 `Configure`方法中)，再呼叫 `UseAuthentication` 或類似的驗證配置中介軟體：</span><span class="sxs-lookup"><span data-stu-id="50500-140">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseAuthentication` or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="50500-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="50500-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="50500-142">先叫用 `UseForwardedHeaders` 方法 (在 *Startup.cs* 的 `Configure` 方法中)，再呼叫 `UseIdentity` 和 `UseFacebookAuthentication` 或類似的驗證配置中介軟體：</span><span class="sxs-lookup"><span data-stu-id="50500-142">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseIdentity` and `UseFacebookAuthentication` or similar authentication scheme middleware:</span></span>

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

### <a name="install-nginx"></a><span data-ttu-id="50500-143">安裝 Nginx</span><span class="sxs-lookup"><span data-stu-id="50500-143">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="50500-144">如果您打算安裝選擇性 Nginx 模組，您可能需要從來源建置 Nginx。</span><span class="sxs-lookup"><span data-stu-id="50500-144">If you plan to install optional Nginx modules, you may be required to build Nginx from source.</span></span>

<span data-ttu-id="50500-145">使用 `apt-get` 來安裝 Nginx。</span><span class="sxs-lookup"><span data-stu-id="50500-145">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="50500-146">安裝程式建立的系統 V init 初始化指令碼，會在系統啟動時將 Nginx 執行為精靈。</span><span class="sxs-lookup"><span data-stu-id="50500-146">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="50500-147">因為 Nginx 是第一次安裝，請透過執行明確啟動它：</span><span class="sxs-lookup"><span data-stu-id="50500-147">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="50500-148">確認瀏覽器會顯示 Nginx 的預設登陸頁面。</span><span class="sxs-lookup"><span data-stu-id="50500-148">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="50500-149">設定 Nginx</span><span class="sxs-lookup"><span data-stu-id="50500-149">Configure Nginx</span></span>

<span data-ttu-id="50500-150">若要將 Nginx 設定為反向 Proxy，將要求轉寄至我們的 ASP.NET Core 應用程式，請修改 `/etc/nginx/sites-available/default`。</span><span class="sxs-lookup"><span data-stu-id="50500-150">To configure Nginx as a reverse proxy to forward requests to our ASP.NET Core application, modify `/etc/nginx/sites-available/default`.</span></span> <span data-ttu-id="50500-151">以文字編輯器開啟它，並以下列項目取代內容：</span><span class="sxs-lookup"><span data-stu-id="50500-151">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

<span data-ttu-id="50500-152">此 Nginx 組態檔會將連入的公用流量從連接埠 `80` 轉送到連接埠 `5000`。</span><span class="sxs-lookup"><span data-stu-id="50500-152">This Nginx configuration file forwards incoming public traffic from port `80` to port `5000`.</span></span>

<span data-ttu-id="50500-153">一旦完成對 Nginx 組態的變更，您就可以執行 `sudo nginx -t` 以驗證組態檔的語法。</span><span class="sxs-lookup"><span data-stu-id="50500-153">Once you have completed making changes to your Nginx configuration, you can run `sudo nginx -t` to verify the syntax of your configuration files.</span></span> <span data-ttu-id="50500-154">如果組態檔測試成功，您可以要求 Nginx 藉由執行 `sudo nginx -s reload` 來挑選這些變更。</span><span class="sxs-lookup"><span data-stu-id="50500-154">If the configuration file test is successful, you can ask Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-our-application"></a><span data-ttu-id="50500-155">監控應用程式</span><span class="sxs-lookup"><span data-stu-id="50500-155">Monitoring our application</span></span>

<span data-ttu-id="50500-156">Nginx 已設定完成，可將向 `http://yourhost:80` 提出的要求轉送給在 `http://127.0.0.1:5000` 的 Kestrel 上執行的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="50500-156">Nginx is now setup to forward requests made to `http://yourhost:80` on to the ASP.NET Core application running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="50500-157">不過，Nginx 未設定管理 Kestrel 程序。</span><span class="sxs-lookup"><span data-stu-id="50500-157">However, Nginx is not set up to manage the Kestrel process.</span></span> <span data-ttu-id="50500-158">您可以使用 *systemd* 並建立服務檔案，啟動及監視基礎的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="50500-158">You can use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="50500-159">*systemd* 是 init 系統，提供許多強大的啟動、停止和管理處理程序功能。</span><span class="sxs-lookup"><span data-stu-id="50500-159">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="50500-160">建立服務檔</span><span class="sxs-lookup"><span data-stu-id="50500-160">Create the service file</span></span>

<span data-ttu-id="50500-161">建立服務定義檔：</span><span class="sxs-lookup"><span data-stu-id="50500-161">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="50500-162">以下是我們的應用程式範例服務檔：</span><span class="sxs-lookup"><span data-stu-id="50500-162">The following is an example service file for our application:</span></span>

```ini
[Unit]
Description=Example .NET Web API Application running on Ubuntu

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

<span data-ttu-id="50500-163">**注意：**如果您的組態不使用使用者 *www-data*，則必須先建立此處定義的使用者，並指定適當的檔案擁有權。</span><span class="sxs-lookup"><span data-stu-id="50500-163">**Note:** If the user *www-data* is not used by your configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="50500-164">儲存檔案並啟用服務。</span><span class="sxs-lookup"><span data-stu-id="50500-164">Save the file, and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="50500-165">啟動服務並確認它正在執行。</span><span class="sxs-lookup"><span data-stu-id="50500-165">Start the service and verify that it is running.</span></span>

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API Application running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="50500-166">透過 systemd 設定反向 Proxy 及管理 Kestrel，可以完全設定 Web 應用程式，並從位在 `http://localhost` 的本機電腦瀏覽器存取它。</span><span class="sxs-lookup"><span data-stu-id="50500-166">With the reverse proxy configured and Kestrel managed through systemd, the web application is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="50500-167">它也可以從遠端電腦存取，阻擋可能執行封鎖的任何防火牆。</span><span class="sxs-lookup"><span data-stu-id="50500-167">It is also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="50500-168">檢查回應標頭，`Server` 標頭顯示 Kestrel 正在為 ASP.NET Core 應用程式提供服務。</span><span class="sxs-lookup"><span data-stu-id="50500-168">Inspecting the response headers, the `Server` header shows the ASP.NET Core application being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="50500-169">檢視記錄</span><span class="sxs-lookup"><span data-stu-id="50500-169">Viewing logs</span></span>

<span data-ttu-id="50500-170">因為使用 `systemd` 管理使用 Kestrel 的 Web 應用程式，所以所有事件和處理程序都會記錄在集中式日誌中。</span><span class="sxs-lookup"><span data-stu-id="50500-170">Since the web application using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="50500-171">不過，此日誌包含 `systemd` 管理的所有服務和處理程序的所有項目。</span><span class="sxs-lookup"><span data-stu-id="50500-171">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="50500-172">若要檢視 `kestrel-hellomvc.service` 的特定項目，請使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="50500-172">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="50500-173">如需進一步篩選，例如 `--since today`、`--until 1 hour ago` 或這些項目的組合等時間選項，可以減少傳回的項目數量。</span><span class="sxs-lookup"><span data-stu-id="50500-173">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a><span data-ttu-id="50500-174">保護應用程式</span><span class="sxs-lookup"><span data-stu-id="50500-174">Securing our application</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="50500-175">啟用 AppArmor</span><span class="sxs-lookup"><span data-stu-id="50500-175">Enable AppArmor</span></span>

<span data-ttu-id="50500-176">Linux 安全性模組 (LSM) 是一種架構，屬於 Linux 2.6 以後的 Linux 核心。</span><span class="sxs-lookup"><span data-stu-id="50500-176">Linux Security Modules (LSM) is a framework that is part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="50500-177">LSM 支援不同的安全性模組實作。</span><span class="sxs-lookup"><span data-stu-id="50500-177">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="50500-178">[AppArmor](https://wiki.ubuntu.com/AppArmor) 是 LSM，它實作的必要存取控制系統，可將程式限定在有限的資源集中。</span><span class="sxs-lookup"><span data-stu-id="50500-178">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="50500-179">確定已啟用並正確設定 AppArmor。</span><span class="sxs-lookup"><span data-stu-id="50500-179">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-our-firewall"></a><span data-ttu-id="50500-180">設定防火牆</span><span class="sxs-lookup"><span data-stu-id="50500-180">Configuring our firewall</span></span>

<span data-ttu-id="50500-181">關閉所有不在使用中的外部連接埠。</span><span class="sxs-lookup"><span data-stu-id="50500-181">Close off all external ports that are not in use.</span></span> <span data-ttu-id="50500-182">簡單的防火牆 (ufw) 提供命令列介面供設定防火牆，為 `iptables` 提供前端。</span><span class="sxs-lookup"><span data-stu-id="50500-182">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="50500-183">確認已設定 `ufw` 允許所需任何連接埠上的流量。</span><span class="sxs-lookup"><span data-stu-id="50500-183">Verify that `ufw` is configured to allow traffic on any ports you need.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="50500-184">保護 Nginx</span><span class="sxs-lookup"><span data-stu-id="50500-184">Securing Nginx</span></span>

<span data-ttu-id="50500-185">Nginx 的預設分佈不會啟用 SSL。</span><span class="sxs-lookup"><span data-stu-id="50500-185">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="50500-186">為啟用其他的安全性功能，請從來源建置。</span><span class="sxs-lookup"><span data-stu-id="50500-186">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="50500-187">下載來源並安裝組建相依性</span><span class="sxs-lookup"><span data-stu-id="50500-187">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="50500-188">變更 Nginx 回應名稱</span><span class="sxs-lookup"><span data-stu-id="50500-188">Change the Nginx response name</span></span>

<span data-ttu-id="50500-189">編輯 *src/http/ngx_http_header_filter_module.c*：</span><span class="sxs-lookup"><span data-stu-id="50500-189">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Your Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Your Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="50500-190">設定選項和組建</span><span class="sxs-lookup"><span data-stu-id="50500-190">Configure the options and build</span></span>

<span data-ttu-id="50500-191">規則運算式需要 PCRE 程式庫。</span><span class="sxs-lookup"><span data-stu-id="50500-191">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="50500-192">規則運算式用於 ngx_http_rewrite_module 的位置指示詞。</span><span class="sxs-lookup"><span data-stu-id="50500-192">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="50500-193">http_ssl_module 新增 HTTPS 通訊協定的支援。</span><span class="sxs-lookup"><span data-stu-id="50500-193">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="50500-194">請考慮使用像 *ModSecurity* 這樣的 Web 應用程式防火牆來強化您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="50500-194">Consider using a web application firewall like *ModSecurity* to harden your application.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="50500-195">設定 SSL</span><span class="sxs-lookup"><span data-stu-id="50500-195">Configure SSL</span></span>

* <span data-ttu-id="50500-196">藉由指定由受信任憑證授權單位 (CA) 核發的有效憑證，設定伺服器接聽連接埠 `443` 上的 HTTPS 流量。</span><span class="sxs-lookup"><span data-stu-id="50500-196">Configure your server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="50500-197">運用以下 */etc/nginx/nginx.conf* 檔案所述的做法，強化您的安全性。</span><span class="sxs-lookup"><span data-stu-id="50500-197">Harden your security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="50500-198">範例包括選擇更強的加密，重新導向 HTTPS 到 HTTP 的所有流量。</span><span class="sxs-lookup"><span data-stu-id="50500-198">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="50500-199">新增 `HTTP Strict-Transport-Security` (HSTS) 標頭可確保用戶端提出的所有後續要求都只會透過 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="50500-199">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="50500-200">如果未來打算停用 SSL，請不要新增 Strict-Transport-Security 標頭，或選擇適當的 `max-age`。</span><span class="sxs-lookup"><span data-stu-id="50500-200">Do not add the Strict-Transport-Security header or chose an appropriate `max-age` if you plan to disable SSL in the future.</span></span>

<span data-ttu-id="50500-201">新增 */etc/nginx/Proxy.conf* 組態檔：</span><span class="sxs-lookup"><span data-stu-id="50500-201">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[Main](linuxproduction/proxy.conf)]

<span data-ttu-id="50500-202">編輯 */etc/nginx/Proxy.conf* 組態檔。</span><span class="sxs-lookup"><span data-stu-id="50500-202">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="50500-203">此範例在一個組態檔中同時包含 `http` 和 `server` 區段。</span><span class="sxs-lookup"><span data-stu-id="50500-203">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[Main](../publishing/linuxproduction/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="50500-204">保護 Nginx 免於點閱綁架</span><span class="sxs-lookup"><span data-stu-id="50500-204">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="50500-205">點閱綁架是收集受感染使用者按一下動作的惡意技術。</span><span class="sxs-lookup"><span data-stu-id="50500-205">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="50500-206">點閱綁架誘騙受害者 (訪客) 到受感染的網站上執行按一下的動作。</span><span class="sxs-lookup"><span data-stu-id="50500-206">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="50500-207">使用 X-FRAME-OPTIONS 保護您的網站。</span><span class="sxs-lookup"><span data-stu-id="50500-207">Use X-FRAME-OPTIONS to secure your site.</span></span>

<span data-ttu-id="50500-208">編輯 *nginx.conf* 檔案：</span><span class="sxs-lookup"><span data-stu-id="50500-208">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="50500-209">新增行 `add_header X-Frame-Options "SAMEORIGIN";` 並儲存檔案，然後重新啟動 Nginx。</span><span class="sxs-lookup"><span data-stu-id="50500-209">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="50500-210">MIME 類型探查</span><span class="sxs-lookup"><span data-stu-id="50500-210">MIME-type sniffing</span></span>

<span data-ttu-id="50500-211">此標頭會防止大部分的瀏覽器對遠離宣告內容類型的回應進行 MIME 探查，因為標頭會指示瀏覽器不要覆寫回應內容類型。</span><span class="sxs-lookup"><span data-stu-id="50500-211">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="50500-212">使用 `nosniff` 選項，如果伺服器顯示的內容是 "text/html"，則瀏覽器就會將它轉譯為 "text/html"。</span><span class="sxs-lookup"><span data-stu-id="50500-212">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="50500-213">編輯 *nginx.conf* 檔案：</span><span class="sxs-lookup"><span data-stu-id="50500-213">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="50500-214">新增 `add_header X-Content-Type-Options "nosniff";` 行並儲存檔案，然後重新啟動 Nginx。</span><span class="sxs-lookup"><span data-stu-id="50500-214">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>
