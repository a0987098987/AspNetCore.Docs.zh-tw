---
title: "在 Linux 上使用 Apache 裝載 ASP.NET Core"
description: "了解如何將 Apache 設定為 CentOS 上的反向 Proxy，將 HTTP 流量重新導向至在 Kestrel 上執行的 ASP.NET Core Web 應用程式。"
keywords: "ASP.NET Core,Apache,CentOS,反向 Proxy,Linux,mod_proxy,httpd,裝載,主控,託管,代管"
author: spboyer
ms.author: spboyer
manager: wpickett
ms.date: 10/19/2016
ms.topic: article
ms.assetid: fa9b0cb7-afb3-4361-9e7e-33afffeaca0c
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/apache-proxy
ms.openlocfilehash: 34ede2fdebe0e9516f62e39618e7adba3c8c89db
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-apache-and-deploy-to-it"></a><span data-ttu-id="b0c5b-104">在 Linux 上使用 Apache 設定並部署到 ASP.NET Core 裝載環境</span><span class="sxs-lookup"><span data-stu-id="b0c5b-104">Set up a hosting environment for ASP.NET Core on Linux with Apache, and deploy to it</span></span>

<span data-ttu-id="b0c5b-105">作者：[Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="b0c5b-105">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="b0c5b-106">Apache 是非常熱門的 HTTP 伺服器，類似於 nginx，可以設定為 Proxy 來重新導向 HTTP 流量。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-106">Apache is a very popular HTTP server and can be configured as a proxy to redirect HTTP traffic similar to nginx.</span></span> <span data-ttu-id="b0c5b-107">在本指南中，我們將了解如何在 CentOS 7 上設定 Apache，並使用它作為反向 Proxy 來接受連入連線並重新導向至在 Kestrel 上執行的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-107">In this guide, we will learn how to set up Apache on CentOS 7 and use it as a reverse proxy to welcome incoming connections and redirect them to the ASP.NET Core application running on Kestrel.</span></span> <span data-ttu-id="b0c5b-108">基於此目的，我們將使用 *mod_proxy* 延伸模組和其他相關的 Apache 模組。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-108">For this purpose, we will use the *mod_proxy* extension and other related Apache modules.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b0c5b-109">必要條件</span><span class="sxs-lookup"><span data-stu-id="b0c5b-109">Prerequisites</span></span>

1. <span data-ttu-id="b0c5b-110">以具有 sudo 權限的標準使用者帳戶執行 CentOS 7 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-110">A server running CentOS 7, with a standard user account with sudo privilege.</span></span>
2. <span data-ttu-id="b0c5b-111">現有的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-111">An existing ASP.NET Core application.</span></span> 

## <a name="publish-your-application"></a><span data-ttu-id="b0c5b-112">發行您的應用程式</span><span class="sxs-lookup"><span data-stu-id="b0c5b-112">Publish your application</span></span>

<span data-ttu-id="b0c5b-113">從開發環境執行 `dotnet publish -c Release`，以便將應用程式封裝成可以在伺服器上執行的獨立目錄。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-113">Run `dotnet publish -c Release` from your development environment to package your application into a self-contained directory that can run on your server.</span></span> <span data-ttu-id="b0c5b-114">接著，必須使用 SCP、FTP 或其他檔案傳送方法，將發行的應用程式複製到伺服器。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-114">The published application must then be copied to the server using SCP, FTP or other file transfer method.</span></span> 

> [!NOTE]
> <span data-ttu-id="b0c5b-115">在生產環境部署案例中，持續整合工作流程會執行這項發行應用程式並將資產複製到伺服器的工作。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-115">Under a production deployment scenario, a continuous integration workflow does the work of publishing the application and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="b0c5b-116">設定 Proxy 伺服器</span><span class="sxs-lookup"><span data-stu-id="b0c5b-116">Configure a proxy server</span></span>

<span data-ttu-id="b0c5b-117">反向 Proxy 是服務動態 Web 應用程式的常見安裝程式。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-117">A reverse proxy is a common setup for serving dynamic web applications.</span></span> <span data-ttu-id="b0c5b-118">反向 Proxy 會終止 HTTP 要求，並將它轉送至 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-118">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET application.</span></span>

<span data-ttu-id="b0c5b-119">Proxy 伺服器是將用戶端要求轉送至另一部伺服器，而不是自己完成這些要求。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-119">A proxy server is one which forwards client requests to another server instead of fulfilling them itself.</span></span> <span data-ttu-id="b0c5b-120">反向 Proxy 會轉送至固定目的地，通常代表任意的用戶端。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-120">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="b0c5b-121">本指南中，Apache 將設定為 Kestrel 為 ASP.NET Core 應用程式提供服務之相同伺服器上執行的反向 Proxy。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-121">In this guide, Apache is being configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core application.</span></span> 

<span data-ttu-id="b0c5b-122">應用程式的每個部分可以根據您的架構需求或限制，位於不同的實體電腦、Docker 容器或組態的組合。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-122">Each piece of the application can exist on separate physical machines, Docker containers, or a combination of configurations depending on your architectural needs or restrictions.</span></span>

### <a name="install-apache"></a><span data-ttu-id="b0c5b-123">安裝 Apache</span><span class="sxs-lookup"><span data-stu-id="b0c5b-123">Install Apache</span></span>

<span data-ttu-id="b0c5b-124">在 CentOS 上安裝 Apache 網頁伺服器是單一命令，但請先更新套件。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-124">Installing the Apache web server on CentOS is a single command, but first let's update our packages.</span></span>

```bash
    sudo yum update -y
```

<span data-ttu-id="b0c5b-125">這可確保所有已安裝的套件會更新為最新的版本。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-125">This ensures that all of the installed packages are updated to their latest version.</span></span> <span data-ttu-id="b0c5b-126">使用 `yum` 安裝 Apache</span><span class="sxs-lookup"><span data-stu-id="b0c5b-126">Install Apache using `yum`</span></span>

```bash
    sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="b0c5b-127">輸出應該會反映與下列類似的項目。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-127">The output should reflect something similar to the following.</span></span>

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
> <span data-ttu-id="b0c5b-128">在此範例中，輸出會反映 httpd.86_64，因為 CentOS 第 7 版是 64 位元。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-128">In this example the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="b0c5b-129">您的伺服器輸出可能會不同。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-129">The output may be different for your server.</span></span> <span data-ttu-id="b0c5b-130">若要確認 Apache 的安裝位置，請從命令提示字元執行 `whereis httpd`。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-130">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span> 

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="b0c5b-131">設定 Apache 以用於反向 Proxy</span><span class="sxs-lookup"><span data-stu-id="b0c5b-131">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="b0c5b-132">Apache 的組態檔是位於 `/etc/httpd/conf.d/` 目錄內。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-132">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="b0c5b-133">除了包含載入模組所需的任何設定檔之 `/etc/httpd/conf.modules.d/` 中的模組組態檔之外，任何副檔名為 **.conf** 的檔案也將依字母順序處理。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-133">Any file with the **.conf** extension will be processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="b0c5b-134">建立應用程式的組態檔，對於此範例我們稱它為 `hellomvc.conf`</span><span class="sxs-lookup"><span data-stu-id="b0c5b-134">Create a configuration file for your app, for this example we'll call it `hellomvc.conf`</span></span>

```text
    <VirtualHost *:80>
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:5000/
        ProxyPassReverse / http://127.0.0.1:5000/
        ErrorLog /var/log/httpd/hellomvc-error.log
        CustomLog /var/log/httpd/hellomvc-access.log common
    </VirtualHost>
```

<span data-ttu-id="b0c5b-135">*VirtualHost* 節點 (一個檔案中或許多檔案的某部伺服器上可以有多個節點) 設定為使用連接埠 80 的任何 IP 位址上接聽。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-135">The *VirtualHost* node, of which there can be multiple in a file or on a server in many files, is set to listen on any IP address using port 80.</span></span> <span data-ttu-id="b0c5b-136">下面兩行都會設定為將根目錄收到的所有要求傳遞至電腦 127.0.0.1 連接埠 5000，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-136">The next two lines are set to pass all requests received at the root to the machine 127.0.0.1 port 5000 and in reverse.</span></span> <span data-ttu-id="b0c5b-137">為了進行雙向通訊，需要 *ProxyPass* 和 *ProxyPassReverse* 這兩個設定。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-137">For there to be bi-directional communication, both settings *ProxyPass* and *ProxyPassReverse* are required.</span></span>

<span data-ttu-id="b0c5b-138">您可以使用 *ErrorLog* 和 *CustomLog* 指示詞設定每個 VirtualHost 的記錄。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-138">Logging can be configured per VirtualHost using *ErrorLog* and *CustomLog* directives.</span></span> <span data-ttu-id="b0c5b-139">*ErrorLog* 是伺服器將記錄錯誤的位置，而 *CustomLog* 會設定記錄檔的檔案名稱和格式。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-139">*ErrorLog* is the location where the server will log errors and *CustomLog* sets the filename and format of log file.</span></span> <span data-ttu-id="b0c5b-140">在我們的案例中，這是記錄要求資訊的位置。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-140">In our case this is where request information will be logged.</span></span> <span data-ttu-id="b0c5b-141">每個要求會有一行。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-141">There will be one line for each request.</span></span>

<span data-ttu-id="b0c5b-142">儲存檔案，並測試組態。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-142">Save the file, and test the configuration.</span></span> <span data-ttu-id="b0c5b-143">如果所有項目都通過，回應應該是 `Syntax [OK]`。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-143">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
    sudo service httpd configtest
```

<span data-ttu-id="b0c5b-144">重新啟動 Apache。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-144">Restart Apache.</span></span>

```text
    sudo systemctl restart httpd
    sudo systemctl enable httpd
```

## <a name="monitoring-our-application"></a><span data-ttu-id="b0c5b-145">監視應用程式</span><span class="sxs-lookup"><span data-stu-id="b0c5b-145">Monitoring our application</span></span>

<span data-ttu-id="b0c5b-146">Apache 已設定完成，可將向 `http://localhost:80` 提出的要求轉送給在 `http://127.0.0.1:5000` 的 Kestrel 上執行的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-146">Apache is now setup to forward requests made to `http://localhost:80` on to the ASP.NET Core application running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="b0c5b-147">不過，Apache 未設定管理 Kestrel 處理序。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-147">However, Apache is not set up to manage the Kestrel process.</span></span> <span data-ttu-id="b0c5b-148">您可以使用 *systemd* 並建立服務檔案，啟動和監視基礎的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-148">We will use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="b0c5b-149">*systemd* 是 init 系統，提供許多強大的啟動、停止和管理處理序功能。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-149">*systemd* is an init system that provides many powerful features for starting, stopping and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="b0c5b-150">建立服務檔</span><span class="sxs-lookup"><span data-stu-id="b0c5b-150">Create the service file</span></span>

<span data-ttu-id="b0c5b-151">建立服務定義檔</span><span class="sxs-lookup"><span data-stu-id="b0c5b-151">Create the service definition file</span></span> 

```bash
    sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="b0c5b-152">我們的應用程式範例服務檔。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-152">An example service file for our application.</span></span>

```text
[Unit]
    Description=Example .NET Web API Application running on CentOS 7

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
> <span data-ttu-id="b0c5b-153">**User** -- 如果您的組態不使用使用者 *apache*，則必須先建立此處定義的使用者，並指定適當的檔案擁有權。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-153">**User** -- If the user *apache* is not used by your configuration, the user defined here must be created first and given proper ownership for files</span></span>

<span data-ttu-id="b0c5b-154">儲存檔案並啟用服務。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-154">Save the file and enable the service.</span></span>

```bash
    systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="b0c5b-155">啟動服務並確認它正在執行。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-155">Start the service and verify that it is running.</span></span>

```
    systemctl start kestrel-hellomvc.service
    systemctl status kestrel-hellomvc.service

    ● kestrel-hellomvc.service - Example .NET Web API Application running on CentOS 7
        Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
        Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
    Main PID: 9021 (dotnet)
        CGroup: /system.slice/kestrel-hellomvc.service
                └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="b0c5b-156">透過 systemd 設定反向 Proxy 及管理 Kestrel，可以完全設定 Web 應用程式，並從位在 `http://localhost` 的本機電腦瀏覽器存取它。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-156">With the reverse proxy configured and Kestrel managed through systemd, the web application is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="b0c5b-157">檢查回應標頭，**Server** 仍然顯示 Kestrel 正在為 ASP.NET Core 應用程式提供服務。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-157">Inspecting the response headers, the **Server** still shows the ASP.NET Core application being served by Kestrel.</span></span>

```text
    HTTP/1.1 200 OK
    Date: Tue, 11 Oct 2016 16:22:23 GMT
    Server: Kestrel
    Keep-Alive: timeout=5, max=98
    Connection: Keep-Alive
    Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="b0c5b-158">檢視記錄</span><span class="sxs-lookup"><span data-stu-id="b0c5b-158">Viewing logs</span></span>

<span data-ttu-id="b0c5b-159">因為使用 systemd 管理使用 Kestrel 的 Web 應用程式，所以所有事件和處理序都會記錄在集中式日誌中。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-159">Since the web application using Kestrel is managed using systemd, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="b0c5b-160">不過，此日誌包含 system 管理的所有服務和處理序的所有項目。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-160">However, this journal includes all entries for all services and processes managed by systemd.</span></span> <span data-ttu-id="b0c5b-161">若要檢視 `kestrel-hellomvc.service` 的特定項目，請使用下列命令。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-161">To view the `kestrel-hellomvc.service` specific items, use the following command.</span></span>

```bash
    sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="b0c5b-162">如需進一步篩選，例如 `--since today`、`--until 1 hour ago` 或這些項目的組合等時間選項，可以減少傳回的項目數量。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-162">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
    sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a><span data-ttu-id="b0c5b-163">保護應用程式</span><span class="sxs-lookup"><span data-stu-id="b0c5b-163">Securing our application</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="b0c5b-164">設定防火牆</span><span class="sxs-lookup"><span data-stu-id="b0c5b-164">Configure firewall</span></span>

<span data-ttu-id="b0c5b-165">*Firewalld* 是支援網路區域的管理防火牆動態精靈，不過您仍然可以使用 iptables 來管理連接埠以及封包篩選。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-165">*Firewalld* is a dynamic daemon to manage firewall with support for network zones, although you can still use iptables to manage ports and packet filtering.</span></span> <span data-ttu-id="b0c5b-166">根據預設應該安裝防火牆，`yum` 可用來安裝套件或確認。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-166">Firewalld should be installed by default, `yum` can be used to install the package or verify.</span></span>

```bash
    sudo yum install firewalld -y
```

<span data-ttu-id="b0c5b-167">使用 `firewalld`，您可以只開啟應用程式所需的連接埠。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-167">Using `firewalld` you can open only the ports needed for the application.</span></span> <span data-ttu-id="b0c5b-168">在此情況下，將使用連接埠 80 和 443。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-168">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="b0c5b-169">下列命令會將這些連接埠永久設定為開啟。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-169">The following commands permanently sets these to open.</span></span>

```bash
    sudo firewall-cmd --add-port=80/tcp --permanent
    sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="b0c5b-170">重新載入防火牆設定，並檢查預設區域中可用的服務和連接埠。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-170">Reload the firewall settings, and check the available services and ports in the default zone.</span></span> <span data-ttu-id="b0c5b-171">選項可透過檢查 `firewall-cmd -h` 來取得</span><span class="sxs-lookup"><span data-stu-id="b0c5b-171">Options are available by inspecting `firewall-cmd -h`</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="b0c5b-172">SSL 組態</span><span class="sxs-lookup"><span data-stu-id="b0c5b-172">SSL configuration</span></span>

<span data-ttu-id="b0c5b-173">為了設定 Apache 以用於 SSL，將使用 mod_ssl 模組。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-173">To configure Apache for SSL, the mod_ssl module is used.</span></span>  <span data-ttu-id="b0c5b-174">這是我們安裝 `httpd` 模組時一開始安裝的項目。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-174">This was installed initially when we installed the `httpd` module.</span></span> <span data-ttu-id="b0c5b-175">如果它已遺漏或未安裝，請使用 yum 將它新增至您的組態。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-175">If it was missed or not installed, use yum to add it to your configuration.</span></span>

```bash
    sudo yum install mod_ssl
```
<span data-ttu-id="b0c5b-176">若要強制使用 SSL，請安裝 `mod_rewrite`</span><span class="sxs-lookup"><span data-stu-id="b0c5b-176">To enforce SSL, install `mod_rewrite`</span></span>

```bash
    sudo yum install mod_rewrite
```

<span data-ttu-id="b0c5b-177">必須修改針對這個範例建立的 `hellomvc.conf` 檔案，才能啟用重寫，以及為 HTTPS 新增新的 **VirtualHost** 區段。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-177">The `hellomvc.conf` file that was created for this example needs to be modified to enable the rewrite as well as adding the new **VirtualHost** section for HTTPS.</span></span>

```text
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
> <span data-ttu-id="b0c5b-178">此範例使用本機產生的憑證。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-178">This example is using a locally generated certificate.</span></span> <span data-ttu-id="b0c5b-179">**SSLCertificateFile** 應該是您網域名稱的主要憑證檔案。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-179">**SSLCertificateFile** should be your primary certificate file for your domain name.</span></span> <span data-ttu-id="b0c5b-180">**SSLCertificateKeyFile** 應該是建立 CSR 時產生的金鑰檔。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-180">**SSLCertificateKeyFile** should be the key file generated when you created the CSR.</span></span> <span data-ttu-id="b0c5b-181">**SSLCertificateChainFile** 應該是憑證授權單位所提供的中繼憑證檔案 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-181">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by your certificate authority</span></span>

<span data-ttu-id="b0c5b-182">儲存檔案，並測試組態。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-182">Save the file, and test the configuration.</span></span>

```bash
    sudo service httpd configtest
```

<span data-ttu-id="b0c5b-183">重新啟動 Apache。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-183">Restart Apache.</span></span>

```bash
    sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="b0c5b-184">Apache 的其他建議</span><span class="sxs-lookup"><span data-stu-id="b0c5b-184">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="b0c5b-185">其他標頭</span><span class="sxs-lookup"><span data-stu-id="b0c5b-185">Additional Headers</span></span> 
<span data-ttu-id="b0c5b-186">為了防禦惡意攻擊，應該要修改或新增幾個標頭。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-186">In order to secure against malicious attacks there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="b0c5b-187">請確認已安裝 `mod_headers` 模組。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-187">Ensure that the `mod_headers` module is installed.</span></span>

```bash
    sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking"></a><span data-ttu-id="b0c5b-188">保護 Apache 免於點閱綁架</span><span class="sxs-lookup"><span data-stu-id="b0c5b-188">Secure Apache from clickjacking</span></span>
<span data-ttu-id="b0c5b-189">點閱綁架是收集受感染使用者按一下動作的惡意技術。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-189">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="b0c5b-190">點閱綁架誘騙受害者 (訪客) 到受感染的網站上執行按一下的動作。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-190">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="b0c5b-191">使用 X-FRAME-OPTIONS 保護您的網站。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-191">Use X-FRAME-OPTIONS to secure your site.</span></span>

<span data-ttu-id="b0c5b-192">編輯 httpd.conf 檔案。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-192">Edit the httpd.conf file.</span></span>

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="b0c5b-193">新增行 `Header append X-FRAME-OPTIONS "SAMEORIGIN"` 並儲存檔案，然後重新啟動 Apache。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-193">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"` and save the file, then restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="b0c5b-194">MIME 類型探查</span><span class="sxs-lookup"><span data-stu-id="b0c5b-194">MIME-type sniffing</span></span>

<span data-ttu-id="b0c5b-195">此標頭會防止 Internet Explorer 對遠離宣告內容類型的回應進行 MIME 探查，因為標頭會指示瀏覽器不要覆寫回應內容類型。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-195">This header prevents Internet Explorer from MIME-sniffing a response away from the declared content-type as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="b0c5b-196">使用 nosniff 選項，如果伺服器顯示的內容是 text/html，則瀏覽器就會將它轉譯為 text/html。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-196">With the nosniff option, if the server says the content is text/html, the browser will render it as text/html.</span></span>

<span data-ttu-id="b0c5b-197">編輯 httpd.conf 檔案。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-197">Edit the httpd.conf file.</span></span>

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="b0c5b-198">新增行 `Header set X-Content-Type-Options "nosniff"` 並儲存檔案，然後重新啟動 Apache。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-198">Add the line `Header set X-Content-Type-Options "nosniff"` and save the file, then restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="b0c5b-199">負載平衡</span><span class="sxs-lookup"><span data-stu-id="b0c5b-199">Load Balancing</span></span> 

<span data-ttu-id="b0c5b-200">這個範例示範如何在 CentOS 7 上安裝和設定 Apache，以及如何在相同的執行個體電腦上安裝和設定 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-200">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span>  <span data-ttu-id="b0c5b-201">不過，為了避免產生單一失敗點，使用 *mod_proxy_balancer* 並修改 VirtualHost 可允許管理 Apache Proxy 伺服器後面之 Web 應用程式的多個執行個體。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-201">However, in order to not have a single point of failure; using *mod_proxy_balancer* and modifying the VirtualHost would allow for managing mutliple instances of the web applications behind the Apache proxy server.</span></span>

```bash
    sudo yum install mod_proxy_balancer
```

<span data-ttu-id="b0c5b-202">在組態檔中，`hellomvc` 應用程式的其他執行個體已設定為在連接埠 5001 上執行，而 *Proxy* 區段已使用含有兩個成員的平衡器組態來設定為負載平衡 *byrequests*。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-202">In the configuration file, an additional instance of the `hellomvc` app has been setup to run on port 5001 and the *Proxy* section has been set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```text
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

### <a name="rate-limits"></a><span data-ttu-id="b0c5b-203">速率限制</span><span class="sxs-lookup"><span data-stu-id="b0c5b-203">Rate Limits</span></span>
<span data-ttu-id="b0c5b-204">使用 `htttpd` 模組中包含的 `mod_ratelimit`，您可以限制用戶端的頻寬數量。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-204">Using `mod_ratelimit`, which is included in the `htttpd` module you can limit the amount of bandwidth of clients.</span></span> 

```bash
    sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="b0c5b-205">此範例檔案將根目錄位置下的頻寬限制為 600 KB/秒。</span><span class="sxs-lookup"><span data-stu-id="b0c5b-205">The example file limits bandwidth as 600 KB/sec under the root location.</span></span>

```text
    <IfModule mod_ratelimit.c>
        <Location />
            SetOutputFilter RATE_LIMIT
            SetEnv rate-limit 600
        </Location>
    </IfModule>
```
