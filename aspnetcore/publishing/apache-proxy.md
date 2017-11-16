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
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-apache-and-deploy-to-it"></a>在 Linux 上使用 Apache 設定並部署到 ASP.NET Core 裝載環境

作者：[Shayne Boyer](https://github.com/spboyer)

Apache 是非常熱門的 HTTP 伺服器，類似於 nginx，可以設定為 Proxy 來重新導向 HTTP 流量。 在本指南中，我們將了解如何在 CentOS 7 上設定 Apache，並使用它作為反向 Proxy 來接受連入連線並重新導向至在 Kestrel 上執行的 ASP.NET Core 應用程式。 基於此目的，我們將使用 *mod_proxy* 延伸模組和其他相關的 Apache 模組。

## <a name="prerequisites"></a>必要條件

1. 以具有 sudo 權限的標準使用者帳戶執行 CentOS 7 的伺服器。
2. 現有的 ASP.NET Core 應用程式。 

## <a name="publish-your-application"></a>發行您的應用程式

從開發環境執行 `dotnet publish -c Release`，以便將應用程式封裝成可以在伺服器上執行的獨立目錄。 接著，必須使用 SCP、FTP 或其他檔案傳送方法，將發行的應用程式複製到伺服器。 

> [!NOTE]
> 在生產環境部署案例中，持續整合工作流程會執行這項發行應用程式並將資產複製到伺服器的工作。 

## <a name="configure-a-proxy-server"></a>設定 Proxy 伺服器

反向 Proxy 是服務動態 Web 應用程式的常見安裝程式。 反向 Proxy 會終止 HTTP 要求，並將它轉送至 ASP.NET 應用程式。

Proxy 伺服器是將用戶端要求轉送至另一部伺服器，而不是自己完成這些要求。 反向 Proxy 會轉送至固定目的地，通常代表任意的用戶端。 本指南中，Apache 將設定為 Kestrel 為 ASP.NET Core 應用程式提供服務之相同伺服器上執行的反向 Proxy。 

應用程式的每個部分可以根據您的架構需求或限制，位於不同的實體電腦、Docker 容器或組態的組合。

### <a name="install-apache"></a>安裝 Apache

在 CentOS 上安裝 Apache 網頁伺服器是單一命令，但請先更新套件。

```bash
    sudo yum update -y
```

這可確保所有已安裝的套件會更新為最新的版本。 使用 `yum` 安裝 Apache

```bash
    sudo yum -y install httpd mod_ssl
```

輸出應該會反映與下列類似的項目。

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
> 在此範例中，輸出會反映 httpd.86_64，因為 CentOS 第 7 版是 64 位元。 您的伺服器輸出可能會不同。 若要確認 Apache 的安裝位置，請從命令提示字元執行 `whereis httpd`。 

### <a name="configure-apache-for-reverse-proxy"></a>設定 Apache 以用於反向 Proxy

Apache 的組態檔是位於 `/etc/httpd/conf.d/` 目錄內。 除了包含載入模組所需的任何設定檔之 `/etc/httpd/conf.modules.d/` 中的模組組態檔之外，任何副檔名為 **.conf** 的檔案也將依字母順序處理。

建立應用程式的組態檔，對於此範例我們稱它為 `hellomvc.conf`

```text
    <VirtualHost *:80>
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:5000/
        ProxyPassReverse / http://127.0.0.1:5000/
        ErrorLog /var/log/httpd/hellomvc-error.log
        CustomLog /var/log/httpd/hellomvc-access.log common
    </VirtualHost>
```

*VirtualHost* 節點 (一個檔案中或許多檔案的某部伺服器上可以有多個節點) 設定為使用連接埠 80 的任何 IP 位址上接聽。 下面兩行都會設定為將根目錄收到的所有要求傳遞至電腦 127.0.0.1 連接埠 5000，反之亦然。 為了進行雙向通訊，需要 *ProxyPass* 和 *ProxyPassReverse* 這兩個設定。

您可以使用 *ErrorLog* 和 *CustomLog* 指示詞設定每個 VirtualHost 的記錄。 *ErrorLog* 是伺服器將記錄錯誤的位置，而 *CustomLog* 會設定記錄檔的檔案名稱和格式。 在我們的案例中，這是記錄要求資訊的位置。 每個要求會有一行。

儲存檔案，並測試組態。 如果所有項目都通過，回應應該是 `Syntax [OK]`。

```bash
    sudo service httpd configtest
```

重新啟動 Apache。

```text
    sudo systemctl restart httpd
    sudo systemctl enable httpd
```

## <a name="monitoring-our-application"></a>監視應用程式

Apache 已設定完成，可將向 `http://localhost:80` 提出的要求轉送給在 `http://127.0.0.1:5000` 的 Kestrel 上執行的 ASP.NET Core 應用程式。  不過，Apache 未設定管理 Kestrel 處理序。 您可以使用 *systemd* 並建立服務檔案，啟動和監視基礎的 Web 應用程式。 *systemd* 是 init 系統，提供許多強大的啟動、停止和管理處理序功能。 


### <a name="create-the-service-file"></a>建立服務檔

建立服務定義檔 

```bash
    sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

我們的應用程式範例服務檔。

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
> **User** -- 如果您的組態不使用使用者 *apache*，則必須先建立此處定義的使用者，並指定適當的檔案擁有權。

儲存檔案並啟用服務。

```bash
    systemctl enable kestrel-hellomvc.service
```

啟動服務並確認它正在執行。

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

透過 systemd 設定反向 Proxy 及管理 Kestrel，可以完全設定 Web 應用程式，並從位在 `http://localhost` 的本機電腦瀏覽器存取它。 檢查回應標頭，**Server** 仍然顯示 Kestrel 正在為 ASP.NET Core 應用程式提供服務。

```text
    HTTP/1.1 200 OK
    Date: Tue, 11 Oct 2016 16:22:23 GMT
    Server: Kestrel
    Keep-Alive: timeout=5, max=98
    Connection: Keep-Alive
    Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>檢視記錄

因為使用 systemd 管理使用 Kestrel 的 Web 應用程式，所以所有事件和處理序都會記錄在集中式日誌中。 不過，此日誌包含 system 管理的所有服務和處理序的所有項目。 若要檢視 `kestrel-hellomvc.service` 的特定項目，請使用下列命令。

```bash
    sudo journalctl -fu kestrel-hellomvc.service
```

如需進一步篩選，例如 `--since today`、`--until 1 hour ago` 或這些項目的組合等時間選項，可以減少傳回的項目數量。

```bash
    sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a>保護應用程式

### <a name="configure-firewall"></a>設定防火牆

*Firewalld* 是支援網路區域的管理防火牆動態精靈，不過您仍然可以使用 iptables 來管理連接埠以及封包篩選。 根據預設應該安裝防火牆，`yum` 可用來安裝套件或確認。

```bash
    sudo yum install firewalld -y
```

使用 `firewalld`，您可以只開啟應用程式所需的連接埠。 在此情況下，將使用連接埠 80 和 443。 下列命令會將這些連接埠永久設定為開啟。

```bash
    sudo firewall-cmd --add-port=80/tcp --permanent
    sudo firewall-cmd --add-port=443/tcp --permanent
```

重新載入防火牆設定，並檢查預設區域中可用的服務和連接埠。 選項可透過檢查 `firewall-cmd -h` 來取得

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

### <a name="ssl-configuration"></a>SSL 組態

為了設定 Apache 以用於 SSL，將使用 mod_ssl 模組。  這是我們安裝 `httpd` 模組時一開始安裝的項目。 如果它已遺漏或未安裝，請使用 yum 將它新增至您的組態。

```bash
    sudo yum install mod_ssl
```
若要強制使用 SSL，請安裝 `mod_rewrite`

```bash
    sudo yum install mod_rewrite
```

必須修改針對這個範例建立的 `hellomvc.conf` 檔案，才能啟用重寫，以及為 HTTPS 新增新的 **VirtualHost** 區段。

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
> 此範例使用本機產生的憑證。 **SSLCertificateFile** 應該是您網域名稱的主要憑證檔案。 **SSLCertificateKeyFile** 應該是建立 CSR 時產生的金鑰檔。 **SSLCertificateChainFile** 應該是憑證授權單位所提供的中繼憑證檔案 (如果有的話)。

儲存檔案，並測試組態。

```bash
    sudo service httpd configtest
```

重新啟動 Apache。

```bash
    sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a>Apache 的其他建議

### <a name="additional-headers"></a>其他標頭 
為了防禦惡意攻擊，應該要修改或新增幾個標頭。 請確認已安裝 `mod_headers` 模組。

```bash
    sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking"></a>保護 Apache 免於點閱綁架
點閱綁架是收集受感染使用者按一下動作的惡意技術。 點閱綁架誘騙受害者 (訪客) 到受感染的網站上執行按一下的動作。 使用 X-FRAME-OPTIONS 保護您的網站。

編輯 httpd.conf 檔案。

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

新增行 `Header append X-FRAME-OPTIONS "SAMEORIGIN"` 並儲存檔案，然後重新啟動 Apache。

#### <a name="mime-type-sniffing"></a>MIME 類型探查

此標頭會防止 Internet Explorer 對遠離宣告內容類型的回應進行 MIME 探查，因為標頭會指示瀏覽器不要覆寫回應內容類型。 使用 nosniff 選項，如果伺服器顯示的內容是 text/html，則瀏覽器就會將它轉譯為 text/html。

編輯 httpd.conf 檔案。

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

新增行 `Header set X-Content-Type-Options "nosniff"` 並儲存檔案，然後重新啟動 Apache。

### <a name="load-balancing"></a>負載平衡 

這個範例示範如何在 CentOS 7 上安裝和設定 Apache，以及如何在相同的執行個體電腦上安裝和設定 Kestrel。  不過，為了避免產生單一失敗點，使用 *mod_proxy_balancer* 並修改 VirtualHost 可允許管理 Apache Proxy 伺服器後面之 Web 應用程式的多個執行個體。

```bash
    sudo yum install mod_proxy_balancer
```

在組態檔中，`hellomvc` 應用程式的其他執行個體已設定為在連接埠 5001 上執行，而 *Proxy* 區段已使用含有兩個成員的平衡器組態來設定為負載平衡 *byrequests*。

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

### <a name="rate-limits"></a>速率限制
使用 `htttpd` 模組中包含的 `mod_ratelimit`，您可以限制用戶端的頻寬數量。 

```bash
    sudo nano /etc/httpd/conf.d/ratelimit.conf
```
此範例檔案將根目錄位置下的頻寬限制為 600 KB/秒。

```text
    <IfModule mod_ratelimit.c>
        <Location />
            SetOutputFilter RATE_LIMIT
            SetEnv rate-limit 600
        </Location>
    </IfModule>
```
