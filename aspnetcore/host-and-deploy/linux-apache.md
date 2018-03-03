---
title: "在 Linux 上使用 Apache 裝載 ASP.NET Core"
description: "了解如何設定 Apache 為反向 proxy 伺服器上 CentOS HTTP 流量重新導向 Kestrel 上執行的 ASP.NET Core web 應用程式。"
author: spboyer
manager: wpickett
ms.author: spboyer
ms.custom: mvc
ms.date: 10/19/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-apache
ms.openlocfilehash: b11bc811b6aefce22b60a28afd72c2a2d0b26955
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="f91fd-103">在 Linux 上使用 Apache 裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f91fd-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="f91fd-104">作者：[Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="f91fd-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="f91fd-105">使用本指南，了解如何設定[Apache](https://httpd.apache.org/)為反向 proxy 伺服器上[CentOS 7](https://www.centos.org/) ASP.NET Core web 應用程式上執行的 HTTP 流量重新導向[Kestrel](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="f91fd-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="f91fd-106">[Mod_proxy 延伸](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html)和相關的模組建立伺服器的反向 proxy。</span><span class="sxs-lookup"><span data-stu-id="f91fd-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f91fd-107">必要條件</span><span class="sxs-lookup"><span data-stu-id="f91fd-107">Prerequisites</span></span>

1. <span data-ttu-id="f91fd-108">具有 sudo 權限的標準使用者帳戶執行 CentOS 7 的伺服器</span><span class="sxs-lookup"><span data-stu-id="f91fd-108">Server running CentOS 7 with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="f91fd-109">ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="f91fd-109">ASP.NET Core app</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="f91fd-110">發行應用程式</span><span class="sxs-lookup"><span data-stu-id="f91fd-110">Publish the app</span></span>

<span data-ttu-id="f91fd-111">發行應用程式做為[獨立的部署](/dotnet/core/deploying/#self-contained-deployments-scd)在 CentOS 7 執行階段的 [發行] 組態 (`centos.7-x64`)。</span><span class="sxs-lookup"><span data-stu-id="f91fd-111">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) in Release configuration for the CentOS 7 runtime (`centos.7-x64`).</span></span> <span data-ttu-id="f91fd-112">複製的內容*bin/Release/netcoreapp2.0/centos.7-x64/publish*使用 SCP、 FTP 或其他檔案傳送方法在伺服器的資料夾。</span><span class="sxs-lookup"><span data-stu-id="f91fd-112">Copy the contents of the *bin/Release/netcoreapp2.0/centos.7-x64/publish* folder to the server using SCP, FTP, or other file transfer method.</span></span>

> [!NOTE]
> <span data-ttu-id="f91fd-113">在生產環境部署案例中，連續整合工作流程會發佈應用程式和資產複製到伺服器的工作。</span><span class="sxs-lookup"><span data-stu-id="f91fd-113">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="f91fd-114">設定 Proxy 伺服器</span><span class="sxs-lookup"><span data-stu-id="f91fd-114">Configure a proxy server</span></span>

<span data-ttu-id="f91fd-115">反向 proxy 是常見的安裝程式來服務動態 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f91fd-115">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="f91fd-116">反向 proxy 終止 HTTP 要求，並將它轉送至 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f91fd-116">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="f91fd-117">用戶端將要求轉送至另一部伺服器，而不是本身完成要求的 proxy 伺服器。</span><span class="sxs-lookup"><span data-stu-id="f91fd-117">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="f91fd-118">反向 Proxy 會轉送至固定目的地，通常代表任意的用戶端。</span><span class="sxs-lookup"><span data-stu-id="f91fd-118">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="f91fd-119">在本指南中，Apache 被設定為在相同的伺服器上執行 Kestrel 提供服務的 ASP.NET Core 應用程式的反向 proxy。</span><span class="sxs-lookup"><span data-stu-id="f91fd-119">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="f91fd-120">要求會透過反向 proxy 來轉送的因為使用轉送標頭中介軟體從[Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/)封裝。</span><span class="sxs-lookup"><span data-stu-id="f91fd-120">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="f91fd-121">中介軟體更新`Request.Scheme`，並使用`X-Forwarded-Proto`標頭，以便讓該重新導向 Uri 和其他的安全性原則運作正確。</span><span class="sxs-lookup"><span data-stu-id="f91fd-121">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="f91fd-122">使用任何類型的驗證中介軟體時，必須先執行轉送標頭中介軟體。</span><span class="sxs-lookup"><span data-stu-id="f91fd-122">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="f91fd-123">這種排序可確保驗證中介軟體可以取用的標頭值，並產生正確的重新導向 Uri。</span><span class="sxs-lookup"><span data-stu-id="f91fd-123">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f91fd-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f91fd-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f91fd-125">叫用[UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)方法中的`Startup.Configure`之前先呼叫[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication)或類似的驗證配置中介軟體：</span><span class="sxs-lookup"><span data-stu-id="f91fd-125">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f91fd-126">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f91fd-126">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f91fd-127">叫用[UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)方法中的`Startup.Configure`之前先呼叫[UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity)和[UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication)或類似的驗證配置中介軟體：</span><span class="sxs-lookup"><span data-stu-id="f91fd-127">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware:</span></span>

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

<span data-ttu-id="f91fd-128">如果沒有[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)指定給中介軟體，轉送的預設標頭`None`。</span><span class="sxs-lookup"><span data-stu-id="f91fd-128">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

### <a name="install-apache"></a><span data-ttu-id="f91fd-129">安裝 Apache</span><span class="sxs-lookup"><span data-stu-id="f91fd-129">Install Apache</span></span>

<span data-ttu-id="f91fd-130">更新至最新穩定版本 CentOS 套件：</span><span class="sxs-lookup"><span data-stu-id="f91fd-130">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="f91fd-131">CentOS 上安裝 Apache web 伺服器，以單一`yum`命令：</span><span class="sxs-lookup"><span data-stu-id="f91fd-131">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="f91fd-132">執行命令之後輸出範例：</span><span class="sxs-lookup"><span data-stu-id="f91fd-132">Sample output after running the command:</span></span>

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
> <span data-ttu-id="f91fd-133">在此範例中，輸出會反映 httpd.86_64 因為 CentOS 7 版本是 64 位元。</span><span class="sxs-lookup"><span data-stu-id="f91fd-133">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="f91fd-134">若要確認 Apache 的安裝位置，請從命令提示字元執行 `whereis httpd`。</span><span class="sxs-lookup"><span data-stu-id="f91fd-134">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span> 

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="f91fd-135">設定 Apache 以用於反向 Proxy</span><span class="sxs-lookup"><span data-stu-id="f91fd-135">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="f91fd-136">Apache 的組態檔是位於 `/etc/httpd/conf.d/` 目錄內。</span><span class="sxs-lookup"><span data-stu-id="f91fd-136">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="f91fd-137">任何檔案*.conf*除了模組組態檔中，依字母順序處理延伸模組`/etc/httpd/conf.modules.d/`，其中包含任何設定檔需要載入模組。</span><span class="sxs-lookup"><span data-stu-id="f91fd-137">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="f91fd-138">建立名為應用程式組態檔`hellomvc.conf`:</span><span class="sxs-lookup"><span data-stu-id="f91fd-138">Create a configuration file for the app named `hellomvc.conf`:</span></span>

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
</VirtualHost>
```

<span data-ttu-id="f91fd-139">**VirtualHost**節點可在伺服器上的一個或多個檔案中出現多次。</span><span class="sxs-lookup"><span data-stu-id="f91fd-139">The **VirtualHost** node can appear multiple times in one or more files on a server.</span></span> <span data-ttu-id="f91fd-140">**VirtualHost**設定為使用通訊埠 80 的任何 IP 位址上接聽。</span><span class="sxs-lookup"><span data-stu-id="f91fd-140">**VirtualHost** is set to listen on any IP address using port 80.</span></span> <span data-ttu-id="f91fd-141">下面兩行 port 5000 到 127.0.0.1 的伺服器根目錄的 proxy 要求會設定。</span><span class="sxs-lookup"><span data-stu-id="f91fd-141">The next two lines are set to proxy requests at the root to the server at 127.0.0.1 on port 5000.</span></span> <span data-ttu-id="f91fd-142">雙向通訊， *ProxyPass*和*ProxyPassReverse*所需。</span><span class="sxs-lookup"><span data-stu-id="f91fd-142">For bi-directional communication, *ProxyPass* and *ProxyPassReverse* are required.</span></span>

<span data-ttu-id="f91fd-143">記錄可以設定每個**VirtualHost**使用**ErrorLog**和**CustomLog**指示詞。</span><span class="sxs-lookup"><span data-stu-id="f91fd-143">Logging can be configured per **VirtualHost** using **ErrorLog** and **CustomLog** directives.</span></span> <span data-ttu-id="f91fd-144">**錯誤記錄檔**是伺服器記錄錯誤，在其中的位置和**CustomLog**設定檔案名稱和記錄檔格式。</span><span class="sxs-lookup"><span data-stu-id="f91fd-144">**ErrorLog** is the location where the server logs errors, and **CustomLog** sets the filename and format of log file.</span></span> <span data-ttu-id="f91fd-145">在此情況下，這是記錄要求資訊的位置。</span><span class="sxs-lookup"><span data-stu-id="f91fd-145">In this case, this is where request information is logged.</span></span> <span data-ttu-id="f91fd-146">沒有為每個要求的一列。</span><span class="sxs-lookup"><span data-stu-id="f91fd-146">There's one line for each request.</span></span>

<span data-ttu-id="f91fd-147">儲存檔案並測試組態。</span><span class="sxs-lookup"><span data-stu-id="f91fd-147">Save the file and test the configuration.</span></span> <span data-ttu-id="f91fd-148">如果所有項目都通過，回應應該是 `Syntax [OK]`。</span><span class="sxs-lookup"><span data-stu-id="f91fd-148">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="f91fd-149">重新啟動 Apache:</span><span class="sxs-lookup"><span data-stu-id="f91fd-149">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="f91fd-150">監視應用程式</span><span class="sxs-lookup"><span data-stu-id="f91fd-150">Monitoring the app</span></span>

<span data-ttu-id="f91fd-151">Apache 現在是安裝所做的要求轉送給`http://localhost:80`Kestrel 在上執行的 ASP.NET Core 應用程式`http://127.0.0.1:5000`。</span><span class="sxs-lookup"><span data-stu-id="f91fd-151">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="f91fd-152">不過，Apache 並未設定來管理 Kestrel 程序。</span><span class="sxs-lookup"><span data-stu-id="f91fd-152">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="f91fd-153">使用*systemd*並建立服務檔案，啟動及監視基礎的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f91fd-153">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="f91fd-154">*systemd* 是 init 系統，提供許多強大的啟動、停止和管理處理程序功能。</span><span class="sxs-lookup"><span data-stu-id="f91fd-154">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="f91fd-155">建立服務檔</span><span class="sxs-lookup"><span data-stu-id="f91fd-155">Create the service file</span></span>

<span data-ttu-id="f91fd-156">建立服務定義檔：</span><span class="sxs-lookup"><span data-stu-id="f91fd-156">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="f91fd-157">範例服務檔案的應用程式：</span><span class="sxs-lookup"><span data-stu-id="f91fd-157">An example service file for the app:</span></span>

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if dotnet service crashes
RestartSec=10
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

> [!NOTE]
> <span data-ttu-id="f91fd-158">**使用者**&mdash;如果使用者*apache*不會使用設定，使用者必須先建立並提供適當的擁有權的檔案。</span><span class="sxs-lookup"><span data-stu-id="f91fd-158">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="f91fd-159">儲存檔案，並啟用服務：</span><span class="sxs-lookup"><span data-stu-id="f91fd-159">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="f91fd-160">啟動服務並確認正在執行：</span><span class="sxs-lookup"><span data-stu-id="f91fd-160">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="f91fd-161">設定反向 proxy 與透過管理 Kestrel *systemd*，完整設定 web 應用程式，並可從在本機電腦上的瀏覽器存取`http://localhost`。</span><span class="sxs-lookup"><span data-stu-id="f91fd-161">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="f91fd-162">檢查回應標頭**伺服器**標頭指出 ASP.NET Core 應用程式由 Kestrel:</span><span class="sxs-lookup"><span data-stu-id="f91fd-162">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="f91fd-163">檢視記錄</span><span class="sxs-lookup"><span data-stu-id="f91fd-163">Viewing logs</span></span>

<span data-ttu-id="f91fd-164">因為 web 應用程式使用 Kestrel 管理使用*systemd*，事件和處理程序會記錄到集中式的日誌。</span><span class="sxs-lookup"><span data-stu-id="f91fd-164">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="f91fd-165">不過，此筆記本中內含的所有服務和處理程序受項目*systemd*。</span><span class="sxs-lookup"><span data-stu-id="f91fd-165">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="f91fd-166">若要檢視 `kestrel-hellomvc.service` 的特定項目，請使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="f91fd-166">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="f91fd-167">時間篩選時，使用命令所指定時間選項。</span><span class="sxs-lookup"><span data-stu-id="f91fd-167">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="f91fd-168">例如，使用`--since today`篩選目前的日期或`--until 1 hour ago`查看前一小時的項目。</span><span class="sxs-lookup"><span data-stu-id="f91fd-168">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="f91fd-169">如需詳細資訊，請參閱[journalctl man 頁面](https://www.unix.com/man-page/centos/1/journalctl/)。</span><span class="sxs-lookup"><span data-stu-id="f91fd-169">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="f91fd-170">保護應用程式</span><span class="sxs-lookup"><span data-stu-id="f91fd-170">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="f91fd-171">設定防火牆</span><span class="sxs-lookup"><span data-stu-id="f91fd-171">Configure firewall</span></span>

<span data-ttu-id="f91fd-172">*Firewalld*是動態管理網路區域的支援具有防火牆協助程式。</span><span class="sxs-lookup"><span data-stu-id="f91fd-172">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="f91fd-173">仍可由 iptables 管理連接埠以及封包篩選。</span><span class="sxs-lookup"><span data-stu-id="f91fd-173">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="f91fd-174">*Firewalld*預設應該安裝。</span><span class="sxs-lookup"><span data-stu-id="f91fd-174">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="f91fd-175">`yum` 可用來安裝封裝，或檢查已安裝。</span><span class="sxs-lookup"><span data-stu-id="f91fd-175">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="f91fd-176">使用`firewalld`開啟應用程式所需的連接埠。</span><span class="sxs-lookup"><span data-stu-id="f91fd-176">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="f91fd-177">在此情況下，將使用連接埠 80 和 443。</span><span class="sxs-lookup"><span data-stu-id="f91fd-177">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="f91fd-178">下列命令會永久設定連接埠 80 和 443 開啟：</span><span class="sxs-lookup"><span data-stu-id="f91fd-178">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="f91fd-179">重新載入的防火牆設定。</span><span class="sxs-lookup"><span data-stu-id="f91fd-179">Reload the firewall settings.</span></span> <span data-ttu-id="f91fd-180">檢查可用的服務和連接埠的預設區域。</span><span class="sxs-lookup"><span data-stu-id="f91fd-180">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="f91fd-181">可用選項藉由檢查`firewall-cmd -h`。</span><span class="sxs-lookup"><span data-stu-id="f91fd-181">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="f91fd-182">SSL 組態</span><span class="sxs-lookup"><span data-stu-id="f91fd-182">SSL configuration</span></span>

<span data-ttu-id="f91fd-183">若要設定 ssl，Apache *mod_ssl*使用模組。</span><span class="sxs-lookup"><span data-stu-id="f91fd-183">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="f91fd-184">當*httpd*模組已安裝， *mod_ssl*也安裝模組。</span><span class="sxs-lookup"><span data-stu-id="f91fd-184">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="f91fd-185">如果未安裝，請使用`yum`將它加入至組態。</span><span class="sxs-lookup"><span data-stu-id="f91fd-185">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="f91fd-186">若要強制 SSL，請安裝`mod_rewrite`模組來啟用 URL 重寫：</span><span class="sxs-lookup"><span data-stu-id="f91fd-186">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="f91fd-187">修改*hellomvc.conf*啟用 URL 重寫和安全通訊連接埠 443 上的檔案：</span><span class="sxs-lookup"><span data-stu-id="f91fd-187">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
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
> <span data-ttu-id="f91fd-188">這個範例會使用在本機產生憑證。</span><span class="sxs-lookup"><span data-stu-id="f91fd-188">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="f91fd-189">**SSLCertificateFile**應該是網域名稱的主要憑證檔案。</span><span class="sxs-lookup"><span data-stu-id="f91fd-189">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="f91fd-190">**SSLCertificateKeyFile**應金鑰檔案產生 CSR 建立時。</span><span class="sxs-lookup"><span data-stu-id="f91fd-190">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="f91fd-191">**SSLCertificateChainFile**應該是中繼憑證檔案 （如果有的話），由憑證授權單位所提供。</span><span class="sxs-lookup"><span data-stu-id="f91fd-191">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="f91fd-192">儲存檔案並測試組態：</span><span class="sxs-lookup"><span data-stu-id="f91fd-192">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="f91fd-193">重新啟動 Apache:</span><span class="sxs-lookup"><span data-stu-id="f91fd-193">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="f91fd-194">Apache 的其他建議</span><span class="sxs-lookup"><span data-stu-id="f91fd-194">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="f91fd-195">其他標頭</span><span class="sxs-lookup"><span data-stu-id="f91fd-195">Additional headers</span></span>

<span data-ttu-id="f91fd-196">為了保護對抗惡意攻擊，有幾個標頭應該可以修改或加入。</span><span class="sxs-lookup"><span data-stu-id="f91fd-196">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="f91fd-197">請確認`mod_headers`模組安裝：</span><span class="sxs-lookup"><span data-stu-id="f91fd-197">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="f91fd-198">Apache 防範 clickjacking 攻擊</span><span class="sxs-lookup"><span data-stu-id="f91fd-198">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="f91fd-199">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger)，亦稱為*UI redress 攻擊*，是一種惡意攻擊網站訪客誘騙比目前瀏覽，按一下連結或不同的頁面上的按鈕。</span><span class="sxs-lookup"><span data-stu-id="f91fd-199">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="f91fd-200">使用`X-FRAME-OPTIONS`來保護安全的站台。</span><span class="sxs-lookup"><span data-stu-id="f91fd-200">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="f91fd-201">編輯*httpd.conf*檔案：</span><span class="sxs-lookup"><span data-stu-id="f91fd-201">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="f91fd-202">將行加入`Header append X-FRAME-OPTIONS "SAMEORIGIN"`。</span><span class="sxs-lookup"><span data-stu-id="f91fd-202">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="f91fd-203">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="f91fd-203">Save the file.</span></span> <span data-ttu-id="f91fd-204">重新啟動 Apache。</span><span class="sxs-lookup"><span data-stu-id="f91fd-204">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="f91fd-205">MIME 類型探查</span><span class="sxs-lookup"><span data-stu-id="f91fd-205">MIME-type sniffing</span></span>

<span data-ttu-id="f91fd-206">`X-Content-Type-Options`標頭會防止 Internet Explorer 的*MIME 探查*(決定檔`Content-Type`檔案的內容中)。</span><span class="sxs-lookup"><span data-stu-id="f91fd-206">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="f91fd-207">如果伺服器設定`Content-Type`標頭`text/html`與`nosniff`選項集，Internet Explorer 會轉譯為內容`text/html`不論檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="f91fd-207">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="f91fd-208">編輯*httpd.conf*檔案：</span><span class="sxs-lookup"><span data-stu-id="f91fd-208">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="f91fd-209">將行加入`Header set X-Content-Type-Options "nosniff"`。</span><span class="sxs-lookup"><span data-stu-id="f91fd-209">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="f91fd-210">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="f91fd-210">Save the file.</span></span> <span data-ttu-id="f91fd-211">重新啟動 Apache。</span><span class="sxs-lookup"><span data-stu-id="f91fd-211">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="f91fd-212">負載平衡</span><span class="sxs-lookup"><span data-stu-id="f91fd-212">Load Balancing</span></span> 

<span data-ttu-id="f91fd-213">這個範例示範如何在 CentOS 7 上安裝和設定 Apache，以及如何在相同的執行個體電腦上安裝和設定 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="f91fd-213">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="f91fd-214">若要不需要單點失敗。使用*mod_proxy_balancer*和修改**VirtualHost**允許管理的 Apache proxy 伺服器後方的 web 應用程式的多個執行個體。</span><span class="sxs-lookup"><span data-stu-id="f91fd-214">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="f91fd-215">在組態檔中顯示以下的其他執行個體`hellomvc`應用程式是在連接埠 5001 上執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="f91fd-215">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="f91fd-216">*Proxy*區段設定兩個成員平衡器組態，以進行負載平衡*byrequests*。</span><span class="sxs-lookup"><span data-stu-id="f91fd-216">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
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

### <a name="rate-limits"></a><span data-ttu-id="f91fd-217">速率限制</span><span class="sxs-lookup"><span data-stu-id="f91fd-217">Rate Limits</span></span>
<span data-ttu-id="f91fd-218">使用*mod_ratelimit*，隨附於*httpd*模組，用戶端頻寬可能會受到限制：</span><span class="sxs-lookup"><span data-stu-id="f91fd-218">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="f91fd-219">範例檔案限制為 600 的 KB/秒根位置下的頻寬：</span><span class="sxs-lookup"><span data-stu-id="f91fd-219">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
